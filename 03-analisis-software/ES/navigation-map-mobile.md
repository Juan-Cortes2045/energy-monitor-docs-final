# Mapa de navegación — Móvil

> Cómo se recorre EnergyMonitor en la app nativa (Expo / React Native, `Energy-Monitor-Mobile`):
> qué pantallas existen, cómo se conectan y qué reglas gobiernan el paso de una a otra.
>
> **Fuente de verdad:** `src/app/` (expo-router) y `src/components/layout/`.
> **Web:** la aplicación web es un proyecto aparte con su propio árbol —
> ver [`navigation-map.md`](./navigation-map.md).
> **Mockups:** [`mockup/layout/mobile/`](../mockup/layout/mobile/) — 27 pantallas, en 7 módulos.

---

## 1. Árbol de pantallas

```
/                                   → Iniciar sesión

├── PÚBLICAS
│   ├── /register                   Registro
│   ├── /verify-account             Verificar cuenta
│   ├── /recover-password           Pedir recuperación
│   ├── /verify-recover-password    Verificar código
│   └── /new-password               Contraseña nueva
│
└── PRIVADAS  (requieren sesión)
    ├── /dashboard                  Panel: lista de hogares
    ├── /home/[id]                  Detalle de un hogar
    │   └── Consumo · Historial · Usuarios · Dispositivos · Umbrales · Hogar
    │       (6 pestañas en barra inferior, NO son rutas)
    ├── /favorites                  Hogares marcados
    ├── /notifications              Alertas y recomendaciones
    ├── /settings                   Ajustes
    ├── /account                    Mi cuenta
    └── /homes                      Pantalla temporal, sin destino
```

**13 rutas de pantalla** más la raíz del enrutador. A diferencia de la web, **no existe landing**: la
app arranca directamente en el login.

### Pantallas sin URL propia

| Pantalla | Dónde vive | Cómo se llega |
|----------|-----------|--------------|
| Las 6 pestañas del detalle | `/home/[id]` | Barra inferior de iconos |
| Editar nombre / apellido / correo | `/account` | Botón de cada campo |
| Cambiar contraseña · Eliminar cuenta | `/account` | Botones al pie de la pantalla |
| Crear hogar · Unirse a un hogar | `/dashboard` | Acción de la cabecera, o el estado vacío |
| Vincular dispositivo | `/home/[id]` | Botón de la pestaña de dispositivos |
| Selector de idioma · temas | `/settings` | Se aplican en el sitio, sin salir de la pantalla |

---

## 2. Puntos de entrada y de salida

| | Desde dónde | Hacia dónde |
|---|---|---|
| **Entrada** | Apertura de la app | `/` (login) |
| **Salida por sesión** | Drawer ▸ avatar ▸ "Cerrar sesión" | `/` |
| **Salida por sesión vencida** | En cualquier momento | `/` |
| **Salida por cuenta eliminada** | Confirmación de eliminación | `/` |
| **Volver atrás** | Detalle de hogar ▸ flecha | `/dashboard` |
| **Salida por error** | `/home/[id]` con id inexistente | Pantalla propia con "Volver al panel" |
| **Sin salida** | `/homes` | Ruta huérfana: nada navega a ella |

---

## 3. Contenedores de navegación

### Drawer — disponible en 5 pantallas

Presente en `/dashboard`, `/favorites`, `/notifications`, `/settings` y `/account`. Se abre con el
botón de menú que flota en la esquina superior izquierda.

```
☰   EnergyMonitor
    ┌────────────────────────────────┐
    │ 🏠 Inicio              ──────► /dashboard
    │ ☁ Hogares           ▾
    │     · Casa Principal  ──────► /home/1
    │     · Apartamento 502 ──────► /home/2
    │ ♥ Favoritos           ──────► /favorites
    │ 🔔 Notificaciones  ④  ──────► /notifications
    │ ⚙ Ajustes             ──────► /settings
    └────────────────────────────────┘
    (avatar) Camila Restrepo   ⋯
                        ┌───────────────────┐
                        │ Mi cuenta    ──────► /account
                        │ Añadir cuenta ────► (sin destino)
                        │ ─────────────────  │
                        │ Cerrar sesión ────► /
                        └───────────────────┘
```

- El drawer **no** aparece en el login ni en el detalle de hogar.
- Se cierra pulsando el fondo oscurecido. No hay botón de cerrar, ni gesto de barrido, ni tecla
  `Esc`.
