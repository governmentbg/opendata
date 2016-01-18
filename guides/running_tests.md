# Как да си пуснем тестовете

## Как да си пуснем тестовете на ckan

Как да си пуснем тестовете на `ckan` е описано най-подробно в този [guide](http://docs.ckan.org/en/ckan-2.2/test.html). Стъпките се изпълняват като root user.

1. Активира се environment - `. /usr/lib/ckan/default/bin/activate`
2. Инсталират се dev dependencies `pip install -r /usr/lib/ckan/default/src/ckan/dev-requirements.txt`
3. Създава се тестова база:
```
sudo -u postgres createdb -O ckan_default ckan_test -E utf-8
sudo -u postgres createdb -O ckan_default datastore_test -E utf-8
paster datastore set-permissions postgres -c test-core.ini
```
4. Навигира се до `/usr/lib/ckan/default/src/ckan` и се пуска `nosetests --ckan --reset-db --with-pylons=test-core.ini ckan ckanext`. Това ще изчисти базата и ще пусне тестовете. Ако не искате да чистите базата, махнете `--reset-db`. Ако не ви намира тестове(или 1-2 теста само) добавете и `--exe` опцията.

Хубаво е да се погледне и самия [guide](http://docs.ckan.org/en/ckan-2.2/test.html), защото има допълнителни инструкции ако са направени промени в базата и т.н.

## Как да си пуснем тестовете на datapusher

Как да си пуснем тестовете на `datapusher` е описано [тук](https://github.com/governmentbg/ckan-datapusher/blob/master/README.markdown).

1. Навигира се до `datapusher` директорията и се инсталират dev dependencies `pip install -r requirements-dev.txt`.
2. Пускате тестовете `nosetests`


Ако нещо не работи, погледнете тези 2 линка [ckan](http://docs.ckan.org/en/ckan-2.2/test.html), [datapusher](https://github.com/governmentbg/ckan-datapusher/blob/master/README.markdown). Там стъпките са описани по-подробно.
