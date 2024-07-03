# DOM (Modelo de Objetos del Documento)

`Document Object Model`, o `DOM`, representa todo el contenido de la página como objetos que pueden ser modificados.

El objeto document es el punto de entrada a la página. Con él podemos cambiar o crear cualquier cosa en la página.

La estructura de un documento HTML son las etiquetas.

Según el `Modelo de Objetos del Documento (DOM)`, cada etiqueta `HTML` es un objeto. Las etiquetas anidadas son llamadas “hijas” de la etiqueta que las contiene. El texto dentro de una etiqueta también es un objeto.

Todos estos objetos son accesibles empleando JavaScript, y podemos usarlos para modificar la página.

Por ejemplo, `document.body` es el objeto que representa la etiqueta `<body>`.

Ejecutar el siguiente código hará que el `<body>` sea de color rojo

```javascript
// cambiar el color de fondo a rojo
document.body.style.background = "red";

// deshacer el cambio después de 1 segundo
setTimeout(() => (document.body.style.background = ""), 1000);
```

En el caso anterior usamos `style.background` para cambiar el color de fondo del `document.body`, pero existen muchas otras propiedades, tales como:

- `innerHTML` – contenido HTML del nodo.
- `offsetWidth` – ancho del nodo (en píxeles).
- …, etc.

## Un ejemplo del DOM

Comencemos con un documento simple:

```html
<!DOCTYPE html>
<html>
  <head>
    <title>About elk</title>
  </head>
  <body>
    The truth about elk.
  </body>
</html>
```

Cada nodo del árbol es un objeto.

Las etiquetas son nodos de elementos (o simplemente “elementos”) y forman la estructura del árbol. `<html>` está ubicado en la raíz del documento, por lo tanto, `<head>` y `<body>` son sus hijos, etc.

El texto dentro de los elementos forma `nodos de texto`, y son etiquetados como `#text`. Un nodo de texto puede contener únicamente una cadena y no puede tener hijos, siempre es una hoja del árbol.

Por ejemplo, la etiqueta `<title>` tiene el texto `"About elk"`.

Hay que tener en cuenta los caracteres especiales en nodos de texto:

una línea nueva: `↵` (en JavaScript se emplea `\n` para obtener este resultado)
un espacio: `␣`
Los espacios y líneas nuevas son caracteres totalmente válidos, al igual que letras y dígitos. Ellos forman nodos de texto y se convierten en parte del DOM. Así, por ejemplo, en el caso de arriba la etiqueta `<head>` contiene algunos espacios antes de la etiqueta `<title>`, entonces ese texto se convierte en el nodo #text, que contiene una nueva línea y solo algunos espacios.

Hay solo dos excepciones de nivel superior:

- Los espacios y líneas nuevas ubicados antes de la etiqueta `<head>` son ignorados por razones históricas.
- Si colocamos algo después de la etiqueta `</body>`, automáticamente se situará dentro de body, en el final, ya que la especificación HTML necesita que todo el contenido esté dentro de la etiqueta `<body>`. No puede haber espacios después de esta.

En otros casos todo es sencillo: si hay espacios (como cualquier carácter) en el documento, se convierten en nodos de texto en el DOM; y si los eliminamos, entonces no habrá nodo.

En el siguiente ejemplo, no hay nodos de texto con espacios en blanco:

```html
<!DOCTYPE html>
<html>
  <head>
    <title>About elk</title>
  </head>
  <body>
    The truth about elk.
  </body>
</html>
```

## Autocorrección

Si el navegador encuentra HTML mal escrito, lo corrige automáticamente al construir el DOM.

Por ejemplo, la etiqueta superior siempre será `<html>`. Incluso si no existe en el documento, ésta existirá en el DOM, puesto que el navegador la creará. Sucede lo mismo con la etiqueta `<body>`.

Como ejemplo de esto, si el archivo HTML es la palabra `"Hello"`, el navegador lo envolverá con las etiquetas `<html>` y `<body>`, y añadirá la etiqueta `<head>` la cual es requerida. Basado en esto, el DOM resultante será:

```html
<html>
  <head></head>
  <body>
    Hello
  </body>
</html>
```

Al generar el DOM, los navegadores procesan automáticamente los errores en el documento, cierran etiquetas, etc.

Un documento sin etiquetas de cierre:

<!-- prettier-ignore-start -->
```html
<p>Hello
<li>Mom
<li>and
<li>Dad
```
<!-- prettier-ignore-end -->

