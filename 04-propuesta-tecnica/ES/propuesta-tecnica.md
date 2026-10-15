# Propuesta Técnica de EnergyMonitor

Juan Esteban Cortés Parra y Miguel Ángel García Artunduaga

Servicio Nacional de Aprendizaje (SENA)

Tecnólogo en Análisis y Desarrollo de Software (ADSO), ficha 3145556

12 de octubre de 2026

## Resumen

Este documento presenta la propuesta técnica de EnergyMonitor, un sistema que permite a los hogares conocer el consumo eléctrico de cada electrodoméstico, identificar los periodos de mayor demanda, recibir alertas y obtener recomendaciones de ahorro. Describe la pila tecnológica elegida y su justificación, la arquitectura de software adoptada (un monolito modular con eventos internos), la infraestructura de alojamiento y los costos del proyecto. La solución integra un dispositivo ESP32 con sensor PZEM-004T V3, un servicio central en Java 21 con Spring Boot 4.1.1, una base de datos MySQL 8.4, un intermediario de mensajes Mosquitto con TLS, una aplicación web en React 19 y una aplicación móvil Android con Expo SDK 57. El costo total estimado es de COP 57.717.597, de los cuales COP 8.073.997 corresponden a hardware, COP 743.600 a infraestructura y COP 48.900.000 a recurso humano. El documento incluye además el cronograma hasta el 16 de octubre de 2026, los riesgos técnicos pendientes y las diferencias con el documento general del proyecto.

*Palabras clave:* propuesta técnica, arquitectura, monolito modular, costos, infraestructura, Internet de las cosas

## Introducción

EnergyMonitor es un proyecto formativo del programa Tecnólogo en Análisis y Desarrollo de Software (ADSO) del SENA, desarrollado por dos aprendices con fecha de entrega el 16 de octubre de 2026. Su finalidad es que los hogares vean cuánta energía consume cada electrodoméstico, detecten los momentos de mayor demanda, reciban alertas por consumo inusual y obtengan recomendaciones de ahorro basadas en sus propias mediciones.

Esta propuesta responde a la pregunta de si el proyecto es técnica y económicamente viable con los recursos del equipo. Se organiza en tres bloques: la justificación de la pila tecnológica, la arquitectura de software y la infraestructura con sus costos. Después presenta el cronograma y el estado por componente, los riesgos técnicos y las diferencias con el documento general del proyecto. Los requisitos funcionales y no funcionales se detallan en la especificación de requisitos (carpeta `02-especificacion-requisitos`), y los objetivos, el alcance y el equipo, en el documento general del proyecto (carpeta `01-documento-general-proyecto`).

Los datos que el equipo aún no ha definido se marcan con `[POR CONFIRMAR]` y no se suponen.

## Justificación de la Pila Tecnológica

### Visión general de la solución

El flujo de datos recorre cinco piezas. El dispositivo ESP32 lee el sensor PZEM-004T V3 por Modbus RTU a 9600 baudios (GPIO16 como recepción y GPIO17 como transmisión, firmware 1.3.8) y publica por MQTT 3.1.1 con TLS en el puerto 8883 hacia el intermediario Mosquitto. El servicio central en Java recibe las lecturas, las almacena en MySQL, evalúa alertas y recomendaciones y envía notificaciones. La aplicación web y la aplicación móvil Android consultan el servicio mediante una API REST. El dispositivo transmite telemetría cada 60 segundos y su estado cada cinco minutos. La vinculación se realiza por Bluetooth Low Energy desde la web, con un portal de configuración como alternativa.

### Decisiones por capa

La Tabla 1 resume la tecnología elegida en cada capa y la razón que la respalda. La última columna compara cada elección con alternativas comunes del mercado. Es una comparación de referencia, no el registro de evaluaciones formales hechas durante el proyecto.

**Tabla 1**

*Tecnología Elegida y Justificación por Capa*

