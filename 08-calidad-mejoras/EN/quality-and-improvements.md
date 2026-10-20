# EnergyMonitor Quality and Improvements Report

Juan Esteban Cortés Parra and Miguel Ángel García Artunduaga

Servicio Nacional de Aprendizaje (SENA)

Technologist in Software Analysis and Development (ADSO), group (ficha) 3145556

October 16, 2026 (updated October 20, 2026)

## Version Control

**Table 1**

*Document version control*

| Version | Date       | Description of change                                                                                      | Authors                                       |
|---------|------------|------------------------------------------------------------------------------------------------------------|-----------------------------------------------|
| 1.0     | 10/16/2026 | First version: clean code audit, technical debt registry and roadmap for version 2.0.                      | Cortés Parra, J. E.; García Artunduaga, M. A. |
| 1.1     | 10/20/2026 | Debt follow-up: status of each item, new static scan, continuous integration with GitHub Actions, operations debt arising from the production deployment (DT-19 to DT-22) and updated roadmap. | Cortés Parra, J. E.; García Artunduaga, M. A. |

## Abstract

This document is the quality and improvements report of EnergyMonitor and is written with a technical audit and scalability approach. It presents the result of the static scans that the project does have configured —ESLint in the web application and `expo lint` in the mobile application— and states as a finding that the core service has neither static analysis nor a coverage report. The web scan reports 5 findings (3 errors and 2 warnings) and the mobile scan 12 warnings in source code files. From those results, from the defects and risks recorded in the test plan and the technical proposal, and from the go-live described in the implementation and deployment manual, a registry of 22 technical debt items is built with their evidence, impact, severity, effort, priority and status. As of October 20, 2026, three items are resolved —among them continuous integration, which now runs the tests on every pull request and deploys to production with approval— and two are partially resolved. On that registry the improvement plan and the roadmap of version 2.0 are defined, prioritizing the payment of the quality, verification and operations debt (coverage, hermetic tests, secure session, off-site backups and monitoring) before the advanced features (over-the-air updates and smart meters).

*Keywords:* software quality, clean code, static analysis, technical debt, continuous integration, scalability, ESLint, EnergyMonitor

## Introduction

### Purpose

To document the quality status of the EnergyMonitor code and plan its improvement before and after the delivery of October 28, 2026. The document brings together the clean code analysis, the technical debt management and the improvement plan and roadmap for version 2.0. The structure of the report is inspired by the ISO/IEC 25010 product quality model (maintainability, reliability, security and performance efficiency), but it does not apply a certified assessment.

### Scope

The audit covers the source code of the components that are versioned in the project repositories: the core service (Java 21 and Spring Boot 4.1.1), the web application (React 19 and Vite 8), the mobile application (React Native 0.86 and Expo SDK 57) and, partially, the device firmware, the infrastructure files and the continuous integration pipelines. Automated static analysis can only run on the clients, because they are the only ones that declare linting tools; the core service is assessed through targeted manual review and through the debt already documented in the test plan (folder `07-pruebas`) and in the technical proposal (folder `04-propuesta-tecnica`). Production operations are assessed with the implementation and deployment manual (folder `09-implementacion`).

### Approach of the Document

The language is that of a technical audit oriented to scalability. For that reason it does not limit itself to listing defects: every finding is turned into a technical debt item with a proposed action and a status, and every action is ordered in a roadmap that separates the payment of debt (version 1.1) from functional growth (version 2.0). The scalability decisions are justified by what the system is today —a modular monolith on a single server— and do not assume an architecture that the project has not adopted.

### Document References

The technical debt identifiers (DT-01 to DT-22) and improvement identifiers (MP-01 to MP-20) are specific to this document. The defects (DEF-01, DEF-02), the observations (DOC-01) and the risks (R-01 to R-09) come from the test plan and report; the technical risks (RT-01 to RT-09) and the documentary discrepancies (DD-01 to DD-03) come from the technical proposal; the requirements (RF-*, RNF-*) come from the requirements specification. The codes and the numbering are identical in the Spanish and English versions.

