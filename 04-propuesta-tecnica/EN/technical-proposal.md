# EnergyMonitor Technical Proposal

Juan Esteban Cortés Parra and Miguel Ángel García Artunduaga

Servicio Nacional de Aprendizaje (SENA)

Technologist in Software Analysis and Development (ADSO), group (ficha) 3145556

October 12, 2026

## Abstract

This document presents the technical proposal of EnergyMonitor, a system that lets households see the electrical consumption of each appliance, identify the periods of highest demand, receive alerts and get saving recommendations. It describes the chosen technology stack and its justification, the adopted software architecture (a modular monolith with internal events), the hosting infrastructure and the project costs. The solution integrates an ESP32 device with a PZEM-004T V3 sensor, a core service in Java 21 with Spring Boot 4.1.1, a MySQL 8.4 database, a Mosquitto message broker with TLS, a React 19 web application and an Android mobile application with Expo SDK 57. The estimated total cost is COP 57,717,597, of which COP 8,073,997 is hardware, COP 743,600 is infrastructure and COP 48,900,000 is human resources. The document also includes the schedule until October 16, 2026, the pending technical risks and the differences with the project charter.

*Keywords:* technical proposal, architecture, modular monolith, costs, infrastructure, Internet of Things

## Introduction

EnergyMonitor is a formative project of the Technologist in Software Analysis and Development (ADSO) program at SENA, developed by two trainees and due on October 16, 2026. Its purpose is for households to see how much energy each appliance uses, detect the moments of highest demand, receive alerts for unusual consumption and get saving recommendations based on their own measurements.

This proposal answers the question of whether the project is technically and economically viable with the team's resources. It is organized in three blocks: the justification of the technology stack, the software architecture, and the infrastructure with its costs. It then presents the schedule and the status by component, the technical risks and the differences with the project charter. The functional and non-functional requirements are detailed in the requirements specification (folder `02-especificacion-requisitos`), and the objectives, scope and team, in the project charter (folder `01-documento-general-proyecto`).

Data that the team has not yet defined is marked `[TO BE CONFIRMED]` and is not assumed.

## Technology Stack Justification

### Solution Overview

Data flows through five pieces. The ESP32 device reads the PZEM-004T V3 sensor over Modbus RTU at 9600 baud (GPIO16 as receive and GPIO17 as transmit, firmware 1.3.8) and publishes over MQTT 3.1.1 with TLS on port 8883 to the Mosquitto broker. The Java core service receives the readings, stores them in MySQL, evaluates alerts and recommendations and sends notifications. The web application and the Android mobile application query the service through a REST API. The device sends telemetry every 60 seconds and its status every five minutes. Linking is done over Bluetooth Low Energy from the web, with a configuration portal as an alternative.

### Decisions by Layer

Table 1 summarizes the technology chosen for each layer and the reason that supports it. The last column compares each choice with common market alternatives. It is a reference comparison, not a record of formal evaluations made during the project.

**Table 1**

*Chosen Technology and Justification by Layer*

