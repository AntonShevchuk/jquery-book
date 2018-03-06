## Селекторы

Как я уже говорил ранее, в поиске элементов на странице заключается практически половина успешной работы с jQuery.
Так что приступим к поискам по документу (чтобы не листать, пусть пример HTML будет и тут):

<div class="jqbook">
<button class="jqbook">📌</button>
<iframe class="jqbook" id="html-example" width="100%" height="700px" border="0" src="../code/css.selectors.html"></iframe>
</div>

А теперь приступим к выборкам — выбор элементов по «id» либо имени класса, аналогично используемым в CSS:

<a class="jqbook" href="#" data-target="#html-example" data-type="highlight">$("#content")</a> – выбираем элемент с «id="content"»

<a class="jqbook" href="#" data-target="#html-example" data-type="highlight">$("section#content")</a> – выбираем `<section>` с «id="content"» (хотя и без «section работает)

<a class="jqbook" href="#" data-target="#html-example" data-type="highlight">$(".intro")</a> – выбираем элементы с «class="intro"»

<a class="jqbook" href="#" data-target="#html-example" data-type="highlight">$("p.intro")</a> – выбираем всё «p» с «class="intro"»

<a class="jqbook" href="#" data-target="#html-example" data-type="highlight">$(".intro.pinned")</a> – выбираем элементы с классами «intro» и «pinned»

<a class="jqbook" href="#" data-target="#html-example" data-type="highlight">$("h3")</a> – выбираем все теги `<h3>`

<a class="jqbook" href="#" data-target="#html-example" data-type="highlight">$("h1, h2")</a> – выбираем все теги `<h1>` и `<h2>`

_Используйте валидные имена классов и идентификаторов_

Теперь вспомним, что мы в DOMе не одни, это-таки иерархическая структура:

<a class="jqbook" href="#" data-target="#html-example" data-type="highlight">$("article h3")</a> – выбираем все теги `<h2>` внутри тега `<article>`

<a class="jqbook" href="#" data-target="#html-example" data-type="highlight">$("article").find("h3")</a> – аналогично примеру выше

<a class="jqbook" href="#" data-target="#html-example" data-type="highlight">$("section article h3")</a> – выбираем все теги `<h3>` внутри тега `<article>`, которые находятся внутри тега `<section>`, _в DOMе который построил Джек_
  
<a class="jqbook" href="#" data-target="#html-example" data-type="highlight">$("section").find("article").find("h3")</a> – и ещё раз, но на другой лад

У нас есть соседи, и у нас с ними налажен контакт. Вот вам несколько способов как найти один и тот же элемент:

<a class="jqbook" href="#" data-target="#html-example" data-type="highlight">$("article + article")</a> – выбор всех элементов `<article>`, перед которыми есть тег `<article>`

<a class="jqbook" href="#" data-target="#html-example" data-type="highlight">$("#stick ~ article")</a> – выбор всех элементов `<article>` после элемента с «id="stick"»

<a class="jqbook" href="#" data-target="#html-example" data-type="highlight">$("#stick").next()</a> – выбор следующего элемента после элемента с «id="stick"»

Родственные связи:

`$("*")` – выбор всех элементов; **никогда не используйте!**

<a class="jqbook" href="#" data-target="#html-example" data-type="highlight">$("article > h3")</a> – выбираем все теги `<h3>`, которые являются непосредственными потомками тега `<article>`

<a class="jqbook" href="#" data-target="#html-example" data-type="highlight">$("article > *")</a> – выбор всех потомков элементов `<article>`

<a class="jqbook" href="#" data-target="#html-example" data-type="highlight">$("article").children()</a> – аналогично примеру выше

<a class="jqbook" href="#" data-target="#html-example" data-type="highlight">$("p").parent()</a> – выбор всех прямых предков элементов `<p>`

<a class="jqbook" href="#" data-target="#html-example" data-type="highlight">$("p").parents()</a> – выбор всех предков элементов `<p>`; очень экзотичная задача, вряд ли понадобится

<a class="jqbook" href="#" data-target="#html-example" data-type="highlight">$("p").parents("section")</a> – выбор всех предков элемента `<p>`, которые есть `<section>` (`parents()` принимает в качестве параметра селектор)

_Если хотите поиграться с селекторами от души, то откройте соответствующую страничку [css.selectors.html](../code/css.selectors.html) в новой вкладке, и с использованием консоли запустите скрипт `$("p").effect("highlight")`_

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