- El número junto a "Notificaciones" son los avisos pendientes sin resolver.
- El grupo "Hogares" es el único desplegable; con la lista vacía muestra "Sin hogares".

### Barra inferior — solo en el detalle de hogar

```
┌──────┬──────┬──────┬──────────┬─────────┬──────┐
│  ⚡   │  🕐   │  👥   │   ⌁      │  ≡      │  ⌂   │
│Consu.│Hist. │Usuar.│Disposit.│Umbral. │Hogar │
└──────┴──────┴──────┴──────────┴─────────┴──────┘
```

Seis destinos, **solo iconos y sin etiqueta**. Solo se ve en `/home/[id]`; en el resto de pantallas la
navegación es exclusivamente por el drawer.

Al abrir un hogar se muestra **siempre "Consumo"**.

### Flecha de volver

Únicamente en el detalle de hogar. Regresa al panel si hay historial y salta a `/dashboard` si se
entró con un enlace directo. **Ninguna otra pantalla tiene botón de volver**: el resto depende del
gesto del sistema.

---

## 4. Reglas de navegación

| Regla | Efecto |
|-------|--------|
| **Sesión obligatoria** | Las 6 rutas privadas devuelven al login si no hay sesión. El login cierra esa comprobación antes de pintar, así que nunca se ve un parpadeo |
| **Sesión vencida a mitad de uso** | Cualquier fallo de sesión devuelve al login desde donde esté el usuario, no solo al abrir la app |
| **Sin roles por ruta** | Responsable y miembro comparten las mismas URLs. Lo que cambia es qué controles se pueden usar dentro |
| **Sin enlace a una pestaña** | Las 6 pestañas son estado interno: no hay URL directa ni se conserva la pestaña al volver atrás |
| **Sin enlace a un estado** | Las 3 pestañas de notificaciones y los 4 bloques de ajustes tampoco tienen dirección propia |
| **Sin página de error general** | La única pantalla de error es la del detalle de hogar con identificador desconocido |
| **Ajustes en el sitio** | Cambiar idioma o tema no navega: se aplica y la pantalla se reconstruye conservando la posición |
| **Reemplazo en los puntos de control** | Login, cierre de sesión y sesión vencida reemplazan la pantalla en lugar de apilarse, para que el botón atrás no devuelva a una pantalla ya cerrada |

---

## 5. Matriz de acceso

| Pantalla | Sin sesión | Miembro | Responsable |
|----------|:---------:|:-------:|:-----------:|
| `/` (login) | ✅ | ✅ | ✅ |
| `/register`, `/verify-account` | ✅ | ✅ | ✅ |
| `/recover-password`, `/verify-recover-password`, `/new-password` | ✅ | ✅ | ✅ |
| `/dashboard`, `/favorites`, `/notifications`, `/settings`, `/account` | ↩️ al login | ✅ | ✅ |
| `/home/[id]` → Consumo, Historial | ↩️ al login | ✅ | ✅ |
| `/home/[id]` → Usuarios | ↩️ al login | solo lectura | quita miembros |
| `/home/[id]` → Dispositivos | ↩️ al login | solo lectura | vincula y elimina |
| `/home/[id]` → Umbrales | ↩️ al login | solo lectura, sin guardar | edita y guarda |
| `/home/[id]` → Hogar | ↩️ al login | sin código de acceso | código y salida del hogar |
| `/homes` | ↩️ al login | ✅ | ✅ |

---

## 6. Flujos principales

### Flujo 1 — Entrar y crear la cuenta

```
/ ──credenciales──► /dashboard ───────────────────────────────► (sesión abierta)
│
├──► /register ──► /verify-account ──código correcto──► / (aviso "correo verificado")
│                       │
│                       └── sin dirección de correo ──► /register
│
└──► /recover-password ──► /verify-recover-password ──► /new-password
                                                        │ (el código no se
                                                        │  comprueba hasta aquí)
                                                        └── contraseña guardada ──► /
```

Diferencias de recorrido frente a la web:

- **La recuperación se parte en dos pantallas** (`/verify-recover-password` y `/new-password`), donde
  la web las mantiene en una sola ruta.
- **Tras verificar la cuenta se vuelve al login**, no al panel como en la web: el usuario tiene que
  entrar otra vez.

### Flujo 2 — Sesión activa y primer hogar

```
/dashboard
   │
   ├── sin hogares ──► [Crear hogar] ──────────────► aparece en la lista
   │                └► [Unirse a un hogar] ─────────► aparece en la lista
   │
   └── hogares ──► tocar una tarjeta ──► /home/[id] ──► Consumo
                                                    │
        ┌───────────────────────────────────────────┤
        ▼           ▼          ▼         ▼         ▼        ▼
    Historial   Usuarios  Dispositivos Umbrales  Hogar   Salir
```

