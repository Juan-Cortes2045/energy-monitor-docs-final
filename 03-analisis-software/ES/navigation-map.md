# Mapa de navegación — Web

> Cómo se recorre EnergyMonitor en la web: qué pantallas existen, cómo se conectan entre sí y qué
> reglas gobiernan el paso de una a otra.
>
> **Fuente de verdad:** `src/routes/AppRoutes.jsx`, contrastado con cada `navigate(...)` y cada
> enlace de navegación declarado en `src/`.
> **Móvil:** la app nativa es un proyecto aparte con su propio árbol de rutas —
> ver [`navigation-map-mobile.md`](./navigation-map-mobile.md).

---

## 1. Árbol de pantallas

```
/                                   → redirige a /home
│
├── PÚBLICAS  (navbar superior)
│   ├── /home                       Landing
│   ├── /about                      Sobre nosotros
│   └── /support                    Soporte
│
├── PÚBLICAS  (sin navbar)
│   ├── /login                      Iniciar sesión
│   ├── /register                   Registro
│   ├── /VerifyAccount              Verificar cuenta
│   ├── /recover-password           Pedir recuperación
│   └── /verifyRecoverPassword      Verificar código → nueva contraseña  (2 pasos, 1 ruta)
│
├── /account                        Mi cuenta
│
└── PROTEGIDAS  (requieren sesión · sidebar lateral)
    ├── /dashboard                  Panel: lista de hogares
    ├── /homes/:homeId              Detalle de un hogar
    │   └── Consumo · Historial · Usuarios · Dispositivos · Umbrales · Hogar
    │       (6 pestañas, NO son rutas: una sola URL)
    ├── /favorites                  Hogares marcados
    ├── /notifications              Alertas y recomendaciones
    └── /settings                   Ajustes
```

**15 rutas declaradas.** Ninguna otra existe.

### Pantallas sin URL propia

| Pantalla | Dónde vive | Cómo se llega |
|----------|-----------|--------------|
| Pestañas del detalle de hogar | `/homes/:homeId` | Cambio de pestaña dentro de la pantalla |
| 2.º paso de la recuperación (nueva contraseña) | `/verifyRecoverPassword` | Continuar tras validar el código |
| Modal de términos legales | `/register` | Enlaces "Términos de uso" / "Política de privacidad" |
| Modal de "Mi cuenta" | `/dashboard`, `/favorites`, … | Menú del avatar del sidebar |
| Aviso de hogar inexistente | `/homes/:homeId` | Se abre sola si el identificador no existe |

---

## 2. Puntos de entrada y de salida

| | Desde dónde | Hacia dónde |
|---|---|---|
| **Entrada** | `/` | `/home` |
| **Salida por sesión** | Menú del avatar → "Cerrar sesión" | `/home` |
| **Salida por cuenta eliminada** | Confirmación de eliminación | `/login` |
| **Salida por error** | `/homes/:homeId` con id inexistente | Aviso propio con "Volver al panel" |
| **Sin salida** | Cualquier URL no declarada | No hay pantalla de error: se renderiza el árbol vacío |

---

## 3. Contenedores de navegación

### Navbar — zona pública

```
[logo]   Inicio · Sobre nosotros · Soporte        [Regístrate] [Iniciar sesión]

Inicio ──────────► /
Sobre nosotros ──► /about
Soporte ─────────► /support
Regístrate ──────► /register
Iniciar sesión ──► /login
```

"Inicio" apunta a `/`, que a su vez redirige a `/home`.

### Sidebar — zona con sesión

```
☰   EnergyMonitor
    ┌────────────────────────────────┐
    │ 🏠 Inicio              ──────► /dashboard
    │ ☁ Hogares           ▾
    │     · Casa Principal  ──────► /homes/1
    │     · Apartamento 502 ──────► /homes/2
    │ ♥ Favoritos           ──────► /favorites
    │ 🔔 Notificaciones  ④  ──────► /notifications
    │ ⚙ Ajustes             ──────► /settings
    └────────────────────────────────┘
    (avatar) Camila Restrepo   ⋯
                        ┌───────────────────┐
                        │ Mi cuenta    ──────► overlay, o /account
                        │ Añadir cuenta ────► (sin destino)
                        │ ─────────────────  │
                        │ Cerrar sesión ────► /home
                        └───────────────────┘
```

- "Hogares" es el **único** grupo desplegable: cada entrada lleva a un hogar distinto, y con la lista
  vacía muestra "Sin hogares".
- El número junto a "Notificaciones" son los pendientes sin resolver: alertas activas + recomendaciones
  sin leer.
