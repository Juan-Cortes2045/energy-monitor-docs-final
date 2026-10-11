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
| Team members         | Juan Esteban Cortes Parra Miguel Angel Garcia Artunduaga                        |
| Instructor           | Karol Daniela Correa                                                            |
| Training center      | Center for Industry, Business and Services (CIES) – SENA Huila Regional         |
| Program              | Technology in Software Analysis and Development (ADSO)                          |
| Group (Ficha)        | 3145556                                                                         |
| Document type        | Software technical manual                                                       |
| Version              | 1.0                                                                             |
| Date prepared        | October 7, 2026                                                                 |

Year 2026

# 2. Document version control

The change history of this manual is recorded in the following table. Every relevant modification to the software must be reflected in a new version of the document.

| **Version** | **Date**   | **Description of change**                                                                                                                                                                | **Responsible**      |
|-------------|------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------|
| 1.0         | 10/07/2026 | Initial creation of the technical manual: general description, technologies, architecture, folder structure, database, modules, API and security of the current version of the software. | Development team     |
| 1.0         | 10/07/2026 | Incorporation of the officially approved diagrams: ERD, class diagram, architecture and authentication sequence, as well as the data dictionary.                                         | Development team     |

# 3. Table of contents

[2. Document version control 1](#document-version-control)

[3. Table of contents 2](#table-of-contents)

[4. Introduction 4](#introduction)

[5. Objective 4](#objective)

[6. Technical scope 5](#technical-scope)

[7. General system description 5](#general-system-description)

> [7.1 What is the system? 5](#what-is-the-system)
>
> [7.2 Main components 5](#main-components)
>
> [7.3 System roles 6](#system-roles)
>
> [7.4 Implementation status by domain 6](#implementation-status-by-domain)

[8. Technologies used 7](#technologies-used)

> [8.1 Main backend dependencies (pom.xml) 7](#main-backend-dependencies-pom.xml)
>
> [8.2 Available scripts 8](#available-scripts)

[9. System architecture 8](#system-architecture)

> [9.1 General architecture 8](#general-architecture)
>
> [9.2 Architectural style and design patterns 8](#architectural-style-and-design-patterns)
>
> [9.3 Communication between components 9](#communication-between-components)

[10. Project structure 10](#project-structure)

> [10.1 Backend structure 10](#backend-structure)
>
> [10.2 Web application structure 11](#web-application-structure)
>
> [10.3 Mobile application structure 11](#mobile-application-structure)
>
> [10.4 Function of the main folders 11](#function-of-the-main-folders)

[11. Database 13](#database)

> [11.1 Entity-relationship diagram 13](#entity-relationship-diagram)
>
> [11.2 Main tables 15](#main-tables)
>
> [11.3 Data dictionary 16](#data-dictionary)

[12. Technical modules 24](#technical-modules)

> [12.1 Overview of the modules 24](#overview-of-the-modules)
>
> [12.2 Domain class diagram 24](#domain-class-diagram)
>
> [12.3 Backend modules by context 26](#backend-modules-by-context)
>
> [12.4 Web front-end module 29](#web-front-end-module)
>
> [12.5 Mobile front-end module 30](#mobile-front-end-module)

[13. API or system routes 31](#api-or-system-routes)

> [13.1 Common response codes 32](#common-response-codes)
>
> [13.2 Request and response example 32](#request-and-response-example)

[14. Security 33](#security)

> [14.1 Implemented controls 33](#implemented-controls)
>
> [14.2 Known limitations and risks 33](#known-limitations-and-risks)
>
> [14.3 Best practices for developers 34](#best-practices-for-developers)

[15. Software version control 35](#software-version-control)

[16. Maintenance recommendations 35](#maintenance-recommendations)

> [16.1 Code updates 35](#code-updates)
>
> [16.2 Database backups 36](#database-backups)
>
> [16.3 Support and incident resolution 36](#support-and-incident-resolution)
>
> [16.4 Declared pending work 36](#declared-pending-work)

[17. Glossary 37](#glossary)

[18. Appendices 37](#appendices)

> [Appendix A. Configuration and commands 37](#appendix-a.-configuration-and-commands)

[Appendix B. ERD by context 39](#appendix-b.-erd-by-context)

[Appendix C. Class diagram by domain 40](#appendix-c.-class-diagram-by-domain)

*Note: when opening the document, Word offers to update the table of contents fields. If necessary, select the list, right-click and choose "Update field".*

# 4. Introduction

This document is the technical manual for the "Electrical Consumption Monitor" software, a project developed by the apprentices of the Software Analysis and Development training program, group (ficha) 3145556, of the Center for Industry, Business and Services (CIES) – SENA Huila Regional.

The system allows monitoring electrical energy consumption in homes: it records measurements from the devices, calculates consumption statistics, generates alerts when the configured thresholds are exceeded and offers a web and mobile interface to consult the information. The solution consists of a REST API, a web application, a mobile application and a MySQL database.

This manual is aimed at developers, technical support staff, the evaluating instructor and, in general, anyone who needs to understand, extend or maintain the software. Its purpose is to describe how the system is built, not how it is used: for everyday use there is the user manual, for the start-up the installation manual and for account and backup management the administrator manual.

The information recorded here was obtained directly from the source code of the project's three repositories and from the design documents (SRS, technical report, ERD, data dictionary and class diagram), so that it corresponds to the final version of the delivered software.

| **Information source**               | **Location**                                                                                                         |
|--------------------------------------|----------------------------------------------------------------------------------------------------------------------|
| Backend source code                  | energymonitor-backend (Java 21 + Spring Boot 4.1.1)                                                                  |
| Web application source code          | Energy-Monitor-application (React 19 + Vite 8)                                                                       |
| Mobile application source code       | Energy-Monitor-Mobile (Expo SDK 57 + React Native 0.86)                                                              |
| Project documentation                | Documentacion-proyecto-Monitor-de-consumo-electrico (SRS, technical report, diagrams, mockups, data dictionary)      |
| Requirements specification           | SRS.docx and TECHNICAL REPORT (RF1 to RF5 and RNF1 to RNF7)                                                          |
| Data model                           | Base de datos/MER.wsd and Base de datos/Diccionario de Datos.xlsx                                                    |
| Class model                          | Diagrama de clases/Diagrama_clases.wsd                                                                               |

# 5. Objective

> To document the architecture, technologies, project structure, database, technical modules, API, security and maintenance recommendations of the "Electrical Consumption Monitor" system, so that a developer outside the team can understand the software, continue its development and support it without prior knowledge of the project.

- Describe the three-layer architecture and the design patterns used in each component.

- Detail the exact technologies and versions used in the backend and in the two front-ends.

- Explain the organization of the source code and the function of each folder.

- Document the data model: entity-relationship diagram, main tables and data dictionary.

- Relate each technical module to its files, tables and endpoints.

- Describe the services exposed by the REST API and the security mechanisms applied.

- Establish recommendations for updating, backing up and supporting the system.

# 6. Technical scope

The technical manual covers the following aspects of the system:

| **Covers**                                                                              | **Does not cover**                                                                                                   |
|-----------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------|
| General architecture, layers and design patterns of the backend and the front-ends.     | Installation and start-up procedures (covered by the installation manual).                                           |
| Technologies, libraries and versions used, as well as their dependencies.               | Usage steps, screens and messages for the end user (covered by the user manual).                                     |
| Folder and file structure of the project's three repositories.                          | Management of users, roles, administrative reports and backups (covered by the administrator manual).                |
| Database: ERD, tables, relationships and data dictionary.                               | Electrical design and firmware of the measurement device (hardware outside the scope of the software).              |
| Technical modules of the backend, web front and mobile front, with their files and tables. | User stories, use cases and acceptance criteria (analysis documents).                                             |
| REST API: method, route, parameters, authentication and responses.                      | Load testing and deployment in a production environment.                                                             |
| Security: authentication, authorization, encryption, validations and known limitations. | Costs, project plan and management reports.                                                                          |
| Version control and maintenance recommendations.                                        |                                                                                                                      |

**Documented version:** software available in the repositories as of October 7, 2026. Features marked as "pending" or "skeleton" are explicitly described in section 12.

# 7. General system description

## 7.1 What is the system?

"Electrical Consumption Monitor" is a web and mobile solution that allows users to know in real time how much energy their homes consume, identify the devices that use the most energy, receive alerts when consumption exceeds the established limits and obtain recommendations to reduce spending and the electricity bill.

## 7.2 Main components

| **Component**      | **Type**                         | **Function**                                                                                                                       |
|--------------------|----------------------------------|------------------------------------------------------------------------------------------------------------------------------------|
| REST API           | Server (Java 21 + Spring Boot)   | Exposes the authentication, household, measurement, threshold and alert services. It is the only source of truth for the data.     |
| Web application    | SPA (React 19 + Vite 8)          | Query and management interface: access, household panel, consumption, history, users, thresholds, alerts and settings.             |
| Mobile application | Hybrid app (Expo SDK 57)         | Mobile interface with the same domain model as the web; today it operates with demonstration data pending integration with the API. |
| Database           | MySQL 8.4                        | Stores users, households, devices, measurements, alerts, recommendations and notifications (27 tables).                            |
| Email              | SMTP (Mailpit locally)           | Sends the email verification and password recovery codes.                                                                          |

## 7.3 System roles

| **Role**                     | **Description**                                                                                  | **How it is implemented**                                                                                                            |
|------------------------------|--------------------------------------------------------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------|
| Household owner (OWNER)      | Creates and manages a household: invites members, configures thresholds and manages devices.    | OWNER enum of the user_home table, with domain permissions in the home module.                                                       |
| Household member (MEMBER)    | Views consumption, history, alerts and settings of the household they were invited to.           | MEMBER enum of the user_home table.                                                                                                  |
| System user                  | Personal account with profile, language, theme and notification preferences.                     | user table with status ACTIVE / INACTIVE / BLOCKED.                                                                                  |
| Access control (RBAC)        | Functional permissions grouped into system roles (codes such as METER.READ, ALERT.READ).         | Tables system_role, permission, user_system_role and system_role_permission.                                                         |
| Administrator                | Global management of the system and accounts, according to the SRS.                              | Partial: the role assignment ports exist in the security module; the administrative interface is not yet exposed over HTTP.          |

## 7.4 Implementation status by domain

The system is documented as it is built. The following table distinguishes what is already connected from what is in the development phase, in accordance with the guide's recommendation not to include modules that do not exist in the software.

| **Requirement (SRS)**                      | **Software module**                                 | **Status**                                                                                                                      |
|--------------------------------------------|-----------------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------|
| RF1 – User management                      | security (backend) + auth (web and mobile)          | Implemented: registration, email verification, login, refresh, logout, password recovery and change, profile and permissions.   |
| RF2 – Project and household management     | home (backend) + dashboard/DetailHome (web)         | Implemented: create household, access code, members, favorites and thresholds.                                                  |
| RF3 – System management and configuration  | settings (web and mobile) + user_configuration (table) | Partial: theme, language and notifications are managed on the client; the preference is stored in the user_configuration table. |
| RF4 – Consumption recording and processing | measurement and alert (backend) + Consumption (web) | Partial: the measurements and alerts API is operational; the web front charts use demonstration data.                           |
| RF5 – Measurement device management        | device (backend)                                    | Skeleton: the table schema and the declared module exist, with no implemented use cases.                                        |
| Automatic recommendations                  | recommendation (backend)                            | Skeleton: table created and module declared; the analysis logic is pending.                                                     |

# 8. Technologies used

The following table records the languages, frameworks, libraries, database, design and version-control tools used in the project, with their version or usage detail.

| **Component**                  | **Technology**                           | **Version / detail**                                              |
|--------------------------------|------------------------------------------|-------------------------------------------------------------------|
| Backend language               | Java                                     | Java 21 (LTS)                                                     |
| Backend framework              | Spring Boot                              | Spring Boot 4.1.1 (Spring Framework 7)                            |
| Architectural style            | Spring Modulith + hexagonal architecture | spring-modulith 2.1.1, 7 modules (bounded contexts)               |
| Backend build                  | Maven Wrapper                            | mvnw / mvnw.cmd, spring-boot-maven-plugin                         |
| Persistence                    | Spring Data JPA + Hibernate              | ddl-auto = validate (the schema is defined by Liquibase)          |
| Database migrations            | Liquibase                                | db/changelog with changesets per module                           |
| Database engine                | MySQL                                    | MySQL 8.4, collation utf8mb4_unicode_ci, schema energy_monitor    |
| Authentication and authorization | Spring Security + OAuth2 Resource Server | JWT RS256 with RSA key pair (PEM), BCrypt for passwords         |
| Data validation                | Jakarta Bean Validation                  | @NotBlank, @Size, @NotNull, @PositiveOrZero                       |
| API documentation              | springdoc-openapi                        | springdoc 3.1.1, Swagger UI and /v3/api-docs                      |
| Email                          | spring-boot-starter-mail                 | SMTP with STARTTLS; Mailpit as local test server                  |
| Monitoring and health          | Spring Boot Actuator                     | /actuator/health and /actuator/info                               |
| Backend tests                  | JUnit 5 + Spring Boot Test               | ≈105 test files in src/test/java                                  |
| Web front-end                  | React + Vite                             | React 19.2, Vite 8.0, JavaScript (ESM), no TypeScript             |
| Web routing                    | react-router-dom                         | react-router-dom 7.17 with protected routes and nested layouts    |
| API consumption (web)          | axios                                    | axios 1.17 with request and response interceptors                 |
| Forms and validation (web)     | react-hook-form + zod                    | react-hook-form 7.72, zod 4.3, @hookform/resolvers                |
| Charts (web)                   | recharts                                 | recharts 3.8 (AreaChart, PieChart, BarChart)                      |
| Internationalization (web)     | i18next + react-i18next                  | 4 languages: es, en, fr, pt                                       |
| Styles (web)                   | CSS Modules + CSS variables              | own design system in src/design, light/dark themes                |
| Tests (web)                    | Vitest + Testing Library                 | vitest 5.0, jsdom, 11 test files                                  |
| Mobile app                     | Expo + React Native                      | Expo SDK 57, React Native 0.86.3, TypeScript 6 (strict)           |
| Mobile routing                 | Expo Router                              | file-based routes in src/app with typed routes                    |
| Charts (mobile)                | react-native-chart-kit                   | LineChart, BarChart and PieChart over react-native-svg            |
| Styles (mobile)                | StyleSheet                               | own design system in src/constants (theme, colors, spacing)       |
| Version control                | Git and GitHub                           | code, documentation and mobile app repositories                   |
| Design and diagrams            | Draw.io and PlantUML                     | use cases, activities, ERD, class diagram and architecture        |

## 8.1 Main backend dependencies (pom.xml)

The backend's pom.xml file declares as core dependencies spring-boot-starter-webmvc, spring-boot-starter-data-jpa, spring-boot-starter-security, spring-boot-starter-oauth2-resource-server, spring-boot-starter-validation, spring-boot-starter-liquibase, spring-boot-starter-mail, spring-boot-starter-actuator, spring-modulith-starter-core (with its observability and actuator modules), springdoc-openapi-starter-webmvc-ui 3.1.1, mysql-connector-j (runtime) and the set of test starters (\*-test and spring-modulith-starter-test).

## 8.2 Available scripts

| **Repository**  | **Script**                      | **Function**                                                    |
|-----------------|---------------------------------|-----------------------------------------------------------------|
| backend         | mvnw spring-boot:run            | Builds and starts the API at http://localhost:8080.             |
| backend         | mvnw test                       | Runs the test suite (requires MySQL running).                   |
| backend         | mvnw package                    | Generates the executable JAR file in /target.                   |
| backend         | docker compose up -d            | Starts the MySQL 8.4 container with the energy_monitor schema.  |
| web             | npm run dev                     | Starts the Vite development server at http://localhost:5173.    |
| web             | npm run build / npm run preview | Builds the application into /dist and serves it for review.     |
| web             | npm test                        | Runs the tests with Vitest.                                     |
| web             | npm run lint                    | Analyzes the code with ESLint.                                  |
| mobile          | npm run start                   | Starts Expo's Metro Bundler.                                    |
| mobile          | npm run android / ios / web     | Runs the application on Android, iOS or the browser.            |
| mobile          | npm run lint                    | Analyzes the code with ESLint (there are no automated tests).   |

# 9. System architecture

## 9.1 General architecture

The system adopts a three-layer client-server architecture. The presentation layer is made up of the web application and the mobile application; the logic layer corresponds to the REST API built in Spring Boot; and the data layer corresponds to MySQL, accessed through JPA/Hibernate and managed with Liquibase.

> **Images and diagrams (Figures 1 to 11):** all the figures in this document are available in the following Google Drive folder.  
> https://drive.google.com/drive/folders/1tIzih2FKP8xte4vKYEtrWThFmZxR1oBX?usp=sharing

**Figure 1.** *General system architecture (three layers: presentation, logic and data).*

| **Layer**      | **Technology**                                        | **Responsibility**                                                                                   |
|----------------|-------------------------------------------------------|------------------------------------------------------------------------------------------------------|
| Presentation   | React 19 (web) and Expo SDK 57 (mobile)               | User interface, form validation, session handling and consumption of the REST API.                   |
| Logic          | Spring Boot 4.1.1 + Spring Security + Spring Modulith | Authentication, authorization, use cases, business rule validation and event publishing.             |
| Data           | MySQL 8.4 + Spring Data JPA + Liquibase               | Persistence of the 27 tables, versioned schema and optimized queries.                                |
| Cross-cutting  | Docker Compose, Mailpit and Actuator                  | Database container, test email and service health check endpoint.                                    |

## 9.2 Architectural style and design patterns

- **Modular monolith (Spring Modulith).** The backend is a single application organized into seven modules or bounded contexts (security, home, measurement, alert, notification, device and recommendation). Each module declares its allowed dependencies in the package-info.java file and can only communicate with the others through its public API; this restriction is automatically verified by the ModularityTests test.

- **Hexagonal architecture per module.** Within each module there are input ports (application/port/in), output ports (application/port/out), use cases (usecase), input adapters (adapter/in/web) and output adapters (adapter/out/persistence, out/security, out/client) and an infrastructure directory with the Spring configuration.

- **In-process events.** The measurement module publishes the MeasurementRecorded event; the alert module listens to it with @EventListener and generates the threshold alerts. The same pattern is used for AlertRaised. This avoids direct coupling between measurements and alerts.

- **Stateless REST API.** No cookies or HTTP sessions are used: each request carries a JWT in the Authorization header. The CORS policy is configured explicitly and restrictively.

- **Front-end by capabilities (features).** Both the web and the mobile apps organize the code in folders by functional capability (auth, dashboard, homes, notifications…) with their own components, styles, zod validations and tests. Global state is handled with React Context and the session is persisted in localStorage (web).

## 9.3 Communication between components

The client applications communicate with the API through HTTP requests in JSON format under the /api/v1 prefix. Every authenticated request includes the Authorization: Bearer \<accessToken\> header; in addition, the operations of the home module receive the X-User-Id header with the account identifier (a temporary solution declared in the code). The web front automatically renews the access token upon a 401 response through POST /auth/refresh, with refresh token rotation and locking between tabs using the Web Locks API.

| **Communication element**    | **Detail**                                                                                |
|------------------------------|-------------------------------------------------------------------------------------------|
| Protocol                     | HTTP/1.1 over HTTPS in production; HTTP in local development.                             |
| Format                       | JSON (Content-Type: application/json).                                                   |
| API prefix                   | /api/v1 (all backend routes).                                                             |
| Authentication               | Authorization: Bearer \<JWT RS256\> with 15-minute expiration.                            |
| Additional identity          | X-User-Id: \<idUser\> (use restricted to the home module, pending removal).               |
| Allowed origin (CORS)        | http://localhost:5173 (variable SECURITY_CORS_ALLOWED_ORIGINS), without credentials.      |
| Error handling               | Response with ApiError structure: timestamp, status, error, message, path and fieldErrors.|
| Documentation                | OpenAPI and Swagger UI available at /v3/api-docs and /swagger-ui.html.                    |
| Service health               | GET /actuator/health returns {"status":"UP"}.                                             |

The following figure describes the complete flow of a login and the subsequent access to a protected resource.

**Figure 2.** *Authentication sequence: login, protected access and token renewal.*

# 10. Project structure

The project is distributed across four repositories: three of source code (backend, web front and mobile app) and one of documentation. The following table summarizes their content and how to run them.

| **Repository**                                      | **Type**                     | **Technology**                                                  | **Entry point**                                                      | **Execution**                            |
|-----------------------------------------------------|------------------------------|-----------------------------------------------------------------|----------------------------------------------------------------------|------------------------------------------|
| energy-monitor-backend                              | REST API (backend)           | Java 21 + Spring Boot 4.1.1 + Maven                             | src/main/java/com/energymonitor/EnergyMonitorBackendApplication.java | .\mvnw.cmd spring-boot:run (port 8080)   |
| Energy-Monitor-application                          | Web application (front-end)  | React 19 + Vite 8 (JavaScript)                                  | src/main.jsx over index.html                                         | npm install / npm run dev (port 5173)    |
| Energy-Monitor-Mobile                               | Mobile application (front-end) | Expo SDK 57 + React Native 0.86                               | src/app/\_layout.tsx (expo-router/entry)                             | npm install / npm run start              |
| Documentacion-proyecto-Monitor-de-consumo-electrico | Project documentation        | SRS, technical report, diagrams, mockups and data dictionary    | SRS.docx, README.md                                                  | Does not require execution               |

## 10.1 Backend structure

> energy-monitor-backend/
>
> ├── pom.xml Maven definition and dependencies
>
> ├── mvnw / mvnw.cmd Maven Wrapper (does not require Maven installed)
>
> ├── docker-compose.yml MySQL 8.4 container (port 3306)
>
> ├── .env.example environment variables template
>
> ├── README.md backend instructions
>
> └── src/
>
> ├── main/
>
> │ ├── java/com/energymonitor/
>
> │ │ ├── EnergyMonitorBackendApplication.java entry point
>
> │ │ ├── security/ identity, JWT, RBAC and audit
>
> │ │ ├── home/ households, members and thresholds
>
> │ │ ├── measurement/ measurements and consumption levels
>
> │ │ ├── alert/ threshold and connectivity alerts
>
> │ │ ├── notification/ sending and logging of emails
>
> │ │ ├── device/ (skeleton) measuring devices
>
> │ │ └── recommendation/ (skeleton) saving recommendations
>
> │ └── resources/
>
> │ ├── application.yaml base configuration
>
> │ ├── application-local.yaml(.example) local profile and keys
>
> │ └── db/changelog/ Liquibase master + changesets
>
> │ ├── db.changelog-master.xml
>
> │ ├── security/ home/ device/ measurement/
>
> │ └── alert/ recommendation/ notification/
>
> └── test/java/com/energymonitor/ ≈105 test files

**Internal organization of each module (example with home):**

> home/
>
> ├── package-info.java declares the module boundaries (@ApplicationModule)
>
> ├── api/ public contract (HomeApi, HomeDto) used by other modules
>
> ├── domain/model/ pure domain entities (Home, UserHome, Role…)
>
> ├── application/
>
> │ ├── port/in/ input ports (CreateHome, JoinHome, ListHomes…)
>
> │ ├── port/out/ output ports (persistence, generators)
>
> │ ├── usecase/ implementation of the use cases
>
> │ ├── command/ result/ input and output structures
>
> │ └── exception/ application exceptions
>
> ├── adapter/
>
> │ ├── in/web/ REST controllers, DTOs and error handlers
>
> │ ├── in/event/ event listeners
>
> │ └── out/persistence/ JPA entities, repositories, mappers and adapters
>
> └── infrastructure/ Spring configuration and the module's internal API

## 10.2 Web application structure

> Energy-Monitor-application/
>
> ├── index.html · package.json · vite.config.js
>
> ├── vitest.config.js · eslint.config.js · .env.example
>
> ├── public/ (icons and resources)
>
> └── src/
>
> ├── main.jsx mounts ThemeProvider \> HomeProvider \> App
>
> ├── App.jsx · index.css
>
> ├── routes/ AppRoutes, ProtectedRoute, HomeRoute
>
> ├── context/ HomeContext, ThemeContext, useHomes
>
> ├── services/
>
> │ ├── http/ httpClient (axios), interceptors and errors
>
> │ ├── auth/ authApi, session (localStorage), useCurrentPerson
>
> │ ├── home/ homeApi (households, members, thresholds)
>
> │ └── alerts/ alertApi (household alerts)
>
> ├── components/ layout/ (Navbar, MainLayout, Sidebar) and shared/
>
> ├── design/ design system (Button, Input, Card, Badge…)
>
> ├── features/ landingPage, auth, dashboard, DetailHome,
>
> │ Favorites, Notifications, Account, SettingsPage
>
> ├── i18n/ locales es, en, fr and pt
>
> └── test/setup.js Vitest configuration

## 10.3 Mobile application structure

> Energy-Monitor-Mobile/
>
> ├── app.json · package.json · tsconfig.json · AGENTS.md
>
> ├── assets/ icons and splash screen
>
> ├── scripts/reset-project.js
>
> └── src/
>
> ├── app/ Expo Router routes:
>
> │ \_layout.tsx (providers and Stack), index.tsx (login),
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
> ├── i18n/ locales es, en, fr and pt
>
> └── validation/ i18n messages of the zod schemas

## 10.4 Function of the main folders

| **Folder**                                                                 | **Function**                                                                             |
|----------------------------------------------------------------------------|------------------------------------------------------------------------------------------|
| backend/src/main/java/com/energymonitor/\<module\>/domain/model            | Pure domain entities, with no Spring dependencies.                                       |
| backend/src/main/java/com/energymonitor/\<module\>/application             | Use cases, input and output ports, commands, results and exceptions.                     |
| backend/src/main/java/com/energymonitor/\<module\>/adapter/in/web          | REST controllers, DTOs and exception handlers (@RestControllerAdvice).                   |
| backend/src/main/java/com/energymonitor/\<module\>/adapter/out/persistence | JPA entities, repositories, mappers and persistence adapters.                            |
| backend/src/main/resources/db/changelog                                    | Liquibase changesets that build the MySQL schema.                                        |
| backend/src/test/java                                                      | Unit, web integration and security tests; module boundary verification.                  |
| web/src/routes                                                             | Definition of routes, protections and layouts of the SPA.                                |
| web/src/services                                                           | API access layer: HTTP client, interceptors, session and endpoints per domain.           |
| web/src/features                                                           | Screens and components grouped by functionality.                                         |
| web/src/design                                                             | Reusable interface components (buttons, cards, fields) and their styles.                 |
| mobile/src/app                                                             | File-based Expo Router routes that mount each screen.                                    |
| mobile/src/features                                                        | Screens, components, hooks, validations and demonstration data per functionality.        |

# 11. Database

**Engine:** MySQL 8.4, run in a Docker container (docker-compose.yml) with collation utf8mb4_unicode_ci.

**Schema:** energy_monitor, with 27 tables distributed across seven contexts. The application user is energy_app.

**Schema management:** Liquibase applies the versioned changesets in src/main/resources/db/changelog; Hibernate is configured with ddl-auto = validate, that is, it only validates that the schema matches the entities and never creates tables by itself.

**Conventions:** names in snake_case; primary keys of type VARCHAR(10); audit columns created_at, updated_at and deleted_at in all tables; logical deletion (no physical DELETE by default); indexes with the pattern idx\_\[table\]\_\[columns\]. References between contexts are stored as identifiers at the application level and do not generate SQL foreign keys.

## 11.1 Entity-relationship diagram

The following diagram shows the 27 entities of the system, grouped by context (Security, Home Management, Devices, Monitoring and measurements, Alerts, Recommendations and Notifications), with their attributes, keys and relationships. The dotted relationships between contexts represent application references (without a foreign key in SQL).

> **Reading recommendation:** due to its size, the diagram is presented on a landscape page; in Word it can be enlarged with the zoom to read the attributes of each entity. Appendix B includes the same model broken down by context, with greater legibility.
>
**Figure 3.** *Complete entity-relationship model (ERD) of the "Electrical Consumption Monitor" system.*

## 11.2 Main tables

The following table lists the 27 tables of the schema, the context to which they belong, their description, their primary key and the relationships declared in the data dictionary.

| **Table**             | **Context**                 | **Description**                                                                                                                                                                                                    | **Primary key**    | **Relationships**                                                                                                                                                       |
|-----------------------|-----------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|--------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Person                | Security                    | Records the personal data associated with a user.                                                                                                                                                                  | —                  | Person 1:1 User                                                                                                                                                         |
| User                  | Security                    | Manages the user's credentials, status and access data.                                                                                                                                                            | —                  | User 1:1 Person; User 1:1 UserConfiguration; User 1:N PasswordResetToken; User 1:N UserSession; User 1:N UserSystemRole; User 1:N AuditLog                              |
| UserConfiguration     | Security                    | Stores each user's configuration preferences.                                                                                                                                                                      | —                  | UserConfiguration 1:1 User                                                                                                                                              |
| PasswordResetToken    | Security                    | Stores tokens for password recovery.                                                                                                                                                                               | —                  | PasswordResetToken N:1 User                                                                                                                                             |
| RefreshToken          | Security                    | Each row is a refresh token generation. Only the SHA-256 hash of the secret is stored, never the secret in plain text. family_id groups all generations of the same login and parent_id chains the genealogy.     | id_refresh_token   | RefreshToken N:1 UserSession; RefreshToken 0..1:1 RefreshToken (parent_id)                                                                                              |
| UserSession           | Security                    | Records the user's active and renewable sessions.                                                                                                                                                                  | —                  | UserSession N:1 User; UserSession 1:N RefreshToken                                                                                                                      |
| SecurityConfiguration | Security                    | Defines configurable security parameters of the system.                                                                                                                                                            | —                  | No relationships recorded                                                                                                                                               |
| PasswordPolicy        | Security                    | Defines the password validation and expiration rules.                                                                                                                                                              | —                  | No relationships recorded                                                                                                                                               |
| LoginErrorLog         | Security                    | Records the errors produced during login.                                                                                                                                                                          | —                  | LoginErrorLog N:1 User                                                                                                                                                  |
| SystemRole            | Security                    | Defines the roles available in the system.                                                                                                                                                                         | —                  | SystemRole 1:N UserSystemRole; SystemRole 1:N SystemRolePermission                                                                                                      |
| UserSystemRole        | Security                    | Relates users to system roles.                                                                                                                                                                                     | —                  | UserSystemRole N:1 User; UserSystemRole N:1 SystemRole                                                                                                                  |
| Permission            | Security                    | Defines the functional permissions of the system.                                                                                                                                                                  | —                  | Permission 1:N SystemRolePermission                                                                                                                                     |
| SystemRolePermission  | Security                    | Relates roles to permissions.                                                                                                                                                                                      | —                  | SystemRolePermission N:1 SystemRole; SystemRolePermission N:1 Permission                                                                                                |
| AuditLog              | Security                    | Records the relevant actions executed by users.                                                                                                                                                                    | —                  | AuditLog N:1 User                                                                                                                                                       |
| HomeType              | Home Management             | Catalog of household types.                                                                                                                                                                                        | —                  | HomeType 1:N Home                                                                                                                                                       |
| Home                  | Home Management             | Records the households managed by the system.                                                                                                                                                                      | —                  | Home N:1 HomeType; Home 1:1 HomeThresholds; Home 1:N UserHome; Home 1:N DeviceHome; Home 1:N Alert; Home 1:N Recommendation                                             |
| HomeThresholds        | Home Management             | Configures a household's consumption limits.                                                                                                                                                                       | —                  | HomeThresholds 1:1 Home                                                                                                                                                 |
| UserHome              | Home Management             | Relates users to households and defines their role.                                                                                                                                                                | —                  | UserHome N:1 User; UserHome N:1 Home                                                                                                                                    |
| ApplianceType         | Devices                     | Catalog of appliance types.                                                                                                                                                                                        | —                  | ApplianceType 1:N Device                                                                                                                                                |
| Device                | Devices                     | Records the installed measurement devices.                                                                                                                                                                         | —                  | Device N:1 ApplianceType; Device 1:N DeviceStatusLog; Device 1:N DeviceHome; Device 1:N Measurement; Device 1:N Alert; Device 1:N Recommendation                        |
| DeviceStatusLog       | Devices                     | Records the status and connectivity of the devices.                                                                                                                                                                | —                  | DeviceStatusLog N:1 Device                                                                                                                                              |
| DeviceHome            | Devices                     | Relates devices to households.                                                                                                                                                                                     | —                  | DeviceHome N:1 Device; DeviceHome N:1 Home                                                                                                                              |
| Measurement           | Monitoring and measurements | Stores the recorded electrical measurements.                                                                                                                                                                       | —                  | Measurement N:1 Device                                                                                                                                                  |
| ConsumptionLevel      | Monitoring and measurements | Defines risk levels and their consumption ranges.                                                                                                                                                                  | —                  | ConsumptionLevel 1:N Alert                                                                                                                                              |
| Alert                 | Alerts                      | Records threshold and connectivity alerts.                                                                                                                                                                         | —                  | Alert N:1 Home; Alert N:1 Device; Alert N:1 ConsumptionLevel; Alert N:1 Measurement                                                                                     |
| Recommendation        | Recommendations             | Records recommendations for households and devices.                                                                                                                                                                | —                  | Recommendation N:1 Home; Recommendation N:1 Device                                                                                                                      |
| Notification          | Notifications               | Record of the delivery of a message to the user. Alert and Recommendation are events; Notification records whether the message was delivered, through which channel and its read status.                          | —                  | Notification N:1 User (application reference); Notification N:1 Home (application reference); Notification references Alert/Recommendation by (source_type, source_id) |

## 11.3 Data dictionary

The data dictionary describes, field by field, each table of the schema: key constraint, data type, size and description. It is reproduced from the file "Base de datos/Diccionario de Datos.xlsx", delivered by the database team.

**Constraint legend:** PK = primary key; FK = foreign key; UNIQUE = unique value; NOT NULL = required field; ENUM = set of allowed values. The audit columns (created_at, updated_at and deleted_at) are repeated in all tables.

### Person — Security

*Records the personal data associated with a user.*

| **Field** | **Type** | **Size**   | **Constraint**  | **Description**             |
|-----------|----------|------------|-----------------|-----------------------------|
| id_person | VARCHAR  | 10         | NOT NULL        | Person identifier           |
| name      | VARCHAR  | 100        | NOT NULL        | Person's first names        |
| last_name | VARCHAR  | 100        | NOT NULL        | Person's last names         |

**Relationships:** Person 1:1 User

### User — Security

*Manages the user's credentials, status and access data.*

| **Field**             | **Type**  | **Size**   | **Constraint**         | **Description**                                |
|-----------------------|-----------|------------|------------------------|------------------------------------------------|
| id_user               | VARCHAR   | 10         | NOT NULL               | User identifier                                |
| person_id             | VARCHAR   | 10         | FK / UNIQUE / NOT NULL | Associated person                              |
| password_hash         | VARCHAR   | 255        | NOT NULL               | Password hash                                  |
| email                 | VARCHAR   | 255        | UNIQUE / NOT NULL      | Email address                                  |
| email_verified        | BOOLEAN   |            | NOT NULL               | Indicates whether the email was verified       |
| registration_date     | TIMESTAMP |            | NOT NULL               | Registration date and time                     |
| status                | ENUM      |            | NOT NULL               | Account status                                 |
| failed_login_attempts | INT       |            | NOT NULL               | Number of failed attempts                      |
| last_login_at         | TIMESTAMP |            | —                      | Last login                                     |
| profile_image         | VARCHAR   | 255        | —                      | Path or URL of the account's profile image     |

**Relationships:** User 1:1 Person • User 1:1 UserConfiguration • User 1:N PasswordResetToken • User 1:N UserSession • User 1:N UserSystemRole • User 1:N AuditLog

### UserConfiguration — Security

*Stores each user's configuration preferences.*

| **Field**        | **Type** | **Size**   | **Constraint**         | **Description**                   |
|------------------|----------|------------|------------------------|-----------------------------------|
| id_configuration | VARCHAR  | 10         | NOT NULL               | Configuration identifier          |
| user_id          | VARCHAR  | 10         | FK / UNIQUE / NOT NULL | Configured user                   |
| notify_by_email  | BOOLEAN  |            | NOT NULL               | Enables email notifications       |
| notify_by_push   | BOOLEAN  |            | NOT NULL               | Enables push notifications        |
| color_theme      | VARCHAR  | 50         | —                      | Color theme                       |
| language         | VARCHAR  | 50         | —                      | Preferred language                |
| social_provider  | VARCHAR  | 50         | —                      | Social authentication provider    |

**Relationships:** UserConfiguration 1:1 User

### PasswordResetToken — Security

*Stores tokens for password recovery.*

| **Field**      | **Type**  | **Size**   | **Constraint**    | **Description**                  |
|----------------|-----------|------------|-------------------|----------------------------------|
| id_reset_token | VARCHAR   | 10         | NOT NULL          | Token identifier                 |
| user_id        | VARCHAR   | 10         | FK / NOT NULL     | Owner user                       |
| reset_token    | VARCHAR   | 100        | UNIQUE / NOT NULL | Recovery token                   |
| used           | BOOLEAN   |            | NOT NULL          | Indicates whether the token was used |
| expiration_at  | TIMESTAMP |            | NOT NULL          | Expiration date                  |

**Relationships:** PasswordResetToken N:1 User

### RefreshToken — Security

*Each row is a refresh token generation. Only the SHA-256 hash of the secret is stored, never the secret in plain text. family_id groups all generations of the same login and parent_id chains the genealogy.*

| **Field**        | **Type**  | **Size**   | **Constraint**    | **Description**                                        |
|------------------|-----------|------------|-------------------|--------------------------------------------------------|
| id_refresh_token | VARCHAR   | 10         | PK / NOT NULL     | Token generation identifier                            |
| id_user_session  | VARCHAR   | 10         | FK / NOT NULL     | Session to which the generation belongs                |
| family_id        | VARCHAR   | 10         | NOT NULL          | Identifier of the root of the generation family        |
| parent_id        | VARCHAR   | 10         | FK                | Generation that this one replaces                      |
| token_hash       | VARCHAR   | 64         | UNIQUE / NOT NULL | SHA-256 hash of the secret (never the secret in plain text) |
| status           | ENUM      |            | NOT NULL          | Lifecycle status of the generation                     |
| expires_at       | TIMESTAMP |            | NOT NULL          | Expiration date                                        |
| rotated_at       | TIMESTAMP |            | —                 | Date on which it was replaced                          |
| revoked_at       | TIMESTAMP |            | —                 | Revocation date                                        |
| revoked_reason   | ENUM      |            | —                 | Reason for revocation                                  |

**Relationships:** RefreshToken N:1 UserSession • RefreshToken 0..1:1 RefreshToken (parent_id)

### UserSession — Security

*Records the user's active and renewable sessions.*

| **Field**       | **Type**  | **Size**   | **Constraint**  | **Description**                         |
|-----------------|-----------|------------|-----------------|-----------------------------------------|
| id_user_session | VARCHAR   | 10         | NOT NULL        | Session identifier                      |
| user_id         | VARCHAR   | 10         | FK / NOT NULL   | Session user                            |
| revoked         | BOOLEAN   |            | NOT NULL        | Indicates whether the session was revoked |
| ip_address      | VARCHAR   | 45         | —               | IP address                              |
| user_agent      | VARCHAR   | 150        | —               | Client agent                            |
| expiration_at   | TIMESTAMP |            | NOT NULL        | Expiration date                         |
| closed_at       | TIMESTAMP |            | —               | Closing date                            |
| revoked_at      | TIMESTAMP |            | —               | Date and time of session revocation     |
| revoked_reason  | ENUM      |            | —               | Reason for revocation                   |

**Relationships:** UserSession N:1 User • UserSession 1:N RefreshToken

### SecurityConfiguration — Security

*Defines configurable security parameters of the system.*

| **Field**                 | **Type** | **Size**   | **Constraint**    | **Description**                |
|---------------------------|----------|------------|-------------------|--------------------------------|
| id_security_configuration | VARCHAR  | 10         | NOT NULL          | Configuration identifier       |
| config_name               | VARCHAR  | 100        | UNIQUE / NOT NULL | Configuration name             |
| config_value              | VARCHAR  | 100        | NOT NULL          | Configuration value            |
| description               | VARCHAR  | 255        | —                 | Description                    |

*Relationships: no relationships declared in the dictionary.*

### PasswordPolicy — Security

*Defines the password validation and expiration rules.*

| **Field**          | **Type** | **Size**   | **Constraint**  | **Description**           |
|--------------------|----------|------------|-----------------|---------------------------|
| id_password_policy | VARCHAR  | 10         | NOT NULL        | Policy identifier         |
| min_length         | INT      |            | NOT NULL        | Minimum length            |
| max_length         | INT      |            | NOT NULL        | Maximum length            |
| require_uppercase  | BOOLEAN  |            | NOT NULL        | Requires uppercase letters |
| require_numbers    | BOOLEAN  |            | NOT NULL        | Requires numbers          |
| require_symbols    | BOOLEAN  |            | NOT NULL        | Requires symbols          |
| expiration_days    | INT      |            | NOT NULL        | Days of validity          |

*Relationships: no relationships declared in the dictionary.*

### LoginErrorLog — Security

*Records the errors produced during login.*

| **Field**      | **Type** | **Size**   | **Constraint**  | **Description**                |
|----------------|----------|------------|-----------------|--------------------------------|
| id_login_error | VARCHAR  | 10         | NOT NULL        | Error identifier               |
| user_id        | VARCHAR  | 10         | FK              | Related user, if any           |
| error_type     | ENUM     |            | NOT NULL        | Error type                     |
| description    | VARCHAR  | 500        | —               | Error description              |
| ip_address     | VARCHAR  | 45         | —               | IP address                     |

**Relationships:** LoginErrorLog N:1 User

### SystemRole — Security

*Defines the roles available in the system.*

| **Field**      | **Type** | **Size**   | **Constraint**    | **Description**           |
|----------------|----------|------------|-------------------|---------------------------|
| id_system_role | VARCHAR  | 10         | NOT NULL          | Role identifier           |
| name           | VARCHAR  | 50         | UNIQUE / NOT NULL | Role name                 |
| description    | VARCHAR  | 200        | —                 | Description               |
| enabled        | BOOLEAN  |            | NOT NULL          | Indicates whether it is enabled |

**Relationships:** SystemRole 1:N UserSystemRole • SystemRole 1:N SystemRolePermission

### UserSystemRole — Security

*Relates users to system roles.*

| **Field**      | **Type**  | **Size**   | **Constraint**  | **Description**     |
|----------------|-----------|------------|-----------------|---------------------|
| user_id        | VARCHAR   | 10         | FK / NOT NULL   | Related user        |
| system_role_id | VARCHAR   | 10         | FK / NOT NULL   | Assigned role       |
| assigned_at    | TIMESTAMP |            | NOT NULL        | Assignment date     |

**Relationships:** UserSystemRole N:1 User • UserSystemRole N:1 SystemRole

### Permission — Security

*Defines the functional permissions of the system.*

| **Field**     | **Type** | **Size**   | **Constraint**    | **Description**           |
|---------------|----------|------------|-------------------|---------------------------|
| id_permission | VARCHAR  | 10         | NOT NULL          | Permission identifier     |
| code          | VARCHAR  | 30         | UNIQUE / NOT NULL | Unique code               |
| name          | VARCHAR  | 50         | NOT NULL          | Permission name           |
| description   | VARCHAR  | 100        | —                 | Description               |

**Relationships:** Permission 1:N SystemRolePermission

### SystemRolePermission — Security

*Relates roles to permissions.*

| **Field**      | **Type**  | **Size**   | **Constraint**  | **Description**     |
|----------------|-----------|------------|-----------------|---------------------|
| system_role_id | VARCHAR   | 10         | FK / NOT NULL   | Related role        |
| permission_id  | VARCHAR   | 10         | FK / NOT NULL   | Assigned permission |
| assigned_at    | TIMESTAMP |            | NOT NULL        | Assignment date     |

**Relationships:** SystemRolePermission N:1 SystemRole • SystemRolePermission N:1 Permission

### AuditLog — Security

*Records the relevant actions executed by users.*

| **Field**    | **Type** | **Size**   | **Constraint**  | **Description**               |
|--------------|----------|------------|-----------------|-------------------------------|
| id_audit_log | VARCHAR  | 10         | NOT NULL        | Record identifier             |
| user_id      | VARCHAR  | 10         | FK              | User who executed the action  |
| action       | ENUM     |            | NOT NULL        | Audited action                |
| description  | VARCHAR  | 255        | —               | Description                   |
| ip_address   | VARCHAR  | 45         | —               | IP address                    |
| application  | VARCHAR  | 255        | —               | Source application            |

**Relationships:** AuditLog N:1 User

### HomeType — Home Management

*Catalog of household types.*

| **Field**    | **Type** | **Size**   | **Constraint**    | **Description**          |
|--------------|----------|------------|-------------------|--------------------------|
| id_home_type | VARCHAR  | 10         | NOT NULL          | Type identifier          |
| name         | VARCHAR  | 50         | UNIQUE / NOT NULL | Household type name      |

**Relationships:** HomeType 1:N Home

### Home — Home Management

*Records the households managed by the system.*

| **Field**     | **Type**  | **Size**   | **Constraint**    | **Description**   |
|---------------|-----------|------------|-------------------|-------------------|
| id_home       | VARCHAR   | 10         | NOT NULL          | Household code    |
| name          | VARCHAR   | 50         | NOT NULL          | Household name    |
| home_type_id  | VARCHAR   | 10         | FK / NOT NULL     | Household type    |
| address       | VARCHAR   | 200        | NOT NULL          | Address           |
| access_code   | VARCHAR   | 8          | UNIQUE / NOT NULL | Access code       |
| description   | VARCHAR   | 200        | —                 | Description       |
| creation_date | TIMESTAMP |            | NOT NULL          | Creation date     |

**Relationships:** Home N:1 HomeType • Home 1:1 HomeThresholds • Home 1:N UserHome • Home 1:N DeviceHome • Home 1:N Alert • Home 1:N Recommendation

### HomeThresholds — Home Management

*Configures a household's consumption limits.*

| **Field**          | **Type** | **Size**   | **Constraint**         | **Description**             |
|--------------------|----------|------------|------------------------|-----------------------------|
| id_threshold       | VARCHAR  | 10         | NOT NULL               | Threshold identifier        |
| home_id            | VARCHAR  | 10         | FK / UNIQUE / NOT NULL | Configured household        |
| daily_limit        | DOUBLE   |            | NOT NULL               | Daily consumption limit     |
| monthly_limit      | DOUBLE   |            | NOT NULL               | Monthly consumption limit   |
| use_system_default | BOOLEAN  |            | NOT NULL               | Uses default limits         |

**Relationships:** HomeThresholds 1:1 Home

### UserHome — Home Management

*Relates users to households and defines their role.*

| **Field** | **Type** | **Size**   | **Constraint**  | **Description**                |
|-----------|----------|------------|-----------------|--------------------------------|
| user_id   | VARCHAR  | 10         | NOT NULL        | Related user                   |
| home_id   | VARCHAR  | 10         | FK / NOT NULL   | Related household              |
| role      | ENUM     |            | NOT NULL        | User's role in the household   |
| favorite  | BOOLEAN  |            | NOT NULL        | Indicates whether it is the favorite household |

**Relationships:** UserHome N:1 User • UserHome N:1 Home

### ApplianceType — Devices

*Catalog of appliance types.*

| **Field**         | **Type** | **Size**   | **Constraint**    | **Description**        |
|-------------------|----------|------------|-------------------|------------------------|
| id_appliance_type | VARCHAR  | 10         | NOT NULL          | Type identifier        |
| name              | VARCHAR  | 50         | UNIQUE / NOT NULL | Type name              |

**Relationships:** ApplianceType 1:N Device

### Device — Devices

*Records the installed measurement devices.*

| **Field**         | **Type**  | **Size**   | **Constraint**    | **Description**               |
|-------------------|-----------|------------|-------------------|-------------------------------|
| id_device         | VARCHAR   | 10         | NOT NULL          | Device identifier             |
| name              | VARCHAR   | 50         | NOT NULL          | Name                          |
| appliance_type_id | VARCHAR   | 10         | FK / NOT NULL     | Appliance type                |
| location          | VARCHAR   | 50         | —                 | Location                      |
| description       | VARCHAR   | 200        | —                 | Description                   |
| installation_date | TIMESTAMP |            | NOT NULL          | Installation date             |
| device_code       | VARCHAR   | 6          | UNIQUE / NOT NULL | Device code                   |
| api_key           | VARCHAR   | 100        | NOT NULL          | Integration key               |

**Relationships:** Device N:1 ApplianceType • Device 1:N DeviceStatusLog • Device 1:N DeviceHome • Device 1:N Measurement • Device 1:N Alert • Device 1:N Recommendation

### DeviceStatusLog — Devices

*Records the status and connectivity of the devices.*

| **Field**        | **Type**  | **Size**   | **Constraint**  | **Description**            |
|------------------|-----------|------------|-----------------|----------------------------|
| id_device_status | VARCHAR   | 10         | NOT NULL        | Record identifier          |
| device_id        | VARCHAR   | 10         | FK              | Related device             |
| status           | ENUM      |            | NOT NULL        | Device status              |
| signal_strength  | INT       |            | —               | Signal strength            |
| last_seen        | TIMESTAMP |            | NOT NULL        | Last connection date       |

**Relationships:** DeviceStatusLog N:1 Device

### DeviceHome — Devices

*Relates devices to households.*

| **Field** | **Type** | **Size**   | **Constraint**  | **Description**         |
|-----------|----------|------------|-----------------|-------------------------|
| device_id | VARCHAR  | 10         | FK / NOT NULL   | Related device          |
| home_id   | VARCHAR  | 10         | NOT NULL        | Related household       |

**Relationships:** DeviceHome N:1 Device • DeviceHome N:1 Home

### Measurement — Monitoring and measurements

*Stores the recorded electrical measurements.*

| **Field**      | **Type**  | **Size**   | **Constraint**  | **Description**                    |
|----------------|-----------|------------|-----------------|------------------------------------|
| id_measurement | VARCHAR   | 10         | NOT NULL        | Measurement identifier             |
| device_id      | VARCHAR   | 10         | NOT NULL        | Device that generates the measurement |
| date_time      | TIMESTAMP |            | NOT NULL        | Date and time of the measurement   |
| voltage        | DOUBLE    |            | NOT NULL        | Measured voltage                   |
| current        | DOUBLE    |            | NOT NULL        | Measured current                   |
| active_power   | DOUBLE    |            | NOT NULL        | Active power                       |
| stored_energy  | DOUBLE    |            | NOT NULL        | Accumulated energy                 |

**Relationships:** Measurement N:1 Device

### ConsumptionLevel — Monitoring and measurements

*Defines risk levels and their consumption ranges.*

| **Field**            | **Type** | **Size**   | **Constraint**    | **Description**         |
|----------------------|----------|------------|-------------------|-------------------------|
| id_consumption_level | VARCHAR  | 10         | NOT NULL          | Level identifier        |
| name                 | ENUM     |            | UNIQUE / NOT NULL | Risk level              |
| description          | VARCHAR  | 200        | NOT NULL          | Description             |
| min_limit            | DOUBLE   |            | NOT NULL          | Lower limit             |
| max_limit            | DOUBLE   |            | NOT NULL          | Upper limit             |

**Relationships:** ConsumptionLevel 1:N Alert

### Alert — Alerts

*Records threshold and connectivity alerts.*

| **Field**            | **Type**  | **Size**   | **Constraint**  | **Description**            |
|----------------------|-----------|------------|-----------------|----------------------------|
| id_alert             | VARCHAR   | 10         | NOT NULL        | Alert identifier           |
| home_id              | VARCHAR   | 10         | NOT NULL        | Household it applies to    |
| device_id            | VARCHAR   | 10         | —               | Related device             |
| type                 | ENUM      |            | NOT NULL        | Alert type                 |
| message_key          | VARCHAR   | 100        | NOT NULL        | Message key                |
| date_time            | TIMESTAMP |            | NOT NULL        | Date and time              |
| alert_status         | ENUM      |            | NOT NULL        | Alert status               |
| consumption_level_id | VARCHAR   | 10         | —               | Associated consumption level |
| measurement_id       | VARCHAR   | 10         | —               | Associated measurement     |

**Relationships:** Alert N:1 Home • Alert N:1 Device • Alert N:1 ConsumptionLevel • Alert N:1 Measurement

### Recommendation — Recommendations

*Records recommendations for households and devices.*

| **Field**         | **Type**  | **Size**   | **Constraint**  | **Description**                   |
|-------------------|-----------|------------|-----------------|-----------------------------------|
| id_recommendation | VARCHAR   | 10         | NOT NULL        | Recommendation identifier         |
| home_id           | VARCHAR   | 10         | NOT NULL        | Household it applies to           |
| device_id         | VARCHAR   | 10         | —               | Related device                    |
| message_key       | VARCHAR   | 100        | NOT NULL        | Message key                       |
| date_time         | TIMESTAMP |            | NOT NULL        | Date and time                     |
| status            | ENUM      |            | NOT NULL        | Read status                       |
| type              | ENUM      |            | NOT NULL        | Recommendation type               |

**Relationships:** Recommendation N:1 Home • Recommendation N:1 Device

### Notification — Notifications

*Record of the delivery of a message to the user. Alert and Recommendation are events; Notification records whether the message was delivered, through which channel and its read status.*

| **Field**       | **Type**  | **Size**             | **Constraint**  | **Description** |
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

**Relationships:** Notification N:1 User (application reference) • Notification N:1 Home (application reference) • Notification references Alert/Recommendation by (source_type, source_id)

# 12. Technical modules

## 12.1 Overview of the modules

The backend is divided into seven modules (bounded contexts). Five of them are operational and two remain as a skeleton reserved for future features.

| **Module**     | **Responsibility**                                                       | **Controllers / use cases**                                                    | **Status** |
|----------------|--------------------------------------------------------------------------|--------------------------------------------------------------------------------|------------|
| security       | Registration, authentication, sessions, passwords, profile, RBAC and audit | AuthController + 15 use cases                                                | Active     |
| home           | Households, members, favorites, access code and thresholds               | HomeController, HomeThresholdsController, HomeTypeController + 11 use cases    | Active     |
| measurement    | Measurement recording, history, latest measurement and statistics        | MeasurementController, ConsumptionLevelController + 6 use cases                | Active     |
| alert          | Automatic alert generation and query/resolution inbox                    | AlertController + 4 use cases                                                  | Active     |
| notification   | Sending and logging of verification and recovery emails                  | No controller of its own (consumed from security)                              | Active     |
| device         | Measuring devices, API key and connection status                         | No implementation (only package-info.java)                                     | Skeleton   |
| recommendation | Saving recommendations generated by consumption analysis                 | No implementation (only package-info.java)                                     | Skeleton   |

## 12.2 Domain class diagram

The class diagram models the classes, interfaces and enumerations of the domain, together with the planned design patterns: Strategy for role-based authorization, Observer for the reaction to measurements and Factory Method for the creation of alerts, recommendations and notifications. The note included in the diagram indicates that the Strategy pattern classes do not yet exist in the Java code.

> **Note:** like the ERD, the complete diagram is presented on a landscape page. Appendix C breaks it down by domain to make it easier to read.
>
**Figure 4.** *Domain class diagram, including the Strategy, Observer and Factory Method patterns.*

## 12.3 Backend modules by context

### 12.3.1 security module

**Function:** Identity, authentication, sessions, access control and audit.

**Status:** Active

**Related files:**

- security/adapter/in/web/AuthController.java (single authentication and profile controller)

- security/adapter/in/web/dto/ (18 DTOs: RegisterRequest, LoginRequest, LoginResponse, RefreshResponse, ProfileResponse, ApiError…)

- security/application/usecase/ (RegisterUserService, AuthenticateUserService, RefreshSessionService, LogoutUserSessionService, ChangePasswordService, CreatePasswordResetTokenService, EmailVerificationService, UpdateUserProfileService, CheckPermissionService, AssignRoleService, ManageUserStatusService…)

- security/domain/model/ (User, Person, Email, PasswordHash, PasswordPolicy, UserSession, RefreshToken, SystemRole, Permission, AuditLog, LoginErrorLog)

- security/infrastructure/ (SecurityFilterChainConfiguration, JwtTokenIssuer, PemRsaKeys, SecurityCorsConfiguration, JwtAuthenticationEntryPoint)

- security/adapter/out/security/ (BCryptPasswordHasherAdapter, Sha256TokenHasher, PepperedHmacResetCodeHasher, InMemoryResetAttemptLimiter)

**Related tables:** person, user, user_configuration, password_reset_token, user_session, refresh_token, security_configuration, password_policy, login_error_log, system_role, permission, user_system_role, system_role_permission, audit_log.

**Main operations:**

- Register an account and verify the email

- Log in and renew tokens

- Log out

- Recover and change passwords

- View and update the profile

- Query permissions (RBAC)

- Record audit and failed attempts

### 12.3.2 home module

**Function:** Management of households, members, favorites and consumption thresholds.

**Status:** Active

**Related files:**

- home/adapter/in/web/HomeController.java, HomeThresholdsController.java, HomeTypeController.java

- home/adapter/in/web/CurrentUserResolver.java (resolves the user from the X-User-Id header)

- home/application/usecase/ (CreateHomeService, JoinHomeService, LeaveHomeService, ListHomesService, ListMembersService, RemoveUserService, ToggleFavoriteService, GetHomeThresholdsService, UpdateHomeThresholdsService, ListHomeTypesService)

- home/domain/model/ (Home, HomeType, HomeThresholds, UserHome, Role OWNER/MEMBER, OwnerPermissions, MemberPermissions)

- home/infrastructure/HomeApiImpl.java (public API consumed by other modules)

**Related tables:** home, home_type, home_thresholds, user_home.

**Main operations:**

- Create a household and generate an access code

- Join a household with a code

- List households and members

- Mark as favorite

- Remove members and leave the household

- View and update daily and monthly thresholds

### 12.3.3 measurement module

**Function:** Recording and analysis of electrical consumption measurements.

**Status:** Active

**Related files:**

- measurement/adapter/in/web/MeasurementController.java, ConsumptionLevelController.java

- measurement/application/usecase/ (RegisterMeasurementService, ListMeasurementsService, GetLatestMeasurementService, GetConsumptionStatisticsService, ListConsumptionLevelsService, TimeRange)

- measurement/domain/model/ (Measurement, ConsumptionLevel)

- measurement/adapter/out/event/MeasurementEventAdapter.java (publishes MeasurementRecorded)

**Related tables:** measurement, consumption_level.

**Main operations:**

- Record measurements (voltage, current, active power, energy)

- Query history by device and date range

- Get the latest measurement

- Calculate consumption statistics

- Publish the MeasurementRecorded event

### 12.3.4 alert module

**Function:** Generation and inbox of threshold and connectivity alerts.

**Status:** Active

**Related files:**

- alert/adapter/in/web/AlertController.java (read and resolve only)

- alert/adapter/in/event/MeasurementRecordedListener.java (@EventListener)

- alert/application/usecase/ (ListAlertsService, GetAlertService, ResolveAlertService, RegisterThresholdAlertService)

- alert/infrastructure/AlertApiImpl.java and alert/api/AlertRaised.java

**Related tables:** alert.

**Main operations:**

- Automatically generate alerts upon receiving a measurement

- List alerts by household and status

- Query an alert

- Resolve an alert

### 12.3.5 notification module

**Function:** Logging of email deliveries (verification and recovery).

**Status:** Active

**Related files:**

- notification/infrastructure/mail/EmailSender.java, MailDeliveryConfiguration.java, AbandonedDeliverySweeper.java

- notification/application/usecase/ (SendPasswordResetEmail, SendEmailVerification)

- notification/adapter/out/client/EmailPasswordResetDeliveryAdapter.java, EmailVerificationDeliveryAdapter.java

- notification/adapter/out/persistence/NotificationPersistenceAdapter.java

**Related tables:** notification.

**Main operations:**

- Send verification and recovery emails

- Record the delivery status (PENDING, SENT, FAILED)

- Sweep abandoned deliveries when the application starts

### 12.3.6 device module

**Function:** Registration and connection of measuring devices (API key, online/offline status).

**Status:** Skeleton: only package-info.java (the table schema already exists in Liquibase)

**Related files:**

- device/package-info.java (@ApplicationModule with allowedDependencies = { "home" })

**Related tables:** device, appliance_type, device_status_log, device_home.

**Main operations:**

- (Pending) Register device

- (Pending) Authenticate device with api_key

- (Pending) Record connection status

### 12.3.7 recommendation module


**Function:** Saving recommendations generated from consumption analysis.

**Status:** Skeleton: only package-info.java (recommendation table already created by Liquibase)

**Related files:**

- recommendation/package-info.java (@ApplicationModule with allowedDependencies = { "device", "measurement" })

**Related tables:** recommendation.

**Main operations:**

- (Pending) Analyze measurement history

- (Pending) Generate and mark recommendations

## 12.4 Web front-end module

**Function:** interface for querying and managing electrical consumption (React SPA)

**Related files:**

- src/routes/AppRoutes.jsx (routes), ProtectedRoute.jsx (protected routes) and HomeRoute.jsx (household detail)

- src/services/http/httpClient.js (axios client, interceptors and token renewal)

- src/services/auth, src/services/home and src/services/alerts (consumed endpoints)

- src/context/HomeContext.jsx and ThemeContext.jsx (global state)

- src/features/ (landingPage, auth, dashboard, DetailHome, Favorites, Notifications, Account, SettingsPage)

**Screens and routes:**

| **Route**                                                 | **Screen**                  | **Function**                                                                 |
|-----------------------------------------------------------|-----------------------------|------------------------------------------------------------------------------|
| /home, /about, /support                                   | Landing page                | Public presentation, about and support pages.                                |
| /login, /register                                         | Login and Registration      | Authentication and account creation with zod validation.                     |
| /VerifyAccount, /recover-password, /verifyRecoverPassword | Verification and recovery   | Email verification code and password recovery.                               |
| /dashboard                                                | Household panel             | Household cards, create household, join with code and favorites.             |
| /homes/:homeId                                            | Household detail            | Six tabs: Consumption, History, Users, Devices, Thresholds and Household.    |
| /favorites                                                | Favorites                   | List of households marked as favorites.                                      |
| /notifications                                            | Notifications               | Household alerts and the action to resolve them.                             |
| /settings                                                 | Settings                    | Appearance, language, notifications and help.                                |
| /account                                                  | Account                     | Profile editing, photo, password and account deletion.                       |

**Main operations:** log in and log out, register and verify account, recover password, create and join households, mark favorites, view and edit members and thresholds, view alerts and resolve them, change language and theme.

## 12.5 Mobile front-end module

**Function:** hybrid application with the same domain model as the web

**Related files:**

- src/app/ (Expo Router routes: \_layout, index, dashboard, home/\[id\], favorites, notifications, settings, account)

- src/features/ (auth, dashboard, homes, favorites, notifications, settings, account)

- src/context/ (HomeContext, UserContext, ThemeContext)

- src/components/ and src/constants/ (own design system with StyleSheet)

**Main operations:** registration, code verification, password recovery, household panel, household detail with six tabs, favorites, notifications, language and theme settings, and account management.

> **Known technical limitation:** the mobile application does not yet consume the REST API. The login navigates directly to the panel and the consumption, device, member and notification data come from demonstration files (features/\*/data). The integration is declared as pending in the code itself.

# 13. API or system routes

**Base URL:** http://localhost:8080/api/v1 \| **Format:** JSON \| **Interactive documentation:** /swagger-ui.html and /v3/api-docs

The backend exposes 35 routes. All those that do not appear in the list of public routes require a valid JWT in the Authorization header; if the token is missing, expired or invalid, the API responds 401, and if the user does not have the required permission, it responds 403.

| **Method** | **Route**                               | **Description**                                             | **Parameters**                                                        | **Authentication** |
|------------|-----------------------------------------|-------------------------------------------------------------|-----------------------------------------------------------------------|--------------------|
| POST       | /api/v1/auth/register                   | Registers a new account.                                    | body: email, password, name, lastName, profileImage (optional)        | Public             |
| POST       | /api/v1/auth/email/verify               | Verifies the email with a 6-digit code.                     | body: email, code                                                     | Public             |
| POST       | /api/v1/auth/email/verification/resend  | Resends the verification code (always responds 202).        | body: email                                                           | Public             |
| POST       | /api/v1/auth/login                      | Authenticates and returns access token and refresh token.   | body: email, password                                                 | Public             |
| POST       | /api/v1/auth/refresh                    | Renews the access token and rotates the refresh token.      | body: refreshToken                                                    | Public             |
| POST       | /api/v1/auth/logout                     | Closes the registered session.                              | body: idUserSession                                                   | JWT                |
| POST       | /api/v1/auth/password/forgot            | Requests the password recovery code.                        | body: email                                                           | Public             |
| POST       | /api/v1/auth/password/reset             | Resets the password with the received code.                 | body: email, resetToken, newPassword                                  | Public             |
| POST       | /api/v1/auth/password/change            | Changes the password knowing the current one.               | body: currentPassword, newPassword                                    | JWT                |
| PUT        | /api/v1/auth/profile                    | Updates first name, last name, image or email.              | body: name, lastName, profileImage, newEmail                          | JWT                |
| GET        | /api/v1/auth/account                    | Returns the authenticated account.                          | none (identity from the JWT)                                          | JWT                |
| GET        | /api/v1/auth/permissions                | Lists the user's permission codes.                          | none                                                                  | JWT                |
| POST       | /api/v1/measurements                    | Records a consumption measurement.                          | body: deviceId, dateTime, voltage, current, activePower, storedEnergy | JWT                |
| GET        | /api/v1/measurements                    | Lists a device's measurements in a range.                   | query: deviceId (required), from, to                                  | JWT                |
| GET        | /api/v1/measurements/latest             | Returns a device's latest measurement.                      | query: deviceId                                                       | JWT                |
| GET        | /api/v1/measurements/statistics         | Consumption statistics for the period.                      | query: deviceId, from, to                                             | JWT                |
| GET        | /api/v1/consumption-levels              | Lists the consumption levels (LOW, MEDIUM, HIGH, CRITICAL). | none                                                                  | JWT                |
| POST       | /api/v1/homes                           | Creates a household and returns its access code.            | body: name, homeTypeId, address, description                          | JWT + X-User-Id    |
| GET        | /api/v1/homes                           | Lists the user's households with their role.                | none                                                                  | JWT + X-User-Id    |
| GET        | /api/v1/homes/{homeId}/members          | Lists the members of a household.                           | path: homeId                                                          | JWT + X-User-Id    |
| POST       | /api/v1/homes/join                      | Allows joining a household with its access code.            | body: accessCode                                                      | JWT + X-User-Id    |
| PUT        | /api/v1/homes/{homeId}/favorite         | Marks or unmarks a household as a favorite.                 | path: homeId                                                          | JWT + X-User-Id    |
| DELETE     | /api/v1/homes/{homeId}/members/{userId} | Removes a member (OWNER only).                              | path: homeId, userId                                                  | JWT + X-User-Id    |
| DELETE     | /api/v1/homes/{homeId}/members/me       | Allows the user to leave the household.                     | path: homeId                                                          | JWT + X-User-Id    |
| GET        | /api/v1/homes/{homeId}/thresholds       | Queries the household's daily and monthly thresholds.       | path: homeId                                                          | JWT + X-User-Id    |
| PUT        | /api/v1/homes/{homeId}/thresholds       | Updates the household's thresholds.                         | path: homeId; body: dailyLimit, monthlyLimit                          | JWT + X-User-Id    |
| GET        | /api/v1/home-types                      | Lists the available household types.                        | none                                                                  | JWT                |
| GET        | /api/v1/home-types/{idHomeType}         | Queries a household type.                                   | path: idHomeType                                                      | JWT                |
| GET        | /api/v1/alerts                          | Lists a household's alerts.                                 | query: homeId (required), status (PENDING \| RESOLVED)                | JWT                |
| GET        | /api/v1/alerts/{idAlert}                | Queries an alert by its identifier.                         | path: idAlert                                                         | JWT                |
| PUT        | /api/v1/alerts/{idAlert}/resolve        | Marks an alert as resolved.                                 | path: idAlert                                                         | JWT                |
| GET        | /actuator/health                        | Service status (UP / DOWN).                                 | none                                                                  | Public             |
| GET        | /actuator/info                          | Application information.                                    | none                                                                  | Public             |
| GET        | /swagger-ui.html and /v3/api-docs       | Interactive API documentation (OpenAPI).                    | none                                                                  | Public             |
| OPTIONS    | /\*\*                                   | CORS preflight.                                             | Origin and Access-Control-Request-\* headers                          | Public             |

## 13.1 Common response codes

| **Code**        | **When it occurs**                                                            | **Response structure**                                         |
|-----------------|-------------------------------------------------------------------------------|----------------------------------------------------------------|
| 200 / 201 / 204 | Successful operation (query, creation or deletion with no content).           | Body with the corresponding resource or message.               |
| 400             | Incorrect data validation (Bean Validation or zod schema on the client).      | ApiError with fieldErrors detailing each field.                |
| 401             | Missing or expired token, or invalid credentials.                             | ApiError with a generic message (does not reveal whether the email exists). |
| 403             | Blocked or inactive account, or insufficient permission.                      | ApiError.                                                      |
| 404             | Nonexistent resource (household, alert, member…).                             | ApiError or {"error": "..."} depending on the module.          |
| 409             | Conflict (email already registered, last owner who cannot leave).             | ApiError.                                                      |
| 422             | The password does not meet the security policy.                               | ApiError.                                                      |
| 429             | Attempt limit reached in verification or recovery.                            | ApiError with Retry-After header (900 seconds).                |
| 500             | Unhandled internal error.                                                     | ApiError without stack trace (a stack trace is never leaked).  |

## 13.2 Request and response example

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

# 14. Security

## 14.1 Implemented controls

| **Control**                   | **Technical implementation**                                                                                                                                                                            |
|-------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Authentication                | JWT signed with the RS256 algorithm and an RSA key pair in PEM files; access token lifetime: 15 minutes; claims sub (idUser), person_id and email.                                                      |
| Sessions                      | Stateless (STATELESS): no cookies or HTTP session; CSRF disabled because there is no ambient credential.                                                                                                |
| Session renewal               | Opaque refresh token rotated on every use, stored as a SHA-256 hash with family_id and parent_id to detect reuse (replay) and revoked when the password is changed.                                      |
| Password encryption           | BCrypt (BCryptPasswordHasherAdapter) isolated from the domain; passwords are never stored in plain text.                                                                                                |
| Password policy               | password_policy table: minimum length 8 and maximum 64, requires uppercase letters, numbers and symbols, expires after 90 days. Non-compliance =\> 422.                                                 |
| Authorization (RBAC)          | Chain of five tables (system_role, permission, user_system_role, system_role_permission and user) queried live by CheckPermissionService; permissions can be queried at GET /api/v1/auth/permissions.    |
| Input validation              | Bean Validation in the backend DTOs (@NotBlank, @Size, @NotNull, @PositiveOrZero) and zod schemas in the front-end forms.                                                                               |
| CORS                          | Explicit list of origins (SECURITY_CORS_ALLOWED_ORIGINS), no wildcards, no credentials and with OPTIONS preflight handling.                                                                             |
| Attempt limit                 | In-memory limiter: 10 attempts per IP every 15 minutes in password recovery and verification =\> 429 with Retry-After header.                                                                           |
| Enumeration protection        | Failed login with a generic message and password/forgot with an identical 202 response whether or not the account exists; the work is done asynchronously.                                              |
| Recovery codes                | 6 digits, valid for 15 minutes, single use, stored as HMAC-SHA256 with pepper (SECURITY_RESET_PEPPER).                                                                                                  |
| Audit                         | Tables audit_log (actions, IP, user) and login_error_log (failed attempts); the IP is taken from getRemoteAddr() ignoring X-Forwarded-For.                                                              |
| Documentation                 | The OpenAPI endpoint declares the bearerAuth scheme; the public endpoints are only the authentication and infrastructure ones.                                                                          |
| Transport                     | In production it must be served over HTTPS; in local development HTTP is used with Mailpit as the test mail server.                                                                                     |

## 14.2 Known limitations and risks

> The following points are declared by the team itself in the code and must be resolved before taking the system to production.

| **Limitation**                                                                                                                       | **Impact**                                                                                      | **Recommendation**                                                                              |
|--------------------------------------------------------------------------------------------------------------------------------------|-------------------------------------------------------------------------------------------------|-------------------------------------------------------------------------------------------------|
| X-User-Id header used by the home module to identify the user (CurrentUserResolver is marked as temporary and insecure).            | A client could send another identifier and impersonate the user in household operations.        | Resolve the identity from the JWT sub claim and remove the header.                              |
| Access token and refresh token stored in localStorage (web).                                                                         | An XSS vulnerability could read the tokens.                                                     | Migrate to httpOnly cookies with SameSite or strengthen the Content Security Policy.            |
| In-memory attempt limiter.                                                                                                           | It resets with every start-up and is not shared between instances.                              | Use Redis or another shared store if the system is scaled horizontally.                         |
| Device module not implemented.                                                                                                       | There is no device authentication or recording of its status.                                   | Implement the API key and the connection validity described in RF5.                             |
| Recommendation module not implemented.                                                                                               | The recommendations functionality of RF4 is not operational.                                    | Define the consumption analyzer based on the MeasurementRecorded events.                        |
| Demonstration data in the front-ends (consumption, history and devices).                                                             | The interface shows information that does not come from the database.                           | Replace the demonstration data with calls to /measurements and /measurements/statistics.        |
| Mobile application without integration with the API.                                                                                 | The mobile app does not reflect the user's real information.                                    | Implement the services layer with axios and the JWT session, replicating the web front pattern. |

## 14.3 Best practices for developers

- Never commit the .env files, application-local.yaml or the RSA keys (\*.pem); both are already declared in .gitignore.

- Do not log passwords, tokens or recovery codes in the application logs.

- Keep the backend and front-end dependencies up to date and review their security advisories.

- Every new feature must validate input in the backend, regardless of the client's validations.

- Respect the module boundaries: do not import classes from another context without going through its public API (verified by ModularityTests).

- Rotate the JWT keys and the recovery pepper if a leak is suspected.

# 15. Software version control

The project uses Git with a remote repository on GitHub. Each component of the system is developed in an independent repository to facilitate its deployment and review.

| **Repository**                                      | **Content**                                                                                                                       | **Remarks**                                                                               |
|-----------------------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------|-------------------------------------------------------------------------------------------|
| energy-monitor-backend                              | REST API in Java 21 / Spring Boot.                                                                                                | Includes tests, docker-compose.yml, .env.example and the Liquibase migration history.     |
| Energy-Monitor-application                          | React 19 / Vite 8 web application.                                                                                                | Includes Vitest tests (11 files) and production build in /dist.                           |
| Energy-Monitor-Mobile                               | Expo SDK 57 mobile application.                                                                                                   | Includes LICENSE, AGENTS.md (versioned Expo 57 documentation) and Expo scripts.           |
| Documentacion-proyecto-Monitor-de-consumo-electrico | SRS, technical report, diagrams (use cases, activities, navigation, ERD, classes), mockups, wireframes and data dictionary.       | Change log in commits.md.                                                                 |

- Changes are documented in commits.md with identifier, date and change message, to trace the project's evolution.

- Before merging changes, the backend test suite (mvnw test) and the web front tests (npm test) must be run.

- Database schema changes are made only through a new Liquibase changeset; changesets that have already been applied are never edited.

- Integration branches must keep the code in a compilable state and the tests green.

- Delivered versions are tagged to preserve the exact reference of the software documented in this manual.

# 16. Maintenance recommendations

## 16.1 Code updates

| **Area**      | **Recommendation**                                                                                                                                                              |
|---------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Backend       | Build with .\mvnw.cmd clean test before each delivery; verify that ModularityTests confirms that the module boundaries were not broken.                                         |
| Database      | Create a new changeset for each schema modification, with a sequential identifier (e.g. security-022); run docker compose up -d to test against a real MySQL 8.4.               |
| Dependencies  | Update Spring Boot, Expo or React in test branches, running the full suite before merging.                                                                                      |
| Web front     | Keep the VITE_API_BASE_URL variable and the list of allowed origins (SECURITY_CORS_ALLOWED_ORIGINS) in sync; run npm test and npm run lint again.                              |
| Mobile front  | Review the versioned Expo 57 documentation before incorporating native libraries; there are no automated tests, so verification is manual.                                      |
| Security      | Review the RBAC permissions, the password policy and the validity of the JWT keys quarterly.                                                                                    |

## 16.2 Database backups

Before each relevant update, the energy_monitor schema must be backed up. Recommended procedure:

1.  Stop writing information (do not run measurement loads during the backup).

2.  Run the dump: mysqldump -h localhost -u energy_app -p energy_monitor \> backup_energy_monitor_YYYY-MM-DD.sql

3.  Save the file in a secure folder outside the repository, with the date in the name.

4.  Verify the backup by restoring it into a test database before considering it valid.

5.  Restore: mysql -h localhost -u energy_app -p energy_monitor \< backup_energy_monitor_YYYY-MM-DD.sql

## 16.3 Support and incident resolution

| **Symptom**                             | **Suggested check**                                                                                                                  |
|-----------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------|
| The backend does not start              | Check port 8080 (netstat), the MySQL container (docker compose ps) and the SPRING_DATASOURCE_PASSWORD variable.                      |
| The API responds 403/401 massively      | Verify the RSA key pair (SECURITY_JWT_PRIVATE_KEY_PATH / PUBLIC) and the server date and time.                                       |
| The web front does not connect to the API | Confirm that the backend responds on the indicated port and that SECURITY_CORS_ALLOWED_ORIGINS includes the origin http://localhost:5173. |
| Emails do not arrive                    | Check MAIL_HOST, MAIL_PORT, MAIL_USERNAME and MAIL_PASSWORD; locally use Mailpit (http://localhost:8025).                            |
| Migrations fail at startup              | Check the Liquibase log and the order of the changesets in db.changelog-master.xml.                                                  |
| Error messages are not readable         | Check the browser console and the ApiError body returned by the API.                                                                 |

## 16.4 Declared pending work

- Remove the X-User-Id header and use exclusively the JWT identity (home module).

- Implement the device and recommendation modules, currently declared only as a skeleton.

- Connect the consumption and history charts of the web front to the measurement endpoints.

- Integrate the mobile application with the REST API (session, households, consumption and alerts).

- Expose over HTTP the administrative operations on roles and user statuses that already exist as application ports.

- Replace the demonstration data of the mobile app and complete its automated tests.

# 17. Glossary

| **Term**              | **Definition**                                                                                                                        |
|-----------------------|---------------------------------------------------------------------------------------------------------------------------------------|
| ADSO                  | Análisis y Desarrollo de Software (Software Analysis and Development): SENA technology program that is part of the project (ficha 3145556). |
| REST API              | HTTP-based programming interface that exposes resources in JSON format under versioned routes (/api/v1).                              |
| Access token          | Short-lived JWT token (15 minutes) that proves the user's identity on each request.                                                   |
| Bounded context       | Bounded domain context (security, home, measurement…) with its own schema and internal API.                                           |
| BCrypt                | Salted hash algorithm used to store passwords irreversibly.                                                                           |
| CORS                  | HTTP policy that controls from which origins (e.g. http://localhost:5173) the browser may call the API.                               |
| Changeset             | Liquibase migration unit: versioned instruction to create or modify tables.                                                           |
| Docker Compose        | Tool that defines and starts containers (for example mysql:8.4) with a single command.                                                |
| DTO                   | Data transfer object: JSON input or output structure of an endpoint.                                                                  |
| ER / ERD              | Entity-relationship diagram: logical model of the tables and their relationships.                                                     |
| JWT                   | JSON Web Token: signed token (RS256 algorithm) that carries the user's identity.                                                      |
| Liquibase             | Schema manager: applies database changesets in a controlled and versioned manner.                                                     |
| Module (Modulith)     | Unit of code with boundaries verified by Spring Modulith; it only relates through its public API.                                     |
| PK / Primary key      | Field that uniquely identifies a row (in this project, VARCHAR(10)).                                                                  |
| FK / Foreign key      | Field that references another table within the same context. Between contexts only IDs are stored (no SQL FK).                       |
| RBAC                  | Role-based access control: system_role, permission, user_system_role and system_role_permission.                                      |
| Refresh token         | Long-lived opaque token stored as a SHA-256 hash that allows renewing the access token (rotation and reuse detection).                |
| SHA-256               | Cryptographic hash function used to store refresh tokens and recovery codes (with HMAC pepper).                                       |
| SPA                   | Single Page Application: web application that runs entirely in the browser and navigates without reloading (React + react-router).    |
| Swagger / OpenAPI     | Interactive documentation automatically generated at /swagger-ui.html.                                                                |
| Thresholds            | Daily and monthly consumption limits configurable per household that trigger alerts.                                                  |
| Logical deletion      | No physical DELETE is executed: a value in deleted_at indicates that the record was deleted.                                          |
| X-User-Id             | Temporary header used by the home module to identify the user; it will be replaced by the JWT identity.                               |

# 18. Appendices

## Appendix A. Configuration and commands

**Environment variables.** The backend reads its secrets from a .env file (imported by spring.config.import) whose structure is documented in .env.example; the web front uses .env.local with the VITE_API_BASE_URL variable. For security reasons, no real values are published:

| **Variable**                                                 | **Description**                                                                                      |
|--------------------------------------------------------------|------------------------------------------------------------------------------------------------------|
| MYSQL_PASSWORD / MYSQL_ROOT_PASSWORD                         | Passwords of the application user and of the MySQL root user (defined in docker-compose.yml).        |
| SPRING_DATASOURCE_PASSWORD                                   | Password used by the tests with the test profile.                                                    |
| MAIL_HOST / MAIL_PORT                                        | SMTP server and port (default smtp.gmail.com:587).                                                   |
| MAIL_USERNAME / MAIL_PASSWORD / MAIL_FROM                    | Sender account and application password to send emails.                                              |
| SECURITY_RESET_PEPPER                                        | HMAC-SHA256 pepper of the password recovery codes.                                                   |
| SECURITY_CORS_ALLOWED_ORIGINS                                | Origins allowed by CORS (default http://localhost:5173).                                             |
| SECURITY_JWT_PRIVATE_KEY_PATH / SECURITY_JWT_PUBLIC_KEY_PATH | Absolute paths to the RSA key pair (private.pem and public.pem) used by RS256.                       |
| VITE_API_BASE_URL (web front)                                | Backend base URL; if not defined, http://localhost:8080 is used.                                     |

**Operation commands:**

| **Command**                             | **Description**                                                                    |
|-----------------------------------------|------------------------------------------------------------------------------------|
| docker compose up -d                    | Starts MySQL 8.4 with the energy_monitor schema.                                   |
| docker compose down -v                  | Stops the database and deletes the volume (regenerates the schema with Liquibase). |
| docker run -p 8025:8025 axllent/mailpit | Starts the test mail server (interface at http://localhost:8025).                  |
| .\mvnw.cmd spring-boot:run              | Starts the REST API at http://localhost:8080.                                      |
| .\mvnw.cmd test                         | Runs the backend test suite.                                                       |
| .\mvnw.cmd package                      | Generates the executable JAR in the /target folder.                                |
| npm install / npm run dev               | Installs dependencies and starts the web application (http://localhost:5173).      |
| npm test / npm run lint                 | Runs the tests and the static analysis of the web front.                           |
| npm run start / npm run android         | Starts the mobile application in Expo (device or emulator).                        |

**Verification after installation:** GET /actuator/health responds {"status":"UP"}, the login screen loads in the browser, registering an account sends the verification code and the households created appear in the application's panel.

# Appendix B. ERD by context

Entity-relationship model broken down by context to make it easier to read. The conventions (snake_case, VARCHAR(10) primary keys, audit columns and logical deletion) are the same as in the complete diagram in section 11.1.

**Figure 5.** *Security context: persons, users, sessions, tokens, roles, permissions and audit.*

**Figure 6.** *Home Management and Devices contexts: households, members, thresholds and devices.*

**Figure 7.** *Monitoring and measurements and Alerts contexts: measurements, consumption levels and alerts.*

**Figure 8.** *Recommendations and Notifications contexts: recommendations and delivery log.*

# Appendix C. Class diagram by domain

**Figure 9.** *Domain_Security and Domain_Access_Control domains, with the Strategy pattern and the enumerations.*

**Figure 10.** *Domain_Home_Management and Domain_Devices domains.*

**Figure 11.** *Domain_Measurements and Domain_Notifications domains, with Observer and Factory Method.*
