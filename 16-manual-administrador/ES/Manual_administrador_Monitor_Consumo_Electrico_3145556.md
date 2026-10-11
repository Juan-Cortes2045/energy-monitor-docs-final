**SERVICIO NACIONAL DE APRENDIZAJE – SENA**

**CENTRO DE LA INDUSTRIA, LA EMPRESA Y LOS SERVICIOS (CIES)**

**REGIONAL HUILA**

Programa de Formación: Tecnología en Análisis y Desarrollo de Software (ADSO)

Ficha: 3145556

**MANUAL DE ADMINISTRADOR**

**Proyecto: Monitor de Consumo Eléctrico**

*Operación, configuración, respaldos y gestión de seguridad de la plataforma*

| **Dato**             | **Información**                                                                 |
|----------------------|---------------------------------------------------------------------------------|
| Integrantes          | Juan Esteban Cortes Parra Miguel Angel Garcia Artunduaga                        |
| Instructor           | Karol Daniela Correa                                                            |
| Centro de Formación  | Centro de la Industria, la Empresa y los Servicios (CIES) – SENA Regional Huila |
| Programa             | Tecnología en Análisis y Desarrollo de Software (ADSO)                          |
| Ficha                | 3145556                                                                         |
| Tipo de documento    | Manual de administrador                                                         |
| Versión              | 1.0                                                                             |
| Fecha de elaboración | 07 de octubre de 2026                                                           |

Año 2026

# 2. Control de versiones del documento

El historial de cambios del presente manual se registra en la siguiente tabla. Cualquier operación administrativa nueva o cambio de configuración soportado por el sistema debe ir acompañado de una actualización del manual.

| **Versión** | **Fecha**  | **Descripción del cambio**                                                                                                                                                                          | **Responsable**      |
|-------------|------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------|
| 1.0         | 07/10/2026 | Creación inicial del manual de administrador: entorno, roles y permisos, política de contraseñas, registro de auditoría, respaldos, actualización e incidentes. Citas y referencias en formato APA. | Equipo desarrollador |

# 3. Tabla de contenido

