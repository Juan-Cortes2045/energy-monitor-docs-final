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
| Version              | 1.3                                                                             |
| Date prepared        | October 7, 2026 (update: October 25, 2026)                                      |

Year 2026

# 2. Document version control

The change history of this manual is recorded in the following table. Every relevant change to the software must be reflected in a new version of the document.

| **Version** | **Date**   | **Description of change**                                                                                                                                                                                                       | **Responsible**      |
|-------------|------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------|
| 1.0         | 10/07/2026 | Initial creation of the technical manual: general description, technologies, architecture, folder structure, database, modules, API and security of the current software version.                                                | Development team     |
| 1.0         | 10/07/2026 | Incorporation of the officially approved diagrams: ERD, class diagram, architecture and authentication sequence, plus the data dictionary.                                                                                      | Development team     |
| 1.1         | 10/13/2026 | Update to the final state: seven active modules, 29 tables, devices and MQTT broker, complete API, identity taken from the JWT, device key hashing, mobile application integrated with the API and current limitations.        | Development team     |
| 1.2         | 10/24/2026 | Update to the architecture in production (https://energy-monitor.tech): API gateway (8080) and security (8082), device (8084) and measurement (8085) microservices; backend as a modular monolith with home, alert, recommendation and notification (8083); internal REST `/internal/**` and RabbitMQ events with transactional outbox; repositories, variables, deployment and pending work (migration of the four remaining modules in progress). | Development team     |
| 1.3         | 10/25/2026 | Data dictionary: the push_device table is added (mobile app installations with push through the Expo relay); 30 documented tables. |

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

The system monitors the consumption of electrical energy in households: a measuring device (ESP32 with a PZEM-004T V3 sensor) sends its readings over MQTT, the measurement service stores them and computes statistics, the backend generates alerts and saving recommendations and sends notifications by email, through the browser and to the mobile app, and the web and mobile applications let users consult the information. The solution is made up of an API gateway, three microservices (security, devices and measurements), a modular backend (households, alerts, recommendations and notifications), a RabbitMQ message broker, an MQTT broker, a measuring device, a web application, an Android mobile application and a MySQL database. Everything has been in production at https://energy-monitor.tech since October 24, 2026.

This manual is aimed at developers, technical support staff, the evaluating instructor and, in general, anyone who needs to understand, extend or maintain the software. Its purpose is to describe how the system is built, not how it is used: the user manual covers day-to-day use, the installation manual covers start-up, and the administrator manual covers account and backup management.

The information recorded here was obtained directly from the source code of the project's repositories (API gateway, security, device and measurement services, backend, web, mobile and infrastructure), from the Liquibase changesets and from the documents of this final documentation, so that it corresponds to the final version of the delivered software.

| **Information source**            | **Location**                                                                                                                                      |
|-----------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------|
| API gateway                       | en-monitor-api-gateway (Java 21 + Spring Boot 4.0.8 + Spring Cloud Gateway Server Web MVC)                                                        |
| Microservices                     | en-monitor-security-service, en-monitor-device-service and en-monitor-measurement-service (Java 21 + Spring Boot 4.1.1)                         |
| Backend source code               | energy-monitor-backend (Java 21 + Spring Boot 4.1.1 + Spring Modulith): home, alert, recommendation and notification                             |
| Web application source code       | Energy-Monitor-application (React 19 + Vite 8)                                                                                                   |
| Mobile application source code    | Energy-Monitor-Mobile (Expo SDK 57 + React Native 0.86)                                                                                           |
| Device firmware                   | `firmware/` folder of energy-monitor-backend (ESP32 + PZEM-004T V3, firmware 1.3.8)                                                               |
| Infrastructure and MQTT broker    | energy-monitor-infra (Docker Compose, Mosquitto 2 with mosquitto-go-auth, RabbitMQ 4.1, MySQL 8.4, deployment)                                   |
| Requirements specification        | [`02-especificacion-requisitos`](../../02-especificacion-requisitos/EN/requirements-specification.md) (RF-xxx and RNF-xxx codes)                  |
| Software design                   | [`05-diseno-software`](../../05-diseno-software/EN/README.md): architectural model (05.1), data model (05.2), class model (05.3), patterns (05.4) and interfaces (05.5) |
| Data model                        | Liquibase changesets in `src/main/resources/db/changelog` of each service and [`05.2-data-model.md`](../../05-diseno-software/EN/05.2-data-model.md)             |

# 5. Objective

> To document the architecture, technologies, project structure, database, technical modules, API, security and maintenance recommendations of the "Electrical Consumption Monitor" system, so that a developer outside the team can understand the software, continue its development and support it without prior knowledge of the project.

- Describe the microservice architecture with an API gateway and the modular monolith that still accompanies it, the communication between services (internal REST and RabbitMQ events), the communication with the devices and the design patterns used in each component.

- Detail the exact technologies and versions used in the backend services, in the two front-ends and in the device.

- Explain the organization of the source code and the function of each folder.

- Document the data model: entity-relationship diagram, main tables and data dictionary.

- Relate each technical module to its files, tables and endpoints.

- Describe the services exposed by the REST API and the security mechanisms applied.

- Establish recommendations for updating, backing up and supporting the system.

# 6. Technical scope

The technical manual covers the following aspects of the system:

| **Covers**                                                                              | **Does not cover**                                                                                                   |
|-----------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------|
| General architecture, services, layers and design patterns of the backend and the front-ends. | Installation, start-up and production deployment procedures (covered by the installation manual).                    |
| Technologies, libraries and versions used, as well as their dependencies.               | Usage steps, screens and messages for the end user (covered by the user manual).                                     |
| Folder and file structure of the project's repositories.                                | Management of users, roles, administrative reports and backups (covered by the administrator manual).                |
| Database: ERD, tables, relationships and data dictionary.                               | Electrical design and wiring of the measuring device (described in the installation manual).                         |
| Technical modules of the backend services, web front and mobile front, with their repository, files and tables. | User stories, use cases and acceptance criteria (analysis documents).                                             |
| REST API: method, route, parameters, authentication and responses; gateway routes, internal communication between services and the MQTT contract. | Load testing.                                                                                                      |
| Security: authentication, authorization, encryption, validations and known limitations. | Costs, project plan and management reports.                                                                         |
| Version control and maintenance recommendations.                                        |                                                                                                                      |

**Documented version:** software in production at https://energy-monitor.tech and on the `main` branch of the repositories as of October 24, 2026 (commits in section 15). Work that remains pending, including the ongoing migration of the remaining backend modules to microservices, is described in sections 14.2 and 16.4.

# 7. General system description

## 7.1 What is the system?

"Electrical Consumption Monitor" is a web and mobile solution that lets users know how much energy their households consume, identify the devices that use the most energy, receive alerts when consumption exceeds the established limits and get recommendations to reduce spending and the electricity bill. Readings are taken by a measuring device attached to each appliance.

## 7.2 Main components

| **Component**        | **Type**                                  | **Function**                                                                                                                                                                                  |
|----------------------|-------------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| API gateway          | Server (Java 21 + Spring Cloud Gateway, port 8080) | Single entry point of the API: routes each request to the owning service, answers CORS, validates the token (except on the public endpoints), limits the public account POSTs and refuses `/internal/**`. |
| Security service     | Microservice (Java 21 + Spring Boot, port 8082) | Accounts, registration with email verification, sign-in (including Google), JWT RS256 and rotating refresh token, HttpOnly cookie sessions for the web, password recovery, profile, account deletion, RBAC, user administration and support contacts. |
| Device service       | Microservice (Java 21 + Spring Boot, port 8084) | Linking, editing, credentials and unlinking of devices, appliance catalog, MQTT broker authentication and ACL, MQTT commands and module connectivity. |
| Measurement service  | Microservice (Java 21 + Spring Boot, port 8085) | The only subscriber that stores telemetry; history, latest reading, statistics, household consumption and consumption levels; publishes each reading as `measurement.recorded`. |
| Backend              | Modular monolith (Java 21 + Spring Boot + Spring Modulith, port 8083) | Households, members and thresholds (`home`), alerts (`alert`), recommendations (`recommendation`) and notifications by email, Web Push, mobile push (Expo) and in-app administrator notices (`notification`). It does not connect to the MQTT broker. |
| Message broker       | RabbitMQ 4.1                              | Carries the events between services (accounts, households, devices and measurements) published with transactional outbox.                                                                    |
| Web application      | SPA (React 19 + Vite 8)                   | Query and management interface: sign-in, household panel, consumption, history, users, devices (Bluetooth linking), thresholds, alerts, recommendations and settings.                         |
| Mobile application   | Android app (Expo SDK 57)                 | Mobile interface connected to the REST API through the gateway: account, households, alerts, recommendations and notification preferences. Android only.                                     |
| MQTT broker          | Eclipse Mosquitto 2 + mosquitto-go-auth   | Receives the telemetry and status of the devices over MQTT 3.1.1 with TLS and authenticates each client against the device service.                                                           |
| Device               | ESP32 + PZEM-004T V3 sensor (firmware 1.3.8) | Measures voltage, current, active power and energy; publishes one reading every 60 seconds and its status every five minutes.                                                             |
| Database             | MySQL 8.4                                 | A single database (`energy_monitor`) shared by the services; each service owns its tables and applies only its changesets. Stores users, households, devices, measurements, alerts, recommendations and notifications. |
| Email                | SMTP (Mailpit locally)                    | Sends, from the backend, the verification and recovery codes requested by the security service and the alert and recommendation emails.                                                     |

## 7.3 System roles

| **Role**                     | **Description**                                                                                  | **How it is implemented**                                                                                                            |
|------------------------------|--------------------------------------------------------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------|
| Household owner (OWNER)      | Creates and manages a household: invites members, configures thresholds, manages devices, transfers ownership or deletes the household. | OWNER enum of the user_home table, with domain permissions in the home module (per-role strategy). Each household has exactly one OWNER. |
| Household member (MEMBER)    | Views the consumption, history, alerts and configuration of the household they were invited to.  | MEMBER enum of the user_home table. A user can be OWNER in one household and MEMBER in another.                                       |
| System user                  | Personal account with profile, language, theme and notification preferences.                     | user table with status ACTIVE / INACTIVE / BLOCKED.                                                                                  |
| Access control (RBAC)        | Global functional permissions grouped into system roles (codes such as METER.READ, ALERT.READ).  | system_role, permission, user_system_role and system_role_permission tables of the security service; checked on every request with CheckPermission (they do not travel in the JWT). The backend asks for them through `GET /internal/users/{id}/permissions` (30-second cache). |
| Administrator                | Global management of the system and accounts.                                                    | Global ADMIN role of the security service (USER.READ, USER.MANAGE and ROLE.MANAGE): user directory, status and role changes at `/api/v1/admin/users` and `/api/v1/admin/roles`; in-app notices in the backend (`/api/v1/admin/notifications`). The first administrator is granted with `make-admin.sh` of the energy-monitor-infra repository. ADMIN gives no access to any household. |

## 7.4 Implementation status by domain

The system is documented as it is built. The table relates each group of requirements of the [requirements specification](../../02-especificacion-requisitos/EN/requirements-specification.md) to the module that implements it.

| **Requirements group**                            | **Software module**                                | **Status**                                                                                                                                                                                       |
|---------------------------------------------------|----------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| RF-SEC – User and Profile Management              | en-monitor-security-service + auth and account (web and mobile) | Implemented: registration, email verification, sign-in (including Google), token renewal and sign-out, password recovery and change, profile, permissions, user administration and account deletion. |
| RF-HOM – Household Management                     | home (backend) + dashboard and DetailHome (web)    | Implemented: create household, access code, members, favorites, thresholds, household deletion and ownership transfer. Pending: device history by linking period (RF-HOM-11).                  |
| RF-DEV – Device Management                        | en-monitor-device-service + devices (web)          | Implemented: appliance catalog, Bluetooth linking, editing, new Wi-Fi credentials, unlinking, MQTT authorization and connectivity.                                                              |
| RF-MED – Measurements and Consumption             | en-monitor-measurement-service + Consumption (web) | Implemented: MQTT ingestion of telemetry, statistics, household consumption summary and history (the connection status is recorded by the device service).                                     |
| RF-ALE – Alerts                                   | alert (backend) + Notifications (web and mobile)   | Implemented: device linked, disconnection, high or critical consumption and daily or monthly limit alerts.                                                                                      |
| RF-REC – Saving Recommendations                   | recommendation (backend)                           | Implemented: five saving rules evaluated every hour.                                                                                                                                             |
| RF-NOT – Notifications                            | notification (backend)                             | Implemented: delivery by email, Web Push and mobile push (Expo) according to each user's preferences, and in-app notices to administrators.                                                     |
| RF-CLI – Web and mobile applications              | Web application and mobile application             | Web implemented. Mobile integrated with the API; deleting and transferring a household only exist on the web.                                                                                   |

# 8. Technologies used

The following table records the languages, frameworks, libraries, database tools, design tools and version control tools used in the project, with their version or usage detail.

| **Component**                  | **Technology**                           | **Version / detail**                                                                               |
|--------------------------------|------------------------------------------|----------------------------------------------------------------------------------------------------|
| Backend language               | Java                                     | Java 21 (LTS) in the five services                                                                 |
| Backend framework              | Spring Boot                              | Spring Boot 4.1.1 (Spring Framework 7) in security, device, measurement and backend; 4.0.8 in the gateway |
| API gateway                    | Spring Cloud Gateway Server Web MVC      | Spring Cloud 2025.1.3, Apache HttpClient 5 and Caffeine; routing, security, ratelimit and error packages |
| Architectural style            | Microservices + Spring Modulith + hexagonal architecture | 3 microservices (security, devices and measurements) behind the gateway; backend with spring-modulith 2.1.1 and 4 modules (home, alert, recommendation and notification) plus 3 proxy packages |
| Backend build                  | Maven Wrapper                            | mvnw / mvnw.cmd in each repository, spring-boot-maven-plugin                                       |
| Persistence                    | Spring Data JPA + Hibernate              | ddl-auto = validate (the schema is defined by Liquibase)                                           |
| Database migrations            | Liquibase                                | db/changelog with changesets and rollback in each service; shared DATABASECHANGELOG table          |
| Database engine                | MySQL                                    | MySQL 8.4, collation utf8mb4_unicode_ci, shared energy_monitor schema (ADR-003)                    |
| Authentication and authorization | Spring Security + OAuth2 Resource Server | JWT RS256 signed only by the security service (PEM private key); the gateway and the services validate it with the public key; BCrypt for passwords |
| Data validation                | Jakarta Bean Validation                  | @NotBlank, @Size, @NotNull, @Positive, @Pattern                                                    |
| API documentation              | springdoc-openapi                        | springdoc 3.1.1, Swagger UI and /v3/api-docs in each service                                       |
| Email and push                 | spring-boot-starter-mail + Web Push + Expo | SMTP with STARTTLS (Mailpit locally), only from the backend; browser push with VAPID keys and mobile app push with Expo |
| Messaging between services     | RabbitMQ 4.1 + Spring AMQP               | Exchanges per context, transactional outbox with a relay every second, durable queues with dead-letter |
| Internal communication         | REST `/internal/**`                      | X-Internal-Token header (INTERNAL_API_TOKEN) from loopback or the Docker network; Caffeine caches |
| Messaging with devices         | MQTT 3.1.1 with TLS                      | Eclipse Mosquitto 2 + mosquitto-go-auth; the device and measurement services subscribe as MQTT clients (Eclipse Paho) |
| Monitoring and health          | Spring Boot Actuator                     | /actuator/health and /actuator/info in each service                                                |
| Backend tests                  | JUnit 5 + Spring Boot Test               | backend 481, security service 660, measurement service 151, device service 130 and gateway 57 tests, all green (October 24, 2026; security, October 22) |
| Containers and deployment      | Docker Compose + Nginx + Certbot         | energy-monitor-infra repository: local, development and production modes; HTTPS with Let's Encrypt |
| Continuous integration and deployment | GitHub Actions                    | tests and build on every PR and push to dev and main; deployment from main with approval of the production environment |
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

The pom.xml of energy-monitor-backend declares as core dependencies spring-boot-starter-webmvc, spring-boot-starter-data-jpa, spring-boot-starter-security, spring-boot-starter-security-oauth2-resource-server, spring-boot-starter-validation, spring-boot-starter-liquibase, spring-boot-starter-mail, spring-boot-starter-actuator, spring-boot-starter-amqp, spring-modulith-starter-core (with its observability and actuator modules), springdoc-openapi-starter-webmvc-ui 3.1.1, caffeine, mysql-connector-j (runtime) and the set of test starters (\*-test, spring-boot-starter-amqp-test and spring-modulith-starter-test). It no longer includes an MQTT client.

The security, device and measurement microservices share the base (webmvc, data-jpa, security, security-oauth2-resource-server, validation, liquibase, actuator, amqp, springdoc and mysql-connector-j); the device and measurement services add spring-integration-mqtt with Eclipse Paho, and the measurement service, caffeine. The gateway declares spring-cloud-starter-gateway-server-webmvc, httpclient5, spring-boot-starter-security, spring-boot-starter-security-oauth2-resource-server, spring-boot-starter-actuator and caffeine.

## 8.2 Available scripts

| **Repository** | **Script**                      | **Function**                                                                       |
|----------------|---------------------------------|------------------------------------------------------------------------------------|
| gateway and services | mvnw spring-boot:run      | Builds and starts each service: gateway on 8080, security on 8082, backend on 8083, devices on 8084 and measurements on 8085. |
| gateway and services | mvnw test                 | Runs the repository's test suite (the services with a database require MySQL running). |
| gateway and services | mvnw package              | Generates the executable JAR in /target.                                           |
| infra          | docker compose up -d --build    | Starts the full local stack (MySQL, RabbitMQ, Mosquitto, the five Java services and the web). |
| infra          | docker compose -f docker-compose.yml -f docker-compose.dev.yml up -d mysql rabbitmq mosquitto | Development mode: only MySQL, RabbitMQ and Mosquitto in Docker; the services run with ./mvnw. |
| infra          | e2e/device-flow.sh and e2e/measurement-flow.sh | End-to-end tests on the local stack.                               |
| web            | npm run dev                     | Starts the Vite development server at http://localhost:5173.                       |
| web            | npm run build / npm run preview | Builds the application in /dist and serves it for review.                          |
| web            | npm test                        | Runs the tests with Vitest.                                                        |
| web            | npm run lint                    | Analyzes the code with ESLint.                                                     |
| mobile         | npm run start                   | Starts the Expo Metro Bundler.                                                     |
| mobile         | npm run android                 | Runs the application on an Android emulator or device.                             |
| mobile         | npm run lint                    | Analyzes the code with ESLint (there are no automated tests).                      |
| firmware (backend/firmware) | arduino-cli compile / upload | Builds and uploads the firmware over USB (no over-the-air, OTA, update).    |

# 9. System architecture

## 9.1 General architecture

The system adopts a layered client-server architecture, with the logic split into microservices behind an API gateway and an additional channel for the devices. The presentation layer is made up of the web application and the mobile application, which only call the gateway; the logic layer is formed by three microservices (security, devices and measurements) and the backend, which is still a modular monolith with the home, alert, recommendation and notification contexts; the services communicate through internal REST (`/internal/**`) and RabbitMQ events; the data layer is a single shared MySQL database, accessed through JPA/Hibernate and managed with Liquibase, in which each service owns its tables; and the measuring devices publish their readings over MQTT with TLS to a Mosquitto broker that queries the device service to authenticate them. The migration of the four modules that remain in the backend to microservices is in progress (section 16.4).

> **Images and diagrams (Figures 1 to 11):** all the figures of this document are in the following Google Drive folder.  
> https://drive.google.com/drive/folders/1tIzih2FKP8xte4vKYEtrWThFmZxR1oBX?usp=sharing
>
> **Figures possibly out of date (`[TO BE CONFIRMED]` after reviewing them in the folder):** Figures 3 and 5 to 8 (ERD) were drawn before the latest tables and columns and do not include `notification_preference`, `push_subscription`, `user.google_subject`, `home_thresholds.limit_period` or `device.api_key_hash`; Figures 4 and 9 to 11 (class diagram) may still show the per-role strategy classes and the device and recommendation modules as pending; and Figures 1 and 2 (architecture and authentication sequence) may show the X-User-Id header, a system without devices or broker, or the backend as a single application, without API gateway or microservices; the current architecture is the one described by the tables of this section.

**Figure 1.** *General system architecture (three layers: presentation, logic and data).*

| **Layer**        | **Technology**                                        | **Responsibility**                                                                                                   |
|------------------|-------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------|
| Presentation     | React 19 (web) and Expo SDK 57 (mobile, Android)      | User interface, form validation, session handling and consumption of the REST API through the gateway.              |
| Entry            | Spring Cloud Gateway (en-monitor-api-gateway, 8080)   | Routing by path prefix, CORS, token validation, request limiting and refusal of `/internal/**`.                      |
| Logic            | Spring Boot 4.1.1 + Spring Security; Spring Modulith in the backend | Security, device and measurement microservices and modular backend: authentication, authorization, use cases, business rules, MQTT ingestion and event publishing. |
| Messaging        | RabbitMQ 4.1 + transactional outbox                   | Events between services with at-least-once delivery and dead-letter queues.                                         |
| Data             | MySQL 8.4 + Spring Data JPA + Liquibase               | A single shared database; each service owns its tables and applies only its changesets; no foreign keys between services. |
| Devices          | ESP32 + PZEM-004T V3 + Mosquitto 2                    | Measurement, MQTT publishing with TLS (port 8883) and authentication of each device against the device service.     |
| Cross-cutting    | Docker Compose (energy-monitor-infra), Nginx + Certbot, GitHub Actions, Mailpit and Actuator | Containers, HTTPS in production, continuous integration and deployment, test email and health of each service. |

**Backend services.** All in Java 21; Spring Boot 4.1.1 except the gateway (4.0.8). The microservices follow hexagonal architecture (ports and adapters) with the `domain`, `application`, `adapter`, `infrastructure` and `api` layers; the gateway has no domain (`routing`, `security`, `ratelimit` and `error` packages).

| **Service (repository)**         | **Port**   | **Responsibility**                                                                                                                                                        |
|----------------------------------|------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| en-monitor-api-gateway           | 8080       | Single entry point of the API (Spring Cloud Gateway Server Web MVC, Spring Cloud 2025.1.3). Answers CORS, validates the token (RS256, issuer and expiry) except on the public endpoints, reads the token from the web's HttpOnly `em_access` cookie (`AccessTokenCookieFilter`), limits the public account POSTs (20 per IP and path per minute, then 429), refuses `/internal/**`, answers 503 when the service does not answer and 504 when it takes more than 30 seconds. |
| en-monitor-security-service      | 8082       | Accounts, registration with email verification, sign-in, Google, JWT RS256 (15-minute access token) and rotating refresh token, HttpOnly cookie sessions for the web, password recovery, profile, account deletion, RBAC and user administration, support contacts (`GET /api/v1/support/contacts`, public). |
| en-monitor-device-service        | 8084       | Devices: linking, editing, credential reissue (API key stored as a SHA-256 hash), unlinking, appliance catalog, MQTT broker authentication and ACL (Mosquitto go-auth asks it), MQTT commands to the modules and connectivity (subscribed to telemetry and status; offline after 150 seconds without data). |
| en-monitor-measurement-service   | 8085       | Measurements: the only subscriber that stores telemetry (MQTT), history, latest reading, statistics, household consumption (summary and history) and consumption levels; publishes each reading as `measurement.recorded` and offers an internal API to the backend. |
| energy-monitor-backend           | 8083       | Modular monolith (Spring Modulith): households, members and thresholds (`home`), alerts (`alert`), recommendations (`recommendation`), notifications by email, Web Push (VAPID), mobile push (Expo) and in-app administrator notices (`notification`), plus the `security`, `device` and `measurement` proxy packages. It no longer connects to the MQTT broker. |

## 9.2 Architectural style and design patterns

- **Microservices behind an API gateway.** The security, device and measurement contexts were extracted from the backend into their own services (en-monitor-security-service, en-monitor-device-service and en-monitor-measurement-service), each with its repository, its deployment and its tables. The gateway (en-monitor-api-gateway) is the single entry point of the API and routes by path prefix, most specific first; the clients call the same routes as always, so the migration did not change the public API.

- **Modular monolith (Spring Modulith) in the backend.** energy-monitor-backend is still a single application with four bounded contexts (home, alert, recommendation and notification) and three proxy packages (security, device and measurement) that serve the `api` contract the other modules already used, with no domain or tables: they call the internal API of the corresponding service (`adapter/out/remote`, cached) and turn its RabbitMQ events into in-memory events (`adapter/in/messaging`). Each module declares its allowed dependencies in the package-info.java file and can only communicate with the others through the types of its api package; this restriction is verified automatically by the ModularityTests test.

- **Hexagonal architecture per module and per service.** Inside each backend module and each microservice live the api, domain, application (inbound ports `port/in`, outbound ports `port/out`, use cases `usecase`), adapter (inbound `adapter/in/web`, `adapter/in/internal`, `adapter/in/messaging`, `adapter/in/mqtt`, `adapter/in/event`, `adapter/in/scheduler` and outbound `adapter/out/persistence`, `out/messaging`, `out/event`, `out/client` or `out/remote`) and infrastructure layers with the Spring configuration.

- **Events between services with RabbitMQ and transactional outbox.** What a service must tell another is written to its outbox table in the same transaction as the change; a relay publishes it every second with broker confirmation. Delivery is at least once, consumers tolerate duplicates and every queue is durable, with dead-letter after three attempts (details in section 9.3).

- **In-process events inside the backend (no Kafka).** The backend modules communicate through in-memory Spring events: `AlertRaised` (alert → notification), `RecommendationCreated` (recommendation → notification), `MeasurementRecorded` and `DeviceConnectivityLost` / `DeviceConnectivityRestored` (republished by the measurement and device proxies when the RabbitMQ message arrives → alert) and `HomeOwnershipTransferred` (home, no consumer yet). A device's loss of connectivity is detected by the device service (last will message or 150 seconds of silence) and published as `device.connectivity-lost`.

- **Shared database (ADR-003).** All the services use the same MySQL database; each one owns its tables and applies only its Liquibase changesets. There are no foreign keys between services: other contexts are referenced by identifier.

- **Recommendations through a scheduler.** The recommendation module does not listen to `MeasurementRecorded`: `RecommendationScheduler` runs the saving rules every hour with the hourly energy it asks the measurement service for, and publishes `RecommendationCreated` for each new recommendation.

- **API without server-side HTTP session.** The services keep no HTTP session: each request carries a JWT. The mobile app sends it in the Authorization header; the web receives it in HttpOnly cookies (`em_access` and `em_refresh`) that the gateway turns into `Authorization: Bearer`. The CORS policy is resolved only in the gateway, explicitly and closed by default.

- **Feature-based front-end.** Both the web and the mobile app organize code in folders by functional capability (auth, dashboard, homes, notifications…) with their own components, styles, zod validations and tests. Global state is handled with React Context; the web keeps in localStorage only the non-secret session data (account, profile, permissions and session id), never the tokens.

## 9.3 Communication between components

The client applications communicate only with the API gateway through HTTP requests in JSON format under the /api/v1 prefix; in production Nginx terminates HTTPS at https://energy-monitor.tech and the requests to /api/ reach the gateway through the web container's nginx. The gateway validates the token and forwards each request to the service that owns the route. The mobile app sends the header Authorization: Bearer \<accessToken\>; the web receives the tokens in HttpOnly cookies and the gateway turns the `em_access` cookie into that header (only when the request carries `X-Requested-With: XMLHttpRequest`). The user's identity is obtained exclusively from the `sub` claim of the JWT through `CurrentUserResolver` in each service; the X-User-Id header is ignored. The web front automatically renews the access token on a 401 response through POST /auth/refresh, with refresh token rotation and locking between tabs using the Web Locks API.

| **Communication element**    | **Detail**                                                                                       |
|------------------------------|--------------------------------------------------------------------------------------------------|
| Protocol (clients)           | HTTP/1.1 over HTTPS in production (Nginx + Let's Encrypt); HTTP in local development.            |
| Format                       | JSON (Content-Type: application/json).                                                           |
| Entry point                  | API gateway: https://energy-monitor.tech/api/v1 in production, http://localhost:8080/api/v1 in development. |
| API prefix                   | /api/v1 (all public routes; the `/internal/**` routes never go through the gateway).             |
| Authentication               | Authorization: Bearer \<JWT RS256\> with 15-minute expiration (mobile) or HttpOnly `em_access` cookie (web). The gateway and each service validate signature, issuer and expiry. |
| Identity                     | `sub` claim of the JWT; X-User-Id is ignored.                                                    |
| Allowed origin (CORS)        | Only in the gateway, which drops the Origin header before forwarding. Closed by default; origins are defined with the gateway's SECURITY_CORS_ALLOWED_ORIGINS (e.g. http://localhost:5173), without credentials. Methods: GET, POST, PUT, DELETE and OPTIONS. |
| Error handling               | Response with ApiError structure: timestamp, status, error, message, path and fieldErrors. The gateway's own answers (401, 404, 429, 503 and 504) use the same shape. |
| Documentation                | OpenAPI and Swagger UI at /v3/api-docs and /swagger-ui.html of each service (in development mode, on ports 8082, 8083, 8084 and 8085). |
| Service health               | GET /actuator/health of each service returns {"status":"UP"} (database and RabbitMQ included).   |
| Internal communication       | REST `/internal/**` between services with the X-Internal-Token header, and RabbitMQ 4.1 events (AMQP, port 5672) with transactional outbox. |
| Devices (MQTT)               | MQTT 3.1.1; port 8883 with TLS (devices, `mqtt.energy-monitor.tech:8883` in production) and 1883 only inside Docker (device and measurement services). |

**API gateway routes.** The device and measurement routes are evaluated before the backend's general route, so `/api/v1/homes/{homeId}`, its members and its thresholds stay in the backend.

| **Routes**                                                                                             | **Service**                              |
|--------------------------------------------------------------------------------------------------------|------------------------------------------|
| `/api/v1/auth/**`, `/api/v1/admin/users/**`, `/api/v1/admin/roles/**`, `/api/v1/support/**`             | en-monitor-security-service (8082)       |
| `/api/v1/homes/{homeId}/devices/**`, `/api/v1/appliance-types/**`                                      | en-monitor-device-service (8084)         |
| `/api/v1/measurements/**`, `/api/v1/consumption-levels/**`, `/api/v1/homes/{homeId}/consumption/**`    | en-monitor-measurement-service (8085)    |
| Rest of `/api/v1/**` (households, members, thresholds, alerts, recommendations, notifications and administrator notices) | energy-monitor-backend (8083) |
| `/internal/**`                                                                                         | Never forwarded (401 without a token, 403 with one) |
| Any other route                                                                                        | 404                                      |

**Internal REST between services.** The `/internal/**` routes are only accepted from loopback or the Docker network (`172.16.0.0/12`) **and** with the `X-Internal-Token` header (value of `INTERNAL_API_TOKEN`, the same in all the services), compared in constant time; with no token configured every call is refused. The broker authentication (`/internal/mqtt/**`) only requires the allowed network.

| **Caller**             | **Endpoint**                                                                                                   | **Purpose**                                                                          |
|------------------------|----------------------------------------------------------------------------------------------------------------|--------------------------------------------------------------------------------------|
| backend → security     | `GET /internal/users/{id}/permissions`, `POST /internal/users/profiles`, `GET /internal/administrators`         | Permissions, profiles and administrators (cached).                                   |
| security → backend     | `GET /internal/accounts/{id}/deletion-check`                                                                   | May the account be deleted? (204 yes; 409 no).                                       |
| backend → device       | `GET /internal/devices/{id}`, `GET /internal/homes/{homeId}/devices`                                           | Read devices (30-second cache; previous copy up to 1 hour when it does not answer).  |
| device → backend       | `GET /internal/homes/{homeId}/access?userId=`                                                                  | Is the user a member or the owner of the household?                                  |
| measurement → backend  | `GET /internal/homes/{homeId}/access?userId=`, `GET /internal/homes/{homeId}/thresholds`                       | Household access and limits.                                                         |
| measurement → device   | `GET /internal/devices/{id}`, `GET /internal/homes/{homeId}/devices`                                           | Household of a module and modules of a household (cached).                           |
| backend → measurement  | `GET /internal/consumption-levels`, `GET /internal/measurements/hourly-energy`, `GET /internal/measurements/reporting-devices` | Classify readings (levels cached 10 minutes) and hourly energy for the limit alerts and the recommendations. |
| Mosquitto → device     | `POST /internal/mqtt/auth/user`, `POST /internal/mqtt/auth/acl`                                                | Authenticate and authorize each MQTT client.                                         |

**RabbitMQ events.** Each producer writes the message to its outbox table in the same transaction as the change; a relay publishes it every second, oldest first, and marks it as published only when the broker confirms it. Delivery is at least once and the queues are durable, with dead-letter after three attempts.

| **Exchange / routing key**                                                                                         | **Outbox (producer)**              | **Consumer**                                     | **Effect**                                                                                       |
|--------------------------------------------------------------------------------------------------------------------|------------------------------------|--------------------------------------------------|--------------------------------------------------------------------------------------------------|
| `energy-monitor.security` / `account.registered`, `account.administered`, `account.deleted`, `mail.email-verification`, `mail.password-reset` | `security_outbox` (security) | backend | In-app notices to the administrators, cleanup of deleted accounts and sending of emails (only the backend sends email). |
| `energy-monitor.home` / `home.deleted`                                                                             | `home_outbox` (backend)            | device                                           | Unlinks the household's modules and sends them the `unlink` command.                             |
| `energy-monitor.device` / `device.linked`, `device.unlinked`, `device.connectivity-lost`, `device.connectivity-restored` | `device_outbox` (device)      | backend and measurement (linked and unlinked only) | Backend: CONNECTIVITY alerts and device cache; measurement: device cache.                       |
| `energy-monitor.measurement` / `measurement.recorded`                                                              | `measurement_outbox` (measurement; delivered rows are purged after 7 days) | backend               | Republished in memory as `MeasurementRecorded`: threshold, limit and linked-device alerts.       |

Inside the backend the following remain in memory (Spring events): `AlertRaised` and `RecommendationCreated` (→ notification), `MeasurementRecorded` and `DeviceConnectivityLost` / `DeviceConnectivityRestored` (republished by the proxies → alert) and `HomeOwnershipTransferred`.

**MQTT contract.**

| **Parameter**             | **Value**                                                                                                                              |
|---------------------------|----------------------------------------------------------------------------------------------------------------------------------------|
| Client ID                 | `esp32-{deviceId}`                                                                                                                     |
| Username / password       | `device_code` / `api_key` (the device service only stores the hash of the key)                                                          |
| Keep alive and last will  | Keep alive of 15 seconds; the broker publishes the last will about 22 seconds after a power cut                                        |
| QoS                       | 0 for telemetry and status; 1 for the command                                                                                          |
| Cadence                   | Telemetry every 60 seconds; status every five minutes and on connect; offline after 150 seconds of silence                             |
| Telemetry topic           | `energy-monitor/devices/{deviceId}/telemetry`                                                                                          |
| Status topic              | `energy-monitor/devices/{deviceId}/status` (retained)                                                                                  |
| Command topic             | `energy-monitor/devices/{deviceId}/command` (QoS 1, retained): `{"cmd":"unlink"}` when the device is unlinked                          |

The broker (`iegomez/mosquitto-go-auth` image) stores no users: it authenticates each client against the device service (`/internal/mqtt/auth/user`) with a five-minute cache, and its ACL lets each device use only its own topics (`/internal/mqtt/auth/acl`). There are three MQTT clients: the device service (`MQTT_DEVICE_SERVICE_USER`: reads telemetry and status and is the only one that publishes commands), the measurement service (`MQTT_MEASUREMENT_SERVICE_USER`: reads telemetry only) and each ESP32 module (`device_code` and API key). The backend no longer connects to the broker. The services take the `deviceId` from the topic and drop messages whose body declares another device.

The following figure describes the full flow of a sign-in and the subsequent access to a protected resource.

**Figure 2.** *Authentication sequence: sign-in, protected access and token renewal.*

# 10. Project structure

The project is distributed across eight code repositories (API gateway, three microservices, backend, web front-end, mobile app and infrastructure) and the documentation repositories. The following table summarizes their content and how to run them.

| **Repository**             | **Type**                           | **Technology**                                          | **Entry point**                                                      | **Execution**                                  |
|----------------------------|------------------------------------|---------------------------------------------------------|----------------------------------------------------------------------|------------------------------------------------|
| en-monitor-api-gateway     | API gateway                        | Java 21 + Spring Boot 4.0.8 + Spring Cloud Gateway + Maven | src/main/java/com/energymonitor/gateway/ApiGatewayApplication.java | ./mvnw spring-boot:run (port 8080)             |
| en-monitor-security-service | Security microservice             | Java 21 + Spring Boot 4.1.1 + Maven                     | src/main/java/com/energymonitor/security/SecurityServiceApplication.java | ./mvnw spring-boot:run (port 8082)         |
| en-monitor-device-service  | Device microservice                | Java 21 + Spring Boot 4.1.1 + Maven                     | src/main/java/com/energymonitor/device/DeviceServiceApplication.java | ./mvnw spring-boot:run (port 8084)             |
| en-monitor-measurement-service | Measurement microservice       | Java 21 + Spring Boot 4.1.1 + Maven                     | src/main/java/com/energymonitor/measurement/MeasurementServiceApplication.java | ./mvnw spring-boot:run (port 8085)     |
| energy-monitor-backend     | Backend (modular monolith) and firmware | Java 21 + Spring Boot 4.1.1 + Spring Modulith + Maven; Arduino firmware | src/main/java/com/energymonitor/EnergyMonitorBackendApplication.java and firmware/esp32-pzem-monitor/esp32-pzem-monitor.ino | ./mvnw spring-boot:run (port 8083) / arduino-cli upload |
| Energy-Monitor-application | Web application (front-end)        | React 19 + Vite 8 (JavaScript)                          | src/main.jsx over index.html                                         | npm install / npm run dev (port 5173)          |
| Energy-Monitor-Mobile      | Mobile application (front-end)     | Expo SDK 57 + React Native 0.86                         | src/app/\_layout.tsx (expo-router/entry)                             | npm install / npm run start                    |
| energy-monitor-infra       | Infrastructure and deployment      | Docker Compose, Mosquitto 2, RabbitMQ 4.1, MySQL 8.4, Nginx | docker-compose.yml, docker-compose.dev.yml and docker-compose.prod.yml | docker compose up -d --build              |
| energy-monitor-docs-final  | Final documentation                | Markdown (ES and EN)                                    | README.md                                                            | Does not need to be run                        |

The legacy energy-monitor-devices repository (old firmware and broker) is no longer used to run anything: the firmware lives in `firmware/` of energy-monitor-backend and the broker in energy-monitor-infra.

## 10.1 Backend structure

```
energy-monitor-backend/
├── pom.xml                       Maven definition and dependencies
├── mvnw / mvnw.cmd               Maven Wrapper (Maven does not need to be installed)
├── Dockerfile                    backend image (Temurin 21, unprivileged user)
├── .env.example                  environment variable template
├── README.md                     backend instructions
├── docs/Architecture.md          modules, proxies and migration status
├── firmware/esp32-pzem-monitor/  ESP32 + PZEM-004T V3 firmware
└── src/
    ├── main/
    │   ├── java/com/energymonitor/
    │   │   ├── EnergyMonitorBackendApplication.java   entry point
    │   │   ├── home/             households, members, thresholds, deletion and transfer
    │   │   ├── alert/            consumption, limit, device and connectivity alerts
    │   │   ├── recommendation/   saving rules and recommendation inbox
    │   │   ├── notification/     email, Web Push, mobile push, preferences and administrator notices
    │   │   ├── security/         proxy of the security service (no domain or tables)
    │   │   ├── device/           proxy of the device service (no domain or tables)
    │   │   └── measurement/      proxy of the measurement service (no domain or tables)
    │   └── resources/
    │       ├── application.yaml                  base configuration
    │       ├── application-local.yaml(.example)  local profile and keys
    │       └── db/changelog/                     Liquibase master + changesets
    │           ├── db.changelog-master.xml
    │           └── home/ alert/ recommendation/ notification/
    └── test/java/com/energymonitor/              96 test files (includes ModularityTests)
```

**Microservices and gateway.** Each microservice keeps the package it had in the backend and its changesets under the same relative path, so Liquibase recognizes them as already applied:

```
en-monitor-security-service/     com.energymonitor.security     db/changelog/security/
en-monitor-device-service/       com.energymonitor.device       db/changelog/device/
en-monitor-measurement-service/  com.energymonitor.measurement  db/changelog/measurement/
en-monitor-api-gateway/          com.energymonitor.gateway      (no database)
    each with pom.xml, mvnw, Dockerfile, .env.example, README.md
    and .github/workflows/ci-cd.yml (docs/Architecture.md in gateway, security and device)
```

**Backend proxies (example with device):** `api/` (contract the other modules already used: DeviceApi, DeviceConnectivityLost, DeviceConnectivityRestored), `adapter/out/remote/` (HTTP client of the service's internal API, cached) and `adapter/in/messaging/` (consumer of the RabbitMQ events), plus `infrastructure/` with the configuration.

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
│   ├── in/internal/         internal API for other services (access, limits, account deletion)
│   ├── in/messaging/        account.deleted consumer (RabbitMQ)
│   ├── out/messaging/       home.deleted outbox, relay and RabbitMQ publisher
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
    │   ├── auth/         authApi, googleSignIn, session (localStorage, no tokens)
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
| \<service\>/src/main/java/com/energymonitor/\<module\>/domain/model        | Pure domain entities, with no Spring dependencies.                                       |
| \<service\>/src/main/java/com/energymonitor/\<module\>/application         | Use cases, inbound and outbound ports, commands, results and exceptions.                 |
| \<service\>/src/main/java/com/energymonitor/\<module\>/adapter/in/web      | REST controllers, DTOs and exception handlers (@RestControllerAdvice).                   |
| \<service\>/src/main/java/com/energymonitor/\<module\>/adapter/in/internal | Internal `/internal/**` API for the other services.                                      |
| \<service\>/src/main/java/com/energymonitor/\<module\>/adapter/in/messaging and out/messaging | RabbitMQ consumers; event outbox, relay and publisher.                   |
| \<service\>/src/main/java/com/energymonitor/\<module\>/adapter/out/persistence | JPA entities, repositories, mappers and persistence adapters.                        |
| energy-monitor-backend/src/main/java/com/energymonitor/{security,device,measurement} | Microservice proxies: api contract, remote client and consumers.          |
| \<service\>/src/main/resources/db/changelog                                | Liquibase changesets of that service's tables, with their rollback.                      |
| \<service\>/src/test/java                                                  | Unit, web integration and security tests; in the backend, module boundary verification.  |
| en-monitor-api-gateway/src/main/java/com/energymonitor/gateway             | Routes (routing), token validation and CORS (security), request limiting (ratelimit) and errors (error). |
| web/src/routes                                                             | Definition of routes, guards and layouts of the SPA.                                     |
| web/src/services                                                           | API access layer: HTTP client, interceptors, session and endpoints per domain.           |
| web/src/features                                                           | Screens and components grouped by functionality.                                         |
| web/src/design                                                             | Reusable interface components (buttons, cards, fields) and their styles.                 |
| mobile/src/app                                                             | File-based Expo Router routes that mount each screen.                                    |
| mobile/src/services                                                        | REST API client of the mobile application (same pattern as the web).                     |
| mobile/src/features                                                        | Screens, components, hooks and validations per functionality.                            |
| energy-monitor-backend/firmware/esp32-pzem-monitor                         | Device firmware and configuration files.                                                 |
| infra/docker-compose\*.yml and infra/mosquitto                             | Local stack, development mode and production (MySQL, RabbitMQ, Mosquitto, services and web) and broker configuration. |
| infra/deploy and infra/e2e                                                 | Deployment (deploy.sh, Nginx, MySQL backup, MQTT certificate renewal) and end-to-end tests. |

# 11. Database

**Engine:** MySQL 8.4, run in a Docker container (energy-monitor-infra repository) with collation utf8mb4_unicode_ci.

**Schema:** energy_monitor, a single database shared by the services (ADR-003). This chapter documents 30 business tables distributed across seven contexts. The application user is energy_app.

**Owner of each context:** each service owns its tables and applies only its changesets: the security service, those of the Security context; the device service, those of Devices; the measurement service, those of Monitoring and measurements; and energy-monitor-backend, those of Home Management, Alerts, Recommendations and Notifications.

**Schema management:** Liquibase applies the versioned changesets in src/main/resources/db/changelog of each service; all of them share the DATABASECHANGELOG table, and the changesets that left the backend kept their path, so they are not run again. Hibernate is configured with ddl-auto = validate, that is, it only checks that the schema matches the entities and never creates tables by itself.

**Conventions:** snake_case names; VARCHAR(10) primary keys; audit columns created_at, updated_at and deleted_at in all tables; soft delete (no physical DELETE by default); indexes with the idx\_\[table\]\_\[columns\] pattern. References between contexts (and therefore between services) are stored as application-level identifiers and do not generate SQL foreign keys.

## 11.1 Entity-relationship diagram

The following diagram shows the system entities, grouped by context (Security, Home Management, Devices, Monitoring and measurements, Alerts, Recommendations and Notifications), with their attributes, keys and relationships. Dotted relationships between contexts represent application references (no SQL foreign key). The diagram published in the figures folder shows 27 entities: `notification_preference` and `push_subscription` and several later columns are missing (see the note in section 9.1).

> **Reading recommendation:** because of its size, the diagram is presented in landscape; in Word it can be enlarged with the zoom to read the attributes of each entity. Appendix B includes the same model split by context, with better legibility.
>
**Figure 3.** *Complete entity-relationship diagram (ERD) of the "Electrical Consumption Monitor" system.*

## 11.2 Main tables

The following table lists the 30 documented business tables, the context they belong to (and, with it, the owning service stated in the introduction of this section), their description, their primary key and the relationships declared in the data dictionary.

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
| PushDevice             | Notifications               | Mobile app installations that receive push through the Expo relay, one row per installation. | id_push_device | PushDevice N:1 User (application reference) |

## 11.3 Data dictionary

The data dictionary describes each table of the schema field by field: key constraint, data type, size and description. It was rebuilt from the services' Liquibase changesets, which are the source of truth of the schema; for the data model with its integrity rules see also [`05.2-data-model.md`](../../05-diseno-software/EN/05.2-data-model.md).

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

### PushDevice — Notifications

*Mobile app installations that receive push notifications through the Expo relay (one row per installation). It is not another kind of PushSubscription: a relay token carries no endpoint or encryption keys.*

| **Field**      | **Type** | **Size** | **Constraint**    | **Description**                                                         |
|----------------|----------|----------|-------------------|-------------------------------------------------------------------------|
| id_push_device | VARCHAR  | 10       | PK / NOT NULL     | Installation identifier                                                 |
| user_id        | VARCHAR  | 10       | NOT NULL          | User who owns the installation (indexed)                                |
| token          | VARCHAR  | 255      | UNIQUE / NOT NULL | Push token of the installation; registering it again changes its owner |
| platform       | VARCHAR  | 10       | NOT NULL          | Platform of the installation (the published app is Android: ANDROID)    |

**Relationships:** PushDevice N:1 User (application reference)


# 12. Technical modules

## 12.1 Overview of the modules

The system's backend keeps its seven modules (bounded contexts), all of them active, but no longer in a single application: security, device and measurement run as microservices, and home, alert, recommendation and notification remain in energy-monitor-backend, which also keeps the security, device and measurement proxy packages (no domain or tables). The migration of the four modules that remain in the backend to microservices is in progress (section 16.4). The API gateway completes the table as a technical component without domain.

| **Module**     | **Responsibility**                                                                     | **Controllers / use cases**                                                                                               | **Status** |
|----------------|----------------------------------------------------------------------------------------|---------------------------------------------------------------------------------------------------------------------------|------------|
| gateway (en-monitor-api-gateway, 8080) | Routing, CORS, token validation, request limiting and its own errors     | GatewayRoutes, SecurityFilterChainConfiguration, AccessTokenCookieFilter, AuthRateLimitFilter and GatewayExceptionHandler | Active     |
| security (en-monitor-security-service, 8082) | Registration, authentication (including Google), sessions, passwords, profile, RBAC, user administration, audit and account deletion | AuthController, AdminController, SupportController, InternalController + use cases | Active     |
| home (energy-monitor-backend, 8083) | Households, members, favorites, access code, thresholds, household deletion and ownership transfer | HomeController, HomeThresholdsController, HomeTypeController, internal controllers + use cases | Active     |
| device (en-monitor-device-service, 8084) | Devices, linking, credentials, connection status, commands and broker authorization | HomeDeviceController, ApplianceTypeController, BrokerAuthController, InternalDeviceController + 5 services | Active     |
| measurement (en-monitor-measurement-service, 8085) | MQTT ingestion of telemetry, history, statistics and household consumption | MeasurementController, ConsumptionLevelController, HomeConsumptionController, InternalMeasurementController + MQTT consumer | Active     |
| alert (energy-monitor-backend, 8083) | Consumption, limit, device linked and connectivity alerts, and their inbox    | AlertController + 10 use cases                                                                                            | Active     |
| recommendation (energy-monitor-backend, 8083) | Saving rules evaluated every hour and recommendation inbox            | RecommendationController, RecommendationScheduler + 2 services                                                            | Active     |
| notification (energy-monitor-backend, 8083) | Delivery by email, Web Push and mobile push, preferences, delivery log and administrator notices | NotificationChannelsController, AdminNotificationsController + RabbitMQ consumers + use cases | Active     |

## 12.2 Domain class diagram

The class diagram models the classes, interfaces and enumerations of the domain, together with the design patterns applied: Strategy for role-based authorization (OwnerPermissions and MemberPermissions), Observer (named events) for the reaction to measurements and Factory Method for creating alerts, recommendations and notifications. The patterns are described in [`05.4-design-patterns.md`](../../05-diseno-software/EN/05.4-design-patterns.md) and the classes in [`05.3-class-model.md`](../../05-diseno-software/EN/05.3-class-model.md).

> **Note:** like the ERD, the complete diagram is presented in landscape. Appendix C splits it by domain to make it easier to read. The published figure may still carry the note that the Strategy pattern classes do not exist in the code; today they do (`[TO BE CONFIRMED]` when reviewing the figure).
>
**Figure 4.** *Domain class diagram, including the Strategy, Observer and Factory Method patterns.*

## 12.3 Backend modules by context

Each subsection states the repository the module runs in; file paths are relative to `src/main/java/com/energymonitor/` of that repository.

### 12.3.1 security module

**Function:** Identity, authentication, sessions, access control, user administration, audit and account deletion.

**Status:** Active

**Location:** en-monitor-security-service (port 8082). energy-monitor-backend keeps the `security` proxy (api: UserProfileQuery, AdministratorQuery; adapter/out/remote: RemotePermissionCheck, RemoteUserProfileQuery, RemoteAdministratorQuery and SecurityServiceClient), with no domain or tables; the backend only validates the tokens with the public key.

**Related files:**

- security/adapter/in/web/AuthController.java (authentication, profile and account), AdminController.java (`/api/v1/admin/users` and `/api/v1/admin/roles`) and SupportController.java (support contacts)

- security/adapter/in/web/internal/InternalController.java (internal API: permissions, profiles and administrators)

- security/adapter/in/web/dto/ (DTOs: RegisterRequest, LoginRequest, GoogleLoginRequest, DeleteAccountRequest, ProfileResponse, ApiError…)

- security/application/usecase/ (RegisterUserService, AuthenticateUserService, SignInWithGoogleService, RefreshSessionService, LogoutUserSessionService, ChangePasswordService, CreatePasswordResetTokenService, ResetPasswordService, EmailVerificationService, UpdateUserProfileService, DeleteAccountService, CheckPermissionService, AssignRoleService, RevokeRoleService, ManageUserStatusService…)

- security/domain/model/ (User, Person, Email, PasswordHash, PasswordPolicy, UserSession, RefreshToken, SystemRole, Permission, AuditLog, LoginErrorLog)

- security/infrastructure/ (SecurityFilterChainConfiguration, JwtTokenIssuer, PemRsaKeys, SecurityCorsConfiguration, JwtAuthenticationEntryPoint)

- security/adapter/out/security/ (BCryptPasswordHasherAdapter, Sha256TokenHasher, PepperedHmacResetCodeHasher, InMemoryResetAttemptLimiter)

- security/adapter/out/messaging/ (OutboxWriter, OutboxRelay, RabbitOutboxPublisher: account events and mail requests) and security/adapter/out/backend/HttpAccountDeletionGuard.java (deletion check against the backend)

**Related tables:** person, user, user_configuration, password_reset_token, user_session, refresh_token, security_configuration, password_policy, login_error_log, system_role, permission, user_system_role, system_role_permission, audit_log and security_outbox.

**Main operations:**

- Register an account and verify the email

- Sign in (email and password or Google account) and renew tokens

- Sign out

- Recover and change passwords

- View and update the profile

- Query permissions (RBAC) and administer users: directory, status change and granting or revoking global roles

- Publish the support contacts (active administrators)

- Delete the account (validates the password, asks the backend through `/internal/accounts/{id}/deletion-check`, closes the sessions, logically deletes personal data, releases the email and publishes `account.deleted` over RabbitMQ)

- Ask the backend, over RabbitMQ, to send the verification and recovery emails (`mail.*`)

- Record audit and failed attempts

### 12.3.2 home module

**Function:** Management of households, members, favorites, consumption thresholds, household deletion and ownership transfer.

**Status:** Active

**Location:** energy-monitor-backend (port 8083). Its migration to a microservice is in progress (section 16.4).

**Related files:**

- home/adapter/in/web/HomeController.java, HomeThresholdsController.java, HomeTypeController.java

- home/adapter/in/web/CurrentUserResolver.java (resolves the user from the JWT `sub` claim)

- home/adapter/in/internal/ (HomeAccessController, HomeLimitsController and AccountDeletionCheckController: internal API for the device, measurement and security services)

- home/adapter/in/messaging/AccountDeletedConsumer.java (consumes `account.deleted`) and home/adapter/out/messaging/ (HomeOutboxWriter, HomeOutboxRelay, RabbitHomeOutboxPublisher: publishes `home.deleted`)

- home/application/usecase/ (CreateHomeService, JoinHomeService, LeaveHomeService, ListHomesService, ListMembersService, RemoveUserService, ToggleFavoriteService, DeleteHomeService, TransferHomeOwnershipService, HomeRemoval, RemoveDeletedAccountService, CheckAccountDeletionService, DescribeHomeAccessService, DescribeHomeLimitsService, GetHomeThresholdsService, UpdateHomeThresholdsService, ListHomeTypesService, GetHomeTypeService)

- home/domain/model/ (Home, HomeType, HomeThresholds, UserHome, Role OWNER/MEMBER, OwnerPermissions, MemberPermissions)

- home/api/ (HomeApi, HomeDeleted, HomeOwnershipTransferred) and home/infrastructure/HomeApiImpl.java (public API consumed by other modules)

**Related tables:** home, home_type, home_thresholds, user_home and home_outbox.

**Main operations:**

- Create a household and generate an access code

- Join a household with a code

- List households and members

- Mark a favorite

- Expel members and leave the household (the last owner cannot leave)

- View, update and reset daily and monthly thresholds

- Delete the household (OWNER only; soft delete of `home`, `user_home` and `home_thresholds`; in the same transaction it writes `home.deleted` to `home_outbox`, and the relay publishes it to RabbitMQ; the device service consumes it, unlinks the modules, revokes their keys and sends them the MQTT unlink command). The same behavior applies when `account.deleted` arrives for an account that was the sole owner of a household with no other members.

- Answer the internal API: a user's access to a household, the household's limits and whether an account may be deleted (409 if it is the sole owner of a household with other members)

- Transfer ownership (OWNER only; atomic role swap with a row lock; publishes `HomeOwnershipTransferred`, which no module consumes yet)

### 12.3.3 measurement module

**Function:** Ingestion and analysis of electrical consumption measurements.

**Status:** Active

**Location:** en-monitor-measurement-service (port 8085). energy-monitor-backend keeps the `measurement` proxy (api: MeasurementApi, MeasurementRecorded, RiskConsumption; adapter/out/remote: RemoteMeasurementApi and MeasurementServiceClient, with consumption levels cached 10 minutes; adapter/in/messaging/MeasurementRecordedConsumer), with no domain or tables.

**Related files:**

- measurement/adapter/in/web/MeasurementController.java, ConsumptionLevelController.java, HomeConsumptionController.java

- measurement/adapter/in/internal/InternalMeasurementController.java (internal API for the backend: levels, hourly energy and reporting devices)

- measurement/adapter/in/mqtt/ (TelemetryMqttConsumer, MqttTopics, MqttIngestionConfiguration; the subscription is enabled with `MEASUREMENT_MQTT_ENABLED`)

- measurement/adapter/in/messaging/DeviceLinksConsumer.java (consumes `device.linked` and `device.unlinked` to clear its cache)

- measurement/application/usecase/ (RegisterMeasurementService, ListMeasurementsService, GetLatestMeasurementService, GetConsumptionStatisticsService, HomeConsumptionUseCase, HourlyEnergyCalculator, ListConsumptionLevelsService, TimeRange)

- measurement/domain/model/ (Measurement, ConsumptionLevel)

- measurement/adapter/out/event/MeasurementEventAdapter.java and measurement/adapter/out/messaging/ (MeasurementOutboxWriter, MeasurementOutboxRelay, RabbitMeasurementOutboxPublisher, MeasurementOutboxPurge: publish `measurement.recorded`)

- measurement/adapter/out/client/ (BackendHomeClient, DeviceServiceClient, CachedDeviceDirectory: household access and limits, and devices)

**Related tables:** measurement, consumption_level and measurement_outbox (connection status is recorded in device_status_log, of the device service).

**Main operations:**

- Record the readings that arrive over MQTT (voltage, current, active power, accumulated energy); it is the only subscriber that stores them

- Query history by device and date range, and the latest measurement

- Compute statistics, and the household consumption summary and history

- Publish each reading as `measurement.recorded` on RabbitMQ (the backend republishes it in memory as MeasurementRecorded)

- Answer the backend's internal API: consumption levels, hourly energy and reporting devices

### 12.3.4 alert module

**Function:** Generation and inbox of consumption, limit, device linked and connectivity alerts.

**Status:** Active

**Location:** energy-monitor-backend (port 8083). Its migration to a microservice is in progress (section 16.4).

**Related files:**

- alert/adapter/in/web/AlertController.java (query, read and delete)

- alert/adapter/in/event/MeasurementRecordedListener.java and DeviceConnectivityLostListener.java (@EventListener on the in-memory events republished by the measurement and device proxies)

- alert/application/usecase/ (ListAlertsService, GetAlertService, ResolveAlertService, DeleteAlertsService, RegisterThresholdAlertService, RegisterLimitAlertService, RegisterConnectivityAlertService, RegisterDeviceLinkedAlertService, ResolveConnectivityAlertsService, AlertAccessService)

- alert/infrastructure/AlertApiImpl.java and alert/api/AlertRaised.java

**Related tables:** alert.

**Main operations:**

- Generate high or critical consumption alerts when a measurement is received

- Generate daily or monthly limit alerts and device linked alerts (with the first reading after linking)

- Generate and resolve connectivity alerts (150 seconds of silence or last will message)

- List alerts by household and status, view one, mark it as read and delete resolved or read ones

### 12.3.5 notification module

**Function:** Delivery of alerts and recommendations by email, Web Push and mobile push, preferences, delivery log and in-app notices to administrators.

**Status:** Active

**Location:** energy-monitor-backend (port 8083). Its migration to a microservice is in progress (section 16.4).

**Related files:**

- notification/adapter/in/web/NotificationChannelsController.java and AdminNotificationsController.java (`/api/v1/admin/notifications`)

- notification/adapter/in/event/AlertRaisedListener.java and RecommendationCreatedListener.java

- notification/adapter/in/messaging/AccountEventsConsumer.java and MailRequestsConsumer.java (`account.*` events and `mail.*` requests of the security service)

- notification/infrastructure/mail/EmailSender.java, MailDeliveryConfiguration.java, AbandonedDeliverySweeper.java and notification/adapter/out/push/WebPushSender.java and ExpoPushSender.java

- notification/application/usecase/ (NotifyAlert, SendPasswordResetEmail, SendEmailVerification, ManageNotificationChannels, AlertMessages, AdminNotifications)

- notification/adapter/out/persistence/ (persistence adapters, entities and repositories)

**Related tables:** notification, notification_preference, push_subscription and push_device.

**Main operations:**

- Send the verification and recovery emails requested by the security service (only the backend sends email)

- Deliver each alert and recommendation to the household members by email, Web Push and mobile push (Expo) according to their preferences

- Record in-app notices for the administrators when an account registers or another account is administered (`account.registered`, `account.administered`)

- Record the delivery status (PENDING, SENT, FAILED) without storing the message content

- Sweep abandoned deliveries when the application starts and every five minutes

- Manage preferences, browser push subscriptions and mobile app devices

### 12.3.6 device module

**Function:** Registration and linking of devices, credentials (API key), connection status, commands and authorization of the MQTT broker.

**Status:** Active

**Location:** en-monitor-device-service (port 8084). energy-monitor-backend keeps the `device` proxy (api: DeviceApi, DeviceConnectivityLost, DeviceConnectivityRestored; adapter/out/remote: RemoteDeviceApi and DeviceServiceClient, with a 30-second cache and a previous copy up to 1 hour; adapter/in/messaging/DeviceEventsConsumer), with no domain or tables.

**Related files:**

- device/adapter/in/web/HomeDeviceController.java, ApplianceTypeController.java and BrokerAuthController.java

- device/adapter/in/internal/InternalDeviceController.java (internal API for the backend and the measurement service)

- device/application/usecase/ (HomeDeviceService, BrokerAccessService, DeviceConnectivityService, DeviceReportService, DeviceQueryService)

- device/domain/model/ (Device, DeviceHome, DeviceStatusLog, ApplianceType, ApiKeyHash)

- device/adapter/in/mqtt/ConnectivityMqttConsumer.java (telemetry and status subscription), device/adapter/in/messaging/HomeDeletedConsumer.java (consumes `home.deleted`), device/adapter/in/event/DeviceCommandListener.java and device/adapter/in/scheduler/DeviceInactivityScheduler.java

- device/adapter/out/ (mqtt/MqttDeviceCommandAdapter, messaging/ with DeviceOutboxRelay and RabbitDeviceOutboxPublisher, home/ to ask the backend about access, persistence and credential generators)

**Related tables:** device, appliance_type, device_status_log, device_home and device_outbox.

**Main operations:**

- Link a device to a household (OWNER only), edit it, issue new credentials to change its Wi-Fi and unlink it (member or OWNER permission is asked to the backend)

- Authenticate the device and authorize its topics for Mosquitto (only the SHA-256 hash of the key is stored)

- Record connectivity from the telemetry and the retained status, and mark the device offline after 150 seconds of silence (`DeviceInactivityScheduler`)

- Publish `device.linked`, `device.unlinked`, `device.connectivity-lost` and `device.connectivity-restored` on RabbitMQ

- Send the MQTT unlink command when unlinking the device or on receiving `home.deleted`

### 12.3.7 recommendation module

**Function:** Saving recommendations generated from the measured consumption.

**Status:** Active

**Location:** energy-monitor-backend (port 8083). Its migration to a microservice is in progress (section 16.4).

**Related files:**

- recommendation/adapter/in/web/RecommendationController.java

- recommendation/adapter/in/scheduler/RecommendationScheduler.java (every hour)

- recommendation/application/usecase/ (EvaluateRecommendationsService, RecommendationInboxService)

- recommendation/adapter/out/client/ (HomeDeviceReaderAdapter and MeasurementConsumptionReaderAdapter: read the household limits through the home api, and the devices and hourly energy through the device and measurement proxies)

- recommendation/adapter/out/event/RecommendationEventAdapter.java (publishes RecommendationCreated)

**Related tables:** recommendation.

**Main operations:**

- Evaluate five saving rules every hour (peak hours, standby consumption, weekly comparison, limit projection and per-device increase), without repeating the same recommendation within 7 days

- List a household's recommendations, mark them as read and delete them

## 12.4 Web front-end module

**Function:** electrical consumption query and management interface (React SPA)

**Related files:**

- src/routes/AppRoutes.jsx (routes), ProtectedRoute.jsx (protected routes) and HomeRoute.jsx (household detail)

- src/services/http/httpClient.js (axios client towards the gateway, interceptors, HttpOnly cookie session with `X-Session-Transport: cookie` and token renewal)

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

**Base URL:** https://energy-monitor.tech/api/v1 in production and http://localhost:8080/api/v1 in development (API gateway) \| **Format:** JSON \| **Interactive documentation:** /swagger-ui.html and /v3/api-docs of each service

The public API totals 66 business routes under `/api/v1`, all through the gateway, which forwards them to the owning service according to the "API gateway routes" table of section 9.3 (auth, admin/users, admin/roles and support: security; devices and appliance catalog: device; measurements, levels and household consumption: measurement; the rest: backend). The migration did not change the routes the clients already used. In addition there are two internal routes for the broker and the infrastructure routes. All those not listed as public require a valid JWT (Authorization header or the web's `em_access` cookie); if the token is missing, expired or invalid, the gateway answers 401 without forwarding the request, and if the user lacks the required permission, the service answers 403.

| **Method** | **Route**                                             | **Description**                                                       | **Parameters**                                                         | **Authentication** |
|------------|-------------------------------------------------------|-----------------------------------------------------------------------|------------------------------------------------------------------------|--------------------|
| POST       | /api/v1/auth/register                                 | Registers a new account.                                              | body: email, password, name, lastName, profileImage (optional)         | Public             |
| POST       | /api/v1/auth/email/verify                             | Verifies the email with a 6-digit code.                               | body: email, code                                                      | Public             |
| POST       | /api/v1/auth/email/verification/resend                | Resends the verification code (always answers 202).                   | body: email                                                            | Public             |
| POST       | /api/v1/auth/login                                    | Authenticates and returns access token and refresh token.             | body: email, password                                                  | Public             |
| POST       | /api/v1/auth/google                                   | Signs in or registers the account with Google (503 if not configured). | body: code                                                            | Public             |
| POST       | /api/v1/auth/refresh                                  | Renews the access token and rotates the refresh token.                | body: refreshToken (mobile) or em_refresh cookie (web)                 | Public             |
| POST       | /api/v1/auth/logout                                   | Closes the registered session.                                        | body: idUserSession                                                    | JWT                |
| POST       | /api/v1/auth/password/forgot                          | Requests the password recovery code (202; 404 if the email has no account). | body: email                                                      | Public             |
| POST       | /api/v1/auth/password/reset                           | Resets the password with the received code.                           | body: email, resetToken, newPassword                                   | Public             |
| POST       | /api/v1/auth/password/change                          | Changes the password knowing the current one.                         | body: currentPassword, newPassword                                     | JWT                |
| PUT        | /api/v1/auth/profile                                  | Updates name, last name, image or email.                              | body: name, lastName, profileImage, newEmail                           | JWT                |
| POST       | /api/v1/auth/account/delete                           | Deletes the account after confirming the password.                    | body: password                                                         | JWT                |
| GET        | /api/v1/auth/account                                  | Returns the authenticated account.                                    | none (identity from the JWT)                                           | JWT                |
| GET        | /api/v1/auth/permissions                              | Lists the user's permission codes.                                    | none                                                                   | JWT                |
| GET        | /api/v1/admin/users                                   | User directory (requires USER.READ).                                  | query: q, page, size (1 to 50; default 20)                             | JWT                |
| GET        | /api/v1/admin/roles                                   | Lists the assignable global roles (requires USER.READ).               | none                                                                   | JWT                |
| PUT        | /api/v1/admin/users/{idUser}/status                   | Changes another account's status (requires USER.MANAGE; 409 on one's own). | path: idUser; body: status (ACTIVE, INACTIVE or BLOCKED)          | JWT                |
| PUT        | /api/v1/admin/users/{idUser}/roles/{roleName}         | Grants a global role (requires ROLE.MANAGE).                          | path: idUser, roleName                                                 | JWT                |
| DELETE     | /api/v1/admin/users/{idUser}/roles/{roleName}         | Revokes a global role (requires ROLE.MANAGE).                         | path: idUser, roleName                                                 | JWT                |
| GET        | /api/v1/support/contacts                              | Name and email of every active administrator, for the support page.   | none                                                                   | Public             |
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
| POST       | /api/v1/notifications/push/devices                    | Registers the mobile app installation for push (Expo).                | body: token, platform (ANDROID or IOS)                                 | JWT                |
| POST       | /api/v1/notifications/push/devices/remove             | Removes the push registration of an app installation.                 | body: token                                                            | JWT                |
| GET        | /api/v1/notifications/push/devices/available          | Tells whether app push is configured (404 when it is not).            | none                                                                   | JWT                |
| GET        | /api/v1/admin/notifications                           | Latest 50 in-app notices of the administrator (requires USER.READ).   | none                                                                   | JWT                |
| PUT        | /api/v1/admin/notifications/{id}/read                 | Marks an administrator notice as read.                                | path: id                                                               | JWT                |
| DELETE     | /api/v1/admin/notifications/{id}                      | Deletes an administrator notice.                                      | path: id                                                               | JWT                |
| POST       | /internal/mqtt/auth/user                              | Internal (device service): Mosquitto authenticates an MQTT client (five-minute cache). The gateway refuses it. | body: username, password, clientid | Internal network |
| POST       | /internal/mqtt/auth/acl                               | Internal (device service): Mosquitto authorizes a client's topic. The gateway refuses it. | body: username, clientid, topic, acc         | Internal network   |
| GET        | /actuator/health                                      | Status of each service (UP / DOWN).                                   | none                                                                   | Public             |
| GET        | /swagger-ui.html and /v3/api-docs                     | Interactive API documentation of each service (OpenAPI).              | none                                                                   | Public             |
| OPTIONS    | /\*\*                                                 | CORS preflight (answered by the gateway).                             | Origin and Access-Control-Request-\* headers                           | Public             |

> *The `/internal/mqtt/**` routes only accept calls from the networks in `MQTT_AUTH_ALLOWED_NETWORKS`; the other `/internal/**` routes (section 9.3) also require the `X-Internal-Token` header. The gateway never forwards them (401 without a token, 403 with one) and the web's nginx only forwards `/api/`.*

## 13.1 Common response codes

| **Code**        | **When it occurs**                                                            | **Response structure**                                         |
|-----------------|-------------------------------------------------------------------------------|----------------------------------------------------------------|
| 200 / 201 / 204 | Successful operation (query, creation or deletion with no content).           | Body with the corresponding resource or message.               |
| 400             | Incorrect data validation (Bean Validation or zod schema in the client).      | ApiError with fieldErrors detailing each field.                |
| 401             | Missing or expired token or invalid credentials (the gateway answers it before forwarding). | ApiError with generic message (does not reveal whether the email exists). |
| 403             | Blocked or inactive account, or insufficient permission (e.g. a MEMBER tries to delete the household). | ApiError.                          |
| 404             | Nonexistent resource (household, alert, member…), household the user does not belong to or route the gateway does not route. | ApiError or {"error": "..."} depending on the module. |
| 409             | Conflict (email already registered, last owner who cannot leave, transfer to the current owner). | ApiError.                    |
| 422             | The password does not meet the security policy.                               | ApiError.                                                      |
| 429             | Attempt limit reached in verification or recovery (security service) or more than 20 public account POSTs per IP and path in one minute (gateway). | ApiError with Retry-After header (900 seconds in recovery). |
| 503             | A service refuses the connection (gateway), an internal service the operation depends on does not answer, or Google sign-in without configured credentials. | ApiError. |
| 504             | A service takes more than 30 seconds to answer (gateway).                     | ApiError.                                                      |
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

This is the answer the mobile app receives. With the `X-Session-Transport: cookie` header (the one the web sends) the security service delivers the tokens in the HttpOnly `em_access` and `em_refresh` cookies, and `accessToken` and `refreshToken` come back `null`.

# 14. Security

## 14.1 Implemented controls

| **Control**                   | **Technical implementation**                                                                                                                                                                            |
|-------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Authentication                | JWT signed with the RS256 algorithm by the security service, the only one holding the private key (PEM); the gateway and the other services validate it with the public key (signature, issuer and expiry), so a service reached without going through the gateway is not open either. Access token lifetime: 15 minutes; no roles inside the JWT; claims sub (idUser), person_id and email. |
| Identity                      | All services and modules obtain the user from the `sub` claim through `CurrentUserResolver`; the X-User-Id header is ignored.                                                                           |
| Sessions                      | No server-side HTTP session (STATELESS). The mobile app receives the tokens in the body; the web, in HttpOnly and SameSite=Strict cookies (Secure in production): `em_access` (Path=/api) and `em_refresh` (Path=/api/v1/auth). The gateway only uses the cookie when the request carries `X-Requested-With: XMLHttpRequest`, a header that a form or an image from another site cannot send (CSRF protection). |
| Session renewal               | Opaque, stateful refresh token rotated on every use, stored as a SHA-256 hash with family_id and parent_id to detect reuse (replay) and revoked when the password changes.                              |
| Password hashing              | BCrypt (BCryptPasswordHasherAdapter) isolated from the domain; passwords are never stored in clear.                                                                                                     |
| Password policy               | password_policy table: minimum length 8 and maximum 64, requires uppercase, numbers and symbols, expires after 90 days. Non-compliance =\> 422.                                                         |
| Global authorization (RBAC)   | Chain of five tables (system_role, permission, user_system_role, system_role_permission and user) queried live by `CheckPermission` in the security service; permissions can be queried at GET /api/v1/auth/permissions. The backend asks for the permissions through the internal API, with a 30-second cache. The gateway decides no permission. |
| Per-household authorization   | OWNER and MEMBER roles per household resolved with the Strategy pattern (`OwnerPermissions`, `MemberPermissions`).                                                                                       |
| Input validation              | Bean Validation in the backend DTOs (@NotBlank, @Size, @NotNull, @Positive, @Pattern) and zod schemas in the front-end forms.                                                                           |
| CORS                          | Only in the gateway, which drops the Origin header before forwarding. Closed by default: explicit list of origins (the gateway's SECURITY_CORS_ALLOWED_ORIGINS), no wildcards, no credentials and with OPTIONS preflight handling; methods GET, POST, PUT, DELETE and OPTIONS. |
| Attempt limit                 | In-memory limiters: in the gateway, 20 POSTs per IP and path per minute on the public account endpoints (sign-in, registration, Google, refresh, recovery and verification) =\> 429 with Retry-After; in the security service, 10 attempts per IP every 15 minutes in password recovery =\> 429 with Retry-After header. |
| Enumeration protection        | Failed login with a generic message; the unverified-email check runs after the password. By documented decision, password/forgot answers 404 when the email has no account (as registration answers 409 when it already exists), and the per-IP limit is charged before the lookup. |
| Recovery codes                | 6 digits, 15-minute validity, single use, stored as HMAC-SHA256 with pepper (SECURITY_RESET_PEPPER).                                                                                                    |
| Device credentials            | Each device uses `device_code` and `api_key`; the device service only stores the SHA-256 hash of the key (`api_key_hash`). Mosquitto authenticates the device against the device service and the ACL limits its topics. |
| Device transport              | MQTT 3.1.1 with TLS on port 8883; port 1883 only exists inside the Docker network and the `/internal/mqtt/**` routes are only accepted from allowed networks.                                          |
| Internal communication        | The `/internal/**` routes require loopback or the Docker network (`172.16.0.0/12`) and the `X-Internal-Token` header (`INTERNAL_API_TOKEN`), compared in constant time; with no token configured every call is refused. The gateway never forwards them and in Docker only the web (80), MQTT with TLS (8883) and, on 127.0.0.1, the administration ports are published. |
| Audit                         | audit_log table (actions, IP, user) and login_error_log (failed attempts). The gateway appends the client's IP to X-Forwarded-For and the security service only takes it from that header when the connection comes from loopback or the Docker network; any other client is recorded by its own address. |
| Documentation                 | The OpenAPI endpoint of each service declares the bearerAuth scheme; the public endpoints are only the authentication ones, `GET /api/v1/support/contacts` and the infrastructure ones.                  |
| Transport                     | In production it is served over HTTPS (Nginx + Certbot with Let's Encrypt) at https://energy-monitor.tech, and MQTT with TLS at mqtt.energy-monitor.tech:8883; in local development HTTP is used with Mailpit as the test email server. |

## 14.2 Known limitations and risks

> The following points are declared by the team itself and must be taken into account before extending the system.

| **Limitation**                                                                                 | **Impact**                                                                            | **Recommendation**                                                                  |
|------------------------------------------------------------------------------------------------|---------------------------------------------------------------------------------------|-------------------------------------------------------------------------------------|
| Telemetry and status with MQTT QoS 0.                                                          | A reading can be lost (no duplicates).                                                | Evaluate an MQTT client with QoS 1 (for example espMqttClient).                     |
| Mosquitto depends on the device service to authenticate each client.                           | If the service goes down, new MQTT logins are refused and connected modules only publish while the cached ACL decisions last (5 minutes). | Monitor the device service's health; the deployment waits until every service is `healthy`. |
| In-memory attempt limiters (gateway and password recovery) and outbox relays designed for a single instance. | They reset on every start and are not shared between instances; a second instance could reorder messages. | Use Redis or another shared store if scaling to several instances. |
| No over-the-air (OTA) firmware update.                                                         | Updating the firmware requires a USB cable.                                           | Evaluate OTA in a future version.                                                   |
| Persistence tests are not hermetic.                                                            | They need the infrastructure repository's MySQL running and SPRING_DATASOURCE_PASSWORD. | Use a database container for tests.                                               |
| The access token remains valid for up to 15 minutes after account deletion.                    | Brief window of residual access.                                                      | Accepted; documented.                                                               |
| Edge cases of connectivity alerts (CONNECTIVITY) without automatic resolution.                 | Some alerts need manual review.                                                       | Define the automatic resolution rule.                                               |
| Web Bluetooth limited to Chrome, Edge and Chrome for Android, and to HTTPS or localhost.       | On other browsers the Wi-Fi configuration portal is used; on iOS there is no push without installing the web. | Keep the `EnergyMonitor-Setup` portal as an alternative.             |
| `HomeOwnershipTransferred` has no consumer.                                                    | The new owner is not notified when receiving ownership.                               | Consume the event and send email or push.                                           |
| A device's history is not limited to the linking period.                                       | A household can see measurements from a previous period of another household.         | Limit the history to the linking period (RF-HOM-11).                                |
| Household deletion and transfer absent from the mobile application; no automated tests.       | They can only be done from the web.                                                   | Implement them in the Android application and add tests.                            |

## 14.3 Best practices for developers

- Never commit the .env files (of each service and of infra), application-local.yaml or the RSA keys (\*.pem); they are already declared in .gitignore. Only the security service receives the private key; the other services receive only `public.pem`.

- Do not log passwords, tokens or recovery codes in the application logs.

- Keep the dependencies of the backend services and the front-end up to date and review their security advisories.

- Every new feature must validate input in the service that serves it, regardless of client-side validations.

- Respect module boundaries: do not import classes from another context without going through its api package (verified by ModularityTests in the backend). Between services, communicate only through the internal REST and the RabbitMQ events; never import another service's entities or repositories or create foreign keys between services.

- A new public endpoint in the security service must also be added to the gateway's list of public endpoints (`SecurityFilterChainConfiguration`).

- Rotate the JWT keys, the recovery pepper and the `INTERNAL_API_TOKEN` (in all services at once) if a leak is suspected.

# 15. Software version control

The project uses Git with a remote repository on GitHub. Each component of the system is developed in an independent repository to ease its deployment and review.

| **Repository**             | **Content**                                                      | **Remarks**                                                                               |
|----------------------------|------------------------------------------------------------------|-------------------------------------------------------------------------------------------|
| en-monitor-api-gateway     | API gateway in Java 21 / Spring Boot 4.0.8 / Spring Cloud Gateway. | https://github.com/Juan-Cortes2045/en-monitor-api-gateway; `main` at `c070cec` (10/24/2026). |
| en-monitor-security-service | Security microservice in Java 21 / Spring Boot 4.1.1.           | https://github.com/Juan-Cortes2045/en-monitor-security-service; `main` at `334691a`. Includes its security-\* changesets. |
| en-monitor-device-service  | Device microservice in Java 21 / Spring Boot 4.1.1.              | https://github.com/Juan-Cortes2045/en-monitor-device-service; `main` at `f8b8526`. Includes its device-\* changesets. |
| en-monitor-measurement-service | Measurement microservice in Java 21 / Spring Boot 4.1.1.     | https://github.com/Juan-Cortes2045/en-monitor-measurement-service; `main` at `ee2cb81`. Includes its measurement-\* changesets. |
| energy-monitor-backend     | Modular backend (home, alert, recommendation and notification) in Java 21 / Spring Boot 4.1.1 and the ESP32 firmware. | https://github.com/Juan-Cortes2045/energy-monitor-backend; `main` at `aa9a713`. Includes tests, Dockerfile, .env.example, its Liquibase migrations and the firmware/ folder. |
| Energy-Monitor-application | React 19 / Vite 8 web application.                               | https://github.com/Juan-Cortes2045/Energy-Monitor-application; `main` at `e59bece`. Includes Vitest tests (14 files) and production build in /dist. |
| Energy-Monitor-Mobile      | Expo SDK 57 mobile application.                                  | https://github.com/angelgarcia0/Energy-Monitor-Mobile; `main` at `894b65a`. Includes LICENSE, AGENTS.md (versioned Expo 57 documentation) and Expo scripts. |
| energy-monitor-infra       | Docker Compose (local, development and production), Mosquitto, Nginx and deployment. | https://github.com/Juan-Cortes2045/energy-monitor-infra; `main` at `eaf961b`. Includes deploy.sh and the end-to-end tests (e2e/). |
| energy-monitor-docs-final  | Bilingual final documentation (ES and EN).                       | Project charter, requirements specification, technical proposal, design and manuals. Other documentation repositories: en-monitor-docs and Energy-Monitor-Documentation. |
| energy-monitor-devices     | Old firmware and broker (legacy).                                | No longer used to run anything; the firmware is in energy-monitor-backend and the broker in energy-monitor-infra. |

- All repositories have the `main` (production) and `dev` (integration) branches. Work branches follow the `[type]/[kebab-case-name]` convention (feat/*, fix/*, chore/*) and commits follow Conventional Commits with the message in English.

- Code is integrated through a pull request to `dev` and then from `dev` to `main`.

- Each code repository has a GitHub Actions workflow that runs the tests and the build on every pull request and push to `dev` and `main`; after a push to `main` and the approval of the `production` environment, it deploys over SSH with `infra/deploy/deploy.sh backend|security|device|measurement|gateway|frontend|infra` (section 16.1).

- Before merging changes, the test suite of the affected repository (./mvnw test in each service) and the web front tests (npm test) must be run.

- Database schema changes are made only through a new Liquibase changeset with rollback; already applied changesets are never edited.

- Integration branches must keep the code compilable and the tests green.

- Delivered versions are tagged to preserve the exact reference of the software documented in this manual.

# 16. Maintenance recommendations

## 16.1 Code updates

| **Area**      | **Recommendation**                                                                                                                                                                |
|---------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Backend       | Build with ./mvnw clean test (.\mvnw.cmd on Windows) in each service repository before each delivery; in energy-monitor-backend, verify that ModularityTests confirms the module boundaries were not broken. |
| Database      | Create a new changeset for each schema change in the repository of the service that owns the table, with a sequential identifier of its module and its rollback; start MySQL from energy-monitor-infra (development mode) to test against a real MySQL 8.4. |
| Dependencies  | Update Spring Boot, Spring Cloud, Expo or React in test branches, running the full suite before merging (the gateway stays on Spring Boot 4.0 because Spring Cloud 2025.1 is the release that supports it). |
| Web front     | Keep `VITE_API_BASE_URL` pointing at the gateway and the allowed origins list (the gateway's SECURITY_CORS_ALLOWED_ORIGINS); rerun npm test and npm run lint.                     |
| Mobile front  | Review the versioned Expo 57 documentation before adding native libraries; there are no automated tests, so verification is manual.                                               |
| Devices       | Rebuild and upload the firmware over USB after changing config.h or the certificate; renew the broker certificate and restart the `mosquitto` container (in production the Certbot hook `deploy/renew-mqtt-cert.sh` does it). |
| Deployment    | Deploy from `main` with GitHub Actions and `infra/deploy/deploy.sh <component>`: it backs up MySQL, updates and rebuilds only that service, waits until the five Java services are `healthy` and the web answers, and rolls back only if it fails. Before that, run `infra/e2e/device-flow.sh` and `infra/e2e/measurement-flow.sh` on the local stack. |
| Security      | Review the RBAC permissions, the password policy and the validity of the JWT keys quarterly.                                                                                      |

## 16.2 Database backups

Before each relevant update, the energy_monitor schema, shared by all the services, must be backed up. In production, `deploy.sh` backs up MySQL before each deployment and `deploy/backup-mysql.sh` makes a daily logical backup. Recommended manual procedure:

1.  Stop writing information (do not run measurement loads during the backup).

2.  Run the dump: mysqldump -h localhost -u energy_app -p energy_monitor \> backup_energy_monitor_YYYY-MM-DD.sql

3.  Save the file in a safe folder outside the repository, with the date in the name.

4.  Verify the backup by restoring it in a test database before considering it valid.

5.  Restore: mysql -h localhost -u energy_app -p energy_monitor \< backup_energy_monitor_YYYY-MM-DD.sql

## 16.3 Support and incident resolution

| **Symptom**                             | **Suggested check**                                                                                                                  |
|-----------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------|
| A service does not start                | Check its port (8080, 8082, 8083, 8084 or 8085), the MySQL and RabbitMQ containers (docker compose ps in infra) and the SPRING_DATASOURCE_PASSWORD variable. Without RabbitMQ, /actuator/health answers DOWN. |
| The API answers 403/401 massively       | Verify that each service's SECURITY_JWT_PUBLIC_KEY_PATH is the public key of the security service's pair, and the server date and time. |
| The gateway answers 503 or 504          | The service that owns the route is not running or takes more than 30 seconds; check its health with docker compose ps.               |
| Internal calls fail (503, missing names) | Verify that INTERNAL_API_TOKEN has the same value in all the services and that the called service is running.                       |
| The web front does not connect to the API | Confirm that the gateway answers on port 8080 and that the gateway's SECURITY_CORS_ALLOWED_ORIGINS includes the origin http://localhost:5173. |
| Emails do not arrive                    | Check the backend's MAIL_HOST, MAIL_PORT, MAIL_USERNAME and MAIL_PASSWORD and that RabbitMQ is running; locally use Mailpit (http://localhost:8025). |
| A device does not send readings         | Check the broker (docker compose logs mosquitto), that the device service is running (the broker asks it about every client), DEVICE_MQTT_PUBLIC_HOST (never localhost), the TLS certificate and the ESP32 serial output. |
| Readings arrive but no alerts are raised | Check that the measurement service answers, the INTERNAL_API_TOKEN and the `energy-monitor-backend.measurement.recorded` queue (or its `.dlq`) in RabbitMQ. |
| Migrations fail on startup              | Check the Liquibase log and the order of the changesets in the db.changelog-master.xml of the failing service.                       |
| Error messages are not readable         | Check the browser console and the ApiError body returned by the API.                                                                 |

## 16.4 Declared pending work

- **Pending work: migration of home, alert, recommendation and notification (in progress by the teammate).** The migration to microservices of the four modules that remain in energy-monitor-backend is being carried out by the project teammate, Miguel Ángel García Artunduaga. It is in progress; its services, ports and dates are not yet defined, so this manual documents those modules as they work today inside the backend.

- Limit a device's history to the linking period (RF-HOM-11).

- Consume `HomeOwnershipTransferred` to notify the household's new owner.

- Implement household deletion and transfer in the mobile application and complete its automated tests.

- Evaluate MQTT with QoS 1 so that readings are not lost.

- Automatically resolve the edge cases of connectivity alerts.

- Administrator notices by email or push, and refusing the household endpoints to administrator accounts on the server (today only the web hides those screens from them).

- Evaluate over-the-air (OTA) firmware updates.

- Decide what to do with the legacy repository `energy-monitor-devices` (archive or delete it): it is no longer used to run anything.

# 17. Glossary

| **Term**              | **Definition**                                                                                                                        |
|-----------------------|---------------------------------------------------------------------------------------------------------------------------------------|
| ADSO                  | Software Analysis and Development: SENA technology program that is part of the project (group/ficha 3145556).                         |
| REST API              | HTTP-based programming interface that exposes resources in JSON format under versioned routes (/api/v1).                              |
| API gateway           | Service that is the single entry point of the API (en-monitor-api-gateway): routes each request to the owning service, validates the token and answers CORS. |
| Access token          | Short-lived JWT (15 minutes) that proves the user's identity on every request.                                                        |
| Bounded context       | Domain bounded context (security, home, measurement…) with its own tables and internal API; it can live in a microservice or in a backend module. |
| Proxy                 | Backend package (security, device or measurement) that serves the api contract of an already extracted context by calling the microservice and translating its events; it has no domain or tables. |
| Microservice          | Independent application, with its own repository, deployment and tables, that implements a context (security, devices or measurements). |
| RabbitMQ              | Message broker (AMQP) that carries the events between services.                                                                      |
| Transactional outbox  | Pattern in which the message is stored in an outbox table in the same transaction as the change and a relay later publishes it to RabbitMQ. |
| X-Internal-Token      | Header with the shared secret (INTERNAL_API_TOKEN) required by the internal `/internal/**` routes between services.                   |
| BCrypt                | Salted hashing algorithm used to store passwords irreversibly.                                                                        |
| MQTT broker           | Server (Eclipse Mosquitto) that receives the messages published by the devices and delivers them to subscribers.                      |
| CORS                  | HTTP policy that controls from which origins (e.g. http://localhost:5173) the browser may call the API.                               |
| Changeset             | Liquibase migration unit: versioned instruction to create or modify tables.                                                           |
| CurrentUserResolver   | Component of each module or service that obtains the user's identity from the `sub` claim of the JWT.                                 |
| Docker Compose        | Tool that defines and starts containers (for example mysql:8.4) with a single command; the project's is in energy-monitor-infra.      |
| DTO                   | Data transfer object: JSON input or output structure of an endpoint.                                                                  |
| ER / ERD              | Entity-relationship diagram: logical model of the tables and their relationships.                                                     |
| JWT                   | JSON Web Token: signed token (RS256 algorithm) that carries the user's identity.                                                      |
| Liquibase             | Schema manager: applies database changesets in a controlled and versioned way.                                                        |
| Module (Modulith)     | Backend code unit with boundaries verified by Spring Modulith; it only relates through its api package.                              |
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
| X-User-Id             | Header the services ignore: identity is always taken from the `sub` claim of the JWT.                                                 |

# 18. Appendices

## Appendix A. Configuration and commands

**Environment variables.** Each service reads its secrets from its own .env file (imported through spring.config.import) whose structure is documented in its .env.example and which is read as `.properties` (values without quotes and without `$`); in Docker, the values come from `infra/.env` (or `.env.prod` on the server) of the energy-monitor-infra repository. The web front uses .env.local with VITE_API_BASE_URL and VITE_GOOGLE_CLIENT_ID, and the mobile app uses .env with EXPO_PUBLIC_API_BASE_URL. For security reasons no real values are published:

| **Variable**                                                 | **Description**                                                                                      |
|--------------------------------------------------------------|------------------------------------------------------------------------------------------------------|
| MYSQL_PASSWORD / MYSQL_ROOT_PASSWORD                         | Passwords of the application user and the MySQL root user (defined in infra/.env).                   |
| SPRING_DATASOURCE_PASSWORD                                   | Password of energy_app in each service with a database (security, device, measurement and backend) and in their tests. |
| MAIL_HOST / MAIL_PORT                                        | Backend: SMTP server and port (default smtp.gmail.com:587).                                          |
| MAIL_USERNAME / MAIL_PASSWORD / MAIL_FROM                    | Backend: sender account and application password to send emails.                                     |
| SECURITY_RESET_PEPPER                                        | Security service: HMAC-SHA256 pepper of the password recovery codes.                                 |
| SECURITY_SESSION_COOKIE_SECURE                               | Security service: Secure flag of the session cookies (only turned off on sites served over HTTP).   |
| SECURITY_CORS_ALLOWED_ORIGINS                                | Gateway: origins allowed by CORS (http://localhost:5173 in the example).                             |
| SECURITY_JWT_PRIVATE_KEY_PATH / SECURITY_JWT_PUBLIC_KEY_PATH | Absolute paths to the RSA key pair used by RS256: the private key only in the security service; the public key in the gateway and all the services. |
| GOOGLE_CLIENT_ID / GOOGLE_CLIENT_SECRET                      | Security service: OAuth credentials to sign in with Google; when empty, the button answers 503.      |
| INTERNAL_API_TOKEN                                           | Secret of the X-Internal-Token header; same value in security, device, measurement and backend.      |
| INTERNAL_API_ALLOWED_NETWORKS                                | Networks allowed to call each service's `/internal/**` routes.                                       |
| SECURITY_SERVICE_URL / DEVICE_SERVICE_URL / MEASUREMENT_SERVICE_URL / BACKEND_URL | Gateway: address of each service (8082, 8084, 8085 and 8083). The backend uses the first three and the measurement service, DEVICE_SERVICE_URL. |
| BACKEND_INTERNAL_URL                                         | Security, device and measurement services: address of the backend's internal API.                    |
| RABBITMQ_HOST / RABBITMQ_PORT / RABBITMQ_USERNAME / RABBITMQ_PASSWORD | RabbitMQ connection of security, device, measurement and backend.                         |
| MQTT_BROKER_URL / MQTT_CLIENT_ID                             | Device and measurement services: connection to the MQTT broker (internal listener 1883).            |
| MQTT_DEVICE_SERVICE_USER / MQTT_DEVICE_SERVICE_PASS          | MQTT user of the device service (reads telemetry and status, publishes commands).                   |
| MQTT_MEASUREMENT_SERVICE_USER / MQTT_MEASUREMENT_SERVICE_PASS | MQTT user of the measurement service (reads telemetry only).                                        |
| MEASUREMENT_MQTT_ENABLED                                     | Measurement service: enables the telemetry subscription (off by default; the infra stack turns it on). |
| MQTT_AUTH_ALLOWED_NETWORKS                                   | Device service: networks allowed to call the broker's internal authentication routes.                |
| DEVICE_MQTT_PUBLIC_HOST / DEVICE_MQTT_PUBLIC_PORT            | Device service: broker address given to the device when linking it (port 8883).                      |
| DEVICE_OFFLINE_AFTER                                         | Device service: silence after which a device is marked offline (PT150S).                             |
| NOTIFICATION_WEB_URL / NOTIFICATION_VAPID_PUBLIC_KEY / NOTIFICATION_VAPID_PRIVATE_KEY / NOTIFICATION_VAPID_SUBJECT | Backend: email links and VAPID keys of browser push. |
| EXPO_ACCESS_TOKEN                                            | Backend: Expo token for mobile app push; when empty, there is no push in the app.                    |
| ALERT_LIMIT_ZONE / RECOMMENDATION_EVALUATE_INTERVAL / RECOMMENDATION_ZONE | Backend: time zone of the limit alerts and frequency and time zone of the recommendations. |
| GATEWAY_READ_TIMEOUT / GATEWAY_RATE_LIMIT_REQUESTS / GATEWAY_RATE_LIMIT_WINDOW | Gateway: maximum wait for a service (30 s) and limit of public POSTs (20 per minute). |
| BACKEND_CONTEXT / SECURITY_CONTEXT / DEVICE_CONTEXT / MEASUREMENT_CONTEXT / GATEWAY_CONTEXT / FRONTEND_CONTEXT | infra/.env: paths of the repositories when they are not cloned side by side. |
| VITE_API_BASE_URL / VITE_GOOGLE_CLIENT_ID (web front)        | Gateway base URL (default http://localhost:8080) and Google client ID.                               |
| EXPO_PUBLIC_API_BASE_URL (mobile app)                        | Gateway base URL: 10.0.2.2 on the Android emulator or the PC's IP on a phone.                        |

> *Variables removed from the backend: MQTT_BACKEND_USER / MQTT_BACKEND_PASS, MQTT_CLIENT_ID and DEVICE_OFFLINE_AFTER (the backend no longer connects to the MQTT broker).*

**Operation commands:**

| **Command**                             | **Description**                                                                    |
|-----------------------------------------|------------------------------------------------------------------------------------|
| docker compose up -d --build (in infra) | Starts the full local stack: MySQL, RabbitMQ, Mosquitto, the five Java services and the web (http://localhost). |
| docker compose -f docker-compose.yml -f docker-compose.dev.yml up -d mysql rabbitmq mosquitto (in infra) | Development mode: starts only MySQL, RabbitMQ and the broker. |
| docker compose down -v (in infra)       | Stops the containers and removes their volumes (regenerates the schema with Liquibase). |
| docker run -p 8025:8025 axllent/mailpit | Starts the test email server (interface at http://localhost:8025).                 |
| ./mvnw spring-boot:run (in each service; .\mvnw.cmd on Windows) | Starts the gateway (8080), security (8082), backend (8083), devices (8084) or measurements (8085). |
| ./mvnw test                             | Runs the service's test suite.                                                     |
| ./mvnw package                          | Generates the executable JAR in the /target folder.                                |
| npm install / npm run dev               | Installs dependencies and starts the web application (http://localhost:5173).     |
| npm test / npm run lint                 | Runs the tests and the static analysis of the web front.                           |
| npm run start / npm run android         | Starts the mobile application in Expo (Android device or emulator).                |
| e2e/device-flow.sh / e2e/measurement-flow.sh (in infra) | End-to-end tests on the local stack.                               |
| deploy/deploy.sh \<component\> (on the server) | Deploys a component; GitHub Actions invokes it.                             |
| arduino-cli compile / upload (in firmware/esp32-pzem-monitor) | Builds and uploads the device firmware over USB.             |

**Post-installation verification:** GET /actuator/health of each service answers {"status":"UP"} (docker compose ps shows gateway, security, device, measurement and backend as healthy), the sign-in screen loads in the browser, registering an account sends the verification code, the households created appear in the application panel and the first reading of a linked device appears in the household.

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