- "Mi cuenta" abre una pantalla sobre la actual; "/account" es su equivalente como URL directa.

### Botón atrás

No hay control de "volver" dentro de la aplicación. Se depende del botón del navegador, y por eso
las rutas usan reemplazo en lugar de apilado en los puntos de entrada y de salida.

---

## 4. Reglas de navegación

| Regla | Efecto |
|-------|--------|
| **Sesión obligatoria** | `/dashboard`, `/homes/:homeId`, `/favorites`, `/notifications` y `/settings` devuelven a `/login` si no hay sesión |
| **Sin roles por ruta** | Responsable y miembro comparten las mismas URLs. Lo que cambia es qué se puede hacer dentro de cada pantalla, no a dónde se puede entrar |
| **Redirección de la raíz** | `/` → `/home`, para que el botón atrás no devuelva a la raíz |
| **Sin página de error** | No hay ruta `*`. Una URL desconocida no muestra nada |
| **Un solo hogar por URL** | `/homes/:homeId`; no existe la vista "todos los hogares" como ruta propia |
| **Pestañas sin URL** | Las 6 pestañas del detalle, las 3 de notificaciones y los 4 bloques de ajustes son estado interno: no hay enlace directo ni se conservan al recargar |
| **Sesión vencida** | Se pierde al navegar, no al recargar: cerrar sesión es inmediato en el navegador, la revocación en el servidor va después |

---

## 5. Matriz de acceso

| Pantalla | Sin sesión | Miembro | Responsable |
|----------|:---------:|:-------:|:-----------:|
| `/home`, `/about`, `/support` | ✅ | ✅ | ✅ |
| `/login`, `/register`, `/VerifyAccount`, `/recover-password`, `/verifyRecoverPassword` | ✅ | ✅ | ✅ |
| `/account` | ⚠️ abre vacío | ✅ | ✅ |
| `/dashboard`, `/favorites`, `/notifications`, `/settings` | ↩️ al login | ✅ | ✅ |
| Detalle → Consumo, Historial | ↩️ al login | ✅ | ✅ |
| Detalle → Usuarios | ↩️ al login | solo lectura | invita y quita |
| Detalle → Dispositivos | ↩️ al login | solo lectura | vincula y elimina |
| Detalle → Umbrales | ↩️ al login | solo lectura | edita y guarda |
| Detalle → Hogar | ↩️ al login | se sale, sin código | código de acceso, elimina |

---

## 6. Flujos principales

### Flujo 1 — Entrar y crear la cuenta

```
/home ──► /login ──credenciales──► /dashboard
            │                       ▲
            │                       └── tras /VerifyAccount
            │
            ├──► /register ──► /VerifyAccount ──código correcto──► /dashboard
            │                     │
            │                     └── sin dirección de correo ──► /register
            │
            └──► /recover-password ──► /verifyRecoverPassword
                                          paso 1: código
                                          paso 2: contraseña nueva ──► /login
```

Dos decisiones de recorrido que conviene conocer:

- **La recuperación avanza siempre.** Enviar el correo lleva a la pantalla siguiente aunque el
  endereço no exista; solo se detiene ahí el caso de "demasiados intentos". Evita revelar qué correos
  están registrados.
- **El código de recuperación no se valida en el paso 1**, solo su formato. El paso 2 es el que
  pregunta al servidor; si el código falla, se vuelve al paso 1 con el aviso.

### Flujo 2 — Sesión activa y primer hogar

```
/dashboard
   │
   ├── sin hogares ──► [Crear hogar] ──► ¿Quién es el responsable? ──► aparece en la lista
   │                └► [Unirme a un hogar] ──► código de 8 caracteres ──► aparece en la lista
   │
   └── hogares ──► clic en una tarjeta ──► /homes/:homeId ──► Consumo
                                                            │
        ┌───────────────────────────────────────────────────┤
        ▼                ▼            ▼          ▼         ▼        ▼
    Historial        Usuarios    Dispositivos  Umbrales   Hogar   (salir)
```

**Al llegar a un hogar, siempre se abre "Consumo".** Ninguna pestaña queda Urlada, así que recargar
la página devuelve siempre a la primera.

### Flujo 3 — Alertas y recomendaciones

```
Consumo de cualquier hogar
   │
   ├── el consumo cruza un umbral o un dispositivo se desconecta ──► alerta ──► /notifications
   └── el modelo detecta un patrón de consumo ────────────────────► recomendación ──► /notifications
                                                          (en ambos casos, un aviso flotante
                                                           lleva a /notifications)

/settings ▸ Notificaciones ──► decide por qué canal se recibe cada tipo de aviso
```