…se convertirá en un DOM normal a medida que el navegador lee las etiquetas y compone las partes faltantes:

```html
<p>Hello</p>
<li>Mom</li>
<li>and</li>
<li>Dad</li>
```

Un caso especial interesante son las tablas. De acuerdo a la especificación DOM deben tener la etiqueta <tbody>, sin embargo el texto HTML puede omitirla: el navegador crea automáticamente la etiqueta <tbody> en el DOM.

```html
<table id="table">
  <tr>
    <td>1</td>
  </tr>
</table>
```

La estructura del DOM será:

```html
<table id="table">
  <tbody>
    <tr>
      <td>1</td>
    </tr>
  </tbody>
</table>
```

## Otros tipos de nodos

Existen otros tipos de nodos además de elementos y nodos de texto.

Por ejemplo, los comentarios:

```html
<!DOCTYPE html>
<html>
  <body>
    The truth about elk.
    <ol>
      <li>An elk is a smart</li>
      <!-- comentario -->
      <li>...y el astuto animal!</li>
    </ol>
  </body>
</html>
```

Podemos pensar: ¿Por qué se agrega un comentario al DOM? Esto no afecta la representación de ninguna manera. Pero hay una regla: si algo está en el código HTML, entonces también debe estar en el árbol DOM.

Todo en HTML, incluso los comentarios, se convierte en parte del DOM.

Hasta la declaración `<!DOCTYPE...>` al principio del HTML es un nodo del DOM. Su ubicación en el DOM es justo antes de la etiqueta `<html>`.

