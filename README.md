# Página de pregunta y constructor de formularios

Además de la pregunta clásica, `/admin` permite crear formularios completos con campos de texto, email, teléfono, número, fecha, hora, URL, áreas de texto, selects, radios y casillas. Cada campo puede tener etiqueta, nombre técnico, texto de ayuda, placeholder, opciones y ser obligatorio. Se puede reordenar, eliminar y previsualizar.

Los enlaces nuevos usan una configuración `v:2` comprimida y ofuscada, y los enlaces antiguos siguen funcionando. La compresión reduce bastante el tamaño, pero ningún sitio estático puede convertir una configuración enorme en una URL corta universal sin guardar esos datos en algún servidor. Para enlaces cortos compartibles habría que añadir un backend o servicio de short links con almacenamiento.

Página de un solo archivo (`index.html`, sin dependencias ni build) que hace una pregunta con dos botones: **Sí** y **No**. El botón **No** huye cuando intentas pulsarlo, y al pulsar **Sí** aparece un formulario cuya respuesta te llega por correo.

## Cómo funciona

- La pregunta, el subtítulo, el estilo visual y el texto de los botones se controlan desde la URL, así que el mismo archivo sirve para enlaces distintos.
- El botón **No** esquiva el ratón, el toque en móvil y el foco con teclado. Con cada esquive el botón **Sí** crece un poco.
- Al pulsar **Sí** se abre un formulario con nombre y mensaje opcional. Al enviarlo, [FormSubmit](https://formsubmit.co) manda un correo con la respuesta.

## Parámetros de la URL

Todos son opcionales.

| Parámetro | Qué hace | Por defecto |
|---|---|---|
| `pregunta` | Texto del título. Usa `\|` para saltar de línea | `¿Quieres mucho a FLO?` |
| `subtexto` | Texto pequeño bajo el título. Puede ir vacío u omitirse | sin subtítulo |
| `estilo` | Número del 1 al 10 (ver abajo) | `1` |
| `si` | Texto del botón afirmativo | `Sí` |
| `no` | Texto del botón que huye | `No` |

Ejemplo:

```
index.html?pregunta=¿Quieres ir al cine|conmigo?&subtexto=Invito yo&estilo=4&si=Claro
```

Sin parámetros se muestra "¿Quieres mucho a FLO?" sin subtítulo.

## Enlaces ofuscados y /admin

Para que el mensaje no se lea en la URL, todos los datos pueden ir en un único parámetro: `?d=...`. La página lo decodifica y muestra lo mismo que con los parámetros normales (que siguen funcionando).

`admin/index.html` es un generador: escribes pregunta, subtexto, estilo y textos de botones, ves una vista previa en directo y copias el enlace ya ofuscado.

- El ofuscado es JSON + XOR con una clave + Base64 seguro para URL. La constante `CLAVE` debe ser **igual** en `index.html` y en `admin/index.html`, y conviene cambiarla por una propia.
- Ofuscar no es cifrar: la clave está en el código de la página, así que sirve para que no se lea a simple vista, no para proteger el contenido.
- `/admin` solo genera enlaces y lleva `noindex`, pero cualquiera que conozca la ruta puede abrirlo. Si quieres restringirlo, protégelo desde el hosting (por ejemplo, Cloudflare Access o contraseña en Netlify).

## Estilos

| Id | Nombre | Look |
|---|---|---|
| 1 | Noche | Violeta oscuro y coral, tipografía serif |
| 2 | Atardecer | Degradado rosa y violeta con cristal translúcido |
| 3 | Minimal | Blanco roto y tinta negra |
| 4 | Neón | Fondo oscuro con brillos magenta y cian |
| 5 | Cuaderno | Papel pautado con sombras duras y letra manuscrita |
| 6 | Arcade | Pixel art, scanlines y colores de videojuego |
| 7 | Bosque | Verdes profundos y tonos naturales |
| 8 | Océano | Azules con burbujas flotantes |
| 9 | Fiesta | Amarillo vivo, bordes gruesos y sombras planas |
| 10 | Terminal | Verde sobre negro, tipografía monoespaciada |

Cada estilo es un bloque de variables CSS (`:root[data-estilo="N"]`). Para añadir uno nuevo, copia un bloque, cambia el número y añade su símbolo de partículas en `SIMBOLOS` dentro del script.

## Configuración del correo

En el script, la constante `FORMSUBMIT_URL` contiene el correo de destino. La primera vez que se envíe una respuesta, FormSubmit pedirá confirmar ese correo.

Campos que recibes: `nombre`, `mensaje`, `pregunta`, `respuesta` (el texto del botón Sí) y `estilo`. El asunto es "¡{nombre} ha dicho que sí!".

## Detalles a tener en cuenta

- **Vista previa del enlace:** WhatsApp, Instagram y similares no ejecutan JavaScript, así que siempre muestran las etiquetas `og:` genéricas del HTML ("Tengo una pregunta para ti"), no la pregunta de la URL.
- **Seguridad:** el texto que llega por la URL se inserta con `textContent`, nunca como HTML.
- **Accesibilidad:** foco visible con teclado y animaciones desactivadas con `prefers-reduced-motion`.
- **Fuentes:** se cargan desde Google Fonts.

## Despliegue

Es un sitio estático: sube `index.html` a cualquier hosting (Netlify, Cloudflare Pages, GitHub Pages, un servidor propio...).
