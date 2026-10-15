# Mapas de navegación

**Proyecto:** Monitor de Consumo Eléctrico — EnergyMonitor
**Ficha:** 3145556 — Análisis y Desarrollo de Software (ADSO), SENA CIES Regional Huila
**Versión:** 1.0

---

## 1. Contenido

Estos mapas son la **versión gráfica** del recorrido de las dos aplicaciones. Cada aplicación se documenta con tres diagramas más el detalle de los puntos de control.

| **Documento** | **Mapa 1** | **Mapa 2** | **Mapa 3** |
|---|---|---|---|
| [`mapa-navegacion-web.md`](mapa-navegacion-web.md) | Flujo general: entrada, zona pública, guarda y destinos del menú lateral | Panel de hogares, estados y las seis secciones del detalle | Favoritos, notificaciones, ajustes y el modal de cuenta |
| [`mapa-navegacion-movil.md`](mapa-navegacion-movil.md) | Flujo general: apertura, restauración, guarda y destinos del menú lateral | Panel principal, estados y las seis secciones del detalle | Favoritos, notificaciones, ajustes y la pantalla de cuenta |

### Documentos complementarios

Estos mapas se apoyan en dos documentos analíticos más completos, querels ya estaban escritos:

| **Documento** | **Aporta** |
|---|---|
| [`../navigation-map.md`](../navigation-map.md) | Árbol de rutas, matriz de acceso por rol, cuatro flujos narrados y ocho puntos débiles del recorrido |
| [`../navigation-map-mobile.md`](../navigation-map-mobile.md) | Equivalente para la app nativa, con el detalle del drawer y de la barra inferior |

> Las dos carpetas se complementan: los documentos de la raíz describen **por qué** se navega como se navega; los de esta carpeta muestran **cómo se ve** ese recorrido.

**Se dividió en tres mapas por una razón práctica:** un único diagrama con todas las pantallas resultaba ilegible, porque las aristas entre zonas obligaban al motor de diagramas a reordenar las cajas y el resultado se leía de abajo arriba.

### Imágenes renderizadas

Cada diagrama incluye además su versión en PNG, lista para insertar en el documento:

| **Archivo** | **Diagrama** |
|---|---|
| `mapa-navegacion-web-1.png` | Flujo general de la web |
| `mapa-navegacion-web-2.png` | Panel de hogares y detalle del hogar |
| `mapa-navegacion-web-3.png` | Pantallas auxiliares de la web |
| `mapa-navegacion-movil-1.png` | Flujo general del móvil |
| `mapa-navegacion-movil-2.png` | Panel principal y detalle del hogar |
| `mapa-navegacion-movil-3.png` | Pantallas auxiliares del móvil |

## 2. Origen de la información

Estos mapas se construyeron **leyendo el código fuente**, no los mockups.

| **Dato del mapa** | **Archivo del proyecto del que se obtuvo** |
|---|---|
| Rutas web y sus protecciones | `Energy-Monitor-application/src/routes/AppRoutes.jsx` |
| Menú lateral web | `Energy-Monitor-application/src/components/layout/MainLayout/Sidebar/Sidebar.jsx` |
| Acordeón de hogares | `Energy-Monitor-application/src/components/layout/MainLayout/Sidebar/NavHomes.jsx` |
| Pestañas del detalle del hogar | `Energy-Monitor-application/src/features/DetailHome/Consumption/Consumption.jsx` |
| Rutas móviles | `Energy-Monitor-Mobile/src/app/` |
| Guarda de autenticación móvil | `Energy-Monitor-Mobile/src/components/layout/AuthGate/AuthGate.tsx` |
| Menú lateral móvil | `Energy-Monitor-Mobile/src/components/layout/Sidebar/Sidebar.tsx` |
| Barra inferior del detalle | `Energy-Monitor-Mobile/src/components/layout/BottomTabBar/BottomTabBar.tsx` |
| Secciones del detalle móvil | `Energy-Monitor-Mobile/src/features/homes/screens/HomeDetailScreen/HomeDetailScreen.tsx` |

> **Los mockups no son el flujo real.** Los mockups y wireframes describen el aspecto visual de cada pantalla; el flujo de navegación se determina por las rutas declaradas y por los menús implementados. Por eso un mapa de navegación construido a partir de un mockup puede mostrar pantallas que el sistema nunca monta o transiciones que nunca ocurren.

