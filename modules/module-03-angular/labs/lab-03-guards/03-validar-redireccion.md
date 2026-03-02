
# 📄 Lab 03 — Guards

## Paso 03 — Validar la redirección y protección de rutas (Bootstrap Manual)

---

## 🎯 Objetivo

Verificar que:

* El `authGuard` se ejecuta correctamente.
* La ruta `/protected` no es accesible sin sesión válida.
* El flujo Angular → Keycloak → Angular funciona sin errores.
* El bootstrap manual no genera loops ni estados inconsistentes.

Seguimos utilizando:

* `keycloak-js`
* Bootstrap manual en `main.ts`
* Guard manual

---

## 🧪 Escenario 1 — Usuario autenticado

1. Asegúrate de haber iniciado sesión previamente.
2. Navega manualmente a:

```
http://localhost:4200/protected
```

### ✅ Resultado esperado

* La vista `Protected` se muestra correctamente.
* No se produce redirección.
* En consola, `keycloak.authenticated` es `true`.

---

## 🧪 Escenario 2 — Usuario NO autenticado

1. Cierra sesión desde Keycloak o limpia sesión del navegador.
2. Abre directamente:

```
http://localhost:4200/protected
```

---

### 🔎 Qué ocurre técnicamente

1️⃣ Angular intenta activar la ruta.
2️⃣ Se ejecuta `authGuard`.
3️⃣ `keycloak.authenticated` es `false`.
4️⃣ Se ejecuta:

```ts
keycloak.login({
  redirectUri: window.location.origin + state.url
});
```

5️⃣ El navegador redirige a Keycloak.
6️⃣ Tras autenticarse:

* Keycloak redirige de vuelta a `/protected`.
* `keycloak.init()` procesa el `code`.
* El guard ahora devuelve `true`.
* La vista protegida se carga.

---

### ✅ Resultado esperado

* No se muestra la vista protegida antes del login.
* Se produce redirección automática.
* Tras autenticarse, vuelve a `/protected`.
* No hay loops de login.
* No hay errores en consola.

---

# 🧠 Qué estamos validando realmente

✔ El bootstrap manual bloquea correctamente el arranque.
✔ `init()` procesa el authorization code correctamente.
✔ El guard se ejecuta después de que Keycloak esté listo.
✔ La URL original se conserva tras autenticación.
✔ El flujo Authorization Code + PKCE está funcionando.

---

# 📌 Estado actual de la aplicación

En este punto:

* Angular está autenticado usando `keycloak-js`.
* El control del ciclo es completamente manual.
* Sabemos exactamente qué hace `init()`.
* Sabemos exactamente qué hace el guard.

Estamos preparados para simplificar esta arquitectura.

