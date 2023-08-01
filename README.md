### author webber (web-ber12@yandex.ru)

### набор eFilter - автоматическое построение фильтров товаров в MODx Evo (версия 0.1)

### DONATE
---------
если считаете данный продукт полезным и хотите отблагодарить автора материально,
либо просто пожертвовать немного средств на развитие проекта - 
можете сделать это на любой удобный Вам электронный кошелек<br><br>
<strong>Яндекс.Деньги</strong> 410011544038803<br>
<strong>Webmoney WMR:</strong> R133161482227<br>
<strong>Webmoney WMZ:</strong> Z202420836069<br><br>
с необязательной пометкой от кого и за что именно :)


### содержит:
- модуль eLists - для удобного формирования списков значений ТВ (чтобы не захламлять дерево и визуально понятно их редактировать)
- плагин tovarParams - для показа в админке при редактировании товара только тех параметров, которые заданы для данной категории товаров
- набор сниппетов для формирования формы и проведения фильтрации

### обязательные дополнительные компоненты (без их установки решение не работает):
- сниппет DocLister
- компонент multiTV

### автоматическая установка
Найдите в Extras сниппет eFilter (версия 0.1) и нажмите установить.<br>
При этом создадутся все необходимые элементы и связи между ними:<br>
- модуль eLists<br>
- сниппеты (eFilter, eFilterResult, multiParams, tovarParams, getSortBlock)<br>
- плагин eFilterHelper<br>
- TV tovarparams<br>
Переходим к пункту "Настройка"

### ручная установка
1. установить необходимые компоненты (модуль, плагин, сниппеты). Также скопировать конфигурацию для multiTV (\assets\tvs\multitv\configs\tovarparams.config.inc)
2. создать специальный TV с именем tovarparams, описание - Параметры товара, тип ввода - CustomInput, возможные значения @INCLUDE assets/tvs/multitv/multitv.customtv.php и присвоить этот tv всем шаблонам-категориям товара
3. создать шаблон для вывода товара
4. Включить 'общие' параметры в настройках модуля и добавить зависимости
==Plugins==
tovarParams
==Snippets==
eFilter
eFilterResult
multiParams
tovarParams

### настройка

Отредактировать параметры настройки модуля<br>
**ID TV параметров товара tovarparams** - вписываем сюда id нашего созданного при установке tv tovarparams<br>
**ID шаблонов товара** - перечисляем через запятую id шаблонов "Товар"<br>
**ID категории параметров** - ID категории, в которой будут храниться все tv, в которых задаются параметры товара (размер, цвет, материал и т.п.)<br>
**Не включать ТВ в параметры при выводе** -  - т.к. изначально в списке параметров будут выведены все параметры для данного типа товара из категории "Параметры товара", то можно его проредить, убрав ненужные ТВ (например цена, наличие и другие общие параметры для всех товаров которые можно вывести отдельно) <br>
**Имя чанка вывода товара** - имя чанка вывода товара в список (используется вместо задания &tpl) <br>
**Папка паттернов** - папка для хранения паттернов (фильтрация по типу "паттерн")<br>
**ID TV, используемого для связки товар-категории через tagSaver** - если вы используете для создания тегов плагин tagSaver и хотите, чтобы товары участвовали в фильтрации по притегованной категории - введите сюда id tv, которые обрабатываются плагином tagSaver<br>


### пример вызова сниппета eFilter
[!eFilter!] [+eFilter_form+]<br>
Данный сниппет, используя заданную для конкретной категории конфигурацию фильтра формирует форму и результаты фильтрации. Сама форма устанавливается в плейсхолдер [+eFilter_form+] и может быть выведена в любом месте шаблона (но обязательно после вызова фильтра).<br>
А результаты фильтрации выводятся сниппетом для вывода результатов eFilterResult - о нем чуть позднее.

