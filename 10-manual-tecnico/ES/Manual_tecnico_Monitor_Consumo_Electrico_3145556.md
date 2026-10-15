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
| Versión              | 1.1                                                                             |
| Fecha de elaboración | 07 de octubre de 2026 (actualización: 13 de octubre de 2026)                    |

Año 2026

# 2. Control de versiones del documento

El historial de cambios del presente manual se registra en la siguiente tabla. Cada modificación relevante del software debe verse reflejada en una nueva versión del documento.

| **Versión** | **Fecha**  | **Descripción del cambio**                                                                                                                                                                                                   | **Responsable**      |
|-------------|------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------|
| 1.0         | 07/10/2026 | Creación inicial del manual técnico: descripción general, tecnologías, arquitectura, estructura de carpetas, base de datos, módulos, API y seguridad de la versión vigente del software.                                      | Equipo desarrollador |
| 1.0         | 07/10/2026 | Incorporación de los diagramas oficialmente aprobados: MER, diagrama de clases, arquitectura y secuencia de autenticación, además del diccionario de datos.                                                                  | Equipo desarrollador |
| 1.1         | 13/10/2026 | Actualización al estado final: siete módulos activos, 29 tablas, dispositivos y broker MQTT, API completa, identidad desde el JWT, hash de claves de dispositivo, aplicación móvil integrada con la API y limitaciones vigentes. | Equipo desarrollador |

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

El sistema permite monitorear el consumo de energía eléctrica en los hogares: un dispositivo de medición (ESP32 con sensor PZEM-004T V3) envía sus lecturas por MQTT, el backend las almacena, calcula estadísticas, genera alertas y recomendaciones de ahorro y envía notificaciones por correo y por el navegador, y las aplicaciones web y móvil permiten consultar la información. La solución se compone de una API REST, un broker MQTT, un dispositivo de medición, una aplicación web, una aplicación móvil Android y una base de datos MySQL.

Este manual está dirigido a desarrolladores, personal de soporte técnico, el instructor evaluador y en general a cualquier persona que deba comprender, ampliar o dar mantenimiento al software. Su propósito es describir cómo está construido el sistema, no cómo se utiliza: para el uso cotidiano existe el manual de usuario, para la puesta en marcha el manual de instalación y para la gestión de cuentas y respaldos el manual de administrador.

La información aquí consignada fue obtenida directamente del código fuente de los repositorios del proyecto (backend, web, móvil y dispositivos), de los changesets de Liquibase y de los documentos de esta documentación final, de modo que corresponda a la versión final del software entregado.

| **Fuente de información**            | **Ubicación**                                                                                                                                    |
|--------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------|
| Código fuente del backend            | energy-monitor-backend (Java 21 + Spring Boot 4.1.1)                                                                                             |
| Código fuente de la aplicación web   | Energy-Monitor-application (React 19 + Vite 8)                                                                                                   |
| Código fuente de la aplicación móvil | Energy-Monitor-Mobile (Expo SDK 57 + React Native 0.86)                                                                                          |
| Firmware y broker MQTT               | energy-monitor-devices (ESP32 + PZEM-004T V3, firmware 1.3.8; Mosquitto 2)                                                                       |
| Especificación de requisitos         | [`02-especificacion-requisitos`](../../02-especificacion-requisitos/ES/especificacion-requisitos.md) (códigos RF-xxx y RNF-xxx)                  |
| Diseño de software                   | [`05-diseno-software`](../../05-diseno-software/ES/README.md): modelo arquitectónico (05.1), modelo de datos (05.2), modelo de clases (05.3), patrones (05.4) e interfaces (05.5) |
| Modelo de datos                      | Changesets de Liquibase en `src/main/resources/db/changelog` y [`05.2-modelo-de-datos.md`](../../05-diseno-software/ES/05.2-modelo-de-datos.md) |

# 5. Objetivo

> Documentar la arquitectura, las tecnologías, la estructura del proyecto, la base de datos, los módulos técnicos, la API, la seguridad y las recomendaciones de mantenimiento del sistema «Monitor de Consumo Eléctrico», de manera que un desarrollador ajeno al equipo pueda comprender el software, continuar su desarrollo y darle soporte sin necesidad de conocimiento previo del proyecto.

- Describir la arquitectura del monolito modular, la comunicación con los dispositivos y los patrones de diseño empleados en cada componente.

- Detallar las tecnologías y versiones exactas utilizadas en el backend, en los dos front-ends y en el dispositivo.

- Explicar la organización del código fuente y la función de cada carpeta.

- Documentar el modelo de datos: diagrama entidad-relación, tablas principales y diccionario de datos.

- Relacionar cada módulo técnico con sus archivos, tablas y endpoints.

- Describir los servicios expuestos por la API REST y los mecanismos de seguridad aplicados.

- Establecer recomendaciones para la actualización, respaldo y soporte del sistema.

# 6. Alcance técnico

El manual técnico cubre los siguientes aspectos del sistema:

| **Sí cubre**                                                                            | **No cubre**                                                                                                         |
|-----------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------|
| Arquitectura general, capas y patrones de diseño del backend y de los front-ends.       | Procedimientos de instalación, puesta en marcha y despliegue en producción (corresponde al manual de instalación).   |
| Tecnologías, librerías y versiones utilizadas, así como sus dependencias.               | Pasos de uso, pantallas y mensajes para el usuario final (corresponde al manual de usuario).                         |
| Estructura de carpetas y archivos de los repositorios del proyecto.                     | Gestión de usuarios, roles, reportes administrativos y copias de seguridad (corresponde al manual de administrador). |
| Base de datos: MER, tablas, relaciones y diccionario de datos.                          | Diseño eléctrico y cableado del dispositivo de medición (se describen en el manual de instalación).                  |
| Módulos técnicos del backend, front web y front móvil, con sus archivos y tablas.       | Historias de usuario, casos de uso y criterios de aceptación (documentos de análisis).                               |
| API REST: método, ruta, parámetros, autenticación y respuestas, y el contrato MQTT.     | Pruebas de carga.                                                                                                    |
| Seguridad: autenticación, autorización, cifrado, validaciones y limitaciones conocidas. | Costos, plan de proyectos e informes de gestión.                                                                     |
| Control de versiones y recomendaciones de mantenimiento.                                |                                                                                                                      |

**Versión documentada:** software disponible en los repositorios al 13 de octubre de 2026. Los trabajos que siguen pendientes se describen en las secciones 14.2 y 16.4.

# 7. Descripción general del sistema

## 7.1 ¿Qué es el sistema?

«Monitor de Consumo Eléctrico» es una solución web y móvil que permite a los usuarios conocer cuánta energía consumen sus hogares, identificar los dispositivos que más energía utilizan, recibir alertas cuando el consumo supera los límites establecidos y obtener recomendaciones para reducir el gasto y la factura eléctrica. Las lecturas las toma un dispositivo de medición conectado a cada electrodoméstico.

## 7.2 Componentes principales

| **Componente**       | **Tipo**                                  | **Función**                                                                                                                                                                                  |
|----------------------|-------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| API REST             | Servidor (Java 21 + Spring Boot)          | Expone los servicios de autenticación, hogares, dispositivos, mediciones, alertas, recomendaciones y notificaciones. Es la única fuente de verdad de los datos.                               |
| Aplicación web       | SPA (React 19 + Vite 8)                   | Interfaz de consulta y gestión: acceso, panel de hogares, consumo, historial, usuarios, dispositivos (vinculación por Bluetooth), umbrales, alertas, recomendaciones y configuración.         |
| Aplicación móvil     | App Android (Expo SDK 57)                 | Interfaz móvil conectada a la API REST: cuenta, hogares, alertas, recomendaciones y preferencias de notificación. Solo Android.                                                               |
| Broker MQTT          | Eclipse Mosquitto 2 + mosquitto-go-auth   | Recibe la telemetría y el estado de los dispositivos por MQTT 3.1.1 con TLS y autentica a cada cliente contra el backend.                                                                    |
| Dispositivo          | ESP32 + sensor PZEM-004T V3 (firmware 1.3.8) | Mide tensión, corriente, potencia activa y energía; publica una lectura cada 60 segundos y su estado cada cinco minutos.                                                                  |
| Base de datos        | MySQL 8.4                                 | Almacena usuarios, hogares, dispositivos, mediciones, alertas, recomendaciones y notificaciones (29 tablas).                                                                                 |
| Correo electrónico   | SMTP (Mailpit en local)                   | Envía los códigos de verificación y recuperación y los correos de alerta y de recomendación.                                                                                                 |

## 7.3 Roles del sistema

