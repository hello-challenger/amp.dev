---
'$title': Acciones y eventos
$order: 0
formats:
  - websites
  - stories
  - ads
teaser:
  text: '[tip type="note"]'
---

<!--
This file is imported from https://github.com/ampproject/amphtml/blob/main/docs/spec/amp-actions-and-events.md.
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

[tip type="note"] Este documento trata sobre las acciones y eventos que puede usar en los sitios web, historias y anuncios de AMP. Lea sobre las [Acciones y eventos en el correo electrónico de AMP](https://github.com/ampproject/amphtml/blob/main/docs/spec/amp-email-actions-and-events.md) para el formato del correo electrónico de AMP. [/tip]

El atributo `on` se utiliza para instalar controladores de eventos en los elementos. Los eventos que sean compatibles dependerán del elemento.

El valor que se le asigna a la sintaxis consiste en usar un lenguaje sencillo específico para el dominio en el formulario:

[sourcecode:javascript]
eventName:targetId[.methodName[(arg1=value, arg2=value)]][/sourcecode]

Consulte en la siguiente tabla la descripción sobre cada parte de la sintaxis.

<table>
  <tr>
    <th width="30%">Sintaxis</th>
    <th width="18%">¿Es necesario?</th>
    <th width="42%">Descripción</th>
  </tr>
  <tr>
    <td><code>eventName</code></td>
    <td>Sí</td>
    <td>Es el nombre del evento donde se presenta un elemento.</td>
  </tr>
  <tr>
    <td><code>targetId</code></td>
    <td>Sí</td>
    <td>Es el ID del DOM para el elemento o un  <a href="#special-targets">objetivo especial</a> que se definió previamente, en el cual le gustaría ejecutar una acción como respuesta a un evento. En el siguiente ejemplo, el <code>targetId</code> es el ID del DOM para los objetivos <code>amp-lightbox</code> y <code>photo-slides</code>.     <pre>&lt;amp-lightbox id="photo-slides">&lt;/amp-lightbox> &lt;button on="tap:photo-slides">Show Images&lt;/button></pre>
</td>
  </tr>
  <tr>
    <td><code>methodName</code></td>
    <td>No</td>
    <td>Se utiliza cuando los elementos tienen acciones predeterminadas.<p>Este es el método que presenta el elemento objetivo (al cual se hace referencia usando <code>targetId</code>) que le gustaría ejecutar cuando se desencadene el evento.</p> <p>AMP cuenta con un concepto de acción predeterminada que puede implementarse en los elementos. Entonces, al omitir <code>methodName</code> AMP ejecutará ese método de forma predeterminada.</p>
</td>
  </tr>
  <tr>
    <td><code>arg=value</code></td>
    <td>No</td>
    <td>Algunas acciones, cuando se documentan, pueden aceptar argumentos. Los argumentos se definen entre paréntesis mediante la notación <code>key=value</code>. Los valores aceptados son:       <ul>         <li>cadenas sencillas sin comillas: <code>simple-value</code> </li>         <li>cadenas con comillas: <code>"string value"</code> o <code>'string value'</code> </li> <li>valores booleanos: <code>true</code> o <code>false</code> </li> <li>números: <code>11</code> o <code>1.1</code> </li> <li>sintaxis dot que hace referencia a los datos de un evento: <code>event.someDataVariableName</code> </li> </ul>
</td>
  </tr>
</table>

## Cómo administrar varios eventos <a name="handling-multiple-events"></a>

Puede concentrarse en varios eventos de un elemento si los separa mediante un punto y coma `;`.

Por ejemplo: `on="submit-success:lightbox1;submit-error:lightbox2"`

## Varias acciones para un evento <a name="multiple-actions-for-one-event"></a>

Puede ejecutar varias acciones de manera consecutiva para el mismo evento si las separa mediante una coma “,”.

Por ejemplo: `on="tap:target1.actionA,target2.actionB"`

## Cómo definir los eventos y acciones de manera global <a name="globally-defined-events-and-actions"></a>

En AMP se define el evento `tap` de manera global para que pueda concentrarse en cualquiera de los elementos HTML (incluidos los elementos de AMP).

En AMP también se definen las acciones `hide`, `show` y `toggleVisibility` globalmente para que pueda activarlas en cualquier elemento HTML.

[tip type="note"]

Un elemento solo puede mostrarse si estaba oculto previamente por una acción `hide` o `toggleVisibility`, o mediante el atributo [`hidden`](https://developer.mozilla.org/en-US/docs/Web/HTML/Global_attributes/hidden). La acción `show` no es compatible con los elementos ocultos por CSS `display:none` o `layout=nodisplay` de AMP.

Por ejemplo, en AMP es posible hacer lo siguiente:

[sourcecode:html]

<div id="warning-message">Warning...</div>

<button on="tap:warning-message.hide">Cool, thanks!</button>
[/sourcecode]

[/tip]

## Eventos con elementos específicos <a name="element-specific-events"></a>

### \* - todos los elementos <a name="---all-elements"></a>

<table>
  <tr>
    <th>Evento</th>
    <th>Descripción</th>
  </tr>
  <tr>
    <td><code>tap</code></td>
    <td>Se activa al presionar o hacer clic sobre el elemento.</td>
  </tr>
</table>

### Elementos de entrada <a name="input-elements"></a>

<table>
  <tr>
    <th width="20%">Evento</th>
    <th width="30%">Descripción</th>
    <th width="40%">Elementos</th>
    <th>Datos</th>
  </tr>
  <!-- change -->
  <tr>
    <td rowspan="3"><code>change</code></td>
    <td rowspan="3">Se activa cuando el valor del elemento cambia y se asigna. <p> Las propiedades de los datos son similares a los que se encuentran en <a href="https://developer.mozilla.org/en-US/docs/Web/API/HTMLInputElement#Properties">HTMLInputElement</a> y de <a href="https://developer.mozilla.org/en-US/docs/Web/API/HTMLSelectElement#Properties">HTMLSelectElement</a>.</p>
</td>
    <td><code>input</code></td>
    <td>
      <pre>event.min
event.max
event.value
event.valueAsNumber</pre>
    </td>
  </tr>
  <tr>
    <td> <code>input[type="radio"]</code>,<br><code>input[type="checkbox"]</code>
</td>
    <td>
      <code>event.checked</code>
    </td>
  </tr>
  <tr>
    <td><code>select</code></td>
    <td>
      <pre>event.min<br>event.max<br>event.value</pre>
    </td>
  </tr>
  <!-- input-debounced -->
  <tr>
    <td><code>input-debounced</code></td>
    <td>Se activa cuando el valor del elemento cambia. Es parecido al evento <code>change</code> estándar, pero solo se activa cuando hayan pasado 300 ms después de que el valor de entrada dejó de modificarse.</td>
    <td>Elementos que activan el evento <code>input</code>.</td>
    <td>Es similar a los datos del evento <code>change</code>.</td>
  </tr>
    <!-- input-throttled -->
  <tr>
    <td><code>input-throttled</code></td>
    <td>Se activa cuando el valor del elemento cambia. Es parecido al evento <code>change</code> estándar, pero está regulado para activarse máximo una vez cada 100 ms mientras el valor de entrada continúe modificándose.</td>
    <td>Elementos que activan el evento <code>input</code>.</td>
    <td>Es similar a los datos del evento <code>change</code>.</td>
  </tr>
</table>

### amp-accordion > sección <a name="amp-accordion"></a>

<table>
  <tr>
    <th width="25%">Evento</th>
    <th width="35%">Descripción</th>
    <th width="40%">Datos</th>
  </tr>
  <tr>
    <td><code>expand</code></td>
    <td>Se activa cuando la sección accordion aumenta.</td>
    <td>Ninguno.</td>
  </tr>
  <tr>
    <td><code>collapse</code></td>
    <td>Se activa cuando la sección accordion colapsa.</td>
    <td>Ninguno.</td>
  </tr>
</table>

### amp-carousel[type="slides"] <a name="amp-carouseltypeslides"></a>

<table>
  <tr>
    <th width="25%">Evento</th>
    <th width="35%">Descripción</th>
    <th width="40%">Datos</th>
  </tr>
  <tr>
    <td><code>slideChange</code></td>
    <td>Se activa cuando cambia la diapositiva en el carrusel.</td>
    <td><pre>// Slide number.<br>event.index</pre></td>
  </tr>
</table>

### amp-lightbox <a name="amp-lightbox-1"></a>

<table>
  <tr>
    <th width="25%">Evento</th>
    <th width="35%">Descripción</th>
    <th width="40%">Datos</th>
  </tr>
  <tr>
    <td><code>lightboxOpen</code></td>
    <td>Se activa cuando lightbox está completamente visible.</td>
    <td>Ninguno</td>
  </tr>
  <tr>
    <td><code>lightboxClose</code></td>
    <td>Se activa cuando lightbox está totalmente cerrado.</td>
    <td>Ninguno</td>
  </tr>
</table>

### amp-list <a name="amp-list-1"></a>

<table>
  <tr>
    <th width="25%">Evento</th>
    <th width="35%">Descripción</th>
    <th width="40%">Datos</th>
  </tr>
  <tr>
    <td><code>changeToLayoutContainer</code></td>
    <td>Actualiza el diseño de <code>amp-list</code> a <code>layout="CONTAINTER"</code> para permitir <a href="https://github.com/ampproject/amphtml/blob/main/docs/spec/../extensions/amp-list/amp-list.md#dynamic-resizing">cambios en el tamaño de forma dinámica</a>.</td>
  </tr>
  <tr>
    <td> <code>fetch-error</code>(poco confiable)</td>
    <td>Se activa cuando ocurre un error durante la consulta de los datos.</td>
    <td>Ninguno</td>
  </tr>
</table>

### amp-selector <a name="amp-selector-1"></a>

<table>
  <tr>
    <th width="25%">Evento</th>
    <th width="35%">Descripción</th>
    <th width="40%">Datos</th>
  </tr>
  <tr>
    <td><code>select</code></td>
    <td>Se activa al seleccionar o anular la selección de una opción.</td>
    <td><pre>// Target element's "option" attribute value.<br>event.targetOption<br>// Array of "option" attribute values of all selected elements.<br>event.selectedOptions</pre></td>
  </tr>
</table>

### amp-sidebar <a name="amp-sidebar-1"></a>

<table>
  <tr>
    <th width="25%">Evento</th>
    <th width="35%">Descripción</th>
    <th width="40%">Datos</th>
  </tr>
  <tr>
    <td><code>sidebarOpen</code></td>
    <td>Se activa cuando la barra lateral se abre completamente después de que terminó la transición.</td>
    <td>Ninguno</td>
  </tr>
  <tr>
    <td><code>sidebarClose</code></td>
    <td>Se activa cuando la barra lateral se cierra completamente después de que terminó la transición.</td>
    <td>Ninguno</td>
  </tr>
</table>

### amp-state <a name="amp-state-1"></a>

<table>
  <tr>
    <th width="25%">Evento</th>
    <th width="35%">Descripción</th>
    <th width="40%">Datos</th>
  </tr>
  <tr>
    <td> <code>fetch-error</code>(poco confiable)</td>
    <td>Se activa cuando ocurre un error durante la consulta de los datos.</td>
    <td>Ninguno</td>
  </tr>
</table>

### amp-video, amp-youtube <a name="amp-video-amp-youtube"></a>

<table>
  <tr>
    <th width="25%">Evento</th>
    <th width="35%">Descripción</th>
    <th width="40%">Datos</th>
  </tr>
  <tr>
    <td> <code>firstPlay</code>(poco confiable)</td>
    <td>Se activa la primera vez que el usuario reproduce el video. En los videos de reproducción automática se activa a partir del momento en que el usuario interactúa con el video. Este evento es poco confiable, lo cual significa que no puede desencadenar la mayoría de las acciones, y que solamente pueden ejecutarse acciones que se consideren poco confiables, como las de tipo <code>amp-animation</code>.</td>
    <td></td>
  </tr>
  <tr>
    <td> <code>timeUpdate</code>(poco confiable)</td>
    <td>Se activa cuando cambia la ubicación del botón de reproducción en un video. AMP controla la frecuencia del evento y actualmente se ajusta en intervalos de 1 segundo. Este evento es poco confiable, lo cual significa que no puede desencadenar la mayoría de las acciones, y que solamente pueden ejecutarse acciones que se consideren poco confiables, como las de tipo <code>amp-animation</code>.</td>
    <td>
<code>{time, percent}</code><code>time</code> indica el tiempo actual en segundos, <code>percent</code> es un número entre 0 y 1, e indica la posición actual como un porcentaje del tiempo total.</td>
  </tr>
</table>

### formulario <a name="form"></a>

<table>
  <tr>
    <th width="25%">Evento</th>
    <th width="35%">Descripción</th>
    <th width="40%">Datos</th>
  </tr>
  <tr>
    <td><code>submit</code></td>
    <td>Se activa cuando se envía el formulario.</td>
    <td></td>
  </tr>
  <tr>
    <td><code>submit-success</code></td>
    <td>Se activa cuando la respuesta por enviar el formulario es exitosa.</td>
    <td><pre>// Response JSON.<br>event.response</pre></td>
  </tr>
  <tr>
    <td><code>submit-error</code></td>
    <td>Se activa cuando ocurre un error en la respuesta por enviar el formulario.</td>
    <td><pre>// Response JSON.<br>event.response</pre></td>
  </tr>
  <tr>
    <td><code>valid</code></td>
    <td>Se activa cuando el formulario es válido.</td>
    <td></td>
  </tr>
  <tr>
    <td><code>invalid</code></td>
    <td>Se activa cuando el formulario no es válido.</td>
    <td></td>
  </tr>
</table>

## Acciones específicas para los elementos <a name="element-specific-actions"></a>

### \* (todos los elementos) <a name="-all-elements"></a>

<table>
  <tr>
    <th width="40%">Acción</th>
    <th>Descripción</th>
  </tr>
  <tr>
    <td><code>hide</code></td>
    <td>Oculta el elemento objetivo.</td>
  </tr>
  <tr>
    <td><code>show</code></td>
    <td>Muestra al elemento objetivo. Si como resultado de ello un <a href="https://developer.mozilla.org/en-US/docs/Web/HTML/Element/input#autofocus">elemento</a><code>autofocus</code> se vuelve visible, aumenta su prioridad.</td>
  </tr>
  <tr>
    <td><code>toggleVisibility</code></td>
    <td>Habilita o deshabilita la visibilidad del elemento objetivo. Si como resultado de ello un <a href="https://developer.mozilla.org/en-US/docs/Web/HTML/Element/input#autofocus">elemento</a><code>autofocus</code> se vuelve visible, aumenta su prioridad.</td>
  </tr>
  <tr>
    <td><code>toggleClass(class=STRING, force=BOOLEAN)</code></td>
    <td>Habilita o deshabilita las clases de un elemento objetivo. El <code>force</code> es opcional y, cuando se define, garantiza que la clase solamente se agregará pero no podrá eliminarse si se establece en <code>true</code>, y solo podrá eliminarse pero no agregarse si se establece en <code>false</code>.</td>
  </tr>
  <tr>
    <td><code>scrollTo(duration=INTEGER, position=STRING)</code></td>
    <td>Desplaza la visualización de un elemento con una animación gradual. El elemento<br><code>duration</code> es opcional. Especifica la duración de la animación en milisegundos. En caso de que no se especifique, puede utilizarse una cantidad relativa a la diferencia de desplazamiento que sea inferior o igual a 500 milisegundos. El elemento <br>     <code>position</code> también es opcional, pero utiliza alguna de las siguientes etiquetas <code>top</code>, <code>center</code>     o <code>bottom</code> (<code>top</code> aparece de forma predeterminada). Especifica la posición relativa que tiene el elemento respecto a la ventana de visualización después del desplazamiento.</td>
  </tr>
  <tr>
    <td><code>focus</code></td>
    <td>Hace que el elemento objetivo aumente su prioridad. Para disminuir la prioridad, puede utilizarse <code>focus</code> en otro elemento (generalmente en un elemento primario). Le recomendamos nuevamente que no pierda de vista la prioridad en <code>body</code>/<code>documentElement</code> por cuestiones de accesibilidad.</td>
  </tr>
</table>

### amp-audio <a name="amp-audio"></a>

<table>
  <tr>
    <th width="20%">Acción</th>
    <th>Descripción</th>
  </tr>
  <tr>
    <td><code>play</code></td>
    <td>Reproduce el sonido. No opera si el elemento <code>&lt;amp-audio></code> es descendiente de <code>&lt;amp-story></code>.</td>
  </tr>
  <tr>
    <td><code>pause</code></td>
    <td>Pone pausa al sonido. No opera si el elemento <code>&lt;amp-audio></code> es descendiente de <code>&lt;amp-story></code>.</td>
  </tr>
</table>

### amp-bodymovin-animation <a name="amp-bodymovin-animation"></a>

<table>
  <tr>
    <th>Acción</th>
    <th>Descripción</th>
  </tr>
  <tr>
    <td><code>play</code></td>
    <td>Reproduce la animación.</td>
  </tr>
  <tr>
    <td><code>pause</code></td>
    <td>Pausa la animación.</td>
  </tr>
  <tr>
    <td><code>stop</code></td>
    <td>Detiene la animación.</td>
  </tr>
  <tr>
    <td><code>seekTo(time=INTEGER)</code></td>
    <td>Establece el tiempo actual de la animación mediante un valor predeterminado y pausa la animación.</td>
  </tr>
  <tr>
    <td><code>seekTo(percent=[0,1])</code></td>
    <td>Utiliza un porcentaje específico para establecer el tiempo actual de la animación mediante un valor predeterminado y pausa la animación.</td>
  </tr>
</table>

### amp-accordion <a name="amp-accordion-1"></a>

<table>
  <tr>
    <th>Acción</th>
    <th>Descripción</th>
  </tr>
  <tr>
    <td><code>toggle(section=STRING)</code></td>
    <td>Habilita o deshabilita los estados <code>expanded</code> y <code>collapsed</code> en las secciones de <code>amp-accordion</code>. Cuando se hacen llamadas sin argumentos, permite alternar todas las secciones del acordeón. Habilita una sección específica al proporcionar el ID de la sección: <code>on="tap:myAccordion.toggle(section=</code>
</td>
</tr>
  <tr>
    <td><code>expand(section=STRING)</code></td>
    <td>Expande las secciones del acordeón. Si una sección ya está expandida permanece de esta manera. Cuando se hacen llamadas sin argumentos, expande todas las secciones del acordeón. Activa una sección específica al proporcionar el ID de la sección: <code>on="tap:myAccordion.expand(section=</code>
</td>
  </tr>
  <tr>
    <td><code>collapse(section=STRING)</code></td>
    <td>Colapsa las secciones del acordeón. Si una sección ya está colapsada permanece de esta manera. Cuando se hacen llamadas sin argumentos, colapsa todas las secciones del acordeón. Activa una sección específica al proporcionar el ID de la sección: <code>on="tap:myAccordion.collapse(section=</code>
</td>
  </tr>
</table>

### amp-carousel[type="slides"] <a name="amp-carouseltypeslides-1"></a>

<table>
  <tr>
    <th>Acción</th>
    <th>Descripción</th>
  </tr>
  <tr>
    <td><code>goToSlide(index=INTEGER)</code></td>
    <td>Hace que el carrusel avance hasta una diapositiva específica del índice.</td>
  </tr>
  <tr>
    <td><code>toggleAutoplay(toggleOn=true|false)</code></td>
    <td>Habilita o deshabilita el estado de reproducción automática en el carrusel. El elemento <code>toggleOn</code> es opcional.</td>
  </tr>
</table>

### amp-image-lightbox <a name="amp-image-lightbox"></a>

<table>
  <tr>
    <th width="40%">Acción</th>
    <th>Descripción</th>
  </tr>
  <tr>
    <td><code>open (default)</code></td>
    <td>Abre la imagen del lightbox, siendo la imagen de origen la que activó dicha acción.</td>
  </tr>
</table>

### amp-lightbox <a name="amp-lightbox"></a>

<table>
  <tr>
    <th>Acción</th>
    <th>Descripción</th>
  </tr>
  <tr>
    <td><code>open (default)</code></td>
    <td>Abre el lightbox.</td>
  </tr>
  <tr>
    <td><code>close</code></td>
    <td>Cierra el lightbox.</td>
  </tr>
</table>

### amp-lightbox-gallery <a name="amp-lightbox-gallery"></a>

<table>
  <tr>
    <th>Acción</th>
    <th>Descripción</th>
  </tr>
  <tr>
    <td><code>open</code></td>
    <td>Abre el lightbox-gallery. Puede activarse al presionar otro elemento, si especifica el ID de la imagen: `on="tap:amp-lightbox-gallery.open(id='image-id')"`.</td>
  </tr>
</table>

### amp-list <a name="amp-list"></a>

<table>
  <tr>
    <th>Acción</th>
    <th>Descripción</th>
  </tr>
  <tr>
    <td><code>refresh</code></td>
    <td>Actualiza los datos del <code>src</code> y vuelve a renderizar la lista.</td>
  </tr>
</table>

### amp-live-list <a name="amp-live-list"></a>

<table>
  <tr>
    <th>Acción</th>
    <th>Descripción</th>
  </tr>
  <tr>
    <td><code>update (default)</code></td>
    <td>Actualiza los elementos del DOM para mostrar el contenido actualizado.</td>
  </tr>
</table>

### amp-selector <a name="amp-selector"></a>

<table>
  <tr>
    <th>Acción</th>
    <th>Descripción</th>
  </tr>
  <tr>
    <td><code>clear</code></td>
    <td>Elimina todas las selecciones de un <code>amp-selector</code> definido.</td>
  </tr>
  <tr>
    <td><code>selectUp(delta=INTEGER)</code></td>
    <td>Desplaza la selección hacia arriba mediante el valor de “delta”. El valor de “delta” se establece en -1. Si no se selecciona ninguna opción, el estado que seleccione tomará el valor de la última opción.</td>
  </tr>
  <tr>
    <td><code>selectDown(delta=INTEGER)</code></td>
    <td>Desplaza la selección hacia abajo mediante el valor de “delta”. El valor de “delta” se establece en 1. Si no se selecciona ninguna opción, el estado que seleccione tomará el valor de la primera opción.</td>
  </tr>
  <tr>
    <td><code>toggle(index=INTEGER, value=BOOLEAN)</code></td>
    <td>Habilita o deshabilita la aplicación “seleccionada”. Si el atributo que se seleccionó está ausente, esta acción lo agregará. Si el atributo que se seleccionó está presente, esta acción lo eliminará. Puede forzar y mantener una adición o eliminación incluyendo un valor booleano en el argumento “value”. Un valor “true” forzará agregar el atributo “seleccionado” y no lo removerá si ya está presente. Un valor “false” eliminará el atributo, pero no lo agregará si está ausente.</td>
  </tr>
</table>

### amp-sidebar <a name="amp-sidebar"></a>

<table>
  <tr>
    <th>Acción</th>
    <th>Descripción</th>
  </tr>
  <tr>
    <td><code>open (default)</code></td>
    <td>Abre la barra lateral.</td>
  </tr>
  <tr>
    <td><code>close</code></td>
    <td>Cierra la barra lateral.</td>
  </tr>
  <tr>
    <td><code>toggle</code></td>
    <td>Habilita o deshabilita el estado de la barra lateral.</td>
  </tr>
</table>

### amp-state <a name="amp-state"></a>

<table>
  <tr>
    <th>Acción</th>
    <th>Descripción</th>
  </tr>
  <tr>
    <td><code>refresh</code></td>
    <td>Obtiene nuevamente los datos del atributo “src” mientras ignora el caché del navegador.</td>
  </tr>
</table>

### amp-user-notification <a name="amp-user-notification"></a>

<table>
  <tr>
    <th>Acción</th>
    <th>Descripción</th>
  </tr>
  <tr>
    <td><code>dismiss (default)</code></td>
    <td>Oculta el elemento que hace referencia a las notificaciones del usuario.</td>
  </tr>
</table>

### Elementos de video <a name="video-elements"></a>

Las acciones que se muestran a continuación son compatibles en AMP con los siguientes elementos de video: `amp-video`, `amp-youtube`, `amp-3q-player`, `amp-brid-player`, `amp-dailymotion`, `amp-delight-player`, `amp-ima-video`.

<table>
  <tr>
    <th>Acción</th>
    <th>Descripción</th>
  </tr>
  <tr>
    <td><code>play</code></td>
    <td>Reproduce el video.</td>
  </tr>
  <tr>
    <td><code>pause</code></td>
    <td>Pausa el video.</td>
  </tr>
  <tr>
    <td><code>mute</code></td>
    <td>Desactiva el audio del video.</td>
  </tr>
  <tr>
    <td><code>unmute</code></td>
    <td>Reactiva el audio del video.</td>
  </tr>
  <tr>
    <td><code>fullscreencenter</code></td>
    <td>Adapta el video a la pantalla completa.</td>
  </tr>
</table>

### formulario <a name="form-1"></a>

<table>
  <tr>
    <th>Acción</th>
    <th>Descripción</th>
  </tr>
  <tr>
    <td><code>clear</code></td>
    <td>Elimina cualquier valor en las entradas del formulario.</td>
  </tr>
  <tr>
    <td><code>submit</code></td>
    <td>Envía el formulario.</td>
  </tr>
</table>

## Objetivos especiales <a name="special-targets"></a>

Los siguientes son objetivos proporcionados por el sistema de AMP cuyos requisitos son especiales:

### Objetivo: AMP <a name="target-amp"></a>

El objetivo de `AMP` es proporcionado por el tiempo de ejecución e implementa acciones de nivel superior que se aplican a todo el documento.

<table>
  <tr>
    <th width="40%">Acción</th>
    <th>Descripción</th>
  </tr>
  <tr>
    <td><code>navigateTo(url=STRING, target=STRING, opener=BOOLEAN)</code></td>
    <td>
      <p>Navega en la ventana actual hacia una URL determinada o hasta el objetivo opcional que se especificó, si se proporcionó alguno (actualmente solo es compatible con <code>_top</code> y <code>_blank </code>). El parámetro opcional <code>opener</code> puede especificarse cuando se utiliza un <code>_blank</code> como objetivo para permitir que la página recién abierta acceda a <a href="https://developer.mozilla.org/en-US/docs/Web/API/Window/opener"><code>window.opener </code></a>. Es compatible con las <a href="https://github.com/ampproject/amphtml/blob/main/docs/spec/amp-var-substitutions.md">sustituciones estándar en las URL</a>.</p>
      <p><strong>Advertencia:</strong> Se recomienda utilizar los enlaces normales <code><a></code> siempre que sea posible, ya que <code>AMP.navigateTo</code> no es reconocido por los rastreadores web.</p>
    </td>
  </tr>
  <tr>
    <td><code>closeOrNavigateTo(url=STRING, target=STRING, opener=BOOLEAN)</code></td>
    <td>
      <p>Su objetivo es tratar de cerrar la ventana, cuando sea posible, de lo contrario permite navegar de manera similar a la acción <code>navigateTo</code>. Es útil para usarse en aquellos casos dónde es necesario cerrar una ventana mediante el botón “Regresar”, cuando se haya abierto una ventana nueva a partir de la página anterior o navegar si no se abrió.</p>
      <p><strong>Advertencia:</strong> Se recomienda utilizar los enlaces normales <code><a></code> siempre que sea posible, ya que <code>AMP.closeOrNavigateTo</code> no es reconocido por los rastreadores web.</p>
    </td>
  </tr>
  <tr>
    <td><code>goBack</code></td>
    <td>Permite navegar hacia atrás en el historial.</td>
  </tr>
  <tr>
    <td><code>print</code></td>
    <td>Abre el diálogo de la impresora para imprimir la página actual.</td>
  </tr>
  <tr>
    <td>scrollTo(id=STRING, duration=INTEGER, position=STRING)</td>
    <td>Se desplaza hasta el ID del elemento que se proporcionó en la página actual.</td>
  </tr>
  <tr>
    <td>optoutOfCid</td>
    <td>Excluye la generación de un ID para el cliente en todos los campos.</td>
  </tr>
  <tr>
    <td>
<code>setState({foo: 'bar'})</code><sup>1</sup>
</td>
    <td>
      <p>Requiere de <a href="https://amp.dev/documentation/components/amp-bind.html#updating-state-with-ampsetstate">amp-bind</a>.</p>
      <p>Fusiona un objeto literal con un estado para crear enlaces.</p>
      <p></p>
    </td>
  </tr>
  <tr>
    <td>
<code>pushState({foo: 'bar'})</code><sup>1</sup>
</td>
    <td>
      <p>Requiere de <a href="https://amp.dev/documentation/components/amp-bind.html#modifying-history-with-amppushstate">amp-bind</a>.</p>
      <p>Fusiona un objeto literal con un estado para crear enlaces e inserta una nueva entrada en el conjunto del historial del navegador. Cuando aparezca la entrada, los valores previos de las variables se restablecerán (en este ejemplo, la variable es <code>foo</code>).</p>
</td>
  </tr>
</table>

<sup>1</sup>Cuando se utiliza con <a href="#multiple-actions-for-one-event">varias acciones</a>, las acciones que se realicen posteriormente esperarán hasta que <code>setState()</code> o <code>pushState()</code> finalicen antes de invocarlas. Solamente se permite un <code>setState()</code> o <code>pushState()</code> por evento.

### Objetivo: amp-access <a name="target-amp-access"></a>

El componente [amp-access](https://amp.dev/documentation/components/amp-access.html) proporciona el objetivo `amp-access`.

El objetivo `amp-access` es especial por las siguientes razones:

1. No puede proporcionar una ID arbitraria para este objetivo. El objetivo siempre será `amp-access`.
2. Las acciones para `amp-access` son dinámicas pero dependen de la estructura de la [configuración de acceso a AMP](https://amp.dev/documentation/components/amp-access#configuration).

Consulte la [información](https://amp.dev/documentation/components/amp-access#login-link) sobre el uso del objetivo `amp-access`.
