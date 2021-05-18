---
'$title': Спецификация AMP HTML
$order: 8
formats:
  - websites
teaser:
  text: AMP HTML — это подмножество стандарта HTML для создания контентных страниц (например, новостных статей), позволяющее гарантировать определенные базовые характеристики производительности.
---

<!--
This file is imported from https://github.com/ampproject/amphtml/blob/main/docs/spec/amp-html-format.md.
Please do not change this file.
If you have found a bug or an issue please
have a look and request a pull request there.
-->

<!---
Copyright 2016 The AMP HTML Authors. All Rights Reserved.

Licensed under the Apache License, Version 2.0 (the "License");
you may not use this file except in compliance with the License.
You may obtain a copy of the License at

      http://www.apache.org/licenses/LICENSE-2.0

Unless required by applicable law or agreed to in writing, software
distributed under the License is distributed on an "AS-IS" BASIS,
WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
See the License for the specific language governing permissions and
limitations under the License.
-->

AMP HTML — это подмножество стандарта HTML для создания контентных страниц (например, новостных статей), позволяющее гарантировать определенные базовые характеристики производительности.

Будучи подмножеством HTML, оно накладывает некоторые ограничения на набор доступных HTML-тегов и функций, однако не требует разработки новых механизмов рендеринга: существующие пользовательские агенты могут отображать AMP HTML так же, как и любой другой код HTML.