| **Rol**                      | **Descripción**                                                                                  | **Cómo se implementa**                                                                                                               |
|------------------------------|--------------------------------------------------------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------|
| Propietario de hogar (OWNER) | Crea y administra un hogar: invita miembros, configura umbrales, gestiona dispositivos, transfiere la propiedad o elimina el hogar. | Enum OWNER de la tabla user_home, con permisos de dominio en el módulo home (estrategia por rol). Cada hogar tiene exactamente un OWNER. |
| Miembro de hogar (MEMBER)    | Consulta el consumo, el historial, las alertas y la configuración del hogar al que fue invitado. | Enum MEMBER de la tabla user_home. Un usuario puede ser OWNER en un hogar y MEMBER en otro.                                           |
| Usuario del sistema          | Cuenta personal con perfil, preferencias de idioma, tema y notificaciones.                       | Tabla user con estado ACTIVE / INACTIVE / BLOCKED.                                                                                   |
| Control de acceso (RBAC)     | Permisos funcionales globales agrupados en roles del sistema (códigos como METER.READ, ALERT.READ). | Tablas system_role, permission, user_system_role y system_role_permission; se consultan en cada petición con CheckPermission (no viajan en el JWT). |
| Administrador                | Gestión global del sistema y de cuentas.                                                         | Parcial: los puertos de asignación de roles existen en el módulo security; la interfaz administrativa aún no está expuesta por HTTP. |

## 7.4 Estado de implementación por dominio

El sistema se documenta tal como está construido. La tabla relaciona cada grupo de requisitos de la [especificación de requisitos](../../02-especificacion-requisitos/ES/especificacion-requisitos.md) con el módulo que lo implementa.

| **Grupo de requisitos**                           | **Módulo del software**                            | **Estado**                                                                                                                                                                                     |
|---------------------------------------------------|----------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| RF-SEC – Gestión de Usuarios y Perfiles           | security (backend) + auth y account (web y móvil)  | Implementado: registro, verificación de correo, inicio de sesión (incluido Google), renovación y cierre de sesión, recuperación y cambio de contraseña, perfil, permisos y eliminación de cuenta. |
| RF-HOM – Gestión de Hogares                       | home (backend) + dashboard y DetailHome (web)      | Implementado: crear hogar, código de acceso, miembros, favoritos, umbrales, eliminación del hogar y transferencia de propiedad. Pendiente: historial del dispositivo según el periodo de vinculación (RF-HOM-11). |
| RF-DEV – Gestión de Dispositivos                  | device (backend) + devices (web)                   | Implementado: catálogo de electrodomésticos, vinculación por Bluetooth, edición, nuevas credenciales de Wi-Fi, desvinculación y autorización MQTT.                                            |
| RF-MED – Mediciones y Consumo                     | measurement (backend) + Consumption (web)          | Implementado: ingesta MQTT de telemetría y estado, estadísticas, resumen e historial de consumo del hogar.                                                                                    |
| RF-ALE – Alertas                                  | alert (backend) + Notifications (web y móvil)      | Implementado: alertas de dispositivo vinculado, desconexión, consumo alto o crítico y límite diario o mensual.                                                                                |
| RF-REC – Recomendaciones de Ahorro                | recommendation (backend)                           | Implementado: cinco reglas de ahorro evaluadas cada hora.                                                                                                                                      |
| RF-NOT – Notificaciones                           | notification (backend)                             | Implementado: entrega por correo y Web Push según las preferencias de cada usuario.                                                                                                           |
| RF-CLI – Aplicaciones web y móvil                 | Aplicación web y aplicación móvil                  | Web implementada. Móvil integrada con la API; eliminar y transferir un hogar solo existen en la web.                                                                                          |

# 8. Tecnologías utilizadas

La siguiente tabla registra los lenguajes, frameworks, librerías, herramientas de base de datos, de diseño y de control de versiones empleados en el proyecto, con su versión o detalle de uso.

| **Componente**                 | **Tecnología**                           | **Versión / detalle**                                                                              |
|--------------------------------|------------------------------------------|----------------------------------------------------------------------------------------------------|
| Lenguaje del backend           | Java                                     | Java 21 (LTS)                                                                                      |
| Framework del backend          | Spring Boot                              | Spring Boot 4.1.1 (Spring Framework 7)                                                             |
| Estilo arquitectónico          | Spring Modulith + arquitectura hexagonal | spring-modulith 2.1.1, 7 módulos (bounded contexts)                                                |
| Construcción del backend       | Maven Wrapper                            | mvnw / mvnw.cmd, spring-boot-maven-plugin                                                          |
| Persistencia                   | Spring Data JPA + Hibernate              | ddl-auto = validate (el esquema lo define Liquibase)                                               |
| Migraciones de base de datos   | Liquibase                                | db/changelog con changesets por módulo y rollback                                                  |
| Motor de base de datos         | MySQL                                    | MySQL 8.4, collation utf8mb4_unicode_ci, esquema energy_monitor                                    |
| Autenticación y autorización   | Spring Security + OAuth2 Resource Server | JWT RS256 con par de claves RSA (PEM), BCrypt para contraseñas                                     |
| Validación de datos            | Jakarta Bean Validation                  | @NotBlank, @Size, @NotNull, @Positive, @Pattern                                                    |
| Documentación de la API        | springdoc-openapi                        | springdoc 3.1.1, Swagger UI y /v3/api-docs                                                         |
| Correo electrónico y push      | spring-boot-starter-mail + Web Push      | SMTP con STARTTLS (Mailpit en local); notificaciones push con claves VAPID                         |
| Mensajería con dispositivos    | MQTT 3.1.1 con TLS                       | Eclipse Mosquitto 2 + mosquitto-go-auth; el backend se suscribe como cliente MQTT                  |
| Monitoreo y salud              | Spring Boot Actuator                     | /actuator/health y /actuator/info                                                                  |
| Pruebas del backend            | JUnit 5 + Spring Boot Test               | 131 archivos de prueba en src/test/java (más de 1.000 casos)                                       |
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

El archivo pom.xml del backend declara como dependencias centrales spring-boot-starter-webmvc, spring-boot-starter-data-jpa, spring-boot-starter-security, spring-boot-starter-oauth2-resource-server, spring-boot-starter-validation, spring-boot-starter-liquibase, spring-boot-starter-mail, spring-boot-starter-actuator, spring-modulith-starter-core (con sus módulos de observabilidad y actuator), springdoc-openapi-starter-webmvc-ui 3.1.1, mysql-connector-j (runtime) y el conjunto de starters de prueba (\*-test y spring-modulith-starter-test).

## 8.2 Scripts disponibles

| **Repositorio** | **Script**                      | **Función**                                                                      |
|-----------------|---------------------------------|----------------------------------------------------------------------------------|
| backend         | mvnw spring-boot:run            | Compila y arranca la API en http://localhost:8080.                               |
| backend         | mvnw test                       | Ejecuta la suite de pruebas (requiere MySQL en ejecución).                       |
| backend         | mvnw package                    | Genera el archivo JAR ejecutable en /target.                                     |
| backend         | docker compose up -d            | Levanta el contenedor MySQL 8.4 con el esquema energy_monitor.                   |
| web             | npm run dev                     | Inicia el servidor de desarrollo Vite en http://localhost:5173.                  |
| web             | npm run build / npm run preview | Construye la aplicación en /dist y la sirve para revisión.                       |
| web             | npm test                        | Ejecuta las pruebas con Vitest.                                                  |
| web             | npm run lint                    | Analiza el código con ESLint.                                                    |
| móvil           | npm run start                   | Inicia Metro Bundler de Expo.                                                    |
| móvil           | npm run android                 | Ejecuta la aplicación en un emulador o dispositivo Android.                      |
| móvil           | npm run lint                    | Analiza el código con ESLint (no hay pruebas automatizadas).                     |
| dispositivos    | docker compose up -d            | Levanta el broker Mosquitto de desarrollo (carpeta devices/docker).              |
| dispositivos    | arduino-cli compile / upload    | Compila y carga el firmware por USB (no hay actualización por aire, OTA).        |

# 9. Arquitectura del sistema

## 9.1 Arquitectura general

El sistema adopta una arquitectura cliente-servidor en capas con un canal adicional para los dispositivos. La capa de presentación está conformada por la aplicación web y la aplicación móvil; la capa de lógica corresponde a la API REST construida en Spring Boot; la capa de datos corresponde a MySQL, accedida mediante JPA/Hibernate y gestionada con Liquibase; y los dispositivos de medición publican sus lecturas por MQTT con TLS a un broker Mosquitto que consulta al backend para autenticarlos.

> **Imágenes y diagramas (Figuras 1 a 11):** todas las figuras de este documento se encuentran en la siguiente carpeta de Google Drive.  
> https://drive.google.com/drive/folders/1tIzih2FKP8xte4vKYEtrWThFmZxR1oBX?usp=sharing
>
> **Figuras posiblemente desactualizadas (`[POR CONFIRMAR]` tras revisarlas en la carpeta):** las Figuras 3 y 5 a 8 (MER) se dibujaron antes de las últimas tablas y columnas y no incluyen `notification_preference`, `push_subscription`, `user.google_subject`, `home_thresholds.limit_period` ni `device.api_key_hash`; las Figuras 4 y 9 a 11 (diagrama de clases) pueden mostrar como pendientes las clases de estrategia por rol y los módulos device y recommendation; y las Figuras 1 y 2 (arquitectura y secuencia de autenticación) pueden mostrar la cabecera X-User-Id o un sistema sin dispositivos ni broker.

