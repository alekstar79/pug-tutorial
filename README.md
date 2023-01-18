# <a id="content" />Препроцессор Pug
Pug - это препроцессор HTML и шаблонизатор, который был написан на JavaScript для Node.js.

## Содержание:
1. [Теги](#tags)
2. [Текст](#text)
3. [Атрибуты](#attributes)
4. [Констуркция Switch Case](#construction-switch-case)
5. [Циклы](#cycles)
6. [Вставка JavaScript кода](#insert-javascript-code)
7. [Комментарии](#comments)
8. [Условия](#conditions)
9. [Тип документа](#document-type)
10. [Инклюды (Includes)](#includes)
11. [Наследование шаблонов](#template-inheritance)
12. [Интерполяция переменных](#variable-interpolation)
13. [Миксины](#mixins)
14. [Многострочный ассоциативный массив](#associative-array)

[Официальная документация по Pug](https://pugjs.org/language/attributes.html)

## <a id="tags" />Теги
В Pug нет закрывающих тегов, вместо этого он использует строгую табуляцию (или отступы) для определения вложености тегов.
Для закрытия тегов в конце необходимо добавить символ `/`: `foo(bar='baz')/`

Pug
````pug
ul
  li Item A
  li Item B
  li Item C
````
HTML
````html
<ul>
  <li>Item A</li>
  <li>Item B</li>
  <li>Item C</li>
</ul>
````
[К содержанию](#content)

## <a id="text" />Текст
Непосредственно в Pug можно вставлять элементы в HTML синтаксисе

Pug
````pug
p This is plain old <em>text</em> content.
````
HTML
```html
<p>This is plain old <em>text</em> content.</p>
````
***
Pug
````pug
p
  | The pipe always goes at the beginning of its own line,
  | not counting indentation.
````
HTML
````html 
<p>The pipe always goes at the beginning of its own line, not counting indentation.</p>
````
[К содержанию](#content)

## <a id="attributes" />Атрибуты
В Pug можно встраивать JavaScript код, благодаря чему возможны конструкции показанные ниже.

Pug
````pug
a(href='google.com') Google
|
|
a(class='button' href='google.com') Google
|
|
a(class='button', href='google.com') Google
````
HTML
````html 
<a href="google.com">Google</a>
<a class="button" href="google.com">Google</a>
<a class="button" href="google.com">Google</a>
````
***
Pug
````pug
- var authenticated = true
body(class=authenticated ? 'authed' : 'anon')
````
HTML
````html 
<body class="authed"></body>
````
***
Pug
````pug
input(
  type='checkbox'
  name='agreement'
  checked
)
````
HTML
````html 
<input type="checkbox" name="agreement" checked="checked" />
````
***
Pug
````pug
- var url = 'pug-test.html';
a(href='/' + url) Link
|
|
- url = 'https://example.com/'
a(href=url) Another link
````
HTML
````html 
<a href="/pug-test.html">Link</a>
<a href="https://example.com/">Another link</a>
````
***
Pug
````pug
- var classes = ['foo', 'bar', 'baz']
a(class=classes)
|
|
//- the class attribute may also be repeated to merge arrays
a.bang(class=classes class=['bing'])
````
HTML
````html 
<a class="foo bar baz"></a>
<a class="bang foo bar baz bing"></a>
````
[К содержанию](#content)

## <a id="construction-switch-case" />Констуркция Switch Case
Pug поддерживает switch case, которая представляет собой более наглядный способ сравнить выражение сразу с несколькими вариантами.

Pug
````pug
- var friends = 10
case friends
  when 0
    p you have no friends
  when 1
    p you have a friend
  default
    p you have #{friends} friends
````
HTML
````html 
<p>you have 10 friends</p>
````
[К содержанию](#content)

## <a id="cycles" />Циклы
Pug
````pug
ul
  each val, index in ['zero', 'one', 'two']
    li= index + ': ' + val
````
HTML
````html 
<ul>
  <li>0: zero</li>
  <li>1: one</li>
  <li>2: two</li>
</ul>
````
***
Pug
````pug
- var values = [];
ul
  each val in values
    li= val
  else
    li There are no values
````
HTML
````html 
<ul>
  <li>There are no values</li>
</ul>
````
***
Pug
````pug
- var n = 0;
ul
  while n < 4
    li= n++
````
HTML
````html 
<ul>
  <li>0</li>
  <li>1</li>
  <li>2</li>
  <li>3</li>
</ul>
````
[К содержанию](#content)

## <a id="insert-javascript-code" />Вставка JavaScript кода
Pug поддерживает вставку частей JavaScript кода в шаблоны.

Не буфферизированный код начинается с символа `-`  
Pug
````pug
- for (var x = 0; x < 3; x++)
  li item
````
HTML
````html 
<li>item</li>
<li>item</li>
<li>item</li>
````
***
Буфферизированный код начинается с символа `=`  
Pug
````pug
p
  = 'This code is <escaped>!'
````
HTML
````html 
<p>This code is &lt;escaped&gt;!</p>
````
[К содержанию](#content)

## <a id="comments" />Комментарии
Существуют различные комментарии, те которые будут отображаться после компиляции и те, которые пропадут.

Pug
````pug
// just some paragraphs
//- will not output within markup
p foo
p bar
````
HTML
````html 
<!-- just some paragraphs-->
<p>foo</p>
<p>bar</p>
````
***
Pug
````pug
body
  //-
    Comments for your template writers.
    Use as much text as you want.
  //
    Comments for your HTML readers.
    Use as much text as you want.
````
HTML
````html 
<body>
  <!--Comments for your HTML readers.
Use as much text as you want.-->
</body>
````
[К содержанию](#content)

## <a id="conditions" />Условия
Pug
````pug
- var user = { description: 'foo bar baz' }
- var authorised = false
#user
  if user.description
    h2.green Description
    p.description= user.description
  else if authorised
    h2.blue Description
    p.description.
      User has no description,
      why not add one...
  else
    h2.red Description
    p.description User has no description
````
HTML
````html
<div id="user">
  <h2 class="green">Description</h2>
  <p class="description">foo bar baz</p>
</div>
````
[К содержанию](#content)

## <a id="document-type" />Тип документа
Pug
````pug
doctype html
````
HTML
````html 
<!DOCTYPE html>
````
[К содержанию](#content)

## <a id="includes" />Инклюды (Includes)
Pug имеет возможность вставки содержимого одного файла в другой файл Pug.

Pug
````pug
//- index.pug
doctype html
html(lang="en")
  head
    title Document
    style
      include style.css
  body
    h1 My Site
    p Welcome to my super lame site.
    script
      include script.js
````
CSS
````css
/* style.css */
h1 {
  color: red;
}
````
JavaScript
````js
// script.js
console.log('You are awesome');
````
HTML

````html 
<!DOCTYPE html>
<html lang="en">
<head>
  <title>Document</title>
  <style>
    /* style.css */
    h1 {
      color: red;
    }
  </style>
  <title>Document</title>
</head>
<body>
<h1>My Site</h1>
<p>Welcome to my super lame site.</p>
<script>
  // script.js
  console.log('You are awesome');
</script>
</body>
</html>
````
[К содержанию](#content)

## <a id="template-inheritance" />Наследование шаблонов
Pug поддерживает наследование шаблонов. Наследование шаблонов работает через ключевые слова `block` и `extend`. В шаблоне `block` - обычный блок Pug, который может заменить дочерний шаблон. Этот процесс является рекурсивным.

Pug
````pug
//- base.pug
html
  head
    title My Site 
    block scripts
      script(src='/jquery.js')
  body
    block content
    block foot
      #footer
        p some footer content
        
//- home.pug
extends base.pug
- var title = 'Animals'
- var pets = ['cat', 'dog']
block content
  h1= title // - or #{title} without =
  each petName in pets
    p= petName // -or #{petName} without =
````
HTML
````html 
<!DOCTYPE html>
<html lang="en">
<head>
  <title>My site</title>
  <script src='/jquery.js'></script>
</head>
<body>
  <h1>Animals</h1>
  <p>cat</p>
  <p>dog</p>
  <div id='footer'>
    <p>some footer content</p>
  </div>
</body>
</html>
````
[К содержанию](#content)

## <a id="variable-interpolation" />Интерполяция переменных
Pug предоставляет различные способы вывода переменных.

Pug
````pug
- var title = "On Dogs: Man's Best Friend";
- var author = "enlore";
- var theGreat = "<span>escape!</span>";

h1= title
p Written with love by #{author}
p This will be safe: !{theGreat}
````
HTML
````html 
<h1>On Dogs: Man's Best Friend</h1>
<p>Written with love by enlore</p>
<p>This will be safe: <span>escape!</span></p>
````
[К содержанию](#content)

## <a id="mixins" />Миксины
Поддержка миксинов позволяет создавать переиспользуемые блоки.

Pug
````pug
//- Declaration
mixin pet(name)
  li.pet= name
//- use
ul
  +pet('cat')
  +pet('dog')
  +pet('pig')
````
HTML
````html 
<ul>
  <li class="pet">cat</li>
  <li class="pet">dog</li>
  <li class="pet">pig</li>
</ul>
````
***
Pug
````pug
mixin article(title)
  .article
    .article-wrapper
      h1= title
      if block
        block
      else
        p No content provided

+article('Hello world')

+article('Hello world')
  p This is my
  p Amazing article
````
HTML
````html 
<div class="article">
  <div class="article-wrapper">
    <h1>Hello world</h1>
    <p>No content provided</p>
  </div>
</div>
<div class="article">
  <div class="article-wrapper">
    <h1>Hello world</h1>
    <p>This is my</p>
    <p>Amazing article</p>
  </div>
</div>
````
***
Pug
````pug
mixin link(href, name)
  //- attributes == {class: "btn"}
  a(class!=attributes.class href=href)= name

+link('/foo', 'foo')(class="btn")
````
HTML
````html 
<a class="btn" href="/foo">foo</a>
````
[К содержанию](#content)

## <a id="associative-array" />Многострочный ассоциативный массив
Pug
````pug
-
  var priceItem = [
    {include: filterInc, parameter : "Розовый фильтр"},
    {include: smileInc, parameter : "Смайлики"},
    {include: commentInc, parameter : "Комментарии"}
  ]
````
[К содержанию](#content)
