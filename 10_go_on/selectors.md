## Селекторы

Как я уже говорил ранее, в поиске элементов на странице заключается практически половина успешной работы с jQuery.
Так что приступим к поискам по документу (данный пример кода вы можете закрепить с помощью кнопки «📌»):

{% jqbFrame "html-example", "../code/css.selectors.html", height="700px" %}
{% sticky %}
{% endjqbFrame %}

А теперь приступим к выборкам — выбор элементов по «id» либо имени класса, аналогично используемым в CSS:


{% jqbHighlight "#html-example" %}$("#content"){% endjqbHighlight %} – выбираем элемент с «id="content"»

{% jqbHighlight "#html-example" %}$("section#content"){% endjqbHighlight %} – выбираем `<section>` с «id="content"»

{% jqbHighlight "#html-example" %}$(".intro"){% endjqbHighlight %} – выбираем элементы с «class="intro"»

{% jqbHighlight "#html-example" %}$("p.intro"){% endjqbHighlight %} – выбираем всё «p» с «class="intro"»

{% jqbHighlight "#html-example" %}$(".intro.pinned"){% endjqbHighlight %} – выбираем элементы с классами «intro» и «pinned»

{% jqbHighlight "#html-example" %}$("h3"){% endjqbHighlight %} – выбираем все теги `<h3>`

{% jqbHighlight "#html-example" %}$("h1, h2"){% endjqbHighlight %} – выбираем все теги `<h1>` и `<h2>`

_Используйте валидные имена классов и идентификаторов_

Теперь вспомним, что мы в DOMе не одни, это-таки иерархическая структура:

{% jqbHighlight "#html-example" %}$("article h3"){% endjqbHighlight %} – выбираем все теги `<h3>` внутри тега `<article>`

{% jqbHighlight "#html-example" %}$("article").find("h3"){% endjqbHighlight %} – аналогично примеру выше

{% jqbHighlight "#html-example" %}$("section article h3"){% endjqbHighlight %} – выбираем все теги `<h3>` внутри тега `<article>`, которые находятся внутри тега `<section>`, _в DOMе который построил Джек_
  
{% jqbHighlight "#html-example" %}$("section").find("article").find("h3"){% endjqbHighlight %} – и ещё раз, но на другой лад

У нас есть соседи, и у нас с ними налажен контакт. Вот вам несколько способов как найти один и тот же элемент:

{% jqbHighlight "#html-example" %}$("article + article"){% endjqbHighlight %} – выбор всех элементов `<article>`, перед которыми есть тег `<article>`

{% jqbHighlight "#html-example" %}$("#stick ~ article"){% endjqbHighlight %} – выбор всех элементов `<article>` после элемента с «id="stick"»

{% jqbHighlight "#html-example" %}$("#stick").next(){% endjqbHighlight %} – выбор следующего элемента после элемента с «id="stick"»

Родственные связи:

`$("*")` – выбор всех элементов; **никогда не используйте!**

{% jqbHighlight "#html-example" %}$("article > h3"){% endjqbHighlight %} – выбираем все теги `<h3>`, которые являются непосредственными потомками тега `<article>`

{% jqbHighlight "#html-example" %}$("article > *"){% endjqbHighlight %} – выбор всех потомков элементов `<article>`

{% jqbHighlight "#html-example" %}$("article").children(){% endjqbHighlight %} – аналогично примеру выше

{% jqbHighlight "#html-example" %}$("p").parent(){% endjqbHighlight %} – выбор всех прямых предков элементов `<p>`

{% jqbHighlight "#html-example" %}$("p").parents(){% endjqbHighlight %} – выбор всех предков элементов `<p>`; очень экзотичная задача, вряд ли понадобится

{% jqbHighlight "#html-example" %}$("p").parents("section"){% endjqbHighlight %} – выбор всех предков элемента `<p>`, которые есть `<section>` (`parents()` принимает в качестве параметра селектор)

_Если хотите поиграться с селекторами от души, то откройте соответствующую страничку [css.selectors.html](../code/css.selectors.html) в новой вкладке, и с использованием консоли запустите скрипт `$("p").effect("highlight")`_

Когда с помощью перечисленных запросов вы нашли (или не нашли) DOM-элементы, вам вернётся jQuery-объект, который будет содержать массив этих элементов. Вот так это будет выглядеть для запроса {% jqbHighlight "#html-example" %}$("p"){% endjqbHighlight %}:

![jQuery length](/assets/img/jquery-length.png)

Возможно, вы заметили свойство `length`. Да-да, именно так, это количество найденных элементов. Так что мы можем легко получить оное число с помощью следующего кода:

{% jqbRun "#html-example" %}{% endjqbRun %}
```javascript
alert( $("p").length )
```

Если перед вами стоит задача достать найденный DOM-элемент, то вы сможете это сделать, зная его индекс. По сути, это выглядит как обращение к элементу массива:

{% jqbRun "#html-example" %}{% endjqbRun %}
```javascript
// мы ищем все параграфы
// берём первый из них
// берём текст параграфа
// возвращаем длину текста
alert( $("p")[0].innerText.length )
```

Если вам не нравится данный способ из эстетических соображений, то вы можете воспользоваться методом «.get()»:

{% jqbRun "#html-example" %}{% endjqbRun %}
```javascript
alert( $("p").get(0).innerText.length )
```

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
