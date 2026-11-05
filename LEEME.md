# Landing Premedic — Guía para editar el sitio

## ¿Qué es esto?

Es la página web (landing) de **Premedic · Agencia Comercial Oficial**: presenta los planes, los beneficios y los prestadores, y lleva al visitante a escribir por WhatsApp para cotizar.

Es un sitio **estático**: no hay base de datos, ni servidor, ni "instalar" nada. Con tener la carpeta y un navegador alcanza.

### Cómo verla en tu computadora

1. Abrí una terminal dentro de esta carpeta.
2. Ejecutá:

   ```
   python3 -m http.server 8000
   ```

   (En Windows, si `python3` no funciona, probá con `python -m http.server 8000`.)
3. Abrí en el navegador: <http://localhost:8000>
4. Para cortar el servidor, `Ctrl + C` en la terminal.

> No abras `index.html` con doble clic: algunos navegadores bloquean la carga de archivos desde `file://`. Usá siempre el servidor local.

La página necesita internet para dos cosas: la tipografía (Fontshare) y el motor que la dibuja (React, desde unpkg).

---

## Mapa de archivos

| Archivo o carpeta | Qué es | ¿Se edita? |
|---|---|---|
| `index.html` | **La página completa**: textos, diseño y comportamiento. Todo lo que cambia seguido está en el bloque `<script>` del final. | **Sí** |
| `support.js` | Motor de Claude Design que "arma" la página. | **No** (es generado) |
| `assets/brand/` | Logo de Premedic. | Sí (reemplazar archivos) |
| `assets/hero/` | Videos e imagen de las tarjetas de la portada. | Sí |
| `assets/plans/` | Imagen de cabecera de cada plan. | Sí |
| `assets/benefits/` | Imágenes de "Pensado para tu día a día" y de "Beneficios que suman". | Sí |
| `assets/providers/` | Logos de clínicas y sanatorios (PNG con fondo transparente). | Sí |
| `assets/agency/` | Logo de Grupo Amro y sello de 12 años (footer). | Sí |
| `LEEME.md` | Esta guía. | Sí |
| `serve.js`, `package.json` | Servidor de prueba viejo (apunta a un archivo que ya no existe). **No se usan**; usá el comando de Python de arriba. | No |

### Cómo está ordenado `index.html`

Tiene tres partes, con carteles de comentario en español:

1. **HEAD**: título y descripción para Google, vista previa al compartir (Open Graph), fuentes, **colores de la marca** y pantalla de carga.
2. **TEMPLATE**: la página en sí, una sección por bloque (`SECCIÓN: Header`, `Hero`, `Planes`, etc.).
3. **SCRIPT** (al final), siempre en este orden:
   1. **CONFIG**: WhatsApp, horarios, Instagram, datos de la agencia y opciones (`SHOW_…`).
   2. **ASSETS**: rutas de todas las imágenes y videos.
   3. **CONTENIDO**: planes, beneficios, destacados, prestadores, preguntas frecuentes, cifras, pasos y mensajes de WhatsApp.
   4. **HELPERS**: utilidades internas.
   5. **class Component**: el comportamiento (animaciones, carruseles, etc.). Rara vez hace falta tocarlo.

**Tip:** buscá con `Ctrl + F` el texto `EDITAR:`. Cada cosa pensada para modificarse está marcada así. Los comentarios `[CONFIRMAR CON CLIENTE]` indican datos que todavía hay que validar con el cliente: no los borres hasta confirmarlos.

---

## Quiero cambiar… → editá…

