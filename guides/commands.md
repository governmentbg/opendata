# Полезни примери: CKAN command line

Следните примери са извадка от документацията за CKAN. Улеснени и преведени.

## Въведение

Конзолните команди в CKAN се облягат на библиотеката [PythonPaste](http://pythonpaste.org/), познато още като просто Paste. Мотото ѝ е "a framework for web frameworks". Source: https://bitbucket.org/ianb

Самият CKAN е изграден с web framework-а [Pylons](http://www.pylonsproject.org/). Pylons ползва Paste, но двете са отделни пакети. Командите на CKAN се подават на Paste като зареждат CKAN като плъгин.

Още инфо: http://docs.ckan.org/en/latest/maintaining/paster.html

## Към командите!

Влезте като www-data на сървъра и активирайте правилната работна среда:

    su - www-data
    source /var/www/ckan/virtualenv/bin/activate

Изпълниямият файл в PythonPaste се нарича`paster`. С него ще работим.

Един от главните аргумент е `-c`, с който се указва пътя до конфигурационния файл (най-честно с име `production.ini`).

Подредбата на аргументите е важна.

### Потребители

Ето как се **създава потребител** в CKAN:

    paster --plugin=ckan user add <username> email=<adress@government.bg> -c /var/www/ckan/config/production.ini

Даване на **супер-права на потребител**:

    paster --plugin=ckan sysadmin add <username> -c /var/www/ckan/config/production.ini

Махане на **супер-права от потребител**:

    paster --plugin=ckan sysadmin remove <username> -c /var/www/ckan/config/production.ini

Показване на **списък с всички администратори**:

    paster --plugin=ckan sysadmin -c /var/www/ckan/config/production.ini

**Списък с потребители**:

    paster --plugin=ckan user list -c /var/www/ckan/config/production.ini

**Смяна на парола на потребител USERNAME** (ще пита на prompt за паролата):

    paster --plugin=ckan user setpass USERNAME -c /var/www/ckan/config/production.ini

**Export на потребителите** в CSV:

    paster --plugin=ckan db user-dump-csv -c /var/www/ckan/config/production.ini my_database_users.csv

Показване на **помощ (help) за дадена команда**:

    paster --plugin=ckan sysadmin --help -c /var/www/ckan/config/production.ini

### База данни

Dump (backup) на базата данни:

    paster --plugin=ckan db dump --config=/var/www/ckan/config/production.ini /data/backups/opendata_`date '+%Y%m%d_%H%M%S'`.pg_dump

### Набори от данни

**Списък с datasets**:

    paster --plugin=ckan dataset list -c /var/www/ckan/config/production.ini

Още инфо за команди с dataset-и:

    paster --plugin=ckan dataset --help -c /var/www/ckan/config/production.ini
t/production.ini

### Помощни команди

**Rebuild entire search** index:

    paster --plugin=ckan search-index rebuild -c /var/www/ckan/config/production.ini

**Build missing search** index:

    paster --plugin=ckan search-index rebuild -o -c /var/www/ckan/config/production.ini

## Преводи

Езикови файлове:

    /var/www/virtualenv/src/ckan/ckan/i18n/

**Компилиране** на преводите:

    msgfmt -cv -o /var/www/virtualenv/src/ckan/ckan/i18n/bg/LC_MESSAGES/ckan.mo /var/www/virtualenv/src/ckan/ckan/i18n/bg/LC_MESSAGES/ckan.po

## Проверки

http://opendata.government.bg/api/util/status

> When editing a resource in CKAN (clicking  the “Manage” button on a resource page), a new tab will appear named  “Resource Data”. This will contain a log of the last attempted upload  and an opportunity to retry to upload.

Hardcoded max file size for datapusher: `MAX_CONTENT_LENGTH = 10485760  # 10MB` ([source file](https://github.com/ckan/datapusher/blob/master/datapusher/jobs.py#L29)).

## Блокиране на IP адреси

`iptables -A INPUT -j DROP -s 1.2.3.4`

Не се пази след рестарт. За запазване след рестарт, може да се ползва iptables-save пакетът (отделен), но обикновено временно блокиране е достатъчно, а и машината не се рестартира често.

## Remaining setup

Mainly crontab useful actions

### Email известия към потребителите

https://ckan.readthedocs.org/en/1485-rtd-theme/maintaining/email-notifications.html

Проверете в `/var/www/ckan/config/production.ini`, че следната опция е коректно настроена:

    ckan.activity_streams_email_notifications = True

В crontab (`crontab -e`, като `www-data`):

    @hourly echo '{}' | /var/www/virtualenv/bin/paster --plugin=ckan post -c /etc/ckan/production.ini /api/action/send_email_notifications > /dev/null

### Измерване на популярност на datasets

http://docs.ckan.org/en/latest/maintaining/tracking.html

    ckan.tracking_enabled = true

The `paster --plugin=ckan tracking update -c ...` and `paster --plugin=ckan search-index rebuild -c ...` commands need to be run periodicially to update this tracking summary data.

## Custom fields research

Tag vocabularies:
http://docs.ckan.org/en/latest/maintaining/tag-vocabularies.html
