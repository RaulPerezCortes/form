# ¿Quieres salir con...?

Página de una sola pantalla con una pregunta importante. El botón "No" huye cuando intentas pulsarlo, y al decir "Sí" aparece un pequeño formulario cuya respuesta llega por correo.

Todo vive en un único archivo: `index.html` (HTML, CSS y JS, sin dependencias salvo la fuente de Google Fonts).

Dominio previsto: `love.raulperez.dev`

## Cómo funciona

### El nombre sale de la URL

La página lee el parámetro `nombre` del enlace y lo pone en el título, el titular y la etiqueta del mensaje:

```
https://love.raulperez.dev/?nombre=Adrià
```

- Si no hay parámetro, usa el nombre por defecto (`Adrià`), que está en esta línea del script:

  ```js
  const NOMBRE = (params.get('nombre') || 'Adrià').trim().slice(0, 40);
  ```

- Para nombres con espacios usa `%20`: `?nombre=Ana%20Maria`.
- Se inserta con `textContent`, así que aunque alguien meta HTML en la URL no se ejecuta.

### El botón "No" esquivo

Se mueve a una posición aleatoria al pasar el ratón, al tocarlo en móvil y también al hacer clic, y muestra frases de broma. No hay forma de pulsarlo.

### El envío del "Sí"

Al pulsar "Sí" se muestra un formulario (nombre de quien responde y mensaje opcional). Al enviarlo, la página hace un `POST` a [FormSubmit](https://formsubmit.co) y te llega un correo con:

- Asunto: `¡<nombre> ha dicho que SÍ a salir con <Adrià>!`
- Cuerpo en formato tabla: nombre, mensaje y respuesta.

## Puesta en marcha

### 1. Activar FormSubmit (solo la primera vez)

1. Abre la página, di que sí y envía el formulario tú mismo como prueba.
2. FormSubmit te manda un correo de confirmación a la dirección configurada. Revisa también spam.
3. Pulsa el enlace de activación. Hasta entonces no llegará ningún envío.

Opcional: tras activarlo, FormSubmit te da una URL con un alias aleatorio. Cámbiala en `FORMSUBMIT_URL` para no tener tu correo visible en el código:

```js
const FORMSUBMIT_URL = 'https://formsubmit.co/ajax/<tu-alias-aleatorio>';
```

### 2. Publicar la página

Al ser un archivo estático vale cualquier hosting estático (GitHub Pages, Cloudflare Pages, Netlify...). Para usar `love.raulperez.dev`:

1. Sube `index.html` al hosting elegido.
2. En el DNS de `raulperez.dev` crea un registro `CNAME` para `love` apuntando al dominio que te dé el hosting.
3. Añade `love.raulperez.dev` como dominio personalizado en el hosting y espera a que se emita el certificado HTTPS.

## Personalización

| Qué | Dónde |
| --- | --- |
| Nombre por defecto | `const NOMBRE` en el script |
| Frases del botón "No" | array `frases` en el script |
| Correo/alias de destino | `FORMSUBMIT_URL` en el script |
| Colores | variables `:root` al inicio del CSS |
| Textos de la tarjeta | bloque `.card` del HTML |

## Notas

- Si el envío falla, se muestra un mensaje de error y el botón se vuelve a activar para reintentar.
- Con `prefers-reduced-motion` activado se desactiva la animación de los corazones.
"# love" 
