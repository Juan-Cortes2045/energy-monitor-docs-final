# Propuesta Técnica de EnergyMonitor

Juan Esteban Cortés Parra y Miguel Ángel García Artunduaga

Servicio Nacional de Aprendizaje (SENA)

Tecnólogo en Análisis y Desarrollo de Software (ADSO), ficha 3145556

12 de octubre de 2026 (actualizado el 20 de octubre de 2026)

## Control de versiones

| Versión | Fecha      | Descripción del cambio                                                                                          | Responsables                                  |
|---------|------------|-----------------------------------------------------------------------------------------------------------------|-----------------------------------------------|
| 1.0     | 12/10/2026 | Primera versión: pila tecnológica, arquitectura, infraestructura, costos, cronograma, riesgos y discrepancias.   | Cortés Parra, J. E.; García Artunduaga, M. A. |
| 1.1     | 20/10/2026 | Alineación con el sistema desplegado: infraestructura real en producción (VPS Hostinger KVM 2 por un mes, dominio gratuito, Nginx, Let's Encrypt), integración y despliegue continuos con GitHub Actions, costos recalculados, administración global y nuevos eventos, estado por componente, riesgos y discrepancias actualizados. | Cortés Parra, J. E.; García Artunduaga, M. A. |

## Resumen

Este documento presenta la propuesta técnica de EnergyMonitor, un sistema que permite a los hogares conocer el consumo eléctrico de cada electrodoméstico, identificar los periodos de mayor demanda, recibir alertas y obtener recomendaciones de ahorro. Describe la pila tecnológica elegida y su justificación, la arquitectura de software adoptada (un monolito modular con eventos internos), la infraestructura de alojamiento y los costos del proyecto. La solución integra un dispositivo ESP32 con sensor PZEM-004T V3, un servicio central en Java 21 con Spring Boot 4.1.1, una base de datos MySQL 8.4, un intermediario de mensajes Mosquitto con TLS, una aplicación web en React 19 y una aplicación móvil Android con Expo SDK 57. El sistema está desplegado en `https://energy-monitor.tech`, en un VPS Hostinger KVM 2 con Docker Compose, y se actualiza mediante integración y despliegue continuos con GitHub Actions. El costo total estimado es de COP 57.021.997, de los cuales COP 8.073.997 corresponden a hardware, COP 48.000 a infraestructura (un mes de servidor; el dominio se obtuvo sin costo) y COP 48.900.000 a recurso humano. El documento incluye además el cronograma, el estado por componente, los riesgos técnicos pendientes y las diferencias con el documento general del proyecto.

*Palabras clave:* propuesta técnica, arquitectura, monolito modular, costos, infraestructura, integración continua, Internet de las cosas

## Introducción

EnergyMonitor es un proyecto formativo del programa Tecnólogo en Análisis y Desarrollo de Software (ADSO) del SENA, desarrollado por dos aprendices con fecha de entrega el 28 de octubre de 2026. Su finalidad es que los hogares vean cuánta energía consume cada electrodoméstico, detecten los momentos de mayor demanda, reciban alertas por consumo inusual y obtengan recomendaciones de ahorro basadas en sus propias mediciones.

Esta propuesta responde a la pregunta de si el proyecto es técnica y económicamente viable con los recursos del equipo. Se organiza en tres bloques: la justificación de la pila tecnológica, la arquitectura de software y la infraestructura con sus costos. Después presenta el cronograma y el estado por componente, los riesgos técnicos y las diferencias con el documento general del proyecto. Los requisitos funcionales y no funcionales se detallan en la especificación de requisitos (carpeta `02-especificacion-requisitos`); los objetivos, el alcance y el equipo, en el documento general del proyecto (carpeta `01-documento-general-proyecto`); y el procedimiento de despliegue, en el manual de implementación y despliegue (carpeta `09-implementacion`).

Los datos que el equipo aún no ha definido se marcan con `[POR CONFIRMAR]` y no se suponen.

## Justificación de la Pila Tecnológica

### Visión general de la solución

El flujo de datos recorre cinco piezas. El dispositivo ESP32 lee el sensor PZEM-004T V3 por Modbus RTU a 9600 baudios (GPIO16 como recepción y GPIO17 como transmisión, firmware 1.3.8) y publica por MQTT 3.1.1 con TLS en el puerto 8883 hacia el intermediario Mosquitto. El servicio central en Java recibe las lecturas, las almacena en MySQL, evalúa alertas y recomendaciones y envía notificaciones. La aplicación web y la aplicación móvil Android consultan el servicio mediante una API REST. El dispositivo transmite telemetría cada 60 segundos y su estado cada cinco minutos. La vinculación se realiza por Bluetooth Low Energy desde la web, con un portal de configuración como alternativa. En producción, la web y la API se publican en `https://energy-monitor.tech` y los dispositivos se conectan a `mqtt.energy-monitor.tech:8883`.

### Decisiones por capa

La Tabla 1 resume la tecnología elegida en cada capa y la razón que la respalda. La última columna compara cada elección con alternativas comunes del mercado. Es una comparación de referencia, no el registro de evaluaciones formales hechas durante el proyecto.

**Tabla 1**

*Tecnología Elegida y Justificación por Capa*

| Capa | Tecnología elegida | Justificación | Alternativas de mercado |
|---|---|---|---|
| Servicio central | Java 21, Spring Boot 4.1.1, Spring Modulith, Spring Security, Spring Data JPA | Herramientas de software libre con eficiencia, escalabilidad y facilidad de integración. Spring Modulith permite separar el sistema en módulos con dependencias controladas dentro de una sola aplicación. | Node.js con NestJS o Express: ecosistema amplio y desarrollo rápido, pero sin la modularidad verificable de Spring Modulith. Django (Python) y ASP.NET Core (C#): válidos, con menos experiencia en el equipo. |
| Base de datos | MySQL 8.4 con migraciones Liquibase | Software libre. Liquibase versiona los cambios de la base de datos con reversión (rollback), lo que da trazabilidad. | PostgreSQL: igual de libre y más rico en funciones avanzadas, innecesarias para este volumen. MongoDB: flexible, pero los datos del proyecto son relacionales (hogares, miembros, dispositivos). Firebase: costo variable por uso y dependencia del proveedor. |
| Mensajería con el dispositivo | MQTT 3.1.1 con TLS sobre Eclipse Mosquitto | Protocolo estándar de Internet de las cosas (OASIS, 2014), apto para dispositivos pequeños. Mosquitto es libre y los certificados se obtienen con Let's Encrypt. El cifrado protege las lecturas en tránsito. | HTTP periódico desde el dispositivo: más simple, pero consume más recursos del microcontrolador y no tiene mecanismos de última voluntad. CoAP: menos soporte en brokers y herramientas. Brokers como EMQX o HiveMQ: más capacidad de la necesaria para un solo servidor. |
| Dispositivo | ESP32 con sensor PZEM-004T V3 | Microcontrolador con Wi-Fi y Bluetooth de bajo costo (COP 24.000) y sensor de energía de COP 70.000. Registra tensión, corriente, potencia activa y energía acumulada. | Arduino con módulo Wi-Fi o Raspberry Pi: el primero exige más componentes y la segunda tiene mayor costo y consumo. Sensores como el SCT-013 (pinza de corriente) requieren calibración y circuito propio, mientras que el PZEM-004T entrega los valores ya calculados. |
| Aplicación web | React 19, Vite 8, Zustand, Axios, React Router, React Hook Form, Zod, i18next, Recharts | Software libre. Permite gráficos interactivos, formularios validados e interfaz en varios idiomas. Web Bluetooth habilita la vinculación directa del dispositivo desde el navegador. | Angular o Vue: igual de viables, con menos conocimiento previo en el equipo. Next.js: añade renderizado en servidor, que la aplicación no necesita. Create React App: en desuso frente a Vite. |
| Aplicación móvil | React Native 0.86 con Expo SDK 57, expo-router, Redux Toolkit | Software libre. Reutiliza el conocimiento de React del equipo y atiende solo Android, que es el alcance definido. | Flutter (Dart): buen rendimiento, pero obliga a otro lenguaje y a no reutilizar lo aprendido en la web. Kotlin nativo: máximo control en Android, con mayor costo de desarrollo. Una aplicación web progresiva: menor acceso a funciones del teléfono. |
| Infraestructura | Docker Compose sobre un VPS de Hostinger KVM 2, Nginx con certificados Let's Encrypt e integración y despliegue continuos con GitHub Actions | Despliegue reproducible en un único servidor de bajo costo. El mismo servidor aloja el servicio central, la base de datos, el intermediario y la aplicación web. GitHub Actions ya está integrado con los repositorios del equipo y permite aprobar cada despliegue. | AWS, Google Cloud o Azure: más servicios y escalado automático, con costo variable y mayor complejidad. Plataformas PaaS: muchas solo publican HTTP y no admiten el puerto 8883 de MQTT. Otros VPS (DigitalOcean, Contabo): comparables. Jenkins o GitLab CI: exigen otro servidor o migrar los repositorios. |

### Criterios generales de selección

Las decisiones de la Tabla 1 comparten tres criterios que orientaron la selección:

- Costo de licencias igual a cero: todo el software de la pila es libre, y solo se paga el servidor. El dominio se obtuvo sin costo.
- Un solo servidor: la carga prevista de un proyecto formativo no justifica varios servicios independientes. El sistema puede crecer aumentando los recursos del servidor, sin migrar a microservicios.
- Conocimiento del equipo: la pila usa herramientas que los dos integrantes manejan, lo que reduce el riesgo de entrega dentro del plazo.

### Limitaciones asumidas

Dos decisiones tienen límites conocidos. Web Bluetooth solo funciona en navegadores compatibles, como Chrome, y exige un contexto seguro (en producción lo da HTTPS); en los demás navegadores se usa el portal de configuración Wi-Fi. La aplicación móvil se limita a Android, por lo que no hay versión para iPhone.

## Arquitectura de Software

### Patrón arquitectónico elegido

EnergyMonitor es un monolito modular construido con Spring Modulith. Es una sola aplicación desplegable, dividida internamente en siete módulos con límites claros. No usa microservicios, Kafka ni API Gateway. La Tabla 2 describe cada módulo.

**Tabla 2**

*Módulos del Servicio Central*

| Módulo | Responsabilidad |
|---|---|
| security | Registro, autenticación, sesiones, permisos, verificación de correo, recuperación de contraseña, Google Sign-In, perfil, borrado de cuenta y administración global de cuentas y roles (rol `ADMIN`) |
| home | Hogares, miembros, roles OWNER y MEMBER, favoritos, umbrales, eliminación del hogar y transferencia de propiedad |
| device | Registro, vinculación, edición y desvinculación de dispositivos, credenciales, autorización MQTT y detección de pérdida de conectividad |
| measurement | Mediciones, telemetría y mensajes de estado |
| alert | Alertas de consumo y de conectividad |
| recommendation | Reglas de ahorro |
| notification | Entregas, preferencias, correo electrónico, Web Push y avisos in-app para administradores |

### Reglas de organización

- Los módulos dependen entre sí solo mediante sus paquetes `api`.
- Dentro de cada módulo se aplica arquitectura hexagonal.
- Los cambios en la base de datos se hacen con changesets de Liquibase que incluyen rollback.
- Los identificadores son VARCHAR(10) y el borrado es lógico mediante `deleted_at`.

### Comunicación entre módulos

Los módulos se comunican con eventos internos de Spring, en memoria, sin Kafka. La Tabla 3 los enumera.

**Tabla 3**

*Eventos Internos entre Módulos*

| Evento | Origen | Destino |
|---|---|---|
| MeasurementRecorded | measurement | alert |
| DeviceConnectivityLost / DeviceConnectivityRestored | device | alert |
| AlertRaised | alert | notification |
| RecommendationCreated | recommendation | notification |
| AccountRegistered / AccountAdministered | security | notification |
| AccountDeleted | security | home |
| HomeDeleted | home | device |
| HomeOwnershipTransferred | home | sin consumidor |

El módulo recommendation no escucha eventos de medición: evalúa las recomendaciones cada hora mediante un programador (`RecommendationScheduler`). La pérdida de conectividad de un dispositivo se identifica mediante los registros de estado y los mecanismos de última voluntad y silencio del protocolo MQTT; el módulo device la anuncia con `DeviceConnectivityLost` y su recuperación con `DeviceConnectivityRestored`. El registro de una cuenta y los cambios de estado o de rol que hace un administrador generan avisos in-app para los demás administradores.

### Interfaces

La API es REST bajo `/api/v1` y cubre autenticación, cuentas, hogares, miembros, umbrales, dispositivos, mediciones, estadísticas, alertas, recomendaciones, preferencias de notificación y administración (`/api/v1/admin`: usuarios, roles y avisos, con los permisos `USER.READ`, `USER.MANAGE` y `ROLE.MANAGE`). Incluye Actuator y Swagger; en producción Swagger no pasa por el proxy. Existen además endpoints internos de autenticación y autorización MQTT para Mosquitto, con caché de cinco minutos, que no se exponen a Internet.

### Seguridad

- Tokens JWT firmados con RS256. El access token dura 15 minutos.
- Refresh tokens opacos con estado, rotación y detección de reutilización, almacenados en la base de datos.
- Permisos globales consultados dinámicamente en cada petición, sin roles dentro del JWT.
- La identidad se toma de la claim `sub` mediante CurrentUserResolver. El encabezado X-User-Id se ignora.
- CORS cerrado por defecto, con orígenes definidos por variables de entorno. En producción la web y la API comparten origen.
- Google Sign-In requiere credenciales OAuth configuradas.
- Los dispositivos se autentican ante el intermediario con `device_code` y `api_key`; el servicio central solo guarda el hash SHA-256 de la clave.
- En producción: HTTPS y MQTT con certificados Let's Encrypt, rutas `/internal/**` sin exposición pública y `X-Forwarded-For` reemplazado por la IP real en el proxy.
- La protección de los datos personales de los usuarios toma como referencia la Ley 1581 de 2012 (Congreso de la República de Colombia, 2012).

### Reglas de dominio relevantes

Los roles globales los administra security, mientras que los roles OWNER y MEMBER son por hogar, de modo que un usuario puede pertenecer a varios hogares con roles distintos. El rol global `ADMIN` no da acceso a ningún hogar y un administrador no puede cambiar su propio estado ni sus roles. Los umbrales pertenecen al hogar, con valores predeterminados de 6 kWh diarios y 180 kWh mensuales. Los niveles de potencia son LOW (0 a 500 W), MEDIUM (500 a 1500 W), HIGH (1500 a 3000 W) y CRITICAL (desde 3000 W). El borrado de cuenta es transaccional: valida la contraseña, cierra las sesiones, elimina lógicamente los datos personales, libera el correo y gestiona las membresías.

### Clientes

La aplicación web separa sus funcionalidades por características, gestiona los textos con i18n y concentra las llamadas HTTP en `src/services/`. Se prueba con Vitest y Testing Library. Una cuenta administradora ve solo la experiencia de administración (usuarios, notificaciones y ajustes). La aplicación móvil usa la estructura de expo-router, archivos de estilos por componente y está disponible en español, inglés, francés y portugués. En el emulador de Android se usa 10.0.2.2 en lugar de localhost, y CORS no aplica en React Native.

### Convenciones de desarrollo

El código se escribe en inglés y los entregables académicos en español. Los commits siguen Conventional Commits con scope por módulo y mensaje en inglés imperativo. Las ramas son `feat/*`, `fix/*` o `chore/*`, con integración mediante pull request a `dev` y luego a `main`; el repositorio de infraestructura integra directamente en `main`. Cada pull request debe pasar sus pruebas automáticas en GitHub Actions antes de fusionarse.

## Infraestructura y Costos

### Infraestructura de alojamiento

La Tabla 4 describe la infraestructura en producción.

**Tabla 4**

*Infraestructura de Alojamiento*

| Elemento | Detalle |
|---|---|
| Proveedor | Hostinger |
| Plan | VPS KVM 2: 2 vCPU, 8 GB de RAM y 100 GB NVMe, con Ubuntu 26.04 LTS |
| Contratación | Un mes, por COP 48.000. Con contrato anual el plan cuesta COP 27.900 mensuales |
| Despliegue | Docker Compose desde el repositorio `energy-monitor-infra`, con el servicio central, MySQL 8.4, Mosquitto y la aplicación web |
| Acceso | Nginx en el servidor como único punto de entrada HTTPS; solo se publican los puertos 80, 443 y 8883 |
| Certificados | Let's Encrypt, sin costo y con renovación automática |
| Dominio | `energy-monitor.tech` (web y API) y `mqtt.energy-monitor.tech` (intermediario), obtenido sin costo |
| Respaldos | Copia de MySQL diaria y antes de cada despliegue, conservada 14 días en el servidor |

### Integración y despliegue continuos

Los repositorios del servicio central, la aplicación web y la infraestructura tienen pipelines de GitHub Actions. En cada pull request se ejecutan las pruebas (1131 pruebas del servicio central contra MySQL 8.4, pruebas y build de la web, y validación de la configuración de infraestructura), que son requisito para fusionar. Cada cambio que llega a `main` se despliega en el servidor tras una aprobación manual. El despliegue toma un respaldo de la base de datos, reconstruye solo el componente modificado, comprueba su salud y, si falla, vuelve a la versión anterior. El detalle está en el manual de implementación y despliegue (Cortés Parra y García Artunduaga, 2026).

### Costo de hardware

La Tabla 5 detalla el hardware. Las primeras diez líneas corresponden al prototipo del dispositivo de medición, y las dos últimas a los equipos del equipo de desarrollo.

**Tabla 5**

*Costo de Hardware (COP)*

| Concepto | Cantidad | Costo unitario | Subtotal |
|---|---|---|---|
| ESP32 con Wi-Fi y Bluetooth | 1 | 24.000 | 24.000 |
| Medidor PZEM-004T-100A V3 | 1 | 70.000 | 70.000 |
| Fuente AC-DC HLK-PM03 | 1 | 20.000 | 20.000 |
| Cables jumper macho-macho de 20 cm | 40 | 300 | 12.000 |
| Cables jumper macho-hembra de 20 cm | 40 | 300 | 12.000 |
| Cable encauchetado 2x14 AWG | 3 | 3.333 | 9.999 |
| Cable calibre 14 rojo y negro | 1 | 2.400 | 2.400 |
| Conector JST XH2.54 | 1 | 1.300 | 1.300 |
| Cables jumper hembra-hembra de 20 cm | 6 | 383 | 2.298 |
| Caja plástica (carcasa del prototipo) | 1 | 20.000 | 20.000 |
| Computador del equipo de desarrollo | 2 | 3.500.000 | 7.000.000 |
| Celular del equipo de desarrollo | 2 | 450.000 | 900.000 |
| **Total de hardware** | | | **8.073.997** |

De este total, COP 173.997 corresponden al prototipo del dispositivo de medición. Es el gasto directo en componentes, cercano a los COP 180.000 que indica el documento general del proyecto.

### Costo de software, licencias e infraestructura

Todo el software de la pila es libre y no genera costo de licencia. La Tabla 6 separa lo gratuito de lo que se paga.

**Tabla 6**

*Costo de Software, Licencias e Infraestructura (COP)*

| Concepto | Tipo | Costo |
|---|---|---|
| Java 21 (OpenJDK), Spring Boot, Spring Modulith, Spring Security, Spring Data JPA | Libre | 0 |
| MySQL 8.4 y Liquibase | Libre | 0 |
| Eclipse Mosquitto | Libre | 0 |
| Docker, Docker Compose y Nginx | Libre | 0 |
| React 19, Vite 8, Zustand | Libre | 0 |
| Expo SDK 57 y React Native 0.86 | Libre | 0 |
| Arduino IDE y Visual Studio Code | Libre | 0 |
| Let's Encrypt (certificados TLS) | Libre | 0 |
| GitHub y GitHub Actions | Gratuito | 0 |
| Dominio `energy-monitor.tech` | Gratuito | 0 |
| Hostinger VPS KVM 2, contrato de un mes | Pago | 48.000 |
| **Total de software e infraestructura** | | **48.000** |

El precio del plan depende del plazo de contratación. El proyecto lo contrató por un mes, suficiente para la demostración y la sustentación. Para una operación continua conviene el contrato anual: COP 27.900 mensuales, equivalentes a COP 334.800 por año, frente a COP 576.000 si se renueva mes a mes. El dominio gratuito debe revisarse antes de su vencimiento, porque la renovación puede tener costo `[POR CONFIRMAR]`.

### Servicios de terceros

La Tabla 7 recoge los servicios externos que el sistema usa. En el volumen actual del proyecto ninguno genera costo.

**Tabla 7**

*Servicios de Terceros*

| Servicio | Uso | Costo |
|---|---|---|
| Let's Encrypt | Certificados TLS | 0 |
| Google Sign-In | Inicio de sesión con Google en la web | 0 |
| Gmail (SMTP con contraseña de aplicación) | Envío de verificación, recuperación y notificaciones | 0 |
| Web Push (VAPID) | Notificaciones en el navegador | 0 |
| Expo Push | Notificaciones en la aplicación móvil | 0 |
| GitHub Actions | Pruebas y despliegue continuo | 0 |

### Costo de recurso humano

La Tabla 8 valora el trabajo del equipo durante siete meses. Es un costo estimado para dimensionar el proyecto, no un pago efectivo.

**Tabla 8**

*Costo Estimado de Recurso Humano (COP)*

| Rol | Meses | Costo mensual | Subtotal |
|---|---|---|---|
| Líder de proyecto | 7 | 2.000.000 | 14.000.000 |
| Desarrollador de software | 7 | 2.500.000 | 17.500.000 |
| Gestor de bases de datos | 4 | 1.200.000 | 4.800.000 |
| Analista | 7 | 1.800.000 | 12.600.000 |
| **Total de recurso humano** | | | **48.900.000** |

### Presupuesto consolidado

**Tabla 9**

*Presupuesto Consolidado del Proyecto (COP)*

| Categoría | Subtotal | Participación |
|---|---|---|
| Hardware | 8.073.997 | 14,16 % |
| Software e infraestructura | 48.000 | 0,08 % |
| Recurso humano (7 meses) | 48.900.000 | 85,76 % |
| Otros | 0 | 0,00 % |
| **Total estimado** | **57.021.997** | **100 %** |

Casi el 86 % del costo es trabajo del equipo. Excluido el recurso humano, la inversión en equipos, prototipo y servidor es de COP 8.121.997, y la que realmente debe pagarse de forma recurrente es el servidor.

### Costos para los usuarios

El usuario no paga licencias de software. Necesita un celular Android o un navegador moderno. La Tabla 10 muestra rangos de referencia de los equipos.

**Tabla 10**

*Rangos de Precio de Equipos de Referencia para Usuarios (COP)*

| Equipo | Rango de precio |
|---|---|
| Celular Xiaomi Redmi 13 | 450.000 a 700.000 |
| Celular Samsung Galaxy A55 | 1.600.000 a 2.200.000 |
| Portátil HP 240 G9 (Core i5, 8 GB, 512 GB SSD) | 2.500.000 a 2.900.000 |
| Portátil Dell Inspiron 15 (Core i5, 8 GB) | 1.800.000 a 2.500.000 |

El costo del dispositivo de medición para un hogar es el de su prototipo, cercano a COP 174.000 en componentes. Un precio de venta o producción en serie queda fuera del alcance `[POR CONFIRMAR]`.

### Mantenimiento y licenciamiento

El plan de mantenimiento contempla tres modalidades. El correctivo atiende fallos de comunicación, carga, notificaciones e historial; un despliegue fallido se revierte de forma automática. El preventivo incluye actualización de dependencias (Spring Boot, React, Expo, firmware), los respaldos automáticos de MySQL (diarios y antes de cada despliegue), la renovación automática de los certificados, snapshots del servidor `[POR CONFIRMAR]` y pruebas de seguridad. El evolutivo considera medidores inteligentes, actualización del firmware por aire (hoy se hace por cable USB), la ampliación de la interfaz de administración (gestión de soporte y de contenidos) y adaptación a nuevas versiones de Android y de los navegadores. Durante el primer mes posterior al despliegue se presta soporte correctivo.

El software y el hardware se licencian bajo una modalidad de uso libre con reconocimiento comunitario: se pueden usar, copiar y modificar con fines educativos, investigativos y sociales, manteniendo la referencia al equipo y al SENA. El uso comercial requiere un acuerdo formal entre los desarrolladores y el SENA, y puede tener una tarifa de licencia `[POR CONFIRMAR]`.

## Cronograma y Estado Actual

### Cronograma

El proyecto inició el 1 de mayo de 2026 y se entrega el 28 de octubre de 2026, fecha fijada para la evaluación; la puesta en producción se hizo entre el 16 y el 20 de octubre, antes de la entrega. La Tabla 11 muestra las fases.

**Tabla 11**

*Fases y Fechas del Proyecto*

| Fase | Periodo | Resultado esperado |
|---|---|---|
| 1. Base y ciclo principal | 1 de mayo a 30 de junio | Un usuario se registra, crea un hogar, vincula un dispositivo y ve llegar mediciones |
| 2. Visibilidad del consumo y alertas | 1 de julio a 31 de agosto | El consumo es visible y el sistema avisa cuando algo es anormal |
| 3. Recomendaciones e internacionalización | 1 a 30 de septiembre | Recomendaciones según el consumo propio e interfaz en más de un idioma |
| 4. Consolidación | 1 a 16 de octubre | Sistema listo para demostración y sustentación |
| 5. Puesta en producción y entrega | 16 a 28 de octubre | Sistema publicado en `https://energy-monitor.tech` con integración y despliegue continuos (16 a 20 de octubre), y entrega con sustentación el 28 de octubre |

### Estado por componente

La Tabla 12 presenta el avance al 20 de octubre de 2026.

**Tabla 12**

*Estado por Componente al 20 de Octubre de 2026*

| Componente | Estado |
|---|---|
| Dispositivo de medición (firmware 1.3.8) | Operativo. La vinculación de un medidor en producción está pendiente de evidencia |
| Servicio central (siete módulos) | Desplegado en producción; 1131 pruebas automáticas en cada pull request |
| Aplicación web | Desplegada en producción en `https://energy-monitor.tech` |
| Aplicación móvil Android | En desarrollo, integrada con la API; fuera del despliegue en producción |
| Infraestructura | VPS Hostinger KVM 2 con Docker Compose, Nginx, Let's Encrypt y GitHub Actions |

Queda pendiente en el servicio central la gestión del historial del dispositivo según el periodo de vinculación. En producción faltan las evidencias funcionales que señala el manual de implementación y despliegue: medidor vinculado, correo, administración, respaldo programado y renovación de certificados.

## Riesgos Técnicos Pendientes

**Tabla 13**

*Riesgos Técnicos Pendientes*

| Código | Riesgo | Efecto |
|---|---|---|
| RT-01 | Mitigado: la `api_key` del dispositivo ya no se guarda en claro; solo se conserva su hash SHA-256 (`api_key_hash`, changesets `device-007` y `device-008`) | Las claves de dispositivos ya vinculados se renuevan al volver a vincularlos |
| RT-02 | La telemetría usa MQTT con QoS 0 | Posible pérdida de lecturas. Se evaluará QoS 1 |
| RT-03 | Los tokens de la web se guardan en localStorage | Riesgo de robo por XSS, reconocido |
| RT-04 | No hay actualización del firmware por aire (OTA) | El dispositivo se actualiza solo con cable |
| RT-05 | Los límites de intentos de autenticación están en memoria | Se reinician al reiniciar el servicio |
| RT-06 | Parcialmente mitigado: las pruebas de persistencia requieren una base de datos activa, pero en GitHub Actions se ejecutan contra un MySQL 8.4 de servicio | En local sigue siendo necesario un MySQL propio |
| RT-07 | El access token sigue válido hasta 15 minutos tras borrar la cuenta | Ventana breve de acceso residual |
| RT-08 | Algunos casos límite de alertas de conectividad no se resuelven solos | Alertas que requieren revisión manual |
| RT-09 | Web Bluetooth tiene limitaciones de compatibilidad y exige contexto seguro | Se debe usar el portal de configuración en otros navegadores; en producción el contexto seguro lo da HTTPS |
| RT-10 | Los respaldos de MySQL se guardan en el mismo servidor que la base de datos | Una falla del servidor pierde los datos y sus respaldos a la vez |
| RT-11 | No hay monitoreo de disponibilidad ni alertas en producción | Una caída solo se detecta durante un despliegue o por aviso de un usuario |

## Discrepancias Documentales

La Tabla 14 señala las diferencias entre lo que dicen otros documentos del proyecto y lo que está implementado o desglosado en esta propuesta. Se verificaron contra el código del servicio central y de la aplicación web. Esta propuesta adopta siempre el estado implementado.

**Tabla 14**

*Diferencias Detectadas y Acción Propuesta*

| Código | Fuente de la diferencia | Lo que dice | Estado verificado | Acción |
|---|---|---|---|---|
| DD-01 | README de la aplicación web | Las recomendaciones son simuladas porque el módulo `recommendation` del servicio central no existe | El módulo `recommendation` existe en el servicio central y evalúa cinco reglas de ahorro | Corregir el README de la aplicación web. Sigue abierta al 20 de octubre de 2026 |
| DD-02 | Documento general del proyecto | Decía: inversión de unos COP 180.000 y costo total `[POR CONFIRMAR]` | Presupuesto total estimado de COP 57.021.997. Los COP 180.000 equivalen al gasto directo en componentes del prototipo (COP 173.997) | Resuelta: la versión 1.1 del documento general incluye el costo total |
| DD-03 | Documento general del proyecto | Decía: alojamiento y servicios de pago no contemplados | VPS Hostinger KVM 2 de pago (COP 48.000 por un mes) y dominio gratuito | Resuelta: la versión 1.1 del documento general refleja la infraestructura de pago |

## Referencias

Congreso de la República de Colombia. (2012, 17 de octubre). *Ley 1581 de 2012: Por la cual se dictan disposiciones generales para la protección de datos personales*. Diario Oficial No. 48.587.

Cortés Parra, J. E., & García Artunduaga, M. A. (2026). *Manual de implementación y despliegue de EnergyMonitor*. SENA – CIES Regional Huila.

OASIS. (2014). *MQTT Version 3.1.1* (OASIS Standard). https://docs.oasis-open.org/mqtt/mqtt/v3.1.1/mqtt-v3.1.1.html