## Audit Methodology

The audit combined four sources: the execution of the available static analyzers, the targeted manual review of the highest-risk code, the consolidation of the debt already detected in the other project documents and the review of the pipelines and of production operations. Version 1.1 repeated the scans on October 20, 2026 and checked the status of each debt item in the code. Table 2 summarizes the sources and their use.

**Table 2**

*Audit sources and their use*

| Source | Type | Use in this report |
|---|---|---|
| `eslint .` (ESLint 9.39.4) in `frontend/energy-monitor-web` | Actual run | Clean code findings of the web application (Table 4) |
| `eslint .` / `expo lint` (`eslint-config-expo`) in `frontend/energy-monitor-mobile` | Actual run | Clean code findings of the mobile application (Table 5) |
| `pom.xml` of the core service | Inspection | Confirms the absence of static analysis and coverage (DT-11) |
| `.github/workflows/ci-cd.yml` pipelines of backend, web and infrastructure | Inspection and actual run | Status of continuous integration (DT-12, DT-16, DT-21) |
| Test plan and report (`07-pruebas`) | Consolidation | Defects, risks and requirements without coverage (DT-01, DT-07, DT-14, DT-18) |
| Technical proposal (`04-propuesta-tecnica`) | Consolidation | Technical risks, evolutionary maintenance and discrepancies (DT-03 to DT-10, DT-17) |
| Implementation and deployment manual (`09-implementacion`) | Consolidation | Production operations debt (DT-19 to DT-22) |
| Manual code review | Inspection | Bad practices with `file:line` evidence (Table 6) and debt status (Table 7) |

*Methodological note:* the project does not have SonarQube or any other continuous analysis platform configured. No SonarQube metrics are reported because they do not exist; their absence is recorded as a finding (DT-11) and as a proposed improvement (MP-16).

## Clean Code Analysis

### Available Static Analysis Tools

Table 3 shows which static analysis and automated verification tools actually exist in each component and in what state they are.

**Table 3**

*Static analysis tools and their state by component*

| Component | Tool | Version | State | Note |
|---|---|---|---|---|
| Web application | ESLint (flat config) with `eslint-plugin-react-hooks` and `eslint-plugin-react-refresh` | 9.39.4 | Configured and runnable (`npm run lint`) | It does report findings (Table 4); not run in CI |
| Mobile application | ESLint with `eslint-config-expo` | via Expo (`expo lint`) | Configured and runnable | It does report findings (Table 5); no CI |
| Core service | Checkstyle / SpotBugs / PMD / Sonar | — | Not configured | No plugin in `pom.xml`; finding DT-11 |
| Core service | JaCoCo (coverage) | — | Not configured | No coverage report; finding DT-11 |
| Infrastructure | `shellcheck`, `bash -n`, `docker compose config`, `nginx -t` | — | Configured in CI | Validate scripts, Compose files and the Nginx template on every pull request |
| Backend, web and infrastructure | GitHub Actions with required checks | — | Configured | Tests and build required to merge; no lint or coverage threshold |
| All | SonarQube / SonarCloud | — | Not configured | No continuous analysis or quality gate |

### Static Scan Results of the Web Application

`eslint .` was run on `frontend/energy-monitor-web`. The result was 5 findings: 3 errors and 2 warnings in 4 files. The version 1.1 scan (October 20, 2026) returned the same 5 findings; only the line of the first `useMemo` changed because of changes in the component. Table 4 details them.

**Table 4**

*ESLint findings in the web application*