**Figura 1.** *Arquitectura general del sistema (tres capas: presentación, lógica y datos).*

| **Capa**         | **Tecnología**                                        | **Responsabilidad**                                                                                                  |
|------------------|-------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------|
| Presentación     | React 19 (web) y Expo SDK 57 (móvil, Android)         | Interfaz de usuario, validación de formularios, manejo de sesión y consumo de la API REST.                           |
| Lógica           | Spring Boot 4.1.1 + Spring Security + Spring Modulith | Autenticación, autorización, casos de uso, validación de reglas de negocio, ingesta MQTT y publicación de eventos.   |
| Datos            | MySQL 8.4 + Spring Data JPA + Liquibase               | Persistencia de las 29 tablas, esquema versionado y consultas optimizadas.                                           |
| Dispositivos     | ESP32 + PZEM-004T V3 + Mosquitto 2                    | Medición, publicación MQTT con TLS (puerto 8883) y autenticación de cada dispositivo contra el backend.              |
| Transversal      | Docker Compose, Mailpit y Actuator                    | Contenedores de base de datos y broker, correo de pruebas y punto de verificación de salud del servicio.             |

## 9.2 Estilo arquitectónico y patrones de diseño

- **Monolito modular (Spring Modulith).** El backend es una única aplicación organizada en siete módulos o contextos acotados (security, home, device, measurement, alert, recommendation y notification). Cada módulo declara sus dependencias permitidas en el archivo package-info.java y sólo puede comunicarse con los demás mediante los tipos de su paquete api; esta restricción se verifica automáticamente con la prueba ModularityTests.

- **Arquitectura hexagonal por módulo.** Dentro de cada módulo conviven las capas api, domain, application (puertos de entrada `port/in`, puertos de salida `port/out`, casos de uso `usecase`), adapter (entrada `adapter/in/web`, `adapter/in/event`, `adapter/in/scheduler` y salida `adapter/out/persistence`, `out/event`, `out/client`) e infrastructure con la configuración de Spring.

- **Eventos en proceso (sin Kafka).** Los módulos se comunican con eventos de Spring en memoria: `MeasurementRecorded` (measurement → alert), `AlertRaised` (alert → notification), `RecommendationCreated` (recommendation → notification), `AccountDeleted` (security → home), `HomeDeleted` (home → device) y `HomeOwnershipTransferred` (home, sin consumidor todavía). La pérdida de conectividad de un dispositivo se detecta mediante `device_status_log` (mensaje de última voluntad o 150 segundos de silencio) y llega a alert como `DeviceConnectivityLost`.

- **Recomendaciones por programador.** El módulo recommendation no escucha `MeasurementRecorded`: `RecommendationScheduler` ejecuta cada hora las reglas de ahorro y publica `RecommendationCreated` por cada recomendación nueva.

- **API REST sin estado (stateless).** No se usan cookies ni sesiones HTTP: cada petición transporta un JWT en la cabecera Authorization. La política de CORS se configura de forma explícita y cerrada por defecto.

- **Front-end por capacidades (features).** Tanto la web como la móvil organizan el código en carpetas por capacidad funcional (auth, dashboard, homes, notifications…) con componentes, estilos, validaciones zod y pruebas propias. El estado global se maneja con React Context y la sesión se persiste en localStorage (web).

## 9.3 Comunicación entre componentes

Las aplicaciones cliente se comunican con la API mediante peticiones HTTP en formato JSON bajo el prefijo /api/v1. Toda petición autenticada incluye la cabecera Authorization: Bearer \<accessToken\>. La identidad del usuario se obtiene exclusivamente del claim `sub` del JWT mediante `CurrentUserResolver`; la cabecera X-User-Id se ignora. El front web renueva automáticamente el access token ante una respuesta 401 mediante POST /auth/refresh, con rotación del refresh token y bloqueo entre pestañas con la API Web Locks.

