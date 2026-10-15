**SERVICIO NACIONAL DE APRENDIZAJE – SENA (National Learning Service)**

**CENTER FOR INDUSTRY, BUSINESS AND SERVICES (CIES)**

**HUILA REGIONAL**

Training Program: Technology in Software Analysis and Development (ADSO)

Group (Ficha): 3145556

**SOFTWARE TECHNICAL MANUAL**

**Project: Electrical Consumption Monitor**

*System technical document: architecture, technologies, code structure, database, modules, API and security*

| **Item**             | **Information**                                                                 |
|----------------------|---------------------------------------------------------------------------------|
| Team members         | Juan Esteban Cortés Parra, Miguel Ángel García Artunduaga                       |
| Instructor           | `[TO BE CONFIRMED]`                                                             |
| Training center      | Center for Industry, Business and Services (CIES) – SENA Huila Regional         |
| Program              | Technology in Software Analysis and Development (ADSO)                          |
| Group (Ficha)        | 3145556                                                                         |
| Document type        | Software technical manual                                                       |
| Version              | 1.1                                                                             |
| Date prepared        | October 7, 2026 (update: October 13, 2026)                                      |

Year 2026

# 2. Document version control

The change history of this manual is recorded in the following table. Every relevant change to the software must be reflected in a new version of the document.

| **Version** | **Date**   | **Description of change**                                                                                                                                                                                                       | **Responsible**      |
|-------------|------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------|
| 1.0         | 10/07/2026 | Initial creation of the technical manual: general description, technologies, architecture, folder structure, database, modules, API and security of the current software version.                                                | Development team     |
| 1.0         | 10/07/2026 | Incorporation of the officially approved diagrams: ERD, class diagram, architecture and authentication sequence, plus the data dictionary.                                                                                      | Development team     |
| 1.1         | 10/13/2026 | Update to the final state: seven active modules, 29 tables, devices and MQTT broker, complete API, identity taken from the JWT, device key hashing, mobile application integrated with the API and current limitations.        | Development team     |

# 3. Table of contents

