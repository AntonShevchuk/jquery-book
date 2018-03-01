## Будь готов

Теперь пора приступить к работе — возьмём какой-нибудь элемент на страничке и попробуем его изменить. Для этого в `<head>` вставим следующий код (пример странички ищите ранее):

```html
<script>
    // мы пытаемся найти все элементы <h2> на странице
    // и изменить цвет шрифта на красный
    jQuery("h2").css("color", "red");
</script>
```

Только подобный код ничего не сделает, так как на момент выполнения на странице ещё не будет тегов `<h2>` – слишком рано выполняется скрипт, до загрузки всего HTML-документа. Для того, чтобы код сработал верно, мы должны либо поместить код после искомого `<h2>`, а то и сразу в самый низ страницы, либо использовать метод «.ready()» для отслеживания события «load» нашего «document»:

```html
<script>
    // ждём загрузки всего документа
    // после этого будет выполнена анонимная функция
    // которую мы передали в качестве параметра
    jQuery(document).ready(function(){
        jQuery("h2").css("color", "red");
    });
</script>
```

Также можно использовать сокращённый вариант без явного вызова метода «.ready()»:

```html
<script>
    $(function(){
        $("h2").css("color", "red");
    });
</script>
```

> _Последний вариант стоит причислить к «best practices», ведь в jQuery 3.0 метод «.ready()» уже помечен как deprecated._

Вы можете создать сколь угодно много подобных функций, не обязательно весь необходимый функционал помещать в одну.

> _`$()` — это синоним для `jQuery()`. Чтобы у вас не возникало конфликтов с другими ~~странами~~ библиотеками за использование «$», советую ваш код оборачивать в анонимную функцию следующего вида (best practice):_

```javascript
;(function($, undefined){
    // тут тихо и уютно
    // мы всегда будем уверены, что $ === jQuery
    // a undefined не переопределен ;)
})(jQuery);
```

> _Посмотрите на пример в [ready.html](../code/ready.html). Откройте исходный код данной страницы и найдите тег `<script>`. Осмыслите суть происходящего. Теперь найдите тег `<script>` в конце страницы и объясните почему он сработал?_