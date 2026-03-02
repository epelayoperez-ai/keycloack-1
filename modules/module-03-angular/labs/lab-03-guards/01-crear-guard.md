# 📄 Lab 03 — Guards

## Paso 01 — Crear el Auth Guard (versión manual)

---

## 🎯 Objetivo

Crear un `Route Guard` que:

* Permita acceso solo si el usuario está autenticado.
* Redirija al login si no lo está.
* Use la instancia manual de `keycloak-js`.

Aquí todavía estamos usando el bootstrap manual del laboratorio anterior.

---

## 📍 Ubicación

Desde:

```bash
apps/angular-app/angular-app
```

---

## 🛠 Generar el guard

Ejecutar:

```bash
npx ng generate guard auth --functional
```

Seleccionar:

```
CanActivate
```

Esto generará:

```
src/app/auth.guard.ts
```

---

# ✍️ Implementar lógica manual

Abrir:

```
src/app/auth.guard.ts
```

Reemplazar el contenido por:

```typescript
import { CanActivateFn } from '@angular/router';
import { getKeycloakInstance } from './keycloak.service';

export const authGuard: CanActivateFn = async (route, state) => {
  const keycloak = getKeycloakInstance();

  if (!keycloak) {
    return false;
  }

  if (keycloak.authenticated) {
    return true;
  }

  await keycloak.login({
    redirectUri: window.location.origin + state.url
  });

  return false;
};
```

---

# 🧠 Qué hace este guard

1️⃣ Consulta la instancia global de Keycloak
2️⃣ Comprueba si el usuario está autenticado
3️⃣ Si no lo está:

* Lanza `login()`
* Redirige a Keycloak
* Conserva la URL original
  4️⃣ Si está autenticado:
* Permite acceso

---

# 🧠 Concepto importante

Este guard funciona porque:

* Keycloak ya fue inicializado en `main.ts`
* El bootstrap quedó bloqueado hasta resolver `init()`
* La propiedad `authenticated` ya está disponible

Si no hubiéramos hecho bootstrap manual correctamente,
este guard fallaría o produciría loops.

---

# 📌 Estado actual

✔ Bootstrap manual activo
✔ Guard manual creado
✔ Autenticación bajo nuestro control

---

## ➡️ Siguiente paso

En el siguiente archivo:

```
02-proteger-rutas.md
```

Aplicaremos el guard a una ruta concreta y veremos el flujo completo en acción.

Después de validar esto, desmontaremos el bootstrap manual y entraremos en el wrapper oficial.
