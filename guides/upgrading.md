# Обновяване на CKAN

Обновяване на CKAN от 2.3a до 2.3 final ([diff & changelog](https://github.com/governmentbg/ckan/compare/930fc27...ckan-2.3)).

Базирано на [това ръководство](http://docs.ckan.org/en/latest/maintaining/upgrading/upgrade-source.html).

    su - www-data
    source /var/www/ckan/virtualenv/bin/activate
    paster --plugin=ckan db dump --config=/var/www/ckan/config/production.ini /data/backups/opendata_`date '+%Y%m%d_%H%M%S'`.pg_dump

Обновяване на конфигурационните файлове като се гледа ръчно diff-а.

След това, инсталация на CKAN, на чисто:

    pip uninstall ckan
    pip install -e "git+https://github.com/governmentbg/ckan.git@master#egg=ckan"

Инсталация на зависимостите му:

    pip install --upgrade -r /var/www/ckan/virtualenv/src/ckan/requirements.txt

Обновяване на базата:

    paster --plugin=ckan db upgrade -c /var/www/ckan/config/production.ini

Rebuild на search index-а:

    paster --plugin=ckan search-index rebuild -r -c /var/www/ckan/config/production.ini

Създаване на изгледи в базата:

    paster --plugin=ckan views create -c /var/www/ckan/config/production.ini

Като root:

    service jetty restart
    service apache2 reload

## Обновяване на темата (визията)

Вижте [това ръководство](update_theme.md).