## 3. Formato

Los mapas están escritos en **Mermaid**, embebido dentro de los documentos Markdown.

| **Ventaja** | **Detalle** |
|---|---|
| Se ve sin instalar nada | GitHub, GitLab, VS Code (con la extensión Mermaid), Notion y Obsidian los renderizan automáticamente |
| Es texto | Se versiona con Git, se puede revisar en los diffs y editar sin herramientas |
| Se puede exportar | Con la extensión Mermaid de VS Code se exporta a PNG, SVG o PDF con un clic |

### Cómo exportarlos a imagen para el documento

| **Opción** | **Pasos** |
|---|---|
| **Extensión de VS Code** | Instalar *Markdown Preview Mermaid Support* → abrir el archivo → botón de exportar a PNG |
| **Mermaid Live Editor** | Pegar el contenido del bloque en <https://mermaid.live> → botón **Actions → PNG** |
| **PlantUML en línea** | No aplica: Mermaid no es PlantUML |

### Si necesitas el código en otro formato

| **Formato** | **Cómo obtenerlo** |
|---|---|
| `.mmd` aislado | Copiar el contenido del bloque ```mermaid a un archivo con extensión `.mmd` |
| draw.io | Exportar el PNG desde Mermaid Live y luego usar *Insert → Image* sobre un lienzo de draw.io |
| PlantUML | Requiere reescribir el diagrama; avisa y lo convierto |

## 4. Cómo leer los mapas

| **Símbolo** | **Significado** |
|---|---|
| Nodo azul redondeado | Punto de entrada o decisión |
| Nodo verde | Módulo o área principal de la aplicación |
| Nodo blanco | Pantalla, sección o acción concreta |
| Nodo ámbar | Guarda de navegación o decisión de estado |
| Nodo rojo | Fin de flujo: cierre de sesión o error de acceso |
| Flecha sólida | Navegación directa entre pantallas |
| Flecha punteada | Reintento o acción bajo demanda |

## 5. Diferencias entre la navegación web y la móvil

| **Aspecto** | **Web** | **Móvil** |
|---|---|---|
| Página de bienvenida | Sí | No: abre en el inicio de sesión |
| Recuperación de contraseña | 2 rutas | 3 rutas |
| Pantalla de cuenta | Modal sobre la pantalla actual | Pantalla propia |
| Menú lateral | Fijo o plegable | Capa deslizable animada |
| Secciones del detalle | Pestañas superiores | Barra inferior con iconos |
| Total de rutas | 14 | 13 |

## 6. Observaciones

| **#** | **Observación** | **Detalle** |
|---|---|---|
| 1 | La ruta `/homes` del móvil no tiene acceso desde ningún menú | Es una pantalla auxiliar declarada en el enrutador pero sin enlace en la interfaz |
| 2 | La opción «Agregar cuenta» aparece en ambos menús de perfil sin funcionar | Está declarada en la interfaz pero no implementada |
| 3 | La web no tiene ruta de reserva | Una dirección desconocida deja la pantalla en blanco |
| 4 | El móvil no tiene ruta de reserva | Expo Router muestra su propia pantalla de «pantalla no encontrada» |
| 5 | La pantalla de cuenta no está protegida por `ProtectedRoute` en la web | La ruta está declarada fuera del bloque protegido; el componente es un modal que no exige prop de cierre |

## 7. Artefactos relacionados

| **Artefacto** | **Ubicación** |
|---|---|
| Mockups en HTML (web y móvil) | `03-analisis-software/mockup/layout/web/` y `03-analisis-software/mockup/layout/mobile/` |
| Mapas de navegación gráficos (Mermaid) | `03-analisis-software/ES/navegacion/mapa-navegacion-web.md` y `03-analisis-software/ES/navegacion/mapa-navegacion-movil.md` |
| Diseño de interfaces gráficas | `05-diseno-software/ES/05.5-interfaces-graficas.md` |
| Diseño de la arquitectura de presentación | `05-diseno-software/ES/05.1-modelo-arquitectonico.md`, sección 6 |
| Versión en inglés | `03-analisis-software/EN/navigation/` |