- [2. Document version control](#2-document-version-control)
- [3. Table of contents](#3-table-of-contents)
- [4. Introduction](#4-introduction)
- [5. Objective](#5-objective)
- [6. Technical scope](#6-technical-scope)
- [7. General system description](#7-general-system-description)
  - [7.1 What is the system?](#71-what-is-the-system)
  - [7.2 Main components](#72-main-components)
  - [7.3 System roles](#73-system-roles)
  - [7.4 Implementation status by domain](#74-implementation-status-by-domain)
- [8. Technologies used](#8-technologies-used)
  - [8.1 Main backend dependencies (pom.xml)](#81-main-backend-dependencies-pomxml)
  - [8.2 Available scripts](#82-available-scripts)
- [9. System architecture](#9-system-architecture)
  - [9.1 General architecture](#91-general-architecture)
  - [9.2 Architectural style and design patterns](#92-architectural-style-and-design-patterns)
  - [9.3 Communication between components](#93-communication-between-components)
- [10. Project structure](#10-project-structure)
  - [10.1 Backend structure](#101-backend-structure)
  - [10.2 Web application structure](#102-web-application-structure)
  - [10.3 Mobile application structure](#103-mobile-application-structure)
  - [10.4 Function of the main folders](#104-function-of-the-main-folders)
- [11. Database](#11-database)
  - [11.1 Entity-relationship diagram](#111-entity-relationship-diagram)
  - [11.2 Main tables](#112-main-tables)
  - [11.3 Data dictionary](#113-data-dictionary)
- [12. Technical modules](#12-technical-modules)
  - [12.1 Overview of the modules](#121-overview-of-the-modules)
  - [12.2 Domain class diagram](#122-domain-class-diagram)
  - [12.3 Backend modules by context](#123-backend-modules-by-context)
  - [12.4 Web front-end module](#124-web-front-end-module)
  - [12.5 Mobile front-end module](#125-mobile-front-end-module)
- [13. API or system routes](#13-api-or-system-routes)
  - [13.1 Common response codes](#131-common-response-codes)
  - [13.2 Request and response example](#132-request-and-response-example)
- [14. Security](#14-security)
  - [14.1 Implemented controls](#141-implemented-controls)
  - [14.2 Known limitations and risks](#142-known-limitations-and-risks)
  - [14.3 Best practices for developers](#143-best-practices-for-developers)
- [15. Software version control](#15-software-version-control)
- [16. Maintenance recommendations](#16-maintenance-recommendations)
  - [16.1 Code updates](#161-code-updates)
  - [16.2 Database backups](#162-database-backups)
  - [16.3 Support and incident resolution](#163-support-and-incident-resolution)
  - [16.4 Declared pending work](#164-declared-pending-work)
- [17. Glossary](#17-glossary)
- [18. Appendices](#18-appendices)
  - [Appendix A. Configuration and commands](#appendix-a-configuration-and-commands)
- [Appendix B. ERD by context](#appendix-b-erd-by-context)
- [Appendix C. Class diagram by domain](#appendix-c-class-diagram-by-domain)

# 4. Introduction

This document is the technical manual of the "Electrical Consumption Monitor" software, a project developed by trainees of the Software Analysis and Development program, group (ficha) 3145556, of the Center for Industry, Business and Services (CIES) – SENA Huila Regional.

The system monitors the consumption of electrical energy in households: a measuring device (ESP32 with a PZEM-004T V3 sensor) sends its readings over MQTT, the backend stores them, computes statistics, generates alerts and saving recommendations and sends notifications by email and through the browser, and the web and mobile applications let users consult the information. The solution is made up of a REST API, an MQTT broker, a measuring device, a web application, an Android mobile application and a MySQL database.

This manual is aimed at developers, technical support staff, the evaluating instructor and, in general, anyone who needs to understand, extend or maintain the software. Its purpose is to describe how the system is built, not how it is used: the user manual covers day-to-day use, the installation manual covers start-up, and the administrator manual covers account and backup management.

The information recorded here was obtained directly from the source code of the project's repositories (backend, web, mobile and devices), from the Liquibase changesets and from the documents of this final documentation, so that it corresponds to the final version of the delivered software.

| **Information source**            | **Location**                                                                                                                                      |
|-----------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------|
| Backend source code               | energy-monitor-backend (Java 21 + Spring Boot 4.1.1)                                                                                              |
| Web application source code       | Energy-Monitor-application (React 19 + Vite 8)                                                                                                   |
| Mobile application source code    | Energy-Monitor-Mobile (Expo SDK 57 + React Native 0.86)                                                                                           |
| Firmware and MQTT broker          | energy-monitor-devices (ESP32 + PZEM-004T V3, firmware 1.3.8; Mosquitto 2)                                                                        |
| Requirements specification        | [`02-especificacion-requisitos`](../../02-especificacion-requisitos/EN/requirements-specification.md) (RF-xxx and RNF-xxx codes)                  |
| Software design                   | [`05-diseno-software`](../../05-diseno-software/EN/README.md): architectural model (05.1), data model (05.2), class model (05.3), patterns (05.4) and interfaces (05.5) |
| Data model                        | Liquibase changesets in `src/main/resources/db/changelog` and [`05.2-data-model.md`](../../05-diseno-software/EN/05.2-data-model.md)             |

# 5. Objective

> To document the architecture, technologies, project structure, database, technical modules, API, security and maintenance recommendations of the "Electrical Consumption Monitor" system, so that a developer outside the team can understand the software, continue its development and support it without prior knowledge of the project.

- Describe the architecture of the modular monolith, the communication with the devices and the design patterns used in each component.

- Detail the exact technologies and versions used in the backend, in the two front-ends and in the device.

- Explain the organization of the source code and the function of each folder.

- Document the data model: entity-relationship diagram, main tables and data dictionary.

- Relate each technical module to its files, tables and endpoints.

- Describe the services exposed by the REST API and the security mechanisms applied.

- Establish recommendations for updating, backing up and supporting the system.

# 6. Technical scope

The technical manual covers the following aspects of the system:

| **Covers**                                                                              | **Does not cover**                                                                                                   |
|-----------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------|
| General architecture, layers and design patterns of the backend and the front-ends.     | Installation, start-up and production deployment procedures (covered by the installation manual).                    |
| Technologies, libraries and versions used, as well as their dependencies.               | Usage steps, screens and messages for the end user (covered by the user manual).                                     |
| Folder and file structure of the project's repositories.                                | Management of users, roles, administrative reports and backups (covered by the administrator manual).                |
| Database: ERD, tables, relationships and data dictionary.                               | Electrical design and wiring of the measuring device (described in the installation manual).                         |
| Technical modules of the backend, web front and mobile front, with their files and tables. | User stories, use cases and acceptance criteria (analysis documents).                                             |
| REST API: method, route, parameters, authentication and responses, and the MQTT contract. | Load testing.                                                                                                      |
| Security: authentication, authorization, encryption, validations and known limitations. | Costs, project plan and management reports.                                                                         |
| Version control and maintenance recommendations.                                        |                                                                                                                      |

**Documented version:** software available in the repositories as of October 13, 2026. Work that remains pending is described in sections 14.2 and 16.4.

# 7. General system description

## 7.1 What is the system?

"Electrical Consumption Monitor" is a web and mobile solution that lets users know how much energy their households consume, identify the devices that use the most energy, receive alerts when consumption exceeds the established limits and get recommendations to reduce spending and the electricity bill. Readings are taken by a measuring device attached to each appliance.

## 7.2 Main components

| **Component**        | **Type**                                  | **Function**                                                                                                                                                                                  |
|----------------------|-------------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| REST API             | Server (Java 21 + Spring Boot)            | Exposes the authentication, household, device, measurement, alert, recommendation and notification services. It is the only source of truth for the data.                                     |
| Web application      | SPA (React 19 + Vite 8)                   | Query and management interface: sign-in, household panel, consumption, history, users, devices (Bluetooth linking), thresholds, alerts, recommendations and settings.                         |
| Mobile application   | Android app (Expo SDK 57)                 | Mobile interface connected to the REST API: account, households, alerts, recommendations and notification preferences. Android only.                                                          |
| MQTT broker          | Eclipse Mosquitto 2 + mosquitto-go-auth   | Receives the telemetry and status of the devices over MQTT 3.1.1 with TLS and authenticates each client against the backend.                                                                  |
| Device               | ESP32 + PZEM-004T V3 sensor (firmware 1.3.8) | Measures voltage, current, active power and energy; publishes one reading every 60 seconds and its status every five minutes.                                                             |
| Database             | MySQL 8.4                                 | Stores users, households, devices, measurements, alerts, recommendations and notifications (29 tables).                                                                                       |
| Email                | SMTP (Mailpit locally)                    | Sends verification and recovery codes and alert and recommendation emails.                                                                                                                    |

## 7.3 System roles

| **Role**                     | **Description**                                                                                  | **How it is implemented**                                                                                                            |
|------------------------------|--------------------------------------------------------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------|
| Household owner (OWNER)      | Creates and manages a household: invites members, configures thresholds, manages devices, transfers ownership or deletes the household. | OWNER enum of the user_home table, with domain permissions in the home module (per-role strategy). Each household has exactly one OWNER. |
| Household member (MEMBER)    | Views the consumption, history, alerts and configuration of the household they were invited to.  | MEMBER enum of the user_home table. A user can be OWNER in one household and MEMBER in another.                                       |
| System user                  | Personal account with profile, language, theme and notification preferences.                     | user table with status ACTIVE / INACTIVE / BLOCKED.                                                                                  |
| Access control (RBAC)        | Global functional permissions grouped into system roles (codes such as METER.READ, ALERT.READ).  | system_role, permission, user_system_role and system_role_permission tables; checked on every request with CheckPermission (they do not travel in the JWT). |
| Administrator                | Global management of the system and accounts.                                                    | Partial: the role assignment ports exist in the security module; the administrative interface is not yet exposed over HTTP.         |

## 7.4 Implementation status by domain

The system is documented as it is built. The table relates each group of requirements of the [requirements specification](../../02-especificacion-requisitos/EN/requirements-specification.md) to the module that implements it.

| **Requirements group**                            | **Software module**                                | **Status**                                                                                                                                                                                       |
|---------------------------------------------------|----------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| RF-SEC – User and Profile Management              | security (backend) + auth and account (web and mobile) | Implemented: registration, email verification, sign-in (including Google), token renewal and sign-out, password recovery and change, profile, permissions and account deletion.            |
| RF-HOM – Household Management                     | home (backend) + dashboard and DetailHome (web)    | Implemented: create household, access code, members, favorites, thresholds, household deletion and ownership transfer. Pending: device history by linking period (RF-HOM-11).                  |
| RF-DEV – Device Management                        | device (backend) + devices (web)                   | Implemented: appliance catalog, Bluetooth linking, editing, new Wi-Fi credentials, unlinking and MQTT authorization.                                                                            |
| RF-MED – Measurements and Consumption             | measurement (backend) + Consumption (web)          | Implemented: MQTT ingestion of telemetry and status, statistics, household consumption summary and history.                                                                                     |
| RF-ALE – Alerts                                   | alert (backend) + Notifications (web and mobile)   | Implemented: device linked, disconnection, high or critical consumption and daily or monthly limit alerts.                                                                                      |
| RF-REC – Saving Recommendations                   | recommendation (backend)                           | Implemented: five saving rules evaluated every hour.                                                                                                                                             |
| RF-NOT – Notifications                            | notification (backend)                             | Implemented: delivery by email and Web Push according to each user's preferences.                                                                                                               |
| RF-CLI – Web and mobile applications              | Web application and mobile application             | Web implemented. Mobile integrated with the API; deleting and transferring a household only exist on the web.                                                                                   |

# 8. Technologies used

The following table records the languages, frameworks, libraries, database tools, design tools and version control tools used in the project, with their version or usage detail.

| **Component**                  | **Technology**                           | **Version / detail**                                                                               |
|--------------------------------|------------------------------------------|----------------------------------------------------------------------------------------------------|
| Backend language               | Java                                     | Java 21 (LTS)                                                                                      |
| Backend framework              | Spring Boot                              | Spring Boot 4.1.1 (Spring Framework 7)                                                             |
| Architectural style            | Spring Modulith + hexagonal architecture | spring-modulith 2.1.1, 7 modules (bounded contexts)                                                |
| Backend build                  | Maven Wrapper                            | mvnw / mvnw.cmd, spring-boot-maven-plugin                                                          |
| Persistence                    | Spring Data JPA + Hibernate              | ddl-auto = validate (the schema is defined by Liquibase)                                           |
| Database migrations            | Liquibase                                | db/changelog with changesets per module and rollback                                               |
| Database engine                | MySQL                                    | MySQL 8.4, collation utf8mb4_unicode_ci, schema energy_monitor                                     |
| Authentication and authorization | Spring Security + OAuth2 Resource Server | JWT RS256 with RSA key pair (PEM), BCrypt for passwords                                          |
| Data validation                | Jakarta Bean Validation                  | @NotBlank, @Size, @NotNull, @Positive, @Pattern                                                    |
| API documentation              | springdoc-openapi                        | springdoc 3.1.1, Swagger UI and /v3/api-docs                                                       |
| Email and push                 | spring-boot-starter-mail + Web Push      | SMTP with STARTTLS (Mailpit locally); push notifications with VAPID keys                           |
| Messaging with devices         | MQTT 3.1.1 with TLS                      | Eclipse Mosquitto 2 + mosquitto-go-auth; the backend subscribes as an MQTT client                  |
| Monitoring and health          | Spring Boot Actuator                     | /actuator/health and /actuator/info                                                                |
| Backend tests                  | JUnit 5 + Spring Boot Test               | 131 test files in src/test/java (more than 1,000 cases)                                            |
| Web front-end                  | React + Vite                             | React 19.2, Vite 8.0, JavaScript (ESM), no TypeScript                                              |
| Web routing                    | react-router-dom                         | react-router-dom 7.17 with protected routes and nested layouts                                     |
| API consumption (web)          | axios                                    | axios 1.17 with request and response interceptors                                                  |
| Forms and validation (web)     | react-hook-form + zod                    | react-hook-form 7.78, zod 4.4, @hookform/resolvers                                                 |
| Charts (web)                   | recharts                                 | recharts 3.8 (AreaChart, PieChart, BarChart)                                                       |
| Internationalization (web)     | i18next + react-i18next                  | 4 languages: es, en, fr, pt                                                                        |
| Styles (web)                   | CSS Modules + CSS variables              | own design system in src/design, light/dark themes                                                 |
| Tests (web)                    | Vitest + Testing Library                 | vitest 5.0, jsdom, 14 test files                                                                   |
| Mobile app                     | Expo + React Native                      | Expo SDK 57, React Native 0.86.3, TypeScript 6 (strict), Android only                              |
| Mobile routing                 | Expo Router                              | file-based routes in src/app with typed routes                                                     |
| Charts (mobile)                | react-native-chart-kit                   | LineChart, BarChart and PieChart on react-native-svg                                               |
| Styles (mobile)                | StyleSheet                               | own design system in src/constants (theme, colors, spacing)                                        |
| Device                         | ESP32 + PZEM-004T V3                     | firmware 1.3.8 (Arduino), Modbus RTU at 9600 baud, MQTT 3.1.1 with TLS on port 8883                |
| Version control                | Git and GitHub                           | code and documentation repositories                                                                |
| Design and diagrams            | Draw.io and PlantUML                     | use cases, activities, ERD, class diagram and architecture                                         |

> *Note on global state: `zustand` (web) and `@reduxjs/toolkit` with `react-redux` (mobile) appear in the package.json files, but the source code does not import them. Global state is handled with React Context and `useSyncExternalStore`.*

## 8.1 Main backend dependencies (pom.xml)

The backend's pom.xml declares as core dependencies spring-boot-starter-webmvc, spring-boot-starter-data-jpa, spring-boot-starter-security, spring-boot-starter-oauth2-resource-server, spring-boot-starter-validation, spring-boot-starter-liquibase, spring-boot-starter-mail, spring-boot-starter-actuator, spring-modulith-starter-core (with its observability and actuator modules), springdoc-openapi-starter-webmvc-ui 3.1.1, mysql-connector-j (runtime) and the set of test starters (\*-test and spring-modulith-starter-test).

## 8.2 Available scripts

| **Repository** | **Script**                      | **Function**                                                                       |
|----------------|---------------------------------|------------------------------------------------------------------------------------|
| backend        | mvnw spring-boot:run            | Builds and starts the API at http://localhost:8080.                                |
| backend        | mvnw test                       | Runs the test suite (requires MySQL running).                                      |
| backend        | mvnw package                    | Generates the executable JAR in /target.                                           |
| backend        | docker compose up -d            | Starts the MySQL 8.4 container with the energy_monitor schema.                     |
| web            | npm run dev                     | Starts the Vite development server at http://localhost:5173.                       |
| web            | npm run build / npm run preview | Builds the application in /dist and serves it for review.                          |
| web            | npm test                        | Runs the tests with Vitest.                                                        |
| web            | npm run lint                    | Analyzes the code with ESLint.                                                     |
| mobile         | npm run start                   | Starts the Expo Metro Bundler.                                                     |
| mobile         | npm run android                 | Runs the application on an Android emulator or device.                             |
| mobile         | npm run lint                    | Analyzes the code with ESLint (there are no automated tests).                      |
| devices        | docker compose up -d            | Starts the development Mosquitto broker (devices/docker folder).                   |
| devices        | arduino-cli compile / upload    | Builds and uploads the firmware over USB (no over-the-air, OTA, update).           |

# 9. System architecture

## 9.1 General architecture

The system adopts a layered client-server architecture with an additional channel for the devices. The presentation layer is made up of the web application and the mobile application; the logic layer is the REST API built in Spring Boot; the data layer is MySQL, accessed through JPA/Hibernate and managed with Liquibase; and the measuring devices publish their readings over MQTT with TLS to a Mosquitto broker that queries the backend to authenticate them.

> **Images and diagrams (Figures 1 to 11):** all the figures of this document are in the following Google Drive folder.  
> https://drive.google.com/drive/folders/1tIzih2FKP8xte4vKYEtrWThFmZxR1oBX?usp=sharing
>
> **Figures possibly out of date (`[TO BE CONFIRMED]` after reviewing them in the folder):** Figures 3 and 5 to 8 (ERD) were drawn before the latest tables and columns and do not include `notification_preference`, `push_subscription`, `user.google_subject`, `home_thresholds.limit_period` or `device.api_key_hash`; Figures 4 and 9 to 11 (class diagram) may still show the per-role strategy classes and the device and recommendation modules as pending; and Figures 1 and 2 (architecture and authentication sequence) may show the X-User-Id header or a system without devices or broker.

**Figure 1.** *General system architecture (three layers: presentation, logic and data).*

| **Layer**        | **Technology**                                        | **Responsibility**                                                                                                   |
|------------------|-------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------|
| Presentation     | React 19 (web) and Expo SDK 57 (mobile, Android)      | User interface, form validation, session handling and consumption of the REST API.                                  |
| Logic            | Spring Boot 4.1.1 + Spring Security + Spring Modulith | Authentication, authorization, use cases, business rule validation, MQTT ingestion and event publishing.            |
| Data             | MySQL 8.4 + Spring Data JPA + Liquibase               | Persistence of the 29 tables, versioned schema and optimized queries.                                                |
| Devices          | ESP32 + PZEM-004T V3 + Mosquitto 2                    | Measurement, MQTT publishing with TLS (port 8883) and authentication of each device against the backend.            |
| Cross-cutting    | Docker Compose, Mailpit and Actuator                  | Database and broker containers, test email and service health check endpoint.                                        |

## 9.2 Architectural style and design patterns

- **Modular monolith (Spring Modulith).** The backend is a single application organized into seven modules or bounded contexts (security, home, device, measurement, alert, recommendation and notification). Each module declares its allowed dependencies in the package-info.java file and can only communicate with the others through the types of its api package; this restriction is verified automatically by the ModularityTests test.

- **Hexagonal architecture per module.** Inside each module live the api, domain, application (inbound ports `port/in`, outbound ports `port/out`, use cases `usecase`), adapter (inbound `adapter/in/web`, `adapter/in/event`, `adapter/in/scheduler` and outbound `adapter/out/persistence`, `out/event`, `out/client`) and infrastructure layers with the Spring configuration.

- **In-process events (no Kafka).** Modules communicate through in-memory Spring events: `MeasurementRecorded` (measurement → alert), `AlertRaised` (alert → notification), `RecommendationCreated` (recommendation → notification), `AccountDeleted` (security → home), `HomeDeleted` (home → device) and `HomeOwnershipTransferred` (home, no consumer yet). A device's loss of connectivity is detected through `device_status_log` (last will message or 150 seconds of silence) and reaches alert as `DeviceConnectivityLost`.

- **Recommendations through a scheduler.** The recommendation module does not listen to `MeasurementRecorded`: `RecommendationScheduler` runs the saving rules every hour and publishes `RecommendationCreated` for each new recommendation.

- **Stateless REST API.** No cookies or HTTP sessions are used: each request carries a JWT in the Authorization header. The CORS policy is configured explicitly and is closed by default.

- **Feature-based front-end.** Both the web and the mobile app organize code in folders by functional capability (auth, dashboard, homes, notifications…) with their own components, styles, zod validations and tests. Global state is handled with React Context and the session is persisted in localStorage (web).

## 9.3 Communication between components

The client applications communicate with the API through HTTP requests in JSON format under the /api/v1 prefix. Every authenticated request includes the header Authorization: Bearer \<accessToken\>. The user's identity is obtained exclusively from the `sub` claim of the JWT through `CurrentUserResolver`; the X-User-Id header is ignored. The web front automatically renews the access token on a 401 response through POST /auth/refresh, with refresh token rotation and locking between tabs using the Web Locks API.

| **Communication element**    | **Detail**                                                                                       |
|------------------------------|--------------------------------------------------------------------------------------------------|
| Protocol (clients)           | HTTP/1.1 over HTTPS in production; HTTP in local development.                                    |
| Format                       | JSON (Content-Type: application/json).                                                           |
| API prefix                   | /api/v1 (all backend routes, except the broker's internal ones).                                 |
| Authentication               | Authorization: Bearer \<JWT RS256\> with 15-minute expiration.                                   |
| Identity                     | `sub` claim of the JWT; X-User-Id is ignored.                                                    |
| Allowed origin (CORS)        | Closed by default; origins are defined with SECURITY_CORS_ALLOWED_ORIGINS (e.g. http://localhost:5173), without credentials. Methods: GET, POST, PUT, DELETE and OPTIONS. |
| Error handling               | Response with ApiError structure: timestamp, status, error, message, path and fieldErrors.       |
| Documentation                | OpenAPI and Swagger UI available at /v3/api-docs and /swagger-ui.html.                           |
| Service health               | GET /actuator/health returns {"status":"UP"}.                                                    |
| Devices (MQTT)               | MQTT 3.1.1; port 8883 with TLS (devices) and internal 1883 (backend).                            |

**MQTT contract.**

| **Parameter**             | **Value**                                                                                                                              |
|---------------------------|----------------------------------------------------------------------------------------------------------------------------------------|
| Client ID                 | `esp32-{deviceId}`                                                                                                                     |
| Username / password       | `device_code` / `api_key` (the backend only stores the hash of the key)                                                                 |
| Keep alive and last will  | Keep alive of 15 seconds; the broker publishes the last will about 22 seconds after a power cut                                        |
| QoS                       | 0 for telemetry and status; 1 for the command                                                                                          |
| Cadence                   | Telemetry every 60 seconds; status every five minutes and on connect; offline after 150 seconds of silence                             |
| Telemetry topic           | `energy-monitor/devices/{deviceId}/telemetry`                                                                                          |
| Status topic              | `energy-monitor/devices/{deviceId}/status` (retained)                                                                                  |
| Command topic             | `energy-monitor/devices/{deviceId}/command` (QoS 1, retained): `{"cmd":"unlink"}` when the device is unlinked                          |

The broker authenticates each device against the backend (`/internal/mqtt/auth/user`) with a five-minute cache, and its ACL only lets it publish on the two topics of the device itself (`/internal/mqtt/auth/acl`). The backend takes the `deviceId` from the topic and drops messages whose body declares another device.

The following figure describes the full flow of a sign-in and the subsequent access to a protected resource.

**Figure 2.** *Authentication sequence: sign-in, protected access and token renewal.*

# 10. Project structure

The project is distributed across four code repositories (backend, web front-end, mobile app and devices) and the final documentation. The following table summarizes their content and how to run them.

| **Repository**             | **Type**                           | **Technology**                                          | **Entry point**                                                      | **Execution**                                  |
|----------------------------|------------------------------------|---------------------------------------------------------|----------------------------------------------------------------------|------------------------------------------------|
| energy-monitor-backend     | REST API (backend)                 | Java 21 + Spring Boot 4.1.1 + Maven                     | src/main/java/com/energymonitor/EnergyMonitorBackendApplication.java | .\mvnw.cmd spring-boot:run (port 8080)         |
| Energy-Monitor-application | Web application (front-end)        | React 19 + Vite 8 (JavaScript)                          | src/main.jsx over index.html                                         | npm install / npm run dev (port 5173)          |
| Energy-Monitor-Mobile      | Mobile application (front-end)     | Expo SDK 57 + React Native 0.86                         | src/app/\_layout.tsx (expo-router/entry)                             | npm install / npm run start                    |
| energy-monitor-devices     | Device firmware and broker         | ESP32 + PZEM-004T V3 (Arduino) and Mosquitto 2          | esp32-pzem-monitor/esp32-pzem-monitor.ino and docker/docker-compose.yml | arduino-cli upload / docker compose up -d   |
| energy-monitor-docs-final  | Final documentation                | Markdown (ES and EN)                                    | README.md                                                            | Does not need to be run                        |

## 10.1 Backend structure

```
energy-monitor-backend/
├── pom.xml                       Maven definition and dependencies
├── mvnw / mvnw.cmd               Maven Wrapper (Maven does not need to be installed)
├── Dockerfile                    backend image (Temurin 21, unprivileged user)
├── docker-compose.yml            MySQL 8.4 container (port 3306)
├── .env.example                  environment variable template
├── README.md                     backend instructions
└── src/
    ├── main/
    │   ├── java/com/energymonitor/
    │   │   ├── EnergyMonitorBackendApplication.java   entry point
    │   │   ├── security/         identity, JWT, Google, RBAC and audit
    │   │   ├── home/             households, members, thresholds, deletion and transfer
    │   │   ├── device/           devices, credentials, MQTT authorization and status
    │   │   ├── measurement/      measurements, MQTT ingestion, household consumption
    │   │   ├── alert/            consumption, limit, device and connectivity alerts
    │   │   ├── recommendation/   saving rules and recommendation inbox
    │   │   └── notification/     email, Web Push and preferences
    │   └── resources/
    │       ├── application.yaml                  base configuration
    │       ├── application-local.yaml(.example)  local profile and keys
    │       └── db/changelog/                     Liquibase master + changesets
    │           ├── db.changelog-master.xml
    │           ├── security/ home/ device/ measurement/
    │           └── alert/ recommendation/ notification/
    └── test/java/com/energymonitor/              131 test files
```

**Internal organization of each module (example with home):**

```
home/
├── package-info.java        declares the module boundaries (@ApplicationModule)
├── api/                     public contract (HomeApi, HomeDeleted, HomeOwnershipTransferred…)
├── domain/model/            pure domain entities (Home, UserHome, OwnerPermissions…)
├── application/
│   ├── port/in/             inbound ports (CreateHome, JoinHome, DeleteHome…)
│   ├── port/out/            outbound ports (persistence, generators)
│   ├── usecase/             use case implementation
│   ├── command/ result/     input and output structures
│   └── exception/           application exceptions
├── adapter/
│   ├── in/web/              REST controllers, DTOs and error handlers
│   ├── in/event/            event listeners
│   └── out/persistence/     JPA entities, repositories, mappers and adapters
└── infrastructure/          Spring configuration and the module's internal API
```

## 10.2 Web application structure

```
Energy-Monitor-application/
├── index.html · package.json · vite.config.js
├── vitest.config.js · eslint.config.js · .env.example
├── public/ (icons and resources)
└── src/
    ├── main.jsx          mounts the providers and App
    ├── App.jsx · index.css
    ├── routes/           AppRoutes, ProtectedRoute, HomeRoute
    ├── context/          HomeContext, ThemeContext, NotificationCenterContext
    ├── services/
    │   ├── http/         httpClient (axios), interceptors and errors
    │   ├── auth/         authApi, googleSignIn, session (localStorage)
    │   ├── home/         homeApi (households, members, thresholds, consumption)
    │   ├── devices/      deviceApi and bleProvisioning (Web Bluetooth)
    │   ├── alerts/       alertApi
    │   ├── recommendations/  recommendationApi
    │   └── notifications/    notificationApi and webPush
    ├── components/       layout/ (Navbar, MainLayout, Sidebar) and shared/
    ├── design/           design system (Button, Input, Card, Badge…)
    ├── features/         landingPage, auth, dashboard, DetailHome,
    │                     Favorites, Notifications, Account, SettingsPage
    ├── i18n/             locales es, en, fr and pt
    └── test/setup.js    Vitest configuration
```

## 10.3 Mobile application structure

```
Energy-Monitor-Mobile/
├── app.json · package.json · tsconfig.json · AGENTS.md · .env.example
├── assets/               icons and splash screen
├── scripts/reset-project.js
└── src/
    ├── app/              Expo Router routes:
    │                     _layout.tsx (providers and Stack), index.tsx (login),
    │                     register.tsx, verify-account.tsx, recover-password.tsx,
    │                     verify-recover-password.tsx, new-password.tsx,
    │                     dashboard.tsx, homes.tsx, home/[id].tsx, favorites.tsx,
    │                     notifications.tsx, settings.tsx, account.tsx
    ├── components/       Button, Card, Input, Modal, Chart, OtpInput… + layout/
    ├── config/           env.ts (EXPO_PUBLIC_API_BASE_URL)
    ├── constants/        theme, colors, spacing, typography
    ├── context/          HomeContext, UserContext, ThemeContext, NotificationCenterContext
    ├── services/         http, auth, home, devices, measurement, alerts,
    │                     recommendations and notifications (API client)
    ├── features/         auth, dashboard, homes, favorites,
    │                     notifications, settings, account
    ├── i18n/             locales es, en, fr and pt
    └── validation/       i18n messages of the zod schemas
```

## 10.4 Function of the main folders

| **Folder**                                                                 | **Function**                                                                             |
|----------------------------------------------------------------------------|------------------------------------------------------------------------------------------|
| backend/src/main/java/com/energymonitor/\<module\>/domain/model            | Pure domain entities, with no Spring dependencies.                                       |
| backend/src/main/java/com/energymonitor/\<module\>/application             | Use cases, inbound and outbound ports, commands, results and exceptions.                 |
| backend/src/main/java/com/energymonitor/\<module\>/adapter/in/web          | REST controllers, DTOs and exception handlers (@RestControllerAdvice).                   |
| backend/src/main/java/com/energymonitor/\<module\>/adapter/out/persistence | JPA entities, repositories, mappers and persistence adapters.                            |
| backend/src/main/resources/db/changelog                                    | Liquibase changesets that build the MySQL schema, with their rollback.                   |
| backend/src/test/java                                                      | Unit, web integration and security tests; module boundary verification.                  |
| web/src/routes                                                             | Definition of routes, guards and layouts of the SPA.                                     |
| web/src/services                                                           | API access layer: HTTP client, interceptors, session and endpoints per domain.           |
| web/src/features                                                           | Screens and components grouped by functionality.                                         |
| web/src/design                                                             | Reusable interface components (buttons, cards, fields) and their styles.                 |
| mobile/src/app                                                             | File-based Expo Router routes that mount each screen.                                    |
| mobile/src/services                                                        | REST API client of the mobile application (same pattern as the web).                     |
| mobile/src/features                                                        | Screens, components, hooks and validations per functionality.                            |
| devices/esp32-pzem-monitor                                                 | Device firmware and configuration files.                                                 |
| devices/docker                                                             | Development Mosquitto broker and production deployment (broker, backend and MySQL).      |

# 11. Database

**Engine:** MySQL 8.4, run in a Docker container (docker-compose.yml) with collation utf8mb4_unicode_ci.

**Schema:** energy_monitor, with 29 tables distributed across seven contexts. The application user is energy_app.

**Schema management:** Liquibase applies the versioned changesets in src/main/resources/db/changelog; Hibernate is configured with ddl-auto = validate, that is, it only checks that the schema matches the entities and never creates tables by itself.

**Conventions:** snake_case names; VARCHAR(10) primary keys; audit columns created_at, updated_at and deleted_at in all tables; soft delete (no physical DELETE by default); indexes with the idx\_\[table\]\_\[columns\] pattern. References between contexts are stored as application-level identifiers and do not generate SQL foreign keys.

## 11.1 Entity-relationship diagram

The following diagram shows the system entities, grouped by context (Security, Home Management, Devices, Monitoring and measurements, Alerts, Recommendations and Notifications), with their attributes, keys and relationships. Dotted relationships between contexts represent application references (no SQL foreign key). The diagram published in the figures folder shows 27 entities: `notification_preference` and `push_subscription` and several later columns are missing (see the note in section 9.1).

> **Reading recommendation:** because of its size, the diagram is presented in landscape; in Word it can be enlarged with the zoom to read the attributes of each entity. Appendix B includes the same model split by context, with better legibility.
>
**Figure 3.** *Complete entity-relationship diagram (ERD) of the "Electrical Consumption Monitor" system.*

## 11.2 Main tables

The following table lists the 29 tables of the schema, the context they belong to, their description, their primary key and the relationships declared in the data dictionary.

| **Table**              | **Context**                 | **Description**                                                                                                                                                                                               | **Primary key**      | **Relationships**                                                                                                                                                      |
|------------------------|-----------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Person                 | Security                    | Records the personal data associated with a user.                                                                                                                                                             | —                    | Person 1:1 User                                                                                                                                                        |
| User                   | Security                    | Manages the user's credentials, status and access data.                                                                                                                                                       | —                    | User 1:1 Person; User 1:1 UserConfiguration; User 1:N PasswordResetToken; User 1:N UserSession; User 1:N UserSystemRole; User 1:N AuditLog                             |
| UserConfiguration      | Security                    | Stores each user's configuration preferences.                                                                                                                                                                 | —                    | UserConfiguration 1:1 User                                                                                                                                             |
| PasswordResetToken     | Security                    | Stores tokens for password recovery.                                                                                                                                                                          | —                    | PasswordResetToken N:1 User                                                                                                                                            |
| RefreshToken           | Security                    | Each row is a refresh token generation. Only the SHA-256 hash of the secret is stored, never the secret in plain text. family_id groups all generations of the same login and parent_id chains the genealogy. | id_refresh_token     | RefreshToken N:1 UserSession; RefreshToken 0..1:1 RefreshToken (parent_id)                                                                                             |
| UserSession            | Security                    | Records the user's active and renewable sessions.                                                                                                                                                             | —                    | UserSession N:1 User; UserSession 1:N RefreshToken                                                                                                                     |
| SecurityConfiguration  | Security                    | Defines configurable security parameters of the system.                                                                                                                                                       | —                    | No relationships recorded                                                                                                                                              |
| PasswordPolicy         | Security                    | Defines the password validation and expiration rules.                                                                                                                                                         | —                    | No relationships recorded                                                                                                                                              |
| LoginErrorLog          | Security                    | Records the errors produced during login.                                                                                                                                                                     | —                    | LoginErrorLog N:1 User                                                                                                                                                 |
| SystemRole             | Security                    | Defines the roles available in the system.                                                                                                                                                                    | —                    | SystemRole 1:N UserSystemRole; SystemRole 1:N SystemRolePermission                                                                                                     |
| UserSystemRole         | Security                    | Relates users to system roles.                                                                                                                                                                                | —                    | UserSystemRole N:1 User; UserSystemRole N:1 SystemRole                                                                                                                 |
| Permission             | Security                    | Defines the functional permissions of the system.                                                                                                                                                             | —                    | Permission 1:N SystemRolePermission                                                                                                                                    |
| SystemRolePermission   | Security                    | Relates roles to permissions.                                                                                                                                                                                 | —                    | SystemRolePermission N:1 SystemRole; SystemRolePermission N:1 Permission                                                                                               |
| AuditLog               | Security                    | Records the relevant actions executed by users.                                                                                                                                                               | —                    | AuditLog N:1 User                                                                                                                                                      |
| HomeType               | Home Management             | Catalog of household types.                                                                                                                                                                                   | —                    | HomeType 1:N Home                                                                                                                                                      |
| Home                   | Home Management             | Records the households managed by the system.                                                                                                                                                                 | —                    | Home N:1 HomeType; Home 1:1 HomeThresholds; Home 1:N UserHome; Home 1:N DeviceHome; Home 1:N Alert; Home 1:N Recommendation                                            |
| HomeThresholds         | Home Management             | Configures a household's consumption limits.                                                                                                                                                                  | —                    | HomeThresholds 1:1 Home                                                                                                                                                |
| UserHome               | Home Management             | Relates users to households and defines their role.                                                                                                                                                           | —                    | UserHome N:1 User; UserHome N:1 Home                                                                                                                                   |
| ApplianceType          | Devices                     | Catalog of appliance types.                                                                                                                                                                                   | —                    | ApplianceType 1:N Device                                                                                                                                               |
| Device                 | Devices                     | Records the installed measurement devices.                                                                                                                                                                    | —                    | Device N:1 ApplianceType; Device 1:N DeviceStatusLog; Device 1:N DeviceHome; Device 1:N Measurement; Device 1:N Alert; Device 1:N Recommendation                       |
| DeviceStatusLog        | Devices                     | Records the status and connectivity of the devices.                                                                                                                                                           | —                    | DeviceStatusLog N:1 Device                                                                                                                                             |
| DeviceHome             | Devices                     | Relates devices to households.                                                                                                                                                                                | —                    | DeviceHome N:1 Device; DeviceHome N:1 Home                                                                                                                             |
| Measurement            | Monitoring and measurements | Stores the recorded electrical measurements.                                                                                                                                                                  | —                    | Measurement N:1 Device                                                                                                                                                 |
| ConsumptionLevel       | Monitoring and measurements | Defines risk levels and their consumption ranges.                                                                                                                                                             | —                    | ConsumptionLevel 1:N Alert                                                                                                                                             |
| Alert                  | Alerts                      | Records threshold and connectivity alerts.                                                                                                                                                                    | —                    | Alert N:1 Home; Alert N:1 Device; Alert N:1 ConsumptionLevel; Alert N:1 Measurement                                                                                    |
| Recommendation         | Recommendations             | Records recommendations for households and devices.                                                                                                                                                           | —                    | Recommendation N:1 Home; Recommendation N:1 Device                                                                                                                     |
| Notification           | Notifications               | Record of the delivery of a message to the user. Alert and Recommendation are events; Notification records whether the message was delivered, through which channel and its read status.                      | —                    | Notification N:1 User (application reference); Notification N:1 Home (application reference); Notification references Alert/Recommendation by (source_type, source_id) |
| NotificationPreference | Notifications               | Notification channel preferences of each user (email and push).                                                                                                                                               | user_id              | NotificationPreference 1:1 User (application reference)                                                                                                                |
| PushSubscription       | Notifications               | Push notification subscriptions of the user's browsers.                                                                                                                                                       | id_push_subscription | PushSubscription N:1 User (application reference)                                                                                                                      |

## 11.3 Data dictionary

The data dictionary describes each table of the schema field by field: key constraint, data type, size and description. It was rebuilt from the backend's Liquibase changesets, which are the source of truth of the schema; for the data model with its integrity rules see also [`05.2-data-model.md`](../../05-diseno-software/EN/05.2-data-model.md).

**Constraint legend:** PK = primary key; FK = foreign key; UNIQUE = unique value; NOT NULL = required field; ENUM = set of allowed values (listed in the Size column). The audit columns (created_at, updated_at and deleted_at) are repeated in all tables and are not included in each dictionary.

### Person — Security

*Records the personal data associated with a user.*

| **Field** | **Type** | **Size** | **Constraint** | **Description**               |
|-----------|----------|----------|----------------|-------------------------------|
| id_person | VARCHAR  | 10       | PK / NOT NULL  | Person identifier             |
| name      | VARCHAR  | 100      | NOT NULL       | Person's first names          |
| last_name | VARCHAR  | 100      | —              | Person's last name (optional) |

**Relationships:** Person 1:1 User

### User — Security

*Manages the user's credentials, status and access data.*

| **Field**             | **Type**   | **Size**                  | **Constraint**         | **Description**                                                                                                 |
|-----------------------|------------|---------------------------|------------------------|-----------------------------------------------------------------------------------------------------------------|
| id_user               | VARCHAR    | 10                        | PK / NOT NULL          | User identifier                                                                                                 |
| person_id             | VARCHAR    | 10                        | FK / UNIQUE / NOT NULL | Associated person                                                                                               |
| password_hash         | VARCHAR    | 255                       | NOT NULL               | Password hash                                                                                                   |
| email                 | VARCHAR    | 255                       | UNIQUE / NOT NULL      | Email address                                                                                                   |
| email_verified        | BOOLEAN    |                           | NOT NULL               | Indicates whether the email was verified                                                                        |
| registration_date     | TIMESTAMP  |                           | NOT NULL               | Registration date and time                                                                                      |
| status                | ENUM       | ACTIVE, INACTIVE, BLOCKED | NOT NULL               | Account status                                                                                                  |
| failed_login_attempts | INT        |                           | NOT NULL               | Number of failed attempts                                                                                       |
| last_login_at         | TIMESTAMP  |                           | —                      | Last login                                                                                                      |
| profile_image         | MEDIUMTEXT |                           | —                      | Account profile image (base64 data URL)                                                                         |
| google_subject        | VARCHAR    | 255                       | UNIQUE                 | Stable identifier (sub) of the Google account the account was created with; empty if registered with a password |

**Relationships:** User 1:1 Person • User 1:1 UserConfiguration • User 1:N PasswordResetToken • User 1:N UserSession • User 1:N UserSystemRole • User 1:N AuditLog

### UserConfiguration — Security

*Stores each user's configuration preferences.*

| **Field**        | **Type** | **Size** | **Constraint**         | **Description**                |
|------------------|----------|----------|------------------------|--------------------------------|
| id_configuration | VARCHAR  | 10       | PK / NOT NULL          | Configuration identifier       |
| user_id          | VARCHAR  | 10       | FK / UNIQUE / NOT NULL | Configured user                |
| notify_by_email  | BOOLEAN  |          | NOT NULL               | Enables email notifications    |
| notify_by_push   | BOOLEAN  |          | NOT NULL               | Enables push notifications     |
| color_theme      | VARCHAR  | 50       | —                      | Color theme                    |
| language         | VARCHAR  | 50       | —                      | Preferred language             |
| social_provider  | VARCHAR  | 50       | —                      | Social authentication provider |

**Relationships:** UserConfiguration 1:1 User

### PasswordResetToken — Security

*Stores tokens for password recovery.*

| **Field**        | **Type**  | **Size** | **Constraint**    | **Description**                                     |
|------------------|-----------|----------|-------------------|-----------------------------------------------------|
| id_reset_token   | VARCHAR   | 10       | PK / NOT NULL     | Token identifier                                    |
| user_id          | VARCHAR   | 10       | FK / NOT NULL     | Owner user                                          |
| reset_token_hash | VARCHAR   | 64       | UNIQUE / NOT NULL | HMAC-SHA256 hash (with pepper) of the recovery code |
| used             | BOOLEAN   |          | NOT NULL          | Indicates whether the token was used                |
| expiration_at    | TIMESTAMP |          | NOT NULL          | Expiration date                                     |
| attempts         | INT       |          | NOT NULL          | Attempts to redeem the code                         |

**Relationships:** PasswordResetToken N:1 User

### UserSession — Security

*Records the user's active and renewable sessions.*

| **Field**       | **Type**  | **Size**                                              | **Constraint** | **Description**                           |
|-----------------|-----------|-------------------------------------------------------|----------------|-------------------------------------------|
| id_user_session | VARCHAR   | 10                                                    | PK / NOT NULL  | Session identifier                        |
| user_id         | VARCHAR   | 10                                                    | FK / NOT NULL  | Session user                              |
| revoked         | BOOLEAN   |                                                       | NOT NULL       | Indicates whether the session was revoked |
| ip_address      | VARCHAR   | 45                                                    | —              | IP address                                |
| user_agent      | VARCHAR   | 150                                                   | —              | Client agent                              |
| expiration_at   | TIMESTAMP |                                                       | NOT NULL       | Expiration date                           |
| closed_at       | TIMESTAMP |                                                       | —              | Closing date                              |
| revoked_at      | TIMESTAMP |                                                       | —              | Date and time of session revocation       |
| revoked_reason  | ENUM      | REFRESH_TOKEN_REUSE, PASSWORD_CHANGED, ADMINISTRATIVE | —              | Reason for revocation                     |

**Relationships:** UserSession N:1 User • UserSession 1:N RefreshToken

### SecurityConfiguration — Security

*Defines configurable security parameters of the system.*

| **Field**                 | **Type** | **Size** | **Constraint**    | **Description**          |
|---------------------------|----------|----------|-------------------|--------------------------|
| id_security_configuration | VARCHAR  | 10       | PK / NOT NULL     | Configuration identifier |
| config_name               | VARCHAR  | 100      | UNIQUE / NOT NULL | Configuration name       |
| config_value              | VARCHAR  | 100      | NOT NULL          | Configuration value      |
| description               | VARCHAR  | 255      | —                 | Description              |

**Relationships:** UserSession N:1 User • UserSession 1:N RefreshToken

### PasswordPolicy — Security

*Defines the password validation and expiration rules.*

| **Field**          | **Type** | **Size** | **Constraint** | **Description**            |
|--------------------|----------|----------|----------------|----------------------------|
| id_password_policy | VARCHAR  | 10       | PK / NOT NULL  | Policy identifier          |
| min_length         | INT      |          | NOT NULL       | Minimum length             |
| max_length         | INT      |          | NOT NULL       | Maximum length             |
| require_uppercase  | BOOLEAN  |          | NOT NULL       | Requires uppercase letters |
| require_numbers    | BOOLEAN  |          | NOT NULL       | Requires numbers           |
| require_symbols    | BOOLEAN  |          | NOT NULL       | Requires symbols           |
| expiration_days    | INT      |          | NOT NULL       | Days of validity           |

**Relationships:** UserSession N:1 User • UserSession 1:N RefreshToken

### LoginErrorLog — Security

*Records the errors produced during login.*

| **Field**      | **Type** | **Size**                                                            | **Constraint** | **Description**      |
|----------------|----------|---------------------------------------------------------------------|----------------|----------------------|
| id_login_error | VARCHAR  | 10                                                                  | PK / NOT NULL  | Error identifier     |
| user_id        | VARCHAR  | 10                                                                  | FK             | Related user, if any |
| error_type     | ENUM     | INVALID_PASSWORD, USER_NOT_FOUND, ACCOUNT_BLOCKED, ACCOUNT_INACTIVE | NOT NULL       | Error type           |
| description    | VARCHAR  | 500                                                                 | —              | Error description    |
| ip_address     | VARCHAR  | 45                                                                  | —              | IP address           |

**Relationships:** LoginErrorLog N:1 User

### SystemRole — Security

*Defines the roles available in the system.*

| **Field**      | **Type** | **Size** | **Constraint**    | **Description**                 |
|----------------|----------|----------|-------------------|---------------------------------|
| id_system_role | VARCHAR  | 10       | PK / NOT NULL     | Role identifier                 |
| name           | VARCHAR  | 50       | UNIQUE / NOT NULL | Role name                       |
| description    | VARCHAR  | 200      | —                 | Description                     |
| enabled        | BOOLEAN  |          | NOT NULL          | Indicates whether it is enabled |

**Relationships:** SystemRole 1:N UserSystemRole • SystemRole 1:N SystemRolePermission

### Permission — Security

*Defines the functional permissions of the system.*

| **Field**     | **Type** | **Size** | **Constraint**    | **Description**       |
|---------------|----------|----------|-------------------|-----------------------|
| id_permission | VARCHAR  | 10       | PK / NOT NULL     | Permission identifier |
| code          | VARCHAR  | 30       | UNIQUE / NOT NULL | Unique code           |
| name          | VARCHAR  | 50       | NOT NULL          | Permission name       |
| description   | VARCHAR  | 100      | —                 | Description           |

**Relationships:** Permission 1:N SystemRolePermission

### UserSystemRole — Security

*Relates users to system roles.*

| **Field**      | **Type**  | **Size** | **Constraint** | **Description** |
|----------------|-----------|----------|----------------|-----------------|
| user_id        | VARCHAR   | 10       | FK / NOT NULL  | Related user    |
| system_role_id | VARCHAR   | 10       | FK / NOT NULL  | Assigned role   |
| assigned_at    | TIMESTAMP |          | NOT NULL       | Assignment date |

**Relationships:** UserSystemRole N:1 User • UserSystemRole N:1 SystemRole

### SystemRolePermission — Security

*Relates roles to permissions.*

| **Field**      | **Type**  | **Size** | **Constraint** | **Description**     |
|----------------|-----------|----------|----------------|---------------------|
| system_role_id | VARCHAR   | 10       | FK / NOT NULL  | Related role        |
| permission_id  | VARCHAR   | 10       | FK / NOT NULL  | Assigned permission |
| assigned_at    | TIMESTAMP |          | NOT NULL       | Assignment date     |

**Relationships:** SystemRolePermission N:1 SystemRole • SystemRolePermission N:1 Permission

### AuditLog — Security

*Records the relevant actions executed by users.*

| **Field**    | **Type** | **Size**                                            | **Constraint** | **Description**              |
|--------------|----------|-----------------------------------------------------|----------------|------------------------------|
| id_audit_log | VARCHAR  | 10                                                  | PK / NOT NULL  | Record identifier            |
| user_id      | VARCHAR  | 10                                                  | FK             | User who executed the action |
| action       | ENUM     | CREATE, UPDATE, DELETE, LOGIN, LOGIN_FAILED, LOGOUT | NOT NULL       | Audited action               |
| description  | VARCHAR  | 255                                                 | —              | Description                  |
| ip_address   | VARCHAR  | 45                                                  | —              | IP address                   |
| application  | VARCHAR  | 255                                                 | —              | Source application           |

**Relationships:** AuditLog N:1 User

### RefreshToken — Security

*Each row is a refresh token generation. Only the SHA-256 hash of the secret is stored, never the secret in plain text. family_id groups all generations of the same login and parent_id chains the genealogy.*

| **Field**        | **Type**  | **Size**                                              | **Constraint**    | **Description**                                             |
|------------------|-----------|-------------------------------------------------------|-------------------|-------------------------------------------------------------|
| id_refresh_token | VARCHAR   | 10                                                    | PK / NOT NULL     | Token generation identifier                                 |
| id_user_session  | VARCHAR   | 10                                                    | FK / NOT NULL     | Session to which the generation belongs                     |
| family_id        | VARCHAR   | 10                                                    | NOT NULL          | Identifier of the root of the generation family             |
| parent_id        | VARCHAR   | 10                                                    | FK                | Generation that this one replaces                           |
| token_hash       | VARCHAR   | 64                                                    | UNIQUE / NOT NULL | SHA-256 hash of the secret (never the secret in plain text) |
| status           | ENUM      | ACTIVE, ROTATED, REVOKED, EXPIRED                     | NOT NULL          | Lifecycle status of the generation                          |
| expires_at       | TIMESTAMP |                                                       | NOT NULL          | Expiration date                                             |
| rotated_at       | TIMESTAMP |                                                       | —                 | Date on which it was replaced                               |
| revoked_at       | TIMESTAMP |                                                       | —                 | Revocation date                                             |
| revoked_reason   | ENUM      | REFRESH_TOKEN_REUSE, PASSWORD_CHANGED, ADMINISTRATIVE | —                 | Reason for revocation                                       |

**Relationships:** RefreshToken N:1 UserSession • RefreshToken 0..1:1 RefreshToken (parent_id)

### HomeType — Home Management

*Catalog of household types.*

| **Field**    | **Type** | **Size** | **Constraint**    | **Description**     |
|--------------|----------|----------|-------------------|---------------------|
| id_home_type | VARCHAR  | 10       | PK / NOT NULL     | Type identifier     |
| name         | VARCHAR  | 50       | UNIQUE / NOT NULL | Household type name |

**Relationships:** HomeType 1:N Home

### Home — Home Management

*Records the households managed by the system.*

| **Field**     | **Type**  | **Size** | **Constraint**    | **Description** |
|---------------|-----------|----------|-------------------|-----------------|
| id_home       | VARCHAR   | 10       | PK / NOT NULL     | Household code  |
| name          | VARCHAR   | 50       | NOT NULL          | Household name  |
| home_type_id  | VARCHAR   | 10       | FK / NOT NULL     | Household type  |
| address       | VARCHAR   | 200      | NOT NULL          | Address         |
| access_code   | VARCHAR   | 8        | UNIQUE / NOT NULL | Access code     |
| description   | VARCHAR   | 200      | —                 | Description     |
| creation_date | TIMESTAMP |          | NOT NULL          | Creation date   |

**Relationships:** Home N:1 HomeType • Home 1:1 HomeThresholds • Home 1:N UserHome • Home 1:N DeviceHome • Home 1:N Alert • Home 1:N Recommendation

### HomeThresholds — Home Management

*Configures a household's consumption limits.*

| **Field**          | **Type** | **Size**       | **Constraint**         | **Description**                           |
|--------------------|----------|----------------|------------------------|-------------------------------------------|
| id_threshold       | VARCHAR  | 10             | PK / NOT NULL          | Threshold identifier                      |
| home_id            | VARCHAR  | 10             | FK / UNIQUE / NOT NULL | Configured household                      |
| daily_limit        | DOUBLE   |                | NOT NULL               | Daily consumption limit                   |
| monthly_limit      | DOUBLE   |                | NOT NULL               | Monthly consumption limit                 |
| use_system_default | BOOLEAN  |                | NOT NULL               | Uses default limits                       |
| limit_period       | ENUM     | DAILY, MONTHLY | NOT NULL               | Limit set by the owner (DAILY or MONTHLY) |

**Relationships:** HomeThresholds 1:1 Home

### UserHome — Home Management

*Relates users to households and defines their role.*

| **Field** | **Type** | **Size**      | **Constraint** | **Description**                                |
|-----------|----------|---------------|----------------|------------------------------------------------|
| user_id   | VARCHAR  | 10            | NOT NULL       | Related user                                   |
| home_id   | VARCHAR  | 10            | FK / NOT NULL  | Related household                              |
| role      | ENUM     | OWNER, MEMBER | NOT NULL       | User's role in the household                   |
| favorite  | BOOLEAN  |               | NOT NULL       | Indicates whether it is the favorite household |

**Relationships:** UserHome N:1 User • UserHome N:1 Home

### ApplianceType — Devices

*Catalog of appliance types.*

| **Field**         | **Type** | **Size** | **Constraint**    | **Description** |
|-------------------|----------|----------|-------------------|-----------------|
| id_appliance_type | VARCHAR  | 10       | PK / NOT NULL     | Type identifier |
| name              | VARCHAR  | 50       | UNIQUE / NOT NULL | Type name       |

**Relationships:** ApplianceType 1:N Device

### Device — Devices

*Records the installed measurement devices.*

| **Field**         | **Type**  | **Size** | **Constraint**    | **Description**                                                      |
|-------------------|-----------|----------|-------------------|----------------------------------------------------------------------|
| id_device         | VARCHAR   | 10       | PK / NOT NULL     | Device identifier                                                    |
| name              | VARCHAR   | 50       | NOT NULL          | Name                                                                 |
| appliance_type_id | VARCHAR   | 10       | FK / NOT NULL     | Appliance type                                                       |
| location          | VARCHAR   | 50       | —                 | Location                                                             |
| description       | VARCHAR   | 200      | —                 | Description                                                          |
| installation_date | TIMESTAMP |          | NOT NULL          | Installation date                                                    |
| device_code       | VARCHAR   | 6        | UNIQUE / NOT NULL | Device code                                                          |
| api_key_hash      | VARCHAR   | 64       | NOT NULL          | SHA-256 hash of the integration key (the key is not stored in clear) |

**Relationships:** Device N:1 ApplianceType • Device 1:N DeviceStatusLog • Device 1:N DeviceHome • Device 1:N Measurement • Device 1:N Alert • Device 1:N Recommendation

### DeviceStatusLog — Devices

*Records the status and connectivity of the devices.*

| **Field**        | **Type**  | **Size**        | **Constraint** | **Description**      |
|------------------|-----------|-----------------|----------------|----------------------|
| id_device_status | VARCHAR   | 10              | PK / NOT NULL  | Record identifier    |
| device_id        | VARCHAR   | 10              | FK             | Related device       |
| status           | ENUM      | ONLINE, OFFLINE | NOT NULL       | Device status        |
| signal_strength  | INT       |                 | —              | Signal strength      |
| last_seen        | TIMESTAMP |                 | NOT NULL       | Last connection date |

**Relationships:** DeviceStatusLog N:1 Device

### DeviceHome — Devices

*Relates devices to households.*

| **Field** | **Type** | **Size** | **Constraint** | **Description**   |
|-----------|----------|----------|----------------|-------------------|
| device_id | VARCHAR  | 10       | FK / NOT NULL  | Related device    |
| home_id   | VARCHAR  | 10       | NOT NULL       | Related household |

**Relationships:** DeviceHome N:1 Device • DeviceHome N:1 Home

### Measurement — Monitoring and measurements

*Stores the recorded electrical measurements.*

| **Field**      | **Type**  | **Size** | **Constraint** | **Description**                       |
|----------------|-----------|----------|----------------|---------------------------------------|
| id_measurement | VARCHAR   | 10       | PK / NOT NULL  | Measurement identifier                |
| device_id      | VARCHAR   | 10       | NOT NULL       | Device that generates the measurement |
| date_time      | TIMESTAMP |          | NOT NULL       | Date and time of the measurement      |
| voltage        | DOUBLE    |          | NOT NULL       | Measured voltage                      |
| current        | DOUBLE    |          | NOT NULL       | Measured current                      |
| active_power   | DOUBLE    |          | NOT NULL       | Active power                          |
| stored_energy  | DOUBLE    |          | NOT NULL       | Accumulated energy                    |

**Relationships:** Measurement N:1 Device

### ConsumptionLevel — Monitoring and measurements

*Defines risk levels and their consumption ranges.*

| **Field**            | **Type** | **Size**                    | **Constraint**    | **Description**  |
|----------------------|----------|-----------------------------|-------------------|------------------|
| id_consumption_level | VARCHAR  | 10                          | PK / NOT NULL     | Level identifier |
| name                 | ENUM     | LOW, MEDIUM, HIGH, CRITICAL | UNIQUE / NOT NULL | Risk level       |
| description          | VARCHAR  | 200                         | NOT NULL          | Description      |
| min_limit            | DOUBLE   |                             | NOT NULL          | Lower limit      |
| max_limit            | DOUBLE   |                             | NOT NULL          | Upper limit      |

**Relationships:** ConsumptionLevel 1:N Alert

### Alert — Alerts

*Records threshold and connectivity alerts.*

| **Field**            | **Type**  | **Size**                               | **Constraint** | **Description**              |
|----------------------|-----------|----------------------------------------|----------------|------------------------------|
| id_alert             | VARCHAR   | 10                                     | PK / NOT NULL  | Alert identifier             |
| home_id              | VARCHAR   | 10                                     | NOT NULL       | Household it applies to      |
| device_id            | VARCHAR   | 10                                     | —              | Related device               |
| type                 | ENUM      | THRESHOLD, CONNECTIVITY, DEVICE, LIMIT | NOT NULL       | Alert type                   |
| message_key          | VARCHAR   | 100                                    | NOT NULL       | Message key                  |
| date_time            | TIMESTAMP |                                        | NOT NULL       | Date and time                |
| alert_status         | ENUM      | PENDING, RESOLVED                      | NOT NULL       | Alert status                 |
| consumption_level_id | VARCHAR   | 10                                     | —              | Associated consumption level |
| measurement_id       | VARCHAR   | 10                                     | —              | Associated measurement       |

**Relationships:** Alert N:1 Home • Alert N:1 Device • Alert N:1 ConsumptionLevel • Alert N:1 Measurement

### Recommendation — Recommendations

*Records recommendations for households and devices.*

| **Field**         | **Type**  | **Size**                                                               | **Constraint** | **Description**           |
|-------------------|-----------|------------------------------------------------------------------------|----------------|---------------------------|
| id_recommendation | VARCHAR   | 10                                                                     | PK / NOT NULL  | Recommendation identifier |
| home_id           | VARCHAR   | 10                                                                     | NOT NULL       | Household it applies to   |
| device_id         | VARCHAR   | 10                                                                     | —              | Related device            |
| type              | ENUM      | SAVING, PEAK_HOURS, HIGH_CONSUMPTION, HISTORICAL_COMPARISON, THRESHOLD | NOT NULL       | Recommendation type       |
| message_key       | VARCHAR   | 100                                                                    | NOT NULL       | Message key               |
| date_time         | TIMESTAMP |                                                                        | NOT NULL       | Date and time             |
| status            | ENUM      | READ, UNREAD                                                           | NOT NULL       | Read status               |

**Relationships:** Recommendation N:1 Home • Recommendation N:1 Device

### Notification — Notifications

*Record of the delivery of a message to the user. Alert and Recommendation are events; Notification records whether the message was delivered, through which channel and its read status.*

| **Field**       | **Type**  | **Size**                        | **Constraint** | **Description**             |
|-----------------|-----------|---------------------------------|----------------|-----------------------------|
| id_notification | VARCHAR   | 10                              | PK / NOT NULL  | Notification identifier     |
| user_id         | VARCHAR   | 10                              | NOT NULL       | Recipient user              |
| home_id         | VARCHAR   | 10                              | —              | Related home                |
| channel         | ENUM      | EMAIL, PUSH                     | NOT NULL       | Delivery channel            |
| source_type     | ENUM      | ALERT, RECOMMENDATION, SECURITY | NOT NULL       | Origin of the message       |
| source_id       | VARCHAR   | 10                              | —              | Origin identifier           |
| message_key     | VARCHAR   | 100                             | NOT NULL       | Message key                 |
| delivery_status | ENUM      | PENDING, SENT, FAILED           | NOT NULL       | Delivery status             |
| sent_at         | TIMESTAMP |                                 | —              | Send date                   |
| read_at         | TIMESTAMP |                                 | —              | Read date                   |
| failure_reason  | VARCHAR   | 200                             | —              | Reason for delivery failure |

**Relationships:** Notification N:1 User (application reference) • Notification N:1 Home (application reference) • Notification references Alert/Recommendation by (source_type, source_id)

### NotificationPreference — Notifications

*Notification channel preferences of each user (email and push).*

| **Field**     | **Type** | **Size** | **Constraint** | **Description**                                 |
|---------------|----------|----------|----------------|-------------------------------------------------|
| user_id       | VARCHAR  | 10       | PK / NOT NULL  | User who owns the preferences (primary key)     |
| email_enabled | BOOLEAN  |          | NOT NULL       | Delivery of alerts and recommendations by email |
| push_enabled  | BOOLEAN  |          | NOT NULL       | Delivery through browser notification           |

**Relationships:** NotificationPreference 1:1 User (application reference)

### PushSubscription — Notifications

*Push notification subscriptions of the user's browsers.*

| **Field**            | **Type** | **Size** | **Constraint**    | **Description**                              |
|----------------------|----------|----------|-------------------|----------------------------------------------|
| id_push_subscription | VARCHAR  | 10       | PK / NOT NULL     | Subscription identifier                      |
| user_id              | VARCHAR  | 10       | NOT NULL          | Subscribed user                              |
| endpoint             | VARCHAR  | 500      | UNIQUE / NOT NULL | Address of the browser push service (unique) |
| p256dh               | VARCHAR  | 100      | NOT NULL          | Public encryption key of the subscription    |
| auth                 | VARCHAR  | 50       | NOT NULL          | Authentication secret of the subscription    |

**Relationships:** PushSubscription N:1 User (application reference)


# 12. Technical modules

## 12.1 Overview of the modules

The backend is divided into seven modules (bounded contexts), all of them active.

| **Module**     | **Responsibility**                                                                     | **Controllers / use cases**                                                                                               | **Status** |
|----------------|----------------------------------------------------------------------------------------|---------------------------------------------------------------------------------------------------------------------------|------------|
| security       | Registration, authentication (including Google), sessions, passwords, profile, RBAC, audit and account deletion | AuthController + 18 use cases                                                                              | Active     |
| home           | Households, members, favorites, access code, thresholds, household deletion and ownership transfer | HomeController, HomeThresholdsController, HomeTypeController + 15 use cases                                 | Active     |
| device         | Devices, linking, credentials, connection status and broker authorization              | HomeDeviceController, ApplianceTypeController, BrokerAuthController + 3 services                                          | Active     |
| measurement    | MQTT ingestion of telemetry and status, history, statistics and household consumption  | MeasurementController, ConsumptionLevelController, HomeConsumptionController + MQTT consumers                             | Active     |
| alert          | Consumption, limit, device linked and connectivity alerts, and their inbox             | AlertController + 10 use cases                                                                                            | Active     |
| recommendation | Saving rules evaluated every hour and recommendation inbox                             | RecommendationController, RecommendationScheduler + 2 services                                                            | Active     |
| notification   | Delivery by email and Web Push, preferences and delivery log                           | NotificationChannelsController + 5 use cases                                                                              | Active     |

## 12.2 Domain class diagram

The class diagram models the classes, interfaces and enumerations of the domain, together with the design patterns applied: Strategy for role-based authorization (OwnerPermissions and MemberPermissions), Observer (named events) for the reaction to measurements and Factory Method for creating alerts, recommendations and notifications. The patterns are described in [`05.4-design-patterns.md`](../../05-diseno-software/EN/05.4-design-patterns.md) and the classes in [`05.3-class-model.md`](../../05-diseno-software/EN/05.3-class-model.md).

> **Note:** like the ERD, the complete diagram is presented in landscape. Appendix C splits it by domain to make it easier to read. The published figure may still carry the note that the Strategy pattern classes do not exist in the code; today they do (`[TO BE CONFIRMED]` when reviewing the figure).
>
**Figure 4.** *Domain class diagram, including the Strategy, Observer and Factory Method patterns.*

## 12.3 Backend modules by context

### 12.3.1 security module

**Function:** Identity, authentication, sessions, access control, audit and account deletion.

**Status:** Active

**Related files:**

- security/adapter/in/web/AuthController.java (single authentication, profile and account controller)

- security/adapter/in/web/dto/ (DTOs: RegisterRequest, LoginRequest, GoogleLoginRequest, DeleteAccountRequest, ProfileResponse, ApiError…)

- security/application/usecase/ (RegisterUserService, AuthenticateUserService, SignInWithGoogleService, RefreshSessionService, LogoutUserSessionService, ChangePasswordService, CreatePasswordResetTokenService, ResetPasswordService, EmailVerificationService, UpdateUserProfileService, DeleteAccountService, CheckPermissionService, AssignRoleService, RevokeRoleService, ManageUserStatusService…)

- security/domain/model/ (User, Person, Email, PasswordHash, PasswordPolicy, UserSession, RefreshToken, SystemRole, Permission, AuditLog, LoginErrorLog)

- security/infrastructure/ (SecurityFilterChainConfiguration, JwtTokenIssuer, PemRsaKeys, SecurityCorsConfiguration, JwtAuthenticationEntryPoint)

- security/adapter/out/security/ (BCryptPasswordHasherAdapter, Sha256TokenHasher, PepperedHmacResetCodeHasher, InMemoryResetAttemptLimiter)

**Related tables:** person, user, user_configuration, password_reset_token, user_session, refresh_token, security_configuration, password_policy, login_error_log, system_role, permission, user_system_role, system_role_permission, audit_log.

**Main operations:**

- Register an account and verify the email

- Sign in (email and password or Google account) and renew tokens

- Sign out

- Recover and change passwords

- View and update the profile

- Query permissions (RBAC)

- Delete the account (transactional: validates the password, closes the sessions, logically deletes personal data, releases the email and publishes `AccountDeleted`)

- Record audit and failed attempts

### 12.3.2 home module

**Function:** Management of households, members, favorites, consumption thresholds, household deletion and ownership transfer.

**Status:** Active

**Related files:**

- home/adapter/in/web/HomeController.java, HomeThresholdsController.java, HomeTypeController.java

- home/adapter/in/web/CurrentUserResolver.java (resolves the user from the JWT `sub` claim)

- home/application/usecase/ (CreateHomeService, JoinHomeService, LeaveHomeService, ListHomesService, ListMembersService, RemoveUserService, ToggleFavoriteService, DeleteHomeService, TransferHomeOwnershipService, HomeRemoval, RemoveDeletedAccountService, GetHomeThresholdsService, UpdateHomeThresholdsService, ListHomeTypesService, GetHomeTypeService)

- home/domain/model/ (Home, HomeType, HomeThresholds, UserHome, Role OWNER/MEMBER, OwnerPermissions, MemberPermissions)

- home/api/ (HomeApi, HomeDeleted, HomeOwnershipTransferred) and home/infrastructure/HomeApiImpl.java (public API consumed by other modules)

**Related tables:** home, home_type, home_thresholds, user_home.

**Main operations:**

- Create a household and generate an access code

- Join a household with a code

- List households and members

- Mark a favorite

- Expel members and leave the household (the last owner cannot leave)

- View, update and reset daily and monthly thresholds

- Delete the household (OWNER only; soft delete of `home`, `user_home` and `home_thresholds`; publishes `HomeDeleted`, which `device` handles in the same transaction to unlink the devices and revoke their keys; after the commit, the MQTT unlink command is sent). The same behavior applies when the sole OWNER deletes their account.

- Transfer ownership (OWNER only; atomic role swap with a row lock; publishes `HomeOwnershipTransferred`, which no module consumes yet)

### 12.3.3 measurement module

**Function:** Ingestion and analysis of electrical consumption measurements.

**Status:** Active

**Related files:**

- measurement/adapter/in/web/MeasurementController.java, ConsumptionLevelController.java, HomeConsumptionController.java

- measurement/adapter/in/mqtt/ (TelemetryMqttConsumer, StatusMqttConsumer, MqttTopics, MqttConfig)

- measurement/application/usecase/ (RegisterMeasurementService, ListMeasurementsService, GetLatestMeasurementService, GetConsumptionStatisticsService, HomeConsumptionUseCase, HourlyEnergyCalculator, ListConsumptionLevelsService, TimeRange)

- measurement/domain/model/ (Measurement, ConsumptionLevel)

- measurement/adapter/out/event/MeasurementEventAdapter.java (publishes MeasurementRecorded)

**Related tables:** measurement, consumption_level (connection status is recorded in device_status_log).

**Main operations:**

- Record the readings that arrive over MQTT (voltage, current, active power, accumulated energy)

- Record the devices' status messages

- Query history by device and date range, and the latest measurement

- Compute statistics, and the household consumption summary and history

- Publish the MeasurementRecorded event

### 12.3.4 alert module

**Function:** Generation and inbox of consumption, limit, device linked and connectivity alerts.

**Status:** Active

**Related files:**

- alert/adapter/in/web/AlertController.java (query, read and delete)

- alert/adapter/in/event/MeasurementRecordedListener.java and DeviceConnectivityLostListener.java (@EventListener)

- alert/application/usecase/ (ListAlertsService, GetAlertService, ResolveAlertService, DeleteAlertsService, RegisterThresholdAlertService, RegisterLimitAlertService, RegisterConnectivityAlertService, RegisterDeviceLinkedAlertService, ResolveConnectivityAlertsService, AlertAccessService)

- alert/infrastructure/AlertApiImpl.java and alert/api/AlertRaised.java

**Related tables:** alert.

**Main operations:**

- Generate high or critical consumption alerts when a measurement is received

- Generate daily or monthly limit alerts and device linked alerts (with the first reading after linking)

- Generate and resolve connectivity alerts (150 seconds of silence or last will message)

- List alerts by household and status, view one, mark it as read and delete resolved or read ones

### 12.3.5 notification module

**Function:** Delivery of alerts and recommendations by email and Web Push, preferences and delivery log.

**Status:** Active

**Related files:**

- notification/adapter/in/web/NotificationChannelsController.java

- notification/adapter/in/event/AlertRaisedListener.java and RecommendationCreatedListener.java

- notification/infrastructure/mail/EmailSender.java, MailDeliveryConfiguration.java, AbandonedDeliverySweeper.java and notification/adapter/out/push/WebPushSender.java

- notification/application/usecase/ (NotifyAlert, SendPasswordResetEmail, SendEmailVerification, ManageNotificationChannels, AlertMessages)

- notification/adapter/out/persistence/adapter/NotificationPersistenceAdapter.java

**Related tables:** notification, notification_preference, push_subscription.

**Main operations:**

- Send verification and recovery emails

- Deliver each alert and recommendation to the household members by email and Web Push according to their preferences

- Record the delivery status (PENDING, SENT, FAILED) without storing the message content

- Sweep abandoned deliveries when the application starts and every five minutes

- Manage preferences and push subscriptions

### 12.3.6 device module

**Function:** Registration and linking of devices, credentials (API key), connection status and authorization of the MQTT broker.

**Status:** Active

**Related files:**

- device/adapter/in/web/HomeDeviceController.java, ApplianceTypeController.java and BrokerAuthController.java

- device/application/usecase/ (HomeDeviceService, BrokerAccessService, DeviceConnectivityService)

- device/domain/model/ (Device, ApiKeyHash…)

- device/adapter/in/event/HomeDeletedListener.java and DeviceCommandListener.java; device/adapter/in/scheduler/DeviceInactivityScheduler.java

- device/adapter/out/ (MqttDeviceCommandAdapter, persistence and credential adapters)

**Related tables:** device, appliance_type, device_status_log, device_home.

**Main operations:**

- Link a device to a household (OWNER only), edit it, issue new credentials to change its Wi-Fi and unlink it

- Authenticate the device and authorize its topics for Mosquitto (only the SHA-256 hash of the key is stored)

- Mark the device offline after 150 seconds of silence (`DeviceInactivityScheduler`)

- Send the MQTT unlink command when deleting the household or unlinking the device

### 12.3.7 recommendation module

**Function:** Saving recommendations generated from the measured consumption.

**Status:** Active

**Related files:**

- recommendation/adapter/in/web/RecommendationController.java

- recommendation/adapter/in/scheduler/RecommendationScheduler.java (every hour)

- recommendation/application/usecase/ (EvaluateRecommendationsService, RecommendationInboxService)

- recommendation/adapter/out/event/RecommendationEventAdapter.java (publishes RecommendationCreated)

**Related tables:** recommendation.

**Main operations:**

- Evaluate five saving rules every hour (peak hours, standby consumption, weekly comparison, limit projection and per-device increase), without repeating the same recommendation within 7 days

- List a household's recommendations, mark them as read and delete them

## 12.4 Web front-end module

**Function:** electrical consumption query and management interface (React SPA)

**Related files:**

- src/routes/AppRoutes.jsx (routes), ProtectedRoute.jsx (protected routes) and HomeRoute.jsx (household detail)

- src/services/http/httpClient.js (axios client, interceptors and token renewal)

- src/services/auth, home, devices, alerts, recommendations and notifications (consumed endpoints)

- src/context/HomeContext.jsx, ThemeContext.jsx and NotificationCenterContext.jsx (global state)

- src/features/ (landingPage, auth, dashboard, DetailHome, Favorites, Notifications, Account, SettingsPage)

**Screens and routes:**

| **Route**                                                 | **Screen**                  | **Function**                                                                                |
|-----------------------------------------------------------|-----------------------------|---------------------------------------------------------------------------------------------|
| /home, /about, /support                                   | Landing page                | Public presentation, about and support pages.                                               |
| /login, /register                                         | Login and Register          | Authentication (including Google) and account creation with zod validation.                 |
| /VerifyAccount, /recover-password, /verifyRecoverPassword | Verification and recovery   | Email verification code and password recovery.                                              |
| /dashboard                                                | Household panel             | Household cards, create household, join with code and favorites.                            |
| /homes/:homeId                                            | Household detail            | Six tabs: Consumption, History, Users (transfer ownership), Devices (Bluetooth linking), Thresholds and Home (delete household). |
| /favorites                                                | Favorites                   | List of households marked as favorites.                                                     |
| /notifications                                            | Notifications               | Household alerts and recommendations; read and delete them.                                 |
| /settings                                                 | Settings                    | Appearance, language, notifications (email and push) and help.                              |
| /account                                                  | Account                     | Profile editing, photo, password and account deletion.                                      |

**Main operations:** sign in and sign out, register and verify an account, recover a password, create and join households, mark favorites, view and edit members and thresholds, link and manage devices, view consumption and history, view alerts and recommendations, enable push notifications, delete a household or transfer its ownership, change language and theme.

## 12.5 Mobile front-end module

**Function:** Android application connected to the REST API

**Related files:**

- src/app/ (Expo Router routes: \_layout, index, register, verify-account, recover-password, dashboard, homes, home/\[id\], favorites, notifications, settings, account)

- src/services/ (http, auth, home, devices, measurement, alerts, recommendations, notifications)

- src/features/ (auth, dashboard, homes, favorites, notifications, settings, account)

- src/context/ (HomeContext, UserContext, ThemeContext, NotificationCenterContext)

- src/components/ and src/constants/ (own design system with StyleSheet)

**Main operations:** registration, code verification, password recovery, household panel, household detail with six tabs, favorites, notifications (alerts and recommendations), language and theme settings, and account management.

> **Current limitations:** deleting a household and transferring its ownership only exist on the web. The mobile application has no automated tests and is not available for iPhone.

# 13. API or system routes

**Base URL:** http://localhost:8080/api/v1 \| **Format:** JSON \| **Interactive documentation:** /swagger-ui.html and /v3/api-docs

The backend exposes 54 business routes under `/api/v1`, two internal routes for the broker and the infrastructure routes. All those not listed as public require a valid JWT in the Authorization header; if the token is missing, expired or invalid, the API answers 401, and if the user lacks the required permission, it answers 403.

| **Method** | **Route**                                             | **Description**                                                       | **Parameters**                                                         | **Authentication** |
|------------|-------------------------------------------------------|-----------------------------------------------------------------------|------------------------------------------------------------------------|--------------------|
| POST       | /api/v1/auth/register                                 | Registers a new account.                                              | body: email, password, name, lastName, profileImage (optional)         | Public             |
| POST       | /api/v1/auth/email/verify                             | Verifies the email with a 6-digit code.                               | body: email, code                                                      | Public             |
| POST       | /api/v1/auth/email/verification/resend                | Resends the verification code (always answers 202).                   | body: email                                                            | Public             |
| POST       | /api/v1/auth/login                                    | Authenticates and returns access token and refresh token.             | body: email, password                                                  | Public             |
| POST       | /api/v1/auth/google                                   | Signs in or registers the account with Google (503 if not configured). | body: code                                                            | Public             |
| POST       | /api/v1/auth/refresh                                  | Renews the access token and rotates the refresh token.                | body: refreshToken                                                     | Public             |
| POST       | /api/v1/auth/logout                                   | Closes the registered session.                                        | body: idUserSession                                                    | JWT                |
| POST       | /api/v1/auth/password/forgot                          | Requests the password recovery code.                                  | body: email                                                            | Public             |
| POST       | /api/v1/auth/password/reset                           | Resets the password with the received code.                           | body: email, resetToken, newPassword                                   | Public             |
| POST       | /api/v1/auth/password/change                          | Changes the password knowing the current one.                         | body: currentPassword, newPassword                                     | JWT                |
| PUT        | /api/v1/auth/profile                                  | Updates name, last name, image or email.                              | body: name, lastName, profileImage, newEmail                           | JWT                |
| POST       | /api/v1/auth/account/delete                           | Deletes the account after confirming the password.                    | body: password                                                         | JWT                |
| GET        | /api/v1/auth/account                                  | Returns the authenticated account.                                    | none (identity from the JWT)                                           | JWT                |
| GET        | /api/v1/auth/permissions                              | Lists the user's permission codes.                                    | none                                                                   | JWT                |
| GET        | /api/v1/measurements                                  | Lists a device's measurements in a range.                             | query: deviceId (required), from, to                                   | JWT                |
| GET        | /api/v1/measurements/latest                           | Returns the latest measurement of a device.                           | query: deviceId                                                        | JWT                |
| GET        | /api/v1/measurements/statistics                       | Consumption statistics for the period.                                | query: deviceId, from, to                                              | JWT                |
| GET        | /api/v1/consumption-levels                            | Lists the consumption levels (LOW, MEDIUM, HIGH, CRITICAL).           | none                                                                   | JWT                |
| GET        | /api/v1/homes/{homeId}/consumption/summary           | Household consumption summary.                                        | path: homeId; query: zone (UTC by default)                             | JWT                |
| GET        | /api/v1/homes/{homeId}/consumption/history           | Household consumption history by period.                              | path: homeId; query: period (month by default), zone                   | JWT                |
| POST       | /api/v1/homes                                         | Creates a household and returns its access code.                      | body: name, homeTypeId, address, description                           | JWT                |
| GET        | /api/v1/homes                                         | Lists the user's households with their role.                          | none                                                                   | JWT                |
| GET        | /api/v1/homes/{homeId}/members                        | Lists a household's members.                                          | path: homeId                                                           | JWT                |
| POST       | /api/v1/homes/join                                    | Lets a user join a household with its access code.                    | body: accessCode                                                       | JWT                |
| PUT        | /api/v1/homes/{homeId}/favorite                       | Marks or unmarks a household as favorite.                             | path: homeId                                                           | JWT                |
| DELETE     | /api/v1/homes/{homeId}/members/{userId}               | Expels a member (OWNER only).                                         | path: homeId, userId                                                   | JWT                |
| DELETE     | /api/v1/homes/{homeId}/members/me                     | Lets the user leave the household.                                    | path: homeId                                                           | JWT                |
| DELETE     | /api/v1/homes/{homeId}                                | Deletes the household (OWNER only): 204; 403 if MEMBER; 404 if not a member. | path: homeId                                                    | JWT                |
| PUT        | /api/v1/homes/{homeId}/owner                          | Transfers ownership to a member: 204; 403, 404 and 409 as applicable. | path: homeId; body: newOwnerId                                         | JWT                |
| GET        | /api/v1/homes/{homeId}/thresholds                     | Queries the household thresholds.                                     | path: homeId                                                           | JWT                |
| PUT        | /api/v1/homes/{homeId}/thresholds                     | Updates the household limit.                                          | path: homeId; body: limitPeriod (DAILY or MONTHLY), limit              | JWT                |
| POST       | /api/v1/homes/{homeId}/thresholds/defaults            | Resets the thresholds to the system values (6 kWh daily and 180 kWh monthly). | path: homeId                                                   | JWT                |
| GET        | /api/v1/home-types                                    | Lists the available household types.                                  | none                                                                   | JWT                |
| GET        | /api/v1/home-types/{idHomeType}                       | Queries a household type.                                             | path: idHomeType                                                       | JWT                |
| GET        | /api/v1/appliance-types                               | Lists the appliance catalog.                                          | none                                                                   | JWT                |
| GET        | /api/v1/homes/{homeId}/devices                        | Lists the household's devices.                                        | path: homeId                                                           | JWT                |
| POST       | /api/v1/homes/{homeId}/devices                        | Links a device and returns its credentials and the broker address (OWNER only). | body: deviceCode, name, applianceTypeId, location            | JWT                |
| PUT        | /api/v1/homes/{homeId}/devices/{deviceId}             | Edits appliance, name and location.                                   | path: homeId, deviceId; body: name, applianceTypeId, location          | JWT                |
| POST       | /api/v1/homes/{homeId}/devices/{deviceId}/credentials | Issues a new key to change the Wi-Fi network (invalidates the previous one). | path: homeId, deviceId                                          | JWT                |
| DELETE     | /api/v1/homes/{homeId}/devices/{deviceId}             | Unlinks the device (OWNER only).                                      | path: homeId, deviceId                                                 | JWT                |
| GET        | /api/v1/alerts                                        | Lists a household's alerts.                                           | query: homeId (required), status (PENDING \| RESOLVED)                 | JWT                |
| GET        | /api/v1/alerts/{idAlert}                              | Queries an alert.                                                     | path: idAlert                                                          | JWT                |
| PUT        | /api/v1/alerts/{idAlert}/read                         | Marks an alert as read.                                               | path: idAlert                                                          | JWT                |
| DELETE     | /api/v1/alerts/{idAlert}                              | Deletes a resolved or read alert.                                     | path: idAlert                                                          | JWT                |
| DELETE     | /api/v1/alerts                                        | Deletes all the resolved alerts of a household.                       | query: homeId                                                          | JWT                |
| GET        | /api/v1/recommendations                               | Lists a household's recommendations.                                  | query: homeId                                                          | JWT                |
| PUT        | /api/v1/recommendations/{idRecommendation}/read       | Marks a recommendation as read.                                       | path: idRecommendation                                                 | JWT                |
| DELETE     | /api/v1/recommendations/{idRecommendation}            | Deletes a read recommendation.                                        | path: idRecommendation                                                 | JWT                |
| DELETE     | /api/v1/recommendations                               | Deletes all the read recommendations of a household.                  | query: homeId                                                          | JWT                |
| GET        | /api/v1/notifications/preferences                     | Queries the user's notification preferences.                          | none                                                                   | JWT                |
| PUT        | /api/v1/notifications/preferences                     | Updates the email and push preferences.                               | body: emailEnabled, pushEnabled                                        | JWT                |
| GET        | /api/v1/notifications/push/public-key                 | Returns the VAPID public key (empty if push is disabled).             | none                                                                   | JWT                |
| POST       | /api/v1/notifications/push/subscriptions              | Registers a browser push subscription.                                | body: endpoint, keys (p256dh, auth)                                    | JWT                |
| POST       | /api/v1/notifications/push/subscriptions/remove       | Removes a push subscription.                                          | body: endpoint, keys                                                   | JWT                |
| POST       | /internal/mqtt/auth/user                              | Internal: Mosquitto authenticates an MQTT client (five-minute cache). The proxy must block it. | body: username, password, clientid          | Internal network   |
| POST       | /internal/mqtt/auth/acl                               | Internal: Mosquitto authorizes a client's topic. The proxy must block it. | body: username, clientid, topic, acc                               | Internal network   |
| GET        | /actuator/health                                      | Service status (UP / DOWN).                                           | none                                                                   | Public             |
| GET        | /swagger-ui.html and /v3/api-docs                     | Interactive API documentation (OpenAPI).                              | none                                                                   | Public             |
| OPTIONS    | /\*\*                                                 | CORS preflight.                                                       | Origin and Access-Control-Request-\* headers                           | Public             |

> *The `/internal/**` routes only accept calls from the networks in `MQTT_AUTH_ALLOWED_NETWORKS`. Behind a reverse proxy every request looks local, so the proxy must not forward them.*

## 13.1 Common response codes

| **Code**        | **When it occurs**                                                            | **Response structure**                                         |
|-----------------|-------------------------------------------------------------------------------|----------------------------------------------------------------|
| 200 / 201 / 204 | Successful operation (query, creation or deletion with no content).           | Body with the corresponding resource or message.               |
| 400             | Incorrect data validation (Bean Validation or zod schema in the client).      | ApiError with fieldErrors detailing each field.                |
| 401             | Missing or expired token or invalid credentials.                              | ApiError with generic message (does not reveal whether the email exists). |
| 403             | Blocked or inactive account, or insufficient permission (e.g. a MEMBER tries to delete the household). | ApiError.                          |
| 404             | Nonexistent resource (household, alert, member…) or household the user does not belong to. | ApiError or {"error": "..."} depending on the module. |
| 409             | Conflict (email already registered, last owner who cannot leave, transfer to the current owner). | ApiError.                    |
| 422             | The password does not meet the security policy.                               | ApiError.                                                      |
| 429             | Attempt limit reached in verification or recovery.                            | ApiError with Retry-After header (900 seconds).                |
| 503             | Google sign-in without configured credentials.                                | ApiError.                                                      |
| 500             | Unhandled internal error.                                                     | ApiError without stack trace (a stack trace is never leaked).  |

## 13.2 Request and response example

```
POST /api/v1/auth/login
Content-Type: application/json

{ "email": "user@email.com", "password": "********" }

200 OK

{
  "accessToken": "eyJhbGciOiJSUzI1NiIsImV4cCI6MTc2…",
  "refreshToken": "7f3c…",
  "tokenType": "Bearer",
  "expiresIn": 900,
  "account": { "idUser": "user000001", "email": "user@email.com", "status": "ACTIVE" }
}
```

# 14. Security

## 14.1 Implemented controls

| **Control**                   | **Technical implementation**                                                                                                                                                                            |
|-------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Authentication                | JWT signed with the RS256 algorithm and RSA key pair in PEM files; access token lifetime: 15 minutes; no roles inside the JWT; claims sub (idUser), person_id and email.                                |
| Identity                      | All modules obtain the user from the `sub` claim through `CurrentUserResolver`; the X-User-Id header is ignored.                                                                                        |
| Sessions                      | STATELESS: no cookies or HTTP session; CSRF disabled because there is no ambient credential.                                                                                                            |
| Session renewal               | Opaque, stateful refresh token rotated on every use, stored as a SHA-256 hash with family_id and parent_id to detect reuse (replay) and revoked when the password changes.                              |
| Password hashing              | BCrypt (BCryptPasswordHasherAdapter) isolated from the domain; passwords are never stored in clear.                                                                                                     |
| Password policy               | password_policy table: minimum length 8 and maximum 64, requires uppercase, numbers and symbols, expires after 90 days. Non-compliance =\> 422.                                                         |
| Global authorization (RBAC)   | Chain of five tables (system_role, permission, user_system_role, system_role_permission and user) queried live by `CheckPermission`; permissions can be queried at GET /api/v1/auth/permissions.         |
| Per-household authorization   | OWNER and MEMBER roles per household resolved with the Strategy pattern (`OwnerPermissions`, `MemberPermissions`).                                                                                       |
| Input validation              | Bean Validation in the backend DTOs (@NotBlank, @Size, @NotNull, @Positive, @Pattern) and zod schemas in the front-end forms.                                                                           |
| CORS                          | Closed by default: explicit list of origins (SECURITY_CORS_ALLOWED_ORIGINS), no wildcards, no credentials and with OPTIONS preflight handling; methods GET, POST, PUT, DELETE and OPTIONS.              |
| Attempt limit                 | In-memory limiter: 10 attempts per IP every 15 minutes in password recovery =\> 429 with Retry-After header.                                                                                            |
| Enumeration protection        | Failed login with a generic message and password/forgot with an identical 202 response whether or not the account exists; the work is done asynchronously.                                              |
| Recovery codes                | 6 digits, 15-minute validity, single use, stored as HMAC-SHA256 with pepper (SECURITY_RESET_PEPPER).                                                                                                    |
| Device credentials            | Each device uses `device_code` and `api_key`; the backend only stores the SHA-256 hash of the key (`api_key_hash`). Mosquitto authenticates the device against the backend and the ACL limits its topics. |
| Device transport              | MQTT 3.1.1 with TLS on port 8883; port 1883 is internal and the `/internal/**` routes are only accepted from allowed networks (the proxy must block them).                                             |
| Audit                         | audit_log table (actions, IP, user) and login_error_log (failed attempts); the IP is taken from getRemoteAddr(), ignoring X-Forwarded-For.                                                              |
| Documentation                 | The OpenAPI endpoint declares the bearerAuth scheme; the public endpoints are only the authentication and infrastructure ones.                                                                          |
| Transport                     | In production it must be served over HTTPS; in local development HTTP is used with Mailpit as the test email server.                                                                                    |

## 14.2 Known limitations and risks

> The following points are declared by the team itself and must be taken into account before extending the system.

| **Limitation**                                                                                 | **Impact**                                                                            | **Recommendation**                                                                  |
|------------------------------------------------------------------------------------------------|---------------------------------------------------------------------------------------|-------------------------------------------------------------------------------------|
| Telemetry and status with MQTT QoS 0.                                                          | A reading can be lost (no duplicates).                                                | Evaluate an MQTT client with QoS 1 (for example espMqttClient).                     |
| Access token and refresh token stored in localStorage (web).                                   | An XSS vulnerability could read the tokens (accepted and documented risk).            | Migrate to httpOnly cookies with SameSite or strengthen the Content Security Policy. |
| In-memory attempt limiter.                                                                     | It resets on every start and is not shared between instances.                         | Use Redis or another shared store, or a limiter at the load balancer.               |
| No over-the-air (OTA) firmware update.                                                         | Updating the firmware requires a USB cable.                                           | Evaluate OTA in a future version.                                                   |
| Persistence tests are not hermetic.                                                            | They need MySQL running and SPRING_DATASOURCE_PASSWORD.                               | Use a database container for tests.                                                 |
| The access token remains valid for up to 15 minutes after account deletion.                    | Brief window of residual access.                                                      | Accepted; documented.                                                               |
| Edge cases of connectivity alerts (CONNECTIVITY) without automatic resolution.                 | Some alerts need manual review.                                                       | Define the automatic resolution rule.                                               |
| Web Bluetooth limited to Chrome, Edge and Chrome for Android, and to HTTPS or localhost.       | On other browsers the Wi-Fi configuration portal is used; on iOS there is no push without installing the web. | Keep the `EnergyMonitor-Setup` portal as an alternative.             |
| `HomeOwnershipTransferred` has no consumer.                                                    | The new owner is not notified when receiving ownership.                               | Consume the event and send email or push.                                           |
| A device's history is not limited to the linking period.                                       | A household can see measurements from a previous period of another household.         | Limit the history to the linking period (RF-HOM-11).                                |
| Household deletion and transfer absent from the mobile application; no automated tests.       | They can only be done from the web.                                                   | Implement them in the Android application and add tests.                            |

## 14.3 Best practices for developers

- Never commit the .env or application-local.yaml files or the RSA keys (\*.pem); both are already declared in .gitignore.

- Do not log passwords, tokens or recovery codes in the application logs.

- Keep the backend and front-end dependencies up to date and review their security advisories.

- Every new feature must validate input in the backend, regardless of client-side validations.

- Respect module boundaries: do not import classes from another context without going through its api package (verified by ModularityTests).

- Rotate the JWT keys and the recovery pepper if a leak is suspected.

# 15. Software version control

The project uses Git with a remote repository on GitHub. Each component of the system is developed in an independent repository to ease its deployment and review.

| **Repository**             | **Content**                                                      | **Remarks**                                                                               |
|----------------------------|------------------------------------------------------------------|-------------------------------------------------------------------------------------------|
| energy-monitor-backend     | REST API in Java 21 / Spring Boot.                               | Includes tests, docker-compose.yml, Dockerfile, .env.example and the Liquibase migration history. |
| Energy-Monitor-application | React 19 / Vite 8 web application.                               | Includes Vitest tests (14 files) and production build in /dist.                           |
| Energy-Monitor-Mobile      | Expo SDK 57 mobile application.                                  | Includes LICENSE, AGENTS.md (versioned Expo 57 documentation) and Expo scripts.           |
| energy-monitor-devices     | ESP32 firmware and Mosquitto broker.                             | Includes development and production docker-compose.yml and the MQTT contract.             |
| energy-monitor-docs-final  | Bilingual final documentation (ES and EN).                       | Project charter, requirements specification, technical proposal, design and manuals.      |

- Branches follow the `[type]/[kebab-case-name]` convention (feat/*, fix/*, chore/*) and commits follow Conventional Commits with the message in English.

- Code is integrated through a pull request to `dev` and then to `main`.

- Before merging changes, the backend test suite (mvnw test) and the web front tests (npm test) must be run.

- Database schema changes are made only through a new Liquibase changeset with rollback; already applied changesets are never edited.

- Integration branches must keep the code compilable and the tests green.

- Delivered versions are tagged to preserve the exact reference of the software documented in this manual.

# 16. Maintenance recommendations

## 16.1 Code updates

| **Area**      | **Recommendation**                                                                                                                                                                |
|---------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Backend       | Build with .\mvnw.cmd clean test before each delivery; verify that ModularityTests confirms the module boundaries were not broken.                                                |
| Database      | Create a new changeset for each schema change, with a sequential identifier (e.g. security-025) and its rollback; run docker compose up -d to test against a real MySQL 8.4.      |
| Dependencies  | Update Spring Boot, Expo or React in test branches, running the full suite before merging.                                                                                        |
| Web front     | Keep the VITE_API_BASE_URL variable and the allowed origins list (SECURITY_CORS_ALLOWED_ORIGINS) in sync; rerun npm test and npm run lint.                                        |
| Mobile front  | Review the versioned Expo 57 documentation before adding native libraries; there are no automated tests, so verification is manual.                                               |
| Devices       | Rebuild and upload the firmware over USB after changing config.h or the certificate; renew the broker certificate and restart the `mosquitto` container.                          |
| Security      | Review the RBAC permissions, the password policy and the validity of the JWT keys quarterly.                                                                                      |

## 16.2 Database backups

Before each relevant update, the energy_monitor schema must be backed up. Recommended procedure:

1.  Stop writing information (do not run measurement loads during the backup).

2.  Run the dump: mysqldump -h localhost -u energy_app -p energy_monitor \> backup_energy_monitor_YYYY-MM-DD.sql

3.  Save the file in a safe folder outside the repository, with the date in the name.

4.  Verify the backup by restoring it in a test database before considering it valid.

5.  Restore: mysql -h localhost -u energy_app -p energy_monitor \< backup_energy_monitor_YYYY-MM-DD.sql

## 16.3 Support and incident resolution

| **Symptom**                             | **Suggested check**                                                                                                                  |
|-----------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------|
| The backend does not start              | Check port 8080 (netstat), the MySQL container (docker compose ps) and the SPRING_DATASOURCE_PASSWORD variable.                      |
| The API answers 403/401 massively       | Verify the RSA key pair (SECURITY_JWT_PRIVATE_KEY_PATH / PUBLIC) and the server date and time.                                       |
| The web front does not connect to the API | Confirm that the backend answers on the indicated port and that SECURITY_CORS_ALLOWED_ORIGINS includes the origin http://localhost:5173. |
| Emails do not arrive                    | Check MAIL_HOST, MAIL_PORT, MAIL_USERNAME and MAIL_PASSWORD; locally use Mailpit (http://localhost:8025).                            |
| A device does not send readings         | Check the broker (docker compose logs mosquitto), DEVICE_MQTT_PUBLIC_HOST (never localhost), the TLS certificate and the ESP32 serial output. |
| Migrations fail on startup              | Check the Liquibase log and the order of the changesets in db.changelog-master.xml.                                                  |
| Error messages are not readable         | Check the browser console and the ApiError body returned by the API.                                                                 |

## 16.4 Declared pending work

- Limit a device's history to the linking period (RF-HOM-11).

- Consume `HomeOwnershipTransferred` to notify the household's new owner.

- Implement household deletion and transfer in the mobile application and complete its automated tests.

- Evaluate MQTT with QoS 1 so that readings are not lost.

- Automatically resolve the edge cases of connectivity alerts.

- Expose over HTTP the administrative operations on roles and user statuses that already exist as application ports.

- Evaluate over-the-air (OTA) firmware updates.

- Decide what to do with the legacy repository `en-monitor-security-service` (archive or delete it): it is not used.

# 17. Glossary

| **Term**              | **Definition**                                                                                                                        |
|-----------------------|---------------------------------------------------------------------------------------------------------------------------------------|
| ADSO                  | Software Analysis and Development: SENA technology program that is part of the project (group/ficha 3145556).                         |
| REST API              | HTTP-based programming interface that exposes resources in JSON format under versioned routes (/api/v1).                              |
| Access token          | Short-lived JWT (15 minutes) that proves the user's identity on every request.                                                        |
| Bounded context       | Domain bounded context (security, home, measurement…) with its own schema and internal API.                                           |
| BCrypt                | Salted hashing algorithm used to store passwords irreversibly.                                                                        |
| MQTT broker           | Server (Eclipse Mosquitto) that receives the messages published by the devices and delivers them to subscribers.                      |
| CORS                  | HTTP policy that controls from which origins (e.g. http://localhost:5173) the browser may call the API.                               |
| Changeset             | Liquibase migration unit: versioned instruction to create or modify tables.                                                           |
| CurrentUserResolver   | Component of each module that obtains the user's identity from the `sub` claim of the JWT.                                            |
| Docker Compose        | Tool that defines and starts containers (for example mysql:8.4) with a single command.                                                |
| DTO                   | Data transfer object: JSON input or output structure of an endpoint.                                                                  |
| ER / ERD              | Entity-relationship diagram: logical model of the tables and their relationships.                                                     |
| JWT                   | JSON Web Token: signed token (RS256 algorithm) that carries the user's identity.                                                      |
| Liquibase             | Schema manager: applies database changesets in a controlled and versioned way.                                                        |
| Module (Modulith)     | Code unit with boundaries verified by Spring Modulith; it only relates through its api package.                                       |
| MQTT / QoS            | Messaging protocol for devices and delivery quality level (0: at most once; 1: at least once).                                        |
| PK / Primary key      | Field that uniquely identifies a row (in this project, VARCHAR(10)).                                                                  |
| FK / Foreign key      | Field that references another table within the same context. Between contexts only IDs are stored (no SQL FK).                        |
| RBAC                  | Role-based access control: system_role, permission, user_system_role and system_role_permission.                                      |
| Refresh token         | Long-lived opaque token stored as a SHA-256 hash that allows renewing the access token (rotation and reuse detection).                |
| SHA-256               | Cryptographic hash function used to store refresh tokens, device keys and, with an HMAC pepper, recovery codes.                       |
| SPA                   | Single Page Application: web application that runs entirely in the browser and navigates without reloading (React + react-router).   |
| Swagger / OpenAPI     | Interactive documentation automatically generated at /swagger-ui.html.                                                                |
| Thresholds            | Daily and monthly consumption limits configurable per household that trigger alerts.                                                  |
| Soft delete           | No physical DELETE is executed: deleted_at with a value indicates that the record was deleted.                                        |
| Web Push              | Browser notifications delivered even when the tab is closed, using VAPID keys.                                                        |
| X-User-Id             | Header the backend ignores: identity is always taken from the `sub` claim of the JWT.                                                 |

# 18. Appendices

## Appendix A. Configuration and commands

**Environment variables.** The backend reads its secrets from a .env file (imported through spring.config.import) whose structure is documented in .env.example and which is read as `.properties` (values without quotes and without `$`); the web front uses .env.local with VITE_API_BASE_URL and VITE_GOOGLE_CLIENT_ID, and the mobile app uses .env with EXPO_PUBLIC_API_BASE_URL. For security reasons no real values are published:

| **Variable**                                                 | **Description**                                                                                      |
|--------------------------------------------------------------|------------------------------------------------------------------------------------------------------|
| MYSQL_PASSWORD / MYSQL_ROOT_PASSWORD                         | Passwords of the application user and the MySQL root user (defined in docker-compose.yml).           |
| SPRING_DATASOURCE_PASSWORD                                   | Password used by the tests with the test profile.                                                    |
| MAIL_HOST / MAIL_PORT                                        | SMTP server and port (default smtp.gmail.com:587).                                                   |
| MAIL_USERNAME / MAIL_PASSWORD / MAIL_FROM                    | Sender account and application password to send emails.                                              |
| SECURITY_RESET_PEPPER                                        | HMAC-SHA256 pepper of the password recovery codes.                                                   |
| SECURITY_CORS_ALLOWED_ORIGINS                                | Origins allowed by CORS (http://localhost:5173 in the example).                                      |
| SECURITY_JWT_PRIVATE_KEY_PATH / SECURITY_JWT_PUBLIC_KEY_PATH | Absolute paths to the RSA key pair (private.pem and public.pem) used by RS256.                       |
| GOOGLE_CLIENT_ID / GOOGLE_CLIENT_SECRET                      | OAuth credentials to sign in with Google; when empty, the button answers 503.                        |
| MQTT_BROKER_URL / MQTT_BACKEND_USER / MQTT_BACKEND_PASS / MQTT_CLIENT_ID | Connection of the backend to the MQTT broker (internal listener 1883).                   |
| MQTT_AUTH_ALLOWED_NETWORKS                                   | Networks allowed to call the broker's internal authentication routes.                                |
| DEVICE_MQTT_PUBLIC_HOST / DEVICE_MQTT_PUBLIC_PORT            | Broker address given to the device when linking it (port 8883).                                      |
| DEVICE_OFFLINE_AFTER                                         | Silence after which a device is marked offline (PT150S).                                             |
| NOTIFICATION_WEB_URL / NOTIFICATION_VAPID_PUBLIC_KEY / NOTIFICATION_VAPID_PRIVATE_KEY / NOTIFICATION_VAPID_SUBJECT | Email links and VAPID keys of browser push.                 |
| ALERT_LIMIT_ZONE / RECOMMENDATION_EVALUATE_INTERVAL / RECOMMENDATION_ZONE | Time zone of the limit alerts and frequency and time zone of the recommendations.       |
| VITE_API_BASE_URL / VITE_GOOGLE_CLIENT_ID (web front)        | Backend base URL (default http://localhost:8080) and Google client ID.                               |
| EXPO_PUBLIC_API_BASE_URL (mobile app)                        | Backend base URL: 10.0.2.2 on the Android emulator or the PC's IP on a phone.                        |

**Operation commands:**

| **Command**                             | **Description**                                                                    |
|-----------------------------------------|------------------------------------------------------------------------------------|
| docker compose up -d                    | Starts MySQL 8.4 with the energy_monitor schema.                                   |
| docker compose down -v                  | Stops the database and removes the volume (regenerates the schema with Liquibase). |
| docker run -p 8025:8025 axllent/mailpit | Starts the test email server (interface at http://localhost:8025).                 |
| .\mvnw.cmd spring-boot:run              | Starts the REST API at http://localhost:8080.                                      |
| .\mvnw.cmd test                         | Runs the backend test suite.                                                       |
| .\mvnw.cmd package                      | Generates the executable JAR in the /target folder.                                |
| npm install / npm run dev               | Installs dependencies and starts the web application (http://localhost:5173).     |
| npm test / npm run lint                 | Runs the tests and the static analysis of the web front.                           |
| npm run start / npm run android         | Starts the mobile application in Expo (Android device or emulator).                |
| docker compose up -d (devices/docker)   | Starts the development Mosquitto broker.                                           |
| arduino-cli compile / upload            | Builds and uploads the device firmware over USB.                                   |

**Post-installation verification:** GET /actuator/health answers {"status":"UP"}, the sign-in screen loads in the browser, registering an account sends the verification code, the households created appear in the application panel and the first reading of a linked device appears in the household.

# Appendix B. ERD by context

Entity-relationship model split by context to make it easier to read. The conventions (snake_case, VARCHAR(10) primary keys, audit columns and soft delete) are the same as in the complete diagram of section 11.1.

**Figure 5.** *Security context: people, users, sessions, tokens, roles, permissions and audit.*

**Figure 6.** *Home Management and Devices contexts: households, members, thresholds and devices.*

**Figure 7.** *Monitoring and measurements and Alerts contexts: measurements, consumption levels and alerts.*

**Figure 8.** *Recommendations and Notifications contexts: recommendations and delivery log.*

# Appendix C. Class diagram by domain

**Figure 9.** *Domain_Security and Domain_Access_Control domains, with the Strategy pattern and the enumerations.*

**Figure 10.** *Domain_Home_Management and Domain_Devices domains.*

**Figure 11.** *Domain_Measurements and Domain_Notifications domains, with Observer and Factory Method.*
