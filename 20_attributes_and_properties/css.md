## CSS стили {#css}

В [предыдущей главе](../10_go_on/be-ready.md) мы уже сталкивались с методом «.css()», с его помощью мы меняли цвет шрифта, но на этом его возможности не заканчиваются. Пора копнуть поглубже, чтобы не штурмовать форумы банальными вопросами ;)

Копать начнём с более досконального изучения метода «.css()»:

`css(property)` — получение значения CSS-свойства

`css(property, value)` — установка значения CSS-свойства

`css({property:value, property:value})` — установка нескольких значений

`css(property, function(index, value) { return value })` — тут для установки значения используется функция обратного вызова (в просторечии — callback-функция), `index` это порядковый номер элемента в выборке, `value` — текущее значение свойства

> _Метод «.css()» возвращает текущее значение, а не прописанное в CSS-файле по указанному селектору._

Вот наш подопытный HTML:

<div class="jqbook">
<button class="jqbook sticky">📌</button>
<iframe class="jqbook" id="css-example" width="100%" height="320px" border="0" src="../code/css.html"></iframe>
</div>

Начнём наши эксперименты (жмите «▷» где возможно):

<a class="jqbook" href="#" data-target="#css-example" data-type="append-script">$("#my").css('color', 'red')</a> — устанавливаем значение цвета шрифта

<a class="jqbook" href="#" data-target="#css-example" data-type="append-script">$("#my").css('background-color', 'yellow')</a> — меняем цвет фона

Для изменения нескольких параметров передаём объект в формате ключ-значение (это фактически JSON):

<button class="jqbook run" data-target="#css-example">▷</button>

```javascript
$("#my").css({
    'color': 'red',
    'font-size': '18px',
    'background-color': 'white'
})
```

Для именования свойств можно использовать как CSS-нотацию (см. пример выше), так и JavaScript вариант:

<button class="jqbook run" data-target="#css-example">▷</button>

```javascript
$("#my").css({
    color: 'black',
    fontSize: '12px',
    backgroundColor: 'transparent'
})
```

А вот перед нами экзотический способ изменения шрифта с использованием функции обратного вызова:

<button class="jqbook run" data-target="#css-example">▷</button>

```javascript
$("#my").css('font-size', function(i, value){
    return parseFloat(value) * 1.5;
})
```
