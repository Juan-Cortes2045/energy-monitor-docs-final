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
| Integrantes          | Juan Esteban Cortes Parra Miguel Angel Garcia Artunduaga                        |
| Instructor           | Karol Daniela Correa                                                            |
| Centro de Formación  | Centro de la Industria, la Empresa y los Servicios (CIES) – SENA Regional Huila |
| Programa             | Tecnología en Análisis y Desarrollo de Software (ADSO)                          |
| Ficha                | 3145556                                                                         |
| Tipo de documento    | Manual técnico de software                                                      |
| Versión              | 1.0                                                                             |
| Fecha de elaboración | 07 de octubre de 2026                                                           |

Año 2026

# 2. Control de versiones del documento

El historial de cambios del presente manual se registra en la siguiente tabla. Cada modificación relevante del software debe verse reflejada en una nueva versión del documento.

| **Versión** | **Fecha**  | **Descripción del cambio**                                                                                                                                                               | **Responsable**      |
|-------------|------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------|
| 1.0         | 07/10/2026 | Creación inicial del manual técnico: descripción general, tecnologías, arquitectura, estructura de carpetas, base de datos, módulos, API y seguridad de la versión vigente del software. | Equipo desarrollador |
| 1.0         | 07/10/2026 | Incorporación de los diagramas oficialmente aprobados: MER, diagrama de clases, arquitectura y secuencia de autenticación, además del diccionario de datos.                              | Equipo desarrollador |

# 3. Tabla de contenido

