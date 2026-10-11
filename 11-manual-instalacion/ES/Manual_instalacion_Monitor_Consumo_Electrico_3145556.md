**SERVICIO NACIONAL DE APRENDIZAJE – SENA**

**CENTRO DE LA INDUSTRIA, LA EMPRESA Y LOS SERVICIOS (CIES)**

**REGIONAL HUILA**

Programa de Formación: Tecnología en Análisis y Desarrollo de Software (ADSO)

Ficha: 3145556

**MANUAL DE INSTALACIÓN**

**Proyecto: Monitor de Consumo Eléctrico**

*Procedimiento para desplegar la API, la aplicación web y la aplicación móvil en un equipo de trabajo*

| **Dato**             | **Información**                                                                 |
|----------------------|---------------------------------------------------------------------------------|
| Integrantes          | Juan Esteban Cortes Parra Miguel Angel Garcia Artunduaga                        |
| Instructor           | Motta Vargas José de Jesús                                                      |
| Centro de Formación  | Centro de la Industria, la Empresa y los Servicios (CIES) – SENA Regional Huila |
| Programa             | Tecnología en Análisis y Desarrollo de Software (ADSO)                          |
| Ficha                | 3145556                                                                         |
| Tipo de documento    | Manual de instalación                                                           |
| Versión              | 1.0                                                                             |
| Fecha de elaboración | 07 de octubre de 2026                                                           |

Año 2026

# 2. Control de versiones del documento

El historial de cambios del presente manual se registra en la siguiente tabla. Cada modificación del proceso de instalación que afecte el software debe verse reflejada en una nueva versión del documento.

| **Versión** | **Fecha**  | **Descripción del cambio**                                                                                                                     | **Responsable**      |
|-------------|------------|------------------------------------------------------------------------------------------------------------------------------------------------|----------------------|
| 1.0         | 07/10/2026 | Creación inicial del manual de instalación para backend, frontend web y frontend móvil, con requisitos de la SRS y referencias en formato APA. | Equipo desarrollador |

# 3. Tabla de contenido

