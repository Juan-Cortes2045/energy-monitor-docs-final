**SERVICIO NACIONAL DE APRENDIZAJE – SENA (National Learning Service)**

**CENTER FOR INDUSTRY, BUSINESS AND SERVICES (CIES)**

**HUILA REGIONAL**

Training Program: Technology in Software Analysis and Development (ADSO)

Group (Ficha): 3145556

**INSTALLATION MANUAL**

**Project: Electrical Consumption Monitor**

*Procedure for deploying the API, the web application and the mobile application on a workstation*

| **Item**             | **Information**                                                                 |
|----------------------|---------------------------------------------------------------------------------|
| Team members         | Juan Esteban Cortes Parra Miguel Angel Garcia Artunduaga                        |
| Instructor           | Motta Vargas José de Jesús                                                      |
| Training center      | Center for Industry, Business and Services (CIES) – SENA Huila Regional         |
| Program              | Technology in Software Analysis and Development (ADSO)                          |
| Group (Ficha)        | 3145556                                                                         |
| Document type        | Installation manual                                                             |
| Version              | 1.0                                                                             |
| Date prepared        | October 7, 2026                                                                 |

Year 2026

# 2. Document version control

The change history of this manual is recorded in the following table. Every modification to the installation process that affects the software must be reflected in a new version of the document.

| **Version** | **Date**   | **Description of change**                                                                                                                      | **Responsible**      |
|-------------|------------|------------------------------------------------------------------------------------------------------------------------------------------------|----------------------|
| 1.0         | 10/07/2026 | Initial creation of the installation manual for the backend, web frontend and mobile frontend, with SRS requirements and references in APA format. | Development team     |

# 3. Table of contents

