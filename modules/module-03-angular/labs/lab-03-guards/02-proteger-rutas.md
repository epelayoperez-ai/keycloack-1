 📄 Lab 03 — Guards

## Paso 02 — Proteger rutas con el Auth Guard

---

## 🎯 Objetivo

Aplicar el `authGuard` manual a la ruta `/protected` para:

* Permitir acceso solo si existe sesión válida.
* Forzar redirección a Keycloak si no hay autenticación.
* Observar el flujo completo desde Angular → Keycloak → Angular.

Seguimos trabajando con el bootstrap manual del laboratorio anterior.

---

## 📍 Editar archivo de rutas

Abrir:

```
src/app/app.routes.ts
```

---

## ✍️ Configurar la protección

Actualizar el contenido a:

```typescript
import { Routes } from '@angular/router';
import { HomeComponent } from './home/home.component';
import { ProtectedComponent } from './protected/protected.component';
import { authGuard } from './auth.guard';

export const routes: Routes = [
  { path: '', component: HomeComponent },
  {
    path: 'protected',
    component: ProtectedComponent,
    canActivate: [authGuard]
  }
];
```

---

## 🧠 Qué está ocurriendo realmente

Cuando el usuario navega a:

```
/protected
```

Angular:

1️⃣ Ejecuta el `authGuard`.
2️⃣ El guard consulta `keycloak.authenticated`.
3️⃣ Si es `false`:

* Se ejecuta `keycloak.login()`.
* El navegador redirige a Keycloak.
  4️⃣ Tras autenticarse:
* Keycloak redirige a Angular.
* `keycloak.init()` procesa el `code`.
* El guard ahora devuelve `true`.
* Se carga `ProtectedComponent`.

---

## ▶️ Probar el flujo

Reiniciar si es necesario:

```bash
npm start
```

Ir a:

```
http://localhost:4200/protected
```

---

## 🔎 Comportamiento esperado

Si NO hay sesión:

* El navegador redirige a Keycloak.
* Se solicita login.
* Tras autenticarse, vuelve a `/protected`.

Si YA hay sesión:

* La ruta se carga directamente.

---

## 🧠 Qué estamos validando

✔ El bootstrap manual funciona correctamente.
✔ El guard se ejecuta después de `init()`.
✔ El flujo Authorization Code + PKCE se completa sin loops.
✔ La URL original se conserva tras login.