[2. Control de versiones del documento [1](#control-de-versiones-del-documento)](#control-de-versiones-del-documento)

[3. Tabla de contenido [2](#tabla-de-contenido)](#tabla-de-contenido)

[4. Introducción [4](#introducción)](#introducción)

[5. Objetivo [4](#objetivo)](#objetivo)

[6. Alcance técnico [4](#alcance-técnico)](#alcance-técnico)

[7. Requisitos mínimos [5](#requisitos-mínimos)](#requisitos-mínimos)

[7.1 Requisitos del equipo (según la SRS) [5](#requisitos-del-equipo-según-la-srs)](#requisitos-del-equipo-según-la-srs)

[7.2 Recursos de hardware recomendados para ejecutar el sistema [5](#recursos-de-hardware-recomendados-para-ejecutar-el-sistema)](#recursos-de-hardware-recomendados-para-ejecutar-el-sistema)

[7.3 Requisitos de software para la instalación [5](#requisitos-de-software-para-la-instalación)](#requisitos-de-software-para-la-instalación)

[8. Variables de entorno que el instalador debe conocer [7](#variables-de-entorno-que-el-instalador-debe-conocer)](#variables-de-entorno-que-el-instalador-debe-conocer)

[9. Instalación del backend [7](#instalación-del-backend)](#instalación-del-backend)

[9.1 Obtener el código [7](#obtener-el-código)](#obtener-el-código)

[9.2 Definir credenciales de base de datos (.env) [7](#definir-credenciales-de-base-de-datos-.env)](#definir-credenciales-de-base-de-datos-.env)

[9.3 Levantar MySQL con Docker Compose [8](#levantar-mysql-con-docker-compose)](#levantar-mysql-con-docker-compose)

[9.4 Crear el perfil de aplicación local [8](#crear-el-perfil-de-aplicación-local)](#crear-el-perfil-de-aplicación-local)

[9.5 Generar el par de claves RSA (tokens JWT) [8](#generar-el-par-de-claves-rsa-tokens-jwt)](#generar-el-par-de-claves-rsa-tokens-jwt)

[9.6 Generar el pepper de recuperación [8](#generar-el-pepper-de-recuperación)](#generar-el-pepper-de-recuperación)

[9.7 Decidir el correo de recuperación [9](#decidir-el-correo-de-recuperación)](#decidir-el-correo-de-recuperación)

[9.8 Compilar y arrancar [9](#compilar-y-arrancar)](#compilar-y-arrancar)

[10. Instalación del frontend web [10](#instalación-del-frontend-web)](#instalación-del-frontend-web)

[10.1 Dependencias [10](#dependencias)](#dependencias)

[10.2 Configurar la URL del backend [10](#configurar-la-url-del-backend)](#configurar-la-url-del-backend)

[10.3 Arrancar en desarrollo [10](#arrancar-en-desarrollo)](#arrancar-en-desarrollo)

[10.4 Permiso de CORS en el backend [10](#permiso-de-cors-en-el-backend)](#permiso-de-cors-en-el-backend)

[11. Instalación de la aplicación móvil [10](#instalación-de-la-aplicación-móvil)](#instalación-de-la-aplicación-móvil)

[11.1 Dependencias [10](#dependencias-1)](#dependencias-1)

[11.2 Arrancar Expo [10](#arrancar-expo)](#arrancar-expo)

[11.3 Probar en un dispositivo físico [11](#probar-en-un-dispositivo-físico)](#probar-en-un-dispositivo-físico)

[11.4 Notas de compatibilidad [11](#notas-de-compatibilidad)](#notas-de-compatibilidad)

[12. Correo local con Mailpit (opcional) [11](#correo-local-con-mailpit-opcional)](#correo-local-con-mailpit-opcional)

[13. Verificación de la instalación [12](#verificación-de-la-instalación)](#verificación-de-la-instalación)

[14. Resumen de comandos de instalación [12](#resumen-de-comandos-de-instalación)](#resumen-de-comandos-de-instalación)

[15. Solución de problemas habituales [12](#solución-de-problemas-habituales)](#solución-de-problemas-habituales)

[16. Referencias (formato APA, 7.ª edición) [13](#referencias-formato-apa-7.ª-edición)](#referencias-formato-apa-7.ª-edición)

# 4. Introducción

El presente documento es el manual de instalación del software «Monitor de Consumo Eléctrico». Su propósito es que cualquier desarrollador, instructor o técnico —sin importar su experiencia previa con el proyecto— sea capaz de obtener el código fuente, configurar las dependencias, echar a andar los tres componentes del sistema y verificar que funcionen de forma conjunta en un equipo de trabajo.

El sistema se compone de tres piezas que se instalan por separado: una API REST (backend) construida con Java 21 y Spring Boot 4.1.1, una aplicación web construida con React 19 y Vite 8, y una aplicación móvil construida con Expo SDK 57 y React Native 0.86 (Spring, s. f.; Vite, s. f.; Expo, s. f.). La base de datos MySQL 8.4 se levanta con Docker Compose y la entrega del correo de recuperación puede simularse localmente con Mailpit (Docker Inc., s. f.; Mailpit, s. f.).

La información consignada fue obtenida directamente de los tres repositorios del proyecto, de los documentos de diseño (SRS, informe técnico) y de los README incluidos en cada repositorio, por lo que corresponde a la versión final del software entregado. Las referencias se formatearon según el estilo APA, séptima edición (American Psychological Association \[APA\], 2020).

# 5. Objetivo

Permitir que un equipo de desarrollo nuevo o de mantenimiento ponga en marcha el sistema «Monitor de Consumo Eléctrico» en un computador Windows, macOS o Linux, a partir de los repositorios oficiales, sin depender de documentación informal ni de conversaciones pasadas.

- Enumerar los requisitos mínimos de hardware, sistema operativo y software, tal como los define la SRS.

- Establecer versiones exactas de las herramientas que el proceso de instalación requiere (Java, Node, Docker, OpenSSL).

- Describir, paso a paso y con los comandos concretos, la instalación de la base de datos, el backend, la aplicación web y la móvil.

- Presentar las variables de configuración que el instalador debe definir antes del primer arranque.

- Indicar cómo verificar que cada componente quedó operativo y cómo diagnosticar los errores más habituales.

# 6. Alcance técnico

El manual cubre la instalación en un equipo de desarrollo local (no producción) de los siguientes componentes:

- Base de datos MySQL 8.4 mediante Docker Compose (energy-monitor-backend).

- API REST del backend (energy-monitor-backend) con perfil local y pruebas JUnit.

- Aplicación web (Energy-Monitor-application) con Vite.

- Aplicación móvil (Energy-Monitor-Mobile) con Expo y Expo Go.

- Utilidades opcionales: Mailpit para correo local y claves RSA para los JWT.

No cubre la puesta en marcha en un servidor de producción, la configuración de un proxy inverso, ni el despliegue en la nube; dichas tareas pertenecen al manual de administración y al informe técnico del proyecto.

# 7. Requisitos mínimos

## 7.1 Requisitos del equipo (según la SRS)

De acuerdo con la sección de interfaces de la SRS, el software se ejecuta en equipos de cómputo y dispositivos móviles en perfecto estado, con conexión a internet, almacenamiento suficiente para guardar la aplicación y la última versión disponible del sistema operativo (Corte Parra y García Artunduaga, 2026):

- Sistema operativo de computador: Windows 10 o superior.

- Sistema operativo móvil: la última versión disponible de Android.

- Navegador web actualizado: Chrome, Firefox, Edge, Safari, Brave, Vivaldi u otro equivalente.

- Conexión a internet (los servicios se comunican mediante protocolos estándar HTTP/HTTPS).

## 7.2 Recursos de hardware recomendados para ejecutar el sistema

Más allá de los requisitos del equipo indicados en la SRS, el equipo donde se instalen los tres componentes debe contar con los siguientes recursos mínimos para una experiencia sin cuelgues ni errores de memoria. Las versiones exactas de software se detallan en la tabla de la sección 7.3.

| **Recurso**               | **Mínimo**                                                     | **Recomendado**                                                                      |
|---------------------------|----------------------------------------------------------------|--------------------------------------------------------------------------------------|
| Sistema operativo (PC)    | Windows 10 de 64 bits (con Docker Desktop y WSL 2)             | Windows 11 de 64 bits o Linux (Ubuntu 24.04+)                                        |
| Sistema operativo (móvil) | Android 10 o superior                                          | Última versión de Android disponible                                                 |
| Procesador                | Intel Core i3 o equivalente                                    | Intel Core i5 o equivalente                                                          |
| Memoria RAM               | 8 GB                                                           | 16 GB (Docker Desktop, MySQL, Node y herramientas de diseño se abren con holgura)    |
| Espacio en disco libre    | 8 GB (código, node_modules, imagen MySQL y contenedores)       | 20 GB o más (para varias versiones, cachés de Docker y archivos de la base de datos) |
| Conectividad              | Internet estable (para clonar, descargar paquetes y Expo Go)   | Conexión de banda ancha                                                              |
| Almacenamiento adicional  | Al menos 5 GB libres para datos de prueba y respaldos de MySQL | Espacio a libre disposición                                                          |

> *Si el equipo cumple el mínimo pero no la recomendación, instala solo los componentes que necesites de forma simultánea (p. ej. ejecuta backend y web en terminal separado y no abras Android Studio a la vez).*

## 7.3 Requisitos de software para la instalación

Las versiones mínimas verificadas sobre el repositorio del backend y de los dos frontends son:

| **Herramienta** | **Versión mínima**         | **Para qué se usa**                                           |
|-----------------|----------------------------|---------------------------------------------------------------|
| Java JDK        | 21 o superior              | Compilar y ejecutar el backend (Spring Boot 4.1.1).           |
| Node.js         | 22 LTS (mín. 20.19)        | Tooling de React 19/Vite 8 y de Expo SDK 57.                  |
| npm             | incluido con Node          | Gestión de dependencias del web y del móvil.                  |
| Docker Desktop  | última estable             | Levantar MySQL 8.4 y Mailpit.                                 |
| Docker Compose  | incluido en Docker Desktop | Crear el esquema y el usuario de base de datos.               |
| OpenSSL         | cualquier versión reciente | Generar el par de claves RSA y el pepper del recovery.        |
| Git             | última estable             | Clonar los repositorios y, en Windows, trae OpenSSL embebido. |
| Expo Go         | app de la tienda           | Probar la app móvil sin emulador en un dispositivo físico.    |

Todas estas herramientas se distribuyen con documentación oficial en línea (Oracle, s. f.; Docker Inc., s. f.; Node.js Foundation, s. f.; Git, s. f.; OpenSSL, s. f.; Expo, s. f.).

# 8. Variables de entorno que el instalador debe conocer

El backend nunca guarda credenciales en el código: todo se inyecta por variables de entorno o por archivos ignorados por Git (\`.env\`, \`application-local.yaml\`). Estos son los nombres exactos que hay que definir (Spring, s. f.):

| **Variable / archivo**                                        | **Para qué sirve**                                                            | **Dónde ir anotada**                              |
|---------------------------------------------------------------|-------------------------------------------------------------------------------|---------------------------------------------------|
| MYSQL_PASSWORD / MYSQL_ROOT_PASSWORD                          | Contraseñas de la base de datos que crea Docker Compose.                      | \`.env\` (raíz de energy-monitor-backend)         |
| SPRING_DATASOURCE_PASSWORD                                    | Misma contraseña que MYSQL_PASSWORD; la usan las pruebas del perfil \`test\`. | \`.env\`                                          |
| MAIL_HOST, MAIL_PORT, MAIL_USERNAME, MAIL_PASSWORD, MAIL_FROM | SMTP para enviar el código de recuperación por correo.                        | \`.env\`                                          |
| SECURITY_RESET_PEPPER                                         | Pepper largo y aleatorio (HMAC) para el token de recuperación.                | \`.env\` o \`SECURITY_RESET_PEPPER\`              |
| SECURITY_CORS_ALLOWED_ORIGINS                                 | Orígenes permitidos para el navegador (p. ej. http://localhost:5173).         | \`.env\` o \`application-local.yaml\`             |
| SECURITY_JWT_PRIVATE_KEY_PATH / SECURITY_JWT_PUBLIC_KEY_PATH  | Rutas absolutas a las claves PEM del token JWT.                               | Variables de entorno o \`application-local.yaml\` |
| VITE_API_BASE_URL                                             | URL del backend para la app web (por defecto http://localhost:8080).          | \`.env.local\` en Energy-Monitor-application      |

# 9. Instalación del backend

Repositorio: \`energy-monitor-backend\` (Java 21, Spring Boot 4.1.1, Spring Modulith, MySQL 8.4, Liquibase, JWT RS256). La aplicación escucha en \`http://localhost:8080\` (Spring, s. f.).

## 9.1 Obtener el código

1.  Clonar o copiar la carpeta del repositorio en el equipo: git clone \<url\> o descomprimir el entregable.

2.  Verifica que contiene \`mvnw\`, \`pom.xml\`, \`docker-compose.yml\` y la carpeta \`src/\`.

## 9.2 Definir credenciales de base de datos (.env)

Copia el archivo de ejemplo y reemplaza cada \`CHANGE_ME\` por valores propios. El archivo \`.env\` está en \`.gitignore\` y no debe subirse al repositorio:

> cp .env.example .env
>
> \# En Windows PowerShell:
>
> Copy-Item .env.example .env

Valores mínimos a completar:

| **Variable**               | **Ejemplo (cámbialo)**         |
|----------------------------|--------------------------------|
| MYSQL_PASSWORD             | Un valor fuerte de tu elección |
| MYSQL_ROOT_PASSWORD        | Otro valor fuerte              |
| SPRING_DATASOURCE_PASSWORD | El mismo que MYSQL_PASSWORD    |

## 9.3 Levantar MySQL con Docker Compose

> docker compose up -d
>
> docker compose ps

Espera a que el contenedor \`energy-monitor-mysql\` reporte «healthy». Docker crea el esquema \`energy_monitor\` y el usuario \`energy_app\` con los valores de \`.env\` (Docker Inc., s. f.).

## 9.4 Crear el perfil de aplicación local

> cp src/main/resources/application-local.yaml.example src/main/resources/application-local.yaml
>
> \# Windows PowerShell:
>
> Copy-Item src\main\resources\application-local.yaml.example src\main\resources\application-local.yaml

Edita el nuevo archivo y coloca: la misma contraseña de \`MYSQL_PASSWORD\` en \`spring.datasource.password\`, las rutas absolutas de las dos claves PEM (sección 9.5) y el pepper (sección 9.6).

## 9.5 Generar el par de claves RSA (tokens JWT)

Los tokens de acceso se firman con RS256 y la aplicación necesita un par de claves. En Linux y macOS:

> mkdir -p ~/.energy-monitor/keys
>
> chmod 700 ~/.energy-monitor/keys
>
> openssl genpkey -algorithm RSA -out ~/.energy-monitor/keys/private.pem -pkeyopt rsa_keygen_bits:2048
>
> openssl rsa -in ~/.energy-monitor/keys/private.pem -pubout -out ~/.energy-monitor/keys/public.pem

En Windows (PowerShell o Git Bash):

> New-Item -ItemType Directory -Force "\$HOME\\energy-monitor\keys"
>
> openssl genpkey -algorithm RSA -out "\$HOME\\energy-monitor\keys\private.pem" -pkeyopt rsa_keygen_bits:2048
>
> openssl rsa -in "\$HOME\\energy-monitor\keys\private.pem" -pubout -out "\$HOME\\energy-monitor\keys\public.pem"

Indica las rutas en \`application-local.yaml\` (absolutas; Java no expande \`~\`):

> security:
>
> jwt:
>
> private-key-path: C:\Users\tu-usuario\\energy-monitor\keys\private.pem
>
> public-key-path: C:\Users\tu-usuario\\energy-monitor\keys\public.pem
>
> *Las claves \`.pem\` son secretos: nunca se commitean, ni se copian a un chat.*

## 9.6 Generar el pepper de recuperación

> openssl rand -hex 32

Copia el resultado en \`application-local.yaml\` bajo \`security.reset.pepper\`. Sin pepper la aplicación se niega a iniciar, del mismo modo que sin las claves RSA. Cambiarlo invalida los códigos de recuperación ya emitidos.

## 9.7 Decidir el correo de recuperación

Elige una de dos opciones: usar Mailpit en local (recomendado para desarrollo) o usar una cuenta Gmail real con una «app password» de 16 caracteres generada en la cuenta de Google (Google, s. f.). Para Mailpit usa \`MAIL_HOST=localhost\`, \`MAIL_PORT=587\`, \`MAIL_USERNAME=dev\`, \`MAIL_PASSWORD=dev\`; los mensajes se leen en \`http://localhost:8025\` (sección 11).

## 9.8 Compilar y arrancar

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

En Windows se usa el intérprete \`./mvnw.cmd\` (Node no instala Maven por separado; el repositorio incluye el wrapper). La API queda en \`http://localhost:8080\`; la documentación OpenAPI en \`/v3/api-docs\` y Swagger UI en \`/swagger-ui.html\` (Spring, s. f.).

> *Las pruebas no son herméticas: las de persistencia corren contra la base de datos real (Liquibase crea el esquema). Por eso la sección 9.3 debe estar sana antes de \`mvnw test\`.*

# 10. Instalación del frontend web

Repositorio: \`Energy-Monitor-application\` (React 19 + Vite 8). Dev server en \`http://localhost:5173\` (Vite, s. f.).

## 10.1 Dependencias

Con Node 22 y npm en el PATH, desde la carpeta del repositorio:

> npm install

## 10.2 Configurar la URL del backend

Copia el ejemplo y ajusta si el backend no usa el puerto por defecto:

> cp .env.example .env.local
>
> \# Windows PowerShell:
>
> Copy-Item .env.example .env.local
>
> VITE_API_BASE_URL=http://localhost:8080

## 10.3 Arrancar en desarrollo

> npm run dev

Abre \`http://localhost:5173\`. Para compilar para producción: \`npm run build\`; para revisar tipos/lint: \`npm run lint\`; para las pruebas: \`npm test\` (Vite, s. f.).

## 10.4 Permiso de CORS en el backend

El navegador solo permitirá llamadas del frontend si el backend la lista como origen permitido. Debe definirse en la misma configuración de instalación del backend (\`SECURITY_CORS_ALLOWED_ORIGINS\` en \`.env\` o \`application-local.yaml\`):

> SECURITY_CORS_ALLOWED_ORIGINS=http://localhost:5173,http://127.0.0.1:5173
>
> *Sin esa variable el navegador bloquea las peticiones aunque el backend responda bien. Opcional: \`SECURITY_CORS_EXPOSED_HEADERS=Retry-After\` para que el frontend lea la espera exacta del límite de intentos; si no, asume 15 minutos.*

# 11. Instalación de la aplicación móvil

Repositorio: \`Energy-Monitor-Mobile\` (Expo SDK 57, React Native 0.86.3, TypeScript). Actualmente opera con datos de demostración y no exige que el backend esté activo (Expo, s. f.).

## 11.1 Dependencias

> npm install

## 11.2 Arrancar Expo

> npm start
>
> \# o directamente:
>
> npx expo start

El CLI de Expo muestra un código QR y menús interactivos. Elige la plataforma:

- Android físico o emulador: \`npm run android\` (o presiona \`a\`).

- iOS (solo en macOS con Xcode): \`npm run ios\` (o presiona \`i\`).

- Navegador (útil en Windows): \`npm run web\` (o presiona \`w\`).

## 11.3 Probar en un dispositivo físico

Instala Expo Go desde la tienda de aplicaciones del teléfono, asegúrate de que el equipo y el móvil estén en la misma red, y escanea el QR con la cámara (Android) o con Expo Go (iOS). Es la vía recomendable en Windows porque no requiere Android Studio ni Xcode (Expo, s. f.).

## 11.4 Notas de compatibilidad

- Node 22 es el entorno de referencia para el tooling de Expo SDK 57.

- La app web de React Native usa \`react-native-web\`, por lo que \`npm run web\` sirve la vista en el navegador sin emulador.

- Si el QR no abre, comprueba el firewall o inicia con \`npx expo start --tunnel\`.

# 12. Correo local con Mailpit (opcional)

Mailpit es un servidor SMTP de desarrollo: acepta cualquier mensaje y los muestra en una bandeja web sin enviar nada por internet real. Se levanta con Docker (Mailpit, s. f.):

> docker run -d --name mailpit -p 587:1025 -p 8025:8025 \\
>
> -e MP_SMTP_AUTH_ACCEPT_ANY=1 -e MP_SMTP_AUTH_ALLOW_INSECURE=1 axllent/mailpit

Usa el puerto 587 del equipo (en lugar del 1025 por defecto de Mailpit) para que al alternar entre Mailpit y Gmail solo cambie \`MAIL_HOST\` y las credenciales. En \`.env\`:

> MAIL_HOST=localhost
>
> MAIL_PORT=587
>
> MAIL_USERNAME=dev
>
> MAIL_PASSWORD=dev
>
> MAIL_FROM=Energy Monitor \<no-reply@localhost\>

Bandeja de entrada del desarrollo: \`http://localhost:8025\`. Cuando uses Gmail, el código de recuperación llega al buzón real asociado a la cuenta con la «app password».

# 13. Verificación de la instalación

Ejecuta esta lista antes de dar por terminada la instalación. Cada comando debe producir el resultado indicado:

| **Componente** | **Comprobación**                                | **Resultado esperado**                                                |
|----------------|-------------------------------------------------|-----------------------------------------------------------------------|
| Docker         | \`docker compose ps\` en energy-monitor-backend | \`energy-monitor-mysql\` en estado \`healthy\`.                       |
| Backend        | Abrir \`http://localhost:8080/swagger-ui.html\` | Swagger UI lista la documentación de \`/api/v1\`.                     |
| OpenAPI        | \`GET http://localhost:8080/v3/api-docs\`       | JSON con la especificación de OpenAPI.                                |
| Web            | \`npm run dev\` en Energy-Monitor-application   | Vite en \`http://localhost:5173\` muestra la pantalla de acceso.      |
| Móvil          | \`npm start\` en Energy-Monitor-Mobile          | Expo muestra el QR y, al abrirlo con Expo Go, la app de demostración. |
| Correo         | \`MAIL_HOST=localhost\` + Mailpit en :587       | Los códigos de recuperación aparecen en \`http://localhost:8025\`.    |

Para una comprobación de punta a punta: registra un usuario desde la web, verifica su correo (Mailpit o Gmail), inicia sesión y confirma que el listado de hogares carga datos del backend (Corte Parra y García Artunduaga, 2026).

# 14. Resumen de comandos de instalación

| **Paso**      | **Backend (energy-monitor-backend)** | **Web (Energy-Monitor-application)** | **Móvil (Energy-Monitor-Mobile)** |
|---------------|--------------------------------------|--------------------------------------|-----------------------------------|
| Variables     | Copy-Item .env.example .env          | Copy-Item .env.example .env.local    | No aplica (datos mock)            |
| Deps.         | (wrapper Maven incluido)             | npm install                          | npm install                       |
| Base de datos | docker compose up -d                 | —                                    | —                                 |
| Claves/pepper | openssl … (sección 9.5, 9.6)         | —                                    | —                                 |
| Pruebas       | .\mvnw.cmd test                      | npm test                             | npm run lint                      |
| Ejecutar      | .\mvnw.cmd spring-boot:run           | npm run dev                          | npm start                         |
| URL típica    | http://localhost:8080                | http://localhost:5173                | QR / localhost:8081 (Expo)        |

# 15. Solución de problemas habituales

| **Síntoma**                                                 | **Causa probable**                                                                                      | **Acción correctiva**                                                                  |
|-------------------------------------------------------------|---------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------|
| El backend no arranca y nombra un path de clave o el pepper | Faltan \`SECURITY_JWT_PRIVATE_KEY_PATH\`, \`SECURITY_JWT_PUBLIC_KEY_PATH\` o \`SECURITY_RESET_PEPPER\`. | Definirlas en \`.env\` o en \`application-local.yaml\` (rutas absolutas) y reintentar. |
| \`Access denied for user 'energy_app'\` al correr tests     | No está \`SPRING_DATASOURCE_PASSWORD\` en \`.env\`.                                                     | Copiarla del valor de \`MYSQL_PASSWORD\` en \`.env\` (sección 9.2).                    |
| El navegador bloquea las llamadas desde la web              | Falta \`SECURITY_CORS_ALLOWED_ORIGINS\` con el origen de Vite.                                          | Agregarlo en \`.env\` del backend (sección 10.4).                                      |
| No llegan los códigos de recuperación                       | Correo mal configurado; el error queda en la tabla \`notification\`.                                    | Revisar \`MAIL\_\*\`, usar Mailpit y leer la bandeja en \`:8025\` (sección 12).        |
| Mailpit/Docker no arranca                                   | Docker Desktop detenido o puerto ocupado.                                                               | Iniciar Docker Desktop; cambiar puerto con \`-p 1587:1025\` si 587 está en uso.        |
| Illinois: "~" no se expande en la ruta de claves            | Java no expande el símbolo en \`private-key-path\`.                                                     | Usar la ruta absoluta completa (\`C:\Users\\\`).                                       |
| Expo no conecta con Expo Go                                 | Distinta red, firewall, o QR inválido.                                                                  | Usar \`npx expo start --tunnel\`; verificar scripts de red corporativa.                |
| \`npm install\` falla en móvil/web                          | Versión de Node distinta a la del software de referencia.                                               | Actualizar a Node 22 LTS (mín. 20.19) y reinstalar.                                    |

# 16. Referencias (formato APA, 7.ª edición)

Las citas en el texto y la lista de referencias siguen el estilo de la séptima edición del manual de publicaciones de la APA (American Psychological Association \[APA\], 2020).

American Psychological Association. (2020). Publication manual of the American Psychological Association (7.ª ed.). https://www.apa.org/

Corte Parra, J. E. y García Artunduaga, M. A. (2026). Especificación de requisitos de software (SRS) del proyecto «Monitor de Consumo Eléctrico» \[Documento interno\]. SENA – CIES Regional Huila.

Docker Inc. (s. f.). Docker documentation. https://docs.docker.com/

Expo. (s. f.). Expo documentation. https://docs.expo.dev/

Git. (s. f.). Git for Windows. https://git-scm.com/download/win

Google. (s. f.). Cómo crear una contraseña de aplicación. https://myaccount.google.com/apppasswords

Mailpit. (s. f.). Mailpit: Web and API based SMTP testing. https://mailpit.axllent.org/

MySQL. (2026). MySQL 8.4 reference manual. https://dev.mysql.com/doc/refman/8.4/en/

Node.js Foundation. (s. f.). Node.js. https://nodejs.org/

OpenSSL. (s. f.). OpenSSL cryptography and SSL/TLS toolkit. https://www.openssl.org/

Oracle. (s. f.). Java SE downloads. https://www.oracle.com/java/technologies/downloads/

Spring. (s. f.). Spring Boot reference documentation. https://docs.spring.io/spring-boot/index.html

Vite. (s. f.). Vite guide. https://vite.dev/guide/