| File:line | Severity | Rule | Description | Proposed action |
|---|---|---|---|---|
| `src/components/layout/Navbar/NavList.jsx:5` | Error | `no-unused-vars` | The variable `setOpen` is declared and not used. | Remove or use the variable |
| `src/context/ThemeContext.jsx:3` | Error | `react-refresh/only-export-components` | The file exports constants in addition to components, which breaks fast refresh. | Move constants to a separate file |
| `src/context/ThemeContext.jsx:182` | Error | `react-refresh/only-export-components` | Same case in a second export. | Move the function/constant to a separate file |
| `src/features/DetailHome/Consumption/Consumption.jsx:114` | Warning | `react-hooks/exhaustive-deps` | The `useMemo` declares an unnecessary dependency (`i18n.language`). | Review the dependency list |
| `src/features/DetailHome/ConsumptionHistory/ComsumptionHistory.jsx:167` | Warning | `react-hooks/exhaustive-deps` | Same case in the second `useMemo`. | Review the dependency list |

### Static Scan Results of the Mobile Application

The Expo lint was run on `frontend/energy-monitor-mobile`. The result was 12 warnings in source code files, with no errors. Table 5 groups them by rule; a warning also appeared about a file generated by Expo (`.expo/types/router.d.ts`) that should be excluded from the scan.

The version 1.1 scan repeated the same 12 warnings and also showed 3 `import/no-unresolved` errors on `expo-notifications` and `react-native-ble-plx`. Those modules are declared in `package.json` but were not installed on the computer where the scan ran, so the errors come from the environment and not from the code: they disappear after `npm install`. They are not counted as findings.

**Table 5**

*Lint findings in the mobile application*

| File:line | Severity | Rule | Description | Proposed action |
|---|---|---|---|---|
| `src/components/layout/Sidebar/Sidebar.tsx:23` | Warning | `@typescript-eslint/no-empty-object-type` | Empty interface that accepts any non-null value. | Type the interface or use `object`/`unknown` |
| `src/features/auth/screens/LoginScreen/LoginScreen.tsx:16` | Warning | `@typescript-eslint/no-empty-object-type` | Empty interface. | Type the interface |
| `src/features/auth/screens/NewPasswordScreen/NewPasswordScreen.tsx:12` | Warning | `@typescript-eslint/no-empty-object-type` | Empty interface. | Type the interface |
| `src/features/auth/screens/RecoverPasswordScreen/RecoverPasswordScreen.tsx:12` | Warning | `@typescript-eslint/no-empty-object-type` | Empty interface. | Type the interface |
| `src/features/auth/screens/RegisterScreen/RegisterScreen.tsx:12` | Warning | `@typescript-eslint/no-empty-object-type` | Empty interface. | Type the interface |
| `src/features/auth/screens/VerifyAccountScreen/VerifyAccountScreen.tsx:12` | Warning | `@typescript-eslint/no-empty-object-type` | Empty interface. | Type the interface |
| `src/features/auth/screens/VerifyRecoverPasswordScreen/VerifyRecoverPasswordScreen.tsx:12` | Warning | `@typescript-eslint/no-empty-object-type` | Empty interface. | Type the interface |
| `src/features/dashboard/screens/DashboardScreen/DashboardScreen.tsx:25` | Warning | `@typescript-eslint/no-empty-object-type` | Empty interface. | Type the interface |
| `src/features/auth/components/VerifyAccountForm/VerifyAccountForm.tsx:44` | Warning | `react-hooks/incompatible-library` | Use of a function incompatible with the hook rules. | Review the hook usage |
| `src/features/auth/components/VerifyRecoverPasswordForm/VerifyRecoverPasswordForm.tsx:45` | Warning | `react-hooks/incompatible-library` | Same case. | Review the hook usage |
| `src/components/OtpInput/OtpInput.tsx:23` | Warning | `@typescript-eslint/array-type` | Array type declared in a non-preferred form. | Use the `Type[]` form |
| `src/services/http/httpClient.ts:19` | Warning | `import/no-named-as-default-member` | `axios.create` is used as a member of the default import. | Import `create` by name or adjust the rule |

### Bad Practices Detected by Manual Review

Beyond the analyzers, the targeted review of the highest-risk points found the practices in Table 6. All of them have evidence in the code or in the test, proposal and deployment documents.

**Table 6**

*Bad practices and risks detected with evidence*