| **Elemento de comunicación** | **Detalle**                                                                                      |
|------------------------------|--------------------------------------------------------------------------------------------------|
| Protocolo (clientes)         | HTTP/1.1 sobre HTTPS en producción; HTTP en desarrollo local.                                    |
| Formato                      | JSON (Content-Type: application/json).                                                           |
| Prefijo de la API            | /api/v1 (todas las rutas del backend, salvo las internas del broker).                            |
| Autenticación                | Authorization: Bearer \<JWT RS256\> con expiración de 15 minutos.                                |
| Identidad                    | Claim `sub` del JWT; X-User-Id se ignora.                                                        |
| Origen permitido (CORS)      | Cerrado por defecto; los orígenes se definen con SECURITY_CORS_ALLOWED_ORIGINS (p. ej. http://localhost:5173), sin credenciales. Métodos: GET, POST, PUT, DELETE y OPTIONS. |
| Manejo de errores            | Respuesta con estructura ApiError: timestamp, status, error, message, path y fieldErrors.        |
| Documentación                | OpenAPI y Swagger UI disponibles en /v3/api-docs y /swagger-ui.html.                             |
| Salud del servicio           | GET /actuator/health devuelve {"status":"UP"}.                                                   |
| Dispositivos (MQTT)          | MQTT 3.1.1; puerto 8883 con TLS (dispositivos) y 1883 interno (backend).                         |

**Contrato MQTT.**

| **Parámetro**             | **Valor**                                                                                                                              |
|---------------------------|----------------------------------------------------------------------------------------------------------------------------------------|
| Client ID                 | `esp32-{deviceId}`                                                                                                                     |
| Usuario / contraseña      | `device_code` / `api_key` (el backend solo guarda el hash de la clave)                                                                  |
| Keep alive y última voluntad | Keep alive de 15 segundos; el broker publica la última voluntad aproximadamente a los 22 segundos de una caída de energía             |
| QoS                       | 0 para telemetría y estado; 1 para el comando                                                                                          |
| Cadencia                  | Telemetría cada 60 segundos; estado cada cinco minutos y al conectar; sin conexión tras 150 segundos de silencio                       |
| Tópico de telemetría      | `energy-monitor/devices/{deviceId}/telemetry`                                                                                          |
| Tópico de estado          | `energy-monitor/devices/{deviceId}/status` (retenido)                                                                                  |
| Tópico de comando         | `energy-monitor/devices/{deviceId}/command` (QoS 1, retenido): `{"cmd":"unlink"}` al desvincular el dispositivo                         |

El broker autentica a cada dispositivo contra el backend (`/internal/mqtt/auth/user`) con una caché de cinco minutos, y su ACL permite publicar únicamente en los dos tópicos del propio dispositivo (`/internal/mqtt/auth/acl`). El backend toma el `deviceId` del tópico y descarta los mensajes cuyo cuerpo declare otro dispositivo.

La figura siguiente describe el flujo completo de un inicio de sesión y el acceso posterior a un recurso protegido.

**Figura 2.** *Secuencia de autenticación: inicio de sesión, acceso protegido y renovación de token.*

# 10. Estructura del proyecto

El proyecto se distribuye en cuatro repositorios de código (backend, front web, app móvil y dispositivos) y la documentación final. La tabla siguiente resume su contenido y la forma de ejecutarlos.

| **Repositorio**            | **Tipo**                           | **Tecnología**                                          | **Punto de entrada**                                                 | **Ejecución**                                  |
|----------------------------|------------------------------------|---------------------------------------------------------|----------------------------------------------------------------------|------------------------------------------------|
| energy-monitor-backend     | API REST (backend)                 | Java 21 + Spring Boot 4.1.1 + Maven                     | src/main/java/com/energymonitor/EnergyMonitorBackendApplication.java | .\mvnw.cmd spring-boot:run (puerto 8080)       |
| Energy-Monitor-application | Aplicación web (front-end)         | React 19 + Vite 8 (JavaScript)                          | src/main.jsx sobre index.html                                        | npm install / npm run dev (puerto 5173)        |
| Energy-Monitor-Mobile      | Aplicación móvil (front-end)       | Expo SDK 57 + React Native 0.86                         | src/app/\_layout.tsx (expo-router/entry)                             | npm install / npm run start                    |
| energy-monitor-devices     | Firmware del dispositivo y broker  | ESP32 + PZEM-004T V3 (Arduino) y Mosquitto 2            | esp32-pzem-monitor/esp32-pzem-monitor.ino y docker/docker-compose.yml | arduino-cli upload / docker compose up -d     |
| energy-monitor-docs-final  | Documentación final                | Markdown (ES y EN)                                      | README.md                                                            | No requiere ejecución                          |

## 10.1 Estructura del backend

```
energy-monitor-backend/
├── pom.xml                       definición Maven y dependencias
├── mvnw / mvnw.cmd               Maven Wrapper (no requiere Maven instalado)
├── Dockerfile                    imagen del backend (Temurin 21, usuario sin privilegios)
├── docker-compose.yml            contenedor MySQL 8.4 (puerto 3306)
├── .env.example                  plantilla de variables de entorno
├── README.md                     instrucciones del backend
└── src/
    ├── main/
    │   ├── java/com/energymonitor/
    │   │   ├── EnergyMonitorBackendApplication.java   punto de entrada
    │   │   ├── security/         identidad, JWT, Google, RBAC y auditoría
    │   │   ├── home/             hogares, miembros, umbrales, eliminación y transferencia
    │   │   ├── device/           dispositivos, credenciales, autorización MQTT y estado
    │   │   ├── measurement/      mediciones, ingesta MQTT, consumo del hogar
    │   │   ├── alert/            alertas de consumo, límite, dispositivo y conectividad
    │   │   ├── recommendation/   reglas de ahorro y bandeja de recomendaciones
    │   │   └── notification/     correo, Web Push y preferencias
    │   └── resources/
    │       ├── application.yaml                  configuración base
    │       ├── application-local.yaml(.example)  perfil local y claves
    │       └── db/changelog/                     maestro Liquibase + changesets
    │           ├── db.changelog-master.xml
    │           ├── security/ home/ device/ measurement/
    │           └── alert/ recommendation/ notification/
    └── test/java/com/energymonitor/              131 archivos de prueba
```

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
│   ├── in/event/            escuchadores de eventos
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
    │   ├── auth/         authApi, googleSignIn, session (localStorage)
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
| backend/src/main/java/com/energymonitor/\<módulo\>/domain/model            | Entidades de dominio puras, sin dependencias de Spring.                                  |
| backend/src/main/java/com/energymonitor/\<módulo\>/application             | Casos de uso, puertos de entrada y de salida, comandos, resultados y excepciones.        |
| backend/src/main/java/com/energymonitor/\<módulo\>/adapter/in/web          | Controladores REST, DTOs y manejadores de excepciones (@RestControllerAdvice).           |
| backend/src/main/java/com/energymonitor/\<módulo\>/adapter/out/persistence | Entidades JPA, repositorios, mappers y adaptadores de persistencia.                      |
| backend/src/main/resources/db/changelog                                    | Changesets de Liquibase que construyen el esquema de MySQL, con su rollback.             |
| backend/src/test/java                                                      | Pruebas unitarias, de integración web y de seguridad; verificación de límites de módulo. |
| web/src/routes                                                             | Definición de rutas, protecciones y layouts de la SPA.                                   |
| web/src/services                                                           | Capa de acceso a la API: cliente HTTP, interceptores, sesión y endpoints por dominio.    |
| web/src/features                                                           | Pantallas y componentes agrupados por funcionalidad.                                     |
| web/src/design                                                             | Componentes reutilizables de interfaz (botones, tarjetas, campos) y sus estilos.         |
| mobile/src/app                                                             | Rutas file-based de Expo Router que montan cada pantalla.                                |
| mobile/src/services                                                        | Cliente de la API REST de la aplicación móvil (mismo patrón que la web).                 |
| mobile/src/features                                                        | Pantallas, componentes, hooks y validaciones por funcionalidad.                          |
| devices/esp32-pzem-monitor                                                 | Firmware del dispositivo y archivos de configuración.                                    |
| devices/docker                                                             | Broker Mosquitto de desarrollo y despliegue de producción (broker, backend y MySQL).     |

# 11. Base de datos

**Motor:** MySQL 8.4, ejecutado en contenedor Docker (docker-compose.yml) con collation utf8mb4_unicode_ci.

**Esquema:** energy_monitor, con 29 tablas distribuidas en siete contextos. El usuario de aplicación es energy_app.

**Gestión del esquema:** Liquibase aplica los changesets versionados en src/main/resources/db/changelog; Hibernate está configurado con ddl-auto = validate, es decir, sólo valida que el esquema coincida con las entidades y nunca crea tablas por sí mismo.

**Convenciones:** nombres en snake_case; llaves primarias de tipo VARCHAR(10); columnas de auditoría created_at, updated_at y deleted_at en todas las tablas; borrado lógico (ningún DELETE físico por defecto); índices con el patrón idx\_\[tabla\]\_\[columnas\]. Las referencias entre contextos se almacenan como identificadores a nivel de aplicación y no generan llaves foráneas SQL.

## 11.1 Diagrama entidad-relación

El siguiente diagrama muestra las entidades del sistema, agrupadas por contexto (Security, Home Management, Devices, Monitoring and measurements, Alerts, Recommendations y Notifications), con sus atributos, llaves y relaciones. Las relaciones punteadas entre contextos representan referencias de aplicación (sin foreign key en SQL). El diagrama publicado en la carpeta de figuras muestra 27 entidades: faltan `notification_preference` y `push_subscription` y varias columnas posteriores (ver la nota de la sección 9.1).

> **Recomendación de lectura:** por su tamaño, el diagrama se presenta en página horizontal; en Word puede ampliarse con el zoom para leer los atributos de cada entidad. En el Anexo B se incluye el mismo modelo desagregado por contexto, con mayor legibilidad.
>
**Figura 3.** *Modelo entidad-relación (MER) completo del sistema «Monitor de Consumo Eléctrico».*

## 11.2 Tablas principales

La tabla siguiente lista las 29 tablas del esquema, el contexto al que pertenecen, su descripción, su llave primaria y las relaciones declaradas en el diccionario de datos.

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

## 11.3 Diccionario de datos

El diccionario de datos describe campo por campo cada tabla del esquema: restricción de clave, tipo de dato, tamaño y descripción. Se reconstruyó a partir de los changesets de Liquibase del backend, que son la fuente de verdad del esquema; para el modelo de datos con sus reglas de integridad véase también [`05.2-modelo-de-datos.md`](../../05-diseno-software/ES/05.2-modelo-de-datos.md).

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


# 12. Módulos técnicos

## 12.1 Vista general de los módulos

El backend está dividido en siete módulos (bounded contexts), todos activos.

| **Módulo**     | **Responsabilidad**                                                                    | **Controladores / casos de uso**                                                                                          | **Estado** |
|----------------|----------------------------------------------------------------------------------------|---------------------------------------------------------------------------------------------------------------------------|------------|
| security       | Registro, autenticación (incluido Google), sesiones, contraseñas, perfil, RBAC, auditoría y borrado de cuenta | AuthController + 18 casos de uso                                                                           | Activo     |
| home           | Hogares, miembros, favoritos, código de acceso, umbrales, eliminación del hogar y transferencia de propiedad | HomeController, HomeThresholdsController, HomeTypeController + 15 casos de uso                              | Activo     |
| device         | Dispositivos, vinculación, credenciales, estado de conexión y autorización del broker  | HomeDeviceController, ApplianceTypeController, BrokerAuthController + 3 servicios                                         | Activo     |
| measurement    | Ingesta de telemetría y estado por MQTT, histórico, estadísticas y consumo del hogar   | MeasurementController, ConsumptionLevelController, HomeConsumptionController + consumidores MQTT                          | Activo     |
| alert          | Alertas de consumo, límite, dispositivo vinculado y conectividad, y su bandeja         | AlertController + 10 casos de uso                                                                                         | Activo     |
| recommendation | Reglas de ahorro evaluadas cada hora y bandeja de recomendaciones                      | RecommendationController, RecommendationScheduler + 2 servicios                                                           | Activo     |
| notification   | Entrega por correo y Web Push, preferencias y registro de entregas                     | NotificationChannelsController + 5 casos de uso                                                                           | Activo     |

## 12.2 Diagrama de clases del dominio

El diagrama de clases modela las clases, interfaces y enumeraciones del dominio, junto con los patrones de diseño aplicados: Strategy para la autorización por rol (OwnerPermissions y MemberPermissions), Observer (eventos con nombre) para la reacción a las mediciones y Factory Method para la creación de alertas, recomendaciones y notificaciones. Los patrones están descritos en [`05.4-patrones-de-diseno.md`](../../05-diseno-software/ES/05.4-patrones-de-diseno.md) y las clases en [`05.3-modelo-de-clases.md`](../../05-diseno-software/ES/05.3-modelo-de-clases.md).

> **Nota:** al igual que el MER, el diagrama completo se presenta en página horizontal. El Anexo C lo desglosa por dominio para facilitar su lectura. La figura publicada puede conservar la nota de que las clases del patrón Strategy no existen en el código; hoy sí existen (`[POR CONFIRMAR]` al revisar la figura).
>
**Figura 4.** *Diagrama de clases del dominio, incluyendo los patrones Strategy, Observer y Factory Method.*

## 12.3 Módulos del backend por contexto

### 12.3.1 Módulo security

**Función:** Identidad, autenticación, sesiones, control de acceso, auditoría y borrado de cuenta.

**Estado:** Activo

**Archivos relacionados:**

- security/adapter/in/web/AuthController.java (controlador único de autenticación, perfil y cuenta)

- security/adapter/in/web/dto/ (DTOs: RegisterRequest, LoginRequest, GoogleLoginRequest, DeleteAccountRequest, ProfileResponse, ApiError…)

- security/application/usecase/ (RegisterUserService, AuthenticateUserService, SignInWithGoogleService, RefreshSessionService, LogoutUserSessionService, ChangePasswordService, CreatePasswordResetTokenService, ResetPasswordService, EmailVerificationService, UpdateUserProfileService, DeleteAccountService, CheckPermissionService, AssignRoleService, RevokeRoleService, ManageUserStatusService…)

- security/domain/model/ (User, Person, Email, PasswordHash, PasswordPolicy, UserSession, RefreshToken, SystemRole, Permission, AuditLog, LoginErrorLog)

- security/infrastructure/ (SecurityFilterChainConfiguration, JwtTokenIssuer, PemRsaKeys, SecurityCorsConfiguration, JwtAuthenticationEntryPoint)

- security/adapter/out/security/ (BCryptPasswordHasherAdapter, Sha256TokenHasher, PepperedHmacResetCodeHasher, InMemoryResetAttemptLimiter)

**Tablas relacionadas:** person, user, user_configuration, password_reset_token, user_session, refresh_token, security_configuration, password_policy, login_error_log, system_role, permission, user_system_role, system_role_permission, audit_log.

**Operaciones principales:**

- Registrar cuenta y verificar el correo

- Iniciar sesión (correo y contraseña o cuenta de Google) y renovar tokens

- Cerrar sesión

- Recuperar y cambiar contraseñas

- Consultar y actualizar el perfil

- Consultar permisos (RBAC)

- Eliminar la cuenta (transaccional: valida la contraseña, cierra las sesiones, elimina lógicamente los datos personales, libera el correo y publica `AccountDeleted`)

- Registrar auditoría e intentos fallidos

### 12.3.2 Módulo home

**Función:** Gestión de hogares, miembros, favoritos, umbrales de consumo, eliminación del hogar y transferencia de propiedad.

**Estado:** Activo

**Archivos relacionados:**

- home/adapter/in/web/HomeController.java, HomeThresholdsController.java, HomeTypeController.java

- home/adapter/in/web/CurrentUserResolver.java (resuelve el usuario desde el claim `sub` del JWT)

- home/application/usecase/ (CreateHomeService, JoinHomeService, LeaveHomeService, ListHomesService, ListMembersService, RemoveUserService, ToggleFavoriteService, DeleteHomeService, TransferHomeOwnershipService, HomeRemoval, RemoveDeletedAccountService, GetHomeThresholdsService, UpdateHomeThresholdsService, ListHomeTypesService, GetHomeTypeService)

- home/domain/model/ (Home, HomeType, HomeThresholds, UserHome, Role OWNER/MEMBER, OwnerPermissions, MemberPermissions)

- home/api/ (HomeApi, HomeDeleted, HomeOwnershipTransferred) y home/infrastructure/HomeApiImpl.java (API pública consumida por otros módulos)

**Tablas relacionadas:** home, home_type, home_thresholds, user_home.

**Operaciones principales:**

- Crear un hogar y generar código de acceso

- Unirse a un hogar con código

- Listar hogares y miembros

- Marcar favorito

- Expulsar miembros y salir del hogar (el último propietario no puede salir)

- Consultar, actualizar y restablecer umbrales diarios y mensuales

- Eliminar el hogar (solo OWNER, eliminación lógica de `home`, `user_home` y `home_thresholds`; publica `HomeDeleted`, que `device` atiende en la misma transacción para desvincular los dispositivos y revocar sus claves; tras confirmar, se envía el comando MQTT de desvinculación). El mismo comportamiento se aplica al borrar la cuenta del único OWNER.

- Transferir la propiedad (solo OWNER; intercambio atómico de roles con bloqueo de fila; publica `HomeOwnershipTransferred`, que ningún módulo consume todavía)

### 12.3.3 Módulo measurement

**Función:** Ingesta y análisis de mediciones de consumo eléctrico.

**Estado:** Activo

**Archivos relacionados:**

- measurement/adapter/in/web/MeasurementController.java, ConsumptionLevelController.java, HomeConsumptionController.java

- measurement/adapter/in/mqtt/ (TelemetryMqttConsumer, StatusMqttConsumer, MqttTopics, MqttConfig)

- measurement/application/usecase/ (RegisterMeasurementService, ListMeasurementsService, GetLatestMeasurementService, GetConsumptionStatisticsService, HomeConsumptionUseCase, HourlyEnergyCalculator, ListConsumptionLevelsService, TimeRange)

- measurement/domain/model/ (Measurement, ConsumptionLevel)

- measurement/adapter/out/event/MeasurementEventAdapter.java (publica MeasurementRecorded)

**Tablas relacionadas:** measurement, consumption_level (el estado de conexión se registra en device_status_log).

**Operaciones principales:**

- Registrar las lecturas que llegan por MQTT (voltaje, corriente, potencia activa, energía acumulada)

- Registrar los mensajes de estado de los dispositivos

- Consultar histórico por dispositivo y rango de fechas y la última medición

- Calcular estadísticas, el resumen y el historial de consumo del hogar

- Publicar el evento MeasurementRecorded

### 12.3.4 Módulo alert

**Función:** Generación y bandeja de alertas de consumo, de límite, de dispositivo vinculado y de conectividad.

**Estado:** Activo

**Archivos relacionados:**

- alert/adapter/in/web/AlertController.java (consulta, lectura y eliminación)

- alert/adapter/in/event/MeasurementRecordedListener.java y DeviceConnectivityLostListener.java (@EventListener)

- alert/application/usecase/ (ListAlertsService, GetAlertService, ResolveAlertService, DeleteAlertsService, RegisterThresholdAlertService, RegisterLimitAlertService, RegisterConnectivityAlertService, RegisterDeviceLinkedAlertService, ResolveConnectivityAlertsService, AlertAccessService)

- alert/infrastructure/AlertApiImpl.java y alert/api/AlertRaised.java

**Tablas relacionadas:** alert.

**Operaciones principales:**

- Generar alertas de consumo alto o crítico al recibir una medición

- Generar alertas de límite diario o mensual y de dispositivo vinculado (con la primera lectura tras vincular)

- Generar y resolver alertas de conectividad (150 segundos de silencio o mensaje de última voluntad)

- Listar alertas por hogar y estado, consultar una, marcarla como leída y eliminar las resueltas o leídas

### 12.3.5 Módulo notification

**Función:** Entrega de alertas y recomendaciones por correo y Web Push, preferencias y registro de entregas.

**Estado:** Activo

**Archivos relacionados:**

- notification/adapter/in/web/NotificationChannelsController.java

- notification/adapter/in/event/AlertRaisedListener.java y RecommendationCreatedListener.java

- notification/infrastructure/mail/EmailSender.java, MailDeliveryConfiguration.java, AbandonedDeliverySweeper.java y notification/adapter/out/push/WebPushSender.java

- notification/application/usecase/ (NotifyAlert, SendPasswordResetEmail, SendEmailVerification, ManageNotificationChannels, AlertMessages)

- notification/adapter/out/persistence/NotificationPersistenceAdapter.java

**Tablas relacionadas:** notification, notification_preference, push_subscription.

**Operaciones principales:**

- Enviar los correos de verificación y recuperación

- Entregar cada alerta y recomendación a los miembros del hogar por correo y Web Push según sus preferencias

- Registrar el estado de entrega (PENDING, SENT, FAILED) sin guardar el contenido del mensaje

- Barrer entregas abandonadas al iniciar la aplicación y cada cinco minutos

- Gestionar las preferencias y las suscripciones push

### 12.3.6 Módulo device

**Función:** Registro y vinculación de dispositivos, credenciales (API key), estado de conexión y autorización del broker MQTT.

**Estado:** Activo

**Archivos relacionados:**

- device/adapter/in/web/HomeDeviceController.java, ApplianceTypeController.java y BrokerAuthController.java

- device/application/usecase/ (HomeDeviceService, BrokerAccessService, DeviceConnectivityService)

- device/domain/model/ (Device, ApiKeyHash…)

- device/adapter/in/event/HomeDeletedListener.java y DeviceCommandListener.java; device/adapter/in/scheduler/DeviceInactivityScheduler.java

- device/adapter/out/ (MqttDeviceCommandAdapter, persistencia y adaptadores de credenciales)

**Tablas relacionadas:** device, appliance_type, device_status_log, device_home.

**Operaciones principales:**

- Vincular un dispositivo a un hogar (solo OWNER), editarlo, emitir nuevas credenciales para cambiar su Wi-Fi y desvincularlo

- Autenticar al dispositivo y autorizar sus tópicos para Mosquitto (solo el hash SHA-256 de la clave se almacena)

- Marcar el dispositivo sin conexión tras 150 segundos de silencio (`DeviceInactivityScheduler`)

- Enviar el comando MQTT de desvinculación al borrar el hogar o desvincular el dispositivo

### 12.3.7 Módulo recommendation

**Función:** Recomendaciones de ahorro generadas a partir del consumo medido.

**Estado:** Activo

**Archivos relacionados:**

- recommendation/adapter/in/web/RecommendationController.java

- recommendation/adapter/in/scheduler/RecommendationScheduler.java (cada hora)

- recommendation/application/usecase/ (EvaluateRecommendationsService, RecommendationInboxService)

- recommendation/adapter/out/event/RecommendationEventAdapter.java (publica RecommendationCreated)

**Tablas relacionadas:** recommendation.

**Operaciones principales:**

- Evaluar cada hora cinco reglas de ahorro (hora punta, consumo en reposo, comparación semanal, proyección de límite y aumento por dispositivo), sin repetir la misma recomendación en 7 días

- Listar las recomendaciones de un hogar, marcarlas como leídas y eliminarlas

## 12.4 Módulo front-end web

**Función:** interfaz de consulta y gestión del consumo eléctrico (SPA React)

**Archivos relacionados:**

- src/routes/AppRoutes.jsx (rutas), ProtectedRoute.jsx (rutas protegidas) y HomeRoute.jsx (detalle del hogar)

- src/services/http/httpClient.js (cliente axios, interceptores y renovación de token)

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

**URL base:** http://localhost:8080/api/v1 \| **Formato:** JSON \| **Documentación interactiva:** /swagger-ui.html y /v3/api-docs

El backend expone 54 rutas de negocio bajo `/api/v1`, dos rutas internas para el broker y las rutas de infraestructura. Todas las que no aparecen como públicas requieren un JWT válido en la cabecera Authorization; si el token falta, expiró o es inválido, la API responde 401, y si el usuario no tiene el permiso requerido, responde 403.

| **Método** | **Ruta**                                              | **Descripción**                                                       | **Parámetros**                                                         | **Autenticación** |
|------------|-------------------------------------------------------|-----------------------------------------------------------------------|------------------------------------------------------------------------|-------------------|
| POST       | /api/v1/auth/register                                 | Registra una cuenta nueva.                                            | body: email, password, name, lastName, profileImage (opcional)         | Pública           |
| POST       | /api/v1/auth/email/verify                             | Verifica el correo con un código de 6 dígitos.                        | body: email, code                                                      | Pública           |
| POST       | /api/v1/auth/email/verification/resend                | Reenvía el código de verificación (siempre responde 202).             | body: email                                                            | Pública           |
| POST       | /api/v1/auth/login                                    | Autentica y devuelve access token y refresh token.                    | body: email, password                                                  | Pública           |
| POST       | /api/v1/auth/google                                   | Inicia sesión o registra la cuenta con Google (503 si no está configurado). | body: code                                                       | Pública           |
| POST       | /api/v1/auth/refresh                                  | Renueva el access token y rota el refresh token.                      | body: refreshToken                                                     | Pública           |
| POST       | /api/v1/auth/logout                                   | Cierra la sesión registrada.                                          | body: idUserSession                                                    | JWT               |
| POST       | /api/v1/auth/password/forgot                          | Solicita el código de recuperación de contraseña.                     | body: email                                                            | Pública           |
| POST       | /api/v1/auth/password/reset                           | Restablece la contraseña con el código recibido.                      | body: email, resetToken, newPassword                                   | Pública           |
| POST       | /api/v1/auth/password/change                          | Cambia la contraseña conociendo la actual.                            | body: currentPassword, newPassword                                     | JWT               |
| PUT        | /api/v1/auth/profile                                  | Actualiza nombre, apellido, imagen o correo.                          | body: name, lastName, profileImage, newEmail                           | JWT               |
| POST       | /api/v1/auth/account/delete                           | Elimina la cuenta tras confirmar la contraseña.                       | body: password                                                         | JWT               |
| GET        | /api/v1/auth/account                                  | Devuelve la cuenta autenticada.                                       | ninguno (identidad del JWT)                                            | JWT               |
| GET        | /api/v1/auth/permissions                              | Lista los códigos de permiso del usuario.                             | ninguno                                                                | JWT               |
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
| POST       | /internal/mqtt/auth/user                              | Interna: Mosquitto autentica a un cliente MQTT (caché de cinco minutos). El proxy debe bloquearla. | body: username, password, clientid          | Red interna       |
| POST       | /internal/mqtt/auth/acl                               | Interna: Mosquitto autoriza el tópico de un cliente. El proxy debe bloquearla. | body: username, clientid, topic, acc                          | Red interna       |
| GET        | /actuator/health                                      | Estado del servicio (UP / DOWN).                                      | ninguno                                                                | Pública           |
| GET        | /swagger-ui.html y /v3/api-docs                       | Documentación interactiva de la API (OpenAPI).                        | ninguno                                                                | Pública           |
| OPTIONS    | /\*\*                                                 | Preflight de CORS.                                                    | cabeceras Origin y Access-Control-Request-\*                           | Pública           |

> *Las rutas `/internal/**` solo aceptan llamadas desde las redes de `MQTT_AUTH_ALLOWED_NETWORKS`. Detrás de un proxy inverso toda petición parece local, por lo que el proxy no debe reenviarlas.*

## 13.1 Códigos de respuesta habituales

| **Código**      | **Cuándo se produce**                                                         | **Estructura de la respuesta**                                 |
|-----------------|-------------------------------------------------------------------------------|----------------------------------------------------------------|
| 200 / 201 / 204 | Operación exitosa (consulta, creación o eliminación sin contenido).           | Cuerpo con el recurso o mensaje correspondiente.               |
| 400             | Validación de datos incorrecta (Bean Validation o esquema zod en el cliente). | ApiError con fieldErrors detalle de cada campo.                |
| 401             | Token ausente, expirado o credenciales inválidas.                             | ApiError con mensaje genérico (no revela si el correo existe). |
| 403             | Cuenta bloqueada o inactiva, o permiso insuficiente (p. ej. un MEMBER intenta eliminar el hogar). | ApiError.                                 |
| 404             | Recurso inexistente (hogar, alerta, miembro…) o hogar al que no se pertenece. | ApiError o {"error": "..."} según el módulo.                   |
| 409             | Conflicto (correo ya registrado, último propietario que no puede salir, transferencia al propietario actual). | ApiError.                      |
| 422             | La contraseña no cumple la política de seguridad.                             | ApiError.                                                      |
| 429             | Límite de intentos alcanzado en verificación o recuperación.                  | ApiError con cabecera Retry-After (900 segundos).              |
| 503             | Inicio de sesión con Google sin credenciales configuradas.                    | ApiError.                                                      |
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

# 14. Seguridad

## 14.1 Controles implementados

| **Control**                   | **Implementación técnica**                                                                                                                                                                              |
|-------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Autenticación                 | JWT firmado con algoritmo RS256 y par de claves RSA en archivos PEM; vida útil del access token: 15 minutos; sin roles dentro del JWT; claims sub (idUser), person_id y email.                           |
| Identidad                     | Todos los módulos obtienen el usuario del claim `sub` mediante `CurrentUserResolver`; la cabecera X-User-Id se ignora.                                                                                  |
| Sesiones                      | Estado sin estado (STATELESS): sin cookies ni sesión HTTP; CSRF deshabilitado por no existir credencial ambiental.                                                                                      |
| Renovación de sesión          | Refresh token opaco con estado, rotado en cada uso, almacenado como hash SHA-256 con family_id y parent_id para detectar reutilización (replay) y revocado al cambiar la contraseña.                     |
| Cifrado de contraseñas        | BCrypt (BCryptPasswordHasherAdapter) aislado del dominio; nunca se almacenan contraseñas en claro.                                                                                                      |
| Política de contraseñas       | Tabla password_policy: longitud mínima 8 y máxima 64, exige mayúsculas, números y símbolos, expira a los 90 días. Incumplimiento =\> 422.                                                               |
| Autorización global (RBAC)    | Cadena de cinco tablas (system_role, permission, user_system_role, system_role_permission y user) consultada en vivo por `CheckPermission`; permisos consultables en GET /api/v1/auth/permissions.          |
| Autorización por hogar        | Roles OWNER y MEMBER por hogar resueltos con el patrón Strategy (`OwnerPermissions`, `MemberPermissions`).                                                                                              |
| Validación de entrada         | Bean Validation en los DTOs del backend (@NotBlank, @Size, @NotNull, @Positive, @Pattern) y esquemas zod en los formularios de los front-ends.                                                          |
| CORS                          | Cerrado por defecto: lista explícita de orígenes (SECURITY_CORS_ALLOWED_ORIGINS), sin comodines, sin credenciales y con manejo del preflight OPTIONS; métodos GET, POST, PUT, DELETE y OPTIONS.         |
| Límite de intentos            | Limitador en memoria: 10 intentos por IP cada 15 minutos en la recuperación de contraseña =\> 429 con cabecera Retry-After.                                                                             |
| Protección contra enumeración | Login fallido con mensaje genérico y password/forgot con respuesta 202 idéntica exista o no la cuenta; el trabajo se realiza de forma asíncrona.                                                        |
| Códigos de recuperación       | 6 dígitos, validez de 15 minutos, uso único, guardados como HMAC-SHA256 con pepper (SECURITY_RESET_PEPPER).                                                                                             |
| Credenciales de dispositivo   | Cada dispositivo usa `device_code` y `api_key`; el backend solo guarda el hash SHA-256 de la clave (`api_key_hash`). Mosquitto autentica al dispositivo contra el backend y la ACL limita sus tópicos.  |
| Transporte de dispositivos    | MQTT 3.1.1 con TLS en el puerto 8883; el puerto 1883 es interno y las rutas `/internal/**` solo se aceptan desde redes permitidas (el proxy debe bloquearlas).                                          |
| Auditoría                     | Tablas audit_log (acciones, IP, usuario) e login_error_log (intentos fallidos); la IP se toma de getRemoteAddr() ignorando X-Forwarded-For.                                                             |
| Documentación                 | El endpoint de OpenAPI declara el esquema bearerAuth; los endpoints públicos son sólo los de autenticación y los de infraestructura.                                                                    |
| Transporte                    | En producción se debe servir por HTTPS; en desarrollo local se usa HTTP con Mailpit como servidor de correo de pruebas.                                                                                 |

## 14.2 Limitaciones y riesgos conocidos

> Los siguientes puntos están declarados por el propio equipo y deben tenerse en cuenta antes de ampliar el sistema.

| **Limitación**                                                                                 | **Impacto**                                                                           | **Recomendación**                                                                   |
|------------------------------------------------------------------------------------------------|---------------------------------------------------------------------------------------|-------------------------------------------------------------------------------------|
| Telemetría y estado con MQTT QoS 0.                                                            | Una lectura puede perderse (sin duplicados).                                          | Evaluar un cliente MQTT con QoS 1 (por ejemplo espMqttClient).                      |
| Access token y refresh token almacenados en localStorage (web).                                | Una vulnerabilidad XSS podría leer los tokens (riesgo aceptado y documentado).        | Migrar a cookies httpOnly con SameSite o reforzar la política de Content Security Policy. |
| Limitador de intentos en memoria.                                                              | Se reinicia con cada arranque y no es compartido entre instancias.                    | Usar Redis u otro almacén compartido, o un limitador en el balanceador.             |
| Sin actualización por aire (OTA) del firmware.                                                 | Actualizar el firmware exige un cable USB.                                            | Evaluar OTA en una versión futura.                                                  |
| Las pruebas de persistencia no son herméticas.                                                 | Necesitan MySQL en ejecución y SPRING_DATASOURCE_PASSWORD.                            | Usar un contenedor de base de datos para pruebas.                                   |
| El access token sigue siendo válido hasta 15 minutos tras borrar la cuenta.                    | Ventana breve de acceso residual.                                                     | Aceptado; documentado.                                                              |
| Casos límite de las alertas de conectividad (CONNECTIVITY) sin resolución automática.          | Algunas alertas requieren revisión manual.                                            | Definir la regla de resolución automática.                                          |
| Web Bluetooth limitado a Chrome, Edge y Chrome para Android, y a HTTPS o localhost.            | En otros navegadores se usa el portal de configuración Wi-Fi; en iOS no hay push sin instalar la web. | Mantener el portal `EnergyMonitor-Setup` como alternativa.            |
| `HomeOwnershipTransferred` no tiene consumidor.                                                | El nuevo responsable no recibe un aviso al recibir la propiedad.                      | Consumir el evento y enviar correo o push.                                          |
| El historial del dispositivo no se limita al periodo de vinculación.                           | Un hogar puede ver mediciones de un periodo anterior de otro hogar.                   | Limitar el historial al periodo de vinculación (RF-HOM-11).                         |
| Borrado y transferencia de hogar ausentes en la aplicación móvil; sin pruebas automatizadas.   | Solo se pueden hacer desde la web.                                                    | Implementarlos en la aplicación Android y agregar pruebas.                          |

## 14.3 Buenas prácticas para los desarrolladores

- No commitear nunca los archivos .env, application-local.yaml ni las claves RSA (\*.pem); ambos ya están declarados en .gitignore.

- No registrar contraseñas, tokens ni códigos de recuperación en los logs de la aplicación.

- Mantener actualizadas las dependencias del backend y del front y revisar sus avisos de seguridad.

- Toda funcionalidad nueva debe validar la entrada en el backend, independientemente de las validaciones del cliente.

- Respetar los límites de módulo: no importar clases de otro contexto sin pasar por su paquete api (lo verifica ModularityTests).

- Rotar las claves JWT y el pepper de recuperación si se sospecha de una filtración.

# 15. Control de versiones del software

El proyecto utiliza Git con repositorio remoto en GitHub. Cada componente del sistema se desarrolla en un repositorio independiente para facilitar su despliegue y su revisión.

| **Repositorio**            | **Contenido**                                                    | **Observaciones**                                                                         |
|----------------------------|------------------------------------------------------------------|-------------------------------------------------------------------------------------------|
| energy-monitor-backend     | API REST en Java 21 / Spring Boot.                               | Incluye pruebas, docker-compose.yml, Dockerfile, .env.example y la historia de migraciones Liquibase. |
| Energy-Monitor-application | Aplicación web React 19 / Vite 8.                                | Incluye pruebas Vitest (14 archivos) y build de producción en /dist.                      |
| Energy-Monitor-Mobile      | Aplicación móvil Expo SDK 57.                                    | Incluye LICENSE, AGENTS.md (documentación versionada de Expo 57) y scripts de Expo.       |
| energy-monitor-devices     | Firmware del ESP32 y broker Mosquitto.                           | Incluye docker-compose.yml de desarrollo y de producción y el contrato MQTT.              |
| energy-monitor-docs-final  | Documentación final bilingüe (ES y EN).                          | Documento general, especificación de requisitos, propuesta técnica, diseño y manuales.    |

- Las ramas siguen la convención `[tipo]/[nombre-en-kebab-case]` (feat/*, fix/*, chore/*) y los commits siguen Conventional Commits con el mensaje en inglés.

- La integración de código se hace mediante pull request a `dev` y luego a `main`.

- Antes de fusionar cambios se debe ejecutar la suite de pruebas del backend (mvnw test) y las pruebas del front web (npm test).

- Los cambios en el esquema de base de datos se realizan únicamente mediante un nuevo changeset de Liquibase con rollback; nunca se editan changesets ya aplicados.

- Las ramas de integración deben mantener el código en estado compilable y las pruebas en verde.

- Las versiones entregadas se etiquetan para conservar la referencia exacta del software documentado en este manual.

# 16. Recomendaciones de mantenimiento

## 16.1 Actualizaciones del código

| **Área**      | **Recomendación**                                                                                                                                                                 |
|---------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Backend       | Compilar con .\mvnw.cmd clean test antes de cada entrega; verificar que ModularityTests confirme que no se rompieron los límites de módulo.                                       |
| Base de datos | Crear un changeset nuevo por cada modificación de esquema, con identificador secuencial (p. ej. security-025) y su rollback; ejecutar docker compose up -d para probar contra MySQL 8.4 real. |
| Dependencias  | Actualizar Spring Boot, Expo o React en ramas de prueba, ejecutando la suite completa antes de fusionar.                                                                          |
| Front web     | Mantener sincronizadas la variable VITE_API_BASE_URL y la lista de orígenes permitidos (SECURITY_CORS_ALLOWED_ORIGINS); volver a ejecutar npm test y npm run lint.                |
| Front móvil   | Revisar la documentación versionada de Expo 57 antes de incorporar librerías nativas; no hay pruebas automatizadas, por lo que la verificación es manual.                         |
| Dispositivos  | Recompilar y cargar el firmware por USB tras cambiar config.h o el certificado; renovar el certificado del broker y reiniciar el contenedor `mosquitto`.                          |
| Seguridad     | Revisar trimestralmente los permisos RBAC, la política de contraseñas y la vigencia de las claves JWT.                                                                            |

## 16.2 Copias de seguridad de la base de datos

Antes de cada actualización relevante se debe respaldar el esquema energy_monitor. Procedimiento recomendado:

1.  Detener la escritura de información (no ejecutar cargas de mediciones durante el respaldo).

2.  Ejecutar el volcado: mysqldump -h localhost -u energy_app -p energy_monitor \> backup_energy_monitor_AAAA-MM-DD.sql

3.  Guardar el archivo en una carpeta segura y fuera del repositorio, con la fecha en el nombre.

4.  Verificar el respaldo restaurándolo en una base de datos de prueba antes de darlo por válido.

5.  Restauración: mysql -h localhost -u energy_app -p energy_monitor \< backup_energy_monitor_AAAA-MM-DD.sql

## 16.3 Soporte y resolución de incidentes

| **Síntoma**                             | **Verificación sugerida**                                                                                                            |
|-----------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------|
| El backend no inicia                    | Revisar el puerto 8080 (netstat), el contenedor MySQL (docker compose ps) y la variable SPRING_DATASOURCE_PASSWORD.                  |
| La API responde 403/401 de forma masiva | Verificar el par de claves RSA (SECURITY_JWT_PRIVATE_KEY_PATH / PUBLIC) y la fecha y hora del servidor.                              |
| El front web no conecta con la API      | Confirmar que el backend responde en el puerto indicado y que SECURITY_CORS_ALLOWED_ORIGINS incluye el origen http://localhost:5173. |
| Los correos no llegan                   | Revisar MAIL_HOST, MAIL_PORT, MAIL_USERNAME y MAIL_PASSWORD; en local usar Mailpit (http://localhost:8025).                          |
| Un dispositivo no envía lecturas        | Revisar el broker (docker compose logs mosquitto), DEVICE_MQTT_PUBLIC_HOST (nunca localhost), el certificado TLS y la salida serie del ESP32. |
| Las migraciones fallan al arrancar      | Revisar el log de Liquibase y el orden de los changesets en db.changelog-master.xml.                                                 |
| Los mensajes de error no son legibles   | Revisar la consola del navegador y el cuerpo ApiError devuelto por la API.                                                           |

## 16.4 Trabajos pendientes declarados

- Limitar el historial del dispositivo al periodo de vinculación (RF-HOM-11).

- Consumir `HomeOwnershipTransferred` para avisar al nuevo responsable del hogar.

- Implementar el borrado y la transferencia de hogar en la aplicación móvil y completar sus pruebas automatizadas.

- Evaluar MQTT con QoS 1 para no perder lecturas.

- Resolver automáticamente los casos límite de las alertas de conectividad.

- Exponer por HTTP las operaciones administrativas de roles y estados de usuario que ya existen como puertos de aplicación.

- Evaluar la actualización del firmware por aire (OTA).

- Decidir qué hacer con el repositorio legado `en-monitor-security-service` (archivarlo o eliminarlo): no se utiliza.

# 17. Glosario

| **Término**           | **Definición**                                                                                                                        |
|-----------------------|---------------------------------------------------------------------------------------------------------------------------------------|
| ADSO                  | Análisis y Desarrollo de Software: tecnología SENA que forma parte del proyecto (ficha 3145556).                                      |
| API REST              | Interfaz de programación basada en HTTP que expone recursos en formato JSON bajo rutas versionadas (/api/v1).                         |
| Access token          | Token JWT de corta duración (15 minutos) que prueba la identidad del usuario en cada petición.                                        |
| Bounded context       | Contexto acotado de dominio (security, home, measurement…) con su propio esquema y API interna.                                       |
| BCrypt                | Algoritmo de hash con sal utilizado para guardar contraseñas de forma irreversible.                                                   |
| Broker MQTT           | Servidor (Eclipse Mosquitto) que recibe los mensajes publicados por los dispositivos y los entrega a los suscriptores.                |
| CORS                  | Política HTTP que controla desde qué orígenes (p. ej. http://localhost:5173) el navegador puede llamar a la API.                      |
| Changeset             | Unidad de migración de Liquibase: instrucción versionada para crear o modificar tablas.                                               |
| CurrentUserResolver   | Componente de cada módulo que obtiene la identidad del usuario del claim `sub` del JWT.                                               |
| Docker Compose        | Herramienta que define y levanta contenedores (por ejemplo mysql:8.4) con un solo comando.                                            |
| DTO                   | Objeto de transferencia de datos: estructura JSON de entrada o salida de un endpoint.                                                 |
| ER / MER              | Diagrama entidad-relación: modelo lógico de las tablas y sus relaciones.                                                              |
| JWT                   | JSON Web Token: token firmado (algoritmo RS256) que transporta la identidad del usuario.                                              |
| Liquibase             | Gestor de esquema: aplica changesets de base de datos de forma controlada y versionada.                                               |
| Módulo (Modulith)     | Unidad de código con límites verificados por Spring Modulith; sólo se relaciona por su paquete api.                                   |
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
| X-User-Id             | Cabecera que el backend ignora: la identidad se obtiene siempre del claim `sub` del JWT.                                              |

# 18. Anexos

## Anexo A. Configuración y comandos

**Variables de entorno.** El backend lee sus secretos desde un archivo .env (importado por spring.config.import) cuya estructura se documenta en .env.example y que se lee como `.properties` (valores sin comillas y sin `$`); el front web usa .env.local con VITE_API_BASE_URL y VITE_GOOGLE_CLIENT_ID, y la app móvil usa .env con EXPO_PUBLIC_API_BASE_URL. Por razones de seguridad no se publican valores reales:

| **Variable**                                                 | **Descripción**                                                                                      |
|--------------------------------------------------------------|------------------------------------------------------------------------------------------------------|
| MYSQL_PASSWORD / MYSQL_ROOT_PASSWORD                         | Contraseñas del usuario de aplicación y del usuario root de MySQL (definidas en docker-compose.yml). |
| SPRING_DATASOURCE_PASSWORD                                   | Contraseña usada por las pruebas con el perfil test.                                                 |
| MAIL_HOST / MAIL_PORT                                        | Servidor SMTP y puerto (por defecto smtp.gmail.com:587).                                             |
| MAIL_USERNAME / MAIL_PASSWORD / MAIL_FROM                    | Cuenta remitente y contraseña de aplicación para enviar correos.                                     |
| SECURITY_RESET_PEPPER                                        | Pepper del HMAC-SHA256 de los códigos de recuperación de contraseña.                                 |
| SECURITY_CORS_ALLOWED_ORIGINS                                | Orígenes permitidos por CORS (por defecto http://localhost:5173 en el ejemplo).                      |
| SECURITY_JWT_PRIVATE_KEY_PATH / SECURITY_JWT_PUBLIC_KEY_PATH | Rutas absolutas al par de claves RSA (private.pem y public.pem) usado por RS256.                     |
| GOOGLE_CLIENT_ID / GOOGLE_CLIENT_SECRET                      | Credenciales OAuth para iniciar sesión con Google; vacías, el botón responde 503.                    |
| MQTT_BROKER_URL / MQTT_BACKEND_USER / MQTT_BACKEND_PASS / MQTT_CLIENT_ID | Conexión del backend al broker MQTT (listener interno 1883).                             |
| MQTT_AUTH_ALLOWED_NETWORKS                                   | Redes autorizadas a llamar a las rutas internas de autenticación del broker.                         |
| DEVICE_MQTT_PUBLIC_HOST / DEVICE_MQTT_PUBLIC_PORT            | Dirección del broker que se entrega al dispositivo al vincularlo (puerto 8883).                      |
| DEVICE_OFFLINE_AFTER                                         | Silencio tras el cual un dispositivo se marca sin conexión (PT150S).                                 |
| NOTIFICATION_WEB_URL / NOTIFICATION_VAPID_PUBLIC_KEY / NOTIFICATION_VAPID_PRIVATE_KEY / NOTIFICATION_VAPID_SUBJECT | Enlaces de los correos y claves VAPID del push del navegador. |
| ALERT_LIMIT_ZONE / RECOMMENDATION_EVALUATE_INTERVAL / RECOMMENDATION_ZONE | Zona horaria de las alertas de límite y frecuencia y zona de las recomendaciones.       |
| VITE_API_BASE_URL / VITE_GOOGLE_CLIENT_ID (front web)        | URL base del backend (por defecto http://localhost:8080) e ID de cliente de Google.                  |
| EXPO_PUBLIC_API_BASE_URL (app móvil)                         | URL base del backend: 10.0.2.2 en el emulador de Android o la IP del PC en un teléfono.              |

**Comandos de operación:**

| **Comando**                             | **Descripción**                                                                    |
|-----------------------------------------|------------------------------------------------------------------------------------|
| docker compose up -d                    | Levanta MySQL 8.4 con el esquema energy_monitor.                                   |
| docker compose down -v                  | Detiene la base de datos y elimina el volumen (regenera el esquema con Liquibase). |
| docker run -p 8025:8025 axllent/mailpit | Levanta el servidor de correo de pruebas (interfaz en http://localhost:8025).      |
| .\mvnw.cmd spring-boot:run              | Arranca la API REST en http://localhost:8080.                                      |
| .\mvnw.cmd test                         | Ejecuta la suite de pruebas del backend.                                           |
| .\mvnw.cmd package                      | Genera el JAR ejecutable en la carpeta /target.                                    |
| npm install / npm run dev               | Instala dependencias y arranca la aplicación web (http://localhost:5173).          |
| npm test / npm run lint                 | Ejecuta las pruebas y el análisis estático del front web.                          |
| npm run start / npm run android         | Arranca la aplicación móvil en Expo (dispositivo o emulador Android).              |
| docker compose up -d (devices/docker)   | Levanta el broker Mosquitto de desarrollo.                                         |
| arduino-cli compile / upload            | Compila y carga el firmware del dispositivo por USB.                               |

**Verificación tras la instalación:** GET /actuator/health responde {"status":"UP"}, la pantalla de inicio de sesión se carga en el navegador, el registro de una cuenta envía el código de verificación, los hogares creados aparecen en el panel de la aplicación y la primera lectura de un dispositivo vinculado aparece en el hogar.

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