**Al abrir un hogar se muestra siempre "Consumo"**, y ninguna pestaña queda registrada: volver atrás
desde Historial devuelve al panel, no a Consumo.

### Flujo 3 — Alertas y recomendaciones

```
Consumo de cualquier hogar
   │
   ├── el consumo cruza un umbral o un dispositivo se desconecta ──► alerta ──► /notifications
   └── el modelo detecta un patrón de consumo ────────────────────► recomendación ──► /notifications

/settings ▸ Notificaciones ──► decide por qué canal se recibe cada tipo de aviso
```

En la app **no hay avisos flotantes**: el centro de notificaciones solo se abre si el usuario navega
hasta él. Es la diferencia de navegación más grande respecto a la web.

### Flujo 4 — Cuenta

```
/account
   │
   ├── editar nombre / apellido / correo ──► formulario ──► Guardar
   ├── Contraseña ──► pide la actual para cambiarla
   ├── eliminar cuenta ──► pide la contraseña ──► confirma ──► /
   └── cerrar sesión ──► drawer ▸ avatar ──► /
```

---

## 7. Puntos débiles del recorrido

| # | Hallazgo | Consecuencia para quien navega |
|---|----------|-------------------------------|
| 1 | No hay botón "atrás" en ninguna pantalla salvo el detalle de hogar | Quien no use el gesto del sistema queda atrapado en un nivel |
| 2 | Las 6 pestañas no tienen URL | No se puede compartir un enlace a una pestaña ni conservarla al recargar |
| 3 | `/homes` existe pero nada lleva a ella | Pantalla muerta en el enrutador, con su propio texto "(placeholder)" |
| 4 | Tras verificar la cuenta se vuelve al login, no al panel | El usuario pierde el avance y tiene que autenticarse otra vez |
| 5 | "Añadir cuenta" no lleva a ninguna parte | Entrada visible en el menú del avatar que no hace nada |
| 6 | El aviso de "correo verificado" nunca se muestra | El login solo sabe leer el aviso de contraseña actualizada, así que el de verificación se pierde en el paso |
| 7 | El drawer no se cierra con gesto ni con `Esc` | En escritorio o con teclado hay que apuntar al fondo oscurecido |
| 8 | El detalle de hogar no da acceso al drawer | Desde las 6 pestañas no se puede saltar a Favoritos, Ajustes o a otro hogar sin volver antes al panel |
| 9 | "Inicio" en el drawer lleva al panel, no a una portada | No existe un equivalente de la landing web |

---

## 8. Web ↔ Móvil: qué cambia al navegar

| | Web | Móvil |
|---|---|---|
| Pantalla de entrada | Landing pública | Login directo, sin landing |
| Recoveración | 1 ruta, 2 pasos | 2 rutas, 1 paso cada una |
| Tras verificar la cuenta | Va al panel | Vuelve al login |
| Menú | Sidebar siempre visible | Drawer que cubre 3/4 de la pantalla, en 5 de 13 pantallas |
| Navegación dentro de un hogar | 6 pestañas con etiqueta arriba | 6 iconos abajo, sin etiqueta, y sin drawer disponible |
| Pestañas de notificaciones | 3, con URL propia cada una | 3, sin URL |
| Avisos de alerta | Notificación flotante + insignia | Solo insignia; el aviso hay que buscarlo |
| Ajustes | Idioma, temas y ayuda en la web | además selector de idioma y de tema, en 4 tarjetas |
| Cierre de sesión | Navega a la landing | Navega al login |
| Botón atrás | Solo el del navegador | El del sistema, más una flecha en el detalle de hogar |

---

## Correlaciones

| Este mapa se alimenta de… | Y alimenta a… |
|--------------------------|---------------|
| `src/app/` (expo-router) | Las 13 rutas de pantalla y la puerta de sesión (`AuthGate`) |
| `src/components/layout/Sidebar/` | El drawer y sus destinos |
| `src/components/layout/BottomTabBar/` | La barra de 6 iconos del detalle de hogar |
| `mockup/layout/mobile/**/README.md` | El diseño de cada pantalla y sus estados alternos |
| [`navigation-map.md`](./navigation-map.md) | El árbol equivalente de la aplicación web |
| — | Las decisiones de arquitectura que hacen que el login sea la raíz y el drawer el único menú |