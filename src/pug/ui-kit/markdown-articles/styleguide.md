# Работа со стилями. Styleguide по написанию стилей

---

-   Используем правила BEM naming нотации для CSS классов.
-   Пробелы в длинном имени класса делаем с помощью дефисов `-`.
-   В каждом less / scss файле все цвета и радиусы скругления выносим в переменные в начало файла.
-   В LESS / SCSS не используем и удаляем кросс-браузерные префиксы. Они добавляются автоматически gulp плагином при компиляции.
-   Стилизация по тегам и ID - ЗАПРЕЩЕНА! Кроме user generated контента.
-   Все компоненты, секции, модули, блоки в проекте - это Блоки по БЭМ неймингу. Кнопка - это блок. Секция с шапкой или подвалом - это блок. Разметка структуры страница - это блок.

-   **Стилизация по тегам и ID - ЗАПРЕЩЕНА!**
-   Классы сетки священны. Не трогаем и не изменяем классы сетки `.container, .row, .col-*** ...`
-   Никакого каскада по тегам, кроме user content.

---

### Используем следующие правила BEM naming нотации для CSS классов

```
block
block__element
block__element--modifier
```

Если надо сделать пробел между словами в CSS классе используем дефис:

```
.long-block-title
.long-block-title__element-name
.long-block-title__element-name--with-modifier
```

Никаких нижних подчеркиваний!

### Стили и переменные

В каждом less / scss / sass файле все цвета и радиусы скругления выносим в переменные в начало файла.
Общие переменные прописаны в файле `_variables.less`

### Стилизация по тегам и ID - ЗАПРЕЩЕНА!

**Кроме вложенных тегов <a>, <span>, <i>, <em>, <strong>, <bold>.**

Вот так нельзя:

```css
.card h4 {
    ...;
}
```

А вот так можно:

```css
.header-1 a {
    ...;
}
```

Никакой стилизации по тегам. Все стили только по классам.
Исключение - только user generated content и теги <a>, <span>, <i>, <em>, <strong>, <bold>. В этом случае делаем каркас и стилизацию по тегам.

User generated content - это контент который генерируется пользователем из админки сайта.
При редактировании старниц пользователь работает в WISIWIG редакторе. Пользователь просто форматирует текст тегами. Поэтому наша задача стилизовать эти теги на сайте. Пользователю не надо думать про добавление классов, и это правильно.

Все что касается любого другого кода на странице должно быть строго стилизовано по классу. Никаких каскадов. Никаких тегов.

Пример стилизации user generated content:

```css
.user-content {
    h1 {
        /*...*/
    }
    ul {
        /*...*/
    }
    ul li {
        /*...*/
    }
}
```

_Пояснение._ Ранее я допускал возможность стилизации навигации каскадом. Например `.nav ul li { ... }`. Когда работаешь один и сам следишь за своим кодом, то можно использовать такие решения. Когда работает команда, порой нелегко каждому объяснить где можно использовать каскад, а где нет. У всех в итоге свои субъективные понятия. А кто-то просто не заморачивается на эту тему, и лупит стилизацию пот тегам где вздумается. Поэтому мы ее запрещаем кроме 2-х оговоренных выше случаев. Теперь данное правило нельзя трактовать двояко.

### Избегаем стилизации по каскаду где это возможно. Никакого каскада по тегам, кроме user content

Стараемся избегать каскадных селекторов вида `body .blog-post .nav-link {}`

Иначе можно легко столкнуться с ситуацией когда надо добавить кнопку например в шапку, а в шапке все ссылки сделаны белым цветом, и теперь цвет текста внутри кнопки (которая прописана через тег `<a>` будут белым на белом фоне)

Стоит заметить что при стилизации больших блоков или секций каскад может быть необходим. Например в этом примере необходимо стилизовать иконку внути дива с датой для комментария:

```css
.comment__date {
    font-size: 14px;
    color: #9b9b9b;

    .fa-clock {
        margin-right: 5px;
    }
}
```

Или вот здесь, силизовать теги внутри блока с текстом в сообщении от пользователя:

```css
.user-message__text {
    line-height: 1.7;
    p {
        line-height: 1.7;
    }
}
```

Еще пример, выравниваем кнопку в карточке "работа в портфолио":

```css
.card-portfolio .button {
    position: absolute;
    bottom: 20px;
    left: 20px;
}
```

### Не переопределать классы сетки которая используется в проекте.

Не переопределать классы сетки которая используется в проекте. Контейнер, ряд и колонки:
`.container, .row, .col-**-**`

Переопределяя классы и стили сетки, она начинает работать неочевидно для других разработчиков. Например вы переопределили ширину контейнера, чтобы на странице с раьботами он был побольше по ширине. Но класс .container исползуется также на всех других страницах сайта, теперь изменения произойдут и там.

Если необходимо, можно добавить свой класс, модификатор который будет изменять сетку.

Никакого каскада относительно классов сетки. Это запрещено: `.container input`.

### Секция, Блок, Модуль?

Четко утвердить терминологию в проекте.

**Блоки** - это небольшие элементы. Кнопка, заголовок, карточка. Для блоков хорошо подходят миксины.

**Секции** - большие разделы страницы, которые содержат в себе несколько блоков. Например шапка сайта. Раздел с несколькими карточками блога. Футер страницы. Секции подключаем через include как часть страницы.

Никаких модулей.

### Секция или Блок?

Секция содержит в себе несколько _блоков_.
Блок содержит в себе несколько _элементов_.

Логотип это блок, он содержит несколько элементов:

-   Название
-   Слоган
-   ГРафический элемент, картинку или иконку

Хедер страницы это секция, она содержит в себе несколько блоков:

-   Логотип
-   Навигацию
-   ...

**Блок** - элемент который повторяется. Логотип может быть использован в шапке и в подвале сайта. На странице может быть несколько блоков c ошибкой. Несмотря на то что кнопка часто является частью секции или другого блока, лучше оформлять ее как самостоятельный блок, а не элемент. Подобное также справедливо для ссылок, элментов форм, заголовков.

Примеры блоков:

-   Логотип
-   Блок с ошибкой
-   Текстовое поле
-   Кнопка
-   Ссылка
-   Заголовки

**Секция** - раздел страницы, который сожержит в себе несколько блоков.

Примеры секций:

-   Шапка
-   Подвал
-   Сайдбар с несколькими блоками
-   Секция "О авторе"

### Размеры отступов

Отступы внутри кнопок, margin для заголовков лучше писать через em. Так они будут привязаны к размеру шрифта элемента. Высоту строки - указываем через множитель.

### Браузерные префиксы

Если используем борку на gulp, в которой gulp плагин компилирует less/sass в CSS и сам дописывает автопрефиксы, тогда договариваемся не использовать браузерные префиксы в LESS / SASS / CSS стилях. Потому что префиксы будут добавлены автоматически при сборке.