### дополнительные параметры вызова сниппета eFilter

        &ajax=`1` - необходимо работать в режиме ajax (default 0)
        &ajaxMode=`2` - 1 - перезагрузка и формы и результатов в режиме ajax, 2 - перегружается только форма, а результаты обновляются только после ручного сабмита формы (default 1)
        &autoSubmit=`0` - автоматический сабмит формы при каждом изменении (default -1)
        &changeState=`0` - изменение url при каждом ajax-запросе (default 1)
        &docid - страница, для которой формируется фильтр (default - текущая страница)
        &cfg - название конфига для вывода фильтра из папки configs (по-умолчанию - default и соответственно конфиг с именем config.default.php)
        &filters_DL - безусловная фильтрация базового набора товаров. Например, &filters_DL=`tvd:color:=:10`
        &removeDisabled=`1` - удалять из формы варианты, для которых нет результатов (default - 0)
        &btnText - текст на кнопке формы (по-умолчанию "Найти")
        &hideZero=`1` - скрывать ли цифру 0 в вариантах, где ничего не найдено (default - 0)
        &nosortTvId - перечень tv, для которых в выводе формы не нужно применять сортировку (по-умолчанию варианты сортируются "по алфавиту" - для строк, и "по возрастанию" - для значений, распознанных как числовые)
        &sortByCountTvId - список id tv для которых необходимо применять при выводе сортировку по количеству доступных вариантов (допустимые значения - список id через запятую либо all - применять для всех)
        &sortByCountDynamic - изменять порядок вывода значений после каждого выбора так, чтобы поддерживать сортировку по количеству доступных вариантов (допустимое значение - 1)
        &DLFilterType=`tv` - тип применяемого DocLister фильтра (default tvd)
        &endings - список из тре слов для plural через запятую (по-умолчанию 'товар,товара,товаров')
        &cntTpl - шаблон для вывода надписи о количестве найденных в форму (по-умолчанию 'Найдено: [+cnt+] [+ending+]')
        &tvConfig - конфиг tovarparams для формирования фильтра (по-умолчанию берется конфиг текущей категории либо первый непустой конфиг родителя)
        &fp - массив для передачи данных запроса на фильтрацию (по-умолчанию используется массив $_GET)
        &submitDocPage=`1` - сабмит формы будет произведен на страницу, заданную в docid
        &submitPage=`25` - id страницы, на которую нужно сабмитить форму (имеет приоритет перед submitDocPage)
        &allowZero=`1` - разрешает 0 в значениях чекбокса
        &useRegexp=`1` - только для сильных духом:)
        &commaAsSeparator=`all`||&commaAsSeparator=`1,2,3` - распознавать запятую как разделитель значений для всех TV (all) либо только для указанных по id (1,2,3)
        &useMultiCategories=`1` - используется плагин MultiCategories для прикрепления товара к множественным категориям
        &evobabel=`1` - для использования переводов evoBabel для названий значений, названий фильтров и категорий фильтров
        &blang=`1` - для использования переводов bLang для названий значений, названий фильтров и категорий фильтров
        &translator=`myCustomTranslator::translate` - для использования собственного переводчика (передается 2 параметра - строка для перевода и значение по-умолчанию)
        &hideSingleVariant=`1` - скрывать блок, если доступен только 1 вариант выбора
<br>

**Eсли хотим искать только по заданным документам, то до вызова [!eFilter!] устанавливаем их спискок в плейсхолдер eFilter_search_ids**<br>
<br>
в режиме ajax три callback-функции javascript
    - beforeFilterSend(_form) - исполняется до отправки состояния формы на сервер<br>
    - afterFilterSend(msg) - исполняется после получения ответа от сервера msg<br>
    - afterFilterComplete(_form) - исполняется после обновления фильтра и результатов поиска из ответа на сервере<br>
<br>
js-события (вызывать $(document).on("before-efilter-send") ...
    - "before-efilter-send", [ _form, updateAll ] - перед отправкой данных на сервер<br>
    - "before-efilter-update", [ msg, _form, updateAll ] - при получении данных с сервера перед вставкой на страницу<br>
    - "after-efilter-update", [ msg, _form, updateAll ] - после вставки новых данных на страницу<br>
    - "after-efilter-change-state", [ state ] - после изменения url в адресной строке<br>
<br>
### пример вызова сниппета eFilterResult - для вывода результатов фильтрации (выведет в том числе и пагинацию)

        [!eFilterResult? &parents=`[*id*]` &depth=`3` &paginate=`pages` &display=`15` &tvList=`image,price`!]

        в данном сниппете используется DocLister, собственно, параметры вызова аналогичны.
        В чанке вывода товара в список &tpl плейсхолдер [+params+] будет заменен на список установленных для данной категории товаров параметров товара
        В параметре &tvList=`image,price` включаем нужные параметры из других категорий, а также общих tv-параметров для всех видов товаров

### полезно
В нужном месте шаблона вывода товара (он у нас общий для всех видов товаров) помещаем вызов сниппета [[tovarParams]] для вывода нужного списка нужных ТВ из категории "Параметры товара" (id этой категории задан в модуле и он важен для всей работы).


### Сотрудничество:
---------
По вопросам сотрудничества обращайтесь на электронный ящик web-ber12@yandex.ru