| ID | Component | Observed practice | Evidence | Impact |
|---|---|---|---|---|
| BP-A | Core service (security) | A global handler catches `Exception.class`, so a request with a missing required parameter responded 500 instead of 400. **Fixed in version 1.1** (DT-01). | `SecurityExceptionHandler.java` (`handleUnexpected`); defect DEF-02 of `07-pruebas` | Error contract inconsistent for the client |
| BP-B | Web application | Session tokens are stored in `localStorage`. | Risk RT-03 of `04-propuesta-tecnica` | Exposure to an XSS attack |
| BP-C | Core service (security) | The recovery attempt counter lives in memory and resets with the service. | Risk RT-05; test `InMemoryResetAttemptLimiterTest` | The limit is not shared between instances |
| BP-D | Core service | The persistence and integration tests are not hermetic: they need a real MySQL and the `SPRING_DATASOURCE_PASSWORD` variable. | Risk R-01 of `07-pruebas` | More effort and fragility when running the tests |
| BP-E | Firmware | Telemetry is published over MQTT at QoS 0. | Risk RT-02 of `04-propuesta-tecnica` | Possible loss of readings |
| BP-F | Infrastructure | MySQL backups are stored on the same server as the database. | `deploy/backup-mysql.sh`; section 8 of `09-implementacion` | A server failure loses the data and its backups at the same time |

### Observed Clean Code Strengths

The audit also recognizes decisions that favor maintainability and that should be preserved in version 2.0:

- **Hexagonal architecture per module.** Each module separates domain, application and adapters, and modules only communicate through their `api` packages. The `ModularityTests` verifies those boundaries.
- **Explicit contracts.** The core service exposes DTOs per module and a REST API under `/api/v1`, with error handling per module.
- **Centralized HTTP access in the client.** The web gathers the calls in `src/services/`, with an HTTP client and a map of errors to i18n keys.
- **Centralized internationalization.** The texts of the web and the mobile are managed with i18n instead of being embedded.
- **Documented development conventions.** Commits in Conventional Commits, one branch per task and review through pull request (`00-governance/git-conventions.md`).
- **No pending-work markers in the code.** The search for `TODO`, `FIXME`, `HACK` and `XXX` found no matches in the source code of the core service or the web application.
- **Continuous integration and deployment (version 1.1).** Every pull request of the backend, web and infrastructure runs its tests in GitHub Actions as a required check. Changes on `main` are deployed with manual approval, a prior backup, a health check and automatic rollback.

## Technical Debt Management

### Technical Debt Registry

Table 7 is the technical debt registry of the project. Each item has its own identifier (DT), component, description, evidence, type, severity, effort, priority, proposed action and status. Effort is estimated qualitatively (low, medium, high) and priority results from the matrix in the following section. The severity and effort values are a team assessment and not an automatic measurement, so they remain subject to review. The status was checked in the code on October 20, 2026: **Resolved**, **Partial** (progress made, work remains) or **Open**. Items DT-19 to DT-22 arose from the production deployment.

**Table 7**

*Technical debt registry*