Es el único flujo que **empuja** al usuario: el resto de la navegación la initiate él.

### Flujo 4 — Cuenta

```
Sidebar ▸ avatar ▸ Mi cuenta
   │
   ├── editar nombre / apellido ──► campo editable ──► Guardar
   ├── avatar ──► agregar · ver · quitar foto
   ├── contraseña ──► pide la actual para cambiarla
   ├── eliminar cuenta ──► pide la contraseña ──► confirma ──► /login
   └── cerrar sesión ──► /home
```

---

## 7. Puntos débiles del recorrido

| # | Hallazgo | Consecuencia para quien navega |
|---|----------|-------------------------------|
| 1 | `/account` está fuera de la protección de sesión | Con la sesión vencida, el enlace abre una pantalla de cuenta vacía en lugar de devolver al login |
| 2 | No existe ruta de error | Cualquier URL mal escrita o caducada muestra una página vacía, sin salida |
| 3 | No hay enlace directo a una pestaña | No se puede compartir un enlace a "Umbrales" ni volver a él tras recargar |
| 4 | **"Inicio" significa dos cosas distintas** | El navbar lleva "Inicio" a `/` (la landing), pero el sidebar lleva "Inicio" a `/dashboard` (el panel de hogares). El mismo texto, dos destinos: quien salga de una pantalla buscando el "inicio" del navbar desde la zona privada aterriza en el panel |
| 5 | "Añadir cuenta" no lleva a ninguna parte | Entrada visible en el menú del avatar que no hace nada |
| 6 | El modal de cuenta se monta por dos vías distintas | El sidebar lo abre con `createPortal` sobre `document.body` y le pasa `onClose`; la ruta `/account` monta el mismo componente **sin** esa propiedad. En esa segunda vía el botón de cerrar queda sin función, porque el cierre del overlay usa la llamada opcional `onClose?.()` pero el botón lo invoca directamente |
| 7 | Las 3 pestañas de notificaciones y los 4 bloques de ajustes no tienen URL propia | Un usuario que llega por un enlace directo aterriza siempre en el primer bloque |
| 8 | Sin ruta "todos los hogares" | La lista de hogares existe en un drawer desplegable y en la pantalla de favoritos, pero no tiene dirección propia |

---

## Correlaciones

| Este mapa se alimenta de… | Y alimenta a… |
|--------------------------|---------------|
| `02-domain/domain-map.md` | Los contextos que cada pantalla expone |
| `04-requirements/user-stories.md` | Los flujos 1–4 y las historias de usuario que los recorren |
| `07-api/contracts/openapi/` | Los recursos que estas pantallas consumen |
| `mockup/layout/web/**/README.md` | El diseño de cada pantalla y sus estados alternos |
| `responsive-web-notes.md` | Cómo se comporta esta misma web en pantallas estrechas |
| [`navegacion/mapa-navegacion-web.md`](./navegacion/mapa-navegacion-web.md) | La versión gráfica de este mismo mapa, en Mermaid |
| [`navigation-map-mobile.md`](./navigation-map-mobile.md) | El árbol equivalente de la app nativa |
| — | El enrutado del frontend y las decisiones de arquitectura que lo sostienen |

---

## Registro de revisión

Este mapa se contrastó contra el código fuente. Correcciones aplicadas:

| # | Afirmación anterior | Situación real | Origen de la comprobación |
|---|---|---|---|
| 1 | «No hay "Inicio" en el sidebar» | **Incorrecto.** El sidebar sí tiene "Inicio" (`Sidebar.jsx`, `NavItem to="/dashboard"`, etiqueta `sidebar.home` = "Inicio") | `src/components/layout/MainLayout/Sidebar/Sidebar.jsx` |
| 2 | «`/about` y `/support` no montan la barra de navegación» | **Incorrecto.** Ambas pantallas renderizan `<Navbar />` | `src/features/landingPage/pages/AboutI.jsx` y `SupportI.jsx`, línea 7 de cada una |

Ambas afirmaciones se sustituyeron por el hallazgo que sí se sostiene: la **ambigüedad de la etiqueta "Inicio"**, que aparece en el navbar apuntando a `/` y en el sidebar apuntando a `/dashboard` (hallazgo 4), y el **doble montaje del modal de cuenta** (hallazgo 6).

El recuento de rutas (15) y el resto de hallazgos se verificaron y se mantienen sin cambios.