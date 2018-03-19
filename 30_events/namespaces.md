## Пространство имен

Как вы уже узнали, когда мы хотим создать свой обработчик событий, мы пишем вот такой код:

<button class="jqbook run" data-target="#handlers-example">▷</button>

```javascript
// создаем свой обработчик
$("p").on("click", function() {
    // что-то делаем
    alert("Click!");
});
```

Когда нам надо удалить обработчики, используем следующий код:

<button class="jqbook run" data-target="#handlers-example">▷</button>

```javascript
// удаляем все обработчики
$("p").off();

// удаляем все обработчики click
$("p").off("click");
```

Для экспериментов воспользуемся уже знакомым примером:

<div class="jqbook">
<button class="jqbook sticky">📌</button>
<iframe class="jqbook" id="handlers-example" width="100%" height="200px" border="0" src="../code/events.handlers.html"></iframe>
</div>

Но, как всегда, есть ситуации, когда нам необходимо задействовать или отключить не все обработчики (как пример, надо отключить обработку какого-то контрола определённым плагином). В этом случае нам на помощь приходят пространства имён, и использовать их достаточно легко.

При создании обработчика события добавляем «namespace» через точку:

<button class="jqbook run" data-target="#handlers-example">▷</button>

```javascript
// создаём обработчик
$("p").on("click.namespace", function(event){
    // что-то делаем
    alert("Click: " + event.namespace + "!");
});
```

Когда нам надо вызвать обработчик события привязанный только к нашему «namespace» используем следующий синтаксис:

<button class="jqbook run" data-target="#handlers-example">▷</button>

```javascript
$("p").trigger("click.namespace");
```

Когда вызываем событие из другого пространства имён, наш обработчик не будет вызван:

<button class="jqbook run" data-target="#handlers-example">▷</button>

```javascript
$("p").trigger("click.other");
```

Когда вызываем все-все обработчики событий:

<button class="jqbook run" data-target="#handlers-example">▷</button>

```javascript
// вызываем все обработчики события click
$("p").trigger("click");
```

Когда вызываем все обработчики без пространства имён:

<button class="jqbook run" data-target="#handlers-example">▷</button>

```javascript
// вызываем все обработчики без пространства имён
$("p").trigger("click.$");
```

> _До версии 1.9 для это цели следовало использовать имя события с восклицательным знаком «click!», а после — вот такой workaround завязанный на недостатке одного регулярного выражения._

И последний случай — удаление обработчика событий привязанного к нашему «namespace»:

<button class="jqbook run" data-target="#handlers-example">▷</button>

```javascript
// удаляем все обработчики click в данном пространстве имён
$("p").off("click.namespace");
```

Хот можно одним махом удалить все обработчики из определённого пространства имён:

<button class="jqbook run" data-target="#handlers-example">▷</button>

```javascript
// обработчик клика
$("p").on("click.color", function() {
  $(this).css("color", "red");
});

// обработчик при наведении
$("p").on("mouseenter.color", function() {
  $(this).css("color", "green");
});
```

<button class="jqbook run" data-target="#handlers-example">▷</button>

```javascript
// передумали, и все отменили
$("p").off(".namespace");
```

Ещё полезный пример хитрого обработчика — он может ловить и обрабатывать данные:

<button class="jqbook run" data-target="#handlers-example">▷</button>

```javascript
// создаём обработчик
$("p").on("click.data", function(event, one, two, three) {
    alert("Click data: " + one + ", " + two + ", " + three);
});
```

Иницируем обработку события, в качестве данных передаём массив аргументов: 

<button class="jqbook run" data-target="#handlers-example">▷</button>

```javascript
$("p").trigger("click.data", [1, 2, 3]);
```

Так же хотел обратить внимание на поддержку нескольких пространств имён:

<button class="jqbook run" data-target="#handlers-example">▷</button>

```javascript
// создаём обработчик для a
$("p").on("click.a", function(event) {
    alert("Click A!");
});

// создаём обработчик для b
$("p").on("click.b", function(event) {
    alert("Click B!");
});

// создаём обработчик для пространства a и b
$("p").on("click.a.b", function(event) {
    // для пространства имён a и b    
    alert("Click: " + event.namespace + "!");
});
```

<button class="jqbook run" data-target="#handlers-example">▷</button>

```javascript
// вызываем обработчик из пространства a
$("p").trigger("click.a");
```

<button class="jqbook run" data-target="#handlers-example">▷</button>

```javascript
// вызываем обработчик из пространства b
$("p").trigger("click.b");
```

<button class="jqbook run" data-target="#handlers-example">▷</button>

```javascript
// отменяем обработчик click для пространства b
$("p").off("click.b");
```

[Официальная документация](http://api.jquery.com/event.namespace/) скудна на этот счёт, и я надеюсь, мои примеры помогли вам лучше разобраться в данном вопросе.