| ID | Component | Description | Evidence | Type | Severity | Effort | Priority | Proposed action | Status |
|----|-----------|-------------|----------|------|----------|--------|----------|-----------------|--------|
| DT-01 | Backend · security | The global handler catches `Exception.class`, and a missing required parameter responds 500 instead of 400. | `SecurityExceptionHandler.java`; DEF-02 | Corrective | High | Low | P1 | Add a handler for `MissingServletRequestParameterException` | **Resolved**: framework errors keep their 4xx status and an invalid type answers 400 |
| DT-02 | Backend · measurement/home | The consumption history of a device is not limited to the period in which it was linked to the home. | RF-HOM-11 of `02-especificacion-requisitos`; Table 17 of `07-pruebas` | Pending feature | Medium | Medium | P2 | Filter the history by the linkage range | Open |
| DT-03 | Firmware · MQTT | Telemetry is published at QoS 0. | RT-02 | Design risk | Medium | Medium | P2 | Evaluate QoS 1 and message persistence | Open |
| DT-04 | Web · security | Session tokens are stored in `localStorage`. | RT-03 | Security risk | High | Medium | P1 | Migrate to `httpOnly` cookies or to memory plus renewal | Open |
| DT-05 | Firmware | There is no over-the-air firmware update (OTA). | RT-04 | Pending feature | Low | High | P3 | Design the over-the-air update | Open |
| DT-06 | Backend · security | The recovery attempt limit lives in memory. | RT-05 | Security risk | Medium | Medium | P2 | Shared and persistent limiter | Open |
| DT-07 | Backend · tests | The persistence and integration tests need a real MySQL; 37 classes were not run. | R-01; results section of `07-pruebas` | Verification debt | Medium | Medium | P1 | Adopt Testcontainers | **Partial**: in CI all 1131 tests run against a MySQL 8.4 service container; locally they are still not hermetic |
| DT-08 | Backend · security | The access token remains valid for up to 15 minutes after deleting the account. | RT-07 | Security risk | Low | Medium | P3 | Revocation list or shorter lifetime | Open |
| DT-09 | Backend · alert | Some edge cases of connectivity alerts require manual review. | RT-08 | Corrective | Low | Medium | P3 | Automatic resolution rules | Open |
| DT-10 | Web · device | Web Bluetooth has compatibility limitations and requires a secure context. | RT-09 | Assumed limitation | Low | Low | P3 | Document and reinforce the configuration portal | Open (in production HTTPS provides the secure context) |
| DT-11 | Backend · tooling | There is no static analysis (Checkstyle, SpotBugs, PMD, Sonar) or coverage (JaCoCo) in the `pom.xml`. | `pom.xml`; Table 4 of `07-pruebas` | Quality debt | Medium | Medium | P1 | Integrate JaCoCo, a static analyzer and SonarQube | Open |
| DT-12 | All · process | There is no continuous integration or mandatory checks, although the convention requires green CI before merging. | Absence of `.github/workflows`; `git-conventions.md` | Process debt | Medium | Medium | P1 | Create pipelines and complete branch protection | **Resolved**: pipelines in backend, web and infrastructure with required checks on `dev` and `main` |
| DT-13 | Mobile · tests | The mobile application has no automated tests and no pipeline. | R-02 | Verification debt | Medium | High | P2 | Component and service tests, and a pipeline of its own | Open |
| DT-14 | Backend · security | The test `SecuritySupportModelTest.refusesMissingNames` became outdated with respect to the code. | DEF-01 | Corrective | Low | Low | P2 | Update the test | **Resolved**: the test requires only the first name and an absent last name is accepted, as in the code |
| DT-15 | Documentation · admin | The administration module exists in the code, but has no requirements or manuals. | DOC-01 | Documentary debt | Low | Medium | P2 | Document requirements, API and administration manuals | **Partial**: the administrator and technical manuals now describe it; the requirement in the specification is still missing |
| DT-16 | Web and Mobile · style | ESLint findings: 5 in the web (3 errors) and 12 in the mobile (0 errors). | Tables 4 and 5 | Corrective | Low | Low | P2 | Fix the findings and enable lint in CI | Open (the pipelines do not run the lint yet) |
| DT-17 | Documentation | Documentary discrepancies DD-01 to DD-03 between documents and code. | `04-propuesta-tecnica`, Table 14 | Documentary debt | Low | Low | P3 | Fix the web README and the general documents | Open (the web README still says recommendations are mocked) |
| DT-18 | All · verification | Forty requirements of the specification have no automated test coverage. | Traceability section of `07-pruebas` | Verification debt | Medium | High | P2 | Complete coverage by priority | Open |
| DT-19 | Infrastructure · backups | MySQL backups are stored on the same VPS as the database and their restore has not been tested. | `deploy/backup-mysql.sh`; BP-F | Operations risk | Medium | Low | P1 | Copy every backup off the server and schedule a restore test | Open |
| DT-20 | Infrastructure · monitoring | There is no availability monitoring or alerting in production: health is only checked during a deployment. | Section 8.1 of `09-implementacion` | Operations risk | Medium | Low | P1 | External monitor of `https://energy-monitor.tech` and the broker with email alerts | Open |
| DT-21 | CI · performance | The backend suite takes about 23 minutes in GitHub Actions, which delays every pull request. | Section 9.9 of `09-implementacion` | Process debt | Low | Medium | P3 | Parallelize the suite or split out the slowest integration tests | Open |
| DT-22 | Infrastructure · environments | There is no staging environment and images are built on the production server. | Section 9.10 of `09-implementacion` | Process debt | Low | Medium | P3 | Second stack for `dev` and images published to a registry | Open |

