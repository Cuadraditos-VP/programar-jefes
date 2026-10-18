# Programación Jefes

App para armar la programación mensual de turnos de jefes. Es una PWA: se instala en el celular o la computadora y funciona sin internet.

## Archivos

| Archivo | Para qué sirve |
|---|---|
| `index.html` | La app completa |
| `manifest.json` | Nombre, colores e íconos para instalarla |
| `sw.js` | Hace que funcione sin internet |
| `icons/` | Íconos de la app |
| `.nojekyll` | Le indica a GitHub que publique los archivos tal cual |

## Publicar en GitHub Pages

1. En GitHub creá un repositorio nuevo, por ejemplo `programacion-jefes` (público).
2. Entrá al repositorio → **Add file → Upload files** y arrastrá **todo el contenido** de esta carpeta (incluida la carpeta `icons`). Confirmá con **Commit changes**.
3. Andá a **Settings → Pages**. En *Build and deployment* elegí **Deploy from a branch**, rama **main**, carpeta **/ (root)** y tocá **Save**.
4. En uno o dos minutos la app queda en `https://TU-USUARIO.github.io/programacion-jefes/`.

## Instalarla

- **Android (Chrome):** abrí el link → menú ⋮ → **Instalar app** (o "Agregar a la pantalla principal").
- **iPhone (Safari):** abrí el link → botón Compartir → **Agregar a inicio**.
- **Computadora (Chrome/Edge):** ícono de instalar en la barra de direcciones.

## Video de apertura

El video de apertura ya está incluido dentro de `index.html` (no hace falta ningún archivo aparte). Dura unos segundos; tocando la pantalla se saltea.

## Tus datos

Los datos (meses, turnos, jefes, preferencias) se guardan en el navegador de cada dispositivo. **No viajan solos entre dispositivos ni desde el archivo que usabas antes.** Para pasarlos:

1. En el lugar donde ya tenés los datos, tocá el botón **JSON** → *Todo* → **Exportar JSON**. Se descarga `jefes_datos_AAAA-MM-DD.json`.
2. En la app publicada, botón **JSON** → **Importar JSON…** y elegí ese archivo. *Combinar* agrega/actualiza los meses del archivo; *Reemplazar todo* deja exactamente lo del archivo. Antes de importar se descarga sola una copia de lo que había.

También podés exportar **solo el mes** que estás viendo (por ejemplo, para mandárselo a otra persona).

Conviene exportar una copia de vez en cuando como resguardo.

## Actualizar la app

Subí el `index.html` nuevo al repositorio (reemplazando el anterior) y en `sw.js` cambiá `VERSION` (por ejemplo de `jefes-v1.0` a `jefes-v1.1`). La próxima vez que se abra con internet, se actualiza sola.
