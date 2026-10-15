**SERVICIO NACIONAL DE APRENDIZAJE – SENA (National Learning Service)**

**CENTER FOR INDUSTRY, BUSINESS AND SERVICES (CIES)**

**HUILA REGIONAL**

Training Program: Technology in Software Analysis and Development (ADSO)

Group (Ficha): 3145556

**ADMINISTRATOR MANUAL**

**Project: Electrical Consumption Monitor**

*Platform operation, configuration, backups and security management*

| **Item**             | **Information**                                                                 |
|----------------------|---------------------------------------------------------------------------------|
| Team members         | Juan Esteban Cortes Parra Miguel Angel Garcia Artunduaga                        |
| Instructor           | Karol Daniela Correa                                                            |
| Training center      | Center for Industry, Business and Services (CIES) – SENA Huila Regional         |
| Program              | Technology in Software Analysis and Development (ADSO)                          |
| Group (Ficha)        | 3145556                                                                         |
| Document type        | Administrator manual                                                            |
| Version              | 1.0                                                                             |
| Date prepared        | October 7, 2026                                                                 |

Year 2026

# 2. Document version control

The change history of this manual is recorded in the following table. Any new administrative operation or configuration change supported by the system must be accompanied by an update to the manual.

| **Version** | **Date**   | **Description of change**                                                                                                                                                                                                       | **Responsible**      |
|-------------|------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------|
| 1.0         | 10/07/2026 | Initial creation of the administrator manual: environment, roles and permissions, password policy, audit log, backups, updates and incidents. Citations and references in APA format.                                          | Development team     |

# 3. Table of contents

