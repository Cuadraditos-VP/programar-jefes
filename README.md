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

La programación (meses, turnos, jefes, preferencias y colores) se guarda en **Firebase** y es la misma para todos los usuarios: lo que cambia uno lo ven los demás al momento. Cada navegador guarda además una copia para abrir al instante y sin internet; lo que se cambie sin conexión se manda cuando vuelve.

- **Entrar:** el administrador entra con su cuenta de Google. Los demás, con su **legajo y una clave de 4 números**, en cualquier computadora o navegador.
- **Invitar:** ☁ → nombre y legajo → Invitar. Da un código de 6 números: la primera vez el usuario lo escribe y elige su clave.
- **Se olvidó la clave:** ☁ → Clave nueva (le das otro código). **Quitar el acceso:** ☁ → Quitar (sale de todas las computadoras).
- El botón de la nube muestra un puntito: verde = todo guardado, amarillo = guardando, rojo = problema.
- Las reglas de Firebase están en `REGLAS-FIREBASE.md`.

## Actualizar la app

Subí el `index.html` nuevo al repositorio (reemplazando el anterior) y en `sw.js` cambiá `VERSION` (por ejemplo de `jefes-v1.0` a `jefes-v1.1`). La próxima vez que se abra con internet, se actualiza sola.
