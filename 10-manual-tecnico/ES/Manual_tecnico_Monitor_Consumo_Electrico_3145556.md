**SERVICIO NACIONAL DE APRENDIZAJE – SENA**

**CENTRO DE LA INDUSTRIA, LA EMPRESA Y LOS SERVICIOS (CIES)**

**REGIONAL HUILA**

Programa de Formación: Tecnología en Análisis y Desarrollo de Software (ADSO)

Ficha: 3145556

**MANUAL TÉCNICO DEL SOFTWARE**

**Proyecto: Monitor de Consumo Eléctrico**

*Documento técnico del sistema: arquitectura, tecnologías, estructura del código, base de datos, módulos, API y seguridad*

| **Dato**             | **Información**                                                                 |
|----------------------|---------------------------------------------------------------------------------|
| Integrantes          | Juan Esteban Cortés Parra, Miguel Ángel García Artunduaga                       |
| Instructor           | `[POR CONFIRMAR]`                                                               |
| Centro de Formación  | Centro de la Industria, la Empresa y los Servicios (CIES) – SENA Regional Huila |
| Programa             | Tecnología en Análisis y Desarrollo de Software (ADSO)                          |
| Ficha                | 3145556                                                                         |
| Tipo de documento    | Manual técnico de software                                                      |
| Versión              | 1.3                                                                             |
| Fecha de elaboración | 07 de octubre de 2026 (actualización: 25 de octubre de 2026)                    |

Año 2026

# 2. Control de versiones del documento

El historial de cambios del presente manual se registra en la siguiente tabla. Cada modificación relevante del software debe verse reflejada en una nueva versión del documento.