Hay [12 tipos de nodos](https://www.w3schools.com/jsref/prop_node_nodetype.asp). En la práctica generalmente trabajamos con 4 de ellos:

- `document` – el “punto de entrada” en el DOM.
- `nodos de elementos` – Etiquetas-HTML, los bloques de construcción del árbol.
- `nodos de texto` – contienen texto.
- `comentarios` – Podríamos colocar información allí. No se mostrará, pero JS puede leerla desde el DOM.

## Buscar: getElement*, querySelector*

### document.getElementById o sólo id

Si un elemento tiene el atributo `id`, podemos obtener el elemento usando el método `document.getElementById(id)`, sin importar dónde se encuentre.

Por ejemplo:

```html
<div id="elem">
  <div id="elem-content">Elemento</div>
</div>

<script>
  // obtener el elemento
  let elem = document.getElementById("elem");

  // hacer que su fondo sea rojo
  elem.style.background = "red";
</script>
```

> [!TIP] > **El id debe ser único**  
> El id debe ser único. Sólo puede haber en todo el documento un elemento con un id determinado.

> [!WARNING] > **Sólo `document.getElementById`, no `anyElem.getElementById`**  
> El método getElementById sólo puede ser llamado en el objeto document. Busca el id dado en todo el documento.

### querySelectorAll

Sin duda el método más versátil, `elem.querySelectorAll(css)` devuelve todos los elementos dentro de elem que coinciden con el selector CSS dado.

Aquí buscamos todos los elementos `<li>` que son los últimos hijos:

```html
<ul>
  <li>La</li>
  <li>prueba</li>
</ul>
<ul>
  <li>ha</li>
  <li>pasado/li></li>
</ul>

<script>
  let elements = document.querySelectorAll("ul > li:last-child");

  for (let elem of elements) {
    alert(elem.innerHTML); // "prueba", "pasado"
  }
</script>
```

Este método es muy poderoso, porque se puede utilizar cualquier selector de CSS.

> [!NOTE] > **También se pueden usar pseudoclases**  
> Las pseudoclases como `:hover` (cuando el cursor sobrevuela el elemento) y `:active` (cuando hace clic con el botón principal) también son soportadas. Por ejemplo, `document.querySelectorAll(':hover')` devolverá una colección de elementos sobre los que el puntero hace hover en ese momento (en orden de anidación: desde el más exterior `<html>` hasta el más anidado).

### querySelector

La llamada a `elem.querySelector(css)` devuelve el primer elemento para el selector CSS dado.

En otras palabras, el resultado es el mismo que `elem.querySelectorAll(css)[0]`, pero este último busca todos los elementos y elige uno, mientras que elem.querySelector sólo busca uno. Así que es más rápido y también más corto de escribir.

### matches

Los métodos anteriores consistían en buscar en el DOM.

El `elem.matches(css)` no busca nada, sólo comprueba si el elem coincide con el selector CSS dado. Devuelve `true` o `false`.

Este método es útil cuando estamos iterando sobre elementos (como en un array) y tratando de filtrar los que nos interesan.

Por ejemplo:

```html
<a href="http://example.com/file.zip">...</a>
<a href="http://ya.ru">...</a>

<script>
  // puede ser cualquier colección en lugar de document.body.children
  for (let elem of document.body.children) {
    if (elem.matches('a[href$="zip"]')) {
      alert("La referencia del archivo: " + elem.href);
    }
  }
</script>
```

### closest

Los ancestros de un elemento son: el padre, el padre del padre, su padre y así sucesivamente. Todos los ancestros juntos forman la cadena de padres desde el elemento hasta la cima.

El método `elem.closest(css)` busca el ancestro más cercano que coincide con el selector CSS. El propio `elem` también se incluye en la búsqueda.

En otras palabras, el método `closest` sube del elemento y comprueba cada uno de los padres. Si coincide con el selector, entonces la búsqueda se detiene y devuelve dicho ancestro.

Por ejemplo:

```html
<h1>Contenido</h1>

<div class="contents">
  <ul class="book">
    <li class="chapter">Capítulo 1</li>
    <li class="chapter">Capítulo 2</li>
  </ul>
</div>

<script>
  let chapter = document.querySelector(".chapter"); // LI

  alert(chapter.closest(".book")); // UL
  alert(chapter.closest(".contents")); // DIV

  alert(chapter.closest("h1")); // null (porque h1 no es un ancestro)
</script>
```

### getElementsBy\*

También hay otros métodos que permiten buscar nodos por una etiqueta, una clase, etc.

- `elem.getElementsByTagName(tag)` busca elementos con la etiqueta dada y devuelve una colección con ellos. El parámetro tag también puede ser un asterisco `"\*"` para “cualquier etiqueta”.
- `elem.getElementsByClassName(className)` devuelve elementos con la clase dada.
- `document.getElementsByName(name)` devuelve elementos con el atributo name dado, en todo el documento. Muy raramente usado.

Por ejemplo:

```html
<table id="table">
  <tr>
    <td>Su edad:</td>

    <td>
      <label>
        <input type="radio" name="age" value="young" checked /> menos de 18
      </label>
      <label>
        <input type="radio" name="age" value="mature" /> de 18 a 50
      </label>
      <label>
        <input type="radio" name="age" value="senior" /> más de 60
      </label>
    </td>
  </tr>
</table>

<script>
  let inputs = table.getElementsByTagName("input");

  for (let input of inputs) {
    alert(input.value + ": " + input.checked);
  }
</script>
```

> [!WARNING] > **¡No olvides la letra "s"!**  
> Los desarrolladores novatos a veces olvidan la letra "s". Esto es, intentan llamar a getElementByTagName en vez de a getElementsByTagName.
> La letra "s" no se encuentra en getElementById porque devuelve sólo un elemento. But getElementsByTagName devuelve una colección de elementos, de ahí que tenga la "s".

> [!WARNING] > **¡Devuelve una colección, no un elemento!**  
> Otro error muy extendido entre los desarrolladores novatos es escribir:
>
> ```javascript
> // no funciona
> document.getElementsByTagName("input").value = 5;
> ```
>
> Esto no funcionará, porque toma una colección de inputs y le asigna el valor a ella en lugar de a los elementos dentro de ella.
> En dicho caso, deberíamos iterar sobre la colección o conseguir un elemento por su índice y luego asignarlo así:
>
> ```javascript
> // debería funcionar (si hay un input)
> document.getElementsByTagName("input")[0].value = 5;
> ```

### Colecciones vivas

Todos los métodos `"getElementsBy*"` devuelven una colección viva (live collection). Tales colecciones siempre reflejan el estado actual del documento y se “auto-actualizan” cuando cambia.

En el siguiente ejemplo, hay dos scripts.

El primero crea una referencia a la colección de `<div>`. Por ahora, su longitud es `1`.
El segundo script se ejecuta después de que el navegador se encuentre con otro `<div>`, por lo que su longitud es de `2`.

```html
<div>Primer div</div>

<script>
  let divs = document.getElementsByTagName("div");
  alert(divs.length); // 1
</script>

<div>Segundo div</div>

<script>
  alert(divs.length); // 2
</script>
```

Por el contrario, `querySelectorAll` devuelve una colección estática. Es como un array de elementos fijos.

Si lo utilizamos en lugar de `getElementsByTagName`, entonces ambos scripts dan como resultado `1`:

```html
<div>Primer div</div>

<script>
  let divs = document.querySelectorAll("div");
  alert(divs.length); // 1
</script>

<div>Segundo div</div>

<script>
  alert(divs.length); // 1
</script>
```

Ahora podemos ver fácilmente la diferencia. La colección estática no aumentó después de la aparición de un nuevo `div` en el documento.

### Resumen

Hay 6 métodos principales para buscar nodos en el DOM:

| Método                 | Busca por…      | ¿Puede llamar a un elemento? | ¿Vivo? |
| ---------------------- | --------------- | ---------------------------- | ------ |
| querySelector          | selector CSS    | ✔                            | -      |
| querySelectorAll       | selector CSS    | ✔                            | -      |
| getElementById         | id              | -                            | -      |
| getElementsByName      | name            | -                            | ✔      |
| getElementsByTagName   | etiqueta o '\*' | ✔                            | ✔      |
| getElementsByClassName | class           | ✔                            | ✔      |

Los más utilizados son `querySelector` y `querySelectorAll`, pero `getElementBy\*` puede ser de ayuda esporádicamente o encontrarse en scripts antiguos.

Aparte de eso:

- Existe `elem.matches(css)` para comprobar si elem coincide con el selector CSS dado.
- Existe `elem.closest(css)` para buscar el ancestro más cercano que coincida con el selector CSS dado. El propio `elem` también se comprueba.
  Y mencionemos un método más para comprobar la relación hijo-padre, ya que a veces es útil:

- `elemA.contains(elemB)` devuelve true si `elemB` está dentro de `elemA` (un descendiente de `elemA`) o cuando `elemA`==`elemB`.

## innerHTML: los contenidos

La propiedad `innerHTML` permite obtener el HTML dentro del elemento como un string.

También podemos modificarlo. Así que es una de las formas más poderosas de cambiar la página.

El ejemplo muestra el contenido de `document.body` y luego lo reemplaza por completo:

```html
<body>
  <p>Un párrafo</p>
  <div>Un div</div>

  <script>
    alert(document.body.innerHTML); // leer el contenido actual
    document.body.innerHTML = "El nuevo BODY!"; // reemplazar
  </script>
</body>
```

Podemos intentar insertar HTML no válido, el navegador corregirá nuestros errores:

```html
<body>
  <script>
    document.body.innerHTML = "<b>prueba"; // olvidé cerrar la etiqueta
    alert(document.body.innerHTML); // <b>prueba</b> (arreglado)
  </script>
</body>
```

> [!NOTE] > **Los scripts no se ejecutan**  
> Si innerHTML inserta una etiqueta `<script>` en el documento, se convierte en parte de HTML, pero no se ejecuta.

## outerHTML: HTML completo del elemento

La propiedad `outerHTML` contiene el HTML completo del elemento. Eso es como `innerHTML` más el elemento en sí.

He aquí un ejemplo:

```html
<div id="elem">Hola <b>Mundo</b></div>

<script>
  alert(elem.outerHTML); // <div id="elem">Hola <b>Mundo</b></div>
</script>
```

## textContent: texto puro

El `textContent` proporciona acceso al texto dentro del elemento: solo texto, menos todas las `<tags>`.

Por ejemplo:

```html
<div id="news">
  <h1>¡Titular!</h1>
  <p>¡Los marcianos atacan a la gente!</p>
</div>

<script>
  // ¡Titular! ¡Los marcianos atacan a la gente!
  alert(news.textContent);
</script>
```

Como podemos ver, solo se devuelve texto, como si todas las `<etiquetas>` fueran recortadas, pero el texto en ellas permaneció.

Escribir en `textContent` es mucho más útil, porque permite escribir texto de “forma segura”.

## Más propiedades

Los elementos DOM también tienen propiedades adicionales, en particular aquellas que dependen de la clase:

- `value` – el valor para `<input>`, `<select>` y `<textarea>` (`HTMLInputElement`, `HTMLSelectElement`, …).
- `href` – el “href” para `<a href="...">` (`HTMLAnchorElement`).
- `id` – el valor del atributo `“id”`, para todos los elementos (`HTMLElement`).
- … y mucho más…

Por ejemplo:

```html
<input type="text" id="elem" value="value" />

<script>
  alert(elem.type); // "text"
  alert(elem.id); // "elem"
  alert(elem.value); // value
</script>
```

El resto de atributos son accesibles usando los siguientes métodos:

- `elem.hasAttribute(nombre)` – comprueba si existe.
- `elem.getAttribute(nombre)` – obtiene el valor.
- `elem.setAttribute(nombre, valor)` – establece el valor.
- `elem.removeAttribute(nombre)` – elimina el atributo.

### Atributos no estándar, dataset

Existe una serie de atributos personalizados no estándares. Los atributos data-\*.

Todos los atributos que comienzan con “data-” están reservados para el uso de los programadores. Están disponibles en la propiedad dataset.

Por ejemplo, si un elem tiene un atributo llamado "data-about", está disponible como elem.dataset.about.

Como esto:

```html
<body data-about="Elefante">
  <script>
    alert(document.body.dataset.about); // Elefante
  </script>
</body>
```

Los atributos de varias palabras como `data-order-state` se convierten en camel-case: `dataset.orderState`

Aquí hay un ejemplo reescrito de “estado del pedido”:

```html
<style>
  .order[data-order-state="nuevo"] {
    color: green;
  }

  .order[data-order-state="pendiente"] {
    color: blue;
  }

  .order[data-order-state="cancelado"] {
    color: red;
  }
</style>

<div id="order" class="order" data-order-state="nuevo">Una nueva orden.</div>

<script>
  // leer
  alert(order.dataset.orderState); // nuevo

  // modificar
  order.dataset.orderState = "pendiente"; // (*)
</script>
```

El uso de los atributos `data-\*` es una forma válida y segura de pasar datos personalizados.

Tenga en cuenta que no solo podemos leer, sino también modificar los atributos de datos. Luego, CSS actualiza la vista en consecuencia: en el ejemplo anterior, la última línea `(\*)` cambia el color a azul.

## Modificando el documento

La modificación del DOM es la clave para crear páginas “vivas”, dinámicas.

Aquí veremos cómo crear nuevos elementos “al vuelo” y modificar el contenido existente de la página.

### Ejemplo: mostrar un mensaje

Hagamos una demostración usando un ejemplo. Añadiremos un mensaje que se vea más agradable que un `alert`.

Así es como se verá:

```html
<style>
  .alert {
    padding: 15px;
    border: 1px solid #d6e9c6;
    border-radius: 4px;
    color: #3c763d;
    background-color: #dff0d8;
  }
</style>

<div class="alert">
  <strong>¡Hola!</strong> Usted ha leído un importante mensaje.
</div>
```

Eso fue el ejemplo HTML. Ahora creemos el mismo `div` con JavaScript (asumiendo que los estilos ya están en HTML/CSS).

#### Creando un elemento

Para crear nodos DOM, hay dos métodos:

`document.createElement(tag);` Crea un nuevo nodo elemento con la etiqueta HTML dada:

```javascript
let div = document.createElement("div");
```

`document.createTextNode(text)` Crea un nuevo nodo texto con el texto dado:

```javascript
let textNode = document.createTextNode("Aquí estoy");
```

La mayor parte del tiempo necesitamos crear nodos de elemento, como el div para el mensaje.

#### Creando el mensjae

Crear el div de mensaje toma 3 pasos:

```javascript
// 1. Crear elemento <div>
let div = document.createElement("div");

// 2. Establecer su clase a "alert"
div.className = "alert";

// 3. Agregar el contenido
div.innerHTML = "<strong>¡Hola!</strong> Usted ha leído un importante mensaje.";
```

Hemos creado el elemento. Pero hasta ahora solamente está en una variable llamada `div`, no aún en la página, y no la podemos ver.

#### Métodos de inserción

Para hacer que el `div` aparezca, necesitamos insertarlo en algún lado dentro de document. Por ejemplo, en el elemento `<body>`, referenciado por `document.body`.

Hay un método especial append para ello: `document.body.append(div)`.

El código completo:

```javascript
<style>
.alert {
  padding: 15px;
  border: 1px solid #d6e9c6;
  border-radius: 4px;
  color: #3c763d;
  background-color: #dff0d8;
}
</style>

<script>
  let div = document.createElement('div');
  div.className = "alert";
  div.innerHTML = "<strong>¡Hola!</strong> Usted ha leído un importante mensaje.";

  document.body.append(div);
</script>
```

Aquí usamos el método `append` sobre `document.body`, pero podemos llamar `append` sobre cualquier elemento para poner otro elemento dentro de él. Por ejemplo, podemos añadir algo a `<div>` llamando `div.append(anotherElement)`.

Aquí hay más métodos de inserción, ellos especifican diferentes lugares donde insertar:

- `node.append(...nodos o strings)` – agrega nodos o strings al final de `node`,
- `node.prepend(...nodos o strings)` – insert nodos o strings al principio de `node`,
- `node.before(...nodos o strings)` –- inserta nodos o strings antes de `node`,
- `node.after(...nodos o strings)` –- inserta nodos o strings después de `node`,
- `node.replaceWith(...nodos o strings)` –- reemplaza node con los nodos o strings dados.

Los argumentos de estos métodos son una lista arbitraria de lo que se va a insertar: nodos DOM o strings de texto (estos se vuelven nodos de texto automáticamente).

Veámoslo en acción.

Aquí tenemos un ejemplo del uso de estos métodos para agregar items a una lista y el texto antes/después de él:

```html
<ol id="ol">
  <li>0</li>
  <li>1</li>
  <li>2</li>
</ol>

<script>
  ol.before("before"); // inserta el string "before" antes de <ol>
  ol.after("after"); // inserta el string "after" después de <ol>

  let liFirst = document.createElement("li");
  liFirst.innerHTML = "prepend";
  ol.prepend(liFirst); // inserta liFirst al principio de <ol>

  let liLast = document.createElement("li");
  liLast.innerHTML = "append";
  ol.append(liLast); // inserta liLast al final de <ol>
</script>
```

La lista final será::

```html
before
<ol id="ol">
  <li>prepend</li>
  <li>0</li>
  <li>1</li>
  <li>2</li>
  <li>append</li>
</ol>
after
```

### Eliminación de nodos

Para quitar un nodo, tenemos el método node.remove().

Hagamos que nuestro mensaje desaparezca después de un segundo:

```html
<style>
  .alert {
    padding: 15px;
    border: 1px solid #d6e9c6;
    border-radius: 4px;
    color: #3c763d;
    background-color: #dff0d8;
  }
</style>

<script>
  let div = document.createElement("div");
  div.className = "alert";
  div.innerHTML =
    "<strong>¡Hola!</strong> Usted ha leído un importante mensaje.";

  document.body.append(div);
  setTimeout(() => div.remove(), 1000);
</script>
```

### Clonando nodos: cloneNode

¿Cómo insertar un mensaje similar más?

Podríamos hacer una función y poner el código allí. Pero la alternativa es clonar el `div` existente, y modificar el texto dentro si es necesario.

A veces, cuando tenemos un elemento grande, esto es más simple y rápido.

La llamada `elem.cloneNode(true)` crea una clonación “profunda” del elemento, con todos los atributos y subelementos. Si llamamos `elem.cloneNode(false)`, la clonación se hace sin sus elementos hijos.
Un ejemplo de copia del mensaje:

```html
<style>
  .alert {
    padding: 15px;
    border: 1px solid #d6e9c6;
    border-radius: 4px;
    color: #3c763d;
    background-color: #dff0d8;
  }
</style>

<div class="alert" id="div">
  <strong>¡Hola!</strong> Usted ha leído un importante mensaje.
</div>

<script>
  let div2 = div.cloneNode(true); // clona el mensaje
  div2.querySelector("strong").innerHTML = "¡Adiós!"; // altera el clon

  div.after(div2); // muestra el clon después del div existente
</script>
```

### Resumen de modificar el documento

- Métodos para crear nuevos nodos:
  - `document.createElement(tag)` – crea un elemento con la etiqueta HTML dada
  - `document.createTextNode(value)` – crea un nodo de texto (raramente usado)
  - `elem.cloneNode(deep)` – clona el elemento. Si deep==true, lo clona con todos sus descendientes.
- Inserción y eliminación:
  - `node.append(...nodes or strings)` – inserta en node, al final
  - `node.prepend(...nodes or strings)` – inserta en node, al principio
  - `node.before(...nodes or strings)` –- inserta inmediatamente antes de node
  - `node.after(...nodes or strings)` –- inserta inmediatamente después de node
  - `node.replaceWith(...nodes or strings)` –- reemplaza node
  - `node.remove()` –- quita el node.
