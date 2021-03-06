## Свойства {#properties}

Кроме атрибутов также есть свойства элементов, к ним относятся «selectedIndex», «tagName», «nodeName», «nodeType», «ownerDocument», «defaultChecked» и «defaultSelected». Ну, вроде бы, список невелик, можно и запомнить. Для работы со свойствами используем методы из семейства «.prop()»:

`prop(propName)` — получение значения свойства

`prop(propName, propValue)` — установка значения свойства (также можно использовать hash либо функцию обратного вызова)

`removeProp(propName)` — удаление свойства (скорей всего, никогда не понадобится)

А теперь выключите музыку, и запомните следующее – **для отключения элементов формы и для проверки/изменения состояния чекбоксов мы всегда используем метод «.prop()»,** пусть вас не смущает наличие одноименных атрибутов в HTML (это я про «disabled» и «checked»), используем «.prop()» и точка.

Давайте на примере [properties.html](../code/properties.html):

{% jqbFrame "properties-example", "../code/properties.html", height="660px" %}
{% sticky %}
{% endjqbFrame %}

Посмотрите, как работает система без нашего вмешательства — кликните чекбокс, селектбокс, попробуйте отправить форму.

Теперь приступим к серии экспериментов (не забудьте обновить страничку):

1. Ставим галочку на чекбоксе посредством метода «.attr()» — {% jqbScript "#properties-example" %}$("#checkbox").attr("checked", "checked"){% endjqbScript %}
2. Теперь снимите галочку мышкой — значение «.attr()» осталось без изменений, значение «.prop()» изменилось
3. Попробуйте ещё раз поставить галочку, используя метод «.attr()»

> _Небольшое пояснение сути происходящего. При первом вызове метода «.attr("checked", "checked")» проставляется галочка, т.к. изменяются и атрибут и свойство «checked». При повторном вызове уже ничего не происходит, меняется только значение атрибута, которое и так уже «checked»._

Следующий эксперимент:

1. Поставьте мышкой галочку на чекбоксе
2. Снимите галочку — значение «.attr()» не изменяется
3. Попробуйте установить значение посредством вызова {% jqbScript "#properties-example" %}$("#checkbox").attr("checked", "checked"){% endjqbScript %}

> _В данном эксперименте интересен следующий момент: вызов метода «.attr("checked", "checked")» не срабатывает после того, как пользователь изменял статус чекбокса_

Ну и ещё один эксперимент со вторым чекбоксом:

1. Удаляем галочку — {% jqbScript "#properties-example" %}$("#checkbox-two").removeAttr("checked"){% endjqbScript %}
2. Ставим галочку — {% jqbScript "#properties-example" %}$("#checkbox-two").attr("checked", "checked"){% endjqbScript %}
3. Опять удаляем галочку, используя метод «.attr()»
4. Повторяем до упаду 

> _Работает — не трожь, мышкой всё сломаете :)_

Сравните с поведением метода «.prop()»:

1. Удаляем галочку — {% jqbScript "#properties-example" %}$("#checkbox-two").prop("checked", false){% endjqbScript %}
2. Ставим галочку — {% jqbScript "#properties-example" %}$("#checkbox-two").prop("checked", true){% endjqbScript %}
3. Можем кликать мышкой по чекбоксу и повторять предыдущие пункты в произвольном порядке, всё будет работать как часы

> _Надеюсь, я достаточно наглядно дал понять, когда надо использовать «.attr()», а когда «.prop()»_

Это ещё не всё, у нас же есть ещё свойство «disabled»! Но не волнуйтесь, его поведение более предсказуемо, т.к. пользователь не может вмешиваться в его состояние:

1. Включаем радио-кнопку — {% jqbScript "#properties-example" %}$("#radio-two").attr("disabled", false){% endjqbScript %}
2. Выключаем — {% jqbScript "#properties-example" %}$("#radio-two").attr("disabled", true){% endjqbScript %}
3. Повторяем

Аналогичное поведение при использовании метода «.prop()»:

1. Включаем — {% jqbScript "#properties-example" %}$("#radio-two").prop("disabled", false){% endjqbScript %}
2. Выключаем — {% jqbScript "#properties-example" %}$("#radio-two").prop("disabled", true){% endjqbScript %}
3. Повторяем

> _Ну, как бы, можно использовать «.attr()», но нет!_
