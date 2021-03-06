## Оптимизируем выборки

Ну, перво-наперво, вам следует запомнить, что

**_результаты поиска не кэшируются — каждый раз, запрашивая элементы по селектору, вы инициируете поиск элементов снова и снова_**

При ознакомлении с алгоритмом работы Sizzle, сходу напрашиваются несколько советов об оптимизации по работе с выборками:

* Сохранять результаты поиска (исходя из постулата выше):

```javascript
// было
$("a.button").addClass("active");
/* ... */
$("a.button").click(function(){ /* ... */ });

// стало
var $button = $("a.button");
$button.addClass("active");
/* ... .*/
$button.click(function(){ /* ... */ });
```

> _Правильная IDE о подобных вещах знает, и будет вам время от времени подсказывать ;)_

* Использовать цепочки вызовов (что по сути аналогично первому правилу):

```javascript
// было
$("a.button").addClass("active");
$("a.button").click(function(){ /* ... */ });

// стало
$("a.button").addClass("active")
    .click(function(){ /* ... */ });
```

* Использовать «context» (это такой второй параметр при выборе по селектору):

```javascript
// было
$(".content a.button");

// стало
$("a.button", ".content");

// ещё вариант, не быстрее, но читаемость выше
$(".content").find("a.button");
```

* Разбивать запрос на более простые составные части, используя «context», и сохранять промежуточные данные:

```javascript
// было
$(".content a.button");
$(".content h3.title");

// стало
var $content = $(".content")
$content.find("a.button");
$content.find("h3.title");
```

* Использовать более «съедобные» селекторы, дабы помочь методу «.querySelectorAll()», т.е. если у вас нет уверенности в правильности написания селектора, или вы сомневаетесь в том, что все браузеры поддерживают необходимый CSS-селектор, то лучше разделить «сложный» селектор на несколько более простых:

```javascript
// было
$(".content div input:disabled");

// стало
$(".content div").find("input:disabled");
```

* Не использовать jQuery, а работать с «native» функциями JavaScript

> _Есть ещё один пункт – выбирать самый быстрый селектор из возможных, но тут без хорошего багажа знаний не обойтись, так что дерзайте, пробуйте и присылайте ваши примеры._

Для наглядности лучше всего взглянуть на сравнительный тест [benchmark.html](../code/benchmark.html):

{% jqbFrame "benchmark", "../code/benchmark.html", height="160px" %}{% endjqbFrame %}

> _Данный тест выполняет поиск элементов несколькими способами и был изначально разработан Ильёй Кантором для [мастер-класса по JavaScript и jQuery](http://javascript.ru/mk)_

> _Маленькая хитрость от создателей jQuery – запросы по id элемента не доходят до Sizzle, а скармливаются «document.getElementById()» в качестве параметра:_

```javascript
$("#content");
// -> 
document.getElementById("content");
```

### Примеры оптимизаций

Выбор по идентификатору элемента — самый быстрый из возможных, старайтесь использовать оный:

```javascript
// внутри одна регулярочка + getElementById()
$("#content");

// вот так ещё быстрее
// экономия незначительна, а удобство использования стремится к нулю
$(document.getElementById("content"));
```

Селектор «div#content» работает на порядок медленнее, нежели поиск лишь по идентификатору «#content», но и он имеет право на существование в случае, если ваш скрипт используется на нескольких страницах, а логика требует лишь обрабатывать поведение для элемента `<div>`. Данный селектор можно представить в двух вариантах:

```javascript
// getElementById() + фильтрация
$("#content").filter("div");

// оставляем как есть и надеемся на QuerySelectorAll()
$("div#content");
```

В результате [тестирования](http://jsperf.com/div-id) получаем следующий расклад:

* пример с использованием «.filter()» работает быстрее в браузерах Chrome, Firefox и IE9.0+
* оба способа работают наравне в браузерах IE8.0 и мобильном Safari
* второй пример работает в два раза быстрее в старых версиях Opera на движке <a href="https://ru.wikipedia.org/wiki/Presto_(Opera)">Presto</a>

Выводы делаем сами.