### Prioritization Criteria

Priority combines the severity of the impact with the correction effort. Table 8 sets the rule: what affects security or the client contract and costs little is addressed first; what is costly and low impact is scheduled for the end.

**Table 8**

*Technical debt prioritization criteria*

| Priority | Rule | Meaning |
|---|---|---|
| P1 | High severity, or medium severity with low effort | Immediate correction; blocks version 1.1 |
| P2 | Medium severity with medium or high effort, or low severity with low effort | Short-term correction |
| P3 | Low severity with medium or high effort | Planned improvement; does not block |

### Priority and Effort Matrix

Table 9 places each debt item by severity and effort. The corner of high severity and low effort (P1) is the one that must be paid first. Struck-through items are already resolved.

**Table 9**

*Severity and effort matrix*

| Severity \ Effort | Low | Medium | High |
|---|---|---|---|
| High | ~~DT-01~~ | DT-04 | — |
| Medium | DT-19, DT-20 | DT-03, DT-06, DT-07, DT-11, ~~DT-12~~ | DT-13, DT-18 |
| Low | DT-10, ~~DT-14~~, DT-16, DT-17 | DT-02, DT-08, DT-09, DT-15, DT-21, DT-22 | DT-05 |

*Note:* in version 1.0, DT-04 appeared in the low-effort column although the registry assigns it medium effort. Version 1.1 places it according to the registry; its priority remains P1 because of its high severity.

## Improvement Plan and Roadmap

### Objectives of Version 2.0

Version 2.0 pursues four quality objectives, in this order: first, make quality verifiable (coverage, static analysis and continuous integration); second, close the recognized security and operations risks; third, grow in functionality without compromising stability; and fourth, prepare the system for more users and devices. Paying the P1 debt that is still open (DT-04, DT-07, DT-11, DT-19 and DT-20) is a precondition for functional growth.

### Planned Improvements

Table 10 relates each improvement to the debt it resolves or the requirement it completes, the expected benefit, its priority and its status as of October 20, 2026.

**Table 10**

*Planned improvements*

