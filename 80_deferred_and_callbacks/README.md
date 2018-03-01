# 80% Объект Deferred и побратимы {#80}

Работа с объектом «Deferred» – это уже высший пилотаж. Это «mad skills» – заставлять асинхронный JavaScript работать так, как нам хочется. Давайте посмотрим, как он работает (данный код можно скопировать в консоль и выполнить на любой странице, где подключен jQuery 3.x):

> _С jQuery версии 3.x, Deferred объект стал совместим с Promise из ES-2015 (т.н. ES6), так что практически всё, что относится к [Promise](https://learn.javascript.ru/promise) верно и для [Deferred](http://api.jquery.com/category/deferred-object/)._

```javascript
// инициализация Deferred объекта
// статус «ожидает выполнение»
var D = $.Deferred();

// подключаем обработчики
D.then(function() { console.log("first") });
D.then(function() { console.log("second") });

// изменяем статус на «fulfilled» - «выполнен успешно»
// для этого вызываем resolve()
// наши обработчики будут вызваны в порядке очереди, но они не ждут друг друга
D.resolve();

// данный обработчик подключён слишком поздно и будет вызван сразу
D.then(function() { console.log("third") });
```

Если всё это перевести на человеческий язык, получится следующий сценарий:

* если всё будет хорошо, тогда выполни вот эту функцию и выведи «first»
* и ещё вот эту функцию — «second»
* resolve() — мы узнали, всё хорошо
* ~~если~~ всё хорошо, выполняем функцию и выводим «third»

Кроме сценариев с «happy end», есть ещё и грустные истории, когда всё пошло не так, как нам бы хотелось:

```javascript
// инициализация Deferred объекта
var D = $.Deferred();

// подключаем обработчики
D.then(function() { console.log("done") });
D.catch(function() { console.log("fail") });

// изменяем статус на «rejected» - «выполнен с ошибкой»
D.reject();

// в консоли нас будет ожидать лишь «fail» :(
```

Получается так:

* если всё будет хорошо, тогда выполни вот эту функцию — «done»
* если всё будет плохо, тогда вот эта функция выведет «fail»
* ой, всё плохо

В действительности, метод «.then()» позволяет вешать одновременно как обработчики для положительного сценария, так и для варианта с ошибкой:

```javascript
D.then(
    function(result) { console.log("done") },
    function(error) { console.log("fail") }
);
```

Становится ли от этого код читаемым – сомневаюсь, но такой вариант существует и используется повсеместно. Я же предпочитаю для отлова ошибок использовать метод «.catch()», который по своей сути лишь сокращенная запись для «.then(null, fn)»:

```javascript
var D = $.Deferred();

// подключаем обработчик ошибок через then()
D.then(null, function() { console.log("fail") });

// подключаем обработчик ошибок через catch()
D.catch(function() { console.log("again fail") });
```

Ещё упомяну метод «.always()» – он добавляет обработчики, которые будут выполнены вне зависимости от случившегося (в действительности, внутри происходит вызов «.done(arguments)» и «.fail(arguments)»).

Чтобы не путаться в перечисленных методах, приведу блок-схему:

<div class="mxgraph" style="max-width:100%;border:1px solid transparent;" data-mxgraph="{&quot;highlight&quot;:&quot;#FFFFFF&quot;,&quot;nav&quot;:true,&quot;resize&quot;:true,&quot;toolbar&quot;:&quot;zoom layers lightbox&quot;,&quot;edit&quot;:&quot;_blank&quot;,&quot;url&quot;:&quot;https://raw.githubusercontent.com/AntonShevchuk/jquery-book/master/assets/deferred.xml&quot;}"></div>
<script type="text/javascript" src="https://www.draw.io/embed2.js?&fetch=https%3A%2F%2Fraw.githubusercontent.com%2FAntonShevchuk%2Fjquery-book%2Fmaster%2Fassets%2Fdeferred.xml"></script>

При вызове «[.resolve()](http://api.jquery.com/deferred.resolve/)» и «[.reject()](http://api.jquery.com/deferred.reject/)» можно передать произвольные данные в зарегистрированные callback-функции для дальнейшей работы. Кроме того, существуют ещё методы «[.resolveWith()](http://api.jquery.com/deferred.resolveWith/)» и «[.rejectWith()](http://api.jquery.com/deferred.rejectWith/)», они позволяют изменять контекст вызываемых callback-функций (т.е. внутри них «this» будет смотреть на указанный контекст).

Отдельно отмечу, что если вы собираетесь передать Deferred объект «на сторону», чтобы «там» могли повесить свои обработчики событий, но не хотите потерять контроль, то возвращайте не сам объект, а результат выполнения метода «.promise()» – фактически это будет искомый объект в режиме «read-only».

А ещё кроме поведения «ждём чуда» с помощью Deferred можно выстраивать цепочки вызовов – «живые очереди»:

```javascript
$.ajax('ajax/example.json')
  .then(function() {
      // подождём окончания AJAX-запроса
      return $('article img').slideUp(2000).promise()
  })
  .then(function(){
      // подождём, пока спрячутся картинки
      return $('article p').slideUp(2000).promise()
  })
  .then(function(){
      // подождём, пока спрячутся параграфы
      return $('article').hide(2000).promise()
  })
  .then(function(){
      // всё сделано, шеф
  });
```

Подобное поведение можно воспроизвести, используя лишь animate, но нам же хочется заглянуть чуть-чуть поглубже — [deferred.pipe.html](http://anton.shevchuk.name/book/code/deferred.pipe.html) (до jQuery 1.8 тут шла речь о методе «.pipe()», а теперь о «.then()»).

В данном примере мы вызываем метод «.then()», которому скормлена callback-функция, которая должна возвращать объект Promise. Это необходимо для соблюдения порядка в очереди – попробуйте убрать в примере один «return», и вы заметите, что следующая анимация наступит не дождавшись завершения предыдущей.

На этом возможности Deferred ещё не завершились. Есть ещё связка методов «.notify()» и «.progress()» – первый шлёт послания в callback-функции, которые зарегистрированы с помощью второго. Приведу наглядный код для демонстрации (скопируйте в консоль и посмотрите, что получается):

```javascript
var D = $.Deferred();

var money = 100; // наш бюджет

// съём денежки
D.progress(function($){
    console.log(money + " - " + $ + " = " + (money-$));
    money -= $;
    if (money < 0) { // деньги закончились
        D.reject();
    }
});

// тратим деньги
setTimeout(function(){ D.notify(40); }, 500);  // покупка 1
setTimeout(function(){ D.notify(50); }, 1000); // покупка 2
setTimeout(function(){ D.notify(30); }, 1500); // покупка 3

D.then(function(){ console.info("All Ok") });
D.catch(function(){ console.error("Insufficient Funds") });
```

Испытайте всю мощь Deferred в примере [deferred.html](http://anton.shevchuk.name/book/code/deferred.html).

Теперь покажу хитрый метод «$.when()»:

```javascript
$.when(
    $.ajax("/ajax/example.json"),
    $("article").slideUp(200)
).then(function() {
    alert("All done");
}, function(){
    alert("Something wrong");
})
```

Поясню происходящее – AJAX-запрос и анимация стартуют одновременно, а когда и тот и другой завершат свою работу, будет вызвана функция, которую мы передаём в качестве аргумента в метод «.then()» (одна из двух, в зависимости от исхода происходящего). Для обеспечения работы этой «магии» методы «$.when()», «$.ajax()» и «.animate()» реализуют интерфейс Deferred. Пример работы на странице [when.html](http://anton.shevchuk.name/book/code/when.html).

> _Теперь можно и заумно – метод «.when()» возвращает проекцию Deferred объекта, принимает в качестве параметров произвольное множество Deferred объектов, когда все они отработают, объект «when» изменит своё состояние в «выполнено», с последующим вызовом всех подписавшихся._