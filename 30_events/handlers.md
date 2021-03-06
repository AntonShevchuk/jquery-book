## Работа с событиями

Для работы с событиями существует три основных метода:

`on(event, handler)` – добавление обработчика (тут я использую простейшую сигнатуру метода)

`trigger(event)` – инициация события из скрипта

`off(event)` – отключение обработчика событий

Давайте рассмотрим метод «.on()»:

{% jqbRun "#handlers-example" %}{% endjqbRun %}
```javascript
// вешаем обработчик
$("p").on("click", function() {
    // что-то делаем
    alert("Click!");
});
```

Запустите код выше, и попробуйте кликнуть по параграфу:

{% jqbFrame "handlers-example", "../code/events.handlers.html", height="200px" %}{% endjqbFrame %}

Можете данный обработчик запустить программно:

{% jqbRun "#handlers-example" %}{% endjqbRun %}
```javascript
$("p").trigger("click");
```

Когда наиграетесь, можете отключить обработчик с помощью метода «.off()»:

{% jqbRun "#handlers-example" %}{% endjqbRun %}
```javascript
$("p").off("click");
```

Хотя я ещё хотел упомянуть один важный момент – внутри обработчика вы можете получить доступ к DOM-элементу используя ключевое слово `this`. Если же надо будет воспользоваться jQuery-инструментами, то используйте конструкцию «$(this)»: 


{% jqbRun "#handlers-example" %}{% endjqbRun %}
```javascript
$("p").on("click", function() {
    $(this).css("color", "red");
});
```