| Quiero cambiar… | Editá… |
|---|---|
| **El número de WhatsApp** (a donde llegan todos los mensajes) | `WHATSAPP_NUMBER` en el bloque **1. CONFIG**. Formato: `549` + código de área + número, sin espacios ni guiones (ej. `5491122334455`). |
| **El número tal como se ve en pantalla** | `WHATSAPP_DISPLAY` (ej. `11 2233-4455`). |
| **El horario de atención** | `WHATSAPP_HOURS`. |
| **El link de Instagram** | `INSTAGRAM_URL`. |
| **Los textos de un plan** (nombre, frase, viñetas, detalle, mensaje de WhatsApp) | En **3. CONTENIDO**, la lista `PLANES`: buscá el plan y cambiá `sub` (frase corta), `points` (viñetas de la tarjeta), `detalle` (lista del panel "Ver más detalles"), `nota` y `msg` (mensaje que llega por WhatsApp). |
| **Agregar un plan** | En `PLANES`, copiá un bloque `{ … }` completo (de la llave `{` a la `},`), pegalo donde quieras que aparezca y cambiá sus textos. Para su imagen, sumá una ruta nueva en **2. ASSETS** (ej. `plan500`) y poné ese nombre en `asset:`. El orden de la lista es el orden en pantalla. |
| **Sacar un plan** | Borrá su bloque `{ … }` en `PLANES`. (El plan "Transporte público" no se borra: se muestra con `SHOW_PLAN_TRANSPORTE = true`). |
| **Una imagen o video** | Reemplazá el archivo en su carpeta de `assets/` **con el mismo nombre**, o subí uno nuevo y cambiá la ruta en **2. ASSETS**. Si dejás la ruta vacía (`''`) se muestra un fondo de relleno. Formatos recomendados: `.avif` o `.webp` para fotos, `.mp4` para videos. |
| **Agregar o sacar un logo de prestador** | Copiá el archivo (PNG con fondo transparente, idealmente de 88×88 px o más) a `assets/providers/` y sumá o borrá una línea en `PRESTADORES_LOGOS`: `{ name: 'Clínica X', src: 'assets/providers/clinica-x.png' }`. |
| **La lista de prestadores de CABA** | `PRESTADORES_CABA` (los nombres de texto de la pestaña CABA). |
| **Las preguntas frecuentes** | `FAQS`: cada línea tiene `q` (pregunta) y `a` (respuesta). Sumá o borrá líneas para agregar o sacar preguntas. |
| **Las cifras** ("+200 clínicas…") | `CIFRAS`: `value` es el número, `pre` y `suf` el prefijo/sufijo (ej. `+` o ` hs`), `label` el texto de abajo. |
| **El título principal de la portada** | `HERO_TITLE`. |
| **Los beneficios o los destacados** | `BENEFICIOS` y `DESTACADOS` en **3. CONTENIDO**. |
| **Los 3 pasos / los mensajes de WhatsApp de "¿Cómo te asociás?"** | `PASOS` y `MSG_ASOCIARTE`. |
| **Los colores de la marca** | En el **HEAD** de `index.html`, bloque `:root { --c-primary: … }`. Cambiás el color ahí y se actualiza en toda la página. (Algunos íconos sueltos llevan el color dentro del propio dibujo SVG.) |
| **Los datos de la agencia** (razón social, CUIT, matrícula, RNEMP, política de privacidad) | `AGENCIA_DATOS`, `RNEMP` y `PRIVACY_URL` en **1. CONFIG**. |
| **El logo de Grupo Amro o el sello de 12 años** | Reemplazá los archivos de `assets/agency/` (mismo nombre) o cambiá `logoGrupoAmro` / `selloAnios` en **2. ASSETS**. |
| **El título y la descripción en Google, y la vista previa al compartir** | En el **HEAD**: `<title>`, `description` y las etiquetas `og:`. La imagen de vista previa (`og:image`) apunta a `assets/og-premedic.webp`, que **todavía no existe**: subí una imagen de 1200×630 px con ese nombre. |
| **Medir clics en WhatsApp** (Meta Pixel u otra herramienta) | Función `trackWhatsAppClick` en **4. HELPERS**. |

Después de cada cambio: guardá, recargá el navegador (`Ctrl + F5`) y revisá en computadora y en celular.

---

## Cómo publicar

El sitio no necesita compilar nada: se publica tal cual. Hay que subir **`index.html`, `support.js` y la carpeta `assets/`** (no hace falta `serve.js` ni `package.json`).

### Opción A — GitHub Pages (gratis)

1. Subí esta carpeta a un repositorio de GitHub.
2. En el repositorio: **Settings → Pages**.
3. En *Source* elegí **Deploy from a branch**, la rama `main` y la carpeta `/ (root)`. Guardá.
4. A los pocos minutos queda publicada en `https://TU-USUARIO.github.io/NOMBRE-DEL-REPO/`.

### Opción B — Cloudflare Pages (gratis)

1. En Cloudflare: **Workers & Pages → Create → Pages → Connect to Git** y elegí el repositorio.
2. *Framework preset*: **None**. *Build command*: dejalo vacío. *Build output directory*: `/` (la raíz).
3. **Save and Deploy**. Cada vez que subas cambios al repositorio, se vuelve a publicar solo.
   (También se puede arrastrar la carpeta con **Direct Upload**, sin usar Git.)

---

## ⚠️ Advertencias importantes

- **`support.js` es generado, no se edita.** Es el motor de Claude Design; si se modifica, la página puede dejar de dibujarse.
- **El formato de `index.html` es el de Claude Design (`<x-dc>`), no es React estándar.** Las partes con `{{ doble llave }}` y etiquetas como `<sc-if>` y `<sc-for>` las interpreta ese motor. No se pueden copiar y pegar como si fueran HTML o React común, ni abrir el archivo en herramientas que esperan un proyecto Vite/React.
- **No pongas `{{` ni la etiqueta `<x-dc>` dentro de comentarios** del archivo: el motor las lee como parte de la página y puede dejarla en blanco.
