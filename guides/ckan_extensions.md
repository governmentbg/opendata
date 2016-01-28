# Правене на CKAN Extensions

За да не преповтарям официалния guide, който е доста хубав, ще добавя няколко наблюдения от писането на даден CKAN Extension:

* Официалният guide е добре да се прочете - <http://docs.ckan.org/en/latest/extensions/index.html>
* Макар и да е отделен guide, **theming-а** също е extension и включва полезни неща в себе си, които не винаги имат общи с **theming** - <http://docs.ckan.org/en/latest/theming/index.html> 
  * Ако не намерите нещо в extensions guide-a, проверете в theming guide-a
* **Неофициалната документация са всички примерни extension–и**, `example_*`, които са качени в repo-то на CKAN - <https://github.com/ckan/ckan/tree/master/ckanext>  - взимайте код от там.
* Ако промените нещо по Python кода-а на extension-а, трябва да рестартирате apache сървъра, за да се отрази промяната.

## Бъркане в templates

Extension-а поддържа същата структура на templates, както в CKAN - <https://github.com/ckan/ckan/tree/master/ckan/templates>

Ако искате да добавите нещо, например в `templates/package/read.html`, търсете в кода на CKAN подобни **template куки**:

```
{% block package_additional_info %}
  {% snippet "package/snippets/additional_info.html", pkg_dict=pkg %}
{% endblock %}
```

Тези **блокове** ни позволяват да добавим наше парче HTML / template код, чрез нашия extension.

Например, за да променим този файл, трябва да създадем същата директорийна структура при нас: `templates/package/read.html` и да се "plug"-нем в блока:

```
{% ckan_extends %}


{% block package_additional_info %}
{{ super() }}

<p>Plugging in here</p>
{% endblock %}
```

Това, което е важно да не забравим в нашия template е `{{ super() }}`, защото без него, ще заместим цялото съдържание на оригиналния блок и няма да получим (освен ако не търсим точно него) желания ефект.

`{{ super() }}` ще се погрижи да добави оригиналното съдържание на блока. Нещо като `super` в програмирането ;)

Като цяло правилото е:

1. Ако има `templates/some/path/to.html` в CKAN, то ние трябва да направим
2. `templates/some/path/to.html` в нашия extension и да се plug-нем в желания блок, като помним за `{{ super() }}` 