| **Versión** | **Fecha**  | **Descripción del cambio**                                                                                                                                                                                                   | **Responsable**      |
|-------------|------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------|
| 1.0         | 07/10/2026 | Creación inicial del manual técnico: descripción general, tecnologías, arquitectura, estructura de carpetas, base de datos, módulos, API y seguridad de la versión vigente del software.                                      | Equipo desarrollador |
| 1.0         | 07/10/2026 | Incorporación de los diagramas oficialmente aprobados: MER, diagrama de clases, arquitectura y secuencia de autenticación, además del diccionario de datos.                                                                  | Equipo desarrollador |
| 1.1         | 13/10/2026 | Actualización al estado final: siete módulos activos, 29 tablas, dispositivos y broker MQTT, API completa, identidad desde el JWT, hash de claves de dispositivo, aplicación móvil integrada con la API y limitaciones vigentes. | Equipo desarrollador |
| 1.2         | 24/10/2026 | Actualización a la arquitectura en producción (https://energy-monitor.tech): API gateway (8080) y microservicios de seguridad (8082), dispositivos (8084) y mediciones (8085); backend como monolito modular con home, alert, recommendation y notification (8083); REST interno `/internal/**` y eventos en RabbitMQ con transactional outbox; repositorios, variables, despliegue y trabajos pendientes (migración de los cuatro módulos restantes en curso). | Equipo desarrollador |
| 1.3         | 25/10/2026 | Diccionario de datos: se agrega la tabla push_device (instalaciones de la app móvil con push por el relay de Expo); 30 tablas documentadas. |

# 3. Tabla de contenido

- [2. Control de versiones del documento](#2-control-de-versiones-del-documento)
- [3. Tabla de contenido](#3-tabla-de-contenido)
- [4. Introducción](#4-introducción)
- [5. Objetivo](#5-objetivo)
- [6. Alcance técnico](#6-alcance-técnico)
- [7. Descripción general del sistema](#7-descripción-general-del-sistema)
  - [7.1 ¿Qué es el sistema?](#71-qué-es-el-sistema)
  - [7.2 Componentes principales](#72-componentes-principales)
  - [7.3 Roles del sistema](#73-roles-del-sistema)
  - [7.4 Estado de implementación por dominio](#74-estado-de-implementación-por-dominio)
- [8. Tecnologías utilizadas](#8-tecnologías-utilizadas)
  - [8.1 Dependencias principales del backend (pom.xml)](#81-dependencias-principales-del-backend-pomxml)
  - [8.2 Scripts disponibles](#82-scripts-disponibles)
- [9. Arquitectura del sistema](#9-arquitectura-del-sistema)
  - [9.1 Arquitectura general](#91-arquitectura-general)
  - [9.2 Estilo arquitectónico y patrones de diseño](#92-estilo-arquitectónico-y-patrones-de-diseño)
  - [9.3 Comunicación entre componentes](#93-comunicación-entre-componentes)
- [10. Estructura del proyecto](#10-estructura-del-proyecto)
  - [10.1 Estructura del backend](#101-estructura-del-backend)
  - [10.2 Estructura de la aplicación web](#102-estructura-de-la-aplicación-web)
  - [10.3 Estructura de la aplicación móvil](#103-estructura-de-la-aplicación-móvil)
  - [10.4 Función de las carpetas principales](#104-función-de-las-carpetas-principales)
- [11. Base de datos](#11-base-de-datos)
  - [11.1 Diagrama entidad-relación](#111-diagrama-entidad-relación)
  - [11.2 Tablas principales](#112-tablas-principales)
  - [11.3 Diccionario de datos](#113-diccionario-de-datos)
- [12. Módulos técnicos](#12-módulos-técnicos)
  - [12.1 Vista general de los módulos](#121-vista-general-de-los-módulos)
  - [12.2 Diagrama de clases del dominio](#122-diagrama-de-clases-del-dominio)
  - [12.3 Módulos del backend por contexto](#123-módulos-del-backend-por-contexto)
  - [12.4 Módulo front-end web](#124-módulo-front-end-web)
  - [12.5 Módulo front-end móvil](#125-módulo-front-end-móvil)
- [13. API o rutas del sistema](#13-api-o-rutas-del-sistema)
  - [13.1 Códigos de respuesta habituales](#131-códigos-de-respuesta-habituales)
  - [13.2 Ejemplo de petición y respuesta](#132-ejemplo-de-petición-y-respuesta)
- [14. Seguridad](#14-seguridad)
  - [14.1 Controles implementados](#141-controles-implementados)
  - [14.2 Limitaciones y riesgos conocidos](#142-limitaciones-y-riesgos-conocidos)
  - [14.3 Buenas prácticas para los desarrolladores](#143-buenas-prácticas-para-los-desarrolladores)
- [15. Control de versiones del software](#15-control-de-versiones-del-software)
- [16. Recomendaciones de mantenimiento](#16-recomendaciones-de-mantenimiento)
  - [16.1 Actualizaciones del código](#161-actualizaciones-del-código)
  - [16.2 Copias de seguridad de la base de datos](#162-copias-de-seguridad-de-la-base-de-datos)
  - [16.3 Soporte y resolución de incidentes](#163-soporte-y-resolución-de-incidentes)
  - [16.4 Trabajos pendientes declarados](#164-trabajos-pendientes-declarados)
- [17. Glosario](#17-glosario)
- [18. Anexos](#18-anexos)
  - [Anexo A. Configuración y comandos](#anexo-a-configuración-y-comandos)
- [Anexo B. MER por contexto](#anexo-b-mer-por-contexto)
- [Anexo C. Diagrama de clases por dominio](#anexo-c-diagrama-de-clases-por-dominio)

# 4. Introducción

El presente documento es el manual técnico del software «Monitor de Consumo Eléctrico», proyecto desarrollado por los aprendices del programa de formación en Análisis y Desarrollo de Software, ficha 3145556, del Centro de la Industria, la Empresa y los Servicios (CIES) – SENA Regional Huila.

El sistema permite monitorear el consumo de energía eléctrica en los hogares: un dispositivo de medición (ESP32 con sensor PZEM-004T V3) envía sus lecturas por MQTT, el servicio de mediciones las almacena y calcula estadísticas, el backend genera alertas y recomendaciones de ahorro y envía notificaciones por correo, por el navegador y a la app móvil, y las aplicaciones web y móvil permiten consultar la información. La solución se compone de un API gateway, tres microservicios (seguridad, dispositivos y mediciones), un backend modular (hogares, alertas, recomendaciones y notificaciones), un broker de mensajes RabbitMQ, un broker MQTT, un dispositivo de medición, una aplicación web, una aplicación móvil Android y una base de datos MySQL. Todo está en producción en https://energy-monitor.tech desde el 24 de octubre de 2026.

Este manual está dirigido a desarrolladores, personal de soporte técnico, el instructor evaluador y en general a cualquier persona que deba comprender, ampliar o dar mantenimiento al software. Su propósito es describir cómo está construido el sistema, no cómo se utiliza: para el uso cotidiano existe el manual de usuario, para la puesta en marcha el manual de instalación y para la gestión de cuentas y respaldos el manual de administrador.

La información aquí consignada fue obtenida directamente del código fuente de los repositorios del proyecto (API gateway, servicios de seguridad, dispositivos y mediciones, backend, web, móvil e infraestructura), de los changesets de Liquibase y de los documentos de esta documentación final, de modo que corresponda a la versión final del software entregado.

| **Fuente de información**            | **Ubicación**                                                                                                                                    |
|--------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------|
| API gateway                          | en-monitor-api-gateway (Java 21 + Spring Boot 4.0.8 + Spring Cloud Gateway Server Web MVC)                                                       |
| Microservicios                       | en-monitor-security-service, en-monitor-device-service y en-monitor-measurement-service (Java 21 + Spring Boot 4.1.1)                           |
| Código fuente del backend            | energy-monitor-backend (Java 21 + Spring Boot 4.1.1 + Spring Modulith): home, alert, recommendation y notification                              |
| Código fuente de la aplicación web   | Energy-Monitor-application (React 19 + Vite 8)                                                                                                   |
| Código fuente de la aplicación móvil | Energy-Monitor-Mobile (Expo SDK 57 + React Native 0.86)                                                                                          |
| Firmware del dispositivo             | carpeta `firmware/` de energy-monitor-backend (ESP32 + PZEM-004T V3, firmware 1.3.8)                                                             |
| Infraestructura y broker MQTT        | energy-monitor-infra (Docker Compose, Mosquitto 2 con mosquitto-go-auth, RabbitMQ 4.1, MySQL 8.4, despliegue)                                   |
| Especificación de requisitos         | [`02-especificacion-requisitos`](../../02-especificacion-requisitos/ES/especificacion-requisitos.md) (códigos RF-xxx y RNF-xxx)                  |
| Diseño de software                   | [`05-diseno-software`](../../05-diseno-software/ES/README.md): modelo arquitectónico (05.1), modelo de datos (05.2), modelo de clases (05.3), patrones (05.4) e interfaces (05.5) |
| Modelo de datos                      | Changesets de Liquibase en `src/main/resources/db/changelog` de cada servicio y [`05.2-modelo-de-datos.md`](../../05-diseno-software/ES/05.2-modelo-de-datos.md) |

# 5. Objetivo

> Documentar la arquitectura, las tecnologías, la estructura del proyecto, la base de datos, los módulos técnicos, la API, la seguridad y las recomendaciones de mantenimiento del sistema «Monitor de Consumo Eléctrico», de manera que un desarrollador ajeno al equipo pueda comprender el software, continuar su desarrollo y darle soporte sin necesidad de conocimiento previo del proyecto.

- Describir la arquitectura de microservicios con API gateway y el monolito modular que aún los acompaña, la comunicación entre servicios (REST interno y eventos en RabbitMQ), la comunicación con los dispositivos y los patrones de diseño empleados en cada componente.

- Detallar las tecnologías y versiones exactas utilizadas en los servicios del backend, en los dos front-ends y en el dispositivo.

- Explicar la organización del código fuente y la función de cada carpeta.

- Documentar el modelo de datos: diagrama entidad-relación, tablas principales y diccionario de datos.

- Relacionar cada módulo técnico con sus archivos, tablas y endpoints.

- Describir los servicios expuestos por la API REST y los mecanismos de seguridad aplicados.

- Establecer recomendaciones para la actualización, respaldo y soporte del sistema.

# 6. Alcance técnico

El manual técnico cubre los siguientes aspectos del sistema:

| **Sí cubre**                                                                            | **No cubre**                                                                                                         |
|-----------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------|
| Arquitectura general, servicios, capas y patrones de diseño del backend y de los front-ends. | Procedimientos de instalación, puesta en marcha y despliegue en producción (corresponde al manual de instalación).   |
| Tecnologías, librerías y versiones utilizadas, así como sus dependencias.               | Pasos de uso, pantallas y mensajes para el usuario final (corresponde al manual de usuario).                         |
| Estructura de carpetas y archivos de los repositorios del proyecto.                     | Gestión de usuarios, roles, reportes administrativos y copias de seguridad (corresponde al manual de administrador). |
| Base de datos: MER, tablas, relaciones y diccionario de datos.                          | Diseño eléctrico y cableado del dispositivo de medición (se describen en el manual de instalación).                  |
| Módulos técnicos de los servicios del backend, front web y front móvil, con su repositorio, archivos y tablas. | Historias de usuario, casos de uso y criterios de aceptación (documentos de análisis).                               |
| API REST: método, ruta, parámetros, autenticación y respuestas; rutas del gateway, comunicación interna entre servicios y el contrato MQTT. | Pruebas de carga.                                                                                                    |
| Seguridad: autenticación, autorización, cifrado, validaciones y limitaciones conocidas. | Costos, plan de proyectos e informes de gestión.                                                                     |
| Control de versiones y recomendaciones de mantenimiento.                                |                                                                                                                      |

**Versión documentada:** software en producción en https://energy-monitor.tech y en la rama `main` de los repositorios al 24 de octubre de 2026 (commits en la sección 15). Los trabajos que siguen pendientes, entre ellos la migración en curso de los módulos restantes del backend a microservicios, se describen en las secciones 14.2 y 16.4.

# 7. Descripción general del sistema

## 7.1 ¿Qué es el sistema?

«Monitor de Consumo Eléctrico» es una solución web y móvil que permite a los usuarios conocer cuánta energía consumen sus hogares, identificar los dispositivos que más energía utilizan, recibir alertas cuando el consumo supera los límites establecidos y obtener recomendaciones para reducir el gasto y la factura eléctrica. Las lecturas las toma un dispositivo de medición conectado a cada electrodoméstico.

## 7.2 Componentes principales

| **Componente**       | **Tipo**                                  | **Función**                                                                                                                                                                                  |
|----------------------|-------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| API gateway          | Servidor (Java 21 + Spring Cloud Gateway, puerto 8080) | Única entrada de la API: enruta cada petición al servicio dueño, responde CORS, valida el token (salvo en los endpoints públicos), limita los POST públicos de cuenta y rechaza `/internal/**`. |
| Servicio de seguridad | Microservicio (Java 21 + Spring Boot, puerto 8082) | Cuentas, registro con verificación de correo, inicio de sesión (incluido Google), JWT RS256 y refresh rotativo, sesiones en cookies HttpOnly para la web, recuperación de contraseña, perfil, borrado de cuenta, RBAC, administración de usuarios y contactos de soporte. |
| Servicio de dispositivos | Microservicio (Java 21 + Spring Boot, puerto 8084) | Vinculación, edición, credenciales y desvinculación de dispositivos, catálogo de electrodomésticos, autenticación y ACL del broker MQTT, comandos MQTT y conectividad de los módulos. |
| Servicio de mediciones | Microservicio (Java 21 + Spring Boot, puerto 8085) | Único suscriptor que guarda la telemetría; historial, última lectura, estadísticas, consumo del hogar y niveles de consumo; publica cada lectura como `measurement.recorded`. |
| Backend              | Monolito modular (Java 21 + Spring Boot + Spring Modulith, puerto 8083) | Hogares, miembros y umbrales (`home`), alertas (`alert`), recomendaciones (`recommendation`) y notificaciones por correo, Web Push, push móvil (Expo) y avisos in-app de administradores (`notification`). No se conecta al broker MQTT. |
| Broker de mensajes   | RabbitMQ 4.1                              | Transporta los eventos entre servicios (cuentas, hogares, dispositivos y mediciones) publicados con transactional outbox.                                                                   |
| Aplicación web       | SPA (React 19 + Vite 8)                   | Interfaz de consulta y gestión: acceso, panel de hogares, consumo, historial, usuarios, dispositivos (vinculación por Bluetooth), umbrales, alertas, recomendaciones y configuración.         |
| Aplicación móvil     | App Android (Expo SDK 57)                 | Interfaz móvil conectada a la API REST a través del gateway: cuenta, hogares, alertas, recomendaciones y preferencias de notificación. Solo Android.                                         |
| Broker MQTT          | Eclipse Mosquitto 2 + mosquitto-go-auth   | Recibe la telemetría y el estado de los dispositivos por MQTT 3.1.1 con TLS y autentica a cada cliente contra el servicio de dispositivos.                                                  |
| Dispositivo          | ESP32 + sensor PZEM-004T V3 (firmware 1.3.8) | Mide tensión, corriente, potencia activa y energía; publica una lectura cada 60 segundos y su estado cada cinco minutos.                                                                  |
| Base de datos        | MySQL 8.4                                 | Una sola base (`energy_monitor`) compartida por los servicios; cada servicio es dueño de sus tablas y aplica solo sus changesets. Almacena usuarios, hogares, dispositivos, mediciones, alertas, recomendaciones y notificaciones. |
| Correo electrónico   | SMTP (Mailpit en local)                   | Envía, desde el backend, los códigos de verificación y recuperación que solicita el servicio de seguridad y los correos de alerta y de recomendación.                                      |

## 7.3 Roles del sistema

| **Rol**                      | **Descripción**                                                                                  | **Cómo se implementa**                                                                                                               |
|------------------------------|--------------------------------------------------------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------|
| Propietario de hogar (OWNER) | Crea y administra un hogar: invita miembros, configura umbrales, gestiona dispositivos, transfiere la propiedad o elimina el hogar. | Enum OWNER de la tabla user_home, con permisos de dominio en el módulo home (estrategia por rol). Cada hogar tiene exactamente un OWNER. |
| Miembro de hogar (MEMBER)    | Consulta el consumo, el historial, las alertas y la configuración del hogar al que fue invitado. | Enum MEMBER de la tabla user_home. Un usuario puede ser OWNER en un hogar y MEMBER en otro.                                           |
| Usuario del sistema          | Cuenta personal con perfil, preferencias de idioma, tema y notificaciones.                       | Tabla user con estado ACTIVE / INACTIVE / BLOCKED.                                                                                   |
| Control de acceso (RBAC)     | Permisos funcionales globales agrupados en roles del sistema (códigos como METER.READ, ALERT.READ). | Tablas system_role, permission, user_system_role y system_role_permission del servicio de seguridad; se consultan en cada petición con CheckPermission (no viajan en el JWT). El backend las pide por `GET /internal/users/{id}/permissions` (caché de 30 segundos). |
| Administrador                | Gestión global del sistema y de cuentas.                                                         | Rol global ADMIN del servicio de seguridad (USER.READ, USER.MANAGE y ROLE.MANAGE): directorio de usuarios, cambio de estado y de roles en `/api/v1/admin/users` y `/api/v1/admin/roles`; avisos in-app en el backend (`/api/v1/admin/notifications`). El primer administrador se asigna con `make-admin.sh` del repositorio energy-monitor-infra. ADMIN no da acceso a ningún hogar. |

## 7.4 Estado de implementación por dominio

El sistema se documenta tal como está construido. La tabla relaciona cada grupo de requisitos de la [especificación de requisitos](../../02-especificacion-requisitos/ES/especificacion-requisitos.md) con el módulo que lo implementa.

| **Grupo de requisitos**                           | **Módulo del software**                            | **Estado**                                                                                                                                                                                     |
|---------------------------------------------------|----------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| RF-SEC – Gestión de Usuarios y Perfiles           | en-monitor-security-service + auth y account (web y móvil) | Implementado: registro, verificación de correo, inicio de sesión (incluido Google), renovación y cierre de sesión, recuperación y cambio de contraseña, perfil, permisos, administración de usuarios y eliminación de cuenta. |
| RF-HOM – Gestión de Hogares                       | home (backend) + dashboard y DetailHome (web)      | Implementado: crear hogar, código de acceso, miembros, favoritos, umbrales, eliminación del hogar y transferencia de propiedad. Pendiente: historial del dispositivo según el periodo de vinculación (RF-HOM-11). |
| RF-DEV – Gestión de Dispositivos                  | en-monitor-device-service + devices (web)          | Implementado: catálogo de electrodomésticos, vinculación por Bluetooth, edición, nuevas credenciales de Wi-Fi, desvinculación, autorización MQTT y conectividad.                              |
| RF-MED – Mediciones y Consumo                     | en-monitor-measurement-service + Consumption (web) | Implementado: ingesta MQTT de telemetría, estadísticas, resumen e historial de consumo del hogar (el estado de conexión lo registra el servicio de dispositivos).                             |
| RF-ALE – Alertas                                  | alert (backend) + Notifications (web y móvil)      | Implementado: alertas de dispositivo vinculado, desconexión, consumo alto o crítico y límite diario o mensual.                                                                                |
| RF-REC – Recomendaciones de Ahorro                | recommendation (backend)                           | Implementado: cinco reglas de ahorro evaluadas cada hora.                                                                                                                                      |
| RF-NOT – Notificaciones                           | notification (backend)                             | Implementado: entrega por correo, Web Push y push móvil (Expo) según las preferencias de cada usuario, y avisos in-app a los administradores.                                                 |
| RF-CLI – Aplicaciones web y móvil                 | Aplicación web y aplicación móvil                  | Web implementada. Móvil integrada con la API; eliminar y transferir un hogar solo existen en la web.                                                                                          |

# 8. Tecnologías utilizadas

La siguiente tabla registra los lenguajes, frameworks, librerías, herramientas de base de datos, de diseño y de control de versiones empleados en el proyecto, con su versión o detalle de uso.

| **Componente**                 | **Tecnología**                           | **Versión / detalle**                                                                              |
|--------------------------------|------------------------------------------|----------------------------------------------------------------------------------------------------|
| Lenguaje del backend           | Java                                     | Java 21 (LTS) en los cinco servicios                                                               |
| Framework del backend          | Spring Boot                              | Spring Boot 4.1.1 (Spring Framework 7) en seguridad, dispositivos, mediciones y backend; 4.0.8 en el gateway |
| API gateway                    | Spring Cloud Gateway Server Web MVC      | Spring Cloud 2025.1.3, Apache HttpClient 5 y Caffeine; paquetes routing, security, ratelimit y error |
| Estilo arquitectónico          | Microservicios + Spring Modulith + arquitectura hexagonal | 3 microservicios (seguridad, dispositivos y mediciones) detrás del gateway; backend con spring-modulith 2.1.1 y 4 módulos (home, alert, recommendation y notification) más 3 paquetes intermediarios |
| Construcción del backend       | Maven Wrapper                            | mvnw / mvnw.cmd en cada repositorio, spring-boot-maven-plugin                                      |
| Persistencia                   | Spring Data JPA + Hibernate              | ddl-auto = validate (el esquema lo define Liquibase)                                               |
| Migraciones de base de datos   | Liquibase                                | db/changelog con changesets y rollback en cada servicio; tabla DATABASECHANGELOG compartida        |
| Motor de base de datos         | MySQL                                    | MySQL 8.4, collation utf8mb4_unicode_ci, esquema energy_monitor compartido (ADR-003)               |
| Autenticación y autorización   | Spring Security + OAuth2 Resource Server | JWT RS256 firmado solo por el servicio de seguridad (clave privada PEM); el gateway y los servicios lo validan con la clave pública; BCrypt para contraseñas |
| Validación de datos            | Jakarta Bean Validation                  | @NotBlank, @Size, @NotNull, @Positive, @Pattern                                                    |
| Documentación de la API        | springdoc-openapi                        | springdoc 3.1.1, Swagger UI y /v3/api-docs en cada servicio                                        |
| Correo electrónico y push      | spring-boot-starter-mail + Web Push + Expo | SMTP con STARTTLS (Mailpit en local), solo desde el backend; push del navegador con claves VAPID y push de la app móvil con Expo |
| Mensajería entre servicios     | RabbitMQ 4.1 + Spring AMQP               | Exchanges por contexto, transactional outbox con relay cada segundo, colas durables con dead-letter |
| Comunicación interna           | REST `/internal/**`                      | Cabecera X-Internal-Token (INTERNAL_API_TOKEN) desde loopback o la red Docker; cachés Caffeine     |
| Mensajería con dispositivos    | MQTT 3.1.1 con TLS                       | Eclipse Mosquitto 2 + mosquitto-go-auth; los servicios de dispositivos y de mediciones se suscriben como clientes MQTT (Eclipse Paho) |
| Monitoreo y salud              | Spring Boot Actuator                     | /actuator/health y /actuator/info en cada servicio                                                 |
| Pruebas del backend            | JUnit 5 + Spring Boot Test               | backend 481, servicio de seguridad 660, servicio de mediciones 151, servicio de dispositivos 130 y gateway 57 pruebas, todas en verde (24 de octubre de 2026; seguridad, 22 de octubre) |
| Contenedores y despliegue      | Docker Compose + Nginx + Certbot         | repositorio energy-monitor-infra: modos local, desarrollo y producción; HTTPS con Let's Encrypt     |
| Integración y despliegue continuos | GitHub Actions                       | pruebas y build en cada PR y push a dev y main; despliegue desde main con aprobación del environment production |
| Front-end web                  | React + Vite                             | React 19.2, Vite 8.0, JavaScript (ESM), sin TypeScript                                             |
| Enrutamiento web               | react-router-dom                         | react-router-dom 7.17 con rutas protegidas y layouts anidados                                      |
| Consumo de API (web)           | axios                                    | axios 1.17 con interceptores de petición y respuesta                                               |
| Formularios y validación (web) | react-hook-form + zod                    | react-hook-form 7.78, zod 4.4, @hookform/resolvers                                                 |
| Gráficas (web)                 | recharts                                 | recharts 3.8 (AreaChart, PieChart, BarChart)                                                       |
| Internacionalización (web)     | i18next + react-i18next                  | 4 idiomas: es, en, fr, pt                                                                          |
| Estilos (web)                  | CSS Modules + variables CSS              | design system propio en src/design, temas claro/oscuro                                             |
| Pruebas (web)                  | Vitest + Testing Library                 | vitest 5.0, jsdom, 14 archivos de prueba                                                           |
| App móvil                      | Expo + React Native                      | Expo SDK 57, React Native 0.86.3, TypeScript 6 (strict), solo Android                              |
| Enrutamiento móvil             | Expo Router                              | rutas file-based en src/app con rutas tipadas                                                      |
| Gráficas (móvil)               | react-native-chart-kit                   | LineChart, BarChart y PieChart sobre react-native-svg                                              |
| Estilos (móvil)                | StyleSheet                               | design system propio en src/constants (theme, colors, spacing)                                     |
| Dispositivo                    | ESP32 + PZEM-004T V3                     | firmware 1.3.8 (Arduino), Modbus RTU a 9600 baudios, MQTT 3.1.1 con TLS en el puerto 8883          |
| Control de versiones           | Git y GitHub                             | repositorios de código y de documentación                                                          |
| Diseño y diagramas             | Draw.io y PlantUML                       | casos de uso, actividades, MER, diagrama de clases y arquitectura                                  |

> *Nota sobre el estado global: `zustand` (web) y `@reduxjs/toolkit` con `react-redux` (móvil) figuran en los package.json, pero el código fuente no los importa. El estado global se maneja con React Context y `useSyncExternalStore`.*

## 8.1 Dependencias principales del backend (pom.xml)

El archivo pom.xml de energy-monitor-backend declara como dependencias centrales spring-boot-starter-webmvc, spring-boot-starter-data-jpa, spring-boot-starter-security, spring-boot-starter-security-oauth2-resource-server, spring-boot-starter-validation, spring-boot-starter-liquibase, spring-boot-starter-mail, spring-boot-starter-actuator, spring-boot-starter-amqp, spring-modulith-starter-core (con sus módulos de observabilidad y actuator), springdoc-openapi-starter-webmvc-ui 3.1.1, caffeine, mysql-connector-j (runtime) y el conjunto de starters de prueba (\*-test, spring-boot-starter-amqp-test y spring-modulith-starter-test). Ya no incluye cliente MQTT.

Los microservicios de seguridad, dispositivos y mediciones comparten la base (webmvc, data-jpa, security, security-oauth2-resource-server, validation, liquibase, actuator, amqp, springdoc y mysql-connector-j); los de dispositivos y mediciones agregan spring-integration-mqtt con Eclipse Paho, y el de mediciones, caffeine. El gateway declara spring-cloud-starter-gateway-server-webmvc, httpclient5, spring-boot-starter-security, spring-boot-starter-security-oauth2-resource-server, spring-boot-starter-actuator y caffeine.

## 8.2 Scripts disponibles

| **Repositorio** | **Script**                      | **Función**                                                                      |
|-----------------|---------------------------------|----------------------------------------------------------------------------------|
| gateway y servicios | mvnw spring-boot:run        | Compila y arranca cada servicio: gateway en 8080, seguridad en 8082, backend en 8083, dispositivos en 8084 y mediciones en 8085. |
| gateway y servicios | mvnw test                   | Ejecuta la suite de pruebas del repositorio (los servicios con base de datos requieren MySQL en ejecución). |
| gateway y servicios | mvnw package                | Genera el archivo JAR ejecutable en /target.                                     |
| infra           | docker compose up -d --build    | Levanta la pila completa local (MySQL, RabbitMQ, Mosquitto, los cinco servicios Java y la web). |
| infra           | docker compose -f docker-compose.yml -f docker-compose.dev.yml up -d mysql rabbitmq mosquitto | Modo desarrollo: solo MySQL, RabbitMQ y Mosquitto en Docker; los servicios corren con ./mvnw. |
| infra           | e2e/device-flow.sh y e2e/measurement-flow.sh | Pruebas de punta a punta sobre la pila local.                       |
| web             | npm run dev                     | Inicia el servidor de desarrollo Vite en http://localhost:5173.                  |
| web             | npm run build / npm run preview | Construye la aplicación en /dist y la sirve para revisión.                       |
| web             | npm test                        | Ejecuta las pruebas con Vitest.                                                  |
| web             | npm run lint                    | Analiza el código con ESLint.                                                    |
| móvil           | npm run start                   | Inicia Metro Bundler de Expo.                                                    |
| móvil           | npm run android                 | Ejecuta la aplicación en un emulador o dispositivo Android.                      |
| móvil           | npm run lint                    | Analiza el código con ESLint (no hay pruebas automatizadas).                     |
| firmware (backend/firmware) | arduino-cli compile / upload | Compila y carga el firmware por USB (no hay actualización por aire, OTA).   |

# 9. Arquitectura del sistema

## 9.1 Arquitectura general

El sistema adopta una arquitectura cliente-servidor en capas, con la lógica repartida en microservicios detrás de un API gateway y un canal adicional para los dispositivos. La capa de presentación está conformada por la aplicación web y la aplicación móvil, que solo llaman al gateway; la capa de lógica la forman tres microservicios (seguridad, dispositivos y mediciones) y el backend, que sigue siendo un monolito modular con los contextos home, alert, recommendation y notification; los servicios se comunican por REST interno (`/internal/**`) y por eventos en RabbitMQ; la capa de datos corresponde a una sola base MySQL compartida, accedida mediante JPA/Hibernate y gestionada con Liquibase, en la que cada servicio es dueño de sus tablas; y los dispositivos de medición publican sus lecturas por MQTT con TLS a un broker Mosquitto que consulta al servicio de dispositivos para autenticarlos. La migración de los cuatro módulos que quedan en el backend a microservicios está en curso (sección 16.4).

> **Imágenes y diagramas (Figuras 1 a 11):** todas las figuras de este documento se encuentran en la siguiente carpeta de Google Drive.  
> https://drive.google.com/drive/folders/1tIzih2FKP8xte4vKYEtrWThFmZxR1oBX?usp=sharing
>
> **Figuras posiblemente desactualizadas (`[POR CONFIRMAR]` tras revisarlas en la carpeta):** las Figuras 3 y 5 a 8 (MER) se dibujaron antes de las últimas tablas y columnas y no incluyen `notification_preference`, `push_subscription`, `user.google_subject`, `home_thresholds.limit_period` ni `device.api_key_hash`; las Figuras 4 y 9 a 11 (diagrama de clases) pueden mostrar como pendientes las clases de estrategia por rol y los módulos device y recommendation; y las Figuras 1 y 2 (arquitectura y secuencia de autenticación) pueden mostrar la cabecera X-User-Id, un sistema sin dispositivos ni broker o el backend como una sola aplicación, sin API gateway ni microservicios; la arquitectura vigente es la que describen las tablas de esta sección.

**Figura 1.** *Arquitectura general del sistema (tres capas: presentación, lógica y datos).*

| **Capa**         | **Tecnología**                                        | **Responsabilidad**                                                                                                  |
|------------------|-------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------|
| Presentación     | React 19 (web) y Expo SDK 57 (móvil, Android)         | Interfaz de usuario, validación de formularios, manejo de sesión y consumo de la API REST a través del gateway.      |
| Entrada          | Spring Cloud Gateway (en-monitor-api-gateway, 8080)   | Enrutamiento por prefijo de ruta, CORS, validación del token, límite de peticiones y rechazo de `/internal/**`.       |
| Lógica           | Spring Boot 4.1.1 + Spring Security; Spring Modulith en el backend | Microservicios de seguridad, dispositivos y mediciones y backend modular: autenticación, autorización, casos de uso, reglas de negocio, ingesta MQTT y publicación de eventos. |
| Mensajería       | RabbitMQ 4.1 + transactional outbox                   | Eventos entre servicios con entrega al menos una vez y colas con dead-letter.                                        |
| Datos            | MySQL 8.4 + Spring Data JPA + Liquibase               | Una sola base compartida; cada servicio es dueño de sus tablas y aplica solo sus changesets; sin llaves foráneas entre servicios. |
| Dispositivos     | ESP32 + PZEM-004T V3 + Mosquitto 2                    | Medición, publicación MQTT con TLS (puerto 8883) y autenticación de cada dispositivo contra el servicio de dispositivos. |
| Transversal      | Docker Compose (energy-monitor-infra), Nginx + Certbot, GitHub Actions, Mailpit y Actuator | Contenedores, HTTPS en producción, integración y despliegue continuos, correo de pruebas y salud de cada servicio. |

**Servicios del backend.** Todos en Java 21; Spring Boot 4.1.1 salvo el gateway (4.0.8). Los microservicios siguen arquitectura hexagonal (puertos y adaptadores) con las capas `domain`, `application`, `adapter`, `infrastructure` y `api`; el gateway no tiene dominio (paquetes `routing`, `security`, `ratelimit` y `error`).

| **Servicio (repositorio)**       | **Puerto** | **Responsabilidad**                                                                                                                                                       |
|----------------------------------|------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| en-monitor-api-gateway           | 8080       | Única entrada de la API (Spring Cloud Gateway Server Web MVC, Spring Cloud 2025.1.3). Responde CORS, valida el token (RS256, emisor y expiración) salvo en los endpoints públicos, lee el token de la cookie HttpOnly `em_access` de la web (`AccessTokenCookieFilter`), limita los POST públicos de cuenta (20 por IP y ruta por minuto, luego 429), rechaza `/internal/**`, responde 503 si el servicio no responde y 504 si tarda más de 30 segundos. |
| en-monitor-security-service      | 8082       | Cuentas, registro con verificación de correo, inicio de sesión, Google, JWT RS256 (access token de 15 minutos) y refresh token rotativo, sesiones en cookies HttpOnly para la web, recuperación de contraseña, perfil, borrado de cuenta, RBAC y administración de usuarios, contactos de soporte (`GET /api/v1/support/contacts`, pública). |
| en-monitor-device-service        | 8084       | Dispositivos: vinculación, edición, reemisión de credenciales (API key guardada como hash SHA-256), desvinculación, catálogo de electrodomésticos, autenticación y ACL del broker MQTT (Mosquitto go-auth le pregunta), comandos MQTT a los módulos y conectividad (suscrito a telemetría y estado; sin conexión tras 150 segundos sin datos). |
| en-monitor-measurement-service   | 8085       | Mediciones: único suscriptor que guarda la telemetría (MQTT), historial, última lectura, estadísticas, consumo de un hogar (resumen e historial) y niveles de consumo; publica cada lectura como `measurement.recorded` y ofrece un API interno al backend. |
| energy-monitor-backend           | 8083       | Monolito modular (Spring Modulith): hogares, miembros y umbrales (`home`), alertas (`alert`), recomendaciones (`recommendation`), notificaciones por correo, Web Push (VAPID), push móvil (Expo) y avisos in-app de administradores (`notification`), más los paquetes intermediarios `security`, `device` y `measurement`. Ya no se conecta al broker MQTT. |

## 9.2 Estilo arquitectónico y patrones de diseño

- **Microservicios detrás de un API gateway.** Los contextos de seguridad, dispositivos y mediciones se extrajeron del backend a servicios propios (en-monitor-security-service, en-monitor-device-service y en-monitor-measurement-service), cada uno con su repositorio, su despliegue y sus tablas. El gateway (en-monitor-api-gateway) es la única entrada de la API y enruta por prefijo de ruta, de la más específica a la más general; los clientes llaman las mismas rutas de siempre, de modo que la migración no cambió la API pública.

- **Monolito modular (Spring Modulith) en el backend.** energy-monitor-backend sigue siendo una única aplicación con cuatro contextos acotados (home, alert, recommendation y notification) y tres paquetes intermediarios (security, device y measurement) que atienden el contrato `api` que los demás módulos ya usaban, sin dominio ni tablas: llaman al API interno del servicio correspondiente (`adapter/out/remote`, con caché) y convierten sus eventos de RabbitMQ en eventos en memoria (`adapter/in/messaging`). Cada módulo declara sus dependencias permitidas en el archivo package-info.java y sólo puede comunicarse con los demás mediante los tipos de su paquete api; esta restricción se verifica automáticamente con la prueba ModularityTests.

- **Arquitectura hexagonal por módulo y por servicio.** Dentro de cada módulo del backend y de cada microservicio conviven las capas api, domain, application (puertos de entrada `port/in`, puertos de salida `port/out`, casos de uso `usecase`), adapter (entrada `adapter/in/web`, `adapter/in/internal`, `adapter/in/messaging`, `adapter/in/mqtt`, `adapter/in/event`, `adapter/in/scheduler` y salida `adapter/out/persistence`, `out/messaging`, `out/event`, `out/client` u `out/remote`) e infrastructure con la configuración de Spring.

- **Eventos entre servicios con RabbitMQ y transactional outbox.** Lo que un servicio debe avisar a otro se escribe en su tabla de outbox en la misma transacción que el cambio; un relay lo publica cada segundo con confirmación del broker. La entrega es al menos una vez, los consumidores toleran duplicados y cada cola es durable, con dead-letter tras tres intentos (detalle en la sección 9.3).

- **Eventos en proceso dentro del backend (sin Kafka).** Los módulos del backend se comunican con eventos de Spring en memoria: `AlertRaised` (alert → notification), `RecommendationCreated` (recommendation → notification), `MeasurementRecorded` y `DeviceConnectivityLost` / `DeviceConnectivityRestored` (los republican los intermediarios measurement y device al recibir el mensaje de RabbitMQ → alert) y `HomeOwnershipTransferred` (home, sin consumidor todavía). La pérdida de conectividad de un dispositivo la detecta el servicio de dispositivos (mensaje de última voluntad o 150 segundos de silencio) y la publica como `device.connectivity-lost`.

- **Base de datos compartida (ADR-003).** Todos los servicios usan la misma base MySQL; cada uno es dueño de sus tablas y aplica solo sus changesets Liquibase. No hay llaves foráneas entre servicios: los otros contextos se referencian por identificador.

- **Recomendaciones por programador.** El módulo recommendation no escucha `MeasurementRecorded`: `RecommendationScheduler` ejecuta cada hora las reglas de ahorro con la energía por hora que pide al servicio de mediciones y publica `RecommendationCreated` por cada recomendación nueva.

- **API sin sesión HTTP en el servidor.** Los servicios no guardan sesión HTTP: cada petición transporta un JWT. La app móvil lo envía en la cabecera Authorization; la web lo recibe en cookies HttpOnly (`em_access` y `em_refresh`) que el gateway convierte en `Authorization: Bearer`. La política de CORS se resuelve solo en el gateway, de forma explícita y cerrada por defecto.

- **Front-end por capacidades (features).** Tanto la web como la móvil organizan el código en carpetas por capacidad funcional (auth, dashboard, homes, notifications…) con componentes, estilos, validaciones zod y pruebas propias. El estado global se maneja con React Context; la web guarda en localStorage solo los datos no secretos de la sesión (cuenta, perfil, permisos e id de sesión), nunca los tokens.

## 9.3 Comunicación entre componentes

Las aplicaciones cliente se comunican únicamente con el API gateway mediante peticiones HTTP en formato JSON bajo el prefijo /api/v1; en producción Nginx termina HTTPS en https://energy-monitor.tech y las peticiones a /api/ llegan al gateway a través del nginx del contenedor web. El gateway valida el token y reenvía cada petición al servicio dueño de la ruta. La app móvil envía la cabecera Authorization: Bearer \<accessToken\>; la web recibe los tokens en cookies HttpOnly y el gateway convierte la cookie `em_access` en esa cabecera (solo si la petición trae `X-Requested-With: XMLHttpRequest`). La identidad del usuario se obtiene exclusivamente del claim `sub` del JWT mediante `CurrentUserResolver` en cada servicio; la cabecera X-User-Id se ignora. El front web renueva automáticamente el access token ante una respuesta 401 mediante POST /auth/refresh, con rotación del refresh token y bloqueo entre pestañas con la API Web Locks.

| **Elemento de comunicación** | **Detalle**                                                                                      |
|------------------------------|--------------------------------------------------------------------------------------------------|
| Protocolo (clientes)         | HTTP/1.1 sobre HTTPS en producción (Nginx + Let's Encrypt); HTTP en desarrollo local.            |
| Formato                      | JSON (Content-Type: application/json).                                                           |
| Punto de entrada             | API gateway: https://energy-monitor.tech/api/v1 en producción, http://localhost:8080/api/v1 en desarrollo. |
| Prefijo de la API            | /api/v1 (todas las rutas públicas; las rutas `/internal/**` nunca pasan por el gateway).         |
| Autenticación                | Authorization: Bearer \<JWT RS256\> con expiración de 15 minutos (móvil) o cookie HttpOnly `em_access` (web). El gateway y cada servicio validan firma, emisor y expiración. |
| Identidad                    | Claim `sub` del JWT; X-User-Id se ignora.                                                        |
| Origen permitido (CORS)      | Solo en el gateway, que elimina la cabecera Origin antes de reenviar. Cerrado por defecto; los orígenes se definen con SECURITY_CORS_ALLOWED_ORIGINS del gateway (p. ej. http://localhost:5173), sin credenciales. Métodos: GET, POST, PUT, DELETE y OPTIONS. |
| Manejo de errores            | Respuesta con estructura ApiError: timestamp, status, error, message, path y fieldErrors. Las respuestas propias del gateway (401, 404, 429, 503 y 504) usan la misma forma. |
| Documentación                | OpenAPI y Swagger UI en /v3/api-docs y /swagger-ui.html de cada servicio (en modo desarrollo, en los puertos 8082, 8083, 8084 y 8085). |
| Salud del servicio           | GET /actuator/health de cada servicio devuelve {"status":"UP"} (incluye base de datos y RabbitMQ). |
| Comunicación interna         | REST `/internal/**` entre servicios con la cabecera X-Internal-Token, y eventos en RabbitMQ 4.1 (AMQP, puerto 5672) con transactional outbox. |
| Dispositivos (MQTT)          | MQTT 3.1.1; puerto 8883 con TLS (dispositivos, `mqtt.energy-monitor.tech:8883` en producción) y 1883 solo dentro de Docker (servicios de dispositivos y de mediciones). |

**Rutas del API gateway.** Las rutas de dispositivos y de mediciones se evalúan antes que la ruta general del backend, de modo que `/api/v1/homes/{homeId}`, sus miembros y sus umbrales siguen en el backend.

| **Rutas**                                                                                              | **Servicio**                             |
|--------------------------------------------------------------------------------------------------------|------------------------------------------|
| `/api/v1/auth/**`, `/api/v1/admin/users/**`, `/api/v1/admin/roles/**`, `/api/v1/support/**`             | en-monitor-security-service (8082)       |
| `/api/v1/homes/{homeId}/devices/**`, `/api/v1/appliance-types/**`                                      | en-monitor-device-service (8084)         |
| `/api/v1/measurements/**`, `/api/v1/consumption-levels/**`, `/api/v1/homes/{homeId}/consumption/**`    | en-monitor-measurement-service (8085)    |
| Resto de `/api/v1/**` (hogares, miembros, umbrales, alertas, recomendaciones, notificaciones y avisos de administrador) | energy-monitor-backend (8083) |
| `/internal/**`                                                                                         | Nunca se reenvía (401 sin token, 403 con token) |
| Cualquier otra ruta                                                                                    | 404                                      |

**REST interno entre servicios.** Las rutas `/internal/**` solo se aceptan desde loopback o la red Docker (`172.16.0.0/12`) **y** con la cabecera `X-Internal-Token` (valor de `INTERNAL_API_TOKEN`, el mismo en todos los servicios), comparada en tiempo constante; sin token configurado se rechaza toda llamada. La autenticación del broker (`/internal/mqtt/**`) solo exige la red permitida.

| **Llama**              | **Endpoint**                                                                                                   | **Para qué**                                                                         |
|------------------------|----------------------------------------------------------------------------------------------------------------|--------------------------------------------------------------------------------------|
| backend → seguridad    | `GET /internal/users/{id}/permissions`, `POST /internal/users/profiles`, `GET /internal/administrators`         | Permisos, perfiles y administradores (con caché).                                    |
| seguridad → backend    | `GET /internal/accounts/{id}/deletion-check`                                                                   | ¿Se puede borrar la cuenta? (204 sí; 409 no).                                        |
| backend → dispositivos | `GET /internal/devices/{id}`, `GET /internal/homes/{homeId}/devices`                                           | Leer dispositivos (caché de 30 segundos; copia anterior hasta 1 hora si no responde). |
| dispositivos → backend | `GET /internal/homes/{homeId}/access?userId=`                                                                  | ¿Es miembro o dueño del hogar?                                                       |
| mediciones → backend   | `GET /internal/homes/{homeId}/access?userId=`, `GET /internal/homes/{homeId}/thresholds`                       | Acceso y límites del hogar.                                                          |
| mediciones → dispositivos | `GET /internal/devices/{id}`, `GET /internal/homes/{homeId}/devices`                                        | Hogar de un módulo y módulos de un hogar (con caché).                                |
| backend → mediciones   | `GET /internal/consumption-levels`, `GET /internal/measurements/hourly-energy`, `GET /internal/measurements/reporting-devices` | Clasificar lecturas (niveles en caché 10 minutos) y energía por hora para las alertas de límite y las recomendaciones. |
| Mosquitto → dispositivos | `POST /internal/mqtt/auth/user`, `POST /internal/mqtt/auth/acl`                                              | Autenticar y autorizar cada cliente MQTT.                                            |

**Eventos en RabbitMQ.** Cada productor escribe el mensaje en su tabla de outbox en la misma transacción que el cambio; un relay lo publica cada segundo, del más antiguo al más reciente, y solo lo marca como publicado cuando el broker lo confirma. La entrega es al menos una vez y las colas son durables, con dead-letter tras tres intentos.

| **Exchange / routing key**                                                                                         | **Outbox (productor)**             | **Consumidor**                                   | **Efecto**                                                                                       |
|--------------------------------------------------------------------------------------------------------------------|------------------------------------|--------------------------------------------------|--------------------------------------------------------------------------------------------------|
| `energy-monitor.security` / `account.registered`, `account.administered`, `account.deleted`, `mail.email-verification`, `mail.password-reset` | `security_outbox` (seguridad) | backend | Avisos in-app a los administradores, limpieza de las cuentas borradas y envío de correos (solo el backend envía correo). |
| `energy-monitor.home` / `home.deleted`                                                                             | `home_outbox` (backend)            | dispositivos                                     | Desvincula los módulos del hogar y les envía el comando `unlink`.                                |
| `energy-monitor.device` / `device.linked`, `device.unlinked`, `device.connectivity-lost`, `device.connectivity-restored` | `device_outbox` (dispositivos) | backend y mediciones (solo linked y unlinked)    | Backend: alertas CONNECTIVITY y caché de dispositivos; mediciones: caché de dispositivos.        |
| `energy-monitor.measurement` / `measurement.recorded`                                                              | `measurement_outbox` (mediciones; los entregados se purgan a los 7 días) | backend                | Se republica en memoria como `MeasurementRecorded`: alertas de umbral, de límite y de dispositivo vinculado. |

Dentro del backend siguen en memoria (eventos de Spring) `AlertRaised` y `RecommendationCreated` (→ notification), `MeasurementRecorded` y `DeviceConnectivityLost` / `DeviceConnectivityRestored` (los republican los intermediarios → alert) y `HomeOwnershipTransferred`.

**Contrato MQTT.**

| **Parámetro**             | **Valor**                                                                                                                              |
|---------------------------|----------------------------------------------------------------------------------------------------------------------------------------|
| Client ID                 | `esp32-{deviceId}`                                                                                                                     |
| Usuario / contraseña      | `device_code` / `api_key` (el servicio de dispositivos solo guarda el hash de la clave)                                                |
| Keep alive y última voluntad | Keep alive de 15 segundos; el broker publica la última voluntad aproximadamente a los 22 segundos de una caída de energía             |
| QoS                       | 0 para telemetría y estado; 1 para el comando                                                                                          |
| Cadencia                  | Telemetría cada 60 segundos; estado cada cinco minutos y al conectar; sin conexión tras 150 segundos de silencio                       |
| Tópico de telemetría      | `energy-monitor/devices/{deviceId}/telemetry`                                                                                          |
| Tópico de estado          | `energy-monitor/devices/{deviceId}/status` (retenido)                                                                                  |
| Tópico de comando         | `energy-monitor/devices/{deviceId}/command` (QoS 1, retenido): `{"cmd":"unlink"}` al desvincular el dispositivo                         |

El broker (imagen `iegomez/mosquitto-go-auth`) no guarda usuarios: autentica a cada cliente contra el servicio de dispositivos (`/internal/mqtt/auth/user`) con una caché de cinco minutos, y su ACL permite a cada dispositivo usar únicamente sus propios tópicos (`/internal/mqtt/auth/acl`). Los clientes MQTT son tres: el servicio de dispositivos (`MQTT_DEVICE_SERVICE_USER`: lee telemetría y estado y es el único que publica comandos), el servicio de mediciones (`MQTT_MEASUREMENT_SERVICE_USER`: solo lee telemetría) y cada módulo ESP32 (`device_code` y API key). El backend ya no se conecta al broker. Los servicios toman el `deviceId` del tópico y descartan los mensajes cuyo cuerpo declare otro dispositivo.

La figura siguiente describe el flujo completo de un inicio de sesión y el acceso posterior a un recurso protegido.

**Figura 2.** *Secuencia de autenticación: inicio de sesión, acceso protegido y renovación de token.*

# 10. Estructura del proyecto

El proyecto se distribuye en ocho repositorios de código (API gateway, tres microservicios, backend, front web, app móvil e infraestructura) y los repositorios de documentación. La tabla siguiente resume su contenido y la forma de ejecutarlos.

| **Repositorio**            | **Tipo**                           | **Tecnología**                                          | **Punto de entrada**                                                 | **Ejecución**                                  |
|----------------------------|------------------------------------|---------------------------------------------------------|----------------------------------------------------------------------|------------------------------------------------|
| en-monitor-api-gateway     | API gateway                        | Java 21 + Spring Boot 4.0.8 + Spring Cloud Gateway + Maven | src/main/java/com/energymonitor/gateway/ApiGatewayApplication.java | ./mvnw spring-boot:run (puerto 8080)           |
| en-monitor-security-service | Microservicio de seguridad        | Java 21 + Spring Boot 4.1.1 + Maven                     | src/main/java/com/energymonitor/security/SecurityServiceApplication.java | ./mvnw spring-boot:run (puerto 8082)       |
| en-monitor-device-service  | Microservicio de dispositivos      | Java 21 + Spring Boot 4.1.1 + Maven                     | src/main/java/com/energymonitor/device/DeviceServiceApplication.java | ./mvnw spring-boot:run (puerto 8084)           |
| en-monitor-measurement-service | Microservicio de mediciones    | Java 21 + Spring Boot 4.1.1 + Maven                     | src/main/java/com/energymonitor/measurement/MeasurementServiceApplication.java | ./mvnw spring-boot:run (puerto 8085)   |
| energy-monitor-backend     | Backend (monolito modular) y firmware | Java 21 + Spring Boot 4.1.1 + Spring Modulith + Maven; firmware Arduino | src/main/java/com/energymonitor/EnergyMonitorBackendApplication.java y firmware/esp32-pzem-monitor/esp32-pzem-monitor.ino | ./mvnw spring-boot:run (puerto 8083) / arduino-cli upload |
| Energy-Monitor-application | Aplicación web (front-end)         | React 19 + Vite 8 (JavaScript)                          | src/main.jsx sobre index.html                                        | npm install / npm run dev (puerto 5173)        |
| Energy-Monitor-Mobile      | Aplicación móvil (front-end)       | Expo SDK 57 + React Native 0.86                         | src/app/\_layout.tsx (expo-router/entry)                             | npm install / npm run start                    |
| energy-monitor-infra       | Infraestructura y despliegue       | Docker Compose, Mosquitto 2, RabbitMQ 4.1, MySQL 8.4, Nginx | docker-compose.yml, docker-compose.dev.yml y docker-compose.prod.yml | docker compose up -d --build              |
| energy-monitor-docs-final  | Documentación final                | Markdown (ES y EN)                                      | README.md                                                            | No requiere ejecución                          |

El repositorio legado energy-monitor-devices (firmware y broker antiguos) ya no se usa para ejecutar nada: el firmware vive en `firmware/` de energy-monitor-backend y el broker en energy-monitor-infra.

## 10.1 Estructura del backend

```
energy-monitor-backend/
├── pom.xml                       definición Maven y dependencias
├── mvnw / mvnw.cmd               Maven Wrapper (no requiere Maven instalado)
├── Dockerfile                    imagen del backend (Temurin 21, usuario sin privilegios)
├── .env.example                  plantilla de variables de entorno
├── README.md                     instrucciones del backend
├── docs/Architecture.md          módulos, intermediarios y estado de la migración
├── firmware/esp32-pzem-monitor/  firmware del ESP32 + PZEM-004T V3
└── src/
    ├── main/
    │   ├── java/com/energymonitor/
    │   │   ├── EnergyMonitorBackendApplication.java   punto de entrada
    │   │   ├── home/             hogares, miembros, umbrales, eliminación y transferencia
    │   │   ├── alert/            alertas de consumo, límite, dispositivo y conectividad
    │   │   ├── recommendation/   reglas de ahorro y bandeja de recomendaciones
    │   │   ├── notification/     correo, Web Push, push móvil, preferencias y avisos de administrador
    │   │   ├── security/         intermediario del servicio de seguridad (sin dominio ni tablas)
    │   │   ├── device/           intermediario del servicio de dispositivos (sin dominio ni tablas)
    │   │   └── measurement/      intermediario del servicio de mediciones (sin dominio ni tablas)
    │   └── resources/
    │       ├── application.yaml                  configuración base
    │       ├── application-local.yaml(.example)  perfil local y claves
    │       └── db/changelog/                     maestro Liquibase + changesets
    │           ├── db.changelog-master.xml
    │           └── home/ alert/ recommendation/ notification/
    └── test/java/com/energymonitor/              96 archivos de prueba (incluye ModularityTests)
```

**Microservicios y gateway.** Cada microservicio conserva el paquete que tenía en el backend y sus changesets con la misma ruta relativa, de modo que Liquibase los reconoce como ya aplicados:

```
en-monitor-security-service/     com.energymonitor.security     db/changelog/security/
en-monitor-device-service/       com.energymonitor.device       db/changelog/device/
en-monitor-measurement-service/  com.energymonitor.measurement  db/changelog/measurement/
en-monitor-api-gateway/          com.energymonitor.gateway      (sin base de datos)
    cada uno con pom.xml, mvnw, Dockerfile, .env.example, README.md
    y .github/workflows/ci-cd.yml (docs/Architecture.md en gateway, seguridad y dispositivos)
```

**Intermediarios del backend (ejemplo con device):** `api/` (contrato que ya usaban los demás módulos: DeviceApi, DeviceConnectivityLost, DeviceConnectivityRestored), `adapter/out/remote/` (cliente HTTP del API interno del servicio, con caché) y `adapter/in/messaging/` (consumidor de los eventos de RabbitMQ), más `infrastructure/` con la configuración.

**Organización interna de cada módulo (ejemplo con home):**

```
home/
├── package-info.java        declara los límites del módulo (@ApplicationModule)
├── api/                     contrato público (HomeApi, HomeDeleted, HomeOwnershipTransferred…)
├── domain/model/            entidades puras de dominio (Home, UserHome, OwnerPermissions…)
├── application/
│   ├── port/in/             puertos de entrada (CreateHome, JoinHome, DeleteHome…)
│   ├── port/out/            puertos de salida (persistencia, generadores)
│   ├── usecase/             implementación de los casos de uso
│   ├── command/ result/     estructuras de entrada y salida
│   └── exception/           excepciones de aplicación
├── adapter/
│   ├── in/web/              controladores REST, DTOs y manejadores de error
│   ├── in/internal/         API interno para otros servicios (acceso, límites, borrado de cuenta)
│   ├── in/messaging/        consumidor de account.deleted (RabbitMQ)
│   ├── out/messaging/       outbox de home.deleted, relay y publicador RabbitMQ
│   └── out/persistence/     entidades JPA, repositorios, mappers y adaptadores
└── infrastructure/          configuración de Spring y API interna del módulo
```

## 10.2 Estructura de la aplicación web

```
Energy-Monitor-application/
├── index.html · package.json · vite.config.js
├── vitest.config.js · eslint.config.js · .env.example
├── public/ (iconos y recursos)
└── src/
    ├── main.jsx          monta los proveedores y App
    ├── App.jsx · index.css
    ├── routes/           AppRoutes, ProtectedRoute, HomeRoute
    ├── context/          HomeContext, ThemeContext, NotificationCenterContext
    ├── services/
    │   ├── http/         httpClient (axios), interceptores y errores
    │   ├── auth/         authApi, googleSignIn, session (localStorage, sin tokens)
    │   ├── home/         homeApi (hogares, miembros, umbrales, consumo)
    │   ├── devices/      deviceApi y bleProvisioning (Web Bluetooth)
    │   ├── alerts/       alertApi
    │   ├── recommendations/  recommendationApi
    │   └── notifications/    notificationApi y webPush
    ├── components/       layout/ (Navbar, MainLayout, Sidebar) y shared/
    ├── design/           design system (Button, Input, Card, Badge…)
    ├── features/         landingPage, auth, dashboard, DetailHome,
    │                     Favorites, Notifications, Account, SettingsPage
    ├── i18n/             locales es, en, fr y pt
    └── test/setup.js    configuración de Vitest
```

## 10.3 Estructura de la aplicación móvil

```
Energy-Monitor-Mobile/
├── app.json · package.json · tsconfig.json · AGENTS.md · .env.example
├── assets/               iconos y pantalla de inicio
├── scripts/reset-project.js
└── src/
    ├── app/              rutas Expo Router:
    │                     _layout.tsx (providers y Stack), index.tsx (login),
    │                     register.tsx, verify-account.tsx, recover-password.tsx,
    │                     verify-recover-password.tsx, new-password.tsx,
    │                     dashboard.tsx, homes.tsx, home/[id].tsx, favorites.tsx,
    │                     notifications.tsx, settings.tsx, account.tsx
    ├── components/       Button, Card, Input, Modal, Chart, OtpInput… + layout/
    ├── config/           env.ts (EXPO_PUBLIC_API_BASE_URL)
    ├── constants/        theme, colors, spacing, typography
    ├── context/          HomeContext, UserContext, ThemeContext, NotificationCenterContext
    ├── services/         http, auth, home, devices, measurement, alerts,
    │                     recommendations y notifications (cliente de la API)
    ├── features/         auth, dashboard, homes, favorites,
    │                     notifications, settings, account
    ├── i18n/             locales es, en, fr y pt
    └── validation/       mensajes i18n de los esquemas zod
```

## 10.4 Función de las carpetas principales

| **Carpeta**                                                                | **Función**                                                                              |
|----------------------------------------------------------------------------|------------------------------------------------------------------------------------------|
| \<servicio\>/src/main/java/com/energymonitor/\<módulo\>/domain/model       | Entidades de dominio puras, sin dependencias de Spring.                                  |
| \<servicio\>/src/main/java/com/energymonitor/\<módulo\>/application        | Casos de uso, puertos de entrada y de salida, comandos, resultados y excepciones.        |
| \<servicio\>/src/main/java/com/energymonitor/\<módulo\>/adapter/in/web     | Controladores REST, DTOs y manejadores de excepciones (@RestControllerAdvice).           |
| \<servicio\>/src/main/java/com/energymonitor/\<módulo\>/adapter/in/internal | API interno `/internal/**` para los demás servicios.                                    |
| \<servicio\>/src/main/java/com/energymonitor/\<módulo\>/adapter/in/messaging y out/messaging | Consumidores de RabbitMQ; outbox, relay y publicador de eventos.        |
| \<servicio\>/src/main/java/com/energymonitor/\<módulo\>/adapter/out/persistence | Entidades JPA, repositorios, mappers y adaptadores de persistencia.                 |
| energy-monitor-backend/src/main/java/com/energymonitor/{security,device,measurement} | Intermediarios de los microservicios: contrato api, cliente remoto y consumidores. |
| \<servicio\>/src/main/resources/db/changelog                               | Changesets de Liquibase de las tablas de ese servicio, con su rollback.                  |
| \<servicio\>/src/test/java                                                 | Pruebas unitarias, de integración web y de seguridad; en el backend, verificación de límites de módulo. |
| en-monitor-api-gateway/src/main/java/com/energymonitor/gateway             | Rutas (routing), validación del token y CORS (security), límite de peticiones (ratelimit) y errores (error). |
| web/src/routes                                                             | Definición de rutas, protecciones y layouts de la SPA.                                   |
| web/src/services                                                           | Capa de acceso a la API: cliente HTTP, interceptores, sesión y endpoints por dominio.    |
| web/src/features                                                           | Pantallas y componentes agrupados por funcionalidad.                                     |
| web/src/design                                                             | Componentes reutilizables de interfaz (botones, tarjetas, campos) y sus estilos.         |
| mobile/src/app                                                             | Rutas file-based de Expo Router que montan cada pantalla.                                |
| mobile/src/services                                                        | Cliente de la API REST de la aplicación móvil (mismo patrón que la web).                 |
| mobile/src/features                                                        | Pantallas, componentes, hooks y validaciones por funcionalidad.                          |
| energy-monitor-backend/firmware/esp32-pzem-monitor                         | Firmware del dispositivo y archivos de configuración.                                    |
| infra/docker-compose\*.yml y infra/mosquitto                               | Pila local, modo desarrollo y producción (MySQL, RabbitMQ, Mosquitto, servicios y web) y configuración del broker. |
| infra/deploy y infra/e2e                                                   | Despliegue (deploy.sh, Nginx, respaldo de MySQL, renovación del certificado MQTT) y pruebas de punta a punta. |

# 11. Base de datos

**Motor:** MySQL 8.4, ejecutado en contenedor Docker (repositorio energy-monitor-infra) con collation utf8mb4_unicode_ci.

**Esquema:** energy_monitor, una sola base compartida por los servicios (ADR-003). Este capítulo documenta 30 tablas de negocio distribuidas en siete contextos. El usuario de aplicación es energy_app.

**Dueño de cada contexto:** cada servicio es dueño de sus tablas y aplica solo sus changesets: el servicio de seguridad, las del contexto Security; el servicio de dispositivos, las de Devices; el servicio de mediciones, las de Monitoring and measurements; y energy-monitor-backend, las de Home Management, Alerts, Recommendations y Notifications.

**Gestión del esquema:** Liquibase aplica los changesets versionados en src/main/resources/db/changelog de cada servicio; todos comparten la tabla DATABASECHANGELOG, y los changesets que salieron del backend conservaron su ruta, por lo que no se vuelven a ejecutar. Hibernate está configurado con ddl-auto = validate, es decir, sólo valida que el esquema coincida con las entidades y nunca crea tablas por sí mismo.

**Convenciones:** nombres en snake_case; llaves primarias de tipo VARCHAR(10); columnas de auditoría created_at, updated_at y deleted_at en todas las tablas; borrado lógico (ningún DELETE físico por defecto); índices con el patrón idx\_\[tabla\]\_\[columnas\]. Las referencias entre contextos (y por tanto entre servicios) se almacenan como identificadores a nivel de aplicación y no generan llaves foráneas SQL.

## 11.1 Diagrama entidad-relación

El siguiente diagrama muestra las entidades del sistema, agrupadas por contexto (Security, Home Management, Devices, Monitoring and measurements, Alerts, Recommendations y Notifications), con sus atributos, llaves y relaciones. Las relaciones punteadas entre contextos representan referencias de aplicación (sin foreign key en SQL). El diagrama publicado en la carpeta de figuras muestra 27 entidades: faltan `notification_preference` y `push_subscription` y varias columnas posteriores (ver la nota de la sección 9.1).

> **Recomendación de lectura:** por su tamaño, el diagrama se presenta en página horizontal; en Word puede ampliarse con el zoom para leer los atributos de cada entidad. En el Anexo B se incluye el mismo modelo desagregado por contexto, con mayor legibilidad.
>
**Figura 3.** *Modelo entidad-relación (MER) completo del sistema «Monitor de Consumo Eléctrico».*

## 11.2 Tablas principales

La tabla siguiente lista las 30 tablas de negocio documentadas, el contexto al que pertenecen (y, con él, el servicio dueño indicado en la introducción de esta sección), su descripción, su llave primaria y las relaciones declaradas en el diccionario de datos.

| **Tabla**              | **Contexto**                | **Descripción**                                                                                                                                                                                                    | **Llave primaria**   | **Relaciones**                                                                                                                                                          |
|------------------------|-----------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Person                 | Security                    | Registra los datos personales asociados a un usuario.                                                                                                                                                              | —                    | Person 1:1 User                                                                                                                                                         |
| User                   | Security                    | Gestiona las credenciales, estado y datos de acceso del usuario.                                                                                                                                                   | —                    | User 1:1 Person; User 1:1 UserConfiguration; User 1:N PasswordResetToken; User 1:N UserSession; User 1:N UserSystemRole; User 1:N AuditLog                              |
| UserConfiguration      | Security                    | Almacena las preferencias de configuración de cada usuario.                                                                                                                                                        | —                    | UserConfiguration 1:1 User                                                                                                                                              |
| PasswordResetToken     | Security                    | Almacena tokens para recuperación de contraseña.                                                                                                                                                                   | —                    | PasswordResetToken N:1 User                                                                                                                                             |
| RefreshToken           | Security                    | Cada fila es una generación de refresh token. Solo se guarda el hash SHA-256 del secreto, nunca el secreto en claro. family_id agrupa todas las generaciones de un mismo login y parent_id encadena la genealogía. | id_refresh_token     | RefreshToken N:1 UserSession; RefreshToken 0..1:1 RefreshToken (parent_id)                                                                                              |
| UserSession            | Security                    | Registra las sesiones activas y renovables del usuario.                                                                                                                                                            | —                    | UserSession N:1 User; UserSession 1:N RefreshToken                                                                                                                      |
| SecurityConfiguration  | Security                    | Define parámetros configurables de seguridad del sistema.                                                                                                                                                          | —                    | Sin relaciones registradas                                                                                                                                              |
| PasswordPolicy         | Security                    | Define las reglas de validación y expiración de contraseñas.                                                                                                                                                       | —                    | Sin relaciones registradas                                                                                                                                              |
| LoginErrorLog          | Security                    | Registra los errores producidos durante el inicio de sesión.                                                                                                                                                       | —                    | LoginErrorLog N:1 User                                                                                                                                                  |
| SystemRole             | Security                    | Define los roles disponibles en el sistema.                                                                                                                                                                        | —                    | SystemRole 1:N UserSystemRole; SystemRole 1:N SystemRolePermission                                                                                                      |
| UserSystemRole         | Security                    | Relaciona usuarios con roles del sistema.                                                                                                                                                                          | —                    | UserSystemRole N:1 User; UserSystemRole N:1 SystemRole                                                                                                                  |
| Permission             | Security                    | Define los permisos funcionales del sistema.                                                                                                                                                                       | —                    | Permission 1:N SystemRolePermission                                                                                                                                     |
| SystemRolePermission   | Security                    | Relaciona roles con permisos.                                                                                                                                                                                      | —                    | SystemRolePermission N:1 SystemRole; SystemRolePermission N:1 Permission                                                                                                |
| AuditLog               | Security                    | Registra las acciones relevantes ejecutadas por los usuarios.                                                                                                                                                      | —                    | AuditLog N:1 User                                                                                                                                                       |
| HomeType               | Home Management             | Catálogo de tipos de hogar.                                                                                                                                                                                        | —                    | HomeType 1:N Home                                                                                                                                                       |
| Home                   | Home Management             | Registra los hogares administrados por el sistema.                                                                                                                                                                 | —                    | Home N:1 HomeType; Home 1:1 HomeThresholds; Home 1:N UserHome; Home 1:N DeviceHome; Home 1:N Alert; Home 1:N Recommendation                                             |
| HomeThresholds         | Home Management             | Configura los límites de consumo de un hogar.                                                                                                                                                                      | —                    | HomeThresholds 1:1 Home                                                                                                                                                 |
| UserHome               | Home Management             | Relaciona usuarios con hogares y define su rol.                                                                                                                                                                    | —                    | UserHome N:1 User; UserHome N:1 Home                                                                                                                                    |
| ApplianceType          | Devices                     | Catálogo de tipos de electrodomésticos.                                                                                                                                                                            | —                    | ApplianceType 1:N Device                                                                                                                                                |
| Device                 | Devices                     | Registra los dispositivos de medición instalados.                                                                                                                                                                  | —                    | Device N:1 ApplianceType; Device 1:N DeviceStatusLog; Device 1:N DeviceHome; Device 1:N Measurement; Device 1:N Alert; Device 1:N Recommendation                        |
| DeviceStatusLog        | Devices                     | Registra el estado y conectividad de los dispositivos.                                                                                                                                                             | —                    | DeviceStatusLog N:1 Device                                                                                                                                              |
| DeviceHome             | Devices                     | Relaciona dispositivos con hogares.                                                                                                                                                                                | —                    | DeviceHome N:1 Device; DeviceHome N:1 Home                                                                                                                              |
| Measurement            | Monitoring and measurements | Almacena las mediciones eléctricas registradas.                                                                                                                                                                    | —                    | Measurement N:1 Device                                                                                                                                                  |
| ConsumptionLevel       | Monitoring and measurements | Define niveles de riesgo y sus rangos de consumo.                                                                                                                                                                  | —                    | ConsumptionLevel 1:N Alert                                                                                                                                              |
| Alert                  | Alerts                      | Registra alertas de umbral y conectividad.                                                                                                                                                                         | —                    | Alert N:1 Home; Alert N:1 Device; Alert N:1 ConsumptionLevel; Alert N:1 Measurement                                                                                     |
| Recommendation         | Recommendations             | Registra recomendaciones para los hogares y dispositivos.                                                                                                                                                          | —                    | Recommendation N:1 Home; Recommendation N:1 Device                                                                                                                      |
| Notification           | Notifications               | Registro de entrega de un mensaje al usuario. Alert y Recommendation son eventos; Notification registra si el mensaje fue entregado, por qué canal y su estado de lectura.                                         | —                    | Notification N:1 User (referencia aplicación); Notification N:1 Home (referencia aplicación); Notification referencia Alert/Recommendation por (source_type, source_id) |
| NotificationPreference | Notifications               | Preferencias de canal de notificación de cada usuario (correo y push).                                                                                                                                             | user_id              | NotificationPreference 1:1 User (referencia aplicación)                                                                                                                 |
| PushSubscription       | Notifications               | Suscripciones de notificaciones push de los navegadores del usuario.                                                                                                                                               | id_push_subscription | PushSubscription N:1 User (referencia aplicación)                                                                                                                       |
| PushDevice             | Notifications               | Instalaciones de la app móvil que reciben push por el relay de Expo, una fila por instalación. | id_push_device | PushDevice N:1 User (referencia aplicación) |

## 11.3 Diccionario de datos

El diccionario de datos describe campo por campo cada tabla del esquema: restricción de clave, tipo de dato, tamaño y descripción. Se reconstruyó a partir de los changesets de Liquibase de los servicios, que son la fuente de verdad del esquema; para el modelo de datos con sus reglas de integridad véase también [`05.2-modelo-de-datos.md`](../../05-diseno-software/ES/05.2-modelo-de-datos.md).

**Leyenda de restricciones:** PK = llave primaria; FK = llave foránea; UNIQUE = valor único; NOT NULL = campo obligatorio; ENUM = conjunto de valores permitidos (se listan en la columna Tamaño). Las columnas de auditoría (created_at, updated_at y deleted_at) se repiten en todas las tablas y no se incluyen en cada diccionario.

### Person — Security

*Registra los datos personales asociados a un usuario.*

| **Campo** | **Tipo** | **Tamaño** | **Restricción** | **Descripción**                    |
|-----------|----------|------------|-----------------|------------------------------------|
| id_person | VARCHAR  | 10         | PK / NOT NULL   | Identificador de la persona        |
| name      | VARCHAR  | 100        | NOT NULL        | Nombres de la persona              |
| last_name | VARCHAR  | 100        | —               | Apellidos de la persona (opcional) |

**Relaciones:** Person 1:1 User

### User — Security

*Gestiona las credenciales, estado y datos de acceso del usuario.*

| **Campo**             | **Tipo**   | **Tamaño**                | **Restricción**        | **Descripción**                                                                                                      |
|-----------------------|------------|---------------------------|------------------------|----------------------------------------------------------------------------------------------------------------------|
| id_user               | VARCHAR    | 10                        | PK / NOT NULL          | Identificador del usuario                                                                                            |
| person_id             | VARCHAR    | 10                        | FK / UNIQUE / NOT NULL | Persona asociada                                                                                                     |
| password_hash         | VARCHAR    | 255                       | NOT NULL               | Hash de la contraseña                                                                                                |
| email                 | VARCHAR    | 255                       | UNIQUE / NOT NULL      | Correo electrónico                                                                                                   |
| email_verified        | BOOLEAN    |                           | NOT NULL               | Indica si el correo fue verificado                                                                                   |
| registration_date     | TIMESTAMP  |                           | NOT NULL               | Fecha y hora de registro                                                                                             |
| status                | ENUM       | ACTIVE, INACTIVE, BLOCKED | NOT NULL               | Estado de la cuenta                                                                                                  |
| failed_login_attempts | INT        |                           | NOT NULL               | Cantidad de intentos fallidos                                                                                        |
| last_login_at         | TIMESTAMP  |                           | —                      | Último inicio de sesión                                                                                              |
| profile_image         | MEDIUMTEXT |                           | —                      | Imagen de perfil de la cuenta (URL de datos en base64)                                                               |
| google_subject        | VARCHAR    | 255                       | UNIQUE                 | Identificador estable (sub) de la cuenta de Google con la que se creó la cuenta; vacío si se registró con contraseña |

**Relaciones:** User 1:1 Person • User 1:1 UserConfiguration • User 1:N PasswordResetToken • User 1:N UserSession • User 1:N UserSystemRole • User 1:N AuditLog

### UserConfiguration — Security

*Almacena las preferencias de configuración de cada usuario.*

| **Campo**        | **Tipo** | **Tamaño** | **Restricción**        | **Descripción**                   |
|------------------|----------|------------|------------------------|-----------------------------------|
| id_configuration | VARCHAR  | 10         | PK / NOT NULL          | Identificador de la configuración |
| user_id          | VARCHAR  | 10         | FK / UNIQUE / NOT NULL | Usuario configurado               |
| notify_by_email  | BOOLEAN  |            | NOT NULL               | Activa notificaciones por correo  |
| notify_by_push   | BOOLEAN  |            | NOT NULL               | Activa notificaciones push        |
| color_theme      | VARCHAR  | 50         | —                      | Tema de color                     |
| language         | VARCHAR  | 50         | —                      | Idioma preferido                  |
| social_provider  | VARCHAR  | 50         | —                      | Proveedor de autenticación social |

**Relaciones:** UserConfiguration 1:1 User

### PasswordResetToken — Security

*Almacena tokens para recuperación de contraseña.*

| **Campo**        | **Tipo**  | **Tamaño** | **Restricción**   | **Descripción**                                          |
|------------------|-----------|------------|-------------------|----------------------------------------------------------|
| id_reset_token   | VARCHAR   | 10         | PK / NOT NULL     | Identificador del token                                  |
| user_id          | VARCHAR   | 10         | FK / NOT NULL     | Usuario propietario                                      |
| reset_token_hash | VARCHAR   | 64         | UNIQUE / NOT NULL | Hash HMAC-SHA256 (con pepper) del código de recuperación |
| used             | BOOLEAN   |            | NOT NULL          | Indica si el token fue utilizado                         |
| expiration_at    | TIMESTAMP |            | NOT NULL          | Fecha de expiración                                      |
| attempts         | INT       |            | NOT NULL          | Intentos de canje del código                             |

**Relaciones:** PasswordResetToken N:1 User

### UserSession — Security

*Registra las sesiones activas y renovables del usuario.*

| **Campo**       | **Tipo**  | **Tamaño**                                            | **Restricción** | **Descripción**                         |
|-----------------|-----------|-------------------------------------------------------|-----------------|-----------------------------------------|
| id_user_session | VARCHAR   | 10                                                    | PK / NOT NULL   | Identificador de sesión                 |
| user_id         | VARCHAR   | 10                                                    | FK / NOT NULL   | Usuario de la sesión                    |
| revoked         | BOOLEAN   |                                                       | NOT NULL        | Indica si la sesión fue revocada        |
| ip_address      | VARCHAR   | 45                                                    | —               | Dirección IP                            |
| user_agent      | VARCHAR   | 150                                                   | —               | Agente del cliente                      |
| expiration_at   | TIMESTAMP |                                                       | NOT NULL        | Fecha de expiración                     |
| closed_at       | TIMESTAMP |                                                       | —               | Fecha de cierre                         |
| revoked_at      | TIMESTAMP |                                                       | —               | Fecha y hora de revocación de la sesión |
| revoked_reason  | ENUM      | REFRESH_TOKEN_REUSE, PASSWORD_CHANGED, ADMINISTRATIVE | —               | Motivo de la revocación                 |

**Relaciones:** UserSession N:1 User • UserSession 1:N RefreshToken

### SecurityConfiguration — Security

*Define parámetros configurables de seguridad del sistema.*

| **Campo**                 | **Tipo** | **Tamaño** | **Restricción**   | **Descripción**                |
|---------------------------|----------|------------|-------------------|--------------------------------|
| id_security_configuration | VARCHAR  | 10         | PK / NOT NULL     | Identificador de configuración |
| config_name               | VARCHAR  | 100        | UNIQUE / NOT NULL | Nombre de la configuración     |
| config_value              | VARCHAR  | 100        | NOT NULL          | Valor de la configuración      |
| description               | VARCHAR  | 255        | —                 | Descripción                    |

**Relaciones:** UserSession N:1 User • UserSession 1:N RefreshToken

### PasswordPolicy — Security

*Define las reglas de validación y expiración de contraseñas.*

| **Campo**          | **Tipo** | **Tamaño** | **Restricción** | **Descripción**           |
|--------------------|----------|------------|-----------------|---------------------------|
| id_password_policy | VARCHAR  | 10         | PK / NOT NULL   | Identificador de política |
| min_length         | INT      |            | NOT NULL        | Longitud mínima           |
| max_length         | INT      |            | NOT NULL        | Longitud máxima           |
| require_uppercase  | BOOLEAN  |            | NOT NULL        | Exige mayúsculas          |
| require_numbers    | BOOLEAN  |            | NOT NULL        | Exige números             |
| require_symbols    | BOOLEAN  |            | NOT NULL        | Exige símbolos            |
| expiration_days    | INT      |            | NOT NULL        | Días de vigencia          |

**Relaciones:** UserSession N:1 User • UserSession 1:N RefreshToken

### LoginErrorLog — Security

*Registra los errores producidos durante el inicio de sesión.*

| **Campo**      | **Tipo** | **Tamaño**                                                          | **Restricción** | **Descripción**                |
|----------------|----------|---------------------------------------------------------------------|-----------------|--------------------------------|
| id_login_error | VARCHAR  | 10                                                                  | PK / NOT NULL   | Identificador del error        |
| user_id        | VARCHAR  | 10                                                                  | FK              | Usuario relacionado, si existe |
| error_type     | ENUM     | INVALID_PASSWORD, USER_NOT_FOUND, ACCOUNT_BLOCKED, ACCOUNT_INACTIVE | NOT NULL        | Tipo de error                  |
| description    | VARCHAR  | 500                                                                 | —               | Descripción del error          |
| ip_address     | VARCHAR  | 45                                                                  | —               | Dirección IP                   |

**Relaciones:** LoginErrorLog N:1 User

### SystemRole — Security

*Define los roles disponibles en el sistema.*

| **Campo**      | **Tipo** | **Tamaño** | **Restricción**   | **Descripción**           |
|----------------|----------|------------|-------------------|---------------------------|
| id_system_role | VARCHAR  | 10         | PK / NOT NULL     | Identificador del rol     |
| name           | VARCHAR  | 50         | UNIQUE / NOT NULL | Nombre del rol            |
| description    | VARCHAR  | 200        | —                 | Descripción               |
| enabled        | BOOLEAN  |            | NOT NULL          | Indica si está habilitado |

**Relaciones:** SystemRole 1:N UserSystemRole • SystemRole 1:N SystemRolePermission

### Permission — Security

*Define los permisos funcionales del sistema.*

| **Campo**     | **Tipo** | **Tamaño** | **Restricción**   | **Descripción**           |
|---------------|----------|------------|-------------------|---------------------------|
| id_permission | VARCHAR  | 10         | PK / NOT NULL     | Identificador del permiso |
| code          | VARCHAR  | 30         | UNIQUE / NOT NULL | Código único              |
| name          | VARCHAR  | 50         | NOT NULL          | Nombre del permiso        |
| description   | VARCHAR  | 100        | —                 | Descripción               |

**Relaciones:** Permission 1:N SystemRolePermission

### UserSystemRole — Security

*Relaciona usuarios con roles del sistema.*

| **Campo**      | **Tipo**  | **Tamaño** | **Restricción** | **Descripción**     |
|----------------|-----------|------------|-----------------|---------------------|
| user_id        | VARCHAR   | 10         | FK / NOT NULL   | Usuario relacionado |
| system_role_id | VARCHAR   | 10         | FK / NOT NULL   | Rol asignado        |
| assigned_at    | TIMESTAMP |            | NOT NULL        | Fecha de asignación |

**Relaciones:** UserSystemRole N:1 User • UserSystemRole N:1 SystemRole

### SystemRolePermission — Security

*Relaciona roles con permisos.*

| **Campo**      | **Tipo**  | **Tamaño** | **Restricción** | **Descripción**     |
|----------------|-----------|------------|-----------------|---------------------|
| system_role_id | VARCHAR   | 10         | FK / NOT NULL   | Rol relacionado     |
| permission_id  | VARCHAR   | 10         | FK / NOT NULL   | Permiso asignado    |
| assigned_at    | TIMESTAMP |            | NOT NULL        | Fecha de asignación |

**Relaciones:** SystemRolePermission N:1 SystemRole • SystemRolePermission N:1 Permission

### AuditLog — Security

*Registra las acciones relevantes ejecutadas por los usuarios.*

| **Campo**    | **Tipo** | **Tamaño**                                          | **Restricción** | **Descripción**               |
|--------------|----------|-----------------------------------------------------|-----------------|-------------------------------|
| id_audit_log | VARCHAR  | 10                                                  | PK / NOT NULL   | Identificador del registro    |
| user_id      | VARCHAR  | 10                                                  | FK              | Usuario que ejecutó la acción |
| action       | ENUM     | CREATE, UPDATE, DELETE, LOGIN, LOGIN_FAILED, LOGOUT | NOT NULL        | Acción auditada               |
| description  | VARCHAR  | 255                                                 | —               | Descripción                   |
| ip_address   | VARCHAR  | 45                                                  | —               | Dirección IP                  |
| application  | VARCHAR  | 255                                                 | —               | Aplicación de origen          |

**Relaciones:** AuditLog N:1 User

### RefreshToken — Security

*Cada fila es una generación de refresh token. Solo se guarda el hash SHA-256 del secreto, nunca el secreto en claro. family_id agrupa todas las generaciones de un mismo login y parent_id encadena la genealogía.*

| **Campo**        | **Tipo**  | **Tamaño**                                            | **Restricción**   | **Descripción**                                        |
|------------------|-----------|-------------------------------------------------------|-------------------|--------------------------------------------------------|
| id_refresh_token | VARCHAR   | 10                                                    | PK / NOT NULL     | Identificador de la generación del token               |
| id_user_session  | VARCHAR   | 10                                                    | FK / NOT NULL     | Sesión a la que pertenece la generación                |
| family_id        | VARCHAR   | 10                                                    | NOT NULL          | Identificador de la raíz de la familia de generaciones |
| parent_id        | VARCHAR   | 10                                                    | FK                | Generación que esta reemplaza                          |
| token_hash       | VARCHAR   | 64                                                    | UNIQUE / NOT NULL | Hash SHA-256 del secreto (nunca el secreto en claro)   |
| status           | ENUM      | ACTIVE, ROTATED, REVOKED, EXPIRED                     | NOT NULL          | Estado del ciclo de vida de la generación              |
| expires_at       | TIMESTAMP |                                                       | NOT NULL          | Fecha de expiración                                    |
| rotated_at       | TIMESTAMP |                                                       | —                 | Fecha en que fue reemplazada                           |
| revoked_at       | TIMESTAMP |                                                       | —                 | Fecha de revocación                                    |
| revoked_reason   | ENUM      | REFRESH_TOKEN_REUSE, PASSWORD_CHANGED, ADMINISTRATIVE | —                 | Motivo de la revocación                                |

**Relaciones:** RefreshToken N:1 UserSession • RefreshToken 0..1:1 RefreshToken (parent_id)

### HomeType — Home Management

*Catálogo de tipos de hogar.*

| **Campo**    | **Tipo** | **Tamaño** | **Restricción**   | **Descripción**          |
|--------------|----------|------------|-------------------|--------------------------|
| id_home_type | VARCHAR  | 10         | PK / NOT NULL     | Identificador del tipo   |
| name         | VARCHAR  | 50         | UNIQUE / NOT NULL | Nombre del tipo de hogar |

**Relaciones:** HomeType 1:N Home

### Home — Home Management

*Registra los hogares administrados por el sistema.*

| **Campo**     | **Tipo**  | **Tamaño** | **Restricción**   | **Descripción**   |
|---------------|-----------|------------|-------------------|-------------------|
| id_home       | VARCHAR   | 10         | PK / NOT NULL     | Código del hogar  |
| name          | VARCHAR   | 50         | NOT NULL          | Nombre del hogar  |
| home_type_id  | VARCHAR   | 10         | FK / NOT NULL     | Tipo de hogar     |
| address       | VARCHAR   | 200        | NOT NULL          | Dirección         |
| access_code   | VARCHAR   | 8          | UNIQUE / NOT NULL | Código de acceso  |
| description   | VARCHAR   | 200        | —                 | Descripción       |
| creation_date | TIMESTAMP |            | NOT NULL          | Fecha de creación |

**Relaciones:** Home N:1 HomeType • Home 1:1 HomeThresholds • Home 1:N UserHome • Home 1:N DeviceHome • Home 1:N Alert • Home 1:N Recommendation

### HomeThresholds — Home Management

*Configura los límites de consumo de un hogar.*

| **Campo**          | **Tipo** | **Tamaño**     | **Restricción**        | **Descripción**                                  |
|--------------------|----------|----------------|------------------------|--------------------------------------------------|
| id_threshold       | VARCHAR  | 10             | PK / NOT NULL          | Identificador del umbral                         |
| home_id            | VARCHAR  | 10             | FK / UNIQUE / NOT NULL | Hogar configurado                                |
| daily_limit        | DOUBLE   |                | NOT NULL               | Límite diario de consumo                         |
| monthly_limit      | DOUBLE   |                | NOT NULL               | Límite mensual de consumo                        |
| use_system_default | BOOLEAN  |                | NOT NULL               | Usa límites predeterminados                      |
| limit_period       | ENUM     | DAILY, MONTHLY | NOT NULL               | Límite que fijó el propietario (DAILY o MONTHLY) |

**Relaciones:** HomeThresholds 1:1 Home

### UserHome — Home Management

*Relaciona usuarios con hogares y define su rol.*

| **Campo** | **Tipo** | **Tamaño**    | **Restricción** | **Descripción**                |
|-----------|----------|---------------|-----------------|--------------------------------|
| user_id   | VARCHAR  | 10            | NOT NULL        | Usuario relacionado            |
| home_id   | VARCHAR  | 10            | FK / NOT NULL   | Hogar relacionado              |
| role      | ENUM     | OWNER, MEMBER | NOT NULL        | Rol del usuario en el hogar    |
| favorite  | BOOLEAN  |               | NOT NULL        | Indica si es el hogar favorito |

**Relaciones:** UserHome N:1 User • UserHome N:1 Home

### ApplianceType — Devices

*Catálogo de tipos de electrodomésticos.*

| **Campo**         | **Tipo** | **Tamaño** | **Restricción**   | **Descripción**        |
|-------------------|----------|------------|-------------------|------------------------|
| id_appliance_type | VARCHAR  | 10         | PK / NOT NULL     | Identificador del tipo |
| name              | VARCHAR  | 50         | UNIQUE / NOT NULL | Nombre del tipo        |

**Relaciones:** ApplianceType 1:N Device

### Device — Devices

*Registra los dispositivos de medición instalados.*

| **Campo**         | **Tipo**  | **Tamaño** | **Restricción**   | **Descripción**                                                          |
|-------------------|-----------|------------|-------------------|--------------------------------------------------------------------------|
| id_device         | VARCHAR   | 10         | PK / NOT NULL     | Identificador del dispositivo                                            |
| name              | VARCHAR   | 50         | NOT NULL          | Nombre                                                                   |
| appliance_type_id | VARCHAR   | 10         | FK / NOT NULL     | Tipo de electrodoméstico                                                 |
| location          | VARCHAR   | 50         | —                 | Ubicación                                                                |
| description       | VARCHAR   | 200        | —                 | Descripción                                                              |
| installation_date | TIMESTAMP |            | NOT NULL          | Fecha de instalación                                                     |
| device_code       | VARCHAR   | 6          | UNIQUE / NOT NULL | Código del dispositivo                                                   |
| api_key_hash      | VARCHAR   | 64         | NOT NULL          | Hash SHA-256 de la clave de integración (la clave no se guarda en claro) |

**Relaciones:** Device N:1 ApplianceType • Device 1:N DeviceStatusLog • Device 1:N DeviceHome • Device 1:N Measurement • Device 1:N Alert • Device 1:N Recommendation

### DeviceStatusLog — Devices

*Registra el estado y conectividad de los dispositivos.*

| **Campo**        | **Tipo**  | **Tamaño**      | **Restricción** | **Descripción**            |
|------------------|-----------|-----------------|-----------------|----------------------------|
| id_device_status | VARCHAR   | 10              | PK / NOT NULL   | Identificador del registro |
| device_id        | VARCHAR   | 10              | FK              | Dispositivo relacionado    |
| status           | ENUM      | ONLINE, OFFLINE | NOT NULL        | Estado del dispositivo     |
| signal_strength  | INT       |                 | —               | Intensidad de señal        |
| last_seen        | TIMESTAMP |                 | NOT NULL        | Última fecha de conexión   |

**Relaciones:** DeviceStatusLog N:1 Device

### DeviceHome — Devices

*Relaciona dispositivos con hogares.*

| **Campo** | **Tipo** | **Tamaño** | **Restricción** | **Descripción**         |
|-----------|----------|------------|-----------------|-------------------------|
| device_id | VARCHAR  | 10         | FK / NOT NULL   | Dispositivo relacionado |
| home_id   | VARCHAR  | 10         | NOT NULL        | Hogar relacionado       |

**Relaciones:** DeviceHome N:1 Device • DeviceHome N:1 Home

### Measurement — Monitoring and measurements

*Almacena las mediciones eléctricas registradas.*

| **Campo**      | **Tipo**  | **Tamaño** | **Restricción** | **Descripción**                    |
|----------------|-----------|------------|-----------------|------------------------------------|
| id_measurement | VARCHAR   | 10         | PK / NOT NULL   | Identificador de la medición       |
| device_id      | VARCHAR   | 10         | NOT NULL        | Dispositivo que genera la medición |
| date_time      | TIMESTAMP |            | NOT NULL        | Fecha y hora de la medición        |
| voltage        | DOUBLE    |            | NOT NULL        | Voltaje medido                     |
| current        | DOUBLE    |            | NOT NULL        | Corriente medida                   |
| active_power   | DOUBLE    |            | NOT NULL        | Potencia activa                    |
| stored_energy  | DOUBLE    |            | NOT NULL        | Energía acumulada                  |

**Relaciones:** Measurement N:1 Device

### ConsumptionLevel — Monitoring and measurements

*Define niveles de riesgo y sus rangos de consumo.*

| **Campo**            | **Tipo** | **Tamaño**                  | **Restricción**   | **Descripción**         |
|----------------------|----------|-----------------------------|-------------------|-------------------------|
| id_consumption_level | VARCHAR  | 10                          | PK / NOT NULL     | Identificador del nivel |
| name                 | ENUM     | LOW, MEDIUM, HIGH, CRITICAL | UNIQUE / NOT NULL | Nivel de riesgo         |
| description          | VARCHAR  | 200                         | NOT NULL          | Descripción             |
| min_limit            | DOUBLE   |                             | NOT NULL          | Límite inferior         |
| max_limit            | DOUBLE   |                             | NOT NULL          | Límite superior         |

**Relaciones:** ConsumptionLevel 1:N Alert

### Alert — Alerts

*Registra alertas de umbral y conectividad.*

| **Campo**            | **Tipo**  | **Tamaño**                             | **Restricción** | **Descripción**            |
|----------------------|-----------|----------------------------------------|-----------------|----------------------------|
| id_alert             | VARCHAR   | 10                                     | PK / NOT NULL   | Identificador de la alerta |
| home_id              | VARCHAR   | 10                                     | NOT NULL        | Hogar al que aplica        |
| device_id            | VARCHAR   | 10                                     | —               | Dispositivo relacionado    |
| type                 | ENUM      | THRESHOLD, CONNECTIVITY, DEVICE, LIMIT | NOT NULL        | Tipo de alerta             |
| message_key          | VARCHAR   | 100                                    | NOT NULL        | Clave del mensaje          |
| date_time            | TIMESTAMP |                                        | NOT NULL        | Fecha y hora               |
| alert_status         | ENUM      | PENDING, RESOLVED                      | NOT NULL        | Estado de la alerta        |
| consumption_level_id | VARCHAR   | 10                                     | —               | Nivel de consumo asociado  |
| measurement_id       | VARCHAR   | 10                                     | —               | Medición asociada          |

**Relaciones:** Alert N:1 Home • Alert N:1 Device • Alert N:1 ConsumptionLevel • Alert N:1 Measurement

### Recommendation — Recommendations

*Registra recomendaciones para los hogares y dispositivos.*

| **Campo**         | **Tipo**  | **Tamaño**                                                             | **Restricción** | **Descripción**                   |
|-------------------|-----------|------------------------------------------------------------------------|-----------------|-----------------------------------|
| id_recommendation | VARCHAR   | 10                                                                     | PK / NOT NULL   | Identificador de la recomendación |
| home_id           | VARCHAR   | 10                                                                     | NOT NULL        | Hogar al que aplica               |
| device_id         | VARCHAR   | 10                                                                     | —               | Dispositivo relacionado           |
| type              | ENUM      | SAVING, PEAK_HOURS, HIGH_CONSUMPTION, HISTORICAL_COMPARISON, THRESHOLD | NOT NULL        | Tipo de recomendación             |
| message_key       | VARCHAR   | 100                                                                    | NOT NULL        | Clave del mensaje                 |
| date_time         | TIMESTAMP |                                                                        | NOT NULL        | Fecha y hora                      |
| status            | ENUM      | READ, UNREAD                                                           | NOT NULL        | Estado de lectura                 |

**Relaciones:** Recommendation N:1 Home • Recommendation N:1 Device

### Notification — Notifications

*Registro de entrega de un mensaje al usuario. Alert y Recommendation son eventos; Notification registra si el mensaje fue entregado, por qué canal y su estado de lectura.*

| **Campo**       | **Tipo**  | **Tamaño**                      | **Restricción** | **Descripción**                  |
|-----------------|-----------|---------------------------------|-----------------|----------------------------------|
| id_notification | VARCHAR   | 10                              | PK / NOT NULL   | Identificador de la notificación |
| user_id         | VARCHAR   | 10                              | NOT NULL        | Usuario destinatario             |
| home_id         | VARCHAR   | 10                              | —               | Hogar relacionado                |
| channel         | ENUM      | EMAIL, PUSH                     | NOT NULL        | Canal de entrega                 |
| source_type     | ENUM      | ALERT, RECOMMENDATION, SECURITY | NOT NULL        | Origen del mensaje               |
| source_id       | VARCHAR   | 10                              | —               | Identificador del origen         |
| message_key     | VARCHAR   | 100                             | NOT NULL        | Clave del mensaje                |
| delivery_status | ENUM      | PENDING, SENT, FAILED           | NOT NULL        | Estado de la entrega             |
| sent_at         | TIMESTAMP |                                 | —               | Fecha de envío                   |
| read_at         | TIMESTAMP |                                 | —               | Fecha de lectura                 |
| failure_reason  | VARCHAR   | 200                             | —               | Motivo del fallo de entrega      |

**Relaciones:** Notification N:1 User (referencia aplicación) • Notification N:1 Home (referencia aplicación) • Notification referencia Alert/Recommendation por (source_type, source_id)

### NotificationPreference — Notifications

*Preferencias de canal de notificación de cada usuario (correo y push).*

| **Campo**     | **Tipo** | **Tamaño** | **Restricción** | **Descripción**                                    |
|---------------|----------|------------|-----------------|----------------------------------------------------|
| user_id       | VARCHAR  | 10         | PK / NOT NULL   | Usuario dueño de las preferencias (clave primaria) |
| email_enabled | BOOLEAN  |            | NOT NULL        | Entrega de alertas y recomendaciones por correo    |
| push_enabled  | BOOLEAN  |            | NOT NULL        | Entrega por notificación del navegador             |

**Relaciones:** NotificationPreference 1:1 User (referencia aplicación)

### PushSubscription — Notifications

*Suscripciones de notificaciones push de los navegadores del usuario.*

| **Campo**            | **Tipo** | **Tamaño** | **Restricción**   | **Descripción**                                   |
|----------------------|----------|------------|-------------------|---------------------------------------------------|
| id_push_subscription | VARCHAR  | 10         | PK / NOT NULL     | Identificador de la suscripción                   |
| user_id              | VARCHAR  | 10         | NOT NULL          | Usuario suscrito                                  |
| endpoint             | VARCHAR  | 500        | UNIQUE / NOT NULL | Dirección del servicio push del navegador (única) |
| p256dh               | VARCHAR  | 100        | NOT NULL          | Clave pública de cifrado de la suscripción        |
| auth                 | VARCHAR  | 50         | NOT NULL          | Secreto de autenticación de la suscripción        |

**Relaciones:** PushSubscription N:1 User (referencia aplicación)

### PushDevice — Notifications

*Instalaciones de la app móvil que reciben notificaciones push por el relay de Expo (una fila por instalación). No es otro tipo de PushSubscription: el token del relay no lleva endpoint ni claves de cifrado.*

| **Campo**      | **Tipo** | **Tamaño** | **Restricción**   | **Descripción**                                                          |
|----------------|----------|------------|-------------------|--------------------------------------------------------------------------|
| id_push_device | VARCHAR  | 10         | PK / NOT NULL     | Identificador de la instalación                                          |
| user_id        | VARCHAR  | 10         | NOT NULL          | Usuario dueño de la instalación (con índice)                             |
| token          | VARCHAR  | 255        | UNIQUE / NOT NULL | Token de push de la instalación; si se registra otra vez, cambia de dueño |
| platform       | VARCHAR  | 10         | NOT NULL          | Plataforma de la instalación (la app publicada es Android: ANDROID)      |

**Relaciones:** PushDevice N:1 User (referencia aplicación)


# 12. Módulos técnicos

## 12.1 Vista general de los módulos

El backend del sistema conserva sus siete módulos (bounded contexts), todos activos, pero ya no en una sola aplicación: security, device y measurement se ejecutan como microservicios, y home, alert, recommendation y notification siguen en energy-monitor-backend, donde además quedan los paquetes intermediarios security, device y measurement (sin dominio ni tablas). La migración de los cuatro módulos que quedan en el backend a microservicios está en curso (sección 16.4). El API gateway completa la tabla como componente técnico sin dominio.

| **Módulo**     | **Responsabilidad**                                                                    | **Controladores / casos de uso**                                                                                          | **Estado** |
|----------------|----------------------------------------------------------------------------------------|---------------------------------------------------------------------------------------------------------------------------|------------|
| gateway (en-monitor-api-gateway, 8080) | Enrutamiento, CORS, validación del token, límite de peticiones y errores propios | GatewayRoutes, SecurityFilterChainConfiguration, AccessTokenCookieFilter, AuthRateLimitFilter y GatewayExceptionHandler | Activo     |
| security (en-monitor-security-service, 8082) | Registro, autenticación (incluido Google), sesiones, contraseñas, perfil, RBAC, administración de usuarios, auditoría y borrado de cuenta | AuthController, AdminController, SupportController, InternalController + casos de uso | Activo     |
| home (energy-monitor-backend, 8083) | Hogares, miembros, favoritos, código de acceso, umbrales, eliminación del hogar y transferencia de propiedad | HomeController, HomeThresholdsController, HomeTypeController, controladores internos + casos de uso | Activo     |
| device (en-monitor-device-service, 8084) | Dispositivos, vinculación, credenciales, estado de conexión, comandos y autorización del broker | HomeDeviceController, ApplianceTypeController, BrokerAuthController, InternalDeviceController + 5 servicios | Activo     |
| measurement (en-monitor-measurement-service, 8085) | Ingesta de telemetría por MQTT, histórico, estadísticas y consumo del hogar | MeasurementController, ConsumptionLevelController, HomeConsumptionController, InternalMeasurementController + consumidor MQTT | Activo     |
| alert (energy-monitor-backend, 8083) | Alertas de consumo, límite, dispositivo vinculado y conectividad, y su bandeja | AlertController + 10 casos de uso                                                                                         | Activo     |
| recommendation (energy-monitor-backend, 8083) | Reglas de ahorro evaluadas cada hora y bandeja de recomendaciones     | RecommendationController, RecommendationScheduler + 2 servicios                                                           | Activo     |
| notification (energy-monitor-backend, 8083) | Entrega por correo, Web Push y push móvil, preferencias, registro de entregas y avisos de administrador | NotificationChannelsController, AdminNotificationsController + consumidores de RabbitMQ + casos de uso | Activo     |

## 12.2 Diagrama de clases del dominio

El diagrama de clases modela las clases, interfaces y enumeraciones del dominio, junto con los patrones de diseño aplicados: Strategy para la autorización por rol (OwnerPermissions y MemberPermissions), Observer (eventos con nombre) para la reacción a las mediciones y Factory Method para la creación de alertas, recomendaciones y notificaciones. Los patrones están descritos en [`05.4-patrones-de-diseno.md`](../../05-diseno-software/ES/05.4-patrones-de-diseno.md) y las clases en [`05.3-modelo-de-clases.md`](../../05-diseno-software/ES/05.3-modelo-de-clases.md).

> **Nota:** al igual que el MER, el diagrama completo se presenta en página horizontal. El Anexo C lo desglosa por dominio para facilitar su lectura. La figura publicada puede conservar la nota de que las clases del patrón Strategy no existen en el código; hoy sí existen (`[POR CONFIRMAR]` al revisar la figura).
>
**Figura 4.** *Diagrama de clases del dominio, incluyendo los patrones Strategy, Observer y Factory Method.*

## 12.3 Módulos del backend por contexto

Cada subsección indica el repositorio en el que se ejecuta el módulo; las rutas de archivo son relativas a `src/main/java/com/energymonitor/` de ese repositorio.

### 12.3.1 Módulo security

**Función:** Identidad, autenticación, sesiones, control de acceso, administración de usuarios, auditoría y borrado de cuenta.

**Estado:** Activo

**Ubicación:** en-monitor-security-service (puerto 8082). En energy-monitor-backend queda el intermediario `security` (api: UserProfileQuery, AdministratorQuery; adapter/out/remote: RemotePermissionCheck, RemoteUserProfileQuery, RemoteAdministratorQuery y SecurityServiceClient), sin dominio ni tablas; el backend solo valida los tokens con la clave pública.

**Archivos relacionados:**

- security/adapter/in/web/AuthController.java (autenticación, perfil y cuenta), AdminController.java (`/api/v1/admin/users` y `/api/v1/admin/roles`) y SupportController.java (contactos de soporte)

- security/adapter/in/web/internal/InternalController.java (API interno: permisos, perfiles y administradores)

- security/adapter/in/web/dto/ (DTOs: RegisterRequest, LoginRequest, GoogleLoginRequest, DeleteAccountRequest, ProfileResponse, ApiError…)

- security/application/usecase/ (RegisterUserService, AuthenticateUserService, SignInWithGoogleService, RefreshSessionService, LogoutUserSessionService, ChangePasswordService, CreatePasswordResetTokenService, ResetPasswordService, EmailVerificationService, UpdateUserProfileService, DeleteAccountService, CheckPermissionService, AssignRoleService, RevokeRoleService, ManageUserStatusService…)

- security/domain/model/ (User, Person, Email, PasswordHash, PasswordPolicy, UserSession, RefreshToken, SystemRole, Permission, AuditLog, LoginErrorLog)

- security/infrastructure/ (SecurityFilterChainConfiguration, JwtTokenIssuer, PemRsaKeys, SecurityCorsConfiguration, JwtAuthenticationEntryPoint)

- security/adapter/out/security/ (BCryptPasswordHasherAdapter, Sha256TokenHasher, PepperedHmacResetCodeHasher, InMemoryResetAttemptLimiter)

- security/adapter/out/messaging/ (OutboxWriter, OutboxRelay, RabbitOutboxPublisher: eventos de cuenta y solicitudes de correo) y security/adapter/out/backend/HttpAccountDeletionGuard.java (consulta de borrado al backend)

**Tablas relacionadas:** person, user, user_configuration, password_reset_token, user_session, refresh_token, security_configuration, password_policy, login_error_log, system_role, permission, user_system_role, system_role_permission, audit_log y security_outbox.

**Operaciones principales:**

- Registrar cuenta y verificar el correo

- Iniciar sesión (correo y contraseña o cuenta de Google) y renovar tokens

- Cerrar sesión

- Recuperar y cambiar contraseñas

- Consultar y actualizar el perfil

- Consultar permisos (RBAC) y administrar usuarios: directorio, cambio de estado y asignación o revocación de roles globales

- Publicar los contactos de soporte (administradores activos)

- Eliminar la cuenta (valida la contraseña, pregunta al backend por `/internal/accounts/{id}/deletion-check`, cierra las sesiones, elimina lógicamente los datos personales, libera el correo y publica `account.deleted` por RabbitMQ)

- Solicitar al backend, por RabbitMQ, el envío de los correos de verificación y recuperación (`mail.*`)

- Registrar auditoría e intentos fallidos

### 12.3.2 Módulo home

**Función:** Gestión de hogares, miembros, favoritos, umbrales de consumo, eliminación del hogar y transferencia de propiedad.

**Estado:** Activo

**Ubicación:** energy-monitor-backend (puerto 8083). Su migración a microservicio está en curso (sección 16.4).

**Archivos relacionados:**

- home/adapter/in/web/HomeController.java, HomeThresholdsController.java, HomeTypeController.java

- home/adapter/in/web/CurrentUserResolver.java (resuelve el usuario desde el claim `sub` del JWT)

- home/adapter/in/internal/ (HomeAccessController, HomeLimitsController y AccountDeletionCheckController: API interno para los servicios de dispositivos, mediciones y seguridad)

- home/adapter/in/messaging/AccountDeletedConsumer.java (consume `account.deleted`) y home/adapter/out/messaging/ (HomeOutboxWriter, HomeOutboxRelay, RabbitHomeOutboxPublisher: publica `home.deleted`)

- home/application/usecase/ (CreateHomeService, JoinHomeService, LeaveHomeService, ListHomesService, ListMembersService, RemoveUserService, ToggleFavoriteService, DeleteHomeService, TransferHomeOwnershipService, HomeRemoval, RemoveDeletedAccountService, CheckAccountDeletionService, DescribeHomeAccessService, DescribeHomeLimitsService, GetHomeThresholdsService, UpdateHomeThresholdsService, ListHomeTypesService, GetHomeTypeService)

- home/domain/model/ (Home, HomeType, HomeThresholds, UserHome, Role OWNER/MEMBER, OwnerPermissions, MemberPermissions)

- home/api/ (HomeApi, HomeDeleted, HomeOwnershipTransferred) y home/infrastructure/HomeApiImpl.java (API pública consumida por otros módulos)

**Tablas relacionadas:** home, home_type, home_thresholds, user_home y home_outbox.

**Operaciones principales:**

- Crear un hogar y generar código de acceso

- Unirse a un hogar con código

- Listar hogares y miembros

- Marcar favorito

- Expulsar miembros y salir del hogar (el último propietario no puede salir)

- Consultar, actualizar y restablecer umbrales diarios y mensuales

- Eliminar el hogar (solo OWNER, eliminación lógica de `home`, `user_home` y `home_thresholds`; en la misma transacción escribe `home.deleted` en `home_outbox`, y el relay lo publica en RabbitMQ; el servicio de dispositivos lo consume, desvincula los módulos, revoca sus claves y les envía el comando MQTT de desvinculación). El mismo comportamiento se aplica cuando llega `account.deleted` de una cuenta que era la única dueña de un hogar sin otros miembros.

- Responder al API interno: acceso de un usuario a un hogar, límites del hogar y si una cuenta se puede borrar (409 si es la única dueña de un hogar con otros miembros)

- Transferir la propiedad (solo OWNER; intercambio atómico de roles con bloqueo de fila; publica `HomeOwnershipTransferred`, que ningún módulo consume todavía)

### 12.3.3 Módulo measurement

**Función:** Ingesta y análisis de mediciones de consumo eléctrico.

**Estado:** Activo

**Ubicación:** en-monitor-measurement-service (puerto 8085). En energy-monitor-backend queda el intermediario `measurement` (api: MeasurementApi, MeasurementRecorded, RiskConsumption; adapter/out/remote: RemoteMeasurementApi y MeasurementServiceClient, con los niveles de consumo en caché 10 minutos; adapter/in/messaging/MeasurementRecordedConsumer), sin dominio ni tablas.

**Archivos relacionados:**

- measurement/adapter/in/web/MeasurementController.java, ConsumptionLevelController.java, HomeConsumptionController.java

- measurement/adapter/in/internal/InternalMeasurementController.java (API interno para el backend: niveles, energía por hora y dispositivos con lecturas)

- measurement/adapter/in/mqtt/ (TelemetryMqttConsumer, MqttTopics, MqttIngestionConfiguration; la suscripción se activa con `MEASUREMENT_MQTT_ENABLED`)

- measurement/adapter/in/messaging/DeviceLinksConsumer.java (consume `device.linked` y `device.unlinked` para limpiar su caché)

- measurement/application/usecase/ (RegisterMeasurementService, ListMeasurementsService, GetLatestMeasurementService, GetConsumptionStatisticsService, HomeConsumptionUseCase, HourlyEnergyCalculator, ListConsumptionLevelsService, TimeRange)

- measurement/domain/model/ (Measurement, ConsumptionLevel)

- measurement/adapter/out/event/MeasurementEventAdapter.java y measurement/adapter/out/messaging/ (MeasurementOutboxWriter, MeasurementOutboxRelay, RabbitMeasurementOutboxPublisher, MeasurementOutboxPurge: publican `measurement.recorded`)

- measurement/adapter/out/client/ (BackendHomeClient, DeviceServiceClient, CachedDeviceDirectory: acceso y límites del hogar y dispositivos)

**Tablas relacionadas:** measurement, consumption_level y measurement_outbox (el estado de conexión se registra en device_status_log, del servicio de dispositivos).

**Operaciones principales:**

- Registrar las lecturas que llegan por MQTT (voltaje, corriente, potencia activa, energía acumulada); es el único suscriptor que las guarda

- Consultar histórico por dispositivo y rango de fechas y la última medición

- Calcular estadísticas, el resumen y el historial de consumo del hogar

- Publicar cada lectura como `measurement.recorded` en RabbitMQ (el backend la republica en memoria como MeasurementRecorded)

- Responder al API interno del backend: niveles de consumo, energía por hora y dispositivos con lecturas

### 12.3.4 Módulo alert

**Función:** Generación y bandeja de alertas de consumo, de límite, de dispositivo vinculado y de conectividad.

**Estado:** Activo

**Ubicación:** energy-monitor-backend (puerto 8083). Su migración a microservicio está en curso (sección 16.4).

**Archivos relacionados:**

- alert/adapter/in/web/AlertController.java (consulta, lectura y eliminación)

- alert/adapter/in/event/MeasurementRecordedListener.java y DeviceConnectivityLostListener.java (@EventListener sobre los eventos en memoria que republican los intermediarios measurement y device)

- alert/application/usecase/ (ListAlertsService, GetAlertService, ResolveAlertService, DeleteAlertsService, RegisterThresholdAlertService, RegisterLimitAlertService, RegisterConnectivityAlertService, RegisterDeviceLinkedAlertService, ResolveConnectivityAlertsService, AlertAccessService)

- alert/infrastructure/AlertApiImpl.java y alert/api/AlertRaised.java

**Tablas relacionadas:** alert.

**Operaciones principales:**

- Generar alertas de consumo alto o crítico al recibir una medición

- Generar alertas de límite diario o mensual y de dispositivo vinculado (con la primera lectura tras vincular)

- Generar y resolver alertas de conectividad (150 segundos de silencio o mensaje de última voluntad)

- Listar alertas por hogar y estado, consultar una, marcarla como leída y eliminar las resueltas o leídas

### 12.3.5 Módulo notification

**Función:** Entrega de alertas y recomendaciones por correo, Web Push y push móvil, preferencias, registro de entregas y avisos in-app a los administradores.

**Estado:** Activo

**Ubicación:** energy-monitor-backend (puerto 8083). Su migración a microservicio está en curso (sección 16.4).

**Archivos relacionados:**

- notification/adapter/in/web/NotificationChannelsController.java y AdminNotificationsController.java (`/api/v1/admin/notifications`)

- notification/adapter/in/event/AlertRaisedListener.java y RecommendationCreatedListener.java

- notification/adapter/in/messaging/AccountEventsConsumer.java y MailRequestsConsumer.java (eventos `account.*` y solicitudes `mail.*` del servicio de seguridad)

- notification/infrastructure/mail/EmailSender.java, MailDeliveryConfiguration.java, AbandonedDeliverySweeper.java y notification/adapter/out/push/WebPushSender.java y ExpoPushSender.java

- notification/application/usecase/ (NotifyAlert, SendPasswordResetEmail, SendEmailVerification, ManageNotificationChannels, AlertMessages, AdminNotifications)

- notification/adapter/out/persistence/ (adaptadores, entidades y repositorios de persistencia)

**Tablas relacionadas:** notification, notification_preference, push_subscription y push_device.

**Operaciones principales:**

- Enviar los correos de verificación y recuperación que solicita el servicio de seguridad (solo el backend envía correo)

- Entregar cada alerta y recomendación a los miembros del hogar por correo, Web Push y push móvil (Expo) según sus preferencias

- Registrar avisos in-app para los administradores cuando se registra una cuenta o se administra otra (`account.registered`, `account.administered`)

- Registrar el estado de entrega (PENDING, SENT, FAILED) sin guardar el contenido del mensaje

- Barrer entregas abandonadas al iniciar la aplicación y cada cinco minutos

- Gestionar las preferencias, las suscripciones push del navegador y los dispositivos de la app móvil

### 12.3.6 Módulo device

**Función:** Registro y vinculación de dispositivos, credenciales (API key), estado de conexión, comandos y autorización del broker MQTT.

**Estado:** Activo

**Ubicación:** en-monitor-device-service (puerto 8084). En energy-monitor-backend queda el intermediario `device` (api: DeviceApi, DeviceConnectivityLost, DeviceConnectivityRestored; adapter/out/remote: RemoteDeviceApi y DeviceServiceClient, con caché de 30 segundos y copia anterior hasta 1 hora; adapter/in/messaging/DeviceEventsConsumer), sin dominio ni tablas.

**Archivos relacionados:**

- device/adapter/in/web/HomeDeviceController.java, ApplianceTypeController.java y BrokerAuthController.java

- device/adapter/in/internal/InternalDeviceController.java (API interno para el backend y el servicio de mediciones)

- device/application/usecase/ (HomeDeviceService, BrokerAccessService, DeviceConnectivityService, DeviceReportService, DeviceQueryService)

- device/domain/model/ (Device, DeviceHome, DeviceStatusLog, ApplianceType, ApiKeyHash)

- device/adapter/in/mqtt/ConnectivityMqttConsumer.java (suscripción a telemetría y estado), device/adapter/in/messaging/HomeDeletedConsumer.java (consume `home.deleted`), device/adapter/in/event/DeviceCommandListener.java y device/adapter/in/scheduler/DeviceInactivityScheduler.java

- device/adapter/out/ (mqtt/MqttDeviceCommandAdapter, messaging/ con DeviceOutboxRelay y RabbitDeviceOutboxPublisher, home/ para consultar el acceso al backend, persistencia y generadores de credenciales)

**Tablas relacionadas:** device, appliance_type, device_status_log, device_home y device_outbox.

**Operaciones principales:**

- Vincular un dispositivo a un hogar (solo OWNER), editarlo, emitir nuevas credenciales para cambiar su Wi-Fi y desvincularlo (el permiso de miembro u OWNER se pregunta al backend)

- Autenticar al dispositivo y autorizar sus tópicos para Mosquitto (solo el hash SHA-256 de la clave se almacena)

- Registrar la conectividad a partir de la telemetría y del estado retenido, y marcar el dispositivo sin conexión tras 150 segundos de silencio (`DeviceInactivityScheduler`)

- Publicar `device.linked`, `device.unlinked`, `device.connectivity-lost` y `device.connectivity-restored` en RabbitMQ

- Enviar el comando MQTT de desvinculación al desvincular el dispositivo o al recibir `home.deleted`

### 12.3.7 Módulo recommendation

**Función:** Recomendaciones de ahorro generadas a partir del consumo medido.

**Estado:** Activo

**Ubicación:** energy-monitor-backend (puerto 8083). Su migración a microservicio está en curso (sección 16.4).

**Archivos relacionados:**

- recommendation/adapter/in/web/RecommendationController.java

- recommendation/adapter/in/scheduler/RecommendationScheduler.java (cada hora)

- recommendation/application/usecase/ (EvaluateRecommendationsService, RecommendationInboxService)

- recommendation/adapter/out/client/ (HomeDeviceReaderAdapter y MeasurementConsumptionReaderAdapter: leen los límites del hogar por el api de home, y los dispositivos y la energía por hora a través de los intermediarios device y measurement)

- recommendation/adapter/out/event/RecommendationEventAdapter.java (publica RecommendationCreated)

**Tablas relacionadas:** recommendation.

**Operaciones principales:**

- Evaluar cada hora cinco reglas de ahorro (hora punta, consumo en reposo, comparación semanal, proyección de límite y aumento por dispositivo), sin repetir la misma recomendación en 7 días

- Listar las recomendaciones de un hogar, marcarlas como leídas y eliminarlas

## 12.4 Módulo front-end web

**Función:** interfaz de consulta y gestión del consumo eléctrico (SPA React)

**Archivos relacionados:**

- src/routes/AppRoutes.jsx (rutas), ProtectedRoute.jsx (rutas protegidas) y HomeRoute.jsx (detalle del hogar)

- src/services/http/httpClient.js (cliente axios hacia el gateway, interceptores, sesión en cookies HttpOnly con `X-Session-Transport: cookie` y renovación de token)

- src/services/auth, home, devices, alerts, recommendations y notifications (endpoints consumidos)

- src/context/HomeContext.jsx, ThemeContext.jsx y NotificationCenterContext.jsx (estado global)

- src/features/ (landingPage, auth, dashboard, DetailHome, Favorites, Notifications, Account, SettingsPage)

**Pantallas y rutas:**

| **Ruta**                                                  | **Pantalla**                | **Función**                                                                                |
|-----------------------------------------------------------|-----------------------------|--------------------------------------------------------------------------------------------|
| /home, /about, /support                                   | Landing page                | Páginas públicas de presentación, acerca de y soporte.                                     |
| /login, /register                                         | Login y Registro            | Autenticación (incluido Google) y creación de cuenta con validación zod.                   |
| /VerifyAccount, /recover-password, /verifyRecoverPassword | Verificación y recuperación | Código de verificación de correo y recuperación de contraseña.                             |
| /dashboard                                                | Panel de hogares            | Tarjetas de hogares, crear hogar, unirse con código y favoritos.                           |
| /homes/:homeId                                            | Detalle del hogar           | Seis pestañas: Consumo, Historial, Usuarios (transferir propiedad), Dispositivos (vincular por Bluetooth), Umbrales y Hogar (eliminar hogar). |
| /favorites                                                | Favoritos                   | Listado de hogares marcados como favoritos.                                                |
| /notifications                                            | Notificaciones              | Alertas y recomendaciones del hogar; leerlas y eliminarlas.                                |
| /settings                                                 | Configuración               | Apariencia, idioma, notificaciones (correo y push) y ayuda.                                |
| /account                                                  | Cuenta                      | Edición de perfil, foto, contraseña y eliminación de cuenta.                               |

**Operaciones principales:** iniciar sesión y cerrar sesión, registrar y verificar cuenta, recuperar contraseña, crear y unirse a hogares, marcar favoritos, consultar y editar miembros y umbrales, vincular y gestionar dispositivos, visualizar consumo e historial, visualizar alertas y recomendaciones, activar notificaciones push, eliminar el hogar o transferir su propiedad, cambiar idioma y tema.

## 12.5 Módulo front-end móvil

**Función:** aplicación Android conectada a la API REST

**Archivos relacionados:**

- src/app/ (rutas Expo Router: \_layout, index, register, verify-account, recover-password, dashboard, homes, home/\[id\], favorites, notifications, settings, account)

- src/services/ (http, auth, home, devices, measurement, alerts, recommendations, notifications)

- src/features/ (auth, dashboard, homes, favorites, notifications, settings, account)

- src/context/ (HomeContext, UserContext, ThemeContext, NotificationCenterContext)

- src/components/ y src/constants/ (design system propio con StyleSheet)

**Operaciones principales:** registro, verificación de código, recuperación de contraseña, panel de hogares, detalle del hogar con seis pestañas, favoritos, notificaciones (alertas y recomendaciones), configuración de idioma y tema, y gestión de cuenta.

> **Limitaciones vigentes:** la eliminación de un hogar y la transferencia de su propiedad solo existen en la web. La aplicación móvil no tiene pruebas automatizadas y no está disponible para iPhone.

# 13. API o rutas del sistema

**URL base:** https://energy-monitor.tech/api/v1 en producción y http://localhost:8080/api/v1 en desarrollo (API gateway) \| **Formato:** JSON \| **Documentación interactiva:** /swagger-ui.html y /v3/api-docs de cada servicio

La API pública suma 66 rutas de negocio bajo `/api/v1`, todas a través del gateway, que las reenvía al servicio dueño según la tabla «Rutas del API gateway» de la sección 9.3 (auth, admin/users, admin/roles y support: seguridad; dispositivos y catálogo de electrodomésticos: dispositivos; mediciones, niveles y consumo del hogar: mediciones; el resto: backend). La migración no cambió las rutas que ya usaban los clientes. A ellas se suman dos rutas internas para el broker y las rutas de infraestructura. Todas las que no aparecen como públicas requieren un JWT válido (cabecera Authorization o cookie `em_access` de la web); si el token falta, expiró o es inválido, el gateway responde 401 sin reenviar la petición, y si el usuario no tiene el permiso requerido, el servicio responde 403.

| **Método** | **Ruta**                                              | **Descripción**                                                       | **Parámetros**                                                         | **Autenticación** |
|------------|-------------------------------------------------------|-----------------------------------------------------------------------|------------------------------------------------------------------------|-------------------|
| POST       | /api/v1/auth/register                                 | Registra una cuenta nueva.                                            | body: email, password, name, lastName, profileImage (opcional)         | Pública           |
| POST       | /api/v1/auth/email/verify                             | Verifica el correo con un código de 6 dígitos.                        | body: email, code                                                      | Pública           |
| POST       | /api/v1/auth/email/verification/resend                | Reenvía el código de verificación (siempre responde 202).             | body: email                                                            | Pública           |
| POST       | /api/v1/auth/login                                    | Autentica y devuelve access token y refresh token.                    | body: email, password                                                  | Pública           |
| POST       | /api/v1/auth/google                                   | Inicia sesión o registra la cuenta con Google (503 si no está configurado). | body: code                                                       | Pública           |
| POST       | /api/v1/auth/refresh                                  | Renueva el access token y rota el refresh token.                      | body: refreshToken (móvil) o cookie em_refresh (web)                   | Pública           |
| POST       | /api/v1/auth/logout                                   | Cierra la sesión registrada.                                          | body: idUserSession                                                    | JWT               |
| POST       | /api/v1/auth/password/forgot                          | Solicita el código de recuperación de contraseña (202; 404 si el correo no tiene cuenta). | body: email                                        | Pública           |
| POST       | /api/v1/auth/password/reset                           | Restablece la contraseña con el código recibido.                      | body: email, resetToken, newPassword                                   | Pública           |
| POST       | /api/v1/auth/password/change                          | Cambia la contraseña conociendo la actual.                            | body: currentPassword, newPassword                                     | JWT               |
| PUT        | /api/v1/auth/profile                                  | Actualiza nombre, apellido, imagen o correo.                          | body: name, lastName, profileImage, newEmail                           | JWT               |
| POST       | /api/v1/auth/account/delete                           | Elimina la cuenta tras confirmar la contraseña.                       | body: password                                                         | JWT               |
| GET        | /api/v1/auth/account                                  | Devuelve la cuenta autenticada.                                       | ninguno (identidad del JWT)                                            | JWT               |
| GET        | /api/v1/auth/permissions                              | Lista los códigos de permiso del usuario.                             | ninguno                                                                | JWT               |
| GET        | /api/v1/admin/users                                   | Directorio de usuarios (requiere USER.READ).                          | query: q, page, size (1 a 50; 20 por defecto)                          | JWT               |
| GET        | /api/v1/admin/roles                                   | Lista los roles globales asignables (requiere USER.READ).             | ninguno                                                                | JWT               |
| PUT        | /api/v1/admin/users/{idUser}/status                   | Cambia el estado de otra cuenta (requiere USER.MANAGE; 409 sobre la propia). | path: idUser; body: status (ACTIVE, INACTIVE o BLOCKED)         | JWT               |
| PUT        | /api/v1/admin/users/{idUser}/roles/{roleName}         | Otorga un rol global (requiere ROLE.MANAGE).                          | path: idUser, roleName                                                 | JWT               |
| DELETE     | /api/v1/admin/users/{idUser}/roles/{roleName}         | Revoca un rol global (requiere ROLE.MANAGE).                          | path: idUser, roleName                                                 | JWT               |
| GET        | /api/v1/support/contacts                              | Nombre y correo de cada administrador activo, para la página de soporte. | ninguno                                                             | Pública           |
| GET        | /api/v1/measurements                                  | Lista las mediciones de un dispositivo en un rango.                   | query: deviceId (obligatorio), from, to                                | JWT               |
| GET        | /api/v1/measurements/latest                           | Devuelve la última medición de un dispositivo.                        | query: deviceId                                                        | JWT               |
| GET        | /api/v1/measurements/statistics                       | Estadísticas de consumo del periodo.                                  | query: deviceId, from, to                                              | JWT               |
| GET        | /api/v1/consumption-levels                            | Lista los niveles de consumo (LOW, MEDIUM, HIGH, CRITICAL).           | ninguno                                                                | JWT               |
| GET        | /api/v1/homes/{homeId}/consumption/summary           | Resumen de consumo del hogar.                                         | path: homeId; query: zone (UTC por defecto)                            | JWT               |
| GET        | /api/v1/homes/{homeId}/consumption/history           | Historial de consumo del hogar por periodo.                           | path: homeId; query: period (month por defecto), zone                  | JWT               |
| POST       | /api/v1/homes                                         | Crea un hogar y devuelve su código de acceso.                         | body: name, homeTypeId, address, description                           | JWT               |
| GET        | /api/v1/homes                                         | Lista los hogares del usuario con su rol.                             | ninguno                                                                | JWT               |
| GET        | /api/v1/homes/{homeId}/members                        | Lista los miembros de un hogar.                                       | path: homeId                                                           | JWT               |
| POST       | /api/v1/homes/join                                    | Permite unirse a un hogar con su código de acceso.                    | body: accessCode                                                       | JWT               |
| PUT        | /api/v1/homes/{homeId}/favorite                       | Marca o desmarca un hogar como favorito.                              | path: homeId                                                           | JWT               |
| DELETE     | /api/v1/homes/{homeId}/members/{userId}               | Expulsa a un miembro (solo OWNER).                                    | path: homeId, userId                                                   | JWT               |
| DELETE     | /api/v1/homes/{homeId}/members/me                     | Permite al usuario salir del hogar.                                   | path: homeId                                                           | JWT               |
| DELETE     | /api/v1/homes/{homeId}                                | Elimina el hogar (solo OWNER): 204; 403 si es MEMBER; 404 si no pertenece. | path: homeId                                                      | JWT               |
| PUT        | /api/v1/homes/{homeId}/owner                          | Transfiere la propiedad a un miembro: 204; 403, 404 y 409 según el caso. | path: homeId; body: newOwnerId                                     | JWT               |
| GET        | /api/v1/homes/{homeId}/thresholds                     | Consulta los umbrales del hogar.                                      | path: homeId                                                           | JWT               |
| PUT        | /api/v1/homes/{homeId}/thresholds                     | Actualiza el límite del hogar.                                        | path: homeId; body: limitPeriod (DAILY o MONTHLY), limit               | JWT               |
| POST       | /api/v1/homes/{homeId}/thresholds/defaults            | Restablece los umbrales a los valores del sistema (6 kWh diarios y 180 kWh mensuales). | path: homeId                                          | JWT               |
| GET        | /api/v1/home-types                                    | Lista los tipos de hogar disponibles.                                 | ninguno                                                                | JWT               |
| GET        | /api/v1/home-types/{idHomeType}                       | Consulta un tipo de hogar.                                            | path: idHomeType                                                       | JWT               |
| GET        | /api/v1/appliance-types                               | Lista el catálogo de electrodomésticos.                               | ninguno                                                                | JWT               |
| GET        | /api/v1/homes/{homeId}/devices                        | Lista los dispositivos del hogar.                                     | path: homeId                                                           | JWT               |
| POST       | /api/v1/homes/{homeId}/devices                        | Vincula un dispositivo y devuelve sus credenciales y la dirección del broker (solo OWNER). | body: deviceCode, name, applianceTypeId, location          | JWT               |
| PUT        | /api/v1/homes/{homeId}/devices/{deviceId}             | Edita electrodoméstico, nombre y ubicación.                           | path: homeId, deviceId; body: name, applianceTypeId, location          | JWT               |
| POST       | /api/v1/homes/{homeId}/devices/{deviceId}/credentials | Emite una clave nueva para cambiar la red Wi-Fi (invalida la anterior). | path: homeId, deviceId                                               | JWT               |
| DELETE     | /api/v1/homes/{homeId}/devices/{deviceId}             | Desvincula el dispositivo (solo OWNER).                               | path: homeId, deviceId                                                 | JWT               |
| GET        | /api/v1/alerts                                        | Lista las alertas de un hogar.                                        | query: homeId (obligatorio), status (PENDING \| RESOLVED)              | JWT               |
| GET        | /api/v1/alerts/{idAlert}                              | Consulta una alerta.                                                  | path: idAlert                                                          | JWT               |
| PUT        | /api/v1/alerts/{idAlert}/read                         | Marca una alerta como leída.                                          | path: idAlert                                                          | JWT               |
| DELETE     | /api/v1/alerts/{idAlert}                              | Elimina una alerta resuelta o leída.                                  | path: idAlert                                                          | JWT               |
| DELETE     | /api/v1/alerts                                        | Elimina todas las alertas resueltas de un hogar.                      | query: homeId                                                          | JWT               |
| GET        | /api/v1/recommendations                               | Lista las recomendaciones de un hogar.                                | query: homeId                                                          | JWT               |
| PUT        | /api/v1/recommendations/{idRecommendation}/read       | Marca una recomendación como leída.                                   | path: idRecommendation                                                 | JWT               |
| DELETE     | /api/v1/recommendations/{idRecommendation}            | Elimina una recomendación leída.                                      | path: idRecommendation                                                 | JWT               |
| DELETE     | /api/v1/recommendations                               | Elimina todas las recomendaciones leídas de un hogar.                 | query: homeId                                                          | JWT               |
| GET        | /api/v1/notifications/preferences                     | Consulta las preferencias de notificación del usuario.                | ninguno                                                                | JWT               |
| PUT        | /api/v1/notifications/preferences                     | Actualiza las preferencias de correo y push.                          | body: emailEnabled, pushEnabled                                        | JWT               |
| GET        | /api/v1/notifications/push/public-key                 | Devuelve la clave pública VAPID (vacía si el push está desactivado).  | ninguno                                                                | JWT               |
| POST       | /api/v1/notifications/push/subscriptions              | Registra una suscripción push del navegador.                          | body: endpoint, keys (p256dh, auth)                                    | JWT               |
| POST       | /api/v1/notifications/push/subscriptions/remove       | Elimina una suscripción push.                                         | body: endpoint, keys                                                   | JWT               |
| POST       | /api/v1/notifications/push/devices                    | Registra la instalación de la app móvil para push (Expo).             | body: token, platform (ANDROID o IOS)                                  | JWT               |
| POST       | /api/v1/notifications/push/devices/remove             | Elimina el registro push de una instalación de la app.                | body: token                                                            | JWT               |
| GET        | /api/v1/notifications/push/devices/available          | Indica si el push de la app está configurado (404 si no lo está).     | ninguno                                                                | JWT               |
| GET        | /api/v1/admin/notifications                           | Últimos 50 avisos in-app del administrador (requiere USER.READ).      | ninguno                                                                | JWT               |
| PUT        | /api/v1/admin/notifications/{id}/read                 | Marca un aviso de administrador como leído.                           | path: id                                                               | JWT               |
| DELETE     | /api/v1/admin/notifications/{id}                      | Elimina un aviso de administrador.                                    | path: id                                                               | JWT               |
| POST       | /internal/mqtt/auth/user                              | Interna (servicio de dispositivos): Mosquitto autentica a un cliente MQTT (caché de cinco minutos). El gateway la rechaza. | body: username, password, clientid | Red interna |
| POST       | /internal/mqtt/auth/acl                               | Interna (servicio de dispositivos): Mosquitto autoriza el tópico de un cliente. El gateway la rechaza. | body: username, clientid, topic, acc | Red interna       |
| GET        | /actuator/health                                      | Estado de cada servicio (UP / DOWN).                                  | ninguno                                                                | Pública           |
| GET        | /swagger-ui.html y /v3/api-docs                       | Documentación interactiva de la API de cada servicio (OpenAPI).       | ninguno                                                                | Pública           |
| OPTIONS    | /\*\*                                                 | Preflight de CORS (lo responde el gateway).                           | cabeceras Origin y Access-Control-Request-\*                           | Pública           |

> *Las rutas `/internal/mqtt/**` solo aceptan llamadas desde las redes de `MQTT_AUTH_ALLOWED_NETWORKS`; las demás rutas `/internal/**` (sección 9.3) exigen además la cabecera `X-Internal-Token`. El gateway nunca las reenvía (401 sin token, 403 con token) y el nginx de la web solo reenvía `/api/`.*

## 13.1 Códigos de respuesta habituales

| **Código**      | **Cuándo se produce**                                                         | **Estructura de la respuesta**                                 |
|-----------------|-------------------------------------------------------------------------------|----------------------------------------------------------------|
| 200 / 201 / 204 | Operación exitosa (consulta, creación o eliminación sin contenido).           | Cuerpo con el recurso o mensaje correspondiente.               |
| 400             | Validación de datos incorrecta (Bean Validation o esquema zod en el cliente). | ApiError con fieldErrors detalle de cada campo.                |
| 401             | Token ausente, expirado o credenciales inválidas (el gateway lo responde antes de reenviar). | ApiError con mensaje genérico (no revela si el correo existe). |
| 403             | Cuenta bloqueada o inactiva, o permiso insuficiente (p. ej. un MEMBER intenta eliminar el hogar). | ApiError.                                 |
| 404             | Recurso inexistente (hogar, alerta, miembro…), hogar al que no se pertenece o ruta que el gateway no enruta. | ApiError o {"error": "..."} según el módulo.  |
| 409             | Conflicto (correo ya registrado, último propietario que no puede salir, transferencia al propietario actual). | ApiError.                      |
| 422             | La contraseña no cumple la política de seguridad.                             | ApiError.                                                      |
| 429             | Límite de intentos alcanzado en verificación o recuperación (servicio de seguridad) o más de 20 POST públicos de cuenta por IP y ruta en un minuto (gateway). | ApiError con cabecera Retry-After (900 segundos en la recuperación). |
| 503             | Un servicio no acepta la conexión (gateway), un servicio interno del que depende la operación no responde, o inicio de sesión con Google sin credenciales configuradas. | ApiError. |
| 504             | Un servicio tarda más de 30 segundos en responder (gateway).                  | ApiError.                                                      |
| 500             | Error interno no controlado.                                                  | ApiError sin traza de pila (nunca se filtra stack trace).      |

## 13.2 Ejemplo de petición y respuesta

```
POST /api/v1/auth/login
Content-Type: application/json

{ "email": "usuario@correo.com", "password": "********" }

200 OK

{
  "accessToken": "eyJhbGciOiJSUzI1NiIsImV4cCI6MTc2…",
  "refreshToken": "7f3c…",
  "tokenType": "Bearer",
  "expiresIn": 900,
  "account": { "idUser": "user000001", "email": "usuario@correo.com", "status": "ACTIVE" }
}
```

Es la respuesta que recibe la app móvil. Con la cabecera `X-Session-Transport: cookie` (la que envía la web) el servicio de seguridad entrega los tokens en las cookies HttpOnly `em_access` y `em_refresh`, y `accessToken` y `refreshToken` llegan en `null`.

# 14. Seguridad

## 14.1 Controles implementados

| **Control**                   | **Implementación técnica**                                                                                                                                                                              |
|-------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Autenticación                 | JWT firmado con algoritmo RS256 por el servicio de seguridad, el único que tiene la clave privada (PEM); el gateway y los demás servicios lo validan con la clave pública (firma, emisor y expiración), de modo que un servicio alcanzado sin pasar por el gateway tampoco queda abierto. Vida útil del access token: 15 minutos; sin roles dentro del JWT; claims sub (idUser), person_id y email. |
| Identidad                     | Todos los servicios y módulos obtienen el usuario del claim `sub` mediante `CurrentUserResolver`; la cabecera X-User-Id se ignora.                                                                       |
| Sesiones                      | Sin sesión HTTP en el servidor (STATELESS). La app móvil recibe los tokens en el cuerpo; la web, en cookies HttpOnly y SameSite=Strict (Secure en producción): `em_access` (Path=/api) y `em_refresh` (Path=/api/v1/auth). El gateway solo usa la cookie si la petición trae `X-Requested-With: XMLHttpRequest`, cabecera que un formulario o una imagen de otro sitio no pueden enviar (protección CSRF). |
| Renovación de sesión          | Refresh token opaco con estado, rotado en cada uso, almacenado como hash SHA-256 con family_id y parent_id para detectar reutilización (replay) y revocado al cambiar la contraseña.                     |
| Cifrado de contraseñas        | BCrypt (BCryptPasswordHasherAdapter) aislado del dominio; nunca se almacenan contraseñas en claro.                                                                                                      |
| Política de contraseñas       | Tabla password_policy: longitud mínima 8 y máxima 64, exige mayúsculas, números y símbolos, expira a los 90 días. Incumplimiento =\> 422.                                                               |
| Autorización global (RBAC)    | Cadena de cinco tablas (system_role, permission, user_system_role, system_role_permission y user) consultada en vivo por `CheckPermission` en el servicio de seguridad; permisos consultables en GET /api/v1/auth/permissions. El backend pide los permisos por el API interno, con caché de 30 segundos. El gateway no decide permisos. |
| Autorización por hogar        | Roles OWNER y MEMBER por hogar resueltos con el patrón Strategy (`OwnerPermissions`, `MemberPermissions`).                                                                                              |
| Validación de entrada         | Bean Validation en los DTOs del backend (@NotBlank, @Size, @NotNull, @Positive, @Pattern) y esquemas zod en los formularios de los front-ends.                                                          |
| CORS                          | Solo en el gateway, que elimina la cabecera Origin antes de reenviar. Cerrado por defecto: lista explícita de orígenes (SECURITY_CORS_ALLOWED_ORIGINS del gateway), sin comodines, sin credenciales y con manejo del preflight OPTIONS; métodos GET, POST, PUT, DELETE y OPTIONS. |
| Límite de intentos            | Limitadores en memoria: en el gateway, 20 POST por IP y ruta por minuto en los endpoints públicos de cuenta (inicio de sesión, registro, Google, refresh, recuperación y verificación) =\> 429 con Retry-After; en el servicio de seguridad, 10 intentos por IP cada 15 minutos en la recuperación de contraseña =\> 429 con cabecera Retry-After. |
| Protección contra enumeración | Login fallido con mensaje genérico; la comprobación de correo no verificado se hace después de la contraseña. Por decisión documentada, password/forgot responde 404 si el correo no tiene cuenta (como el registro responde 409 si ya existe), y el límite por IP se cobra antes de la búsqueda. |
| Códigos de recuperación       | 6 dígitos, validez de 15 minutos, uso único, guardados como HMAC-SHA256 con pepper (SECURITY_RESET_PEPPER).                                                                                             |
| Credenciales de dispositivo   | Cada dispositivo usa `device_code` y `api_key`; el servicio de dispositivos solo guarda el hash SHA-256 de la clave (`api_key_hash`). Mosquitto autentica al dispositivo contra el servicio de dispositivos y la ACL limita sus tópicos. |
| Transporte de dispositivos    | MQTT 3.1.1 con TLS en el puerto 8883; el puerto 1883 solo existe dentro de la red Docker y las rutas `/internal/mqtt/**` solo se aceptan desde redes permitidas.                                       |
| Comunicación interna          | Las rutas `/internal/**` exigen loopback o la red Docker (`172.16.0.0/12`) y la cabecera `X-Internal-Token` (`INTERNAL_API_TOKEN`), comparada en tiempo constante; sin token configurado se rechaza toda llamada. El gateway nunca las reenvía y en Docker solo se publican la web (80), MQTT con TLS (8883) y, en 127.0.0.1, los puertos de administración. |
| Auditoría                     | Tablas audit_log (acciones, IP, usuario) e login_error_log (intentos fallidos). El gateway agrega la IP del cliente a X-Forwarded-For y el servicio de seguridad solo la toma de esa cabecera cuando la conexión llega desde loopback o la red Docker; cualquier otro cliente se registra con su propia dirección. |
| Documentación                 | El endpoint de OpenAPI de cada servicio declara el esquema bearerAuth; los endpoints públicos son sólo los de autenticación, `GET /api/v1/support/contacts` y los de infraestructura.                     |
| Transporte                    | En producción se sirve por HTTPS (Nginx + Certbot con Let's Encrypt) en https://energy-monitor.tech, y MQTT con TLS en mqtt.energy-monitor.tech:8883; en desarrollo local se usa HTTP con Mailpit como servidor de correo de pruebas. |

## 14.2 Limitaciones y riesgos conocidos

> Los siguientes puntos están declarados por el propio equipo y deben tenerse en cuenta antes de ampliar el sistema.

| **Limitación**                                                                                 | **Impacto**                                                                           | **Recomendación**                                                                   |
|------------------------------------------------------------------------------------------------|---------------------------------------------------------------------------------------|-------------------------------------------------------------------------------------|
| Telemetría y estado con MQTT QoS 0.                                                            | Una lectura puede perderse (sin duplicados).                                          | Evaluar un cliente MQTT con QoS 1 (por ejemplo espMqttClient).                      |
| Mosquitto depende del servicio de dispositivos para autenticar a cada cliente.                 | Si el servicio cae, se rechazan los nuevos inicios de sesión MQTT y los módulos conectados solo publican mientras duran las decisiones de ACL en caché (5 minutos). | Vigilar la salud del servicio de dispositivos; el despliegue espera a que todos los servicios estén `healthy`. |
| Limitadores de intentos (gateway y recuperación de contraseña) en memoria y relays de outbox pensados para una sola instancia. | Se reinician con cada arranque y no se comparten entre instancias; una segunda instancia podría reordenar mensajes. | Usar Redis u otro almacén compartido si se escala a varias instancias.          |
| Sin actualización por aire (OTA) del firmware.                                                 | Actualizar el firmware exige un cable USB.                                            | Evaluar OTA en una versión futura.                                                  |
| Las pruebas de persistencia no son herméticas.                                                 | Necesitan el MySQL del repositorio de infraestructura en ejecución y SPRING_DATASOURCE_PASSWORD. | Usar un contenedor de base de datos para pruebas.                        |
| El access token sigue siendo válido hasta 15 minutos tras borrar la cuenta.                    | Ventana breve de acceso residual.                                                     | Aceptado; documentado.                                                              |
| Casos límite de las alertas de conectividad (CONNECTIVITY) sin resolución automática.          | Algunas alertas requieren revisión manual.                                            | Definir la regla de resolución automática.                                          |
| Web Bluetooth limitado a Chrome, Edge y Chrome para Android, y a HTTPS o localhost.            | En otros navegadores se usa el portal de configuración Wi-Fi; en iOS no hay push sin instalar la web. | Mantener el portal `EnergyMonitor-Setup` como alternativa.            |
| `HomeOwnershipTransferred` no tiene consumidor.                                                | El nuevo responsable no recibe un aviso al recibir la propiedad.                      | Consumir el evento y enviar correo o push.                                          |
| El historial del dispositivo no se limita al periodo de vinculación.                           | Un hogar puede ver mediciones de un periodo anterior de otro hogar.                   | Limitar el historial al periodo de vinculación (RF-HOM-11).                         |
| Borrado y transferencia de hogar ausentes en la aplicación móvil; sin pruebas automatizadas.   | Solo se pueden hacer desde la web.                                                    | Implementarlos en la aplicación Android y agregar pruebas.                          |

## 14.3 Buenas prácticas para los desarrolladores

- No commitear nunca los archivos .env (de cada servicio y de infra), application-local.yaml ni las claves RSA (\*.pem); ya están declarados en .gitignore. La clave privada solo la recibe el servicio de seguridad; los demás servicios reciben únicamente `public.pem`.

- No registrar contraseñas, tokens ni códigos de recuperación en los logs de la aplicación.

- Mantener actualizadas las dependencias de los servicios del backend y del front y revisar sus avisos de seguridad.

- Toda funcionalidad nueva debe validar la entrada en el servicio que la atiende, independientemente de las validaciones del cliente.

- Respetar los límites de módulo: no importar clases de otro contexto sin pasar por su paquete api (lo verifica ModularityTests en el backend). Entre servicios, comunicarse solo por el REST interno y los eventos de RabbitMQ; nunca importar entidades o repositorios de otro servicio ni crear llaves foráneas entre servicios.

- Un endpoint público nuevo en el servicio de seguridad debe agregarse también a la lista de endpoints públicos del gateway (`SecurityFilterChainConfiguration`).

- Rotar las claves JWT, el pepper de recuperación y el `INTERNAL_API_TOKEN` (en todos los servicios a la vez) si se sospecha de una filtración.

# 15. Control de versiones del software

El proyecto utiliza Git con repositorio remoto en GitHub. Cada componente del sistema se desarrolla en un repositorio independiente para facilitar su despliegue y su revisión.

| **Repositorio**            | **Contenido**                                                    | **Observaciones**                                                                         |
|----------------------------|------------------------------------------------------------------|-------------------------------------------------------------------------------------------|
| en-monitor-api-gateway     | API gateway en Java 21 / Spring Boot 4.0.8 / Spring Cloud Gateway. | https://github.com/Juan-Cortes2045/en-monitor-api-gateway; `main` en `c070cec` (24/10/2026). |
| en-monitor-security-service | Microservicio de seguridad en Java 21 / Spring Boot 4.1.1.      | https://github.com/Juan-Cortes2045/en-monitor-security-service; `main` en `334691a`. Incluye sus changesets security-\*. |
| en-monitor-device-service  | Microservicio de dispositivos en Java 21 / Spring Boot 4.1.1.    | https://github.com/Juan-Cortes2045/en-monitor-device-service; `main` en `f8b8526`. Incluye sus changesets device-\*. |
| en-monitor-measurement-service | Microservicio de mediciones en Java 21 / Spring Boot 4.1.1.  | https://github.com/Juan-Cortes2045/en-monitor-measurement-service; `main` en `ee2cb81`. Incluye sus changesets measurement-\*. |
| energy-monitor-backend     | Backend modular (home, alert, recommendation y notification) en Java 21 / Spring Boot 4.1.1 y firmware del ESP32. | https://github.com/Juan-Cortes2045/energy-monitor-backend; `main` en `aa9a713`. Incluye pruebas, Dockerfile, .env.example, sus migraciones Liquibase y la carpeta firmware/. |
| Energy-Monitor-application | Aplicación web React 19 / Vite 8.                                | https://github.com/Juan-Cortes2045/Energy-Monitor-application; `main` en `e59bece`. Incluye pruebas Vitest (14 archivos) y build de producción en /dist. |
| Energy-Monitor-Mobile      | Aplicación móvil Expo SDK 57.                                    | https://github.com/angelgarcia0/Energy-Monitor-Mobile; `main` en `894b65a`. Incluye LICENSE, AGENTS.md (documentación versionada de Expo 57) y scripts de Expo. |
| energy-monitor-infra       | Docker Compose (local, desarrollo y producción), Mosquitto, Nginx y despliegue. | https://github.com/Juan-Cortes2045/energy-monitor-infra; `main` en `eaf961b`. Incluye deploy.sh y las pruebas de punta a punta (e2e/). |
| energy-monitor-docs-final  | Documentación final bilingüe (ES y EN).                          | Documento general, especificación de requisitos, propuesta técnica, diseño y manuales. Otros repositorios de documentación: en-monitor-docs y Energy-Monitor-Documentation. |
| energy-monitor-devices     | Firmware y broker antiguos (legado).                             | Ya no se usa para ejecutar nada; el firmware está en energy-monitor-backend y el broker en energy-monitor-infra. |

- Todos los repositorios tienen las ramas `main` (producción) y `dev` (integración). Las ramas de trabajo siguen la convención `[tipo]/[nombre-en-kebab-case]` (feat/*, fix/*, chore/*) y los commits siguen Conventional Commits con el mensaje en inglés.

- La integración de código se hace mediante pull request a `dev` y luego de `dev` a `main`.

- Cada repositorio de código tiene un flujo de GitHub Actions que ejecuta las pruebas y el build en cada pull request y push a `dev` y `main`; tras un push a `main` y la aprobación del environment `production`, despliega por SSH con `infra/deploy/deploy.sh backend|security|device|measurement|gateway|frontend|infra` (sección 16.1).

- Antes de fusionar cambios se debe ejecutar la suite de pruebas del repositorio afectado (./mvnw test en cada servicio) y las pruebas del front web (npm test).

- Los cambios en el esquema de base de datos se realizan únicamente mediante un nuevo changeset de Liquibase con rollback; nunca se editan changesets ya aplicados.

- Las ramas de integración deben mantener el código en estado compilable y las pruebas en verde.

- Las versiones entregadas se etiquetan para conservar la referencia exacta del software documentado en este manual.

# 16. Recomendaciones de mantenimiento

## 16.1 Actualizaciones del código

| **Área**      | **Recomendación**                                                                                                                                                                 |
|---------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Backend       | Compilar con ./mvnw clean test (.\mvnw.cmd en Windows) en cada repositorio de servicio antes de cada entrega; en energy-monitor-backend, verificar que ModularityTests confirme que no se rompieron los límites de módulo. |
| Base de datos | Crear un changeset nuevo por cada modificación de esquema en el repositorio del servicio dueño de la tabla, con identificador secuencial de su módulo y su rollback; levantar MySQL desde energy-monitor-infra (modo desarrollo) para probar contra MySQL 8.4 real. |
| Dependencias  | Actualizar Spring Boot, Spring Cloud, Expo o React en ramas de prueba, ejecutando la suite completa antes de fusionar (el gateway sigue en Spring Boot 4.0 porque Spring Cloud 2025.1 es la versión que lo soporta). |
| Front web     | Mantener `VITE_API_BASE_URL` apuntando al gateway y la lista de orígenes permitidos (SECURITY_CORS_ALLOWED_ORIGINS del gateway); volver a ejecutar npm test y npm run lint.        |
| Front móvil   | Revisar la documentación versionada de Expo 57 antes de incorporar librerías nativas; no hay pruebas automatizadas, por lo que la verificación es manual.                         |
| Dispositivos  | Recompilar y cargar el firmware por USB tras cambiar config.h o el certificado; renovar el certificado del broker y reiniciar el contenedor `mosquitto` (en producción lo hace el hook de Certbot `deploy/renew-mqtt-cert.sh`). |
| Despliegue    | Desplegar desde `main` con GitHub Actions y `infra/deploy/deploy.sh <componente>`: respalda MySQL, actualiza y reconstruye solo ese servicio, espera a que los cinco servicios Java estén `healthy` y la web responda, y revierte solo si falla. Antes, correr `infra/e2e/device-flow.sh` y `infra/e2e/measurement-flow.sh` sobre la pila local. |
| Seguridad     | Revisar trimestralmente los permisos RBAC, la política de contraseñas y la vigencia de las claves JWT.                                                                            |

## 16.2 Copias de seguridad de la base de datos

Antes de cada actualización relevante se debe respaldar el esquema energy_monitor, que comparten todos los servicios. En producción, `deploy.sh` respalda MySQL antes de cada despliegue y `deploy/backup-mysql.sh` hace un respaldo lógico diario. Procedimiento manual recomendado:

1.  Detener la escritura de información (no ejecutar cargas de mediciones durante el respaldo).

2.  Ejecutar el volcado: mysqldump -h localhost -u energy_app -p energy_monitor \> backup_energy_monitor_AAAA-MM-DD.sql

3.  Guardar el archivo en una carpeta segura y fuera del repositorio, con la fecha en el nombre.

4.  Verificar el respaldo restaurándolo en una base de datos de prueba antes de darlo por válido.

5.  Restauración: mysql -h localhost -u energy_app -p energy_monitor \< backup_energy_monitor_AAAA-MM-DD.sql

## 16.3 Soporte y resolución de incidentes

| **Síntoma**                             | **Verificación sugerida**                                                                                                            |
|-----------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------|
| Un servicio no inicia                   | Revisar su puerto (8080, 8082, 8083, 8084 u 8085), los contenedores de MySQL y RabbitMQ (docker compose ps en infra) y la variable SPRING_DATASOURCE_PASSWORD. Sin RabbitMQ, /actuator/health responde DOWN. |
| La API responde 403/401 de forma masiva | Verificar que SECURITY_JWT_PUBLIC_KEY_PATH de cada servicio sea la clave pública del par del servicio de seguridad, y la fecha y hora del servidor. |
| El gateway responde 503 o 504           | El servicio dueño de la ruta no está en ejecución o tarda más de 30 segundos; revisar su salud con docker compose ps.                 |
| Fallan las llamadas internas (503, nombres faltantes) | Verificar que INTERNAL_API_TOKEN tenga el mismo valor en todos los servicios y que el servicio llamado esté en ejecución.   |
| El front web no conecta con la API      | Confirmar que el gateway responde en el puerto 8080 y que SECURITY_CORS_ALLOWED_ORIGINS del gateway incluye el origen http://localhost:5173. |
| Los correos no llegan                   | Revisar MAIL_HOST, MAIL_PORT, MAIL_USERNAME y MAIL_PASSWORD del backend y que RabbitMQ esté en ejecución; en local usar Mailpit (http://localhost:8025). |
| Un dispositivo no envía lecturas        | Revisar el broker (docker compose logs mosquitto), que el servicio de dispositivos esté en ejecución (el broker le pregunta por cada cliente), DEVICE_MQTT_PUBLIC_HOST (nunca localhost), el certificado TLS y la salida serie del ESP32. |
| Llegan lecturas pero no se generan alertas | Revisar que el servicio de mediciones responda, el INTERNAL_API_TOKEN y la cola `energy-monitor-backend.measurement.recorded` (o su `.dlq`) en RabbitMQ. |
| Las migraciones fallan al arrancar      | Revisar el log de Liquibase y el orden de los changesets en el db.changelog-master.xml del servicio que falla.                       |
| Los mensajes de error no son legibles   | Revisar la consola del navegador y el cuerpo ApiError devuelto por la API.                                                           |

## 16.4 Trabajos pendientes declarados

- **Trabajo pendiente: migración de home, alert, recommendation y notificación (en curso por el compañero).** La migración a microservicios de los cuatro módulos que siguen en energy-monitor-backend la está realizando el compañero del proyecto, Miguel Ángel García Artunduaga. Está en curso; sus servicios, puertos y fechas aún no están definidos, por lo que este manual documenta esos módulos tal como funcionan hoy dentro del backend.

- Limitar el historial del dispositivo al periodo de vinculación (RF-HOM-11).

- Consumir `HomeOwnershipTransferred` para avisar al nuevo responsable del hogar.

- Implementar el borrado y la transferencia de hogar en la aplicación móvil y completar sus pruebas automatizadas.

- Evaluar MQTT con QoS 1 para no perder lecturas.

- Resolver automáticamente los casos límite de las alertas de conectividad.

- Avisos de administrador por correo o push y negar en el servidor los endpoints de hogares a las cuentas administradoras (hoy solo la web les oculta esas pantallas).

- Evaluar la actualización del firmware por aire (OTA).

- Decidir qué hacer con el repositorio legado `energy-monitor-devices` (archivarlo o eliminarlo): ya no se usa para ejecutar nada.

# 17. Glosario

| **Término**           | **Definición**                                                                                                                        |
|-----------------------|---------------------------------------------------------------------------------------------------------------------------------------|
| ADSO                  | Análisis y Desarrollo de Software: tecnología SENA que forma parte del proyecto (ficha 3145556).                                      |
| API REST              | Interfaz de programación basada en HTTP que expone recursos en formato JSON bajo rutas versionadas (/api/v1).                         |
| API gateway           | Servicio que es la única entrada de la API (en-monitor-api-gateway): enruta cada petición al servicio dueño, valida el token y responde CORS. |
| Access token          | Token JWT de corta duración (15 minutos) que prueba la identidad del usuario en cada petición.                                        |
| Bounded context       | Contexto acotado de dominio (security, home, measurement…) con sus propias tablas y API interna; puede vivir en un microservicio o en un módulo del backend. |
| Intermediario (proxy) | Paquete del backend (security, device o measurement) que atiende el contrato api de un contexto ya extraído llamando al microservicio y traduciendo sus eventos; no tiene dominio ni tablas. |
| Microservicio         | Aplicación independiente, con su repositorio, su despliegue y sus tablas, que implementa un contexto (seguridad, dispositivos o mediciones). |
| RabbitMQ              | Broker de mensajes (AMQP) que transporta los eventos entre servicios.                                                                 |
| Transactional outbox  | Patrón por el que el mensaje se guarda en una tabla de outbox en la misma transacción que el cambio y un relay lo publica después en RabbitMQ. |
| X-Internal-Token      | Cabecera con el secreto compartido (INTERNAL_API_TOKEN) que exigen las rutas internas `/internal/**` entre servicios.                  |
| BCrypt                | Algoritmo de hash con sal utilizado para guardar contraseñas de forma irreversible.                                                   |
| Broker MQTT           | Servidor (Eclipse Mosquitto) que recibe los mensajes publicados por los dispositivos y los entrega a los suscriptores.                |
| CORS                  | Política HTTP que controla desde qué orígenes (p. ej. http://localhost:5173) el navegador puede llamar a la API.                      |
| Changeset             | Unidad de migración de Liquibase: instrucción versionada para crear o modificar tablas.                                               |
| CurrentUserResolver   | Componente de cada módulo o servicio que obtiene la identidad del usuario del claim `sub` del JWT.                                    |
| Docker Compose        | Herramienta que define y levanta contenedores (por ejemplo mysql:8.4) con un solo comando; la del proyecto está en energy-monitor-infra. |
| DTO                   | Objeto de transferencia de datos: estructura JSON de entrada o salida de un endpoint.                                                 |
| ER / MER              | Diagrama entidad-relación: modelo lógico de las tablas y sus relaciones.                                                              |
| JWT                   | JSON Web Token: token firmado (algoritmo RS256) que transporta la identidad del usuario.                                              |
| Liquibase             | Gestor de esquema: aplica changesets de base de datos de forma controlada y versionada.                                               |
| Módulo (Modulith)     | Unidad de código del backend con límites verificados por Spring Modulith; sólo se relaciona por su paquete api.                       |
| MQTT / QoS            | Protocolo de mensajería para dispositivos y nivel de calidad de entrega (0: como máximo una vez; 1: al menos una vez).                |
| PK / Llave primaria   | Campo que identifica de forma única una fila (en este proyecto, VARCHAR(10)).                                                         |
| FK / Llave foránea    | Campo que referencia a otra tabla dentro del mismo contexto. Entre contextos sólo se guardan IDs (sin FK SQL).                        |
| RBAC                  | Control de acceso basado en roles: system_role, permission, user_system_role y system_role_permission.                                |
| Refresh token         | Token opaco de larga duración guardado como hash SHA-256 que permite renovar el access token (rotación y detección de reutilización). |
| SHA-256               | Función de hash criptográfico usada para guardar refresh tokens, claves de dispositivo y, con pepper HMAC, códigos de recuperación.   |
| SPA                   | Single Page Application: aplicación web que se ejecuta entera en el navegador y navega sin recargar (React + react-router).           |
| Swagger / OpenAPI     | Documentación interactiva generada automáticamente en /swagger-ui.html.                                                               |
| Umbrales (thresholds) | Límites diario y mensual de consumo configurables por hogar que disparan alertas.                                                     |
| Borrado lógico        | No se ejecutan DELETE físicos: deleted_at con valor indica que el registro fue eliminado.                                             |
| Web Push              | Notificaciones del navegador entregadas aunque la pestaña esté cerrada, con claves VAPID.                                             |
| X-User-Id             | Cabecera que los servicios ignoran: la identidad se obtiene siempre del claim `sub` del JWT.                                          |

# 18. Anexos

## Anexo A. Configuración y comandos

**Variables de entorno.** Cada servicio lee sus secretos desde su propio archivo .env (importado por spring.config.import) cuya estructura se documenta en su .env.example y que se lee como `.properties` (valores sin comillas y sin `$`); en Docker, los valores salen de `infra/.env` (o `.env.prod` en el servidor) del repositorio energy-monitor-infra. El front web usa .env.local con VITE_API_BASE_URL y VITE_GOOGLE_CLIENT_ID, y la app móvil usa .env con EXPO_PUBLIC_API_BASE_URL. Por razones de seguridad no se publican valores reales:

| **Variable**                                                 | **Descripción**                                                                                      |
|--------------------------------------------------------------|------------------------------------------------------------------------------------------------------|
| MYSQL_PASSWORD / MYSQL_ROOT_PASSWORD                         | Contraseñas del usuario de aplicación y del usuario root de MySQL (definidas en infra/.env).         |
| SPRING_DATASOURCE_PASSWORD                                   | Contraseña de energy_app en cada servicio con base de datos (seguridad, dispositivos, mediciones y backend) y en sus pruebas. |
| MAIL_HOST / MAIL_PORT                                        | Backend: servidor SMTP y puerto (por defecto smtp.gmail.com:587).                                    |
| MAIL_USERNAME / MAIL_PASSWORD / MAIL_FROM                    | Backend: cuenta remitente y contraseña de aplicación para enviar correos.                            |
| SECURITY_RESET_PEPPER                                        | Servicio de seguridad: pepper del HMAC-SHA256 de los códigos de recuperación de contraseña.          |
| SECURITY_SESSION_COOKIE_SECURE                               | Servicio de seguridad: marca Secure de las cookies de sesión (solo se desactiva en sitios servidos por HTTP). |
| SECURITY_CORS_ALLOWED_ORIGINS                                | Gateway: orígenes permitidos por CORS (por defecto http://localhost:5173 en el ejemplo).             |
| SECURITY_JWT_PRIVATE_KEY_PATH / SECURITY_JWT_PUBLIC_KEY_PATH | Rutas absolutas al par de claves RSA usado por RS256: la privada solo en el servicio de seguridad; la pública en el gateway y en todos los servicios. |
| GOOGLE_CLIENT_ID / GOOGLE_CLIENT_SECRET                      | Servicio de seguridad: credenciales OAuth para iniciar sesión con Google; vacías, el botón responde 503. |
| INTERNAL_API_TOKEN                                           | Secreto de la cabecera X-Internal-Token; mismo valor en seguridad, dispositivos, mediciones y backend. |
| INTERNAL_API_ALLOWED_NETWORKS                                | Redes autorizadas a llamar a las rutas `/internal/**` de cada servicio.                              |
| SECURITY_SERVICE_URL / DEVICE_SERVICE_URL / MEASUREMENT_SERVICE_URL / BACKEND_URL | Gateway: dirección de cada servicio (8082, 8084, 8085 y 8083). El backend usa las tres primeras y el servicio de mediciones, DEVICE_SERVICE_URL. |
| BACKEND_INTERNAL_URL                                         | Servicios de seguridad, dispositivos y mediciones: dirección del API interno del backend.            |
| RABBITMQ_HOST / RABBITMQ_PORT / RABBITMQ_USERNAME / RABBITMQ_PASSWORD | Conexión a RabbitMQ de seguridad, dispositivos, mediciones y backend.                     |
| MQTT_BROKER_URL / MQTT_CLIENT_ID                             | Servicios de dispositivos y de mediciones: conexión al broker MQTT (listener interno 1883).          |
| MQTT_DEVICE_SERVICE_USER / MQTT_DEVICE_SERVICE_PASS          | Usuario MQTT del servicio de dispositivos (lee telemetría y estado, publica comandos).               |
| MQTT_MEASUREMENT_SERVICE_USER / MQTT_MEASUREMENT_SERVICE_PASS | Usuario MQTT del servicio de mediciones (solo lee telemetría).                                      |
| MEASUREMENT_MQTT_ENABLED                                     | Servicio de mediciones: activa la suscripción a la telemetría (desactivada por defecto; la pila de infra la activa). |
| MQTT_AUTH_ALLOWED_NETWORKS                                   | Servicio de dispositivos: redes autorizadas a llamar a las rutas internas de autenticación del broker. |
| DEVICE_MQTT_PUBLIC_HOST / DEVICE_MQTT_PUBLIC_PORT            | Servicio de dispositivos: dirección del broker que se entrega al dispositivo al vincularlo (puerto 8883). |
| DEVICE_OFFLINE_AFTER                                         | Servicio de dispositivos: silencio tras el cual un dispositivo se marca sin conexión (PT150S).        |
| NOTIFICATION_WEB_URL / NOTIFICATION_VAPID_PUBLIC_KEY / NOTIFICATION_VAPID_PRIVATE_KEY / NOTIFICATION_VAPID_SUBJECT | Backend: enlaces de los correos y claves VAPID del push del navegador. |
| EXPO_ACCESS_TOKEN                                            | Backend: token de Expo para el push de la app móvil; vacío, no hay push en la app.                  |
| ALERT_LIMIT_ZONE / RECOMMENDATION_EVALUATE_INTERVAL / RECOMMENDATION_ZONE | Backend: zona horaria de las alertas de límite y frecuencia y zona de las recomendaciones. |
| GATEWAY_READ_TIMEOUT / GATEWAY_RATE_LIMIT_REQUESTS / GATEWAY_RATE_LIMIT_WINDOW | Gateway: espera máxima de un servicio (30 s) y límite de POST públicos (20 por minuto). |
| BACKEND_CONTEXT / SECURITY_CONTEXT / DEVICE_CONTEXT / MEASUREMENT_CONTEXT / GATEWAY_CONTEXT / FRONTEND_CONTEXT | infra/.env: rutas de los repositorios si no están clonados uno al lado del otro. |
| VITE_API_BASE_URL / VITE_GOOGLE_CLIENT_ID (front web)        | URL base del gateway (por defecto http://localhost:8080) e ID de cliente de Google.                  |
| EXPO_PUBLIC_API_BASE_URL (app móvil)                         | URL base del gateway: 10.0.2.2 en el emulador de Android o la IP del PC en un teléfono.              |

> *Variables retiradas del backend: MQTT_BACKEND_USER / MQTT_BACKEND_PASS, MQTT_CLIENT_ID y DEVICE_OFFLINE_AFTER (el backend ya no se conecta al broker MQTT).*

**Comandos de operación:**

| **Comando**                             | **Descripción**                                                                    |
|-----------------------------------------|------------------------------------------------------------------------------------|
| docker compose up -d --build (en infra) | Levanta la pila completa local: MySQL, RabbitMQ, Mosquitto, los cinco servicios Java y la web (http://localhost). |
| docker compose -f docker-compose.yml -f docker-compose.dev.yml up -d mysql rabbitmq mosquitto (en infra) | Modo desarrollo: levanta solo MySQL, RabbitMQ y el broker. |
| docker compose down -v (en infra)       | Detiene los contenedores y elimina sus volúmenes (regenera el esquema con Liquibase). |
| docker run -p 8025:8025 axllent/mailpit | Levanta el servidor de correo de pruebas (interfaz en http://localhost:8025).      |
| ./mvnw spring-boot:run (en cada servicio; .\mvnw.cmd en Windows) | Arranca el gateway (8080), seguridad (8082), backend (8083), dispositivos (8084) o mediciones (8085). |
| ./mvnw test                             | Ejecuta la suite de pruebas del servicio.                                          |
| ./mvnw package                          | Genera el JAR ejecutable en la carpeta /target.                                    |
| npm install / npm run dev               | Instala dependencias y arranca la aplicación web (http://localhost:5173).          |
| npm test / npm run lint                 | Ejecuta las pruebas y el análisis estático del front web.                          |
| npm run start / npm run android         | Arranca la aplicación móvil en Expo (dispositivo o emulador Android).              |
| e2e/device-flow.sh / e2e/measurement-flow.sh (en infra) | Pruebas de punta a punta sobre la pila local.                      |
| deploy/deploy.sh \<componente\> (en el servidor) | Despliega un componente; lo invoca GitHub Actions.                        |
| arduino-cli compile / upload (en firmware/esp32-pzem-monitor) | Compila y carga el firmware del dispositivo por USB.         |

**Verificación tras la instalación:** GET /actuator/health de cada servicio responde {"status":"UP"} (docker compose ps muestra gateway, security, device, measurement y backend como healthy), la pantalla de inicio de sesión se carga en el navegador, el registro de una cuenta envía el código de verificación, los hogares creados aparecen en el panel de la aplicación y la primera lectura de un dispositivo vinculado aparece en el hogar.

# Anexo B. MER por contexto

Modelo entidad-relación desagregado por contexto para facilitar su lectura. Las convenciones (snake_case, llaves primarias VARCHAR(10), columnas de auditoría y borrado lógico) son las mismas del diagrama completo de la sección 11.1.

**Figura 5.** *Contexto Security: personas, usuarios, sesiones, tokens, roles, permisos y auditoría.*

**Figura 6.** *Contextos Home Management y Devices: hogares, miembros, umbrales y dispositivos.*

**Figura 7.** *Contextos Monitoring and measurements y Alerts: mediciones, niveles de consumo y alertas.*

**Figura 8.** *Contextos Recommendations y Notifications: recomendaciones y registro de entregas.*

# Anexo C. Diagrama de clases por dominio

**Figura 9.** *Dominios Domain_Security y Domain_Access_Control, con el patrón Strategy y las enumeraciones.*

**Figura 10.** *Dominios Domain_Home_Management y Domain_Devices.*

**Figura 11.** *Dominios Domain_Measurements y Domain_Notifications, con Observer y Factory Method.*