[2. Control de versiones del documento [1](#control-de-versiones-del-documento)](#control-de-versiones-del-documento)

[3. Tabla de contenido [2](#tabla-de-contenido)](#tabla-de-contenido)

[4. Introducción [3](#introducción)](#introducción)

[5. Objetivo [3](#objetivo)](#objetivo)

[6. Alcance técnico [3](#alcance-técnico)](#alcance-técnico)

[7. Entorno y credenciales de administración [4](#entorno-y-credenciales-de-administración)](#entorno-y-credenciales-de-administración)

[7.1 Componentes que el administrador debe vigilar [4](#componentes-que-el-administrador-debe-vigilar)](#componentes-que-el-administrador-debe-vigilar)

[7.2 Dónde están los secretos de la plataforma [4](#dónde-están-los-secretos-de-la-plataforma)](#dónde-están-los-secretos-de-la-plataforma)

[8. Modelo de seguridad rodeand la plataforma [5](#modelo-de-seguridad-rodeand-la-plataforma)](#modelo-de-seguridad-rodeand-la-plataforma)

[8.1 Tablas que gobiernan la seguridad [5](#tablas-que-gobiernan-la-seguridad)](#tablas-que-gobiernan-la-seguridad)

[8.2 Auditoría (\`audit_log\`) [5](#auditoría-audit_log)](#auditoría-audit_log)

[8.3 Intentos de acceso fallidos (\`login_error_log\`) [5](#intentos-de-acceso-fallidos-login_error_log)](#intentos-de-acceso-fallidos-login_error_log)

[9. Roles y permisos (RBAC) [5](#roles-y-permisos-rbac)](#roles-y-permisos-rbac)

[9.1 Modelo [6](#modelo)](#modelo)

[10. Política de contraseñas y configuración de seguridad [6](#política-de-contraseñas-y-configuración-de-seguridad)](#política-de-contraseñas-y-configuración-de-seguridad)

[10.1 Política activa (\`password_policy\`) [6](#política-activa-password_policy)](#política-activa-password_policy)

[10.2 Parámetros dinámicos (\`security_configuration\`) [6](#parámetros-dinámicos-security_configuration)](#parámetros-dinámicos-security_configuration)

[11. Sesiones, tokens y revocación [7](#sesiones-tokens-y-revocación)](#sesiones-tokens-y-revocación)

[12. Copias de seguridad y restauración [7](#copias-de-seguridad-y-restauración)](#copias-de-seguridad-y-restauración)

[12.1 Backup del esquema (recomendado) [7](#backup-del-esquema-recomendado)](#backup-del-esquema-recomendado)

[12.2 Restaurar [7](#restaurar)](#restaurar)

[12.3 Volumen de datos [7](#volumen-de-datos)](#volumen-de-datos)

[13. Actualización del software [7](#actualización-del-software)](#actualización-del-software)

[14. Verificación del correo de recuperación [9](#verificación-del-correo-de-recuperación)](#verificación-del-correo-de-recuperación)

[15. Incidentes frecuentes y cómo diagnosticarlos [9](#incidentes-frecuentes-y-cómo-diagnosticarlos)](#incidentes-frecuentes-y-cómo-diagnosticarlos)

[16. Comandos rápidos de administración [9](#comandos-rápidos-de-administración)](#comandos-rápidos-de-administración)

[17. Checklist diario / semanal del administrador [9](#checklist-diario-semanal-del-administrador)](#checklist-diario-semanal-del-administrador)

[18. Referencias (formato APA, 7.ª edición) [10](#referencias-formato-apa-7.ª-edición)](#referencias-formato-apa-7.ª-edición)

# 4. Introducción

El presente documento es el manual de administrador del software «Monitor de Consumo Eléctrico». Está orientado a quien tendrá a su cargo la operación diaria del sistema: arranque y parada de servicios, configuraciones que gobiernan la seguridad, vigilancia del comportamiento del sistema, respaldos, actualización del software y atención de incidentes (Corte Parra y García Artunduaga, 2026).

El sistema expone la mayor parte de su administración directamente desde la API REST y la base de datos MySQL 8.4 (Spring, s. f.; Docker Inc., s. f.). Es importante que el lector sepa que, en esta versión, el backend no expone un panel web de administración: la gestión de roles, permisos, políticas y claves se realiza mediante las tablas correspondientes y los archivos de configuración que se detallan aquí.

Las referencias académicas y técnicas citadas en el cuerpo del documento se formatearon según la 7.ª edición del manual APA (American Psychological Association \[APA\], 2020).

# 5. Objetivo

Dotar al equipo que administra el sistema de una guía clara, operativa y verificable para realizar, sin riesgos, las tareas propias de la administración de la plataforma.

- Describir el entorno mínimo de administración y dónde se encuentra cada credencial o archivo de configuración.

- Documentar el modelo de roles y permisos presente en la base de datos y su gestión vía SQL.

- Explicar cómo revisar, rotar y restaurar claves de cifrado (pepper de recuperación y par RSA).

- Registrar el procedimiento para ejecutar copias de seguridad y restaurarlas.

- Definir el flujo para actualizar el software sin perder datos y para diagnosticar incidentes frecuentes.

# 6. Alcance técnico

Este manual cubre la administración en entorno de desarrollo:

- Contenedor MySQL 8.4 y base de datos \`energy_monitor\` gestionados con Docker Compose.

- Backend Spring Boot 4.1.1 (Java 21, Spring Modulith, Liquibase, JWT RS256) y su perfil \`local\`.

- Correo de recuperación vía SMTP (Mailpit en local o Gmail en la nube).

- Aplicación web y móvil: solo se mencionan aspectos de su encaje operativo.

No cubre la administración de bases de datos de producción, de servicios en la nube, ni la puesta en marcha desde cero (todo ello ya está en el \*Manual de instalación\*). Tampoco describe la puesta a punto del proyecto desde el código: los tres repositorios se describen en el \*Manual técnico\*.

# 7. Entorno y credenciales de administración

## 7.1 Componentes que el administrador debe vigilar

| **Componente**   | **Cómo se mueve**                                                | **Evidencia/verificación**                                       |
|------------------|------------------------------------------------------------------|------------------------------------------------------------------|
| MySQL 8.4        | \`docker compose up/stop/restart\` en \`energy-monitor-backend\` | \`docker compose ps\` → \`energy-monitor-mysql\` en \`healthy\`. |
| Backend API      | \`./mvnw.cmd spring-boot:run\`                                   | GET \`/v3/api-docs\` y \`/swagger-ui.html\`                      |
| Correo (Mailpit) | \`docker run … axllent/mailpit\`                                 | Bandeja en \`http://localhost:8025\`                             |
| Web (Vite 8)     | \`npm run dev\` en \`Energy-Monitor-application\`                | Cliente en \`http://localhost:5173\`                             |
| Mobile (Expo)    | \`npm start\` en \`Energy-Monitor-Mobile\`                       | QR + Expo Go / web (datos mock)                                  |

## 7.2 Dónde están los secretos de la plataforma

Ningún secreto se guarda en el código ni en Git. El administrador debe ubicarlos únicamente en estos lugares:

- \`energy-monitor-backend/.env\` — contraseñas \`MYSQL_PASSWORD\`, \`MYSQL_ROOT_PASSWORD\`, \`SPRING_DATASOURCE_PASSWORD\`, credenciales \`MAIL\_\*\`, \`SECURITY_RESET_PEPPER\` y \`SECURITY_CORS_ALLOWED_ORIGINS\`.

- \`energy-monitor-backend/src/main/resources/application-local.yaml\` — perfil local: \`spring.datasource.url\`, \`security.jwt.\*\` (rutas a las claves) y \`security.reset.pepper\`.

- \`\$HOME/.energy-monitor/keys/\` — par \`private.pem\` / \`public.pem\` (permisos 700 en Linux/macOS; carpeta de usuario en Windows).

- \`Energy-Monitor-application/.env.local\` — \`VITE_API_BASE_URL\`.

> *Regla de oro: ningún secreto va a commits, a anexos del manual, a chats ni a directorios compartidos. Los archivos \`.env\`, \`application-local.yaml\` y \`\*.pem\` están en \`.gitignore\`.*

# 8. Modelo de seguridad rodeand la plataforma

## 8.1 Tablas que gobiernan la seguridad

| **Tabla**                                                         | **Propósito**                                                                                     |
|-------------------------------------------------------------------|---------------------------------------------------------------------------------------------------|
| user, person, user_configuration, user_session                    | Identidad, datos básicos, configuración propia y sesiones de cada cuenta.                         |
| password_policy                                                   | Política activa de contraseñas (por defecto min 8, max 64, 90 días, mayúsculas/números/símbolos). |
| password_reset_token                                              | Tokens (hash con pepper) de recuperación de contraseña.                                           |
| security_configuration                                            | Pares nombre/valor de parámetros de seguridad que administra el backend.                          |
| login_error_log                                                   | Intentos fallidos de acceso con tipo, descripción, IP y fecha.                                    |
| system_role, permission, user_system_role, system_role_permission | Modelo RBAC que define qué acción puede ejecutar cada rol.                                        |
| audit_log                                                         | Auditoría de CREATE/UPDATE/DELETE/LOGIN/LOGIN_FAILED/LOGOUT con IP y aplicación.                  |
| notification                                                      | Bitácora de intentos de correo (estado SENT/FAILED y motivo).                                     |

## 8.2 Auditoría (\`audit_log\`)

Toda operación relevante sobre cuentas, sesiones y credenciales debería dejar rastro en \`audit_log\`. El administrador puede consultar las últimas acciones así:

> SELECT id_audit_log, user_id, action, description, ip_address, created_at
>
> FROM audit_log
>
> ORDER BY created_at DESC
>
> LIMIT 50;

El campo \`action\` admite CREATE, UPDATE, DELETE, LOGIN, LOGIN_FAILED y LOGOUT (MySQL, 2026).

## 8.3 Intentos de acceso fallidos (\`login_error_log\`)

Los inicios de sesión denegados quedan registrados con motivo y dirección de origen. Sirve para detectar fuerza bruta:

> SELECT error_type, ip_address, COUNT(\*) AS intentos, MAX(created_at) AS ultimo
>
> FROM login_error_log
>
> GROUP BY error_type, ip_address
>
> ORDER BY intentos DESC
>
> LIMIT 20;

\`error_type\` puede ser \`INVALID_PASSWORD\`, \`USER_NOT_FOUND\`, \`ACCOUNT_BLOCKED\` o \`ACCOUNT_INACTIVE\`.

# 9. Roles y permisos (RBAC)

El modelo RBAC existe en la base de datos, pero en esta versión \*\*no hay endpoints HTTP ni interfaz web para administrarlo\*\*. La administración se realiza por SQL con el usuario de la aplicación (\`energy_app\`) o el root, desde un terminal seguro (Corte Parra y García Artunduaga, 2026).

## 9.1 Modelo

> system_role (id_system_role, name, description, enabled)
>
> permission (id_permission, code, name, description)
>
> system_role_permission (system_role_id, permission_id)
>
> user_system_role (user_id, system_role_id)

Ejemplo de alta de un rol y su asignación de un permiso concreto:

> INSERT INTO system_role (id_system_role, name, description, enabled) VALUES ('rol0000001', 'ADMIN', 'Administrador de la plataforma', 1);
>
> INSERT INTO permission (id_permission, code, name, description) VALUES ('prm0000001', 'METER.READ', 'Lectura de medidores', 'Permite ver dispositivos y mediciones');
>
> INSERT INTO system_role_permission (system_role_id, permission_id, assigned_at) VALUES ('rol0000001', 'prm0000001', NOW());
>
> INSERT INTO user_system_role (user_id, system_role_id, assigned_at) VALUES ('usr0000001', 'rol0000001', NOW());
>
> *Acuerdo de seguridad actual: aunque la tabla exista,ninguna interfaz pública renombra, crea ni asigna roles; todo acceso adicional debe pasar por revisión del equipo desarrollador antes de habilitarse vía HTTP.*

# 10. Política de contraseñas y configuración de seguridad

## 10.1 Política activa (\`password_policy\`)

La aplicación toma la política de la \*\*primera fila\*\* sin \`deleted_at\`. Por seguridad operativa, el esquema semilla una única fila por defecto (id \`pol0000001\`): mín. 8, máx. 64, expiración 90 días, mayúsculas, números y símbolos obligatorios. Para cambiarla:

> UPDATE password_policy SET min_length = 10, expiration_days = 60 WHERE id_password_policy = 'pol0000001';

No insertes una segunda fila activa: la selección no tiene \`ORDER BY\` y el comportamiento sería no determinista.

## 10.2 Parámetros dinámicos (\`security_configuration\`)

La tabla guarda pares \`config_name / config_value\` que el administrador puede ajustar sin recompilar. Edita valores existentes:

> SELECT id_security_configuration, config_name, config_value, description FROM security_configuration;
>
> UPDATE security_configuration SET config_value = 'valor' WHERE config_name = 'nombre_del_parametro';
>
> *Cambiar valores que el código usa para firmar/validar tokens puede invalidar sesiones: coordina con el equipo desarrollador antes de tocar esos parámetros.*

# 11. Sesiones, tokens y revocación

Desde la seguridad de la plataforma, toda revocación de sesión o de refresh token acepta como motivo \`ADMINISTRATIVE\`, \`PASSWORD_CHANGED\` o \`REFRESH_TOKEN_REUSE\` (MySQL, 2026). Para forzar el cierre de las sesiones de un usuario (p. ej. durante la atención de un incidente):

> UPDATE user_session SET revoked_at = NOW(), revoked_reason = 'ADMINISTRATIVE' WHERE user_id = 'usr0000001' AND revoked_at IS NULL;

El campo \`revoked_reason\` guarda la causa (\`ADMINISTRATIVE\`, \`PASSWORD_CHANGED\`, \`REFRESH_TOKEN_REUSE\`). Tras una revocación, el siguiente \`POST /api/v1/auth/refresh\` del cliente responderá \`401\`, obligando al cliente a autenticarse de nuevo.

# 12. Copias de seguridad y restauración

## 12.1 Backup del esquema (recomendado)

Con el contenedor de MySQL activo, un volcado completo de \`energy_monitor\` es:

> docker exec -t energy-monitor-mysql
>
> mysqldump -u root -p"\$env:MYSQL_ROOT_PASSWORD" --databases energy_monitor
>
> \| gzip \> energy_monitor\_\$(Get-Date -Format yyyyMMdd).sql.gz

## 12.2 Restaurar

> gzip -dc energy_monitor_20261007.sql.gz \|
>
> docker exec -i energy-monitor-mysql mysql -u root -p"\$env:MYSQL_ROOT_PASSWORD"

## 12.3 Volumen de datos

Los datos persistentes de MySQL viven en el volumen Docker declarado como \`mysql-data\`. Eliminarlo con \`docker compose down -v\` borra el esquema y, por ende, la base de datos; no lo hagas salvo que quieras borrar toda la información (Docker Inc., s. f.).

> *Nunca guardes los respaldos en la misma máquina que la base de datos; copia el SQL cifrado a un almacenamiento externo o a la nube con acceso restringido.*

# 13. Actualización del software

1.  Comunicar la ventana de mantenimiento al interesado y hacer un respaldo previo (sección 12.1).

2.  Detener el backend y los clientes: \`Ctrl+C\` en la terminal del backend; \`docker compose stop\` si vas a tocar la base de datos.

3.  \`git pull\` (o descargar el nuevo entregable) en los tres repositorios afectados.

4.  Revisar \`.env.example\` y \`application-local.yaml.example\` por nuevas variables; reproducir esos cambios en el \`.env\` y \`application-local.yaml\` reales sin compartir secretos.

5.  Levantar dependencias: \`docker compose up -d\` (Liquibase aplicará los changesets nuevos al arrancar el backend).

6.  Correr pruebas: \`.\mvnw.cmd test\` (backend) y \`npm test\` / \`npm run lint\` (web y móvil). Todas deben pasar antes de re-arrancar.

7.  Arrancar de nuevo: \`.\mvnw.cmd spring-boot:run\`, \`npm run dev\`, y validar el healthcheck \`/v3/api-docs\`.

> *Cambiar \`SECURITY_RESET_PEPPER\` invalida todos los códigos de recuperación vigentes. Rotarlo es una decisión de seguridad, no de costumbre.*

# 14. Verificación del correo de recuperación

Cada intento de envío queda en la tabla \`notification\` con estado \`SENT\` o \`FAILED\` y el motivo. Para auditar si el correo falla:

> SELECT id_notification, status, reason, created_at
>
> FROM notification
>
> WHERE status = 'FAILED'
>
> ORDER BY created_at DESC
>
> LIMIT 20;

Si casi todos fallan, suele deberse a credenciales SMTP incorrectas, a que Gmail exige una «app password» en lugar de la contraseña de la cuenta, o a que \`MAIL_FROM\` no coincide con \`MAIL_USERNAME\` (Google, s. f.). En desarrollo, comprueba la columna \`status\` y el motivo registrado y revisa que Mailpit esté activo (\`http://localhost:8025\`).

# 15. Incidentes frecuentes y cómo diagnosticarlos

| **Síntoma**                                                                        | **Posible causa**                                               | **Acción**                                                                                                    |
|------------------------------------------------------------------------------------|-----------------------------------------------------------------|---------------------------------------------------------------------------------------------------------------|
| El backend no arranca y nombra claves o pepper                                     | Faltan \`SECURITY_JWT\_\*\` o \`SECURITY_RESET_PEPPER\`.        | Definirlas en \`.env\`/\`application-local.yaml\` con rutas absolutas (ver manual de instalación, sección 9). |
| Tests o backend dan \`Access denied for user 'energy_app'\`                        | No está \`SPRING_DATASOURCE_PASSWORD\` en \`.env\`.             | Replicarla desde \`MYSQL_PASSWORD\` (sección 9.2 del manual de instalación).                                  |
| No llegan correos de recuperación                                                  | SMTP mal configurado o Mailpit apagado.                         | Revisar \`notification\` (sección 14) y \`MAIL\_\*\`.                                                         |
| El cliente web muestra errores CORS                                                | \`SECURITY_CORS_ALLOWED_ORIGINS\` no incluye el origen de Vite. | Agregarlo (sección 10.4 del manual de instalación).                                                           |
| Errores frecuentes \`INVALID_PASSWORD\`, \`USER_NOT_FOUND\` en \`login_error_log\` | Intentos de acceso inválidos o fuerza bruta.                    | Revisar IPs repetidas y bloquear en el firewall si hace falta.                                                |
| Sesiones que siguen válidas tras cambio de contraseña                              | Revocación no propagada por delante de reloj.                   | Revisar \`user_session.revoked_at\` y \`refresh_token.revoked_reason\`; repetir la revocación (sección 11).   |
| Docker / MySQL no levanta                                                          | Docker Desktop apagado o puerto 3306 ocupado.                   | Iniciar Docker Desktop; cambiar puerto en \`docker-compose.yml\` y re-crear contenedor.                       |

# 16. Comandos rápidos de administración

| **Tarea**                                   | **Comando**                                                                                                                                                  |
|---------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Ver estado de los contenedores              | \`docker compose ps\` (desde \`energy-monitor-backend\`)                                                                                                     |
| Ver últimas 10 acciones auditadas           | \`SELECT \* FROM audit_log ORDER BY created_at DESC LIMIT 10;\`                                                                                              |
| Ver últimos errores de login                | \`SELECT \* FROM login_error_log ORDER BY created_at DESC LIMIT 20;\`                                                                                        |
| Forzar cierre de sesiones de un usuario     | UPDATE \`user_session\` SET revoked_at/grace… (sección 11)                                                                                                   |
| Respaldo del esquema                        | \`mysqldump … \| gzip\` (sección 12.1)                                                                                                                       |
| Corregir permisos de clave privada          | \`icacls private.pem /inheritance:r /grant:r "%USERNAME%:(R)"\`                                                                                              |
| Consultar rol de un usuario                 | \`SELECT u.id_usuario, r.name FROM user u JOIN user_system_role usr ON usr.user_id=u.id_usuario JOIN system_role r ON r.id_system_role=usr.system_role_id;\` |
| Cerrar todos los contenedores de desarrollo | \`docker compose stop\` (backend) y \`Ctrl+C\` en Vite/Expo                                                                                                  |

# 17. Checklist diario / semanal del administrador

| **Revisión**                                             | **Frecuencia** | **Estado esperado**                                   |
|----------------------------------------------------------|----------------|-------------------------------------------------------|
| Contenedores sanos (\`docker compose ps\`)               | Diaria         | \`energy-monitor-mysql → healthy\`                    |
| Logs de la aplicación y Liquibase                        | Diaria         | Sin errores de contexto ni excepciones no controladas |
| \`audit_log\` y \`login_error_log\`                      | Diaria         | Sin ráfagas de \`LOGIN_FAILED\` ni picos por IP       |
| Notificaciones \`FAILED\`                                | Diaria         | Cola \`FAILED\` en cero o con motivo conocido         |
| Respaldo SQL cifrado                                     | Semanal        | Al menos un archivo reciente fuera del servidor       |
| Revisar \`.env\` y claves sin commitear                  | Semanal        | \`git status\` limpio; \`pem\` y \`.env\` ignorados   |
| Notificar al equipo dev si cambió la política o permisos | Por cambio     | Manual y pruebas del UseCaseModel actualizadas        |

# 18. Referencias (formato APA, 7.ª edición)

Las citas en el texto y la lista de referencias siguen el estilo de la séptima edición del manual de publicaciones de la APA (American Psychological Association \[APA\], 2020).

American Psychological Association. (2020). Publication manual of the American Psychological Association (7.ª ed.). https://www.apa.org/

Corte Parra, J. E. y García Artunduaga, M. A. (2026). Especificación de requisitos de software (SRS) del proyecto «Monitor de Consumo Eléctrico» \[Documento interno\]. SENA – CIES Regional Huila.

Docker Inc. (s. f.). Docker documentation. https://docs.docker.com/

Google. (s. f.). Cómo crear una contraseña de aplicación. https://myaccount.google.com/apppasswords

MySQL. (2026). MySQL 8.4 reference manual. https://dev.mysql.com/doc/refman/8.4/en/

Spring. (s. f.). Spring Boot reference documentation. https://docs.spring.io/spring-boot/index.html
