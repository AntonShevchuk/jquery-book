# 20% Атрибуты элементов и CSS {#20}

В предыдущих примерах мы уже изменяли CSS-свойства DOM-элементов, используя одноимённый метод «.css()», но это далеко не всё. Теперь копнём поглубже, чтобы не штурмовать форумы банальными вопросами ;)

Копать начнём с более досконального изучения метода «.css()»:

`css(property)` — получение значения CSS-свойства

`css(property, value)` — установка значения CSS-свойства

`css({property:value, property:value})` — установка нескольких значений

`css(property, function(index, value) { return value })` — тут для установки значения используется функция обратного вызова (в просторечии — callback-функция), index это порядковый номер элемента в выборке, value — старое значение свойства

> _Метод «.css()» возвращает текущее значение, а не прописанное в CSS-файле по указанному селектору._

Примеры использования ([css.html](http://anton.shevchuk.name/book/code/css.html)):

```javascript
// получаем значение цвета шрифта
$("#my").css('color');

// устанавливаем значение цвета шрифта
$("#my").css('color', 'red'); 

// установка нескольких значений
$("#my").css({
    'color':'red',
    'font-size':'14px',
    'margin-left':'10px'
})

// альтернативный способ
$("#my").css({
    color:'red',
    fontSize:'14px',
    marginLeft:'10px'
})

// используя функцию обратного вызова
$("#my").css('height', function(i, value){
    return parseFloat(value) * 1.2;
})
```

Ну, вроде с CSS разобрались, хотя нет — стоит ещё описать манипуляции с классами, тоже из разряда первичных навыков:

`addClass(className)` — добавление класса элементу

`addClass(function(index, currentClass){ return className })` — добавление класса с помощью функции обратного вызова

`hasClass(className)` — проверка на причастность к определённому классу

`removeClass(className)` — удаление класса

`removeClass(function(index, currentClass){ return className })` — удаление класса с помощью функции обратного вызова

`toggleClass(className)` — переключение класса

`toggleClass(className, switch)` — переключение класса по флагу switch

`toggleClass(function(index, currentClass, switch){ return className }, switch)` — переключение класса с помощью функции обратного вызова

> _В приведённых методах в качестве «className» может быть строка, содержащая список классов через пробел._

> _Мне ни разу не приходилось использовать данные методы с функциями обратного вызова, и лишь единожды пригодился флаг «switch», так что не заморачивайтесь всё это запоминать, да и в дальнейшем, цитируя руководство по jQuery, я буду сознательно опускать некоторые «возможности»._

Но хватит заниматься переводом официальной документации, перейдём к наглядным примерам ([class.html](http://anton.shevchuk.name/book/code/class.html)):

```javascript
// добавляем несколько классов за раз
$("#my").addClass('active notice')

// переключаем несколько классов
$("#my").toggleClass('active notice')

// работает вот так (похоже на классовый XOR):
<div id="my" class="active notice"> → <div id="my" class="">

<div id="my" class="active"> → <div id="my" class="notice">

<div id="my" class=""> → <div id="my" class="active notice">

// аналогично предыдущему примеру
$("#my").toggleClass('active')
$("#my").toggleClass('notice')

// проверяем наличие класса(-ов)
$("#my").hasClass('active')

// удаляем несколько классов за раз
$("#my").removeClass('active notice')

```

Также, стоит вспомнить, что у DOM-элементов бывают атрибуты отличные от класса, и мы их тоже можем изменять. Для этого нам потребуются следующие методы:

`attr(attrName)` — получение значения атрибута

`attr(attrName, attrValue)` — установка значения атрибута (также можно использовать hash либо функцию обратного вызова)

`removeAttr(attrName)` — удаление атрибута

Атрибуты – это всё то, что мы видим внутри угловых скобочек, когда пишем HTML-код:

```html
<!-- В данном примере это href, title, class -->
<a href="#top" title="anchor" class="simple">To Top</a>
```

Атрибуты, с которыми вам чаще других придётся сталкиваться:

```javascript
// получение альтернативного текста картинки

var altText = $('img').attr('alt')

// изменение адреса картинки
$('img').attr('src', '/images/default.png')

// работаем со ссылками
$('a#my').attr({
    'href':'http://anton.shevchuk.name',
    'title':'My Personal Blog'
});
```

Кроме атрибутов также есть свойства элементов, к ним относятся «selectedIndex», «tagName», «nodeName», «nodeType», «ownerDocument», «defaultChecked» и «defaultSelected». Ну, вроде бы, список невелик, можно и запомнить. Для работы со свойствами используем методы из семейства «.prop()»:

`prop(propName)` — получение значения свойства

`prop(propName, propValue)` — установка значения свойства (также можно использовать hash либо функцию обратного вызова)

`removeProp(propName)` — удаление свойства (скорей всего, никогда не понадобится)

А теперь выключите музыку, и запомните следующее – **для отключения элементов формы, и для проверки/изменения состояния чекбоксов мы всегда используем метод «.prop()»,** пусть вас не смущает наличие одноименных атрибутов в HTML (это я про «disabled» и «checked»), используем «.prop()» и точка (наглядный пример [property.html](http://anton.shevchuk.name/book/code/property.html))