[2. Document version control [1](#document-version-control)](#document-version-control)

[3. Table of contents [2](#table-of-contents)](#table-of-contents)

[4. Introduction [3](#introduction)](#introduction)

[5. Objective [3](#objective)](#objective)

[6. Technical scope [3](#technical-scope)](#technical-scope)

[7. Administration environment and credentials [4](#administration-environment-and-credentials)](#administration-environment-and-credentials)

[7.1 Components the administrator must monitor [4](#components-the-administrator-must-monitor)](#components-the-administrator-must-monitor)

[7.2 Where the platform secrets are stored [4](#where-the-platform-secrets-are-stored)](#where-the-platform-secrets-are-stored)

[8. Security model surrounding the platform [5](#security-model-surrounding-the-platform)](#security-model-surrounding-the-platform)

[8.1 Tables that govern security [5](#tables-that-govern-security)](#tables-that-govern-security)

[8.2 Audit (\`audit_log\`) [5](#audit-audit_log)](#audit-audit_log)

[8.3 Failed access attempts (\`login_error_log\`) [5](#failed-access-attempts-login_error_log)](#failed-access-attempts-login_error_log)

[9. Roles and permissions (RBAC) [5](#roles-and-permissions-rbac)](#roles-and-permissions-rbac)

[9.1 Model [6](#model)](#model)

[10. Password policy and security configuration [6](#password-policy-and-security-configuration)](#password-policy-and-security-configuration)

[10.1 Active policy (\`password_policy\`) [6](#active-policy-password_policy)](#active-policy-password_policy)

[10.2 Dynamic parameters (\`security_configuration\`) [6](#dynamic-parameters-security_configuration)](#dynamic-parameters-security_configuration)

[11. Sessions, tokens and revocation [7](#sessions-tokens-and-revocation)](#sessions-tokens-and-revocation)

[12. Backups and restoration [7](#backups-and-restoration)](#backups-and-restoration)

[12.1 Schema backup (recommended) [7](#schema-backup-recommended)](#schema-backup-recommended)

[12.2 Restore [7](#restore)](#restore)

[12.3 Data volume [7](#data-volume)](#data-volume)

[13. Software updates [7](#software-updates)](#software-updates)

[14. Verifying the recovery email [9](#verifying-the-recovery-email)](#verifying-the-recovery-email)

[15. Common incidents and how to diagnose them [9](#common-incidents-and-how-to-diagnose-them)](#common-incidents-and-how-to-diagnose-them)

[16. Quick administration commands [9](#quick-administration-commands)](#quick-administration-commands)

[17. Administrator daily / weekly checklist [9](#administrator-daily-weekly-checklist)](#administrator-daily-weekly-checklist)

[18. References (APA format, 7th edition) [10](#references-apa-format-7th-edition)](#references-apa-format-7th-edition)

# 4. Introduction

This document is the administrator manual for the "Electrical Consumption Monitor" software. It is aimed at the person who will be in charge of the day-to-day operation of the system: starting and stopping services, security-governing configurations, monitoring system behavior, backups, software updates and incident handling (Cortés Parra & García Artunduaga, 2026).

Most of the system's administration is exposed directly through the REST API and the MySQL 8.4 database (Spring, n.d.; Docker Inc., n.d.). It is important for the reader to know that, in this version, the backend does not expose a web administration panel: the management of roles, permissions, policies and keys is carried out through the corresponding tables and the configuration files detailed here.

The academic and technical references cited in the body of the document were formatted according to the 7th edition of the APA manual (American Psychological Association \[APA\], 2020).

# 5. Objective

To provide the team that administers the system with a clear, operational and verifiable guide to safely carry out the tasks specific to platform administration.

- Describe the minimum administration environment and where each credential or configuration file is located.

- Document the role and permission model present in the database and its management via SQL.

- Explain how to review, rotate and restore encryption keys (recovery pepper and RSA pair).

- Record the procedure to run backups and restore them.

- Define the flow for updating the software without losing data and for diagnosing common incidents.

# 6. Technical scope

This manual covers administration in a development environment:

- MySQL 8.4 container and the \`energy_monitor\` database managed with Docker Compose.

- Spring Boot 4.1.1 backend (Java 21, Spring Modulith, Liquibase, JWT RS256) and its \`local\` profile.

- Recovery email via SMTP (Mailpit locally or Gmail in the cloud).

- Web and mobile application: only their operational fit is mentioned.

It does not cover the administration of production databases, cloud services, or the initial setup from scratch (all of which is already in the \*Installation Manual\*). Nor does it describe setting up the project from source code: the three repositories are described in the \*Technical Manual\*.

# 7. Administration environment and credentials

## 7.1 Components the administrator must monitor

| **Component**    | **How it is operated**                                           | **Evidence/verification**                                        |
|------------------|------------------------------------------------------------------|------------------------------------------------------------------|
| MySQL 8.4        | \`docker compose up/stop/restart\` in \`energy-monitor-backend\` | \`docker compose ps\` → \`energy-monitor-mysql\` in \`healthy\`. |
| Backend API      | \`./mvnw.cmd spring-boot:run\`                                   | GET \`/v3/api-docs\` and \`/swagger-ui.html\`                    |
| Email (Mailpit)  | \`docker run … axllent/mailpit\`                                 | Inbox at \`http://localhost:8025\`                               |
| Web (Vite 8)     | \`npm run dev\` in \`Energy-Monitor-application\`                | Client at \`http://localhost:5173\`                              |
| Mobile (Expo)    | \`npm start\` in \`Energy-Monitor-Mobile\`                       | QR + Expo Go / web (connected to the API)                                   |

## 7.2 Where the platform secrets are stored

No secret is stored in the code or in Git. The administrator must keep them only in these locations:

- \`energy-monitor-backend/.env\` — passwords \`MYSQL_PASSWORD\`, \`MYSQL_ROOT_PASSWORD\`, \`SPRING_DATASOURCE_PASSWORD\`, \`MAIL\_\*\` credentials, \`SECURITY_RESET_PEPPER\` and \`SECURITY_CORS_ALLOWED_ORIGINS\`.

- \`energy-monitor-backend/src/main/resources/application-local.yaml\` — local profile: \`spring.datasource.url\`, \`security.jwt.\*\` (paths to the keys) and \`security.reset.pepper\`.

- \`\$HOME/.energy-monitor/keys/\` — \`private.pem\` / \`public.pem\` pair (700 permissions on Linux/macOS; user folder on Windows).

- \`Energy-Monitor-application/.env.local\` — \`VITE_API_BASE_URL\`.

> *Golden rule: no secret goes into commits, manual appendices, chats or shared directories. The \`.env\` files, \`application-local.yaml\` and \`\*.pem\` are in \`.gitignore\`.*

# 8. Security model surrounding the platform

## 8.1 Tables that govern security

| **Table**                                                         | **Purpose**                                                                                       |
|-------------------------------------------------------------------|---------------------------------------------------------------------------------------------------|
| user, person, user_configuration, user_session                    | Identity, basic data, own configuration and sessions of each account.                             |
| password_policy                                                   | Active password policy (default min 8, max 64, 90 days, uppercase/numbers/symbols).               |
| password_reset_token                                              | Password recovery tokens (hash with pepper).                                                      |
| security_configuration                                            | Name/value pairs of security parameters managed by the backend.                                   |
| login_error_log                                                   | Failed access attempts with type, description, IP and date.                                       |
| system_role, permission, user_system_role, system_role_permission | RBAC model that defines which action each role can execute.                                       |
| audit_log                                                         | Audit of CREATE/UPDATE/DELETE/LOGIN/LOGIN_FAILED/LOGOUT with IP and application.                  |
| notification                                                      | Log of email attempts (SENT/FAILED status and reason).                                            |

## 8.2 Audit (\`audit_log\`)

Every relevant operation on accounts, sessions and credentials should leave a trace in \`audit_log\`. The administrator can query the latest actions as follows:

> SELECT id_audit_log, user_id, action, description, ip_address, created_at
>
> FROM audit_log
>
> ORDER BY created_at DESC
>
> LIMIT 50;

The \`action\` field accepts CREATE, UPDATE, DELETE, LOGIN, LOGIN_FAILED and LOGOUT (MySQL, 2026).

## 8.3 Failed access attempts (\`login_error_log\`)

Denied logins are recorded with the reason and origin address. This is useful for detecting brute force:

> SELECT error_type, ip_address, COUNT(\*) AS intentos, MAX(created_at) AS ultimo
>
> FROM login_error_log
>
> GROUP BY error_type, ip_address
>
> ORDER BY intentos DESC
>
> LIMIT 20;

\`error_type\` can be \`INVALID_PASSWORD\`, \`USER_NOT_FOUND\`, \`ACCOUNT_BLOCKED\` or \`ACCOUNT_INACTIVE\`.

# 9. Roles and permissions (RBAC)

The RBAC model exists in the database, but in this version \*\*there are no HTTP endpoints or web interface to manage it\*\*. Administration is done via SQL with the application user (\`energy_app\`) or root, from a secure terminal (Cortés Parra & García Artunduaga, 2026).

## 9.1 Model

> system_role (id_system_role, name, description, enabled)
>
> permission (id_permission, code, name, description)
>
> system_role_permission (system_role_id, permission_id)
>
> user_system_role (user_id, system_role_id)

Example of creating a role and assigning it a specific permission:

> INSERT INTO system_role (id_system_role, name, description, enabled) VALUES ('rol0000001', 'ADMIN', 'Administrador de la plataforma', 1);
>
> INSERT INTO permission (id_permission, code, name, description) VALUES ('prm0000001', 'METER.READ', 'Lectura de medidores', 'Permite ver dispositivos y mediciones');
>
> INSERT INTO system_role_permission (system_role_id, permission_id, assigned_at) VALUES ('rol0000001', 'prm0000001', NOW());
>
> INSERT INTO user_system_role (user_id, system_role_id, assigned_at) VALUES ('usr0000001', 'rol0000001', NOW());
>
> *Current security agreement: even though the table exists, no public interface renames, creates or assigns roles; any additional access must go through review by the development team before being enabled via HTTP.*

# 10. Password policy and security configuration

## 10.1 Active policy (\`password_policy\`)

The application takes the policy from the \*\*first row\*\* without \`deleted_at\`. For operational security, the schema seeds a single default row (id \`pol0000001\`): min. 8, max. 64, expiration 90 days, uppercase letters, numbers and symbols required. To change it:

> UPDATE password_policy SET min_length = 10, expiration_days = 60 WHERE id_password_policy = 'pol0000001';

Do not insert a second active row: the selection has no \`ORDER BY\` and the behavior would be non-deterministic.

## 10.2 Dynamic parameters (\`security_configuration\`)

The table stores \`config_name / config_value\` pairs that the administrator can adjust without recompiling. Edit existing values:

> SELECT id_security_configuration, config_name, config_value, description FROM security_configuration;
>
> UPDATE security_configuration SET config_value = 'valor' WHERE config_name = 'nombre_del_parametro';
>
> *Changing values the code uses to sign/validate tokens may invalidate sessions: coordinate with the development team before touching those parameters.*

# 11. Sessions, tokens and revocation

From the platform's security standpoint, any session or refresh token revocation accepts \`ADMINISTRATIVE\`, \`PASSWORD_CHANGED\` or \`REFRESH_TOKEN_REUSE\` as the reason (MySQL, 2026). To force the closing of a user's sessions (e.g. while handling an incident):

> UPDATE user_session SET revoked_at = NOW(), revoked_reason = 'ADMINISTRATIVE' WHERE user_id = 'usr0000001' AND revoked_at IS NULL;

The \`revoked_reason\` field stores the cause (\`ADMINISTRATIVE\`, \`PASSWORD_CHANGED\`, \`REFRESH_TOKEN_REUSE\`). After a revocation, the client's next \`POST /api/v1/auth/refresh\` will respond \`401\`, forcing the client to authenticate again.

# 12. Backups and restoration

## 12.1 Schema backup (recommended)

With the MySQL container running, a full dump of \`energy_monitor\` is:

> docker exec -t energy-monitor-mysql
>
> mysqldump -u root -p"\$env:MYSQL_ROOT_PASSWORD" --databases energy_monitor
>
> \| gzip \> energy_monitor\_\$(Get-Date -Format yyyyMMdd).sql.gz

## 12.2 Restore

> gzip -dc energy_monitor_20261007.sql.gz \|
>
> docker exec -i energy-monitor-mysql mysql -u root -p"\$env:MYSQL_ROOT_PASSWORD"

## 12.3 Data volume

MySQL's persistent data lives in the Docker volume declared as \`mysql-data\`. Deleting it with \`docker compose down -v\` erases the schema and, therefore, the database; do not do so unless you want to erase all the information (Docker Inc., n.d.).

> *Never store backups on the same machine as the database; copy the encrypted SQL to external storage or to the cloud with restricted access.*

# 13. Software updates

1.  Communicate the maintenance window to the stakeholder and make a prior backup (section 12.1).

2.  Stop the backend and the clients: \`Ctrl+C\` in the backend terminal; \`docker compose stop\` if you are going to touch the database.

3.  \`git pull\` (or download the new deliverable) in the three affected repositories.

4.  Review \`.env.example\` and \`application-local.yaml.example\` for new variables; reproduce those changes in the actual \`.env\` and \`application-local.yaml\` without sharing secrets.

5.  Bring up dependencies: \`docker compose up -d\` (Liquibase will apply the new changesets when the backend starts).

6.  Run tests: \`.\mvnw.cmd test\` (backend) and \`npm test\` / \`npm run lint\` (web and mobile). All must pass before restarting.

7.  Start again: \`.\mvnw.cmd spring-boot:run\`, \`npm run dev\`, and validate the healthcheck \`/v3/api-docs\`.

> *Changing \`SECURITY_RESET_PEPPER\` invalidates all current recovery codes. Rotating it is a security decision, not a habit.*

# 14. Verifying the recovery email

Each send attempt is recorded in the \`notification\` table with status \`SENT\` or \`FAILED\` and the reason. To audit whether email is failing:

> SELECT id_notification, status, reason, created_at
>
> FROM notification
>
> WHERE status = 'FAILED'
>
> ORDER BY created_at DESC
>
> LIMIT 20;

If almost all fail, it is usually due to incorrect SMTP credentials, to Gmail requiring an "app password" instead of the account password, or to \`MAIL_FROM\` not matching \`MAIL_USERNAME\` (Google, n.d.). In development, check the \`status\` column and the recorded reason, and verify that Mailpit is running (\`http://localhost:8025\`).

# 15. Common incidents and how to diagnose them

| **Symptom**                                                                        | **Possible cause**                                              | **Action**                                                                                                    |
|------------------------------------------------------------------------------------|-----------------------------------------------------------------|---------------------------------------------------------------------------------------------------------------|
| The backend does not start and mentions keys or pepper                             | \`SECURITY_JWT\_\*\` or \`SECURITY_RESET_PEPPER\` are missing.  | Define them in \`.env\`/\`application-local.yaml\` with absolute paths (see installation manual, section 9).  |
| Tests or backend give \`Access denied for user 'energy_app'\`                      | \`SPRING_DATASOURCE_PASSWORD\` is not in \`.env\`.              | Replicate it from \`MYSQL_PASSWORD\` (section 9.2 of the installation manual).                                |
| Recovery emails do not arrive                                                      | SMTP misconfigured or Mailpit turned off.                       | Check \`notification\` (section 14) and \`MAIL\_\*\`.                                                         |
| The web client shows CORS errors                                                   | \`SECURITY_CORS_ALLOWED_ORIGINS\` does not include the Vite origin. | Add it (section 10.4 of the installation manual).                                                         |
| Frequent \`INVALID_PASSWORD\`, \`USER_NOT_FOUND\` in \`login_error_log\`           | Invalid access attempts or brute force.                         | Review repeated IPs and block them in the firewall if necessary.                                              |
| Sessions that remain valid after a password change                                 | Revocation not propagated ahead of clock.                       | Check \`user_session.revoked_at\` and \`refresh_token.revoked_reason\`; repeat the revocation (section 11).   |
| Docker / MySQL does not start                                                      | Docker Desktop is off or port 3306 is in use.                   | Start Docker Desktop; change the port in \`docker-compose.yml\` and re-create the container.                  |

# 16. Quick administration commands

| **Task**                                    | **Command**                                                                                                                                                  |
|---------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------|
| View container status                       | \`docker compose ps\` (from \`energy-monitor-backend\`)                                                                                                     |
| View last 10 audited actions                | \`SELECT \* FROM audit_log ORDER BY created_at DESC LIMIT 10;\`                                                                                              |
| View latest login errors                    | \`SELECT \* FROM login_error_log ORDER BY created_at DESC LIMIT 20;\`                                                                                        |
| Force closing a user's sessions             | UPDATE \`user_session\` SET revoked_at/grace… (section 11)                                                                                                   |
| Schema backup                               | \`mysqldump … \| gzip\` (section 12.1)                                                                                                                       |
| Fix private key permissions                 | \`icacls private.pem /inheritance:r /grant:r "%USERNAME%:(R)"\`                                                                                              |
| Query a user's role                         | \`SELECT u.id_usuario, r.name FROM user u JOIN user_system_role usr ON usr.user_id=u.id_usuario JOIN system_role r ON r.id_system_role=usr.system_role_id;\` |
| Shut down all development containers       | \`docker compose stop\` (backend) and \`Ctrl+C\` in Vite/Expo                                                                                                |

# 17. Administrator daily / weekly checklist

| **Check**                                                | **Frequency**  | **Expected status**                                   |
|----------------------------------------------------------|----------------|-------------------------------------------------------|
| Healthy containers (\`docker compose ps\`)              | Daily          | \`energy-monitor-mysql → healthy\`                    |
| Application and Liquibase logs                           | Daily          | No context errors or unhandled exceptions             |
| \`audit_log\` and \`login_error_log\`                   | Daily          | No bursts of \`LOGIN_FAILED\` or per-IP spikes        |
| \`FAILED\` notifications                                | Daily          | \`FAILED\` queue at zero or with a known reason       |
| Encrypted SQL backup                                     | Weekly         | At least one recent file outside the server           |
| Review \`.env\` and keys not committed                  | Weekly         | Clean \`git status\`; \`pem\` and \`.env\` ignored    |
| Notify the dev team if the policy or permissions changed | Per change     | Manual and UseCaseModel tests updated                 |

# 18. References (APA format, 7th edition)

In-text citations and the reference list follow the style of the seventh edition of the APA publication manual (American Psychological Association \[APA\], 2020).

American Psychological Association. (2020). Publication manual of the American Psychological Association (7th ed.). https://www.apa.org/

Cortés Parra, J. E., & García Artunduaga, M. A. (2026). *EnergyMonitor software requirements specification* (version 2.0). SENA – CIES Huila Regional.

Docker Inc. (n.d.). Docker documentation. https://docs.docker.com/

Google. (n.d.). How to create an app password. https://myaccount.google.com/apppasswords

MySQL. (2026). MySQL 8.4 reference manual. https://dev.mysql.com/doc/refman/8.4/en/

Spring. (n.d.). Spring Boot reference documentation. https://docs.spring.io/spring-boot/index.html