[2. Control de versiones del documento 1](#control-de-versiones-del-documento)

[3. Tabla de contenido 2](#tabla-de-contenido)

[4. Introducción 4](#introducción)

[5. Objetivo 4](#objetivo)

[6. Alcance técnico 5](#alcance-técnico)

[7. Descripción general del sistema 5](#descripción-general-del-sistema)

> [7.1 ¿Qué es el sistema? 5](#qué-es-el-sistema)
>
> [7.2 Componentes principales 5](#componentes-principales)
>
> [7.3 Roles del sistema 6](#roles-del-sistema)
>
> [7.4 Estado de implementación por dominio 6](#estado-de-implementación-por-dominio)

[8. Tecnologías utilizadas 7](#tecnologías-utilizadas)

> [8.1 Dependencias principales del backend (pom.xml) 7](#dependencias-principales-del-backend-pom.xml)
>
> [8.2 Scripts disponibles 8](#scripts-disponibles)

[9. Arquitectura del sistema 8](#arquitectura-del-sistema)

> [9.1 Arquitectura general 8](#arquitectura-general)
>
> [9.2 Estilo arquitectónico y patrones de diseño 8](#estilo-arquitectónico-y-patrones-de-diseño)
>
> [9.3 Comunicación entre componentes 9](#comunicación-entre-componentes)

[10. Estructura del proyecto 10](#estructura-del-proyecto)

> [10.1 Estructura del backend 10](#estructura-del-backend)
>
> [10.2 Estructura de la aplicación web 11](#estructura-de-la-aplicación-web)
>
> [10.3 Estructura de la aplicación móvil 11](#estructura-de-la-aplicación-móvil)
>
> [10.4 Función de las carpetas principales 11](#función-de-las-carpetas-principales)

[11. Base de datos 13](#base-de-datos)

> [11.1 Diagrama entidad-relación 13](#diagrama-entidad-relación)
>
> [11.2 Tablas principales 15](#tablas-principales)
>
> [11.3 Diccionario de datos 16](#diccionario-de-datos)

[12. Módulos técnicos 24](#módulos-técnicos)

> [12.1 Vista general de los módulos 24](#vista-general-de-los-módulos)
>
> [12.2 Diagrama de clases del dominio 24](#diagrama-de-clases-del-dominio)
>
> [12.3 Módulos del backend por contexto 26](#módulos-del-backend-por-contexto)
>
> [12.4 Módulo front-end web 29](#módulo-front-end-web)
>
> [12.5 Módulo front-end móvil 30](#módulo-front-end-móvil)

[13. API o rutas del sistema 31](#api-o-rutas-del-sistema)

> [13.1 Códigos de respuesta habituales 32](#códigos-de-respuesta-habituales)
>
> [13.2 Ejemplo de petición y respuesta 32](#ejemplo-de-petición-y-respuesta)

[14. Seguridad 33](#seguridad)

> [14.1 Controles implementados 33](#controles-implementados)
>
> [14.2 Limitaciones y riesgos conocidos 33](#limitaciones-y-riesgos-conocidos)
>
> [14.3 Buenas prácticas para los desarrolladores 34](#buenas-prácticas-para-los-desarrolladores)

[15. Control de versiones del software 35](#control-de-versiones-del-software)

[16. Recomendaciones de mantenimiento 35](#recomendaciones-de-mantenimiento)

> [16.1 Actualizaciones del código 35](#actualizaciones-del-código)
>
> [16.2 Copias de seguridad de la base de datos 36](#copias-de-seguridad-de-la-base-de-datos)
>
> [16.3 Soporte y resolución de incidentes 36](#soporte-y-resolución-de-incidentes)
>
> [16.4 Trabajos pendientes declarados 36](#trabajos-pendientes-declarados)

[17. Glosario 37](#glosario)

[18. Anexos 37](#anexos)

> [Anexo A. Configuración y comandos 37](#anexo-a.-configuración-y-comandos)

[Anexo B. MER por contexto 39](#anexo-b.-mer-por-contexto)

[Anexo C. Diagrama de clases por dominio 40](#anexo-c.-diagrama-de-clases-por-dominio)

*Nota: al abrir el documento, Word ofrece actualizar los campos de la tabla de contenido. Si es necesario, seleccione el listado, haga clic derecho y elija «Actualizar campo».*

# 4. Introducción

El presente documento es el manual técnico del software «Monitor de Consumo Eléctrico», proyecto desarrollado por los aprendices del programa de formación en Análisis y Desarrollo de Software, ficha 3145556, del Centro de la Industria, la Empresa y los Servicios (CIES) – SENA Regional Huila.

El sistema permite monitorear el consumo de energía eléctrica en los hogares: registra mediciones de los dispositivos, calcula estadísticas de consumo, genera alertas cuando se superan los umbrales configurados y ofrece una interfaz web y móvil para la consulta de la información. La solución se compone de una API REST, una aplicación web, una aplicación móvil y una base de datos MySQL.

Este manual está dirigido a desarrolladores, personal de soporte técnico, el instructor evaluador y en general a cualquier persona que deba comprender, ampliar o dar mantenimiento al software. Su propósito es describir cómo está construido el sistema, no cómo se utiliza: para el uso cotidiano existe el manual de usuario, para la puesta en marcha el manual de instalación y para la gestión de cuentas y respaldos el manual de administrador.

La información aquí consignada fue obtenida directamente del código fuente de los tres repositorios del proyecto y de los documentos de diseño (SRS, informe técnico, MER, diccionario de datos y diagrama de clases), de modo que corresponda a la versión final del software entregado.

| **Fuente de información**            | **Ubicación**                                                                                                        |
|--------------------------------------|----------------------------------------------------------------------------------------------------------------------|
| Código fuente del backend            | energymonitor-backend (Java 21 + Spring Boot 4.1.1)                                                                  |
| Código fuente de la aplicación web   | Energy-Monitor-application (React 19 + Vite 8)                                                                       |
| Código fuente de la aplicación móvil | Energy-Monitor-Mobile (Expo SDK 57 + React Native 0.86)                                                              |
| Documentación del proyecto           | Documentacion-proyecto-Monitor-de-consumo-electrico (SRS, informe técnico, diagramas, mockups, diccionario de datos) |
| Especificación de requisitos         | SRS.docx e INFORME TECNICO (RF1 a RF5 y RNF1 a RNF7)                                                                 |
| Modelo de datos                      | Base de datos/MER.wsd y Base de datos/Diccionario de Datos.xlsx                                                      |
| Modelo de clases                     | Diagrama de clases/Diagrama_clases.wsd                                                                               |

# 5. Objetivo

> Documentar la arquitectura, las tecnologías, la estructura del proyecto, la base de datos, los módulos técnicos, la API, la seguridad y las recomendaciones de mantenimiento del sistema «Monitor de Consumo Eléctrico», de manera que un desarrollador ajeno al equipo pueda comprender el software, continuar su desarrollo y darle soporte sin necesidad de conocimiento previo del proyecto.

- Describir la arquitectura de tres capas y los patrones de diseño empleados en cada componente.

- Detallar las tecnologías y versiones exactas utilizadas en el backend y en los dos front-ends.

- Explicar la organización del código fuente y la función de cada carpeta.

- Documentar el modelo de datos: diagrama entidad-relación, tablas principales y diccionario de datos.

- Relacionar cada módulo técnico con sus archivos, tablas y endpoints.

- Describir los servicios expuestos por la API REST y los mecanismos de seguridad aplicados.

- Establecer recomendaciones para la actualización, respaldo y soporte del sistema.

# 6. Alcance técnico

El manual técnico cubre los siguientes aspectos del sistema:

| **Sí cubre**                                                                            | **No cubre**                                                                                                         |
|-----------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------|
| Arquitectura general, capas y patrones de diseño del backend y de los front-ends.       | Procedimientos de instalación y puesta en marcha (corresponde al manual de instalación).                             |
| Tecnologías, librerías y versiones utilizadas, así como sus dependencias.               | Pasos de uso, pantallas y mensajes para el usuario final (corresponde al manual de usuario).                         |
| Estructura de carpetas y archivos de los tres repositorios del proyecto.                | Gestión de usuarios, roles, reportes administrativos y copias de seguridad (corresponde al manual de administrador). |
| Base de datos: MER, tablas, relaciones y diccionario de datos.                          | Diseño eléctrico y firmware del dispositivo de medición (hardware fuera del alcance del software).                   |
| Módulos técnicos del backend, front web y front móvil, con sus archivos y tablas.       | Historia de usuario, casos de uso y criterios de aceptación (documentos de análisis).                                |
| API REST: método, ruta, parámetros, autenticación y respuestas.                         | Pruebas de carga y despliegue en un ambiente productivo.                                                             |
| Seguridad: autenticación, autorización, cifrado, validaciones y limitaciones conocidas. | Costos, plan de proyectos e informes de gestión.                                                                     |
| Control de versiones y recomendaciones de mantenimiento.                                |                                                                                                                      |

**Versión documentada:** software disponible en los repositorios al 07 de octubre de 2026. Las funcionalidades marcadas como «pendiente» o «esqueleto» se describen explícitamente en la sección 12.

# 7. Descripción general del sistema

## 7.1 ¿Qué es el sistema?

«Monitor de Consumo Eléctrico» es una solución web y móvil que permite a los usuarios conocer en tiempo real cuánta energía consumen sus hogares, identificar los dispositivos que más energía utilizan, recibir alertas cuando el consumo supera los límites establecidos y obtener recomendaciones para reducir el gasto y la factura eléctrica.

## 7.2 Componentes principales

| **Componente**     | **Tipo**                         | **Función**                                                                                                                        |
|--------------------|----------------------------------|------------------------------------------------------------------------------------------------------------------------------------|
| API REST           | Servidor (Java 21 + Spring Boot) | Expone los servicios de autenticación, hogares, mediciones, umbrales y alertas. Es la única fuente de verdad de los datos.         |
| Aplicación web     | SPA (React 19 + Vite 8)          | Interfaz de consulta y gestión: acceso, panel de hogares, consumo, historial, usuarios, umbrales, alertas y configuración.         |
| Aplicación móvil   | App híbrida (Expo SDK 57)        | Interfaz móvil con el mismo modelo de dominio que la web; hoy opera con datos de demostración pendiente de integración con la API. |
| Base de datos      | MySQL 8.4                        | Almacena usuarios, hogares, dispositivos, mediciones, alertas, recomendaciones y notificaciones (27 tablas).                       |
| Correo electrónico | SMTP (Mailpit en local)          | Envía los códigos de verificación de correo y de recuperación de contraseña.                                                       |

## 7.3 Roles del sistema

| **Rol**                      | **Descripción**                                                                                  | **Cómo se implementa**                                                                                                               |
|------------------------------|--------------------------------------------------------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------|
| Propietario de hogar (OWNER) | Crea y administra un hogar: invita miembros, configura umbrales y gestiona dispositivos.         | Enum OWNER de la tabla user_home, con permisos de dominio en el módulo home.                                                         |
| Miembro de hogar (MEMBER)    | Consulta el consumo, el historial, las alertas y la configuración del hogar al que fue invitado. | Enum MEMBER de la tabla user_home.                                                                                                   |
| Usuario del sistema          | Cuenta personal con perfil, preferencias de idioma, tema y notificaciones.                       | Tabla user con estado ACTIVE / INACTIVE / BLOCKED.                                                                                   |
| Control de acceso (RBAC)     | Permisos funcionales agrupados en roles del sistema (códigos como METER.READ, ALERT.READ).       | Tablas system_role, permission, user_system_role y system_role_permission.                                                           |
| Administrador                | Gestión global del sistema y de cuentas, según el SRS.                                           | Parcial: los puertos de asignación de roles existen en el módulo security; la interfaz administrativa aún no está expuesta por HTTP. |

## 7.4 Estado de implementación por dominio

El sistema se documenta tal como está construido. La siguiente tabla distingue lo que ya está conectado de lo que se encuentra en fase de desarrollo, de conformidad con la recomendación de la guía de no incluir módulos que no existen en el software.

| **Requerimiento (SRS)**                    | **Módulo del software**                             | **Estado**                                                                                                                      |
|--------------------------------------------|-----------------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------|
| RF1 – Gestión de usuarios                  | security (backend) + auth (web y móvil)             | Implementado: registro, verificación de correo, login, refresh, logout, recuperación y cambio de contraseña, perfil y permisos. |
| RF2 – Gestión de proyectos y hogares       | home (backend) + dashboard/DetailHome (web)         | Implementado: crear hogar, código de acceso, miembros, favoritos y umbrales.                                                    |
| RF3 – Gestión y configuración del sistema  | settings (web y móvil) + user_configuration (tabla) | Parcial: tema, idioma y notificaciones se gestionan en el cliente; la preferencia se almacena en la tabla user_configuration.   |
| RF4 – Registro y procesamiento del consumo | measurement y alert (backend) + Consumption (web)   | Parcial: la API de mediciones y alertas está operativa; las gráficas del front web usan datos de demostración.                  |
| RF5 – Gestión del dispositivo de medición  | device (backend)                                    | Esqueleto: existe el esquema de tablas y el módulo declarado, sin casos de uso implementados.                                   |
| Recomendaciones automáticas                | recommendation (backend)                            | Esqueleto: tabla creada y módulo declarado; la lógica de análisis está pendiente.                                               |

# 8. Tecnologías utilizadas

La siguiente tabla registra los lenguajes, frameworks, librerías, herramientas de base de datos, de diseño y de control de versiones empleados en el proyecto, con su versión o detalle de uso.

| **Componente**                 | **Tecnología**                           | **Versión / detalle**                                             |
|--------------------------------|------------------------------------------|-------------------------------------------------------------------|
| Lenguaje del backend           | Java                                     | Java 21 (LTS)                                                     |
| Framework del backend          | Spring Boot                              | Spring Boot 4.1.1 (Spring Framework 7)                            |
| Estilo arquitectónico          | Spring Modulith + arquitectura hexagonal | spring-modulith 2.1.1, 7 módulos (bounded contexts)               |
| Construcción del backend       | Maven Wrapper                            | mvnw / mvnw.cmd, spring-boot-maven-plugin                         |
| Persistencia                   | Spring Data JPA + Hibernate              | ddl-auto = validate (el esquema lo define Liquibase)              |
| Migraciones de base de datos   | Liquibase                                | db/changelog con changesets por módulo                            |
| Motor de base de datos         | MySQL                                    | MySQL 8.4, collation utf8mb4_unicode_ci, esquema energy_monitor   |
| Autenticación y autorización   | Spring Security + OAuth2 Resource Server | JWT RS256 con par de claves RSA (PEM), BCrypt para contraseñas    |
| Validación de datos            | Jakarta Bean Validation                  | @NotBlank, @Size, @NotNull, @PositiveOrZero                       |
| Documentación de la API        | springdoc-openapi                        | springdoc 3.1.1, Swagger UI y /v3/api-docs                        |
| Correo electrónico             | spring-boot-starter-mail                 | SMTP con STARTTLS; Mailpit como servidor local de pruebas         |
| Monitoreo y salud              | Spring Boot Actuator                     | /actuator/health e /actuator/info                                 |
| Pruebas del backend            | JUnit 5 + Spring Boot Test               | ≈105 archivos de prueba en src/test/java                          |
| Front-end web                  | React + Vite                             | React 19.2, Vite 8.0, JavaScript (ESM), sin TypeScript            |
| Enrutamiento web               | react-router-dom                         | react-router-dom 7.17 con rutas protegidas y layouts anidados     |
| Consumo de API (web)           | axios                                    | axios 1.17 con interceptores de petición y respuesta              |
| Formularios y validación (web) | react-hook-form + zod                    | react-hook-form 7.72, zod 4.3, @hookform/resolvers                |
| Gráficas (web)                 | recharts                                 | recharts 3.8 (AreaChart, PieChart, BarChart)                      |
| Internacionalización (web)     | i18next + react-i18next                  | 4 idiomas: es, en, fr, pt                                         |
| Estilos (web)                  | CSS Modules + variables CSS              | design system propio en src/design, temas claro/oscuro            |
| Pruebas (web)                  | Vitest + Testing Library                 | vitest 5.0, jsdom, 11 archivos de prueba                          |
| App móvil                      | Expo + React Native                      | Expo SDK 57, React Native 0.86.3, TypeScript 6 (strict)           |
| Enrutamiento móvil             | Expo Router                              | rutas file-based en src/app con rutas tipadas                     |
| Gráficas (móvil)               | react-native-chart-kit                   | LineChart, BarChart y PieChart sobre react-native-svg             |
| Estilos (móvil)                | StyleSheet                               | design system propio en src/constants (theme, colors, spacing)    |
| Control de versiones           | Git y GitHub                             | repositorios de código, documentación y app móvil                 |
| Diseño y diagramas             | Draw.io y PlantUML                       | casos de uso, actividades, MER, diagrama de clases y arquitectura |

## 8.1 Dependencias principales del backend (pom.xml)

El archivo pom.xml del backend declara como dependencias centrales spring-boot-starter-webmvc, spring-boot-starter-data-jpa, spring-boot-starter-security, spring-boot-starter-oauth2-resource-server, spring-boot-starter-validation, spring-boot-starter-liquibase, spring-boot-starter-mail, spring-boot-starter-actuator, spring-modulith-starter-core (con sus módulos de observabilidad y actuator), springdoc-openapi-starter-webmvc-ui 3.1.1, mysql-connector-j (runtime) y el conjunto de starters de prueba (\*-test y spring-modulith-starter-test).

## 8.2 Scripts disponibles

| **Repositorio** | **Script**                      | **Función**                                                     |
|-----------------|---------------------------------|-----------------------------------------------------------------|
| backend         | mvnw spring-boot:run            | Compila y arranca la API en http://localhost:8080.              |
| backend         | mvnw test                       | Ejecuta la suite de pruebas (requiere MySQL en ejecución).      |
| backend         | mvnw package                    | Genera el archivo JAR ejecutable en /target.                    |
| backend         | docker compose up -d            | Levanta el contenedor MySQL 8.4 con el esquema energy_monitor.  |
| web             | npm run dev                     | Inicia el servidor de desarrollo Vite en http://localhost:5173. |
| web             | npm run build / npm run preview | Construye la aplicación en /dist y la sirve para revisión.      |
| web             | npm test                        | Ejecuta las pruebas con Vitest.                                 |
| web             | npm run lint                    | Analiza el código con ESLint.                                   |
| móvil           | npm run start                   | Inicia Metro Bundler de Expo.                                   |
| móvil           | npm run android / ios / web     | Ejecuta la aplicación en Android, iOS o navegador.              |
| móvil           | npm run lint                    | Analiza el código con ESLint (no hay pruebas automatizadas).    |

# 9. Arquitectura del sistema

## 9.1 Arquitectura general

El sistema adopta una arquitectura cliente-servidor en tres capas. La capa de presentación está conformada por la aplicación web y la aplicación móvil; la capa de lógica corresponde a la API REST construida en Spring Boot; y la capa de datos corresponde a MySQL, accedida mediante JPA/Hibernate y gestionada con Liquibase.

> **Imágenes y diagramas (Figuras 1 a 11):** todas las figuras de este documento se encuentran en la siguiente carpeta de Google Drive.  
> https://drive.google.com/drive/folders/1tIzih2FKP8xte4vKYEtrWThFmZxR1oBX?usp=sharing

**Figura 1.** *Arquitectura general del sistema (tres capas: presentación, lógica y datos).*

| **Capa**     | **Tecnología**                                        | **Responsabilidad**                                                                                  |
|--------------|-------------------------------------------------------|------------------------------------------------------------------------------------------------------|
| Presentación | React 19 (web) y Expo SDK 57 (móvil)                  | Interfaz de usuario, validación de formularios, manejo de sesión y consumo de la API REST.           |
| Lógica       | Spring Boot 4.1.1 + Spring Security + Spring Modulith | Autenticación, autorización, casos de uso, validación de reglas de negocio y publicación de eventos. |
| Datos        | MySQL 8.4 + Spring Data JPA + Liquibase               | Persistencia de las 27 tablas, esquema versionado y consultas optimizadas.                           |
| Transversal  | Docker Compose, Mailpit y Actuator                    | Contenedor de base de datos, correo de pruebas y punto de verificación de salud del servicio.        |

## 9.2 Estilo arquitectónico y patrones de diseño

- **Monolito modular (Spring Modulith).** El backend es una única aplicación organizada en siete módulos o contextos acotados (security, home, measurement, alert, notification, device y recommendation). Cada módulo declara sus dependencias permitidas en el archivo package-info.java y sólo puede comunicarse con los demás mediante su API pública; esta restricción se verifica automáticamente con la prueba ModularityTests.

- **Arquitectura hexagonal por módulo.** Dentro de cada módulo conviven puertos de entrada (application/port/in), puertos de salida (application/port/out), casos de uso (usecase), adaptadores de entrada (adapter/in/web) y de salida (adapter/out/persistence, out/security, out/client) y un directorio infrastructure con la configuración de Spring.

- **Eventos en proceso.** El módulo measurement publica el evento MeasurementRecorded; el módulo alert lo escucha con @EventListener y genera las alertas de umbral. El mismo patrón se usa para AlertRaised. Así se evita el acoplamiento directo entre mediciones y alertas.

- **API REST sin estado (stateless).** No se usan cookies ni sesiones HTTP: cada petición transporta un JWT en la cabecera Authorization. La política de CORS se configura de forma explícita y restringida.

- **Front-end por capacidades (features).** Tanto la web como la móvil organizan el código en carpetas por capacidad funcional (auth, dashboard, homes, notifications…) con componentes, estilos, validaciones zod y pruebas propias. El estado global se maneja con React Context y la sesión se persiste en localStorage (web).

## 9.3 Comunicación entre componentes

Las aplicaciones cliente se comunican con la API mediante peticiones HTTP en formato JSON bajo el prefijo /api/v1. Toda petición autenticada incluye la cabecera Authorization: Bearer \<accessToken\>; además, las operaciones del módulo home reciben la cabecera X-User-Id con el identificador de la cuenta (solución temporal declarada en el código). El front web renueva automáticamente el access token ante una respuesta 401 mediante POST /auth/refresh, con rotación del refresh token y bloqueo entre pestañas con la API Web Locks.

| **Elemento de comunicación** | **Detalle**                                                                               |
|------------------------------|-------------------------------------------------------------------------------------------|
| Protocolo                    | HTTP/1.1 sobre HTTPS en producción; HTTP en desarrollo local.                             |
| Formato                      | JSON (Content-Type: application/json).                                                    |
| Prefijo de la API            | /api/v1 (todas las rutas del backend).                                                    |
| Autenticación                | Authorization: Bearer \<JWT RS256\> con expiración de 15 minutos.                         |
| Identidad adicional          | X-User-Id: \<idUser\> (uso restringido al módulo home, pendiente de eliminar).            |
| Origen permitido (CORS)      | http://localhost:5173 (variable SECURITY_CORS_ALLOWED_ORIGINS), sin credenciales.         |
| Manejo de errores            | Respuesta con estructura ApiError: timestamp, status, error, message, path y fieldErrors. |
| Documentación                | OpenAPI y Swagger UI disponibles en /v3/api-docs y /swagger-ui.html.                      |
| Salud del servicio           | GET /actuator/health devuelve {"status":"UP"}.                                            |

La figura siguiente describe el flujo completo de un inicio de sesión y el acceso posterior a un recurso protegido.

**Figura 2.** *Secuencia de autenticación: inicio de sesión, acceso protegido y renovación de token.*

# 10. Estructura del proyecto

El proyecto se distribuye en cuatro repositorios: tres de código fuente (backend, front web y app móvil) y uno de documentación. La tabla siguiente resume su contenido y la forma de ejecutarlos.

| **Repositorio**                                     | **Tipo**                     | **Tecnología**                                                  | **Punto de entrada**                                                 | **Ejecución**                            |
|-----------------------------------------------------|------------------------------|-----------------------------------------------------------------|----------------------------------------------------------------------|------------------------------------------|
| energy-monitor-backend                              | API REST (backend)           | Java 21 + Spring Boot 4.1.1 + Maven                             | src/main/java/com/energymonitor/EnergyMonitorBackendApplication.java | .\mvnw.cmd spring-boot:run (puerto 8080) |
| Energy-Monitor-application                          | Aplicación web (front-end)   | React 19 + Vite 8 (JavaScript)                                  | src/main.jsx sobre index.html                                        | npm install / npm run dev (puerto 5173)  |
| Energy-Monitor-Mobile                               | Aplicación móvil (front-end) | Expo SDK 57 + React Native 0.86                                 | src/app/\_layout.tsx (expo-router/entry)                             | npm install / npm run start              |
| Documentacion-proyecto-Monitor-de-consumo-electrico | Documentación del proyecto   | SRS, informe técnico, diagramas, mockups y diccionario de datos | SRS.docx, README.md                                                  | No requiere ejecución                    |

## 10.1 Estructura del backend

> energy-monitor-backend/
>
> ├── pom.xml definición Maven y dependencias
>
> ├── mvnw / mvnw.cmd Maven Wrapper (no requiere Maven instalado)
>
> ├── docker-compose.yml contenedor MySQL 8.4 (puerto 3306)
>
> ├── .env.example plantilla de variables de entorno
>
> ├── README.md instrucciones del backend
>
> └── src/
>
> ├── main/
>
> │ ├── java/com/energymonitor/
>
> │ │ ├── EnergyMonitorBackendApplication.java punto de entrada
>
> │ │ ├── security/ identidad, JWT, RBAC y auditoría
>
> │ │ ├── home/ hogares, miembros y umbrales
>
> │ │ ├── measurement/ mediciones y niveles de consumo
>
> │ │ ├── alert/ alertas por umbral y conectividad
>
> │ │ ├── notification/ envío y registro de correos
>
> │ │ ├── device/ (esqueleto) dispositivos medidores
>
> │ │ └── recommendation/ (esqueleto) recomendaciones de ahorro
>
> │ └── resources/
>
> │ ├── application.yaml configuración base
>
> │ ├── application-local.yaml(.example) perfil local y claves
>
> │ └── db/changelog/ maestro Liquibase + changesets
>
> │ ├── db.changelog-master.xml
>
> │ ├── security/ home/ device/ measurement/
>
> │ └── alert/ recommendation/ notification/
>
> └── test/java/com/energymonitor/ ≈105 archivos de prueba

**Organización interna de cada módulo (ejemplo con home):**

> home/
>
> ├── package-info.java declara los límites del módulo (@ApplicationModule)
>
> ├── api/ contrato público (HomeApi, HomeDto) usado por otros módulos
>
> ├── domain/model/ entidades puras de dominio (Home, UserHome, Role…)
>
> ├── application/
>
> │ ├── port/in/ puertos de entrada (CreateHome, JoinHome, ListHomes…)
>
> │ ├── port/out/ puertos de salida (persistencia, generadores)
>
> │ ├── usecase/ implementación de los casos de uso
>
> │ ├── command/ result/ estructuras de entrada y salida
>
> │ └── exception/ excepciones de aplicación
>
> ├── adapter/
>
> │ ├── in/web/ controladores REST, DTOs y manejadores de error
>
> │ ├── in/event/ escuchadores de eventos
>
> │ └── out/persistence/ entidades JPA, repositorios, mappers y adaptadores
>
> └── infrastructure/ configuración de Spring y API interna del módulo

## 10.2 Estructura de la aplicación web

> Energy-Monitor-application/
>
> ├── index.html · package.json · vite.config.js
>
> ├── vitest.config.js · eslint.config.js · .env.example
>
> ├── public/ (iconos y recursos)
>
> └── src/
>
> ├── main.jsx monta ThemeProvider \> HomeProvider \> App
>
> ├── App.jsx · index.css
>
> ├── routes/ AppRoutes, ProtectedRoute, HomeRoute
>
> ├── context/ HomeContext, ThemeContext, useHomes
>
> ├── services/
>
> │ ├── http/ httpClient (axios), interceptores y errores
>
> │ ├── auth/ authApi, session (localStorage), useCurrentPerson
>
> │ ├── home/ homeApi (hogares, miembros, umbrales)
>
> │ └── alerts/ alertApi (alertas del hogar)
>
> ├── components/ layout/ (Navbar, MainLayout, Sidebar) y shared/
>
> ├── design/ design system (Button, Input, Card, Badge…)
>
> ├── features/ landingPage, auth, dashboard, DetailHome,
>
> │ Favorites, Notifications, Account, SettingsPage
>
> ├── i18n/ locales es, en, fr y pt
>
> └── test/setup.js configuración de Vitest

## 10.3 Estructura de la aplicación móvil

> Energy-Monitor-Mobile/
>
> ├── app.json · package.json · tsconfig.json · AGENTS.md
>
> ├── assets/ iconos y pantalla de inicio
>
> ├── scripts/reset-project.js
>
> └── src/
>
> ├── app/ rutas Expo Router:
>
> │ \_layout.tsx (providers y Stack), index.tsx (login),
>
> │ register.tsx, verify-account.tsx, recover-password.tsx,
>
> │ dashboard.tsx, home/\[id\].tsx, favorites.tsx,
>
> │ notifications.tsx, settings.tsx, account.tsx
>
> ├── components/ Button, Card, Input, Modal, Chart, OtpInput… + layout/
>
> ├── constants/ theme, colors, spacing, typography
>
> ├── context/ HomeContext, UserContext, ThemeContext
>
> ├── features/ auth, dashboard, homes, favorites,
>
> │ notifications, settings, account
>
> ├── i18n/ locales es, en, fr y pt
>
> └── validation/ mensajes i18n de los esquemas zod

## 10.4 Función de las carpetas principales

| **Carpeta**                                                                | **Función**                                                                              |
|----------------------------------------------------------------------------|------------------------------------------------------------------------------------------|
| backend/src/main/java/com/energymonitor/\<módulo\>/domain/model            | Entidades de dominio puras, sin dependencias de Spring.                                  |
| backend/src/main/java/com/energymonitor/\<módulo\>/application             | Casos de uso, puertos de entrada y de salida, comandos, resultados y excepciones.        |
| backend/src/main/java/com/energymonitor/\<módulo\>/adapter/in/web          | Controladores REST, DTOs y manejadores de excepciones (@RestControllerAdvice).           |
| backend/src/main/java/com/energymonitor/\<módulo\>/adapter/out/persistence | Entidades JPA, repositorios, mappers y adaptadores de persistencia.                      |
| backend/src/main/resources/db/changelog                                    | Cambiosets de Liquibase que construyen el esquema de MySQL.                              |
| backend/src/test/java                                                      | Pruebas unitarias, de integración web y de seguridad; verificación de límites de módulo. |
| web/src/routes                                                             | Definición de rutas, protecciones y layouts de la SPA.                                   |
| web/src/services                                                           | Capa de acceso a la API: cliente HTTP, interceptores, sesión y endpoints por dominio.    |
| web/src/features                                                           | Pantallas y componentes agrupados por funcionalidad.                                     |
| web/src/design                                                             | Componentes reutilizables de interfaz (botones, tarjetas, campos) y sus estilos.         |
| mobile/src/app                                                             | Rutas file-based de Expo Router que montan cada pantalla.                                |
| mobile/src/features                                                        | Pantallas, componentes, hooks, validaciones y datos de demostración por funcionalidad.   |

# 11. Base de datos

**Motor:** MySQL 8.4, ejecutado en contenedor Docker (docker-compose.yml) con collation utf8mb4_unicode_ci.

**Esquema:** energy_monitor, con 27 tablas distribuidas en siete contextos. El usuario de aplicación es energy_app.

**Gestión del esquema:** Liquibase aplica los cambiosets versionados en src/main/resources/db/changelog; Hibernate está configurado con ddl-auto = validate, es decir, sólo valida que el esquema coincida con las entidades y nunca crea tablas por sí mismo.

**Convenciones:** nombres en snake_case; llaves primarias de tipo VARCHAR(10); columnas de auditoría created_at, updated_at y deleted_at en todas las tablas; borrado lógico (ningún DELETE físico por defecto); índices con el patrón idx\_\[tabla\]\_\[columnas\]. Las referencias entre contextos se almacenan como identificadores a nivel de aplicación y no generan llaves foráneas SQL.

## 11.1 Diagrama entidad-relación

El siguiente diagrama muestra las 27 entidades del sistema, agrupadas por contexto (Security, Home Management, Devices, Monitoring and measurements, Alerts, Recommendations y Notifications), con sus atributos, llaves y relaciones. Las relaciones punteadas entre contextos representan referencias de aplicación (sin foreign key en SQL).

> **Recomendación de lectura:** por su tamaño, el diagrama se presenta en página horizontal; en Word puede ampliarse con el zoom para leer los atributos de cada entidad. En el Anexo B se incluye el mismo modelo desagregado por contexto, con mayor legibilidad.
>
**Figura 3.** *Modelo entidad-relación (MER) completo del sistema «Monitor de Consumo Eléctrico».*

## 11.2 Tablas principales

La tabla siguiente lista las 27 tablas del esquema, el contexto al que pertenecen, su descripción, su llave primaria y las relaciones declaradas en el diccionario de datos.

| **Tabla**             | **Contexto**                | **Descripción**                                                                                                                                                                                                    | **Llave primaria** | **Relaciones**                                                                                                                                                          |
|-----------------------|-----------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|--------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Person                | Security                    | Registra los datos personales asociados a un usuario.                                                                                                                                                              | —                  | Person 1:1 User                                                                                                                                                         |
| User                  | Security                    | Gestiona las credenciales, estado y datos de acceso del usuario.                                                                                                                                                   | —                  | User 1:1 Person; User 1:1 UserConfiguration; User 1:N PasswordResetToken; User 1:N UserSession; User 1:N UserSystemRole; User 1:N AuditLog                              |
| UserConfiguration     | Security                    | Almacena las preferencias de configuración de cada usuario.                                                                                                                                                        | —                  | UserConfiguration 1:1 User                                                                                                                                              |
| PasswordResetToken    | Security                    | Almacena tokens para recuperación de contraseña.                                                                                                                                                                   | —                  | PasswordResetToken N:1 User                                                                                                                                             |
| RefreshToken          | Security                    | Cada fila es una generación de refresh token. Solo se guarda el hash SHA-256 del secreto, nunca el secreto en claro. family_id agrupa todas las generaciones de un mismo login y parent_id encadena la genealogía. | id_refresh_token   | RefreshToken N:1 UserSession; RefreshToken 0..1:1 RefreshToken (parent_id)                                                                                              |
| UserSession           | Security                    | Registra las sesiones activas y renovables del usuario.                                                                                                                                                            | —                  | UserSession N:1 User; UserSession 1:N RefreshToken                                                                                                                      |
| SecurityConfiguration | Security                    | Define parámetros configurables de seguridad del sistema.                                                                                                                                                          | —                  | Sin relaciones registradas                                                                                                                                              |
| PasswordPolicy        | Security                    | Define las reglas de validación y expiración de contraseñas.                                                                                                                                                       | —                  | Sin relaciones registradas                                                                                                                                              |
| LoginErrorLog         | Security                    | Registra los errores producidos durante el inicio de sesión.                                                                                                                                                       | —                  | LoginErrorLog N:1 User                                                                                                                                                  |
| SystemRole            | Security                    | Define los roles disponibles en el sistema.                                                                                                                                                                        | —                  | SystemRole 1:N UserSystemRole; SystemRole 1:N SystemRolePermission                                                                                                      |
| UserSystemRole        | Security                    | Relaciona usuarios con roles del sistema.                                                                                                                                                                          | —                  | UserSystemRole N:1 User; UserSystemRole N:1 SystemRole                                                                                                                  |
| Permission            | Security                    | Define los permisos funcionales del sistema.                                                                                                                                                                       | —                  | Permission 1:N SystemRolePermission                                                                                                                                     |
| SystemRolePermission  | Security                    | Relaciona roles con permisos.                                                                                                                                                                                      | —                  | SystemRolePermission N:1 SystemRole; SystemRolePermission N:1 Permission                                                                                                |
| AuditLog              | Security                    | Registra las acciones relevantes ejecutadas por los usuarios.                                                                                                                                                      | —                  | AuditLog N:1 User                                                                                                                                                       |
| HomeType              | Home Management             | Catálogo de tipos de hogar.                                                                                                                                                                                        | —                  | HomeType 1:N Home                                                                                                                                                       |
| Home                  | Home Management             | Registra los hogares administrados por el sistema.                                                                                                                                                                 | —                  | Home N:1 HomeType; Home 1:1 HomeThresholds; Home 1:N UserHome; Home 1:N DeviceHome; Home 1:N Alert; Home 1:N Recommendation                                             |
| HomeThresholds        | Home Management             | Configura los límites de consumo de un hogar.                                                                                                                                                                      | —                  | HomeThresholds 1:1 Home                                                                                                                                                 |
| UserHome              | Home Management             | Relaciona usuarios con hogares y define su rol.                                                                                                                                                                    | —                  | UserHome N:1 User; UserHome N:1 Home                                                                                                                                    |
| ApplianceType         | Devices                     | Catálogo de tipos de electrodomésticos.                                                                                                                                                                            | —                  | ApplianceType 1:N Device                                                                                                                                                |
| Device                | Devices                     | Registra los dispositivos de medición instalados.                                                                                                                                                                  | —                  | Device N:1 ApplianceType; Device 1:N DeviceStatusLog; Device 1:N DeviceHome; Device 1:N Measurement; Device 1:N Alert; Device 1:N Recommendation                        |
| DeviceStatusLog       | Devices                     | Registra el estado y conectividad de los dispositivos.                                                                                                                                                             | —                  | DeviceStatusLog N:1 Device                                                                                                                                              |
| DeviceHome            | Devices                     | Relaciona dispositivos con hogares.                                                                                                                                                                                | —                  | DeviceHome N:1 Device; DeviceHome N:1 Home                                                                                                                              |
| Measurement           | Monitoring and measurements | Almacena las mediciones eléctricas registradas.                                                                                                                                                                    | —                  | Measurement N:1 Device                                                                                                                                                  |
| ConsumptionLevel      | Monitoring and measurements | Define niveles de riesgo y sus rangos de consumo.                                                                                                                                                                  | —                  | ConsumptionLevel 1:N Alert                                                                                                                                              |
| Alert                 | Alerts                      | Registra alertas de umbral y conectividad.                                                                                                                                                                         | —                  | Alert N:1 Home; Alert N:1 Device; Alert N:1 ConsumptionLevel; Alert N:1 Measurement                                                                                     |
| Recommendation        | Recommendations             | Registra recomendaciones para los hogares y dispositivos.                                                                                                                                                          | —                  | Recommendation N:1 Home; Recommendation N:1 Device                                                                                                                      |
| Notification          | Notifications               | Registro de entrega de un mensaje al usuario. Alert y Recommendation son eventos; Notification registra si el mensaje fue entregado, por qué canal y su estado de lectura.                                         | —                  | Notification N:1 User (referencia aplicación); Notification N:1 Home (referencia aplicación); Notification referencia Alert/Recommendation por (source_type, source_id) |

## 11.3 Diccionario de datos

El diccionario de datos describe campo por campo cada tabla del esquema: restricción de clave, tipo de dato, tamaño y descripción. Se reproduce a partir del archivo «Base de datos/Diccionario de Datos.xlsx», entregado por el equipo de bases de datos.

**Leyenda de restricciones:** PK = llave primaria; FK = llave foránea; UNIQUE = valor único; NOT NULL = campo obligatorio; ENUM = conjunto de valores permitidos. Las columnas de auditoría (created_at, updated_at y deleted_at) se repiten en todas las tablas.

### Person — Security

*Registra los datos personales asociados a un usuario.*

| **Campo** | **Tipo** | **Tamaño** | **Restricción** | **Descripción**             |
|-----------|----------|------------|-----------------|-----------------------------|
| id_person | VARCHAR  | 10         | NOT NULL        | Identificador de la persona |
| name      | VARCHAR  | 100        | NOT NULL        | Nombres de la persona       |
| last_name | VARCHAR  | 100        | NOT NULL        | Apellidos de la persona     |

**Relaciones:** Person 1:1 User

### User — Security

*Gestiona las credenciales, estado y datos de acceso del usuario.*

| **Campo**             | **Tipo**  | **Tamaño** | **Restricción**        | **Descripción**                                |
|-----------------------|-----------|------------|------------------------|------------------------------------------------|
| id_user               | VARCHAR   | 10         | NOT NULL               | Identificador del usuario                      |
| person_id             | VARCHAR   | 10         | FK / UNIQUE / NOT NULL | Persona asociada                               |
| password_hash         | VARCHAR   | 255        | NOT NULL               | Hash de la contraseña                          |
| email                 | VARCHAR   | 255        | UNIQUE / NOT NULL      | Correo electrónico                             |
| email_verified        | BOOLEAN   |            | NOT NULL               | Indica si el correo fue verificado             |
| registration_date     | TIMESTAMP |            | NOT NULL               | Fecha y hora de registro                       |
| status                | ENUM      |            | NOT NULL               | Estado de la cuenta                            |
| failed_login_attempts | INT       |            | NOT NULL               | Cantidad de intentos fallidos                  |
| last_login_at         | TIMESTAMP |            | —                      | Último inicio de sesión                        |
| profile_image         | VARCHAR   | 255        | —                      | Ruta o URL de la imagen de perfil de la cuenta |

**Relaciones:** User 1:1 Person • User 1:1 UserConfiguration • User 1:N PasswordResetToken • User 1:N UserSession • User 1:N UserSystemRole • User 1:N AuditLog

### UserConfiguration — Security

*Almacena las preferencias de configuración de cada usuario.*

| **Campo**        | **Tipo** | **Tamaño** | **Restricción**        | **Descripción**                   |
|------------------|----------|------------|------------------------|-----------------------------------|
| id_configuration | VARCHAR  | 10         | NOT NULL               | Identificador de la configuración |
| user_id          | VARCHAR  | 10         | FK / UNIQUE / NOT NULL | Usuario configurado               |
| notify_by_email  | BOOLEAN  |            | NOT NULL               | Activa notificaciones por correo  |
| notify_by_push   | BOOLEAN  |            | NOT NULL               | Activa notificaciones push        |
| color_theme      | VARCHAR  | 50         | —                      | Tema de color                     |
| language         | VARCHAR  | 50         | —                      | Idioma preferido                  |
| social_provider  | VARCHAR  | 50         | —                      | Proveedor de autenticación social |

**Relaciones:** UserConfiguration 1:1 User

### PasswordResetToken — Security

*Almacena tokens para recuperación de contraseña.*

| **Campo**      | **Tipo**  | **Tamaño** | **Restricción**   | **Descripción**                  |
|----------------|-----------|------------|-------------------|----------------------------------|
| id_reset_token | VARCHAR   | 10         | NOT NULL          | Identificador del token          |
| user_id        | VARCHAR   | 10         | FK / NOT NULL     | Usuario propietario              |
| reset_token    | VARCHAR   | 100        | UNIQUE / NOT NULL | Token de recuperación            |
| used           | BOOLEAN   |            | NOT NULL          | Indica si el token fue utilizado |
| expiration_at  | TIMESTAMP |            | NOT NULL          | Fecha de expiración              |

**Relaciones:** PasswordResetToken N:1 User

### RefreshToken — Security

*Cada fila es una generación de refresh token. Solo se guarda el hash SHA-256 del secreto, nunca el secreto en claro. family_id agrupa todas las generaciones de un mismo login y parent_id encadena la genealogía.*

| **Campo**        | **Tipo**  | **Tamaño** | **Restricción**   | **Descripción**                                        |
|------------------|-----------|------------|-------------------|--------------------------------------------------------|
| id_refresh_token | VARCHAR   | 10         | PK / NOT NULL     | Identificador de la generación del token               |
| id_user_session  | VARCHAR   | 10         | FK / NOT NULL     | Sesión a la que pertenece la generación                |
| family_id        | VARCHAR   | 10         | NOT NULL          | Identificador de la raíz de la familia de generaciones |
| parent_id        | VARCHAR   | 10         | FK                | Generación que esta reemplaza                          |
| token_hash       | VARCHAR   | 64         | UNIQUE / NOT NULL | Hash SHA-256 del secreto (nunca el secreto en claro)   |
| status           | ENUM      |            | NOT NULL          | Estado del ciclo de vida de la generación              |
| expires_at       | TIMESTAMP |            | NOT NULL          | Fecha de expiración                                    |
| rotated_at       | TIMESTAMP |            | —                 | Fecha en que fue reemplazada                           |
| revoked_at       | TIMESTAMP |            | —                 | Fecha de revocación                                    |
| revoked_reason   | ENUM      |            | —                 | Motivo de la revocación                                |

**Relaciones:** RefreshToken N:1 UserSession • RefreshToken 0..1:1 RefreshToken (parent_id)

### UserSession — Security

*Registra las sesiones activas y renovables del usuario.*

| **Campo**       | **Tipo**  | **Tamaño** | **Restricción** | **Descripción**                         |
|-----------------|-----------|------------|-----------------|-----------------------------------------|
| id_user_session | VARCHAR   | 10         | NOT NULL        | Identificador de sesión                 |
| user_id         | VARCHAR   | 10         | FK / NOT NULL   | Usuario de la sesión                    |
| revoked         | BOOLEAN   |            | NOT NULL        | Indica si la sesión fue revocada        |
| ip_address      | VARCHAR   | 45         | —               | Dirección IP                            |
| user_agent      | VARCHAR   | 150        | —               | Agente del cliente                      |
| expiration_at   | TIMESTAMP |            | NOT NULL        | Fecha de expiración                     |
| closed_at       | TIMESTAMP |            | —               | Fecha de cierre                         |
| revoked_at      | TIMESTAMP |            | —               | Fecha y hora de revocación de la sesión |
| revoked_reason  | ENUM      |            | —               | Motivo de la revocación                 |

**Relaciones:** UserSession N:1 User • UserSession 1:N RefreshToken

### SecurityConfiguration — Security

*Define parámetros configurables de seguridad del sistema.*

| **Campo**                 | **Tipo** | **Tamaño** | **Restricción**   | **Descripción**                |
|---------------------------|----------|------------|-------------------|--------------------------------|
| id_security_configuration | VARCHAR  | 10         | NOT NULL          | Identificador de configuración |
| config_name               | VARCHAR  | 100        | UNIQUE / NOT NULL | Nombre de la configuración     |
| config_value              | VARCHAR  | 100        | NOT NULL          | Valor de la configuración      |
| description               | VARCHAR  | 255        | —                 | Descripción                    |

*Relaciones: sin relaciones declaradas en el diccionario.*

### PasswordPolicy — Security

*Define las reglas de validación y expiración de contraseñas.*

| **Campo**          | **Tipo** | **Tamaño** | **Restricción** | **Descripción**           |
|--------------------|----------|------------|-----------------|---------------------------|
| id_password_policy | VARCHAR  | 10         | NOT NULL        | Identificador de política |
| min_length         | INT      |            | NOT NULL        | Longitud mínima           |
| max_length         | INT      |            | NOT NULL        | Longitud máxima           |
| require_uppercase  | BOOLEAN  |            | NOT NULL        | Exige mayúsculas          |
| require_numbers    | BOOLEAN  |            | NOT NULL        | Exige números             |
| require_symbols    | BOOLEAN  |            | NOT NULL        | Exige símbolos            |
| expiration_days    | INT      |            | NOT NULL        | Días de vigencia          |

*Relaciones: sin relaciones declaradas en el diccionario.*

### LoginErrorLog — Security

*Registra los errores producidos durante el inicio de sesión.*

| **Campo**      | **Tipo** | **Tamaño** | **Restricción** | **Descripción**                |
|----------------|----------|------------|-----------------|--------------------------------|
| id_login_error | VARCHAR  | 10         | NOT NULL        | Identificador del error        |
| user_id        | VARCHAR  | 10         | FK              | Usuario relacionado, si existe |
| error_type     | ENUM     |            | NOT NULL        | Tipo de error                  |
| description    | VARCHAR  | 500        | —               | Descripción del error          |
| ip_address     | VARCHAR  | 45         | —               | Dirección IP                   |

**Relaciones:** LoginErrorLog N:1 User

### SystemRole — Security

*Define los roles disponibles en el sistema.*

| **Campo**      | **Tipo** | **Tamaño** | **Restricción**   | **Descripción**           |
|----------------|----------|------------|-------------------|---------------------------|
| id_system_role | VARCHAR  | 10         | NOT NULL          | Identificador del rol     |
| name           | VARCHAR  | 50         | UNIQUE / NOT NULL | Nombre del rol            |
| description    | VARCHAR  | 200        | —                 | Descripción               |
| enabled        | BOOLEAN  |            | NOT NULL          | Indica si está habilitado |

**Relaciones:** SystemRole 1:N UserSystemRole • SystemRole 1:N SystemRolePermission

### UserSystemRole — Security

*Relaciona usuarios con roles del sistema.*

| **Campo**      | **Tipo**  | **Tamaño** | **Restricción** | **Descripción**     |
|----------------|-----------|------------|-----------------|---------------------|
| user_id        | VARCHAR   | 10         | FK / NOT NULL   | Usuario relacionado |
| system_role_id | VARCHAR   | 10         | FK / NOT NULL   | Rol asignado        |
| assigned_at    | TIMESTAMP |            | NOT NULL        | Fecha de asignación |

**Relaciones:** UserSystemRole N:1 User • UserSystemRole N:1 SystemRole

### Permission — Security

*Define los permisos funcionales del sistema.*

| **Campo**     | **Tipo** | **Tamaño** | **Restricción**   | **Descripción**           |
|---------------|----------|------------|-------------------|---------------------------|
| id_permission | VARCHAR  | 10         | NOT NULL          | Identificador del permiso |
| code          | VARCHAR  | 30         | UNIQUE / NOT NULL | Código único              |
| name          | VARCHAR  | 50         | NOT NULL          | Nombre del permiso        |
| description   | VARCHAR  | 100        | —                 | Descripción               |

**Relaciones:** Permission 1:N SystemRolePermission

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

| **Campo**    | **Tipo** | **Tamaño** | **Restricción** | **Descripción**               |
|--------------|----------|------------|-----------------|-------------------------------|
| id_audit_log | VARCHAR  | 10         | NOT NULL        | Identificador del registro    |
| user_id      | VARCHAR  | 10         | FK              | Usuario que ejecutó la acción |
| action       | ENUM     |            | NOT NULL        | Acción auditada               |
| description  | VARCHAR  | 255        | —               | Descripción                   |
| ip_address   | VARCHAR  | 45         | —               | Dirección IP                  |
| application  | VARCHAR  | 255        | —               | Aplicación de origen          |

**Relaciones:** AuditLog N:1 User

### HomeType — Home Management

*Catálogo de tipos de hogar.*

| **Campo**    | **Tipo** | **Tamaño** | **Restricción**   | **Descripción**          |
|--------------|----------|------------|-------------------|--------------------------|
| id_home_type | VARCHAR  | 10         | NOT NULL          | Identificador del tipo   |
| name         | VARCHAR  | 50         | UNIQUE / NOT NULL | Nombre del tipo de hogar |

**Relaciones:** HomeType 1:N Home

### Home — Home Management

*Registra los hogares administrados por el sistema.*

| **Campo**     | **Tipo**  | **Tamaño** | **Restricción**   | **Descripción**   |
|---------------|-----------|------------|-------------------|-------------------|
| id_home       | VARCHAR   | 10         | NOT NULL          | Código del hogar  |
| name          | VARCHAR   | 50         | NOT NULL          | Nombre del hogar  |
| home_type_id  | VARCHAR   | 10         | FK / NOT NULL     | Tipo de hogar     |
| address       | VARCHAR   | 200        | NOT NULL          | Dirección         |
| access_code   | VARCHAR   | 8          | UNIQUE / NOT NULL | Código de acceso  |
| description   | VARCHAR   | 200        | —                 | Descripción       |
| creation_date | TIMESTAMP |            | NOT NULL          | Fecha de creación |

**Relaciones:** Home N:1 HomeType • Home 1:1 HomeThresholds • Home 1:N UserHome • Home 1:N DeviceHome • Home 1:N Alert • Home 1:N Recommendation

### HomeThresholds — Home Management

*Configura los límites de consumo de un hogar.*

| **Campo**          | **Tipo** | **Tamaño** | **Restricción**        | **Descripción**             |
|--------------------|----------|------------|------------------------|-----------------------------|
| id_threshold       | VARCHAR  | 10         | NOT NULL               | Identificador del umbral    |
| home_id            | VARCHAR  | 10         | FK / UNIQUE / NOT NULL | Hogar configurado           |
| daily_limit        | DOUBLE   |            | NOT NULL               | Límite diario de consumo    |
| monthly_limit      | DOUBLE   |            | NOT NULL               | Límite mensual de consumo   |
| use_system_default | BOOLEAN  |            | NOT NULL               | Usa límites predeterminados |

**Relaciones:** HomeThresholds 1:1 Home

### UserHome — Home Management

*Relaciona usuarios con hogares y define su rol.*

| **Campo** | **Tipo** | **Tamaño** | **Restricción** | **Descripción**                |
|-----------|----------|------------|-----------------|--------------------------------|
| user_id   | VARCHAR  | 10         | NOT NULL        | Usuario relacionado            |
| home_id   | VARCHAR  | 10         | FK / NOT NULL   | Hogar relacionado              |
| role      | ENUM     |            | NOT NULL        | Rol del usuario en el hogar    |
| favorite  | BOOLEAN  |            | NOT NULL        | Indica si es el hogar favorito |

**Relaciones:** UserHome N:1 User • UserHome N:1 Home

### ApplianceType — Devices

*Catálogo de tipos de electrodomésticos.*

| **Campo**         | **Tipo** | **Tamaño** | **Restricción**   | **Descripción**        |
|-------------------|----------|------------|-------------------|------------------------|
| id_appliance_type | VARCHAR  | 10         | NOT NULL          | Identificador del tipo |
| name              | VARCHAR  | 50         | UNIQUE / NOT NULL | Nombre del tipo        |

**Relaciones:** ApplianceType 1:N Device

### Device — Devices

*Registra los dispositivos de medición instalados.*

| **Campo**         | **Tipo**  | **Tamaño** | **Restricción**   | **Descripción**               |
|-------------------|-----------|------------|-------------------|-------------------------------|
| id_device         | VARCHAR   | 10         | NOT NULL          | Identificador del dispositivo |
| name              | VARCHAR   | 50         | NOT NULL          | Nombre                        |
| appliance_type_id | VARCHAR   | 10         | FK / NOT NULL     | Tipo de electrodoméstico      |
| location          | VARCHAR   | 50         | —                 | Ubicación                     |
| description       | VARCHAR   | 200        | —                 | Descripción                   |
| installation_date | TIMESTAMP |            | NOT NULL          | Fecha de instalación          |
| device_code       | VARCHAR   | 6          | UNIQUE / NOT NULL | Código del dispositivo        |
| api_key           | VARCHAR   | 100        | NOT NULL          | Clave de integración          |

**Relaciones:** Device N:1 ApplianceType • Device 1:N DeviceStatusLog • Device 1:N DeviceHome • Device 1:N Measurement • Device 1:N Alert • Device 1:N Recommendation

### DeviceStatusLog — Devices

*Registra el estado y conectividad de los dispositivos.*

| **Campo**        | **Tipo**  | **Tamaño** | **Restricción** | **Descripción**            |
|------------------|-----------|------------|-----------------|----------------------------|
| id_device_status | VARCHAR   | 10         | NOT NULL        | Identificador del registro |
| device_id        | VARCHAR   | 10         | FK              | Dispositivo relacionado    |
| status           | ENUM      |            | NOT NULL        | Estado del dispositivo     |
| signal_strength  | INT       |            | —               | Intensidad de señal        |
| last_seen        | TIMESTAMP |            | NOT NULL        | Última fecha de conexión   |

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
| id_measurement | VARCHAR   | 10         | NOT NULL        | Identificador de la medición       |
| device_id      | VARCHAR   | 10         | NOT NULL        | Dispositivo que genera la medición |
| date_time      | TIMESTAMP |            | NOT NULL        | Fecha y hora de la medición        |
| voltage        | DOUBLE    |            | NOT NULL        | Voltaje medido                     |
| current        | DOUBLE    |            | NOT NULL        | Corriente medida                   |
| active_power   | DOUBLE    |            | NOT NULL        | Potencia activa                    |
| stored_energy  | DOUBLE    |            | NOT NULL        | Energía acumulada                  |

**Relaciones:** Measurement N:1 Device

### ConsumptionLevel — Monitoring and measurements

*Define niveles de riesgo y sus rangos de consumo.*

| **Campo**            | **Tipo** | **Tamaño** | **Restricción**   | **Descripción**         |
|----------------------|----------|------------|-------------------|-------------------------|
| id_consumption_level | VARCHAR  | 10         | NOT NULL          | Identificador del nivel |
| name                 | ENUM     |            | UNIQUE / NOT NULL | Nivel de riesgo         |
| description          | VARCHAR  | 200        | NOT NULL          | Descripción             |
| min_limit            | DOUBLE   |            | NOT NULL          | Límite inferior         |
| max_limit            | DOUBLE   |            | NOT NULL          | Límite superior         |

**Relaciones:** ConsumptionLevel 1:N Alert

### Alert — Alerts

*Registra alertas de umbral y conectividad.*

| **Campo**            | **Tipo**  | **Tamaño** | **Restricción** | **Descripción**            |
|----------------------|-----------|------------|-----------------|----------------------------|
| id_alert             | VARCHAR   | 10         | NOT NULL        | Identificador de la alerta |
| home_id              | VARCHAR   | 10         | NOT NULL        | Hogar al que aplica        |
| device_id            | VARCHAR   | 10         | —               | Dispositivo relacionado    |
| type                 | ENUM      |            | NOT NULL        | Tipo de alerta             |
| message_key          | VARCHAR   | 100        | NOT NULL        | Clave del mensaje          |
| date_time            | TIMESTAMP |            | NOT NULL        | Fecha y hora               |
| alert_status         | ENUM      |            | NOT NULL        | Estado de la alerta        |
| consumption_level_id | VARCHAR   | 10         | —               | Nivel de consumo asociado  |
| measurement_id       | VARCHAR   | 10         | —               | Medición asociada          |

**Relaciones:** Alert N:1 Home • Alert N:1 Device • Alert N:1 ConsumptionLevel • Alert N:1 Measurement

### Recommendation — Recommendations

*Registra recomendaciones para los hogares y dispositivos.*

| **Campo**         | **Tipo**  | **Tamaño** | **Restricción** | **Descripción**                   |
|-------------------|-----------|------------|-----------------|-----------------------------------|
| id_recommendation | VARCHAR   | 10         | NOT NULL        | Identificador de la recomendación |
| home_id           | VARCHAR   | 10         | NOT NULL        | Hogar al que aplica               |
| device_id         | VARCHAR   | 10         | —               | Dispositivo relacionado           |
| message_key       | VARCHAR   | 100        | NOT NULL        | Clave del mensaje                 |
| date_time         | TIMESTAMP |            | NOT NULL        | Fecha y hora                      |
| status            | ENUM      |            | NOT NULL        | Estado de lectura                 |
| type              | ENUM      |            | NOT NULL        | Tipo de recomendación             |

**Relaciones:** Recommendation N:1 Home • Recommendation N:1 Device

### Notification — Notifications

*Registro de entrega de un mensaje al usuario. Alert y Recommendation son eventos; Notification registra si el mensaje fue entregado, por qué canal y su estado de lectura.*

| **Campo**       | **Tipo**  | **Tamaño**           | **Restricción** | **Descripción** |
|-----------------|-----------|----------------------|-----------------|-----------------|
| id_notification | VARCHAR   | 10                   | NOT NULL        | —               |
| user_id         | VARCHAR   | 10                   | NOT NULL        | —               |
| home_id         | VARCHAR   | 10                   | —               | —               |
| channel         | ENUM      | EMAIL,PUSH           | NOT NULL        | —               |
| source_type     | ENUM      | ALERT,RECOMMENDATION | NOT NULL        | —               |
| source_id       | VARCHAR   | 10                   | —               | —               |
| message_key     | VARCHAR   | 100                  | NOT NULL        | —               |
| delivery_status | ENUM      | PENDING,SENT,FAILED  | NOT NULL        | —               |
| sent_at         | TIMESTAMP |                      | —               | —               |
| read_at         | TIMESTAMP |                      | —               | —               |
| failure_reason  | VARCHAR   | 200                  | —               | —               |
| created_at      | TIMESTAMP |                      | —               | —               |
| updated_at      | TIMESTAMP |                      | —               | —               |
| deleted_at      | TIMESTAMP |                      | —               | —               |

**Relaciones:** Notification N:1 User (referencia aplicación) • Notification N:1 Home (referencia aplicación) • Notification referencia Alert/Recommendation por (source_type, source_id)

# 12. Módulos técnicos

## 12.1 Vista general de los módulos

El backend está dividido en siete módulos (bounded contexts). Cinco de ellos están operativos y dos permanecen como esqueleto reservado para futuras funcionalidades.

| **Módulo**     | **Responsabilidad**                                                      | **Controladores / casos de uso**                                               | **Estado** |
|----------------|--------------------------------------------------------------------------|--------------------------------------------------------------------------------|------------|
| security       | Registro, autenticación, sesiones, contraseñas, perfil, RBAC y auditoría | AuthController + 15 casos de uso                                               | Activo     |
| home           | Hogares, miembros, favoritos, código de acceso y umbrales                | HomeController, HomeThresholdsController, HomeTypeController + 11 casos de uso | Activo     |
| measurement    | Registro de mediciones, histórico, última medición y estadísticas        | MeasurementController, ConsumptionLevelController + 6 casos de uso             | Activo     |
| alert          | Generación automática de alertas y bandeja de consulta/resolución        | AlertController + 4 casos de uso                                               | Activo     |
| notification   | Envío y registro de correos de verificación y recuperación               | Sin controlador propio (se consume desde security)                             | Activo     |
| device         | Dispositivos medidores, API key y estado de conexión                     | Sin implementación (sólo package-info.java)                                    | Esqueleto  |
| recommendation | Recomendaciones de ahorro generadas por análisis del consumo             | Sin implementación (sólo package-info.java)                                    | Esqueleto  |

## 12.2 Diagrama de clases del dominio

El diagrama de clases modela las clases, interfaces y enumeraciones del dominio, junto con los patrones de diseño previstos: Strategy para la autorización por rol, Observer para la reacción a las mediciones y Factory Method para la creación de alertas, recomendaciones y notificaciones. La nota incluida en el diagrama indica que las clases del patrón Strategy aún no existen en el código Java.

> **Nota:** al igual que el MER, el diagrama completo se presenta en página horizontal. El Anexo C lo desglosa por dominio para facilitar su lectura.
>
**Figura 4.** *Diagrama de clases del dominio, incluyendo los patrones Strategy, Observer y Factory Method.*

## 12.3 Módulos del backend por contexto

### 12.3.1 Módulo security

**Función:** Identidad, autenticación, sesiones, control de acceso y auditoría.

**Estado:** Activo

**Archivos relacionados:**

- security/adapter/in/web/AuthController.java (controlador único de autenticación y perfil)

- security/adapter/in/web/dto/ (18 DTOs: RegisterRequest, LoginRequest, LoginResponse, RefreshResponse, ProfileResponse, ApiError…)

- security/application/usecase/ (RegisterUserService, AuthenticateUserService, RefreshSessionService, LogoutUserSessionService, ChangePasswordService, CreatePasswordResetTokenService, EmailVerificationService, UpdateUserProfileService, CheckPermissionService, AssignRoleService, ManageUserStatusService…)

- security/domain/model/ (User, Person, Email, PasswordHash, PasswordPolicy, UserSession, RefreshToken, SystemRole, Permission, AuditLog, LoginErrorLog)

- security/infrastructure/ (SecurityFilterChainConfiguration, JwtTokenIssuer, PemRsaKeys, SecurityCorsConfiguration, JwtAuthenticationEntryPoint)

- security/adapter/out/security/ (BCryptPasswordHasherAdapter, Sha256TokenHasher, PepperedHmacResetCodeHasher, InMemoryResetAttemptLimiter)

**Tablas relacionadas:** person, user, user_configuration, password_reset_token, user_session, refresh_token, security_configuration, password_policy, login_error_log, system_role, permission, user_system_role, system_role_permission, audit_log.

**Operaciones principales:**

- Registrar cuenta y verificar el correo

- Iniciar sesión y renovar tokens

- Cerrar sesión

- Recuperar y cambiar contraseñas

- Consultar y actualizar el perfil

- Consultar permisos (RBAC)

- Registrar auditoría e intentos fallidos

### 12.3.2 Módulo home

**Función:** Gestión de hogares, miembros, favoritos y umbrales de consumo.

**Estado:** Activo

**Archivos relacionados:**

- home/adapter/in/web/HomeController.java, HomeThresholdsController.java, HomeTypeController.java

- home/adapter/in/web/CurrentUserResolver.java (resuelve el usuario desde la cabecera X-User-Id)

- home/application/usecase/ (CreateHomeService, JoinHomeService, LeaveHomeService, ListHomesService, ListMembersService, RemoveUserService, ToggleFavoriteService, GetHomeThresholdsService, UpdateHomeThresholdsService, ListHomeTypesService)

- home/domain/model/ (Home, HomeType, HomeThresholds, UserHome, Role OWNER/MEMBER, OwnerPermissions, MemberPermissions)

- home/infrastructure/HomeApiImpl.java (API pública consumida por otros módulos)

**Tablas relacionadas:** home, home_type, home_thresholds, user_home.

**Operaciones principales:**

- Crear un hogar y generar código de acceso

- Unirse a un hogar con código

- Listar hogares y miembros

- Marcar favorito

- Expulsar miembros y salir del hogar

- Consultar y actualizar umbrales diarios y mensuales

### 12.3.3 Módulo measurement

**Función:** Registro y análisis de mediciones de consumo eléctrico.

**Estado:** Activo

**Archivos relacionados:**

- measurement/adapter/in/web/MeasurementController.java, ConsumptionLevelController.java

- measurement/application/usecase/ (RegisterMeasurementService, ListMeasurementsService, GetLatestMeasurementService, GetConsumptionStatisticsService, ListConsumptionLevelsService, TimeRange)

- measurement/domain/model/ (Measurement, ConsumptionLevel)

- measurement/adapter/out/event/MeasurementEventAdapter.java (publica MeasurementRecorded)

**Tablas relacionadas:** measurement, consumption_level.

**Operaciones principales:**

- Registrar mediciones (voltaje, corriente, potencia activa, energía)

- Consultar histórico por dispositivo y rango de fechas

- Obtener la última medición

- Calcular estadísticas de consumo

- Publicar el evento MeasurementRecorded

### 12.3.4 Módulo alert

**Función:** Generación y bandeja de alertas por umbral y conectividad.

**Estado:** Activo

**Archivos relacionados:**

- alert/adapter/in/web/AlertController.java (solo lectura y resolución)

- alert/adapter/in/event/MeasurementRecordedListener.java (@EventListener)

- alert/application/usecase/ (ListAlertsService, GetAlertService, ResolveAlertService, RegisterThresholdAlertService)

- alert/infrastructure/AlertApiImpl.java y alert/api/AlertRaised.java

**Tablas relacionadas:** alert.

**Operaciones principales:**

- Generar alertas automáticamente al recibir una medición

- Listar alertas por hogar y estado

- Consultar una alerta

- Resolver una alerta

### 12.3.5 Módulo notification

**Función:** Registro de entregas de correo (verificación y recuperación).

**Estado:** Activo

**Archivos relacionados:**

- notification/infrastructure/mail/EmailSender.java, MailDeliveryConfiguration.java, AbandonedDeliverySweeper.java

- notification/application/usecase/ (SendPasswordResetEmail, SendEmailVerification)

- notification/adapter/out/client/EmailPasswordResetDeliveryAdapter.java, EmailVerificationDeliveryAdapter.java

- notification/adapter/out/persistence/NotificationPersistenceAdapter.java

**Tablas relacionadas:** notification.

**Operaciones principales:**

- Enviar correos de verificación y recuperación

- Registrar el estado de entrega (PENDING, SENT, FAILED)

- Barrer entregas abandonadas al iniciar la aplicación

### 12.3.6 Módulo device

**Función:** Registro y conexión de dispositivos medidores (API key, estado en línea/fuera de línea).

**Estado:** Esqueleto: sólo package-info.java (el esquema de tablas ya existe en Liquibase)

**Archivos relacionados:**

- device/package-info.java (@ApplicationModule con allowedDependencies = { "home" })

**Tablas relacionadas:** device, appliance_type, device_status_log, device_home.

**Operaciones principales:**

- (Pendiente) Registrar dispositivo

- (Pendiente) Autenticar dispositivo con api_key

- (Pendiente) Registrar estado de conexión

### 12.3.7 Módulo recommendation

**Función:** Recomendaciones de ahorro generadas a partir del análisis del consumo.

**Estado:** Esqueleto: sólo package-info.java (tabla recommendation ya creada por Liquibase)

**Archivos relacionados:**

- recommendation/package-info.java (@ApplicationModule con allowedDependencies = { "device", "measurement" })

**Tablas relacionadas:** recommendation.

**Operaciones principales:**

- (Pendiente) Analizar histórico de mediciones

- (Pendiente) Generar y marcar recomendaciones

## 12.4 Módulo front-end web

**Función:** interfaz de consulta y gestión del consumo eléctrico (SPA React)

**Archivos relacionados:**

- src/routes/AppRoutes.jsx (rutas), ProtectedRoute.jsx (rutas protegidas) y HomeRoute.jsx (detalle del hogar)

- src/services/http/httpClient.js (cliente axios, interceptores y renovación de token)

- src/services/auth, src/services/home y src/services/alerts (endpoints consumidos)

- src/context/HomeContext.jsx y ThemeContext.jsx (estado global)

- src/features/ (landingPage, auth, dashboard, DetailHome, Favorites, Notifications, Account, SettingsPage)

**Pantallas y rutas:**

| **Ruta**                                                  | **Pantalla**                | **Función**                                                                  |
|-----------------------------------------------------------|-----------------------------|------------------------------------------------------------------------------|
| /home, /about, /support                                   | Landing page                | Páginas públicas de presentación, acerca de y soporte.                       |
| /login, /register                                         | Login y Registro            | Autenticación y creación de cuenta con validación zod.                       |
| /VerifyAccount, /recover-password, /verifyRecoverPassword | Verificación y recuperación | Código de verificación de correo y recuperación de contraseña.               |
| /dashboard                                                | Panel de hogares            | Tarjetas de hogares, crear hogar, unirse con código y favoritos.             |
| /homes/:homeId                                            | Detalle del hogar           | Seis pestañas: Consumo, Historial, Usuarios, Dispositivos, Umbrales y Hogar. |
| /favorites                                                | Favoritos                   | Listado de hogares marcados como favoritos.                                  |
| /notifications                                            | Notificaciones              | Alertas del hogar y acción de resolverlas.                                   |
| /settings                                                 | Configuración               | Apariencia, idioma, notificaciones y ayuda.                                  |
| /account                                                  | Cuenta                      | Edición de perfil, foto, contraseña y eliminación de cuenta.                 |

**Operaciones principales:** iniciar sesión y cerrar sesión, registrar y verificar cuenta, recuperar contraseña, crear y unirse a hogares, marcar favoritos, consultar y editar miembros y umbrales, visualizar alertas y resolverlas, cambiar idioma y tema.

## 12.5 Módulo front-end móvil

**Función:** aplicación híbrida con el mismo modelo de dominio que la web

**Archivos relacionados:**

- src/app/ (rutas Expo Router: \_layout, index, dashboard, home/\[id\], favorites, notifications, settings, account)

- src/features/ (auth, dashboard, homes, favorites, notifications, settings, account)

- src/context/ (HomeContext, UserContext, ThemeContext)

- src/components/ y src/constants/ (design system propio con StyleSheet)

**Operaciones principales:** registro, verificación de código, recuperación de contraseña, panel de hogares, detalle del hogar con seis pestañas, favoritos, notificaciones, configuración de idioma y tema, y gestión de cuenta.

> **Limitación técnica conocida:** la aplicación móvil todavía no consume la API REST. El login navega directamente al panel y los datos de consumo, dispositivos, miembros y notificaciones provienen de archivos de demostración (features/\*/data). La integración está declarada como pendiente en el propio código.

# 13. API o rutas del sistema

**URL base:** http://localhost:8080/api/v1 \| **Formato:** JSON \| **Documentación interactiva:** /swagger-ui.html y /v3/api-docs

El backend expone 35 rutas. Todas las que no aparecen en la lista de rutas públicas requieren un JWT válido en la cabecera Authorization; si el token falta, expiró o es inválido, la API responde 401, y si el usuario no tiene el permiso requerido, responde 403.

| **Método** | **Ruta**                                | **Descripción**                                             | **Parámetros**                                                        | **Autenticación** |
|------------|-----------------------------------------|-------------------------------------------------------------|-----------------------------------------------------------------------|-------------------|
| POST       | /api/v1/auth/register                   | Registra una cuenta nueva.                                  | body: email, password, name, lastName, profileImage (opcional)        | Pública           |
| POST       | /api/v1/auth/email/verify               | Verifica el correo con un código de 6 dígitos.              | body: email, code                                                     | Pública           |
| POST       | /api/v1/auth/email/verification/resend  | Reenvía el código de verificación (siempre responde 202).   | body: email                                                           | Pública           |
| POST       | /api/v1/auth/login                      | Autentica y devuelve access token y refresh token.          | body: email, password                                                 | Pública           |
| POST       | /api/v1/auth/refresh                    | Renueva el access token y rota el refresh token.            | body: refreshToken                                                    | Pública           |
| POST       | /api/v1/auth/logout                     | Cierra la sesión registrada.                                | body: idUserSession                                                   | JWT               |
| POST       | /api/v1/auth/password/forgot            | Solicita el código de recuperación de contraseña.           | body: email                                                           | Pública           |
| POST       | /api/v1/auth/password/reset             | Restablece la contraseña con el código recibido.            | body: email, resetToken, newPassword                                  | Pública           |
| POST       | /api/v1/auth/password/change            | Cambia la contraseña conociendo la actual.                  | body: currentPassword, newPassword                                    | JWT               |
| PUT        | /api/v1/auth/profile                    | Actualiza nombre, apellido, imagen o correo.                | body: name, lastName, profileImage, newEmail                          | JWT               |
| GET        | /api/v1/auth/account                    | Devuelve la cuenta autenticada.                             | ninguno (identidad del JWT)                                           | JWT               |
| GET        | /api/v1/auth/permissions                | Lista los códigos de permiso del usuario.                   | ninguno                                                               | JWT               |
| POST       | /api/v1/measurements                    | Registra una medición de consumo.                           | body: deviceId, dateTime, voltage, current, activePower, storedEnergy | JWT               |
| GET        | /api/v1/measurements                    | Lista las mediciones de un dispositivo en un rango.         | query: deviceId (obligatorio), from, to                               | JWT               |
| GET        | /api/v1/measurements/latest             | Devuelve la última medición de un dispositivo.              | query: deviceId                                                       | JWT               |
| GET        | /api/v1/measurements/statistics         | Estadísticas de consumo del periodo.                        | query: deviceId, from, to                                             | JWT               |
| GET        | /api/v1/consumption-levels              | Lista los niveles de consumo (LOW, MEDIUM, HIGH, CRITICAL). | ninguno                                                               | JWT               |
| POST       | /api/v1/homes                           | Crea un hogar y devuelve su código de acceso.               | body: name, homeTypeId, address, description                          | JWT + X-User-Id   |
| GET        | /api/v1/homes                           | Lista los hogares del usuario con su rol.                   | ninguno                                                               | JWT + X-User-Id   |
| GET        | /api/v1/homes/{homeId}/members          | Lista los miembros de un hogar.                             | path: homeId                                                          | JWT + X-User-Id   |
| POST       | /api/v1/homes/join                      | Permite unirse a un hogar con su código de acceso.          | body: accessCode                                                      | JWT + X-User-Id   |
| PUT        | /api/v1/homes/{homeId}/favorite         | Marca o desmarca un hogar como favorito.                    | path: homeId                                                          | JWT + X-User-Id   |
| DELETE     | /api/v1/homes/{homeId}/members/{userId} | Expulsa a un miembro (solo OWNER).                          | path: homeId, userId                                                  | JWT + X-User-Id   |
| DELETE     | /api/v1/homes/{homeId}/members/me       | Permite al usuario salir del hogar.                         | path: homeId                                                          | JWT + X-User-Id   |
| GET        | /api/v1/homes/{homeId}/thresholds       | Consulta los umbrales diario y mensual del hogar.           | path: homeId                                                          | JWT + X-User-Id   |
| PUT        | /api/v1/homes/{homeId}/thresholds       | Actualiza los umbrales del hogar.                           | path: homeId; body: dailyLimit, monthlyLimit                          | JWT + X-User-Id   |
| GET        | /api/v1/home-types                      | Lista los tipos de hogar disponibles.                       | ninguno                                                               | JWT               |
| GET        | /api/v1/home-types/{idHomeType}         | Consulta un tipo de hogar.                                  | path: idHomeType                                                      | JWT               |
| GET        | /api/v1/alerts                          | Lista las alertas de un hogar.                              | query: homeId (obligatorio), status (PENDING \| RESOLVED)             | JWT               |
| GET        | /api/v1/alerts/{idAlert}                | Consulta una alerta por su identificador.                   | path: idAlert                                                         | JWT               |
| PUT        | /api/v1/alerts/{idAlert}/resolve        | Marca una alerta como resuelta.                             | path: idAlert                                                         | JWT               |
| GET        | /actuator/health                        | Estado del servicio (UP / DOWN).                            | ninguno                                                               | Pública           |
| GET        | /actuator/info                          | Información de la aplicación.                               | ninguno                                                               | Pública           |
| GET        | /swagger-ui.html y /v3/api-docs         | Documentación interactiva de la API (OpenAPI).              | ninguno                                                               | Pública           |
| OPTIONS    | /\*\*                                   | Preflight de CORS.                                          | cabeceras Origin y Access-Control-Request-\*                          | Pública           |

## 13.1 Códigos de respuesta habituales

| **Código**      | **Cuándo se produce**                                                         | **Estructura de la respuesta**                                 |
|-----------------|-------------------------------------------------------------------------------|----------------------------------------------------------------|
| 200 / 201 / 204 | Operación exitosa (consulta, creación o eliminación sin contenido).           | Cuerpo con el recurso o mensaje correspondiente.               |
| 400             | Validación de datos incorrecta (Bean Validation o esquema zod en el cliente). | ApiError con fieldErrors detalle de cada campo.                |
| 401             | Token ausente, expirado o credenciales inválidas.                             | ApiError con mensaje genérico (no revela si el correo existe). |
| 403             | Cuenta bloqueada o inactiva, o permiso insuficiente.                          | ApiError.                                                      |
| 404             | Recurso inexistente (hogar, alerta, miembro…).                                | ApiError o {"error": "..."} según el módulo.                   |
| 409             | Conflicto (correo ya registrado, último propietario que no puede salir).      | ApiError.                                                      |
| 422             | La contraseña no cumple la política de seguridad.                             | ApiError.                                                      |
| 429             | Límite de intentos alcanzado en verificación o recuperación.                  | ApiError con cabecera Retry-After (900 segundos).              |
| 500             | Error interno no controlado.                                                  | ApiError sin traza de pila (nunca se filtra stack trace).      |

## 13.2 Ejemplo de petición y respuesta

> POST /api/v1/auth/login
>
> Content-Type: application/json
>
> { "email": "usuario@correo.com", "password": "\*\*\*\*\*\*\*\*" }
>
> 200 OK
>
> {
>
> "accessToken": "eyJhbGciOiJSUzI1NiIsImV4cCI6MTc2…",
>
> "refreshToken": "7f3c…",
>
> "tokenType": "Bearer",
>
> "expiresIn": 900,
>
> "account": { "idUser": "user000001", "email": "usuario@correo.com", "status": "ACTIVE" }
>
> }

# 14. Seguridad

## 14.1 Controles implementados

| **Control**                   | **Implementación técnica**                                                                                                                                                                              |
|-------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Autenticación                 | JWT firmado con algoritmo RS256 y par de claves RSA en archivos PEM; vida útil del access token: 15 minutos; claims sub (idUser), person_id y email.                                                    |
| Sesiones                      | Estado sin estado (STATELESS): sin cookies ni sesión HTTP; CSRF deshabilitado por no existir credencial ambiental.                                                                                      |
| Renovación de sesión          | Refresh token opaco rotado en cada uso, almacenado como hash SHA-256 con family_id y parent_id para detectar reutilización (replay) y revocado al cambiar la contraseña.                                |
| Cifrado de contraseñas        | BCrypt (BCryptPasswordHasherAdapter) aislado del dominio; nunca se almacenan contraseñas en claro.                                                                                                      |
| Política de contraseñas       | Tabla password_policy: longitud mínima 8 y máxima 64, exige mayúsculas, números y símbolos, expira a los 90 días. Incumplimiento =\> 422.                                                               |
| Autorización (RBAC)           | Cadena de cinco tablas (system_role, permission, user_system_role, system_role_permission y user) consultada en vivo por CheckPermissionService; permisos consultables en GET /api/v1/auth/permissions. |
| Validación de entrada         | Bean Validation en los DTOs del backend (@NotBlank, @Size, @NotNull, @PositiveOrZero) y esquemas zod en los formularios de los front-ends.                                                              |
| CORS                          | Lista explícita de orígenes (SECURITY_CORS_ALLOWED_ORIGINS), sin comodines, sin credenciales y con manejo del preflight OPTIONS.                                                                        |
| Límite de intentos            | Limitador en memoria: 10 intentos por IP cada 15 minutos en recuperación y verificación de contraseña =\> 429 con cabecera Retry-After.                                                                 |
| Protección contra enumeración | Login fallido con mensaje genérico y password/forgot con respuesta 202 idéntica exista o no la cuenta; el trabajo se realiza de forma asíncrona.                                                        |
| Códigos de recuperación       | 6 dígitos, validez de 15 minutos, uso único, guardados como HMAC-SHA256 con pepper (SECURITY_RESET_PEPPER).                                                                                             |
| Auditoría                     | Tablas audit_log (acciones, IP, usuario) e login_error_log (intentos fallidos); la IP se toma de getRemoteAddr() ignorando X-Forwarded-For.                                                             |
| Documentación                 | El endpoint de OpenAPI declara el esquema bearerAuth; los endpoints públicos son sólo los de autenticación y los de infraestructura.                                                                    |
| Transporte                    | En producción se debe servir por HTTPS; en desarrollo local se usa HTTP con Mailpit como servidor de correo de pruebas.                                                                                 |

## 14.2 Limitaciones y riesgos conocidos

> Los siguientes puntos están declarados por el propio equipo en el código y deben resolverse antes de llevar el sistema a producción.

| **Limitación**                                                                                                                       | **Impacto**                                                                                     | **Recomendación**                                                                               |
|--------------------------------------------------------------------------------------------------------------------------------------|-------------------------------------------------------------------------------------------------|-------------------------------------------------------------------------------------------------|
| Cabecera X-User-Id usada por el módulo home para identificar al usuario (CurrentUserResolver está marcado como temporal e inseguro). | Un cliente podría enviar otro identificador y suplantar al usuario en las operaciones de hogar. | Resolver la identidad a partir del claim sub del JWT y eliminar la cabecera.                    |
| Access token y refresh token almacenados en localStorage (web).                                                                      | Una vulnerabilidad XSS podría leer los tokens.                                                  | Migrar a cookies httpOnly con SameSite o reforzar la política de Content Security Policy.       |
| Limitador de intentos en memoria.                                                                                                    | Se reinicia con cada arranqueste y no es compartido entre instancias.                           | Usar Redis u otro almacén compartido si el sistema se escala horizontalmente.                   |
| Módulo device sin implementar.                                                                                                       | No hay autenticación de dispositivos ni registro de su estado.                                  | Implementar la API key y la vigencia de conexión descritas en el RF5.                           |
| Módulo recommendation sin implementar.                                                                                               | La funcionalidad de recomendaciones del RF4 no está operativa.                                  | Definir el analizador de consumo a partir de los eventos MeasurementRecorded.                   |
| Datos de demostración en los front-ends (consumo, historial y dispositivos).                                                         | La interfaz muestra información que no proviene de la base de datos.                            | Sustituir los datos de demostración por llamadas a /measurements y /measurements/statistics.    |
| Aplicación móvil sin integración con la API.                                                                                         | La app móvil no refleja la información real del usuario.                                        | Implementar la capa de servicios con axios y la sesión JWT, replicando el patrón del front web. |

## 14.3 Buenas prácticas para los desarrolladores

- No commitear nunca los archivos .env, application-local.yaml ni las claves RSA (\*.pem); ambos ya están declarados en .gitignore.

- No registrar contraseñas, tokens ni códigos de recuperación en los logs de la aplicación.

- Mantener actualizadas las dependencias del backend y del front y revisar sus avisos de seguridad.

- Toda funcionalidad nueva debe validar la entrada en el backend, independientemente de las validaciones del cliente.

- Respetar los límites de módulo: no importar clases de otro contexto sin pasar por su API pública (lo verifica ModularityTests).

- Rotar las claves JWT y el pepper de recuperación si se sospecha de una filtración.

# 15. Control de versiones del software

El proyecto utiliza Git con repositorio remoto en GitHub. Cada componente del sistema se desarrolla en un repositorio independiente para facilitar su despliegue y su revisión.

| **Repositorio**                                     | **Contenido**                                                                                                                     | **Observaciones**                                                                         |
|-----------------------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------|-------------------------------------------------------------------------------------------|
| energy-monitor-backend                              | API REST en Java 21 / Spring Boot.                                                                                                | Incluye pruebas, docker-compose.yml, .env.example y la historia de migraciones Liquibase. |
| Energy-Monitor-application                          | Aplicación web React 19 / Vite 8.                                                                                                 | Incluye pruebas Vitest (11 archivos) y build de producción en /dist.                      |
| Energy-Monitor-Mobile                               | Aplicación móvil Expo SDK 57.                                                                                                     | Incluye LICENSE, AGENTS.md (documentación versionada de Expo 57) y scripts de Expo.       |
| Documentacion-proyecto-Monitor-de-consumo-electrico | SRS, informe técnico, diagramas (casos de uso, actividades, navegación, MER, clases), mockups, wireframes y diccionario de datos. | Registro de cambios en commits.md.                                                        |

- Los cambios se documentan en commits.md con identificador, fecha y mensaje del cambio, para trazar la evolución del proyecto.

- Antes de fusionar cambios se debe ejecutar la suite de pruebas del backend (mvnw test) y las pruebas del front web (npm test).

- Los cambios en el esquema de base de datos se realizan únicamente mediante un nuevo cambioset de Liquibase; nunca se editan cambiosets ya aplicados.

- Las ramas de integración deben mantener el código en estado compilable y las pruebas en verde.

- Las versiones entregadas se etiquetan para conservar la referencia exacta del software documentado en este manual.

# 16. Recomendaciones de mantenimiento

## 16.1 Actualizaciones del código

| **Área**      | **Recomendación**                                                                                                                                                               |
|---------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Backend       | Compilar con .\mvnw.cmd clean test antes de cada entrega; verificar que ModularityTests confirme que no se rompieron los límites de módulo.                                     |
| Base de datos | Crear un cambioset nuevo por cada modificación de esquema, con identificador secuencial (p. ej. security-022); ejecutar docker compose up -d para probar contra MySQL 8.4 real. |
| Dependencias  | Actualizar Spring Boot, Expo o React en ramas de prueba, ejecutando la suite completa antes de fusionar.                                                                        |
| Front web     | Mantener sincronizadas la variable VITE_API_BASE_URL y la lista de orígenes permitidos (SECURITY_CORS_ALLOWED_ORIGINS); volver a ejecutar npm test y npm run lint.              |
| Front móvil   | Revisar la documentación versionada de Expo 57 antes de incorporar librerías nativas; no hay pruebas automatizadas, por lo que la verificación es manual.                       |
| Seguridad     | Revisar trimestralmente los permisos RBAC, la política de contraseñas y la vigencia de las claves JWT.                                                                          |

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
| Las migraciones fallan al arrancar      | Revisar el log de Liquibase y el orden de los cambiosets en db.changelog-master.xml.                                                 |
| Los mensajes de error no son legibles   | Revisar la consola del navegador y el cuerpo ApiError devuelto por la API.                                                           |

## 16.4 Trabajos pendientes declarados

- Eliminar la cabecera X-User-Id y usar exclusivamente la identidad del JWT (módulo home).

- Implementar los módulos device y recommendation, hoy declarados únicamente como esqueleto.

- Conectar las gráficas de consumo e historial del front web a los endpoints de mediciones.

- Integrar la aplicación móvil con la API REST (sesión, hogares, consumo y alertas).

- Exponer por HTTP las operaciones administrativas de roles y estados de usuario que ya existen como puertos de aplicación.

- Reemplazar los datos de demostración de la app móvil y completar sus pruebas automatizadas.

# 17. Glosario

| **Término**           | **Definición**                                                                                                                        |
|-----------------------|---------------------------------------------------------------------------------------------------------------------------------------|
| ADSO                  | Análisis y Desarrollo de Software: tecnología SENA que forma parte del proyecto (ficha 3145556).                                      |
| API REST              | Interfaz de programación basada en HTTP que expone recursos en formato JSON bajo rutas versionadas (/api/v1).                         |
| Access token          | Token JWT de corta duración (15 minutos) que prueba la identidad del usuario en cada petición.                                        |
| Bounded context       | Contexto acotado de dominio (security, home, measurement…) con su propio esquema y API interna.                                       |
| BCrypt                | Algoritmo de hash con sal utilizado para guardar contraseñas de forma irreversible.                                                   |
| CORS                  | Política HTTP que controla desde qué orígenes (p. ej. http://localhost:5173) el navegador puede llamar a la API.                      |
| Changeset             | Unidad de migración de Liquibase: instrucción versionada para crear o modificar tablas.                                               |
| Docker Compose        | Herramienta que define y levanta contenedores (por ejemplo mysql:8.4) con un solo comando.                                            |
| DTO                   | Objeto de transferencia de datos: estructura JSON de entrada o salida de un endpoint.                                                 |
| ER / MER              | Diagrama entidad-relación: modelo lógico de las tablas y sus relaciones.                                                              |
| JWT                   | JSON Web Token: token firmado (algoritmo RS256) que transporta la identidad del usuario.                                              |
| Liquibase             | Gestor de esquema: aplica cambiosets de base de datos de forma controlada y versionada.                                               |
| Módulo (Modulith)     | Unidad de código con límites verificados por Spring Modulith; sólo se relaciona por su API pública.                                   |
| PK / Llave primaria   | Campo que identifica de forma única una fila (en este proyecto, VARCHAR(10)).                                                         |
| FK / Llave foránea    | Campo que referencia a otra tabla dentro del mismo contexto. Entre contextos sólo se guardan IDs (sin FK SQL).                        |
| RBAC                  | Control de acceso basado en roles: system_role, permission, user_system_role y system_role_permission.                                |
| Refresh token         | Token opaco de larga duración guardado como hash SHA-256 que permite renovar el access token (rotación y detección de reutilización). |
| SHA-256               | Función de hash criptográfico usada para guardar refresh tokens y códigos de recuperación (con pepper HMAC).                          |
| SPA                   | Single Page Application: aplicación web que se ejecuta entera en el navegador y navega sin recargar (React + react-router).           |
| Swagger / OpenAPI     | Documentación interactiva generada automáticamente en /swagger-ui.html.                                                               |
| Umbrales (thresholds) | Límites diario y mensual de consumo configurables por hogar que disparan alertas.                                                     |
| Borrado lógico        | No se ejecutan DELETE físicos: deleted_at con valor indica que el registro fue eliminado.                                             |
| X-User-Id             | Cabecera temporal usada por el módulo home para conocer al usuario; será reemplazada por la identidad del JWT.                        |

# 18. Anexos

## Anexo A. Configuración y comandos

**Variables de entorno.** El backend lee sus secretos desde un archivo .env (importado por spring.config.import) cuya estructura se documenta en .env.example; el front web usa .env.local con la variable VITE_API_BASE_URL. Por razones de seguridad no se publican valores reales:

| **Variable**                                                 | **Descripción**                                                                                      |
|--------------------------------------------------------------|------------------------------------------------------------------------------------------------------|
| MYSQL_PASSWORD / MYSQL_ROOT_PASSWORD                         | Contraseñas del usuario de aplicación y del usuario root de MySQL (definidas en docker-compose.yml). |
| SPRING_DATASOURCE_PASSWORD                                   | Contraseña usada por las pruebas con el perfil test.                                                 |
| MAIL_HOST / MAIL_PORT                                        | Servidor SMTP y puerto (por defecto smtp.gmail.com:587).                                             |
| MAIL_USERNAME / MAIL_PASSWORD / MAIL_FROM                    | Cuenta remitente y contraseña de aplicación para enviar correos.                                     |
| SECURITY_RESET_PEPPER                                        | Pepper del HMAC-SHA256 de los códigos de recuperación de contraseña.                                 |
| SECURITY_CORS_ALLOWED_ORIGINS                                | Orígenes permitidos por CORS (por defecto http://localhost:5173).                                    |
| SECURITY_JWT_PRIVATE_KEY_PATH / SECURITY_JWT_PUBLIC_KEY_PATH | Rutas absolutas al par de claves RSA (private.pem y public.pem) usado por RS256.                     |
| VITE_API_BASE_URL (front web)                                | URL base del backend; si no se define se usa http://localhost:8080.                                  |

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
| npm run start / npm run android         | Arranca la aplicación móvil en Expo (dispositivo o emulador).                      |

**Verificación tras la instalación:** GET /actuator/health responde {"status":"UP"}, la pantalla de inicio de sesión se carga en el navegador, el registro de una cuenta envía el código de verificación y los hogares creados aparecen en el panel de la aplicación.

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