| ID | Improvement | Associated debt or requirement | Benefit | Priority | Status |
|----|-------------|--------------------------------|---------|----------|--------|
| MP-01 | Correct handling of missing required parameters (400) | DT-01, DEF-02 | Consistent error contract | P1 | **Delivered** |
| MP-02 | Test coverage with JaCoCo and a minimum threshold | DT-11 | Measurable quality | P1 | Pending |
| MP-03 | Continuous integration with lint, tests and build | DT-12 | Early detection of failures | P1 | **Delivered** (tests and build; lint moves to MP-06) |
| MP-04 | Hermetic tests with Testcontainers | DT-07 | Reproducible tests without an external MySQL | P1 | Partial (MySQL service container in CI) |
| MP-05 | `httpOnly` cookies for the web session | DT-04 | Reduces the XSS risk | P1 | Pending |
| MP-06 | Cleanup of the ESLint findings and lint in CI | DT-16 | Sustained clean code | P2 | Pending |
| MP-07 | Component and service tests in the mobile app, with a pipeline | DT-13 | Verification of the mobile client | P2 | Pending |
| MP-08 | Shared and persistent attempt limiter | DT-06 | Effective limit between instances | P2 | Pending |
| MP-09 | Telemetry with QoS 1 and persistence | DT-03 | Fewer lost readings | P2 | Pending |
| MP-10 | Consumption history by linkage period | DT-02, RF-HOM-11 | Correct data per device | P2 | Pending |
| MP-11 | Recommendation notifications (email and push) | RF-NOT-02, RF-NOT-03 | Closes unimplemented requirements | P2 | **Delivered** (the notification module consumes `RecommendationCreated`) |
| MP-12 | Complete the coverage of the pending requirements | DT-18 | Greater confidence in the requirements | P2 | Pending |
| MP-13 | Administration interface and its requirements | DT-15, DOC-01 | Documented and usable administration | P2 | Partial (interface and manuals ready; requirement missing) |
| MP-14 | Over-the-air firmware update (OTA) | DT-05, RT-04 | Firmware updatable without a cable | P3 | Pending |
| MP-15 | Smart meters and new data sources | Evolutionary maintenance | Broader product scope | P3 | Pending |
| MP-16 | Continuous analysis with SonarQube | DT-11 | Sustained quality gate | P3 | Pending |
| MP-17 | Off-site backups and periodic restore test | DT-19 | Data survives a VPS failure | P1 | Pending |
| MP-18 | Availability monitoring with alerts | DT-20 | Outages detected without relying on a user | P1 | Pending |
| MP-19 | Staging environment and images in a registry | DT-22 | Test before production and faster deployments | P3 | Pending |
| MP-20 | Faster backend suite in CI | DT-21 | Pull requests reviewable in less time | P3 | Pending |

### Optional Modules and Advanced Features

In addition to paying the debt, version 2.0 contemplates optional modules that are currently out of scope. Table 11 lists them with the scalability criterion that justifies them.

**Table 11**

*Optional modules of version 2.0*

| Module or feature | Description | Scalability justification |
|---|---|---|
| Expanded administration interface | On top of the current administration (users, statuses, roles and notices), add support, audit and content management from the web | Reduces manual work on the database |
| Over-the-air update (OTA) | Delivery of signed firmware from the server | Allows growth in the number of devices without physical visits |
| Smart meters | Integration with commercial meters in addition to the prototype | Expands the market without changing the core service |
| Advanced analytics and recommendations | Finer rules and comparisons by home type | Leverages the accumulated history |
| Enriched notifications | More channels and preferences per home and per member | Increases user retention |
| Expanded audit log | Traceability of administration actions | Common requirement when opening the system to more organizations |

### Scalability Strategy

The system is today a modular monolith on a single server: a Hostinger KVM 2 VPS (2 vCPU and 8 GB of RAM). Table 12 orders the scalability measures from lowest to highest cost, to avoid over-dimensioning the solution.

**Table 12**

*Scalability strategy*

| Level | Measure | When to apply it | Expected effect |
|---|---|---|---|
| 1 | Vertical scaling of the VPS (from KVM 2 to KVM 4: 4 vCPU and 16 GB of RAM) | As users or devices increase | Increases capacity without changing the code or reinstalling |
| 2 | Redis for cache and for the attempt limiter | When the in-memory limiter is not enough | Shares state between instances and offloads the database |
| 3 | Indexes and time partitioning of the measurements table | As the reading history grows | Faster history queries |
| 4 | Asynchronous processing with the outbox pattern | If modules are split into services | Avoids losing events when decoupling |
| 5 | Telemetry with QoS 1 and persistence | Along with improvement MP-09 | Reduces loss of readings under load |
| 6 | Monitoring with Actuator and metrics | Before growth, along with MP-18 | Visibility to decide the scaling |

The rule is not to adopt a higher level while the lower one is not exhausted. Moving to microservices is only justified if the load of one module forces scaling it separately; until then, the modular monolith with larger resources is the lowest-cost option.

### Roadmap

Table 13 orders the work in two versions: 1.1, dedicated to paying the quality, security, verification and operations debt, and 2.0, dedicated to functional growth. MP-01, MP-03 and MP-11 were already delivered in October 2026. The remaining dates are tentative and remain to be confirmed with the team.

**Table 13**

*Improvement roadmap*

