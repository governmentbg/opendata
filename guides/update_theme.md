# Обновяване на темата на CKAN

Порталът използва собствена тема, [ckan-bulgarian-theme](https://github.com/governmentbg/ckan-bulgarian-theme). Повечето визуални промени, свързани с HTML, CSS или JavaScript код, трябва да се правят в това хранилище.

След като промяната е направена, промените могат да се качат на портала с командите по-долу.

## Процедура за обновяване

Като `root` на сървъра:

    su - www-data
    source /var/www/ckan/virtualenv/bin/activate
    pip install -e "git+https://github.com/governmentbg/ckan-bulgarian-theme.git@master#egg=bulgarian_theme"

Това би трябвало да е достатъчно. При необходимост, като `root`, рестартирайте Apache:

    service apache2 restart