| Capa | Tecnología elegida | Justificación | Alternativas de mercado |
|---|---|---|---|
| Servicio central | Java 21, Spring Boot 4.1.1, Spring Modulith, Spring Security, Spring Data JPA | Herramientas de software libre con eficiencia, escalabilidad y facilidad de integración. Spring Modulith permite separar el sistema en módulos con dependencias controladas dentro de una sola aplicación. || Node.js con NestJS o Express: ecosistema amplio y desarrollo rápido, pero sin la modularidad verificable de Spring Modulith. Django (Python) y ASP.NET Core (C#): válidos, con menos experiencia en el equipo. |
| Base de datos | MySQL 8.4 con migraciones Liquibase | Software libre. Liquibase versiona los cambios de la base de datos con reversión (rollback), lo que da trazabilidad. || PostgreSQL: igual de libre y más rico en funciones avanzadas, innecesarias para este volumen. MongoDB: flexible, pero los datos del proyecto son relacionales (hogares, miembros, dispositivos). Firebase: costo variable por uso y dependencia del proveedor. |
| Mensajería con el dispositivo | MQTT 3.1.1 con TLS sobre Eclipse Mosquitto | Protocolo estándar de Internet de las cosas (OASIS, 2014), apto para dispositivos pequeños. Mosquitto es libre y los certificados se obtienen con Let's Encrypt. El cifrado protege las lecturas en tránsito. || HTTP periódico desde el dispositivo: más simple, pero consume más recursos del microcontrolador y no tiene mecanismos de última voluntad. CoAP: menos soporte en brokers y herramientas. Brokers como EMQX o HiveMQ: más capacidad de la necesaria para un solo servidor. |
| Dispositivo | ESP32 con sensor PZEM-004T V3 | Microcontrolador con Wi-Fi y Bluetooth de bajo costo (COP 24.000) y sensor de energía de COP 70.000. Registra tensión, corriente, potencia activa y energía acumulada. || Arduino con módulo Wi-Fi o Raspberry Pi: el primero exige más componentes y la segunda tiene mayor costo y consumo. Sensores como el SCT-013 (pinza de corriente) requieren calibración y circuito propio, mientras que el PZEM-004T entrega los valores ya calculados. |
| Aplicación web | React 19, Vite 8, Zustand, Axios, React Router, React Hook Form, Zod, i18next, Recharts | Software libre. Permite gráficos interactivos, formularios validados e interfaz en varios idiomas. Web Bluetooth habilita la vinculación directa del dispositivo desde el navegador. || Angular o Vue: igual de viables, con menos conocimiento previo en el equipo. Next.js: añade renderizado en servidor, que la aplicación no necesita. Create React App: en desuso frente a Vite. |
| Aplicación móvil | React Native 0.86 con Expo SDK 57, expo-router, Redux Toolkit | Software libre. Reutiliza el conocimiento de React del equipo y atiende solo Android, que es el alcance definido. || Flutter (Dart): buen rendimiento, pero obliga a otro lenguaje y a no reutilizar lo aprendido en la web. Kotlin nativo: máximo control en Android, con mayor costo de desarrollo. Una aplicación web progresiva: menor acceso a funciones del teléfono. |
| Infraestructura | Docker Compose sobre un VPS de Hostinger KVM 2 | Despliegue reproducible en un único servidor de bajo costo. El mismo servidor aloja el servicio central, la base de datos y el intermediario. || AWS, Google Cloud o Azure: más servicios y escalado automático, con costo variable y mayor complejidad. Otros VPS (DigitalOcean, Contabo): comparables. Un servidor propio: exige red, energía y seguridad física. |

### Criterios generales de selección

Las decisiones de la Tabla 1 comparten tres criterios que orientaron la selección:

- Costo de licencias igual a cero: todo el software de la pila es libre, y solo se paga la infraestructura y el dominio.
- Un solo servidor: la carga prevista de un proyecto formativo no justifica varios servicios independientes. El sistema puede crecer aumentando los recursos del servidor, sin migrar a microservicios.
- Conocimiento del equipo: la pila usa herramientas que los dos integrantes manejan, lo que reduce el riesgo de entrega dentro del plazo.

### Limitaciones asumidas

Dos decisiones tienen límites conocidos. Web Bluetooth solo funciona en navegadores compatibles, como Chrome, y exige un contexto seguro; en los demás navegadores se usa el portal de configuración Wi-Fi. La aplicación móvil se limita a Android, por lo que no hay versión para iPhone.

## Arquitectura de Software

### Patrón arquitectónico elegido

EnergyMonitor es un monolito modular construido con Spring Modulith. Es una sola aplicación desplegable, dividida internamente en siete módulos con límites claros. No usa microservicios, Kafka ni API Gateway. La Tabla 2 describe cada módulo.

**Tabla 2**

*Módulos del Servicio Central*

| Módulo | Responsabilidad |
|---|---|
| security | Registro, autenticación, sesiones, permisos, verificación de correo, recuperación de contraseña, Google Sign-In, perfil y borrado de cuenta |
| home | Hogares, miembros, roles OWNER y MEMBER, favoritos, umbrales, eliminación del hogar y transferencia de propiedad |
| device | Registro, vinculación, edición y desvinculación de dispositivos, credenciales y autorización MQTT |
| measurement | Mediciones, telemetría y mensajes de estado |
| alert | Alertas de consumo y de conectividad |
| recommendation | Reglas de ahorro |
| notification | Entregas, preferencias, correo electrónico y Web Push |

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
| AlertRaised | alert | notification |
| RecommendationCreated | recommendation | notification |
| AccountDeleted | security | home |
| HomeDeleted | home | device |
| HomeOwnershipTransferred | home | sin consumidor |

El módulo recommendation no escucha eventos de medición: evalúa las recomendaciones cada hora mediante un programador (`RecommendationScheduler`). La pérdida de conectividad de un dispositivo se identifica mediante los registros de estado y los mecanismos de última voluntad y silencio del protocolo MQTT.

### Interfaces

La API es REST bajo `/api/v1` y cubre autenticación, cuentas, hogares, miembros, umbrales, dispositivos, mediciones, estadísticas, alertas, recomendaciones y preferencias de notificación. Incluye Actuator y Swagger. Existen además endpoints internos de autenticación y autorización MQTT para Mosquitto, con caché de cinco minutos.

### Seguridad

- Tokens JWT firmados con RS256. El access token dura 15 minutos.
- Refresh tokens opacos con estado, rotación y detección de reutilización, almacenados en la base de datos.
- Permisos globales consultados dinámicamente, sin roles dentro del JWT.
- La identidad se toma de la claim `sub` mediante CurrentUserResolver. El encabezado X-User-Id se ignora.
- CORS cerrado por defecto, con orígenes definidos por variables de entorno.
- Google Sign-In requiere credenciales OAuth configuradas.
- Los dispositivos se autentican ante el intermediario con `device_code` y `api_key`.
- La protección de los datos personales de los usuarios toma como referencia la Ley 1581 de 2012 (Congreso de la República de Colombia, 2012).

### Reglas de dominio relevantes

Los roles globales los administra security, mientras que los roles OWNER y MEMBER son por hogar, de modo que un usuario puede pertenecer a varios hogares con roles distintos. Los umbrales pertenecen al hogar, con valores predeterminados de 6 kWh diarios y 180 kWh mensuales. Los niveles de potencia son LOW (0 a 500 W), MEDIUM (500 a 1500 W), HIGH (1500 a 3000 W) y CRITICAL (desde 3000 W). El borrado de cuenta es transaccional: valida la contraseña, cierra las sesiones, elimina lógicamente los datos personales, libera el correo y gestiona las membresías.

### Clientes

La aplicación web separa sus funcionalidades por características, gestiona los textos con i18n y concentra las llamadas HTTP en `src/services/`. Se prueba con Vitest y Testing Library. La aplicación móvil usa la estructura de expo-router, archivos de estilos por componente y está disponible en español, inglés, francés y portugués. En el emulador de Android se usa 10.0.2.2 en lugar de localhost, y CORS no aplica en React Native.

### Convenciones de desarrollo

El código se escribe en inglés y los entregables académicos en español. Los commits siguen Conventional Commits con scope por módulo y mensaje en inglés imperativo. Las ramas son `feat/*` o `fix/*`, con integración mediante pull request a `dev` y luego a `main`.

## Infraestructura y Costos

### Infraestructura de alojamiento

La Tabla 4 describe la infraestructura prevista.

**Tabla 4**

*Infraestructura de Alojamiento*

| Elemento | Detalle |
|---|---|
| Proveedor | Hostinger |
| Plan | VPS KVM 2: 2 vCPU, 8 GB de RAM y 100 GB NVMe |
| Despliegue | Docker Compose con el servicio central, MySQL 8.4 y Mosquitto |
| Acceso | Proxy HTTPS delante de la API |
| Certificados | Let's Encrypt, sin costo |
| Dominio | Dominio .com, estimado en COP 50.000 por año `[POR CONFIRMAR]` al momento del pago |

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
| Docker y Docker Compose | Libre | 0 |
| React 19, Vite 8, Zustand | Libre | 0 |
| Expo SDK 57 y React Native 0.86 | Libre | 0 |
| Arduino IDE y Visual Studio Code | Libre | 0 |
| Let's Encrypt (certificados TLS) | Libre | 0 |
| Hostinger VPS KVM 2, 24 meses a COP 28.900 mensuales | Pago | 693.600 |
| Dominio .com, 1 año (estimado) | Pago | 50.000 |
| **Total de software e infraestructura** | | **743.600** |

La tarifa de COP 28.900 mensuales es promocional. La renovación del plan cuesta COP 50.900 mensuales, equivalentes a COP 610.800 por año.

### Servicios de terceros

La Tabla 7 recoge los servicios externos que el sistema usa. El costo de estos servicios aún no está definido, salvo el de los certificados.

**Tabla 7**

*Servicios de Terceros*

| Servicio | Uso | Costo |
|---|---|---|
| Let's Encrypt | Certificados TLS | 0 |
| Google Sign-In | Inicio de sesión con Google en la web | `[POR CONFIRMAR]` |
| Servicio de correo SMTP | Envío de verificación, recuperación y notificaciones | `[POR CONFIRMAR]` |
| Web Push | Notificaciones en el navegador | `[POR CONFIRMAR]` |

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
| Hardware | 8.073.997 | 14,0 % |
| Software e infraestructura | 743.600 | 1,3 % |
| Recurso humano (7 meses) | 48.900.000 | 84,7 % |
| Otros | 0 | 0,0 % |
| **Total estimado** | **57.717.597** | **100 %** |

Casi el 85 % del costo es trabajo del equipo. Excluido el recurso humano, la inversión en equipos, prototipo y servidor es de COP 8.817.597, y la que realmente debe pagarse de forma recurrente es la infraestructura.

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

El plan de mantenimiento contempla tres modalidades. El correctivo atiende fallos de comunicación, carga, notificaciones e historial. El preventivo incluye actualización de dependencias (Spring Boot, React, Expo, firmware), copias de seguridad periódicas de MySQL y snapshots del servidor, y pruebas de seguridad. El evolutivo considera medidores inteligentes, actualización del firmware por aire (hoy se hace por cable USB), una interfaz de administración y adaptación a nuevas versiones de Android y de los navegadores. Durante el primer mes posterior al despliegue se presta soporte correctivo.

El software y el hardware se licencian bajo una modalidad de uso libre con reconocimiento comunitario: se pueden usar, copiar y modificar con fines educativos, investigativos y sociales, manteniendo la referencia al equipo y al SENA. El uso comercial requiere un acuerdo formal entre los desarrolladores y el SENA, y puede tener una tarifa de licencia `[POR CONFIRMAR]`.

## Cronograma y Estado Actual

### Cronograma

El proyecto inició el 1 de mayo de 2026 y termina con la entrega del 16 de octubre de 2026. La Tabla 11 muestra las fases.

**Tabla 11**

*Fases y Fechas del Proyecto*

| Fase | Periodo | Resultado esperado |
|---|---|---|
| 1. Base y ciclo principal | 1 de mayo a 30 de junio | Un usuario se registra, crea un hogar, vincula un dispositivo y ve llegar mediciones |
| 2. Visibilidad del consumo y alertas | 1 de julio a 31 de agosto | El consumo es visible y el sistema avisa cuando algo es anormal |
| 3. Recomendaciones e internacionalización | 1 a 30 de septiembre | Recomendaciones según el consumo propio e interfaz en más de un idioma |
| 4. Consolidación y entrega | 1 a 16 de octubre | Sistema listo para demostración y sustentación |

### Estado por componente

La Tabla 12 presenta el avance al 12 de octubre de 2026, cuando el proyecto está en la fase 4.

**Tabla 12**

*Estado por Componente al 12 de Octubre de 2026*

| Componente | Estado |
|---|---|
| Dispositivo de medición (firmware 1.3.8) | Operativo |
| Servicio central (siete módulos) | Funcional de extremo a extremo |
| Aplicación web | En ajustes finales |
| Aplicación móvil Android | En desarrollo |

Queda pendiente en el servicio central la gestión del historial del dispositivo según el periodo de vinculación. Antes de la entrega falta confirmar y registrar los cambios locales pendientes, y completar los manuales de usuario, las pruebas y el informe técnico y de costos.

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
| RT-06 | Las pruebas de persistencia requieren una base de datos activa | Mayor esfuerzo para ejecutarlas |
| RT-07 | El access token sigue válido hasta 15 minutos tras borrar la cuenta | Ventana breve de acceso residual |
| RT-08 | Algunos casos límite de alertas de conectividad no se resuelven solos | Alertas que requieren revisión manual |
| RT-09 | Web Bluetooth tiene limitaciones de compatibilidad y exige contexto seguro | Se debe usar el portal de configuración en otros navegadores |

## Discrepancias Documentales

La Tabla 14 señala las diferencias entre lo que dicen otros documentos del proyecto y lo que está implementado o desglosado en esta propuesta. Se verificaron contra el código del servicio central y de la aplicación web. Esta propuesta adopta siempre el estado implementado.

**Tabla 14**

*Diferencias Detectadas y Acción Propuesta*

| Código | Fuente de la diferencia | Lo que dice | Estado verificado | Acción |
|---|---|---|---|---|
| DD-01 | README de la aplicación web | Las recomendaciones son simuladas porque el módulo `recommendation` del servicio central no existe | El módulo `recommendation` existe en el servicio central y evalúa cinco reglas de ahorro | Corregir el README de la aplicación web |
| DD-02 | Documento general del proyecto | Inversión de unos COP 180.000 y costo total `[POR CONFIRMAR]` | Presupuesto total estimado de COP 57.717.597. Los COP 180.000 equivalen al gasto directo en componentes del prototipo (COP 173.997) | Completar el costo total del documento general con esta cifra |
| DD-03 | Documento general del proyecto | Alojamiento y servicios de pago: no contemplados | VPS Hostinger KVM 2 de pago (COP 693.600 por 24 meses) y dominio (COP 50.000 por año) | Reflejar la infraestructura de pago en el presupuesto del documento general |

## Referencias

Congreso de la República de Colombia. (2012, 17 de octubre). *Ley 1581 de 2012: Por la cual se dictan disposiciones generales para la protección de datos personales*. Diario Oficial No. 48.587.

OASIS. (2014). *MQTT Version 3.1.1* (OASIS Standard). https://docs.oasis-open.org/mqtt/mqtt/v3.1.1/mqtt-v3.1.1.html