| Version | Period | Deliverables | Debt it closes |
|---|---|---|---|
| 1.1 | Fourth quarter of 2026 `[TO BE CONFIRMED]` | Delivered: MP-01 (error handling) and MP-03 (continuous integration). Pending: MP-02 (coverage), MP-04 (hermetic tests), MP-05 (secure cookies), MP-17 (off-site backups) and MP-18 (monitoring) | DT-01 and DT-12 (closed); DT-04, DT-07, DT-11, DT-19 and DT-20 |
| 2.0 | 2027 `[TO BE CONFIRMED]` | MP-06 to MP-10, MP-12 to MP-16, MP-19 and MP-20: lint in CI, mobile tests, shared limiter, QoS 1, history by linkage, requirement coverage, administration requirements, OTA, meters, continuous analysis, staging and faster CI. MP-11 was already delivered | DT-02, DT-03, DT-05, DT-06, DT-13, DT-15 to DT-18, DT-21 and DT-22 |

## Conclusions

The project has an orderly code base —hexagonal architecture per module, explicit contracts, centralized HTTP access and no pending-work markers— and a broad test suite in the core service and in the web. The analyzers that exist report few and low-severity findings (5 in the web and 12 in the mobile, almost all style or typing warnings), which confirms that the code is not degraded.

Between versions 1.0 and 1.1 three debt items were closed. The error-contract defect (DT-01) was fixed and the outdated test (DT-14) was aligned with the code. In addition, continuous integration was put in place (DT-12): no branch is merged without passing its tests, and production is updated with approval, a prior backup, a health check and automatic rollback. With it, the 1131 tests of the core service run against MySQL on every pull request, which partially reduces the verification debt (DT-07).

The remaining shortcomings are in quality governance and operations: there is no static analysis or coverage in the core service, lint does not run in CI, the mobile application has no tests, and in production the backups live on the same server and there is no availability monitoring. The open P1 debt is concentrated in five items (DT-04, DT-07, DT-11, DT-19 and DT-20), of limited correction and high impact. Addressing them first —in version 1.1— leaves the system in a position to grow toward version 2.0 without dragging fragility along. Scalability is solved in levels, starting with the vertical scaling of the current VPS, and moving to microservices is conditioned on the load requiring it.

## References

Cortés Parra, J. E., & García Artunduaga, M. A. (2026). *EnergyMonitor software requirements specification* (version 2.0). SENA – CIES Regional Huila.

Cortés Parra, J. E., & García Artunduaga, M. A. (2026). *EnergyMonitor implementation and deployment manual*. SENA – CIES Regional Huila.

Cortés Parra, J. E., & García Artunduaga, M. A. (2026). *EnergyMonitor technical proposal*. SENA – CIES Regional Huila.

Cortés Parra, J. E., & García Artunduaga, M. A. (2026). *EnergyMonitor test plan and report*. SENA – CIES Regional Huila.

ESLint. (2025). *ESLint: Find and fix problems in your JavaScript code* [Documentation]. https://eslint.org/docs/latest/

GitHub. (2026). *GitHub Actions documentation* [Documentation]. https://docs.github.com/actions

International Organization for Standardization. (2023). *Systems and software engineering — Systems and software Quality Requirements and Evaluation (SQuaRE) — Product quality model* (ISO/IEC 25010:2023). https://www.iso.org/standard/78176.html

SonarSource. (2025). *SonarQube documentation* [Documentation]. https://docs.sonarsource.com/sonarqube/

## Pending Items to Confirm

- Responsible person and date for each technical debt item (columns not included so as not to assume them).
- Minimum test coverage threshold (MP-02).
- Exact periods of versions 1.1 and 2.0 of the roadmap (Table 13).
- Decision on SonarQube: self-hosted instance or SonarCloud (MP-16).
- Destination of the off-site backups and frequency of the restore test (MP-17).
- Availability monitoring tool (MP-18).
- Budget and scope of the higher-cost scalability hierarchy (levels 3 to 5 of Table 12).