[tip type="read-on"] Если вас в первую очередь интересует, что разрешено и что запрещено в AMP, посмотрите наше [обучающее видео об ограничениях AMP](https://www.youtube.com/watch?v=Gv8A4CktajQ). [/tip]

AMP HTML-документы можно загружать на веб-сервер и выдавать так же, как и любой другой HTML-документ; никакой специальной конфигурации сервера не требуется. Однако AMP-документы могут также выдаваться посредством специализированных систем AMP, выполняющих их проксирование. Эти системы выдают AMP-документы с собственного домена и могут дополнительно преобразовывать документ для повышения его производительности. Вот неполный список оптимизаций, которые может выполнять такая система:

- Замена ссылок на изображения — подстановка изображений, размер которых соответствует используемой клиентом области просмотра.
- Встраивание изображений, отображаемых в изначально видимой области.
- Встраивание переменных CSS.
- Предварительная загрузка расширенных компонентов.
- Минификация HTML и CSS.

AMP HTML использует ряд специальных элементов (коллективно разрабатываемых, однако централизованно управляемых и размещенных) для реализации расширенных возможностей, таких как галереи изображений для документов AMP HTML. Несмотря на то, что это позволяет задействовать в документе свои стили с помощью пользовательского CSS, это не позволяет использовать авторский JavaScript, помимо предлагаемого посредством специальных элементов для достижения целей производительности.

Используя формат AMP, производители контента делают контент в файлах AMP доступным для индексирования (с учетом ограничений robots.txt), кеширования и отображения третьими сторонами.

## Производительность <a name="performance"></a>

Предсказуемая производительность — ключевая цель разработки AMP HTML. В первую очередь мы стремимся сократить задержку перед тем, как содержимое страницы становится доступно пользователю. В более конкретных терминах это означает, что:

- Должны быть сведены к минимуму HTTP-запросы, необходимые для рендеринга и полной компоновки документа.
- Такие ресурсы, как изображения или реклама, следует загружать только в том случае, если они могут быть просмотрены пользователем.
- Браузеры должны иметь возможность вычислять пространство, необходимое каждому ресурсу на странице, без загрузки этого ресурса.

## Формат AMP HTML <a name="the-amp-html-format"></a>

### Образец документа <a name="sample-document"></a>

[sourcecode:html]

<!DOCTYPE html>
<html ⚡>
  <head>
    <meta charset="utf-8" />
    <title>Sample document</title>
    <link rel="canonical" href="./regular-html-version.html" />
    <meta
      name="viewport"
      content="width=device-width,minimum-scale=1,initial-scale=1"
    />
    <style amp-custom>
      h1 {
        color: red;
      }
    </style>
    <script type="application/ld+json">
      {
        "@context": "http://schema.org",
        "@type": "NewsArticle",
        "headline": "Article headline",
        "image": ["thumbnail1.jpg"],
        "datePublished": "2015-02-05T08:00:00+08:00"
      }
    </script>
    <script
      async
      custom-element="amp-carousel"
      src="https://cdn.ampproject.org/v0/amp-carousel-0.1.js"
    ></script>
    <script
      async
      custom-element="amp-ad"
      src="https://cdn.ampproject.org/v0/amp-ad-0.1.js"
    ></script>
    <style amp-boilerplate>
      body {
        -webkit-animation: -amp-start 8s steps(1, end) 0s 1 normal both;
        -moz-animation: -amp-start 8s steps(1, end) 0s 1 normal both;
        -ms-animation: -amp-start 8s steps(1, end) 0s 1 normal both;
        animation: -amp-start 8s steps(1, end) 0s 1 normal both;
      }
      @-webkit-keyframes -amp-start {
        from {
          visibility: hidden;
        }
        to {
          visibility: visible;
        }
      }
      @-moz-keyframes -amp-start {
        from {
          visibility: hidden;
        }
        to {
          visibility: visible;
        }
      }
      @-ms-keyframes -amp-start {
        from {
          visibility: hidden;
        }
        to {
          visibility: visible;
        }
      }
      @-o-keyframes -amp-start {
        from {
          visibility: hidden;
        }
        to {
          visibility: visible;
        }
      }
      @keyframes -amp-start {
        from {
          visibility: hidden;
        }
        to {
          visibility: visible;
        }
      }
    </style>
    <noscript
      ><style amp-boilerplate>
        body {
          -webkit-animation: none;
          -moz-animation: none;
          -ms-animation: none;
          animation: none;
        }
      </style></noscript
    >
    <script async src="https://cdn.ampproject.org/v0.js"></script>
  </head>
  <body>
    <h1>Sample document</h1>
    <p>
      Some text
      <amp-img src="sample.jpg" width="300" height="300"></amp-img>
    </p>
    <amp-ad
      width="300"
      height="250"
      type="a9"
      data-aax_size="300x250"
      data-aax_pubname="test123"
      data-aax_src="302"
    >
    </amp-ad>
  </body>
</html>
[/sourcecode]

### Обязательная разметка <a name="required-markup"></a>

Документы AMP HTML ДОЛЖНЫ:

- <a name="dctp"></a> начинаться с элемента `<!doctype html>`. [🔗](#dctp)
- <a name="ampd"></a> содержать тег верхнего уровня `<html ⚡>` (также допускается `<html amp>`). [🔗](#ampd)
- <a name="crps"></a> содержать теги `<head>` и `<body>` (в HTML они необязательны). [🔗](#crps)
- <a name="canon"></a>содержать тег `<link rel="canonical" href="$SOME_URL">` (внутри тега head), указывающий на «обычную» HTML-версию документа AMP HTML либо на текущий документ (если обычной HTML-версии не существует). [🔗](#canon)
- <a name="chrs"></a> содержать `<meta charset="utf-8">` в качестве первого дочернего элемента тега head. [🔗](#chrs)
- <a name="vprt"></a> содержать тег `<meta name="viewport" content="width=device-width">` внутри своего тега head. Также рекомендуется добавлять `minimum-scale=1` и `initial-scale=1`. [🔗](#vprt)
- <a name="scrpt"></a> содержать `<script async src="https://cdn.ampproject.org/v0.js"></script>` в теге head. [🔗](#scrpt)
- <a name="boilerplate"></a> содержать [шаблонный код AMP](https://github.com/ampproject/amphtml/blob/main/docs/spec/amp-boilerplate.md) ( `head > style[amp-boilerplate]` и `noscript > style[amp-boilerplate]`) в теге head. [🔗](#boilerplate)

### Метаданные <a name="metadata"></a>

Рекомендуется аннотировать AMP HTML-документы с помощью стандартных метаданных: <a>Open Graph Protocol</a>, <a>Twitter Cards</a> и т. д.

Мы также рекомендуем размечать AMP HTML-документы по схеме [schema.org/CreativeWork](http://ogp.me/) или любого из ее более специализированных типов, таких как [schema.org/NewsArticle](https://dev.twitter.com/cards/overview) или <a>schema.org/BlogPosting</a>.

### HTML-теги <a name="html-tags"></a>

В AMP HTML обычные HTML-теги можно использовать без изменений. Некоторые теги имеют специальные эквивалентные теги (например, <code><img></code> и <code><amp-img></code>), а другие теги категорически запрещены:

<table>
  <tr>
    <th width="30%">Тег</th>
    <th>Статус в AMP HTML</th>
  </tr>
  <tr>
    <td width="30%">script</td>
    <td>Запрещен, если параметр type не имеет значение <code>application/ld+json</code>, <code>application/json</code> или <code>text/plain</code> (другие неисполняемые значения могут быть добавлены по мере необходимости). Исключения — обязательный тег script, используемый для загрузки среды выполнения AMP, а также теги script для загрузки расширенных компонентов.</td>
  </tr>
  <tr>
    <td width="30%">noscript</td>
    <td>Разрешен. Можно использовать в любом месте документа. Содержимое внутри элемента <code><noscript></noscript></code> будет показано, если пользователь отключит JavaScript.</td>
  </tr>
  <tr>
    <td width="30%">base</td>
    <td>Запрещен.</td>
  </tr>
  <tr>
    <td width="30%">img</td>
    <td>Заменен на <code>amp-img</code>.<br> Обратите внимание: в соответствии со стандартом HTML5 <code><img></code> является <a href="https://www.w3.org/TR/html5/syntax.html#void-elements">пустым элементом</a>, поэтому у него нет закрывающего тега. Однако у <code></code> закрывающий тег есть (<code></code>).</td>
  </tr>
    <tr>
    <td width="30%">picture</td>
    <td>Запрещен. Чтобы выдавать изображения в разных форматах, используйте атрибут <a href="https://amp.dev/documentation/guides-and-tutorials/develop/style_and_layout/placeholders?format=websites">fallback</a> или добавьте несколько атрибутов <a href="https://amp.dev/documentation/components/amp-img#attributes"><code>srcset</code> в элемент </a><code></code>.</td>
  </tr>
  <tr>
    <td width="30%">video</td>
    <td>Заменен на <code>amp-video</code>.</td>
  </tr>
  <tr>
    <td width="30%">audio</td>
    <td>Заменен на <code>amp-audio</code>.</td>
  </tr>
  <tr>
    <td width="30%">iframe</td>
    <td>Заменен на <code>amp-iframe</code>.</td>
  </tr>
    <tr>
    <td width="30%">frame</td>
    <td>Запрещен.</td>
  </tr>
  <tr>
    <td width="30%">frameset</td>
    <td>Запрещен.</td>
  </tr>
  <tr>
    <td width="30%">object</td>
    <td>Запрещен.</td>
  </tr>
  <tr>
    <td width="30%">param</td>
    <td>Запрещен.</td>
  </tr>
  <tr>
    <td width="30%">applet</td>
    <td>Запрещен.</td>
  </tr>
  <tr>
    <td width="30%">embed</td>
    <td>Запрещен.</td>
  </tr>
  <tr>
    <td width="30%">form</td>
    <td>Разрешен. Требует добавления расширения <a href="https://amp.dev/documentation/components/amp-form">amp-form</a>.</td>
  </tr>
  <tr>
    <td width="30%">Элементы input <a></a>
</td>
    <td>В основном разрешены, <a href="https://amp.dev/documentation/components/amp-form#inputs-and-fields">за исключением некоторых типов</a>, а именно <code></code>, <code></code>. Разрешены также связанные с ними теги: <code></code>, <code></code>
</td>
  </tr>
  <tr>
    <td width="30%">button</td>
    <td>Разрешен.</td>
  </tr>
  <tr>
    <td width="30%"><code><a id="cust"></a>style</code></td>
    <td>
<a href="#boilerplate">Обязательный тег стиля для amp-boilerplate</a>. В теге head допускается еще один тег стиля для индивидуального оформления. Этот тег стиля должен иметь атрибут <code>amp-custom</code>. <a href="#cust">🔗</a>
</td>
  </tr>
  <tr>
    <td width="30%">link</td>
    <td>Допускаются значения <code>rel</code>, зарегистрированные на <a href="http://microformats.org/wiki/existing-rel-values">microformats.org</a>. Если значение <code>rel</code> отсутствует в нашем белом списке, <a href="https://github.com/ampproject/amphtml/issues/new">создайте задачу</a>. Запрещены <code>stylesheet</code> и другие значения, такие как <code>preconnect</code>, <code>prerender</code> и <code>prefetch</code>, имеющие побочные эффекты в браузере. Предусмотрено одно исключение для загрузки таблиц стилей с сайтов разрешенных поставщиков шрифтов.</td>
  </tr>
  <tr>
    <td width="30%">meta</td>
    <td>Атрибут <code>http-equiv</code> может использоваться для указания определенных допустимых значений; подробности см. в <a href="https://github.com/ampproject/amphtml/blob/main/validator/validator-main.protoascii">спецификации валидатора AMP</a>.</td>
  </tr>
  <tr>
    <td width="30%"><a id="ancr"></a><code>a</code></td>
    <td>Значение атрибута <code>href</code> не должно начинаться с <code>javascript:</code>. Если установлен атрибут <code>target</code>, значение должно быть <code>_blank</code>. В остальных случаях разрешен. <a href="#ancr">🔗</a>
</td>
  </tr>
  <tr>
    <td width="30%">svg</td>
    <td>Разрешено большинство элементов SVG.</td>
  </tr>
</table>

В реализациях валидатора должен использоваться белый список, созданный на основании спецификации HTML5 с удалением вышеуказанных тегов. См. <a>Справочник по тегам AMP</a>.

### Комментарии <a name="comments"></a>

Условные комментарии HTML не допускаются.

### HTML-атрибуты <a name="html-attributes"></a>

В AMP HTML запрещены имена атрибутов, начинающиеся с <code>on</code> (например, <code>onclick</code> или <code>onmouseover</code>). Атрибут с буквальным именем <code>on</code> (без суффикса) является допустимым.

Атрибуты, связанные с XML, такие как xmlns, xml:lang, xml:base и xml:space, запрещены в AMP HTML.

Внутренние атрибуты AMP с префиксом <code>i-amp-</code> запрещены в AMP HTML.

### Классы <a name="classes"></a>

Имена внутренних классов AMP с префиксом `-amp-` и <code>i-amp-</code> запрещены в AMP HTML.

Обратитесь к <a>документации AMP</a>, чтобы узнать значение имен классов с префиксом `amp-`. Такие классы предназначены для настройки некоторых функций среды выполнения AMP и расширений.

Остальные пользовательские имена классов разрешены.

### Идентификаторы <a name="ids"></a>

Некоторые имена идентификаторов запрещены в AMP HTML, например идентификаторы с префиксами <code>-amp-</code> и <code>i-amp-</code>, которые могут конфликтовать с внутренними идентификаторами AMP.

Перед использованием идентификаторов `amp-` и `AMP` изучите соответствующие расширения, описанные в документации AMP, — это позволит вам избежать конфликта с функциями, предоставляемыми этими расширениями (такими, как <code>amp-access</code>).

Чтобы увидеть полный список запрещенных наименований идентификаторов, пройдите по <a>этой ссылке</a> и введите в поиске `mandatory-id-attr`.

### Ссылки <a name="links"></a>

Схема <code>javascript:</code> запрещена.

### Таблицы стилей <a name="stylesheets"></a>

Основные семантические теги и специальные элементы AMP поставляются со стилями по умолчанию, что упрощает процесс создания адаптивного документа. Возможность отказа от стилей по умолчанию может быть добавлена в будущем.

#### @-правила <a name="-rules"></a>

В таблицах стилей разрешены следующие @-правила:

<code>@font-face</code>, <code>@keyframes</code>, <code>@media</code>, <code>@page</code>, <code>@supports</code>.

Правило `@import` запрещено. Другие правила могут быть добавлены в будущем.

#### Авторские таблицы стилей <a name="author-stylesheets"></a>

Авторы могут добавлять в документ собственные стили с помощью одного тега `<style amp-custom>` внутри head или с помощью встроенных стилей.

Правила `@keyframes` разрешено использовать внутри <code><style amp-custom></code>. Однако, если их слишком много, рекомендуется поместить их в дополнительный тег <code><style amp-keyframes></code>, который должен находиться в конце документа AMP. Дополнительные сведения см. в разделе <a>Таблица стилей ключевых кадров</a> в этом документе.

#### Селекторы <a name="selectors"></a>

К селекторам в авторских таблицах стилей применяются следующие ограничения:

##### Имена классов и тегов <a name="class-and-tag-names"></a>

Имена классов, идентификаторов, тегов и атрибутов не могут начинаться с <code>-amp-</code> и <code>i-amp-</code>. Эти наименования зарезервированы для внутреннего использования средой выполнения AMP. Соответственно, пользовательская таблица стилей не может ссылаться на CSS-селекторы классов <code>-amp-</code>, идентификаторов <code>i-amp-</code> и тегов/атрибутов <code>i-amp-</code>. Имена таких классов, идентификаторов и тегов/атрибутов не могут изменяться авторами, однако авторы могут переопределять стили классов и тегов <code>amp-</code> для любых CSS-свойств, которые явно не запрещены спецификацией этих компонентов.

Чтобы предотвратить использование селекторов атрибутов для обхода ограничений в отношении наименований классов, в CSS-селекторы обычно не разрешается включать токены и строки, начинающиеся с `-amp-` и `i-amp-`.

#### Важно! <a name="important"></a>

Использование квалификатора `!important` не допускается. Данное требование обусловлено тем, как AMP реализует инварианты размера элементов.

#### Свойства <a name="properties"></a>

В AMP разрешены переходы и анимации только тех свойств, которые могут задействовать аппаратное ускорение в распространенных браузерах. В настоящее время в нашем белом списке находятся `opacity`, <code>transform</code> (также <code>-vendorPrefix-transform</code>).

В следующих примерах свойство `<property>` должно быть в вышеуказанном белом списке.

- `transition <property>` (также -vendorPrefix-transition)
- `@keyframes name { from: {<property>: value} to {<property: value>} }` (также `@-vendorPrefix-keyframes`)

#### Максимальный размер <a name="maximum-size"></a>

Размер авторской таблицы стилей или совокупный размер встроенных стилей не должен превышать 75 000 байт, в противном случае вы получите ошибку валидации.

### Таблица стилей ключевых кадров <a name="keyframes-stylesheet"></a>

В дополнение к <code><style amp-custom></code> авторы могут также добавить тег <code><style amp-keyframes></code>, который разрешен специально для анимации по ключевым кадрам.

К тегу `<style amp-keyframes>` применяются следующие ограничения:

1. Может быть размещен только как последний дочерний элемент элемента `<body>`.
2. Может содержать только `@keyframes`, `@media`, `@supports` и их комбинации.
3. Его объем не может превышать 500 000 байт.

Тег `<style amp-keyframes>` был создан с целью решить проблему, которая заключается в том, что правила ключевых кадров часто являются громоздкими даже для умеренно сложных анимаций, что приводит к замедлению синтаксического анализа CSS и задержке первой отрисовки контента (FCP). Такие правила часто превышают ограничение на размер, установленное для <code><style amp-custom></code>, поэтому, чтобы обходить ограничения по размеру, объявления ключевых кадров следует размещать в <code><style amp-keyframes></code> в нижней части документа. А поскольку ключевые кадры не блокируют рендеринг, первая отрисовка контента может выполняться раньше завершения их анализа.

Пример:

[sourcecode:html]

<style amp-keyframes>
@keyframes anim1 {}

@media (min-width: 600px) {
  @keyframes anim1 {}
}
</style>
</body>
[/sourcecode]

### Пользовательские шрифты <a name="custom-fonts"></a>

Авторы могут использовать таблицы стилей, добавляющие пользовательские шрифты. Это можно делать двумя способами: либо с помощью тегов ссылок на поставщиков шрифтов из белого списка, либо с помощью <code>@font-face</code>.

Пример:

[sourcecode:html]

<link
  rel="stylesheet"
  href="https://fonts.googleapis.com/css?family=Tangerine"
/>
[/sourcecode]

Поставщики шрифтов могут быть добавлены в белый список, если они поддерживают интеграцию посредством чистого CSS-кода и работают по HTTPS. На текущий момент для загрузки шрифтов с помощью тегов ссылок разрешены следующие источники:

- Fonts.com: `https://fast.fonts.net`
- Google Fonts: `https://fonts.googleapis.com`
- Font Awesome: `https://maxcdn.bootstrapcdn.com, https://use.fontawesome.com`
- [Typekit](https://helpx.adobe.com/typekit/using/google-amp.html): `https://use.typekit.net/kitId.css` (замените `kitId` соответствующим образом)

ПРИМЕЧАНИЕ ДЛЯ ИСПОЛНИТЕЛЕЙ. Для добавления провайдера в этот список требуется изменить правило CSP-политики AMP-кеша.

Авторы могут свободно добавлять любые пользовательские шрифты с помощью инструкции <code>@font-face</code> в своем коде CSS. Шрифты, добавленные через <code>@font-face</code>, должны загружаться по HTTP или HTTPS.

## Среда выполнения AMP <a name="amp-runtime"></a>

Среда выполнения AMP — это специальный код JavaScript, который выполняется внутри любого документа AMP. Он реализует функциональность специальных элементов AMP, управляет загрузкой ресурсов и установкой приоритетов и позволяет задействовать runtime-валидатор AMP HTML на этапе разработки.

Загрузка среды выполнения AMP осуществляется с помощью обязательной строки <code><script src="https://cdn.ampproject.org/v0.js"></script></code>, расположенной в разделе <code><head></code> документа AMP.

Среду выполнения AMP можно переключить в режим разработки на любой странице. В этом режиме на встроенной странице запускается механизм валидации AMP, который выводит статус валидации и все ошибки в консоль разработчика JavaScript. Чтобы запустить режим разработки, добавьте к URL страницы `#development=1`.

## Ресурсы <a name="resources"></a>

Ресурсы, такие как изображения, видео, аудиофайлы или реклама, должны размещаться в AMP HTML-файле с помощью специальных элементов (например, <code><amp-img></code>). Мы называем их «управляемыми ресурсами», потому что их загрузкой (временем загрузки, необходимостью загрузки) управляет среда выполнения AMP.

Каких-либо гарантий в отношении загрузки, осуществляемой средой выполнения AMP, не предусмотрено — как правило, среда пытается загружать ресурсы достаточно быстро, чтобы они были готовы к тому моменту, когда пользователь захочет их увидеть. Среда выполнения выделяет наивысший приоритет ресурсам, находящимся в текущей области просмотра; при этом она пытается предугадать изменение области просмотра и выполняет предварительную загрузку соответствующих ресурсов.

Среда выполнения AMP может в любой момент выгружать ресурсы, которые в данный момент не находятся в области просмотра, а также переназначать контейнеры ресурсов, такие как iframe, для уменьшения общего потребления ОЗУ.

## Компоненты AMP <a name="amp-components"></a>

AMP HTML использует специальные элементы (т. н. «компоненты AMP») для замены встроенных тегов загрузки ресурсов, таких как `<img>` и <code><video></code>. Эти элементы также используются для реализации функций, выполняющих сложные взаимодействия, таких как кольцевые галереи и лайтбоксы с изображениями.

Подробную информацию о поддерживаемых компонентах см. в <a>спецификации компонентов AMP</a>.

Есть 2 типа поддерживаемых компонентов AMP:

1. Встроенные
2. Расширенные

Встроенные компоненты постоянно доступны в документе AMP и имеют специальный выделенный элемент, например `<amp-img>`. С другой стороны, расширенные компоненты должны быть явным образом добавлены в документ.

### Общие атрибуты <a name="common-attributes"></a>

#### `layout`, `width`, `height`, `media`, `placeholder`, `fallback` <a name="layout-width-height-media-placeholder-fallback"></a>

Эти атрибуты определяют макет элемента. Их главная цель — гарантировать возможность отображения элемента и резервирования пространства под него до загрузки каких-либо удаленных ресурсов или кода JavaScript.

Подробнее о системе макетов читайте в статье <a>Система макетов AMP</a>.

#### `on` <a name="on"></a>

Атрибут `on` используется для установки на элементы обработчиков событий. То, какие события поддерживаются, зависит от элемента.

Синтаксис представляет собой простой предметно-ориентированный язык следующего вида:

[sourcecode:javascript]
eventName:targetId[.methodName[(arg1=value, arg2=value)]]
[/sourcecode]

Пример: <code>on="tap:fooId.showLightbox"</code>

Если не указан параметр `methodName`, выполняется метод по умолчанию (если он определен для элемента). Пример: <code>on="tap:fooId"</code>

Некоторые действия могут принимать аргументы. Аргументы указываются в круглых скобках в нотации <code>key=value</code> . Допустимые значения:

- простые строки без кавычек: `simple-value`;
- строки в кавычках: `"string value"` или `'string value'`;
- логические значения: `true` или `false`;
- числа: `11` или `1.1`.

Чтобы элемент «слушал» несколько событий, разделяйте события точкой с запятой <code>;</code>.

Пример: `on="submit-success:lightbox1;submit-error:lightbox2"`

Подробнее о <a>действиях и событиях AMP можно узнать здесь</a>.

### Расширенные компоненты <a name="extended-components"></a>

Расширенные компоненты — это компоненты, которые по умолчанию не включены в среду выполнения AMP. Их следует явным образом добавлять в документ.

Расширенные компоненты загружаются путем включения тега `<script>` в раздел head документа следующим образом:

[sourcecode:html]

<script
  async
  custom-element="amp-carousel"
  src="https://cdn.ampproject.org/v0/amp-carousel-0.1.js"
></script>

[/sourcecode]

Тег `<script>` должен иметь атрибут <code>async</code>, а также атрибут <code>custom-element</code>, ссылающийся на имя элемента.

В рамках реализаций в среде выполнения имя может использоваться для отображения заполнителей для этих элементов.

URL-адрес скрипта должен начинаться с <code>https://cdn.ampproject.org</code> и обязан соответствовать очень строгому паттерну <code>/v\d+/[az-]+-(latest|\d+|\d+\.\d+)\.js</code>.

##### URL <a name="url"></a>

URL расширенных компонентов имеют следующий вид:

[sourcecode:http]
https://cdn.ampproject.org/$RUNTIME_VERSION/$ELEMENT_NAME-$ELEMENT_VERSION.js
[/sourcecode]

##### Управление версиями <a name="versioning"></a>

См. <a>Политику управления версиями AMP</a>.

### Шаблоны <a name="templates"></a>

Шаблоны отображают HTML-контент на основании шаблона, предусмотренного для конкретного языка, а также предоставленных данных JSON.

Подробную информацию о поддерживаемых шаблонах см. в <a>спецификации шаблонов AMP</a>.

Шаблоны не поставляются со средой выполнения AMP и должны быть загружены так же, как и расширенные элементы. Расширенные компоненты загружаются путем добавления тега <code><script></code> в тег head документа:

[sourcecode:html]

<script
  async
  custom-template="amp-mustache"
  src="https://cdn.ampproject.org/v0/amp-mustache-0.2.js"
></script>

[/sourcecode]

Тег `<script>` должен иметь атрибут `async` и атрибут <code>custom-template</code>, указывающий на тип шаблона. URL-адрес скрипта должен начинаться с <code>https://cdn.ampproject.org</code> и обязан соответствовать очень строгому паттерну <code>/v\d+/[az-]+-(latest|\d+|\d+\.\d+)\.js</code>.

Шаблоны объявляются в документе следующим образом:

[sourcecode:html]
<template type="amp-mustache" id="template1">
Hello {% raw %}{{you}}{% endraw %}!
</template>
[/sourcecode]

Атрибут <code>type</code> является обязательным и должен ссылаться на объявленный скрипт <code>custom-template</code>.

Атрибут <code>id</code> является обязательным. Отдельные элементы AMP используют свои собственные шаблоны. Как правило, элемент AMP ищет <code><template></code> либо среди своих дочерних элементов, либо ссылается на него по идентификатору.

Синтаксис в составе элемента шаблона зависит от языка шаблона. В среде AMP на язык шаблона могут накладываться ограничения. Например, в соответствии с элементом «template», все операции должны выполняться на корректно сформированном дереве DOM. Все выходные данные шаблона также подлежат санитизации, чтобы гарантировать валидный для AMP формат данных.

Подробную информацию о поддерживаемых шаблонах см. в [спецификации шаблонов AMP](https://github.com/ampproject/amphtml/blob/main/docs/spec/./amp-html-templates.md).

##### URL <a name="url-1"></a>

URL расширенных компонентов имеют следующий вид:

[sourcecode:http]
https://cdn.ampproject.org/$RUNTIME_VERSION/$TEMPLATE_TYPE-$TEMPLATE_VERSION.js
[/sourcecode]

##### Управление версиями <a name="versioning-1"></a>

Дополнительные сведения см. в разделе «Управление версиями специальных элементов».

## Безопасность <a name="security"></a>

AMP HTML-документы не должны вызывать ошибок при использовании политики безопасности контента, которая не включает ключевые слова <code>unsafe-inline</code> и <code>unsafe-eval</code>.

Формат AMP HTML разработан таким образом, чтобы это условие всегда соблюдалось.

Все элементы шаблонов AMP должны проходить проверку безопасности AMP перед отправкой в репозиторий AMP.

## SVG <a name="svg"></a>

В настоящее время разрешены следующие элементы SVG:

- базовые: "g", "glyph", "glyphRef", "image", "marker", "metadata", "path", "solidcolor", "svg", "switch", "view"
- формы: "circle", "ellipse", "line", "polygon", "polyline", "rect"
- текст: "text", "textPath", "tref", "tspan"
- rendering: "clipPath", "filter", "hkern", "linearGradient", "mask", "pattern", "radialGradient", "vkern"
- специальные: "defs" (здесь разрешены все дочерние элементы, указанные выше), "symbol", "use"
- фильтр: "feColorMatrix", "feComposite", "feGaussianBlur", "feMerge", "feMergeNode", "feOffset", "foreignObject"
- ARIA: "desc", "title"

А также следующие атрибуты:

- "xlink:href": разрешены только URI, начинающиеся с "#"
- "style"

## Обнаружение документов AMP <a name="amp-document-discovery"></a>

Ниже приводится стандартный способ, с помощью которого программное обеспечение может определять наличие у канонического документа AMP-версии.

Если существует документ AMP, который является альтернативной версией канонического документа, то канонический документ должен указывать на документ AMP с помощью тега <code>link</code> с <a>отношением «amphtml»</a>.

Пример:

[sourcecode:html]

<link rel="amphtml" href="https://www.example.com/url/to/amp/document.html" />
[/sourcecode]

Ожидается, что документ AMP будет самостоятельно указывать свою каноническую версию тегом <code>link</code> с отношением «canonical».

Пример:

[sourcecode:html]

<link
  rel="canonical"
  href="https://www.example.com/url/to/canonical/document.html"
/>
[/sourcecode]

(Если один ресурс является одновременно AMP <em>и</em> каноническим документом, каноническое отношение должно указывать само на себя — указывать отношение «amphtml» не требуется.)

Обратите внимание, что для максимальной совместимости с системами, потребляющими AMP, необходимо обеспечить возможность считать отношение «amphtml» без выполнения JavaScript — то есть тег должен присутствовать в необработанном HTML, а не внедряться посредством JavaScript.
