# EnergyMonitor — Documentación final

Documentación final bilingüe (español / inglés) del proyecto **EnergyMonitor – Monitor de Consumo Eléctrico**, SENA ADSO, ficha 3145556 (CIES, Regional Huila). *Final bilingual (Spanish / English) documentation of the EnergyMonitor project.*

EnergyMonitor mide el consumo eléctrico de los electrodomésticos de un hogar con un módulo ESP32 + PZEM-004T V3 y lo muestra en una aplicación web y una aplicación móvil Android, con alertas, recomendaciones de ahorro y notificaciones. En producción en <https://energy-monitor.tech>.

## Arquitectura (27/10/2026)

| Componente | Puerto | Responsabilidad |
|---|---|---|
| en-monitor-api-gateway | 8080 | Única entrada de la API: rutas, CORS, validación del JWT, límite de peticiones; 404 a lo que ningún servicio atiende |
| en-monitor-security-service | 8082 | Cuentas, verificación de correo, términos, sesiones JWT RS256, Google, RBAC y administración |
| en-monitor-device-service | 8084 | Dispositivos, vinculación BLE, credenciales, autenticación del broker MQTT y conectividad |
| en-monitor-measurement-service | 8085 | Telemetría MQTT, niveles, energía por hora, consumo e historial |
| en-monitor-home-service | 8086 | Hogares, miembros (OWNER/MEMBER), código de acceso, favoritos y umbrales |
| en-monitor-notification-service | 8087 | Correo, Web Push, push móvil, actividad y avisos de administración |
| en-monitor-alert-service | 8088 | Alertas de umbral, límite, conectividad y dispositivo vinculado |
| en-monitor-recommendation-service | 8089 | Recomendaciones de ahorro evaluadas cada hora |
| Web (React + Vite) / Móvil (Expo SDK 57) | — | Clientes; la web incluye la landing page y la administración |
| MySQL 8.4 · RabbitMQ 4.1 · Mosquitto 2 | 3306 · 5672 · 8883 | Esquema compartido `energy_monitor` (cada servicio dueño de sus tablas), eventos con outbox transaccional y broker MQTT con TLS |

El antiguo backend modular `energy-monitor-backend` está retirado. Los enlaces a todos los repositorios están en [`15-enlaces-repositorios-github/enlaces.md`](15-enlaces-repositorios-github/enlaces.md).

## Contenido

| Carpeta | Contenido |
|---|---|
| `01-documento-general-proyecto` | Documento general del proyecto |
| `02-especificacion-requisitos` | Especificación de requisitos (RF / RNF) |
| `03-analisis-software` | Mapas de navegación web y móvil, mockups HTML/CSS y recursos gráficos |
| `04-propuesta-tecnica` | Propuesta técnica |
| `05-diseno-software` | Modelo arquitectónico, de datos y de clases, patrones e interfaces (fuentes PlantUML en `assets/diagramas`) |
| `06-base-datos` | [Diccionario de datos](06-base-datos/diccionario-datos/ES/diccionario-datos.md) ([EN](06-base-datos/diccionario-datos/EN/data-dictionary.md)), [script SQL](06-base-datos/script-DB/energy_monitor.sql), [MER](06-base-datos/modelo-ER/MER.wsd) y [diagrama de clases](06-base-datos/modelo-ER/Diagrama_clases.wsd) |
| `07-pruebas` | Plan de pruebas y evidencias |
| `08-calidad-mejoras` | Calidad, deuda técnica y mejoras |
| `09-implementacion` | Manual de implementación y despliegue |
| `10-manual-tecnico` | Manual técnico |
| `11-manual-instalacion` | Manual de instalación |
| `12-manual-usuario` | Manual de usuario único para web y móvil ([ES](12-manual-usuario/ES/MD/Manual_usuario_EnergyMonitor.md), [EN](12-manual-usuario/EN/MD/User_Manual_EnergyMonitor_EN.md)) y sus capturas |
| `13-articulo-proyecto` | Artículo del proyecto |
| `14-anexos` | Anexos |
| `15-enlaces-repositorios-github` | Enlaces a los repositorios, pipelines y commits revisados |
| `16-manual-administrador` | Manual de administrador |

## Equipo

Juan Esteban Cortés Parra y Miguel Ángel García Artunduaga.
