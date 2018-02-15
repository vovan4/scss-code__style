# SCSS style guide :v:

- [Именование селекторов](#step-1)
- [Свойства](#step-4)
- [Форматирование](#step-2)
- [Общие правила](#step-3)

## Именование селекторов

Для именования селекторов используется методология [BEM](https://ru.bem.info/methodology/naming-convention/). Основная идея соглашения по именованию -- сделать имена CSS-селекторов максимально информативными и понятными.

**По следующим причинам:**

- [x] Упрощает код и облегчает рефакторинг
- [x] Вы получаете самодокументируемый код
- [x] Меньше вложенностей, низкая специфичность правил.
- [x] Дает возможность повторно использовать код и избежать взаимного влияния компонентов друг на друга
- [x] Дает возможность разместить несколько сущностей на одном DOM-узле и избежать «Copy-Paste»
- [x] Это помогает создать чистую, строгую связь между CSS и HTML

### Имя блока

Имя **_блока_** формируется по схеме block-name и задает пространство имен для элементов и модификаторов.

```scss
.block-name {
  //...
}
```

### Имя элемента

Пространство имен, заданное именем блока, определяет принадлежность элемента к данному блоку. Имя **_элемента_** отделяется от имени блока двумя подчеркиваниями `__`

```scss
.block-name__elem-name {
  //...
}
```

### Имя модификатора

Пространство имен, заданное именем блока, определяет принадлежность модификатора к данному блоку или его элементу. Имя модификатора отделяется от имени блока или элемента двумя дифисами `--`.

```scss
.block-name__elem-name--modificator {
  //...
}
```

### Булевые модификаторы

Название модификатора пары ключ-значение формируется с помощью разделения нижним подчеркиванием `_`.

```scss
.block-name__elem-name--key_value {
  //...
}
```

### Пример использования

_HTML_

```html
<nav class="nav nav--fixed">
  <ul class="nav__list">
    <li class="nav__item">
      <a class="nav__link" href="#">Link</a>
    </li>
    <li class="nav__item">
      <a class="nav__link" href="#">Link</a>
    </li>
    <li class="nav__item">
      <a class="nav__link" href="#">Link</a>
    </li>
  </ul>
</nav>
```

_SCSS_

```scss
.nav {
  &--fixed {}
  &__list {}
  &__item {}
  &__link {}
}
```

_CSS_

```css
.nav {}
.nav--fixed {}
.nav__list {}
.nav__item {}
.nav__link {}
```

--------------------------------------------------------------------------------

## Свойства

### Vendor Prefixes

Никогда не пишите вендорные префиксы потому что браузеры обновляються и требования от проекта к проекту разные. Используйте _Autoprefixer_.

### Краткие записи

Не используйте краткие записи когда это __не нужно__.

:poop:
```scss
.selector {
  margin: 0 0 20rem;
  @media screen and (max-width: 768px) {
    margin: 0 0 10rem;
  }
  @media screen and (max-width: 320px) {
    margin: 0 0 5rem;
  }
}
```

:ok_hand:

```scss
.selector {
  margin-bottom: 20rem;
  @media screen and (max-width: 768px) {
    margin-bottom: 10rem;
  }
  @media screen and (max-width: 320px) {
    margin-bottom: 10rem;
  }
}
```

--------------------------------------------------------------------------------

## Форматирование

### Общие правила

- Используйте 2 пробела для отступа.
- Не используйте CamelCase в именах классов, а используейте дефис.
- Не используйте селекторы по ID.
- Используя несколько селекторов в объявлении правила переносите каждый селектор на отдельную строку.
- Ставьте пробел перед открывающей скобкой {.
- В свойствах ставьте пробел после двоеточия :, но не перед.
- После объявления свойства переносите закрывающую скобку } на новую строку.

```scss
[selector] {
  [property]: [value];
}
```

### Цвета

- указывайте цвета в нижнем регистре.
- указывайте цвета в кратком виде `#000000` == :poop:, а `#000` == :ok_hand:.
- указывайте цвета в `rgba`-формате без "ручной" конвертации: `rgba(255, 123, 22, .5)` == :poop:, `rgba(#ff7b16, .5)` == :ok_hand:.
- прозрачный цвет в градиентах указывайте как `rgba(#fff, 0)` (баг айфона).

### Комментарии

- Старайтесь использывать однострочные `//` комментарии.
- Пишите комментарии в отдельные строки. Старайтесь избегать комментариев в конце строки.
- Пишите детальные комментарии для неочевидного кода

--------------------------------------------------------------------------------

## Общие правила

Всегда используйте .scss синтаксис, и никогда оригинальный .sass синтаксис.

### Порядок объявления свойств

#### @include-объявления

Группирование @include-объявлений в конце правила делает код более читаемым.

```scss
.btn {
  background: green;
  font-weight: bold;
  //..
  @include transition(background 0.5s ease);
}
```

#### Вложенные селекторы

Вложенные селекторы, если необходимо, идут последними, и ничего не должно идти после них.

```scss
.btn {
  background: green;
  font-weight: bold;
  @include transition(background 0.5s ease);
  &::before {
    content: '';
    display: block;
  }
  &:hover{
    background: red;
  }
  &__icon {
    margin-right: 10px;
  }
}
```

:fire: _Вложенные селекторы не должны быть глубже трёх вложений_

```scss
.slider {  
  &__item {
    &--active {
      // STOP!
    }  
  }
}
```

#### Миксины (примеси)

Миксины должны использоваться для поддержания чистоты и ясности кода или абстрактной сложности во многом так же, как и хорошо названные функции. Миксины, не принимающие никаких аргументов, могут быть полезны для этого.

#### Наследование

Использование `@extend` лучше минимизировать из-за его неинтуитивности и потенциальной опасности в поведении, особенно при использовании вместе со вложенными селекторами. Даже наследование селекторов верхнего уровня может создать проблемы, если в будущем будет изменён порядок селекторов.

Используйте `@extend` только с некомпилируемыми селекторами, правила внутри которых являются наиболее частыми:

```scss
%clear-list {
  margin: 0;
  padding: 0;
  list-style-type: none;
}
//...
.nav-list {
  @extend %clear-list;
}

```

#### Импорт

SCSS имеет возможность импорта, которая позволяет разделить ваш SCSS-файл на более мелкие и облегчить их поддержку.

Каждый блок с его элементами лучше описывать в отдельном файле, названием которого будет название блока. Удобство такого метода заключается в том, что поиск блока становится поиском по __названию файла__, а не поиском по файлам.

### Организация файловой структуры

За основу берется файловая структура [Markup framework](https://github.com/dshm/markup-framework).

```
.
└── scss
    ├── app
    |    └── .gitkeep
    ├── common.scss
    ├── components.scss
    ├── extends.scss
    ├── fonts.scss
    ├── functions.scss
    ├── mixins.scss
    ├── normalize.scss
    ├── reset.scss
    ├── sprite-png.scss
    ├── typography.scss
    ├── variable.scss
    └── index.scss
```

#### Подробнее о предназначении каждого файла

* `common.scss` - стили, которые применяются к основным тегам относительно проекта, никаких селекторов по классу тут быть не должно, например:
```scss
// for using rem units
html {
  font-size: 10px;
}
// for sticky footer
html,
body {
  height: 100%;
}
```

* `components.scss` - импорты стилей библиотек, установленных с помощью __npm__ или __bower__, например:
```scss
@import "../components/slick-carousel/slick/slick.scss";
@import "../../node_modules/malihu-custom-scrollbar-plugin/jquery.mCustomScrollbar";
```

* `extends.scss` - некомпилируемые селекторы для наследования по всему проекту, например:
```scss
%clear-list {
  list-style: none;
  margin: 0;
  padding: 0;
}
%clearfix {
  &::after {
    content: '';
    display: block;
    clear: both;
  }
}
```

* `fonts.scss` - только подключение шрифтов и, по необходимости, миксины для шрифтов. Базовая миксина для подключения шрифта из файлов уже объявлена здесь, вы можете изменить её относительно вашего проекта. В файле `fonts.scss` не должна быть определена типографика проекта, для этого есть отдельный файл `typography.scss`. Например:
```scss
@include font('pr', 'proxima-nova-regular');

@mixin pr($size: default) {
  font-family: 'pr', sans-serif;
  @if size != default {
    font-size: $size;
  }
}
```

* `functions.scss` - функции, которые будут использованы в вашем проекте, не путайте их с миксинами. Например:
```scss
@function data-url($url) {
  @return url('/image/data/'+$url);
}
```

* `mixins.scss` - миксины, которые будут использованы в вашем проекте, не путайте их с функциями. Все миксины для медиа-запросов должны быть определены здесь. Например:
```scss
@mixin desktop-properties() {
  @media only screen and (min-width: 1025px) {
    @content;
  }
}
```

* `normalize.scss` - [Normalize CSS](https://necolas.github.io/normalize.css/)

* `reset.scss` - сброс стилей, необходимый в вашем проекте, который неучтен в `normalize.scss`. По-умолчанию, этот файл не импортируется в `index.scss`, потому что для большинства проектов вполне хватает `normalize.scss`. Вы можете добавить в `reset.scss` нетривиальные сбросы, например:
```scss
* {
  -webkit-tap-highlight-color: rgba(#000, .2);
}
```

* `sprite-png.scss` - файл генерируется автоматически генератором спрайтов, файл не трогаем.

* `typography.scss` - типографика проекта, это могут быть стили относительно всего проекта, либо некомпилируемый селектор, который будет содержать в себе стили тектовых блоков. Например:
```scss
html {
  font-family: 'Roboto', sans-serif;
}
p {
  font-size: 1.6rem;
}
```

* `variable.scss` - все базовые переменные (цвета, временные фукции и т.п.). Наример:
```scss
$blue: #008aff;
$easing: cubic-bezier(0, .74, .91, .37);
```

* `app` - папка, файлы которой импортируются в алфавитном порядке, поэтому следите за весами своих селекторов в проекте.

* `index.scss` - файл, который будет компилироваться gulp-ом, в нём все импорты. Раскоментируйте `reset`, если используете его на своем проекте.
