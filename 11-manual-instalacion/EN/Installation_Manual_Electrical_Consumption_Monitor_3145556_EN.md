**SERVICIO NACIONAL DE APRENDIZAJE – SENA (National Learning Service)**

**CENTER FOR INDUSTRY, BUSINESS AND SERVICES (CIES)**

**HUILA REGIONAL**

Training Program: Technology in Software Analysis and Development (ADSO)

Group (Ficha): 3145556

**INSTALLATION MANUAL**

**Project: Electrical Consumption Monitor**

*Procedure for deploying the API, the MQTT broker, the measuring device, the web application and the mobile application*

| **Item**             | **Information**                                                                 |
|----------------------|---------------------------------------------------------------------------------|
| Team members         | Juan Esteban Cortés Parra, Miguel Ángel García Artunduaga                       |
| Instructor           | `[TO BE CONFIRMED]`                                                             |
| Training center      | Center for Industry, Business and Services (CIES) – SENA Huila Regional         |
| Program              | Technology in Software Analysis and Development (ADSO)                          |
| Group (Ficha)        | 3145556                                                                         |
| Document type        | Installation manual                                                             |
| Version              | 1.1                                                                             |
| Date prepared        | October 7, 2026 (update: October 13, 2026)                                      |

Year 2026

# 2. Document version control

The change history of this manual is recorded in the following table. Every modification to the installation process that affects the software must be reflected in a new version of the document.

| **Version** | **Date**   | **Description of change**                                                                                                                                                                                                                       | **Responsible**      |
|-------------|------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------|
| 1.0         | 10/07/2026 | Initial creation of the installation manual for the backend, web frontend and mobile frontend, with requirements from the requirements specification and references in APA format.                                                              | Development team     |
| 1.1         | 10/13/2026 | Update to the final state: five system parts, complete environment variables, MQTT broker, firmware, linking the first device, production, mobile application integrated with the API, and expanded verifications and troubleshooting.         | Development team     |

# 3. Table of contents

