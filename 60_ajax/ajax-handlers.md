## Обработчики AJAX-событий {#ajax}

Для удобства разработки AJAX-запросы бросают несколько событий, и их, естественно, можно и нужно обрабатывать. jQuery позволяет обрабатывать эти события для каждого AJAX-запроса и в отдельности и глобально. Приведу схемку, на которой наглядно виден порядок возникновения событий в jQuery:

<div class="mxgraph" style="max-width:100%;border:1px solid transparent;" data-mxgraph="{&quot;highlight&quot;:&quot;#FFFFFF&quot;,&quot;nav&quot;:true,&quot;resize&quot;:true,&quot;toolbar&quot;:&quot;zoom layers lightbox&quot;,&quot;edit&quot;:&quot;_blank&quot;,&quot;url&quot;:&quot;https://raw.githubusercontent.com/AntonShevchuk/jquery-book/master/assets/ajax-events.xml&quot;}"></div>
<script type="text/javascript" src="https://www.draw.io/embed2.js?&fetch=https%3A%2F%2Fraw.githubusercontent.com%2FAntonShevchuk%2Fjquery-book%2Fmaster%2Fassets%2Fajax-events.xml"></script>

Вот и полный список событий с небольшими ремарками:

`ajaxStart` — данное событие возникает в случае, когда побежал первый AJAX-запрос, и при этом других активных AJAX-запросов в данный момент нет

`beforeSend` — возникает до отправки запроса, и позволяет редактировать XMLHttpRequest, локальное событие

`ajaxSend` — возникает до отправки запроса, как и «beforeSend»

`success` — возникает по возвращении ответа, когда в нём нет ошибок, локальное событие

`ajaxSuccess` — возникает по возвращении ответа, аналогично событию «success»

`error` — возникает в случае ошибки, локальное событие

`ajaxError` — возникает в случае ошибки

`complete` — возникает по завершении текущего AJAX-запроса при любом раскладе, локальное событие

`ajaxComplete` — глобальное событие, аналогичное complete

`ajaxStop` — данное событие возникает в случае, когда больше нет активных AJAX-запросов

Пример для отображения элемента с «id="loading"» во время выполнения любого AJAX-запроса (т.е. мы обрабатываем глобальное событие):

```javascript
$(document).on("ajaxSend", function() {
    $("#loading").show(); // показываем элемент
}).on("ajaxStop", function(){
    $("#loading").hide(); // скрываем элемент
});
```

> _Это задачка по юзабилити – мы всегда должны держать пользователя сайта в курсе дела о происходящем на странице и отправка AJAX-запроса тоже попадает под разряд «must know». Подобное решение у вас будет практически на любом сайте, где ходит AJAX._

Для локальных событий – вносим изменения в опции метода «.ajax()»:

```javascript
$.ajax({
    beforeSend: function() {
        // данный обработчик будет вызван перед отправкой данного AJAX-запроса
    },
    success: function() {
        // а этот при удачном завершении
    },
    error: function() {
        // этот при возникновении ошибки
    },
    complete: function() {
        // и по завершении запроса (удачном или нет)
    }
});
```

Можно глобальные обработчики отключить принудительно, используя флаг «global». Для этого выставляем его на «false» и пишем функционал в обход событий «ajaxStart» и «ajaxStop»:

```javascript
$.ajax({
    global: false,
    beforeSend: function() {
        // ...
    },
    success: function() {
        // ...
    },
    error: function() {
        // ...
    },
    complete: function() {
        // ...
    }
});
```

> _Данная опция частенько помогает избежать конфликтов при работе с AJAX-запросами, но не стоит ей злоупотреблять._
