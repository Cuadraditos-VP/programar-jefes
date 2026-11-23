# Reglas de Firebase para Programación Jefes

Usa el mismo proyecto que la app de jefes (**programacionjefes**) y la función `esAdmin()` que ya está.
Este bloque va **adentro** de `match /databases/{database}/documents { ... }`, junto a los demás.
También tiene que estar activado **Correo electrónico/contraseña** en Authentication (ya lo usa la clave de la app de jefes).

- `progr_jefes`: la programación (un doc por mes `mes_AAAA-MM` y `general` con preferencias y colores).
- `progr_invitados/{código}`: invitaciones que crea el admin (nombre, legajo, v).
- `progr_cuentas/{usuario}`: la cuenta de cada usuario (correo inventado `p<usuario>-<v>@progrjefes.example.com`, contraseña `vp<clave>-<usuario>`).
  Clave nueva = se borra y se hace otra invitación (con otro `v`). Quitar = se borra.
- `progr_usuarios/{uid}`: uid → usuario (el nombre sin acentos ni espacios, ej. `juanperez`) y nombre.
- `progr_historial`: quién hizo cada cambio y cuándo (cada casillero además guarda el último en `por`).

```
    // ===== NUEVO: Programación Jefes (la app para armar la programación) =====
    // Cada usuario entra con nombre + clave de 4 números (una cuenta por usuario, en progr_cuentas).
    function esUsuarioProg() {
      return request.auth != null
        && exists(/databases/$(database)/documents/progr_usuarios/$(request.auth.uid))
        && get(/databases/$(database)/documents/progr_cuentas/$(get(/databases/$(database)/documents/progr_usuarios/$(request.auth.uid)).data.leg)).data.uid == request.auth.uid;
    }
    function invProg(code) {
      return get(/databases/$(database)/documents/progr_invitados/$(code)).data;
    }
    // La programación: un doc por mes (mes_AAAA-MM) y "general" (preferencias y colores)
    match /progr_jefes/{doc} {
      allow read, write: if esAdmin() || esUsuarioProg();
    }
    // Invitaciones (código de 6 números para elegir la clave la primera vez). Las crea y borra el admin.
    match /progr_invitados/{code} {
      allow get: if request.auth != null;
      allow list, create, delete: if esAdmin();
      allow update: if request.auth != null
        && !('uid' in resource.data)
        && request.resource.data.diff(resource.data).affectedKeys().hasOnly(['uid', 'usadoEn'])
        && request.resource.data.uid == request.auth.uid;
    }
    // Qué cuenta tiene cada legajo. La crea quien usó la invitación de ese legajo.
    match /progr_cuentas/{legajo} {
      allow get: if request.auth != null;
      allow list, delete: if esAdmin();
      allow create, update: if esAdmin() || (request.auth != null
        && request.resource.data.uid == request.auth.uid
        && request.resource.data.keys().hasOnly(['uid', 'v', 'code'])
        && invProg(request.resource.data.code).uid == request.auth.uid
        && invProg(request.resource.data.code).leg == legajo
        && invProg(request.resource.data.code).v == request.resource.data.v);
    }
    // Historial: quién cambió qué. Lo agregan los usuarios (a su nombre); se borra solo desde la consola.
    match /progr_historial/{id} {
      allow read: if esAdmin() || esUsuarioProg();
      allow create: if (esAdmin() || esUsuarioProg()) && request.resource.data.uid == request.auth.uid;
    }
    // Quién es cada cuenta (uid → usuario y nombre)
    match /progr_usuarios/{uid} {
      allow get: if esAdmin() || (request.auth != null && request.auth.uid == uid);
      allow list, delete: if esAdmin();
      allow create, update: if request.auth != null && request.auth.uid == uid
        && get(/databases/$(database)/documents/progr_cuentas/$(request.resource.data.leg)).data.uid == uid;
    }
```
