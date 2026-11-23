# Reglas de Firebase para Programación Jefes

Usa el mismo proyecto que la app de jefes (**programacionjefes**). Estos bloques van **adentro** de
`match /databases/{database}/documents { ... }`, junto a los demás, y usan la función `esAdmin()` que ya está.

- `progr_jefes`: la programación (un doc por mes `mes_AAAA-MM` y `general` con preferencias y colores).
- `progr_invitados/{código}`: invitaciones que crea el admin (una por computadora/navegador).
- `progr_usuarios/{uid}`: quién entró con qué código. Quitar el acceso = borrar la invitación.

```
    // ===== Programación Jefes (app para armar la programación) =====
    function esUsuarioProg() {
      return request.auth != null
        && exists(/databases/$(database)/documents/progr_usuarios/$(request.auth.uid))
        && get(/databases/$(database)/documents/progr_invitados/$(get(/databases/$(database)/documents/progr_usuarios/$(request.auth.uid)).data.code)).data.uid == request.auth.uid;
    }
    match /progr_jefes/{doc} {
      allow read, write: if esAdmin() || esUsuarioProg();
    }
    match /progr_invitados/{code} {
      allow get: if request.auth != null;
      allow list, create, delete: if esAdmin();
      allow update: if request.auth != null
        && !('uid' in resource.data)
        && request.resource.data.diff(resource.data).affectedKeys().hasOnly(['uid', 'usadoEn'])
        && request.resource.data.uid == request.auth.uid;
    }
    match /progr_usuarios/{uid} {
      allow get: if esAdmin() || (request.auth != null && request.auth.uid == uid);
      allow list, delete: if esAdmin();
      allow create: if request.auth != null && request.auth.uid == uid
        && get(/databases/$(database)/documents/progr_invitados/$(request.resource.data.code)).data.uid == uid;
      allow update: if request.auth != null && request.auth.uid == uid
        && request.resource.data.code == resource.data.code;
    }
```
