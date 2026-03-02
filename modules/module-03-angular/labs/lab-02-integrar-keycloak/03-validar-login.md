# Lab 02 — Integrar Keycloak (Bootstrap Manual)

## Paso 03 — Validar la inicialización

---

## 🎯 Objetivo

Comprobar que:

* Keycloak se inicializa correctamente antes del arranque.
* La aplicación conoce el estado de autenticación.
* No hay redirecciones automáticas.
* El ciclo de bootstrap está bajo nuestro control.

En este laboratorio **no forzamos login todavía**.

---

## ▶️ Reiniciar la aplicación

Desde:

```bash
apps/angular-app/angular-app
npm start
```

---

## 🌐 Abrir la aplicación

Ir a:

```
http://localhost:4200
```

---

## 🔎 Comportamiento esperado

* La aplicación Angular se muestra normalmente.
* No hay redirección automática a Keycloak.
* No aparecen errores en consola.
* La URL no contiene `?code=`.

Esto ocurre porque estamos usando:

```ts
onLoad: 'check-sso'
```

---

# 🧪 Validar el estado de autenticación

Editar temporalmente:

```
src/app/app.component.ts
```

Añadir:

```ts
import { getKeycloakInstance } from './keycloak.service';

ngOnInit() {
  const kc = getKeycloakInstance();
  console.log('Inicializado:', !!kc);
  console.log('Autenticado:', kc?.authenticated);
}
```

---

## 📊 Resultado esperado en consola

Si no hay sesión:

```
Inicializado: true
Autenticado: false
```

Si ya existía sesión activa en el navegador:

```
Inicializado: true
Autenticado: true
```

---

# 🧠 Qué estamos validando realmente

✔ `keycloak.init()` se ejecuta antes de arrancar Angular
✔ El adapter procesa posibles redirecciones
✔ La instancia está disponible globalmente
✔ El estado de autenticación está resuelto

Esto es bootstrapear autenticación manualmente.

---

# 🧩 Qué NO estamos haciendo todavía

* No protegemos rutas.
* No forzamos login.
* No usamos guards.
* No usamos wrapper Angular.

Todo eso viene en el siguiente laboratorio.