[2. Document version control](#2-document-version-control)

[3. Table of contents](#3-table-of-contents)

[4. Introduction](#4-introduction)

[5. Objective](#5-objective)

[6. Technical scope](#6-technical-scope)

[7. Minimum requirements](#7-minimum-requirements)

- [7.1 Equipment requirements (according to the requirements specification)](#71-equipment-requirements-according-to-the-requirements-specification)
- [7.2 Recommended hardware resources to run the system](#72-recommended-hardware-resources-to-run-the-system)
- [7.3 Software requirements for the installation](#73-software-requirements-for-the-installation)

[8. Environment variables the installer must know](#8-environment-variables-the-installer-must-know)

[9. Backend installation](#9-backend-installation)

- [9.1 Obtain the code](#91-obtain-the-code) · [9.2 Define database credentials (.env)](#92-define-database-credentials-env) · [9.3 Start MySQL with Docker Compose](#93-start-mysql-with-docker-compose) · [9.4 Create the local application profile](#94-create-the-local-application-profile) · [9.5 Generate the RSA key pair (JWT tokens)](#95-generate-the-rsa-key-pair-jwt-tokens) · [9.6 Generate the recovery pepper](#96-generate-the-recovery-pepper) · [9.7 Decide on the recovery email](#97-decide-on-the-recovery-email) · [9.8 Build and start](#98-build-and-start)

[10. Web frontend installation](#10-web-frontend-installation)

- [10.1 Dependencies](#101-dependencies) · [10.2 Configure the backend URL](#102-configure-the-backend-url) · [10.3 Start in development](#103-start-in-development) · [10.4 CORS permission in the backend](#104-cors-permission-in-the-backend)

[11. Mobile application installation](#11-mobile-application-installation)

- [11.1 Dependencies](#111-dependencies) · [11.2 Configure the backend URL](#112-configure-the-backend-url) · [11.3 Start Expo](#113-start-expo) · [11.4 Test on a physical device](#114-test-on-a-physical-device) · [11.5 Compatibility notes](#115-compatibility-notes)

[12. MQTT broker](#12-mqtt-broker)

[13. Measuring device firmware](#13-measuring-device-firmware)

[14. Link the first device](#14-link-the-first-device)

[15. Production](#15-production)

[16. Local email with Mailpit (optional)](#16-local-email-with-mailpit-optional)

[17. Installation verification](#17-installation-verification)

[18. Installation command summary](#18-installation-command-summary)

[19. Common troubleshooting](#19-common-troubleshooting)

[20. References (APA format, 7th edition)](#20-references-apa-format-7th-edition)

# 4. Introduction

This document is the installation manual of the "Electrical Consumption Monitor" software. Its purpose is for any developer, instructor or technician —regardless of prior experience with the project— to be able to obtain the source code, configure the dependencies, start the five components of the system and verify that they work together on a workstation.

The system is made up of five parts that are installed separately: a REST API (backend) built with Java 21 and Spring Boot 4.1.1, a web application built with React 19 and Vite 8, an Android mobile application built with Expo SDK 57 and React Native 0.86, an Eclipse Mosquitto MQTT broker that authenticates against the backend, and a measuring device made of an ESP32 board and a PZEM-004T V3 sensor (Spring, n.d.; Vite, n.d.; Expo, n.d.; Eclipse Foundation, n.d.). The MySQL 8.4 database is started with Docker Compose and email delivery can be simulated locally with Mailpit (Docker Inc., n.d.; Mailpit, n.d.).

The information recorded was obtained directly from the project's repositories (backend, web, mobile and devices), from the requirements specification and from the READMEs included in each repository, so it corresponds to the final version of the delivered software. References were formatted according to APA style, seventh edition (American Psychological Association \[APA\], 2020).

# 5. Objective

To allow a new or maintenance development team to start the "Electrical Consumption Monitor" system on a Windows, macOS or Linux computer, from the official repositories, without depending on informal documentation or past conversations.

- List the minimum hardware, operating system and software requirements, consistent with the requirements specification.

- Establish exact versions of the tools the installation process requires (Java, Node, Docker, OpenSSL, Arduino).

- Describe, step by step and with the concrete commands, the installation of the database, the backend, the web application, the mobile application, the MQTT broker and the device firmware.

- Present the configuration variables the installer must define before the first start.

- Explain how to link the first device, how to deploy to production, how to verify that each component is operational and how to diagnose the most common errors.

# 6. Technical scope

The manual covers installation on a local development workstation of the following components and, in section 15, deployment on a production server with Docker Compose:

- MySQL 8.4 database through Docker Compose (energy-monitor-backend).

- Backend REST API (energy-monitor-backend) with the local profile and JUnit tests.

- Web application (Energy-Monitor-application) with Vite.

- Android mobile application (Energy-Monitor-Mobile) with Expo and Expo Go.

- Mosquitto MQTT broker and device firmware (devices, repository energy-monitor-devices).

- Optional utilities: Mailpit for local email and RSA keys for the JWTs.

It does not cover hiring a server or a domain, nor the detailed configuration of a reverse proxy; proxy setup is limited to the rules given in section 15.

# 7. Minimum requirements

## 7.1 Equipment requirements (according to the requirements specification)

According to the compatibility requirements of the requirements specification (RNF-COM), the software runs on computers and mobile devices in perfect condition, with an internet connection, enough storage to hold the application and the latest available version of the operating system (Cortés Parra & García Artunduaga, 2026):

- Computer operating system: Windows 10 or higher.

- Mobile operating system: the latest available version of Android. The mobile application is not available for iPhone.

- Up-to-date web browser: Chrome, Firefox, Safari or Edge. Linking the device over Bluetooth is only available in desktop Chrome or Edge and in Chrome for Android, and requires a secure connection (HTTPS or `localhost`).

- Internet connection (services communicate through standard HTTP/HTTPS protocols).

- For the measuring device: an ESP32 DevKit board with Wi-Fi and Bluetooth, a PZEM-004T V3 sensor, an HLK-PM03 power supply and a USB cable.

## 7.2 Recommended hardware resources to run the system

Beyond the equipment requirements stated in the requirements specification, the machine where the components are installed must have the following minimum resources for a smooth experience without freezes or memory errors. The exact software versions are detailed in the table of section 7.3.

| **Resource**             | **Minimum**                                                    | **Recommended**                                                                       |
|--------------------------|----------------------------------------------------------------|---------------------------------------------------------------------------------------|
| Operating system (PC)    | 64-bit Windows 10 (with Docker Desktop and WSL 2)              | 64-bit Windows 11 or Linux (Ubuntu 24.04+)                                            |
| Operating system (mobile)| Android 10 or higher                                           | Latest available Android version                                                      |
| Processor                | Intel Core i3 or equivalent                                    | Intel Core i5 or equivalent                                                           |
| RAM                      | 8 GB                                                           | 16 GB (Docker Desktop, MySQL, Node and design tools open comfortably)                 |
| Free disk space          | 8 GB (code, node_modules, MySQL image and containers)          | 20 GB or more (for several versions, Docker caches and database files)                |
| Connectivity             | Stable internet (to clone, download packages and Expo Go)      | Broadband connection                                                                  |
| Additional storage       | At least 5 GB free for test data and MySQL backups             | Free space at your disposal                                                           |

> *If the machine meets the minimum but not the recommendation, install only the components you need at the same time (e.g. run backend and web in separate terminals and do not open Android Studio at the same time).*

## 7.3 Software requirements for the installation

The minimum versions verified on the backend repository, the two frontends and the devices repository are:

| **Tool**                        | **Minimum version**                  | **What it is used for**                                                       |
|---------------------------------|--------------------------------------|-------------------------------------------------------------------------------|
| Java JDK                        | 21 or higher                         | Build and run the backend (Spring Boot 4.1.1).                                |
| Node.js                         | 22 LTS (min. 20.19)                  | Tooling for React 19/Vite 8 and Expo SDK 57.                                  |
| npm                             | included with Node                   | Dependency management for web and mobile.                                     |
| Docker Desktop                  | latest stable                        | Start MySQL 8.4, the Mosquitto broker and Mailpit.                            |
| Docker Compose                  | v2, included in Docker Desktop       | Create the database schema and user and start the broker.                     |
| OpenSSL and bash                | any recent version                   | Generate the RSA key pair, the pepper and the development certificates.       |
| Git                             | latest stable                        | Clone the repositories; on Windows it bundles OpenSSL.                        |
| Expo Go                         | store app                            | Test the mobile app without an emulator on a physical device.                 |
| arduino-cli or Arduino IDE 2    | latest stable                        | Build and upload the device firmware (Espressif esp32 core 3.x).              |

All these tools are distributed with official online documentation (Oracle, n.d.; Docker Inc., n.d.; Node.js Foundation, n.d.; Git, n.d.; OpenSSL, n.d.; Expo, n.d.; Arduino, n.d.).

# 8. Environment variables the installer must know

The backend never stores credentials in the code: everything is injected through environment variables or Git-ignored files (`.env`, `application-local.yaml`). The names below are the exact ones in the `.env.example` of the backend, the web and the mobile application, and in `application.yaml` where indicated (Spring, n.d.).

> *The `.env` file is read as `.properties`: write values without quotes and without `$`. A value containing `$` is treated as a placeholder and the application fails to start.*

| **Variable / file**                                            | **What it is for**                                                                                                                | **Where to set it**                               |
|----------------------------------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------|---------------------------------------------------|
| MYSQL_PASSWORD / MYSQL_ROOT_PASSWORD                           | Passwords of the database that Docker Compose creates.                                                                            | `.env` (root of energy-monitor-backend)           |
| SPRING_DATASOURCE_PASSWORD                                     | Same password as MYSQL_PASSWORD; used by the tests of the `test` profile.                                                         | `.env`                                            |
| MAIL_HOST, MAIL_PORT, MAIL_USERNAME, MAIL_PASSWORD, MAIL_FROM  | SMTP to send verification and recovery codes and alert emails.                                                                    | `.env`                                            |
| SECURITY_RESET_PEPPER                                          | Long, random pepper (HMAC) for the recovery code.                                                                                 | `.env`                                            |
| SECURITY_CORS_ALLOWED_ORIGINS                                  | Origins allowed for the browser (e.g. http://localhost:5173).                                                                     | `.env` or `application-local.yaml`                |
| SECURITY_CORS_EXPOSED_HEADERS                                  | Headers the browser may read. `Allow` and `Retry-After` by default.                                                               | Environment variable (optional)                   |
| SECURITY_JWT_PRIVATE_KEY_PATH / SECURITY_JWT_PUBLIC_KEY_PATH   | Absolute paths to the PEM keys of the JWT token.                                                                                  | Environment variables or `application-local.yaml` |
| GOOGLE_CLIENT_ID / GOOGLE_CLIENT_SECRET                        | OAuth credentials ("Web application") to sign in with Google. If empty, the Google button answers `503`.                          | `.env`                                            |
| MQTT_BROKER_URL                                                | Address of the broker's internal listener (`tcp://localhost:1883`).                                                               | `.env`                                            |
| MQTT_BACKEND_USER / MQTT_BACKEND_PASS                          | Backend credentials in the broker. Any random value works (`openssl rand -hex 24`).                                               | `.env`                                            |
| MQTT_CLIENT_ID                                                 | Identifier of the backend in the broker; it must differ per instance.                                                             | `.env`                                            |
| MQTT_AUTH_ALLOWED_NETWORKS                                     | Networks allowed to call `/internal/mqtt/auth/**` (default `127.0.0.1/32,::1/128,172.16.0.0/12`).                                 | Environment variable (optional)                   |
| DEVICE_MQTT_PUBLIC_HOST / DEVICE_MQTT_PUBLIC_PORT              | Broker address the web writes to the device when linking it. Never `localhost`: in development, the LAN IP of the PC; in production, the broker's domain. Port `8883`. | `.env`                  |
| DEVICE_OFFLINE_AFTER                                           | Silence time after which a device is marked offline (default `PT150S`).                                                           | Environment variable (optional)                   |
| NOTIFICATION_WEB_URL                                           | Address of the web used by the links in alert emails (`http://localhost:5173`).                                                   | `.env`                                            |
| NOTIFICATION_VAPID_PUBLIC_KEY / NOTIFICATION_VAPID_PRIVATE_KEY | VAPID key pair for browser push notifications. Without it push is disabled and email keeps working.                               | `.env`                                            |
| NOTIFICATION_VAPID_SUBJECT                                     | Contact associated with the VAPID keys (a support `mailto:` by default).                                                          | Environment variable (optional)                   |
| ALERT_LIMIT_ZONE                                               | Time zone in which the day and month of limit alerts start (default `America/Bogota`).                                            | Environment variable (optional)                   |
| RECOMMENDATION_EVALUATE_INTERVAL / RECOMMENDATION_ZONE         | How often the saving rules run (`PT1H`) and the time zone of the day-part rules (`America/Bogota`).                               | Environment variables (optional)                  |
| VITE_API_BASE_URL                                              | Backend URL for the web app (default http://localhost:8080).                                                                      | `.env.local` in Energy-Monitor-application        |
| VITE_GOOGLE_CLIENT_ID                                          | The same client ID as GOOGLE_CLIENT_ID. Empty: the Google button is not shown.                                                    | `.env.local` in Energy-Monitor-application        |
| EXPO_PUBLIC_API_BASE_URL                                       | Backend URL for the mobile app, without `/api/v1` (section 11.2).                                                                 | `.env` in Energy-Monitor-Mobile                   |

# 9. Backend installation

Repository: `energy-monitor-backend` (Java 21, Spring Boot 4.1.1, Spring Modulith, MySQL 8.4, Liquibase, JWT RS256). The application listens on `http://localhost:8080` (Spring, n.d.).

## 9.1 Obtain the code

1.  Clone or copy the repository folder to the machine: git clone \<url\> or unzip the deliverable.

2.  Verify that it contains `mvnw`, `pom.xml`, `docker-compose.yml` and the `src/` folder.

## 9.2 Define database credentials (.env)

Copy the example file and replace every `CHANGE_ME` with your own values. The `.env` file is in `.gitignore` and must not be uploaded to the repository:

```bash
cp .env.example .env
# On Windows PowerShell:
Copy-Item .env.example .env
```

Minimum values to fill in:

| **Variable**               | **Example (change it)**        |
|----------------------------|--------------------------------|
| MYSQL_PASSWORD             | A strong value of your choice  |
| MYSQL_ROOT_PASSWORD        | Another strong value           |
| SPRING_DATASOURCE_PASSWORD | The same as MYSQL_PASSWORD     |

The remaining variables (broker, devices, notifications, Google) are described in section 8 and are completed when each part is installed.

## 9.3 Start MySQL with Docker Compose

```bash
docker compose up -d
docker compose ps
```

Wait for the `energy-monitor-mysql` container to report "healthy". Docker creates the `energy_monitor` schema and the `energy_app` user with the values from `.env` (Docker Inc., n.d.).

## 9.4 Create the local application profile

```bash
cp src/main/resources/application-local.yaml.example src/main/resources/application-local.yaml
# Windows PowerShell:
Copy-Item src\main\resources\application-local.yaml.example src\main\resources\application-local.yaml
```

Edit the new file and set: the same `MYSQL_PASSWORD` password in `spring.datasource.password`, the absolute paths of the two PEM keys (section 9.5) and the pepper (section 9.6).

## 9.5 Generate the RSA key pair (JWT tokens)

Access tokens are signed with RS256 and the application needs a key pair. On Linux and macOS:

```bash
mkdir -p ~/.energy-monitor/keys
chmod 700 ~/.energy-monitor/keys
openssl genpkey -algorithm RSA -out ~/.energy-monitor/keys/private.pem -pkeyopt rsa_keygen_bits:2048
openssl rsa -in ~/.energy-monitor/keys/private.pem -pubout -out ~/.energy-monitor/keys/public.pem
```

On Windows (PowerShell or Git Bash), with the same hidden `.energy-monitor` folder:

```powershell
New-Item -ItemType Directory -Force "$HOME\.energy-monitor\keys"
openssl genpkey -algorithm RSA -out "$HOME\.energy-monitor\keys\private.pem" -pkeyopt rsa_keygen_bits:2048
openssl rsa -in "$HOME\.energy-monitor\keys\private.pem" -pubout -out "$HOME\.energy-monitor\keys\public.pem"
```

Set the paths in `application-local.yaml` (absolute; Java does not expand `~`):

```yaml
security:
  jwt:
    private-key-path: C:\Users\your-user\.energy-monitor\keys\private.pem
    public-key-path: C:\Users\your-user\.energy-monitor\keys\public.pem
```

> *The `.pem` keys are secrets: never commit them or paste them in a chat.*

## 9.6 Generate the recovery pepper

```bash
openssl rand -hex 32
```

Copy the result into `application-local.yaml` under `security.reset.pepper`. Without a pepper the application refuses to start, just as without the RSA keys. Changing it invalidates recovery codes already issued.

## 9.7 Decide on the recovery email

Choose one of two options: use Mailpit locally (recommended for development) or use a real Gmail account with a 16-character "app password" generated in the Google account (Google, n.d.). For Mailpit use `MAIL_HOST=localhost`, `MAIL_PORT=587`, `MAIL_USERNAME=dev`, `MAIL_PASSWORD=dev`; messages are read at `http://localhost:8025` (section 16).

## 9.8 Build and start

```bash
./mvnw clean test
./mvnw spring-boot:run
# Windows:
.\mvnw.cmd clean test
.\mvnw.cmd spring-boot:run
```

On Windows `mvnw.cmd` is used. The repository includes the Maven Wrapper, so there is no need to install Maven separately (Node.js is only needed for the frontends and does not install Maven). The API runs at `http://localhost:8080`; the OpenAPI documentation at `/v3/api-docs` and Swagger UI at `/swagger-ui.html` (Spring, n.d.). The health status is at `/actuator/health`.

> *The tests are not hermetic: the persistence tests run against the real database (Liquibase creates the schema). That is why section 9.3 must be healthy before `mvnw test`.*

# 10. Web frontend installation

Repository: `Energy-Monitor-application` (React 19 + Vite 8). Dev server at `http://localhost:5173` (Vite, n.d.).

## 10.1 Dependencies

With Node 22 and npm in the PATH, from the repository folder:

```bash
npm install
```

## 10.2 Configure the backend URL

Copy the example and adjust it if the backend does not use the default port:

```bash
cp .env.example .env.local
# Windows PowerShell:
Copy-Item .env.example .env.local
```

```
VITE_API_BASE_URL=http://localhost:8080
VITE_GOOGLE_CLIENT_ID=
```

`VITE_GOOGLE_CLIENT_ID` must be the same client ID as the backend's `GOOGLE_CLIENT_ID`. If left empty, the Google button is not shown.

## 10.3 Start in development

```bash
npm run dev
```

Open `http://localhost:5173`. To build for production: `npm run build`; to check the code: `npm run lint`; for the tests: `npm test` (Vite, n.d.).

## 10.4 CORS permission in the backend

The browser only allows calls from the frontend if the backend lists it as an allowed origin. It must be defined in the same backend installation configuration (`SECURITY_CORS_ALLOWED_ORIGINS` in `.env` or `application-local.yaml`):

```
SECURITY_CORS_ALLOWED_ORIGINS=http://localhost:5173,http://127.0.0.1:5173
```

> *Without that variable the browser blocks requests even if the backend responds correctly. The backend already exposes the `Allow` and `Retry-After` headers by default, so the frontend reads the exact wait of the attempt limit; `SECURITY_CORS_EXPOSED_HEADERS` is only needed to change that list.*

# 11. Mobile application installation

Repository: `Energy-Monitor-Mobile` (Expo SDK 57, React Native 0.86.3, TypeScript, Android only). The application is integrated with the backend API, so the backend must be running to sign in and query data (Expo, n.d.).

## 11.1 Dependencies

```bash
npm install
```

## 11.2 Configure the backend URL

Copy the example and set the backend address, without `/api/v1`:

```bash
cp .env.example .env
# Windows PowerShell:
Copy-Item .env.example .env
```

```
EXPO_PUBLIC_API_BASE_URL=http://10.0.2.2:8080
```

The value depends on where the application runs:

| **Where the app runs**     | **Value of EXPO_PUBLIC_API_BASE_URL**                                              |
|----------------------------|------------------------------------------------------------------------------------|
| Android emulator           | `http://10.0.2.2:8080` (`localhost` would be the emulator itself)                  |
| Physical phone             | `http://<LAN IP of the PC>:8080`, for example `http://192.168.1.50:8080`           |
| Browser (`npm run web`)    | `http://localhost:8080`                                                            |

Expo embeds the `EXPO_PUBLIC_*` variables at build time: after changing the value, restart `npx expo start`. CORS does not apply to React Native, so the app does not need to be added to `SECURITY_CORS_ALLOWED_ORIGINS`.

## 11.3 Start Expo

```bash
npm start
# or directly:
npx expo start
```

The Expo CLI shows a QR code and interactive menus. Choose the platform:

- Physical Android or emulator: `npm run android` (or press `a`).

- Browser, only as a development aid: `npm run web` (or press `w`).

## 11.4 Test on a physical device

Install Expo Go from Google Play, make sure the computer and the phone are on the same network, and scan the QR from Expo Go. It is the recommended route on Windows because it does not require Android Studio (Expo, n.d.). For the phone to reach the backend, allow port 8080 in the computer's firewall.

## 11.5 Compatibility notes

- Node 22 is the reference environment for the Expo SDK 57 tooling.

- The mobile application is not available for iPhone: this manual does not cover iOS or Xcode.

- If the QR does not open, check the firewall or start with `npx expo start --tunnel`.

# 12. MQTT broker

The broker is Eclipse Mosquitto with the `mosquitto-go-auth` plugin: each device authenticates against the backend and can only publish to its two topics. It is started from the `devices/docker/` folder of the devices repository, whose development `docker-compose.yml` starts only the broker; the backend runs on the host machine and the container reaches it through `host-gateway` (Eclipse Foundation, n.d.).

The repositories must sit side by side (`backend/energy-monitor-backend`, `frontend/energy-monitor-web` and `devices`). Steps:

1.  Set in the backend's `.env` `MQTT_BACKEND_USER`, `MQTT_BACKEND_PASS`, `DEVICE_MQTT_PUBLIC_HOST` (the LAN IP of the PC; on Linux, `hostname -I`) and `DEVICE_MQTT_PUBLIC_PORT=8883` (section 8).

2.  Start the backend (section 9.8) and check that `http://localhost:8080/actuator/health` answers `UP`. The broker asks the backend to authenticate every client, including the backend itself.

3.  Generate the development TLS certificate (once):

```bash
cd devices/docker
./mosquitto/generate-dev-certs.sh
```

The script creates a development certificate authority and the broker certificate in `mosquitto/certs/`, valid for the machine's IPs, and writes `esp32-pzem-monitor/dev_ca.h` so the firmware trusts that authority. If the machine's IP changes, run it again: the authority is reused and the devices do not need to be reprogrammed.

4.  On Linux with ufw, allow the container to call the backend (once), from the Docker networks only:

```bash
sudo ufw allow from 172.16.0.0/12 to any port 8080 proto tcp comment 'mosquitto -> backend'
```

5.  With the backend running, start the broker:

```bash
docker compose up -d
docker compose logs -f mosquitto
```

The backend shows up as `energy-monitor-backend-telemetry` and `energy-monitor-backend-status`. Devices use port 8883 with TLS; 1883 is internal, for the backend only, and Docker publishes it on the loopback interface only.

> *If the container exits with a permission error on `server.key`, the container reads the file as user 1000: run `sudo chown 1000 docker/mosquitto/certs/server.key`.*

# 13. Measuring device firmware

The firmware (version 1.3.8) is in `devices/esp32-pzem-monitor/` and is built with arduino-cli or Arduino IDE 2 (Arduino, n.d.). There is no over-the-air (OTA) update: the firmware is always uploaded over USB.

## 13.1 Wiring

| PZEM-004T V3 | ESP32                  | Note                               |
|--------------|------------------------|------------------------------------|
| TX           | GPIO16 (RX2)           | TX and RX are crossed              |
| RX           | GPIO17 (TX2)           |                                    |
| VCC          | 3V3                    | If it does not answer, use 5 V from VIN |
| GND          | GND                    | Common with the PZEM               |

Communication is Modbus RTU at 9600 baud, 8N1, with address `0xF8`. The HLK-PM03 power supply powers the board from the same circuit being measured.

> **Safety warning:** the PZEM-004T is not galvanically isolated. Do not touch the side connected to the mains and do not handle the mains wiring with the prototype powered.

## 13.2 Tools and libraries

```bash
arduino-cli core update-index
arduino-cli core install esp32:esp32
arduino-cli lib install PZEM004Tv30 PubSubClient ArduinoJson WiFiManager NimBLE-Arduino
```

| **Library**    | **Author**      | **Tested version** |
|----------------|-----------------|--------------------|
| PZEM004Tv30    | Jakub Mandula   | 1.2.1              |
| PubSubClient   | Nick O'Leary    | 2.8                |
| ArduinoJson    | Benoit Blanchon | 7.x                |
| WiFiManager    | tzapu           | 2.0.17             |
| NimBLE-Arduino | h2zero          | 2.5.1              |

Do not confuse `PZEM004Tv30` with the old `PZEM-004T` library (v1/v2).

## 13.3 Preparation

```bash
cd devices/esp32-pzem-monitor
cp secrets.h.example secrets.h
```

Set at least `PORTAL_PASSWORD` (8 characters or more) in `secrets.h`: it protects the `EnergyMonitor-Setup` access point. The `dev_ca.h` file is generated by `generate-dev-certs.sh` (section 12). Both files are ignored by Git.

## 13.4 Build and upload over USB

The partition scheme must be **Minimal SPIFFS (1.9 MB APP with OTA)**, because with Bluetooth the binary (~1.4 MB) does not fit in the default scheme.

```bash
arduino-cli compile --fqbn esp32:esp32:esp32:PartitionScheme=min_spiffs .
arduino-cli upload  --fqbn esp32:esp32:esp32:PartitionScheme=min_spiffs -p /dev/ttyUSB0 .
arduino-cli monitor -p /dev/ttyUSB0 -c baudrate=115200
```

In Arduino IDE: board **ESP32 Dev Module** and *Partition Scheme* **Minimal SPIFFS**. Close any other serial monitor before uploading, or the port will be busy. On Linux, the user needs access to the port (`sudo usermod -aG dialout $USER` and log in again).

## 13.5 Setup mode

A new device, with no configuration, enters setup mode on its own. To activate it on one that is already configured, hold the **BOOT** button (GPIO0) for 3 seconds. With no known network for 5 minutes, the device offers Bluetooth configuration by itself.

# 14. Link the first device

The device is registered from the web application; nothing needs to be inserted manually into the database. Requirements: backend, database and broker running, and the web open in desktop Chrome or Edge, or Chrome on Android, over HTTPS or `localhost`.

1.  In the web: household → **Devices** → **Link device** (owner only).

2.  Hold **BOOT** on the ESP32 for 3 seconds until the LED blinks (a device that was never linked is already in setup mode after uploading).

3.  **Find device**: the browser shows `EnergyMonitor-<code>`; select it.

4.  Choose the appliance, the name and the room.

5.  Choose the household Wi-Fi from the list (they are the networks the ESP32 sees) and type its password.

6.  The web registers the device in the backend, which generates its `api_key`, and sends the device over Bluetooth the network, the password, the server (`DEVICE_MQTT_PUBLIC_HOST`), the identifier and the key.

7.  Once connected to the broker, the device leaves setup mode on its own.

**Alternative without Bluetooth** (Safari, Firefox or other browsers): join the `EnergyMonitor-Setup` Wi-Fi network with the `PORTAL_PASSWORD` password, open `http://192.168.4.1` → **Configure WiFi** and fill in the network, the MQTT server (the PC's IP or the broker's domain), port 8883, the identifier, the code and the device key. This route requires the device to already exist in the backend with its key.

> *On Linux, Chrome ships with Web Bluetooth disabled: enable `chrome://flags/#enable-experimental-web-platform-features` and restart the browser. Web Bluetooth only works on `https://` or `http://localhost`.*

Expected output in the serial monitor:

```
=== EnergyMonitor ESP32 + PZEM-004T V3 fw=1.3.8 ===
[PZEM] UART2 ready at 9600 baud, addr=0xf8
[WiFi] connected to 'MyHome', IP=192.168.1.37
[MQTT] connected as esp32-DEV0000042
[MQTT] telemetry published
```

# 15. Production

This section summarizes deployment on a server with Docker Compose: broker, backend and MySQL (`devices/docker/docker-compose.prod.yml`). The `mosquitto.conf` is the same file as in development (Let's Encrypt, n.d.).

1.  **Server and domain.** A server with Docker and a domain for the broker (for example `mqtt.example.com`) pointing to it. It can be the same domain as the API.

2.  **Certificate.** Obtain a Let's Encrypt certificate for that domain (`certbot certonly --standalone -d mqtt.example.com`), copy `fullchain.pem` as `server.crt` and `privkey.pem` as `server.key` into the `MQTT_CERT_DIR` folder (readable by user 1000) and repeat the copy on every renewal with a `--deploy-hook` that restarts the `mosquitto` container. The firmware does not change: it trusts the Let's Encrypt roots.

3.  **Configuration.** `cp docker/.env.prod.example docker/.env.prod` and fill it in: MySQL passwords, pepper, JWT key pair folder, a random `MQTT_BACKEND_PASS`, `DEVICE_MQTT_PUBLIC_HOST` with the broker's domain, `NOTIFICATION_WEB_URL` and the VAPID keys. The JWT private key must be readable by `BACKEND_UID` (10001 by default).

4.  **Start.**

```bash
cd docker
docker compose -f docker-compose.prod.yml --env-file .env.prod up -d --build
```

5.  **Backend image.** The backend `Dockerfile` builds with Temurin 21 (JDK) and runs with Temurin 21 (JRE) as an unprivileged user (UID 10001), on port 8080.

6.  **Firewall.** Open ports 443 (HTTPS proxy) and 8883 (MQTT with TLS). Keep 1883, 3306 and 8080 closed.

7.  **HTTPS reverse proxy** (Caddy or nginx) in front of `127.0.0.1:8080` for the API and the web. The proxy **must block `/internal/**`**: behind the proxy every request looks local. The web must be served over HTTPS, because Web Bluetooth and push require it.

8.  **Real client address.** Behind a proxy, the password-recovery attempt limit is shared among all users. To avoid it, add `server.forward-headers-strategy=native` to the deployment profile (`prod`) configuration, not to `application.yaml`. The file of that profile is not versioned in the repository: its location is `[TO BE CONFIRMED]`.

VAPID keys are generated once per environment (changing them invalidates every push subscription); the commands are in `.env.example` and in section 7 of the devices README. Several backend instances need different `MQTT_CLIENT_ID` values.

# 16. Local email with Mailpit (optional)

Mailpit is a development SMTP server: it accepts any message and shows them in a web inbox without sending anything over the real internet. It is started with Docker (Mailpit, n.d.):

```bash
docker run -d --name mailpit -p 587:1025 -p 8025:8025 \
  -e MP_SMTP_AUTH_ACCEPT_ANY=1 -e MP_SMTP_AUTH_ALLOW_INSECURE=1 axllent/mailpit
```

Use port 587 on the machine (instead of Mailpit's default 1025) so that when switching between Mailpit and Gmail only `MAIL_HOST` and the credentials change. In `.env`:

```
MAIL_HOST=localhost
MAIL_PORT=587
MAIL_USERNAME=dev
MAIL_PASSWORD=dev
MAIL_FROM=Energy Monitor <no-reply@localhost>
```

Development inbox: `http://localhost:8025`. When you use Gmail, the recovery code arrives in the real mailbox associated with the account with the "app password".

# 17. Installation verification

Run this checklist before considering the installation finished. Each command must produce the indicated result:

| **Component**    | **Check**                                                                         | **Expected result**                                                                              |
|------------------|-----------------------------------------------------------------------------------|--------------------------------------------------------------------------------------------------|
| Docker           | `docker compose ps` in energy-monitor-backend                                     | `energy-monitor-mysql` in `healthy` state.                                                       |
| Backend          | Open `http://localhost:8080/actuator/health`                                      | Answers `UP`.                                                                                    |
| Backend          | Open `http://localhost:8080/swagger-ui.html`                                      | Swagger UI lists the `/api/v1` documentation.                                                    |
| OpenAPI          | `GET http://localhost:8080/v3/api-docs`                                           | JSON with the OpenAPI specification.                                                             |
| Broker           | `docker compose ps` and `docker compose logs -f mosquitto` in `devices/docker`    | `energy-monitor-mqtt` running with port 8883 published; the backend appears connected as `energy-monitor-backend-telemetry` and `-status`. |
| Device           | ESP32 serial monitor                                                              | `[MQTT] connected as esp32-<identifier>` and `[MQTT] telemetry published`.                       |
| First measurement| Household panel in the web                                                        | The device's first measurement appears in the web in about one minute.                           |
| Web              | `npm run dev` in Energy-Monitor-application                                       | Vite at `http://localhost:5173` shows the sign-in screen.                                        |
| Mobile           | `npm start` in Energy-Monitor-Mobile                                              | Expo shows the QR and, when opened with Expo Go, the app lets you sign in against the backend.   |
| Email            | `MAIL_HOST=localhost` + Mailpit on :587                                           | Recovery codes appear at `http://localhost:8025`.                                                |

For an end-to-end check: register a user from the web, verify their email (Mailpit or Gmail), log in, create a household, link the device (section 14) and confirm that its first measurement appears (Cortés Parra & García Artunduaga, 2026).

# 18. Installation command summary

| **Step**     | **Backend (energy-monitor-backend)** | **Web (Energy-Monitor-application)** | **Mobile (Energy-Monitor-Mobile)**      |
|--------------|--------------------------------------|--------------------------------------|-----------------------------------------|
| Variables    | Copy-Item .env.example .env          | Copy-Item .env.example .env.local    | Copy-Item .env.example .env             |
| Deps.        | (Maven wrapper included)             | npm install                          | npm install                             |
| Database     | docker compose up -d                 | —                                    | —                                       |
| Keys/pepper  | openssl … (section 9.5, 9.6)         | —                                    | —                                       |
| Tests        | .\mvnw.cmd test                      | npm test                             | npm run lint                            |
| Run          | .\mvnw.cmd spring-boot:run           | npm run dev                          | npm start                               |
| Typical URL  | http://localhost:8080                | http://localhost:5173                | QR / localhost:8081 (Expo)              |

Broker (`devices/docker`): `./mosquitto/generate-dev-certs.sh` and `docker compose up -d`. Firmware (`devices/esp32-pzem-monitor`): `arduino-cli compile` and `arduino-cli upload` (section 13).

# 19. Common troubleshooting

| **Symptom**                                                 | **Probable cause**                                                                                      | **Corrective action**                                                                                           |
|-------------------------------------------------------------|---------------------------------------------------------------------------------------------------------|-----------------------------------------------------------------------------------------------------------------|
| The backend does not start and names a key path or the pepper | `SECURITY_JWT_PRIVATE_KEY_PATH`, `SECURITY_JWT_PUBLIC_KEY_PATH` or `SECURITY_RESET_PEPPER` are missing. | Define them in `.env` or in `application-local.yaml` (absolute paths) and retry.                              |
| `Access denied for user 'energy_app'` when running tests    | `SPRING_DATASOURCE_PASSWORD` is not in `.env`.                                                          | Copy it from the `MYSQL_PASSWORD` value in `.env` (section 9.2).                                                |
| The backend does not start because of a `.env` value        | A value with quotes or `$` (the file is read as `.properties`).                                         | Write values without quotes and without `$`.                                                                    |
| The browser blocks calls from the web                       | `SECURITY_CORS_ALLOWED_ORIGINS` with the Vite origin is missing.                                        | Add it in the backend's `.env` (section 10.4).                                                                  |
| Recovery codes do not arrive                                | Email misconfigured; the error is recorded in the `notification` table.                                 | Check `MAIL_*`, use Mailpit and read the inbox at `:8025` (section 16).                                         |
| Mailpit/Docker does not start                               | Docker Desktop stopped or port in use.                                                                  | Start Docker Desktop; change the port with `-p 1587:1025` if 587 is in use.                                     |
| The device does not connect (`[MQTT] connection failed, rc=-2`) | `DEVICE_MQTT_PUBLIC_HOST` points to `localhost` or a wrong IP, the certificate is not valid for the current IP, or the network blocks it. | Use the PC's LAN IP, run `generate-dev-certs.sh` again, restart the broker and check the firewall.  |
| The device shows `rc=4` or `rc=5`                           | The code or key do not match the backend's, or the backend is stopped.                                  | Start the backend and link the device again.                                                                    |
| The broker logs `context deadline exceeded` calling `backend:8080` | The backend is stopped or the ufw rule is missing.                                              | Start the backend and apply the rule from section 12.                                                           |
| The web does not find the device over Bluetooth             | The device is not in setup mode, the browser is not compatible or the page is not secure.               | Hold BOOT for 3 s until the LED blinks, use Chrome or Edge and open the web on `localhost` or over HTTPS; on Linux enable the flag from section 14. |
| `wifi_failed:auth` while linking                            | Wrong Wi-Fi password.                                                                                   | Link again with the correct password.                                                                           |
| Browser push does not arrive                                | The VAPID keys are missing or the user did not grant notification permission; the browser requires HTTPS or `localhost`. | Define `NOTIFICATION_VAPID_PUBLIC_KEY` and `NOTIFICATION_VAPID_PRIVATE_KEY`, restart the backend and enable notifications in the browser. |
| The Google button answers `503`                             | `GOOGLE_CLIENT_ID` and `GOOGLE_CLIENT_SECRET` are empty in the backend.                                 | Set both with the OAuth credentials ("Web application") and restart the backend.                                |
| Firmware upload fails: port busy                            | Another program is using the serial port.                                                               | Close the Arduino IDE serial monitor or any other program using `/dev/ttyUSB0`.                                 |
| "Sketch too big" when compiling                             | The partition scheme is not Minimal SPIFFS.                                                             | Compile with `PartitionScheme=min_spiffs` (section 13.4).                                                       |
| "~" is not expanded in the key path                         | Java does not expand the symbol in `private-key-path`.                                                  | Use the full absolute path (`C:\Users\your-user\.energy-monitor\keys\`).                                        |
| The mobile app does not reach the backend                   | `EXPO_PUBLIC_API_BASE_URL` points to `localhost`, which on the emulator or the phone is the device itself. | Use `10.0.2.2` on the emulator or the PC's IP on the phone and restart Expo (section 11.2).                   |
| Expo does not connect with Expo Go                          | Different network, firewall, or invalid QR.                                                             | Use `npx expo start --tunnel`; check corporate network scripts.                                                 |
| `npm install` fails on mobile/web                           | Node version different from the reference software.                                                     | Update to Node 22 LTS (min. 20.19) and reinstall.                                                               |

# 20. References (APA format, 7th edition)

In-text citations and the reference list follow the style of the seventh edition of the APA publication manual (American Psychological Association \[APA\], 2020).

American Psychological Association. (2020). Publication manual of the American Psychological Association (7th ed.). https://www.apa.org/

Arduino. (n.d.). Arduino CLI. https://arduino.github.io/arduino-cli/

Cortés Parra, J. E., & García Artunduaga, M. A. (2026). *EnergyMonitor software requirements specification* (version 2.0). SENA – CIES Huila Regional.

Docker Inc. (n.d.). Docker documentation. https://docs.docker.com/

Eclipse Foundation. (n.d.). Eclipse Mosquitto. https://mosquitto.org/

Expo. (n.d.). Expo documentation. https://docs.expo.dev/

Git. (n.d.). Git for Windows. https://git-scm.com/download/win

Google. (n.d.). How to create an app password. https://myaccount.google.com/apppasswords

Let's Encrypt. (n.d.). Documentation. https://letsencrypt.org/docs/

Mailpit. (n.d.). Mailpit: Web and API based SMTP testing. https://mailpit.axllent.org/

MySQL. (2026). MySQL 8.4 reference manual. https://dev.mysql.com/doc/refman/8.4/en/

Node.js Foundation. (n.d.). Node.js. https://nodejs.org/

OpenSSL. (n.d.). OpenSSL cryptography and SSL/TLS toolkit. https://www.openssl.org/

Oracle. (n.d.). Java SE downloads. https://www.oracle.com/java/technologies/downloads/

Spring. (n.d.). Spring Boot reference documentation. https://docs.spring.io/spring-boot/index.html

Vite. (n.d.). Vite guide. https://vite.dev/guide/
