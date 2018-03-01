## Селекторы

Как я уже говорил ранее, в поиске элементов на странице заключается практически половина успешной работы с jQuery.
Так что приступим к поискам по документу (чтобы не листать, пусть пример HTML будет и тут):

```html
<!DOCTYPE html>
<html dir="ltr" lang="en-US">
<head>
  <meta charset="UTF-8"/>
  <title>Page Title</title>
</head>
<body>
<header>
  <h1>Page Title</h1>
  <p>Page Description</p>
</header>
<div id="content" class="wrapper box">
  <article>
    <h2>Article Title</h2>
    <p>Lorem ipsum dolor sit amet, consectetuer adipiscing.
      Nunc urna metus, ultricies eu, congue, laoreet...</p>
  </article>
  <hr id="stick"/>
  <article>
    <h2>Article Title</h2>
    <p>Morbi malesuada, ante at feugiat tincidunt, enim
      gravida metus, lacinia massa diam vel eros...</p>
  </article>
</div>
<footer>&copy;copyright 2018</footer>
</body>
</html>
```

А теперь приступим к выборкам — выбор элементов по «id» либо имени класса, аналогично используемым в CSS:

`$("#content")` – выбираем элемент с «id="content"»

`$("div#content")` – выбираем `<div>` с «id="content"» (хотя и без div работает)

`$(".wrapper")` – выбираем элементы с «class="wrapper"»

`$("div.wrapper")` – выбираем div'ы с «class="wrapper"»

`$(".wrapper.box")` – выбираем элементы с классами «wrapper» и «box»

`$("h2")` – выбираем все теги `<h2>`

`$("h1, h2")` – выбираем все теги `<h1>` и `<h2>`

_Используйте валидные имена классов и идентификаторов_

Теперь вспомним, что мы в DOMе не одни, это-таки иерархическая структура:

`$("article h2")` – выбираем все теги `<h2>` внутри тега `<article>`
`$("article").find("h2")` – аналогично примеру выше

`$("div article h2")` – выбираем все теги `<h2>` внутри тега `<article>`, которые находятся внутри тега `<div>`, 
_в DOMе который построил Джек_
`$("div").find("article").find("h2")` – и ещё раз, но на другой лад

У нас есть соседи, и у нас с ними налажен контакт:

`$("h1 + p")` – выбор всех элементов `<p>`, перед которыми есть тег `<h1>`

`$("#stick ~ article")` – выбор всех элементов `<article>` после элемента с «id="stick"» (у нас только один такой)

`$("#stick").prev()` – выбор предыдущего элемента от найденного

`$("#stick").next()` – выбор следующего элемента от найденного

Родственные связи:

`$("*")` – выбор всех элементов; никогда не используйте!

`$("article > h2")` – выбираем все теги `<h2>`, которые являются непосредственными потомками тега `<article>`

`$("article > *")` – выбор всех потомков элементов `<article>`

`$("article").children()` – аналогично примеру выше

`$("p").parent()` – выбор всех прямых предков элементов `<p>`

`$("p").parents()` – выбор всех предков элементов `<p>`; очень экзотичная задача, вряд ли понадобится

`$("p").parents("div")` – выбор всех предков элемента `<p>`, которые есть `<div>` (`parents()` принимает в качестве параметра селектор)

_Если хотите поиграться с селекторами от души, то я припас для вас соответствующую страничку — [css.selectors.html](http://anton.shevchuk.name/book/code/css.selectors.html)_

### Поиск по атрибутам

Ещё со времён CSS 2 была возможность найти элемент с определёнными атрибутами, в CSS 3 добавили ещё возможностей по поиску:

`a[href]` — все ссылки с атрибутом «href»

`a[href=#]` — все ссылки с «href=#»

`a[href~=#]` — все ссылки с «#» где-то в «href»

`a[hreflang|=en]` — все ссылки, для которых hreflang начинается с «en» и обрезается по символу «-» — «en», «en-US», «en-UK»

`a[href^=http]` — ссылки, начинающиеся с «http»

`a[href*="google.com"]` — ссылки на погуглить

`a[href$=.pdf]` — ссылки на PDF-файлы (по идее)

_Заглянув внутрь jQuery, вы, скорей всего, найдёте то место, где ваше выражение будет анализироваться с помощью регулярных выражений, по этой причине в селекторах необходимо экранировать специальные символы, используя двойной обратный слеш «\\»:_

```javascript
$("a[href^=\\/]").addClass("internal");
```

### Поиск по дочерним элементам

Хотелось бы ещё обратить внимание на селекторы из спецификации [CSS 3](http://www.w3.org/TR/css3-selectors/) — много интересных:

`:first-child` — первый дочерний элемент

`:last-child` — последний дочерний элемент

`:nth-child(2n+1)` — выборка элементов по несложному уравнению
  подробнее можно прочитать в статье «[Как работает nth-child](http://web-standards.ru/articles/nth-child/)»

`:not(…)` — выбрать те, что не подпадают под вложенную выборку

Но поскольку не все браузеры знакомы с селекторами из CSS 3, то мы можем использовать jQuery для назначения стилей:

```javascript
$("div:last-child").addClass("last-paragraph");
```