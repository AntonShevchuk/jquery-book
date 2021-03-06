## Touch-события {#touch}

Смартфоны с большим сенсорным экраном — это уже норма жизни, и любому web-разработчику рано или поздно потребуется разрабатывать интерфейсы с поддержкой touch-событий. На этот случай в JavaScript предусмотрены следующие события:

`touchstart` — событие схоже с «mousedown», происходит при касании пальцем экрана

`touchend` — убираем палец с экрана, ака «mouseup»

`touchmove` — водим пальцем по экрану — «mousemove»

`touchcancel` — странное событие, отмена «touch» до того, как палец был убран

О том, как с ними работать, можно разузнать из отличной статьи на английском языке — «[Touching and Gesturing on iPhone, Android, and More](http://www.sitepen.com/blog/?p=3425)» (хотя там рассказ и о Dojo Toolkit).

В [jQuery Mobile](/appendix/jquery_mobile.md) работа с touch-событиями идёт «из коробки». Чтобы jQuery UI заставить работать с touch-событиями, следует использовать библиотеку [jQuery UI Touch Punch](http://touchpunch.furf.com/). Пробуйте, но учтите, теоретическая разработка интерфейсов для подобных устройств – нонсенс (_англ. nonsense_), соответствующее touch-устройство должно быть доступно для тестирования.