[2. Document version control [1](#document-version-control)](#document-version-control)

[3. Table of contents [2](#table-of-contents)](#table-of-contents)

[4. Introduction [4](#introduction)](#introduction)

[5. Objective [4](#objective)](#objective)

[6. Technical scope [4](#technical-scope)](#technical-scope)

[7. Minimum requirements [5](#minimum-requirements)](#minimum-requirements)

[7.1 Equipment requirements (according to the SRS) [5](#equipment-requirements-according-to-the-srs)](#equipment-requirements-according-to-the-srs)

[7.2 Recommended hardware resources to run the system [5](#recommended-hardware-resources-to-run-the-system)](#recommended-hardware-resources-to-run-the-system)

[7.3 Software requirements for the installation [5](#software-requirements-for-the-installation)](#software-requirements-for-the-installation)

[8. Environment variables the installer must know [7](#environment-variables-the-installer-must-know)](#environment-variables-the-installer-must-know)

[9. Backend installation [7](#backend-installation)](#backend-installation)

[9.1 Obtain the code [7](#obtain-the-code)](#obtain-the-code)

[9.2 Define database credentials (.env) [7](#define-database-credentials-.env)](#define-database-credentials-.env)

[9.3 Start MySQL with Docker Compose [8](#start-mysql-with-docker-compose)](#start-mysql-with-docker-compose)

[9.4 Create the local application profile [8](#create-the-local-application-profile)](#create-the-local-application-profile)

[9.5 Generate the RSA key pair (JWT tokens) [8](#generate-the-rsa-key-pair-jwt-tokens)](#generate-the-rsa-key-pair-jwt-tokens)

[9.6 Generate the recovery pepper [8](#generate-the-recovery-pepper)](#generate-the-recovery-pepper)

[9.7 Decide on the recovery email [9](#decide-on-the-recovery-email)](#decide-on-the-recovery-email)

[9.8 Build and start [9](#build-and-start)](#build-and-start)

[10. Web frontend installation [10](#web-frontend-installation)](#web-frontend-installation)

[10.1 Dependencies [10](#dependencies)](#dependencies)

[10.2 Configure the backend URL [10](#configure-the-backend-url)](#configure-the-backend-url)

[10.3 Start in development [10](#start-in-development)](#start-in-development)

[10.4 CORS permission in the backend [10](#cors-permission-in-the-backend)](#cors-permission-in-the-backend)

[11. Mobile application installation [10](#mobile-application-installation)](#mobile-application-installation)

[11.1 Dependencies [10](#dependencies-1)](#dependencies-1)

[11.2 Start Expo [10](#start-expo)](#start-expo)

[11.3 Test on a physical device [11](#test-on-a-physical-device)](#test-on-a-physical-device)

[11.4 Compatibility notes [11](#compatibility-notes)](#compatibility-notes)

[12. Local email with Mailpit (optional) [11](#local-email-with-mailpit-optional)](#local-email-with-mailpit-optional)

[13. Installation verification [12](#installation-verification)](#installation-verification)

[14. Installation command summary [12](#installation-command-summary)](#installation-command-summary)

[15. Common troubleshooting [12](#common-troubleshooting)](#common-troubleshooting)

[16. References (APA format, 7th edition) [13](#references-apa-format-7th-edition)](#references-apa-format-7th-edition)

# 4. Introduction

This document is the installation manual for the "Electrical Consumption Monitor" software. Its purpose is for any developer, instructor or technician —regardless of their prior experience with the project— to be able to obtain the source code, configure the dependencies, get the three components of the system running and verify that they work together on a workstation.

The system consists of three pieces that are installed separately: a REST API (backend) built with Java 21 and Spring Boot 4.1.1, a web application built with React 19 and Vite 8, and a mobile application built with Expo SDK 57 and React Native 0.86 (Spring, n.d.; Vite, n.d.; Expo, n.d.). The MySQL 8.4 database is started with Docker Compose and the delivery of the recovery email can be simulated locally with Mailpit (Docker Inc., n.d.; Mailpit, n.d.).

The information recorded was obtained directly from the project's three repositories, the design documents (SRS, technical report) and the READMEs included in each repository, so it corresponds to the final version of the delivered software. References were formatted according to APA style, seventh edition (American Psychological Association \[APA\], 2020).

# 5. Objective

To allow a new or maintenance development team to start up the "Electrical Consumption Monitor" system on a Windows, macOS or Linux computer, from the official repositories, without depending on informal documentation or past conversations.

- List the minimum hardware, operating system and software requirements, as defined by the SRS.

- Establish exact versions of the tools that the installation process requires (Java, Node, Docker, OpenSSL).

- Describe, step by step and with the specific commands, the installation of the database, the backend, the web application and the mobile application.

- Present the configuration variables that the installer must define before the first start.

- Indicate how to verify that each component is operational and how to diagnose the most common errors.

# 6. Technical scope

The manual covers the installation on a local development machine (not production) of the following components:

- MySQL 8.4 database via Docker Compose (energy-monitor-backend).

- Backend REST API (energy-monitor-backend) with local profile and JUnit tests.

- Web application (Energy-Monitor-application) with Vite.

- Mobile application (Energy-Monitor-Mobile) with Expo and Expo Go.

- Optional utilities: Mailpit for local email and RSA keys for the JWTs.

It does not cover putting the system into operation on a production server, configuring a reverse proxy, or cloud deployment; those tasks belong to the administration manual and the project's technical report.

# 7. Minimum requirements

## 7.1 Equipment requirements (according to the SRS)

According to the interfaces section of the SRS, the software runs on computers and mobile devices in perfect condition, with an internet connection, sufficient storage to save the application and the latest available version of the operating system (Corte Parra & García Artunduaga, 2026):

- Computer operating system: Windows 10 or higher.

- Mobile operating system: the latest available version of Android.

- Up-to-date web browser: Chrome, Firefox, Edge, Safari, Brave, Vivaldi or an equivalent.

- Internet connection (the services communicate through standard HTTP/HTTPS protocols).

## 7.2 Recommended hardware resources to run the system

Beyond the equipment requirements stated in the SRS, the machine where the three components are installed must have the following minimum resources for an experience free of freezes or memory errors. The exact software versions are detailed in the table in section 7.3.

| **Resource**              | **Minimum**                                                    | **Recommended**                                                                      |
|---------------------------|----------------------------------------------------------------|--------------------------------------------------------------------------------------|
| Operating system (PC)     | 64-bit Windows 10 (with Docker Desktop and WSL 2)              | 64-bit Windows 11 or Linux (Ubuntu 24.04+)                                           |
| Operating system (mobile) | Android 10 or higher                                           | Latest available Android version                                                     |
| Processor                 | Intel Core i3 or equivalent                                    | Intel Core i5 or equivalent                                                          |
| RAM                       | 8 GB                                                           | 16 GB (Docker Desktop, MySQL, Node and design tools open comfortably)                |
| Free disk space           | 8 GB (code, node_modules, MySQL image and containers)          | 20 GB or more (for several versions, Docker caches and database files)               |
| Connectivity              | Stable internet (to clone, download packages and Expo Go)      | Broadband connection                                                                 |
| Additional storage        | At least 5 GB free for test data and MySQL backups             | Space freely available                                                               |

> *If the machine meets the minimum but not the recommendation, install only the components you need simultaneously (e.g. run the backend and web in separate terminals and do not open Android Studio at the same time).*

## 7.3 Software requirements for the installation

The minimum versions verified on the backend repository and the two frontends are:

| **Tool**        | **Minimum version**        | **What it is used for**                                       |
|-----------------|----------------------------|---------------------------------------------------------------|
| Java JDK        | 21 or higher               | Build and run the backend (Spring Boot 4.1.1).                |
| Node.js         | 22 LTS (min. 20.19)        | Tooling for React 19/Vite 8 and Expo SDK 57.                  |
| npm             | included with Node         | Dependency management for web and mobile.                     |
| Docker Desktop  | latest stable              | Start MySQL 8.4 and Mailpit.                                  |
| Docker Compose  | included in Docker Desktop | Create the database schema and user.                          |
| OpenSSL         | any recent version         | Generate the RSA key pair and the recovery pepper.            |
| Git             | latest stable              | Clone the repositories and, on Windows, it bundles OpenSSL.   |
| Expo Go         | store app                  | Test the mobile app without an emulator on a physical device. |

All of these tools are distributed with official online documentation (Oracle, n.d.; Docker Inc., n.d.; Node.js Foundation, n.d.; Git, n.d.; OpenSSL, n.d.; Expo, n.d.).

# 8. Environment variables the installer must know

The backend never stores credentials in the code: everything is injected through environment variables or files ignored by Git (\`.env\`, \`application-local.yaml\`). These are the exact names that must be defined (Spring, n.d.):

| **Variable / file**                                           | **What it is for**                                                            | **Where to record it**                            |
|---------------------------------------------------------------|-------------------------------------------------------------------------------|---------------------------------------------------|
| MYSQL_PASSWORD / MYSQL_ROOT_PASSWORD                          | Passwords of the database created by Docker Compose.                          | \`.env\` (root of energy-monitor-backend)         |
| SPRING_DATASOURCE_PASSWORD                                    | Same password as MYSQL_PASSWORD; used by the tests of the \`test\` profile.   | \`.env\`                                          |
| MAIL_HOST, MAIL_PORT, MAIL_USERNAME, MAIL_PASSWORD, MAIL_FROM | SMTP to send the recovery code by email.                                      | \`.env\`                                          |
| SECURITY_RESET_PEPPER                                         | Long, random pepper (HMAC) for the recovery token.                            | \`.env\` or \`SECURITY_RESET_PEPPER\`             |
| SECURITY_CORS_ALLOWED_ORIGINS                                 | Origins allowed for the browser (e.g. http://localhost:5173).                 | \`.env\` or \`application-local.yaml\`            |
| SECURITY_JWT_PRIVATE_KEY_PATH / SECURITY_JWT_PUBLIC_KEY_PATH  | Absolute paths to the PEM keys of the JWT token.                              | Environment variables or \`application-local.yaml\` |
| VITE_API_BASE_URL                                             | Backend URL for the web app (default http://localhost:8080).                  | \`.env.local\` in Energy-Monitor-application      |

# 9. Backend installation

Repository: \`energy-monitor-backend\` (Java 21, Spring Boot 4.1.1, Spring Modulith, MySQL 8.4, Liquibase, JWT RS256). The application listens on \`http://localhost:8080\` (Spring, n.d.).

## 9.1 Obtain the code

1.  Clone or copy the repository folder onto the machine: git clone \<url\> or unzip the deliverable.

2.  Verify that it contains \`mvnw\`, \`pom.xml\`, \`docker-compose.yml\` and the \`src/\` folder.

## 9.2 Define database credentials (.env)

Copy the example file and replace each \`CHANGE_ME\` with your own values. The \`.env\` file is in \`.gitignore\` and must not be uploaded to the repository:

> cp .env.example .env
>
> \# On Windows PowerShell:
>
> Copy-Item .env.example .env

Minimum values to complete:

| **Variable**               | **Example (change it)**        |
|----------------------------|--------------------------------|
| MYSQL_PASSWORD             | A strong value of your choice  |
| MYSQL_ROOT_PASSWORD        | Another strong value           |
| SPRING_DATASOURCE_PASSWORD | The same as MYSQL_PASSWORD     |

## 9.3 Start MySQL with Docker Compose

> docker compose up -d
>
> docker compose ps

Wait for the \`energy-monitor-mysql\` container to report "healthy". Docker creates the \`energy_monitor\` schema and the \`energy_app\` user with the values from \`.env\` (Docker Inc., n.d.).

## 9.4 Create the local application profile

> cp src/main/resources/application-local.yaml.example src/main/resources/application-local.yaml
>
> \# Windows PowerShell:
>
> Copy-Item src\main\resources\application-local.yaml.example src\main\resources\application-local.yaml

Edit the new file and set: the same \`MYSQL_PASSWORD\` password in \`spring.datasource.password\`, the absolute paths of the two PEM keys (section 9.5) and the pepper (section 9.6).

## 9.5 Generate the RSA key pair (JWT tokens)

Access tokens are signed with RS256 and the application needs a key pair. On Linux and macOS:

> mkdir -p ~/.energy-monitor/keys
>
> chmod 700 ~/.energy-monitor/keys
>
> openssl genpkey -algorithm RSA -out ~/.energy-monitor/keys/private.pem -pkeyopt rsa_keygen_bits:2048
>
> openssl rsa -in ~/.energy-monitor/keys/private.pem -pubout -out ~/.energy-monitor/keys/public.pem

On Windows (PowerShell or Git Bash):

> New-Item -ItemType Directory -Force "\$HOME\\energy-monitor\keys"
>
> openssl genpkey -algorithm RSA -out "\$HOME\\energy-monitor\keys\private.pem" -pkeyopt rsa_keygen_bits:2048
>
> openssl rsa -in "\$HOME\\energy-monitor\keys\private.pem" -pubout -out "\$HOME\\energy-monitor\keys\public.pem"

Specify the paths in \`application-local.yaml\` (absolute; Java does not expand \`~\`):

> security:
>
> jwt:
>
> private-key-path: C:\Users\your-user\\energy-monitor\keys\private.pem
>
> public-key-path: C:\Users\your-user\\energy-monitor\keys\public.pem
>
> *The \`.pem\` keys are secrets: they are never committed, nor copied into a chat.*

## 9.6 Generate the recovery pepper

> openssl rand -hex 32

Copy the result into \`application-local.yaml\` under \`security.reset.pepper\`. Without the pepper the application refuses to start, just as it does without the RSA keys. Changing it invalidates the recovery codes already issued.

## 9.7 Decide on the recovery email

Choose one of two options: use Mailpit locally (recommended for development) or use a real Gmail account with a 16-character "app password" generated in the Google account (Google, n.d.). For Mailpit use \`MAIL_HOST=localhost\`, \`MAIL_PORT=587\`, \`MAIL_USERNAME=dev\`, \`MAIL_PASSWORD=dev\`; messages are read at \`http://localhost:8025\` (section 11).

## 9.8 Build and start

> ./mvnw clean test
>
> \# Windows:
>
> .\mvnw.cmd clean test
>
> ./mvnw spring-boot:run
>
> \# Windows:
>
> .\mvnw.cmd spring-boot:run

On Windows the \`./mvnw.cmd\` interpreter is used (Node does not install Maven separately; the repository includes the wrapper). The API is available at \`http://localhost:8080\`; the OpenAPI documentation at \`/v3/api-docs\` and Swagger UI at \`/swagger-ui.html\` (Spring, n.d.).

> *The tests are not hermetic: the persistence tests run against the real database (Liquibase creates the schema). That is why section 9.3 must be healthy before \`mvnw test\`.*

# 10. Web frontend installation

Repository: \`Energy-Monitor-application\` (React 19 + Vite 8). Dev server at \`http://localhost:5173\` (Vite, n.d.).

## 10.1 Dependencies

With Node 22 and npm in the PATH, from the repository folder:

> npm install

## 10.2 Configure the backend URL

Copy the example and adjust it if the backend does not use the default port:

> cp .env.example .env.local
>
> \# Windows PowerShell:
>
> Copy-Item .env.example .env.local
>
> VITE_API_BASE_URL=http://localhost:8080

## 10.3 Start in development

> npm run dev

Open \`http://localhost:5173\`. To build for production: \`npm run build\`; to check types/lint: \`npm run lint\`; for the tests: \`npm test\` (Vite, n.d.).

## 10.4 CORS permission in the backend

The browser will only allow calls from the frontend if the backend lists it as an allowed origin. It must be defined in the backend's same installation configuration (\`SECURITY_CORS_ALLOWED_ORIGINS\` in \`.env\` or \`application-local.yaml\`):

> SECURITY_CORS_ALLOWED_ORIGINS=http://localhost:5173,http://127.0.0.1:5173
>
> *Without that variable the browser blocks the requests even if the backend responds correctly. Optional: \`SECURITY_CORS_EXPOSED_HEADERS=Retry-After\` so that the frontend reads the exact wait time of the attempt limit; otherwise, it assumes 15 minutes.*

# 11. Mobile application installation

Repository: \`Energy-Monitor-Mobile\` (Expo SDK 57, React Native 0.86.3, TypeScript). It currently operates with demonstration data and does not require the backend to be running (Expo, n.d.).

## 11.1 Dependencies

> npm install

## 11.2 Start Expo

> npm start
>
> \# or directly:
>
> npx expo start

The Expo CLI shows a QR code and interactive menus. Choose the platform:

- Physical Android or emulator: \`npm run android\` (or press \`a\`).

- iOS (only on macOS with Xcode): \`npm run ios\` (or press \`i\`).

- Browser (useful on Windows): \`npm run web\` (or press \`w\`).

## 11.3 Test on a physical device

Install Expo Go from the phone's app store, make sure the computer and the mobile device are on the same network, and scan the QR with the camera (Android) or with Expo Go (iOS). It is the recommended route on Windows because it requires neither Android Studio nor Xcode (Expo, n.d.).

## 11.4 Compatibility notes

- Node 22 is the reference environment for the Expo SDK 57 tooling.

- The React Native web app uses \`react-native-web\`, so \`npm run web\` serves the view in the browser without an emulator.

- If the QR does not open, check the firewall or start with \`npx expo start --tunnel\`.

# 12. Local email with Mailpit (optional)

Mailpit is a development SMTP server: it accepts any message and shows them in a web inbox without sending anything over the real internet. It is started with Docker (Mailpit, n.d.):

> docker run -d --name mailpit -p 587:1025 -p 8025:8025 \\
>
> -e MP_SMTP_AUTH_ACCEPT_ANY=1 -e MP_SMTP_AUTH_ALLOW_INSECURE=1 axllent/mailpit

Use port 587 on the machine (instead of Mailpit's default 1025) so that when switching between Mailpit and Gmail only \`MAIL_HOST\` and the credentials change. In \`.env\`:

> MAIL_HOST=localhost
>
> MAIL_PORT=587
>
> MAIL_USERNAME=dev
>
> MAIL_PASSWORD=dev
>
> MAIL_FROM=Energy Monitor \<no-reply@localhost\>

Development inbox: \`http://localhost:8025\`. When you use Gmail, the recovery code arrives in the real mailbox associated with the account with the "app password".

# 13. Installation verification

Run this checklist before considering the installation complete. Each command must produce the indicated result:

| **Component**  | **Check**                                       | **Expected result**                                                   |
|----------------|-------------------------------------------------|-----------------------------------------------------------------------|
| Docker         | \`docker compose ps\` in energy-monitor-backend | \`energy-monitor-mysql\` in \`healthy\` state.                        |
| Backend        | Open \`http://localhost:8080/swagger-ui.html\`  | Swagger UI lists the \`/api/v1\` documentation.                       |
| OpenAPI        | \`GET http://localhost:8080/v3/api-docs\`       | JSON with the OpenAPI specification.                                  |
| Web            | \`npm run dev\` in Energy-Monitor-application   | Vite at \`http://localhost:5173\` shows the login screen.             |
| Mobile         | \`npm start\` in Energy-Monitor-Mobile          | Expo shows the QR and, when opened with Expo Go, the demo app.        |
| Email          | \`MAIL_HOST=localhost\` + Mailpit on :587       | Recovery codes appear at \`http://localhost:8025\`.                   |

For an end-to-end check: register a user from the web, verify their email (Mailpit or Gmail), log in and confirm that the household list loads data from the backend (Corte Parra & García Artunduaga, 2026).

# 14. Installation command summary

| **Step**      | **Backend (energy-monitor-backend)** | **Web (Energy-Monitor-application)** | **Mobile (Energy-Monitor-Mobile)** |
|---------------|--------------------------------------|--------------------------------------|------------------------------------|
| Variables     | Copy-Item .env.example .env          | Copy-Item .env.example .env.local    | Not applicable (mock data)         |
| Deps.         | (Maven wrapper included)             | npm install                          | npm install                        |
| Database      | docker compose up -d                 | —                                    | —                                  |
| Keys/pepper   | openssl … (section 9.5, 9.6)         | —                                    | —                                  |
| Tests         | .\mvnw.cmd test                      | npm test                             | npm run lint                       |
| Run           | .\mvnw.cmd spring-boot:run           | npm run dev                          | npm start                          |
| Typical URL   | http://localhost:8080                | http://localhost:5173                | QR / localhost:8081 (Expo)         |

# 15. Common troubleshooting

| **Symptom**                                                 | **Probable cause**                                                                                      | **Corrective action**                                                                  |
|-------------------------------------------------------------|---------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------|
| The backend does not start and names a key path or the pepper | \`SECURITY_JWT_PRIVATE_KEY_PATH\`, \`SECURITY_JWT_PUBLIC_KEY_PATH\` or \`SECURITY_RESET_PEPPER\` are missing. | Define them in \`.env\` or in \`application-local.yaml\` (absolute paths) and retry. |
| \`Access denied for user 'energy_app'\` when running tests  | \`SPRING_DATASOURCE_PASSWORD\` is not in \`.env\`.                                                      | Copy it from the value of \`MYSQL_PASSWORD\` in \`.env\` (section 9.2).                |
| The browser blocks calls from the web                       | \`SECURITY_CORS_ALLOWED_ORIGINS\` with the Vite origin is missing.                                      | Add it in the backend's \`.env\` (section 10.4).                                       |
| Recovery codes do not arrive                                | Email misconfigured; the error is recorded in the \`notification\` table.                               | Check \`MAIL\_\*\`, use Mailpit and read the inbox at \`:8025\` (section 12).          |
| Mailpit/Docker does not start                               | Docker Desktop stopped or port in use.                                                                  | Start Docker Desktop; change the port with \`-p 1587:1025\` if 587 is in use.          |
| Illinois: "~" is not expanded in the key path               | Java does not expand the symbol in \`private-key-path\`.                                                | Use the full absolute path (\`C:\Users\\\`).                                           |
| Expo does not connect with Expo Go                          | Different network, firewall, or invalid QR.                                                             | Use \`npx expo start --tunnel\`; check corporate network scripts.                      |
| \`npm install\` fails on mobile/web                         | Node version different from the reference software.                                                     | Update to Node 22 LTS (min. 20.19) and reinstall.                                      |

# 16. References (APA format, 7th edition)

In-text citations and the reference list follow the style of the seventh edition of the APA publication manual (American Psychological Association \[APA\], 2020).

American Psychological Association. (2020). Publication manual of the American Psychological Association (7th ed.). https://www.apa.org/

Corte Parra, J. E., & García Artunduaga, M. A. (2026). Software requirements specification (SRS) of the "Electrical Consumption Monitor" project \[Internal document\]. SENA – CIES Huila Regional.

Docker Inc. (n.d.). Docker documentation. https://docs.docker.com/

Expo. (n.d.). Expo documentation. https://docs.expo.dev/

Git. (n.d.). Git for Windows. https://git-scm.com/download/win

Google. (n.d.). How to create an app password. https://myaccount.google.com/apppasswords

Mailpit. (n.d.). Mailpit: Web and API based SMTP testing. https://mailpit.axllent.org/

MySQL. (2026). MySQL 8.4 reference manual. https://dev.mysql.com/doc/refman/8.4/en/

Node.js Foundation. (n.d.). Node.js. https://nodejs.org/

OpenSSL. (n.d.). OpenSSL cryptography and SSL/TLS toolkit. https://www.openssl.org/

Oracle. (n.d.). Java SE downloads. https://www.oracle.com/java/technologies/downloads/

Spring. (n.d.). Spring Boot reference documentation. https://docs.spring.io/spring-boot/index.html

Vite. (n.d.). Vite guide. https://vite.dev/guide/