| Layer | Chosen technology | Justification | Market alternatives |
|---|---|---|---|
| Core service | Java 21, Spring Boot 4.1.1, Spring Modulith, Spring Security, Spring Data JPA | Free software tools with efficiency, scalability and ease of integration. Spring Modulith splits the system into modules with controlled dependencies inside a single application. || Node.js with NestJS or Express: broad ecosystem and fast development, but without the verifiable modularity of Spring Modulith. Django (Python) and ASP.NET Core (C#): valid, with less team experience. |
| Database | MySQL 8.4 with Liquibase migrations | Free software. Liquibase versions database changes with rollback, which gives traceability. || PostgreSQL: equally free and richer in advanced features, unnecessary at this volume. MongoDB: flexible, but the project data is relational (homes, members, devices). Firebase: usage-based cost and provider dependency. |
| Messaging with the device | MQTT 3.1.1 with TLS on Eclipse Mosquitto | Standard Internet of Things protocol, suitable for small devices (OASIS, 2014). Mosquitto is free and the certificates come from Let's Encrypt. Encryption protects readings in transit. || Periodic HTTP from the device: simpler, but uses more of the microcontroller's resources and has no last will mechanism. CoAP: less support in brokers and tools. Brokers such as EMQX or HiveMQ: more capacity than a single server needs. |
| Device | ESP32 with PZEM-004T V3 sensor | Low-cost microcontroller with Wi-Fi and Bluetooth (COP 24,000) and an energy sensor of COP 70,000. It records voltage, current, active power and accumulated energy. || Arduino with a Wi-Fi module or Raspberry Pi: the first needs more components and the second has higher cost and consumption. Sensors such as the SCT-013 (current clamp) need calibration and their own circuit, while the PZEM-004T delivers already computed values. |
| Web application | React 19, Vite 8, Zustand, Axios, React Router, React Hook Form, Zod, i18next, Recharts | Free software. It supports interactive charts, validated forms and an interface in several languages. Web Bluetooth enables linking the device directly from the browser. || Angular or Vue: equally viable, with less prior team knowledge. Next.js: adds server-side rendering, which the application does not need. Create React App: deprecated in favor of Vite. |
| Mobile application | React Native 0.86 with Expo SDK 57, expo-router, Redux Toolkit | Free software. It reuses the team's React knowledge and serves Android only, which is the defined scope. || Flutter (Dart): good performance, but requires another language and does not reuse what was learned on the web. Native Kotlin: maximum control on Android, at a higher development cost. A progressive web app: less access to phone features. |
| Infrastructure | Docker Compose on a Hostinger KVM 2 VPS | Reproducible deployment on a single low-cost server. The same server hosts the core service, the database and the broker. || AWS, Google Cloud or Azure: more services and automatic scaling, with variable cost and higher complexity. Other VPS providers (DigitalOcean, Contabo): comparable. Own server: requires network, power and physical security. |

### General Selection Criteria

The decisions in Table 1 share three criteria that guided the selection:

- Zero license cost: all software in the stack is free, and only the infrastructure and the domain are paid.
- A single server: the expected load of a formative project does not justify several independent services. The system can grow by increasing the server's resources, without migrating to microservices.
- Team knowledge: the stack uses tools that both members handle, which lowers the risk of delivering on time.

### Accepted Limitations

Two decisions have known limits. Web Bluetooth works only in compatible browsers, such as Chrome, and requires a secure context; in other browsers the Wi-Fi configuration portal is used. The mobile application is limited to Android, so there is no iPhone version.

## Software Architecture

### Chosen Architectural Pattern

EnergyMonitor is a modular monolith built with Spring Modulith. It is a single deployable application, divided internally into seven modules with clear boundaries. It does not use microservices, Kafka or an API Gateway. Table 2 describes each module.

**Table 2**

*Core Service Modules*

| Module | Responsibility |
|---|---|
| security | Registration, authentication, sessions, permissions, email verification, password recovery, Google Sign-In, profile and account deletion |
| home | Homes, members, OWNER and MEMBER roles, favorites and thresholds |
| device | Registration, linking, editing and unlinking of devices, credentials and MQTT authorization |
| measurement | Measurements, telemetry and status messages |
| alert | Consumption and connectivity alerts |
| recommendation | Saving rules |
| notification | Deliveries, preferences, email and Web Push |

### Organization Rules

- Modules depend on each other only through their `api` packages.
- Hexagonal architecture is applied inside each module.
- Database changes are made with Liquibase changesets that include rollback.
- Identifiers are VARCHAR(10) and deletion is logical through `deleted_at`.

### Communication between Modules

Modules communicate through internal Spring events, in memory, without Kafka. Table 3 lists them.

**Table 3**

*Internal Events between Modules*

| Event | Source | Destination |
|---|---|---|
| MeasurementRecorded | measurement | alert and recommendation |
| AlertRaised | alert | notification |
| RecommendationCreated | recommendation | notification |
| AccountDeleted | security | home |

A device's loss of connectivity is identified through the status records and the last will and silence mechanisms of the MQTT protocol.

### Interfaces

The API is REST under `/api/v1` and covers authentication, accounts, homes, members, thresholds, devices, measurements, statistics, alerts, recommendations and notification preferences. It includes Actuator and Swagger. There are also internal MQTT authentication and authorization endpoints for Mosquitto, with a five-minute cache.

### Security

- JWT tokens signed with RS256. The access token lasts 15 minutes.
- Opaque, stateful refresh tokens with rotation and reuse detection, stored in the database.
- Global permissions queried dynamically, with no roles inside the JWT.
- Identity is taken from the `sub` claim through CurrentUserResolver. The X-User-Id header is ignored.
- CORS closed by default, with origins defined through environment variables.
- Google Sign-In requires configured OAuth credentials.
- Devices authenticate to the broker with `device_code` and `api_key`.
- The protection of users' personal data refers to Law 1581 of 2012 (Congreso de la República de Colombia, 2012).

### Relevant Domain Rules

Global roles are managed by security, while the OWNER and MEMBER roles are per home, so a user can belong to several homes with different roles. Thresholds belong to the home, with default values of 6 kWh daily and 180 kWh monthly. The power levels are LOW (0 to 500 W), MEDIUM (500 to 1500 W), HIGH (1500 to 3000 W) and CRITICAL (from 3000 W). Account deletion is transactional: it validates the password, closes the sessions, logically deletes personal data, releases the email and manages memberships.

### Clients

The web application separates its features by characteristic, manages texts with i18n and concentrates HTTP calls in `src/services/`. It is tested with Vitest and Testing Library. The mobile application uses the expo-router structure and per-component style files, and is available in Spanish, English, French and Portuguese. On the Android emulator 10.0.2.2 is used instead of localhost, and CORS does not apply in React Native.

### Development Conventions

Code is written in English and academic deliverables in Spanish. Commits follow Conventional Commits with a module scope and an imperative English message. Branches are `feat/*` or `fix/*`, with integration through a pull request to `dev` and then to `main`.

## Infrastructure and Costs

### Hosting Infrastructure

Table 4 describes the planned infrastructure.

**Table 4**

*Hosting Infrastructure*

| Element | Detail |
|---|---|
| Provider | Hostinger |
| Plan | VPS KVM 2: 2 vCPU, 8 GB of RAM and 100 GB NVMe |
| Deployment | Docker Compose with the core service, MySQL 8.4 and Mosquitto |
| Access | HTTPS proxy in front of the API |
| Certificates | Let's Encrypt, free of charge |
| Domain | .com domain, estimated at COP 50,000 per year `[TO BE CONFIRMED]` at the time of payment |

### Hardware Cost

Table 5 details the hardware. The first ten lines are the prototype of the measuring device, and the last two are the development team's equipment.

**Table 5**

*Hardware Cost (COP)*

| Item | Quantity | Unit cost | Subtotal |
|---|---|---|---|
| ESP32 with Wi-Fi and Bluetooth | 1 | 24,000 | 24,000 |
| PZEM-004T-100A V3 meter | 1 | 70,000 | 70,000 |
| HLK-PM03 AC-DC power supply | 1 | 20,000 | 20,000 |
| 20 cm male-to-male jumper wires | 40 | 300 | 12,000 |
| 20 cm male-to-female jumper wires | 40 | 300 | 12,000 |
| 2x14 AWG sheathed cable | 3 | 3,333 | 9,999 |
| Gauge 14 red and black wire | 1 | 2,400 | 2,400 |
| JST XH2.54 connector | 1 | 1,300 | 1,300 |
| 20 cm female-to-female jumper wires | 6 | 383 | 2,298 |
| Plastic box (prototype casing) | 1 | 20,000 | 20,000 |
| Development team computer | 2 | 3,500,000 | 7,000,000 |
| Development team phone | 2 | 450,000 | 900,000 |
| **Total hardware** | | | **8,073,997** |

Of this total, COP 173,997 is the prototype of the measuring device. It is the direct spending on components, close to the COP 180,000 stated in the project charter.

### Software, Licensing and Infrastructure Cost

All software in the stack is free and generates no license cost. Table 6 separates what is free from what is paid.

**Table 6**

*Software, Licensing and Infrastructure Cost (COP)*

| Item | Type | Cost |
|---|---|---|
| Java 21 (OpenJDK), Spring Boot, Spring Modulith, Spring Security, Spring Data JPA | Free | 0 |
| MySQL 8.4 and Liquibase | Free | 0 |
| Eclipse Mosquitto | Free | 0 |
| Docker and Docker Compose | Free | 0 |
| React 19, Vite 8, Zustand | Free | 0 |
| Expo SDK 57 and React Native 0.86 | Free | 0 |
| Arduino IDE and Visual Studio Code | Free | 0 |
| Let's Encrypt (TLS certificates) | Free | 0 |
| Hostinger VPS KVM 2, 24 months at COP 28,900 per month | Paid | 693,600 |
| .com domain, 1 year (estimated) | Paid | 50,000 |
| **Total software and infrastructure** | | **743,600** |

The rate of COP 28,900 per month is promotional. Renewing the plan costs COP 50,900 per month, equal to COP 610,800 per year.

### Third-Party Services

Table 7 lists the external services the system uses. The cost of these services is not yet defined, except for the certificates.

**Table 7**

*Third-Party Services*

| Service | Use | Cost |
|---|---|---|
| Let's Encrypt | TLS certificates | 0 |
| Google Sign-In | Google sign-in on the web | `[TO BE CONFIRMED]` |
| SMTP email service | Sending verification, recovery and notification messages | `[TO BE CONFIRMED]` |
| Web Push | Browser notifications | `[TO BE CONFIRMED]` |

### Human Resources Cost

Table 8 values the team's work over seven months. It is an estimated cost to size the project, not an actual payment.

**Table 8**

*Estimated Human Resources Cost (COP)*

| Role | Months | Monthly cost | Subtotal |
|---|---|---|---|
| Project lead | 7 | 2,000,000 | 14,000,000 |
| Software developer | 7 | 2,500,000 | 17,500,000 |
| Database manager | 4 | 1,200,000 | 4,800,000 |
| Analyst | 7 | 1,800,000 | 12,600,000 |
| **Total human resources** | | | **48,900,000** |

### Consolidated Budget

**Table 9**

*Consolidated Project Budget (COP)*

| Category | Subtotal | Share |
|---|---|---|
| Hardware | 8,073,997 | 14.0% |
| Software and infrastructure | 743,600 | 1.3% |
| Human resources (7 months) | 48,900,000 | 84.7% |
| Other | 0 | 0.0% |
| **Estimated total** | **57,717,597** | **100%** |

Nearly 85% of the cost is the team's work. Excluding human resources, the investment in equipment, prototype and server is COP 8,817,597, and the part that actually has to be paid on a recurring basis is the infrastructure.

### Costs for Users

Users pay no software licenses. They need an Android phone or a modern browser. Table 10 shows reference price ranges for the equipment.

**Table 10**

*Reference Price Ranges of User Equipment (COP)*

| Equipment | Price range |
|---|---|
| Xiaomi Redmi 13 phone | 450,000 to 700,000 |
| Samsung Galaxy A55 phone | 1,600,000 to 2,200,000 |
| HP 240 G9 laptop (Core i5, 8 GB, 512 GB SSD) | 2,500,000 to 2,900,000 |
| Dell Inspiron 15 laptop (Core i5, 8 GB) | 1,800,000 to 2,500,000 |

The cost of the measuring device for one home is that of its prototype, about COP 174,000 in components. A sale price or serial production cost is outside the scope `[TO BE CONFIRMED]`.

### Maintenance and Licensing

The maintenance plan has three modes. Corrective maintenance handles communication, loading, notification and history failures. Preventive maintenance includes updating dependencies (Spring Boot, React, Expo, firmware), periodic MySQL backups and server snapshots, and security tests. Evolutionary maintenance considers smart meters, over-the-air firmware updates (today done by USB cable), an administration interface and adaptation to new versions of Android and browsers. Corrective support is provided during the first month after deployment.

The software and hardware are licensed under a free-use modality with community recognition: they may be used, copied and modified for educational, research and social purposes, keeping the reference to the team and SENA. Commercial use requires a formal agreement between the developers and SENA, and may carry a license fee `[TO BE CONFIRMED]`.

## Schedule and Current Status

### Schedule

The project started on May 1, 2026 and ends with delivery on October 16, 2026. Table 11 shows the phases.

**Table 11**

*Project Phases and Dates*

| Phase | Period | Expected result |
|---|---|---|
| 1. Foundation and core loop | May 1 to June 30 | A user registers, creates a home, links a device and sees measurements arrive |
| 2. Consumption visibility and alerts | July 1 to August 31 | Consumption is visible and the system warns when something is abnormal |
| 3. Recommendations and internationalization | September 1 to 30 | Recommendations based on own consumption and an interface in more than one language |
| 4. Consolidation and delivery | October 1 to 16 | System ready for demonstration and defense |

### Status by Component

Table 12 presents progress as of October 12, 2026, when the project is in phase 4.

**Table 12**

*Status by Component as of October 12, 2026*

| Component | Status |
|---|---|
| Measuring device (firmware 1.3.8) | Operational |
| Core service (seven modules) | Working end to end |
| Web application | In final adjustments |
| Android mobile application | Under development |

Pending in the core service are the integration of home deletion into `dev` (branch `feat/home-deletion`, through a pull request), home ownership transfer (functionality that does not yet exist), logical deletion of a home and device history management by linking period. Before delivery, the pending local changes must be confirmed and recorded, and the user manuals, tests and the technical and cost report must be completed.

## Pending Technical Risks

**Table 13**

*Pending Technical Risks*

| Code | Risk | Effect |
|---|---|---|
| RT-01 | The device `api_key` is stored without a hash | Credentials exposed if the database is accessed |
| RT-02 | Telemetry uses MQTT with QoS 0 | Possible loss of readings. QoS 1 will be evaluated |
| RT-03 | Web tokens are stored in localStorage | Risk of theft through XSS, acknowledged |
| RT-04 | No over-the-air (OTA) firmware update | The device is updated only by cable |
| RT-05 | Authentication attempt limits are kept in memory | They reset when the service restarts |
| RT-06 | Persistence tests require an active database | More effort to run them |
| RT-07 | The access token remains valid for up to 15 minutes after account deletion | Brief window of residual access |
| RT-08 | Some edge cases of connectivity alerts do not resolve automatically | Alerts that need manual review |
| RT-09 | Web Bluetooth has compatibility limitations and requires a secure context | The configuration portal must be used in other browsers |

## Documentary Discrepancies

Table 14 points out the differences between what other project documents say and what is implemented or broken down in this proposal. They were verified against the code of the core service and the web application. This proposal always adopts the implemented state.

**Table 14**

*Detected Differences and Proposed Action*

| Code | Source of the difference | What it says | Verified state | Action |
|---|---|---|---|---|
| DD-01 | Web application README | Recommendations are simulated because the `recommendation` module of the core service does not exist | The `recommendation` module exists in the core service and evaluates five saving rules | Correct the web application README |
| DD-02 | Project charter | Investment of about COP 180,000 and total cost `[TO BE CONFIRMED]` | Total estimated budget of COP 57,717,597. The COP 180,000 is the direct spending on prototype components (COP 173,997) | Complete the charter's total cost with this figure |
| DD-03 | Project charter | Hosting and paid services: not contemplated | Paid Hostinger KVM 2 VPS (COP 693,600 for 24 months) and domain (COP 50,000 per year) | Reflect the paid infrastructure in the charter's budget |

## References

Congreso de la República de Colombia. (2012, October 17). *Ley 1581 de 2012: Por la cual se dictan disposiciones generales para la protección de datos personales* [Law 1581 of 2012: By which general provisions for the protection of personal data are issued]. Diario Oficial No. 48,587.

OASIS. (2014). *MQTT Version 3.1.1* (OASIS Standard). https://docs.oasis-open.org/mqtt/mqtt/v3.1.1/mqtt-v3.1.1.html
