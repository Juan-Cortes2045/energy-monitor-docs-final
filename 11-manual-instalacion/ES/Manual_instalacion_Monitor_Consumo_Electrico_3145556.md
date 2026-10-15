**SERVICIO NACIONAL DE APRENDIZAJE – SENA**

**CENTRO DE LA INDUSTRIA, LA EMPRESA Y LOS SERVICIOS (CIES)**

**REGIONAL HUILA**

Programa de Formación: Tecnología en Análisis y Desarrollo de Software (ADSO)

Ficha: 3145556

**MANUAL DE INSTALACIÓN**

**Proyecto: Monitor de Consumo Eléctrico**

*Procedimiento para desplegar la API, el broker MQTT, el dispositivo de medición, la aplicación web y la aplicación móvil*

| **Dato**             | **Información**                                                                 |
|----------------------|---------------------------------------------------------------------------------|
| Integrantes          | Juan Esteban Cortés Parra, Miguel Ángel García Artunduaga                       |
| Instructor           | `[POR CONFIRMAR]`                                                               |
| Centro de Formación  | Centro de la Industria, la Empresa y los Servicios (CIES) – SENA Regional Huila |
| Programa             | Tecnología en Análisis y Desarrollo de Software (ADSO)                          |
| Ficha                | 3145556                                                                         |
| Tipo de documento    | Manual de instalación                                                           |
| Versión              | 1.1                                                                             |
| Fecha de elaboración | 07 de octubre de 2026 (actualización: 13 de octubre de 2026)                    |

Año 2026

# 2. Control de versiones del documento

El historial de cambios del presente manual se registra en la siguiente tabla. Cada modificación del proceso de instalación que afecte el software debe verse reflejada en una nueva versión del documento.

| **Versión** | **Fecha**  | **Descripción del cambio**                                                                                                                                                                                                                        | **Responsable**      |
|-------------|------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------|
| 1.0         | 07/10/2026 | Creación inicial del manual de instalación para backend, frontend web y frontend móvil, con requisitos de la especificación de requisitos y referencias en formato APA.                                                                           | Equipo desarrollador |
| 1.1         | 13/10/2026 | Actualización al estado final: cinco piezas del sistema, variables de entorno completas, broker MQTT, firmware, vinculación del primer dispositivo, producción, aplicación móvil integrada con la API y verificaciones y soluciones ampliadas.   | Equipo desarrollador |

# 3. Tabla de contenido

[2. Control de versiones del documento](#2-control-de-versiones-del-documento)

[3. Tabla de contenido](#3-tabla-de-contenido)

[4. Introducción](#4-introducción)

[5. Objetivo](#5-objetivo)

[6. Alcance técnico](#6-alcance-técnico)

[7. Requisitos mínimos](#7-requisitos-mínimos)

- [7.1 Requisitos del equipo (según la especificación de requisitos)](#71-requisitos-del-equipo-según-la-especificación-de-requisitos)
- [7.2 Recursos de hardware recomendados para ejecutar el sistema](#72-recursos-de-hardware-recomendados-para-ejecutar-el-sistema)
- [7.3 Requisitos de software para la instalación](#73-requisitos-de-software-para-la-instalación)

[8. Variables de entorno que el instalador debe conocer](#8-variables-de-entorno-que-el-instalador-debe-conocer)

[9. Instalación del backend](#9-instalación-del-backend)

- [9.1 Obtener el código](#91-obtener-el-código) · [9.2 Definir credenciales de base de datos (.env)](#92-definir-credenciales-de-base-de-datos-env) · [9.3 Levantar MySQL con Docker Compose](#93-levantar-mysql-con-docker-compose) · [9.4 Crear el perfil de aplicación local](#94-crear-el-perfil-de-aplicación-local) · [9.5 Generar el par de claves RSA (tokens JWT)](#95-generar-el-par-de-claves-rsa-tokens-jwt) · [9.6 Generar el pepper de recuperación](#96-generar-el-pepper-de-recuperación) · [9.7 Decidir el correo de recuperación](#97-decidir-el-correo-de-recuperación) · [9.8 Compilar y arrancar](#98-compilar-y-arrancar)

[10. Instalación del frontend web](#10-instalación-del-frontend-web)

- [10.1 Dependencias](#101-dependencias) · [10.2 Configurar la URL del backend](#102-configurar-la-url-del-backend) · [10.3 Arrancar en desarrollo](#103-arrancar-en-desarrollo) · [10.4 Permiso de CORS en el backend](#104-permiso-de-cors-en-el-backend)

[11. Instalación de la aplicación móvil](#11-instalación-de-la-aplicación-móvil)

- [11.1 Dependencias](#111-dependencias) · [11.2 Configurar la URL del backend](#112-configurar-la-url-del-backend) · [11.3 Arrancar Expo](#113-arrancar-expo) · [11.4 Probar en un dispositivo físico](#114-probar-en-un-dispositivo-físico) · [11.5 Notas de compatibilidad](#115-notas-de-compatibilidad)

[12. Broker MQTT](#12-broker-mqtt)

[13. Firmware del dispositivo de medición](#13-firmware-del-dispositivo-de-medición)

[14. Vincular el primer dispositivo](#14-vincular-el-primer-dispositivo)

[15. Producción](#15-producción)

[16. Correo local con Mailpit (opcional)](#16-correo-local-con-mailpit-opcional)

[17. Verificación de la instalación](#17-verificación-de-la-instalación)

[18. Resumen de comandos de instalación](#18-resumen-de-comandos-de-instalación)

[19. Solución de problemas habituales](#19-solución-de-problemas-habituales)

[20. Referencias (formato APA, 7.ª edición)](#20-referencias-formato-apa-7ª-edición)

# 4. Introducción

El presente documento es el manual de instalación del software «Monitor de Consumo Eléctrico». Su propósito es que cualquier desarrollador, instructor o técnico —sin importar su experiencia previa con el proyecto— sea capaz de obtener el código fuente, configurar las dependencias, echar a andar los cinco componentes del sistema y verificar que funcionen de forma conjunta en un equipo de trabajo.

El sistema se compone de cinco piezas que se instalan por separado: una API REST (backend) construida con Java 21 y Spring Boot 4.1.1, una aplicación web construida con React 19 y Vite 8, una aplicación móvil Android construida con Expo SDK 57 y React Native 0.86, un broker MQTT Eclipse Mosquitto con autenticación contra el backend y un dispositivo de medición formado por una placa ESP32 y un sensor PZEM-004T V3 (Spring, s. f.; Vite, s. f.; Expo, s. f.; Eclipse Foundation, s. f.). La base de datos MySQL 8.4 se levanta con Docker Compose y la entrega del correo puede simularse localmente con Mailpit (Docker Inc., s. f.; Mailpit, s. f.).

La información consignada fue obtenida directamente de los repositorios del proyecto (backend, web, móvil y dispositivos), de la especificación de requisitos y de los README incluidos en cada repositorio, por lo que corresponde a la versión final del software entregado. Las referencias se formatearon según el estilo APA, séptima edición (American Psychological Association \[APA\], 2020).

# 5. Objetivo

Permitir que un equipo de desarrollo nuevo o de mantenimiento ponga en marcha el sistema «Monitor de Consumo Eléctrico» en un computador Windows, macOS o Linux, a partir de los repositorios oficiales, sin depender de documentación informal ni de conversaciones pasadas.

- Enumerar los requisitos mínimos de hardware, sistema operativo y software, en coherencia con la especificación de requisitos.

- Establecer versiones exactas de las herramientas que el proceso de instalación requiere (Java, Node, Docker, OpenSSL, Arduino).

- Describir, paso a paso y con los comandos concretos, la instalación de la base de datos, el backend, la aplicación web, la móvil, el broker MQTT y el firmware del dispositivo.

- Presentar las variables de configuración que el instalador debe definir antes del primer arranque.

- Indicar cómo vincular el primer dispositivo, cómo desplegar en producción, cómo verificar que cada componente quedó operativo y cómo diagnosticar los errores más habituales.

# 6. Alcance técnico

El manual cubre la instalación en un equipo de desarrollo local de los siguientes componentes y, en la sección 15, el despliegue en un servidor de producción con Docker Compose:

- Base de datos MySQL 8.4 mediante Docker Compose (energy-monitor-backend).

- API REST del backend (energy-monitor-backend) con perfil local y pruebas JUnit.

- Aplicación web (Energy-Monitor-application) con Vite.

- Aplicación móvil Android (Energy-Monitor-Mobile) con Expo y Expo Go.

- Broker MQTT Mosquitto y firmware del dispositivo (devices, repositorio energy-monitor-devices).

- Utilidades opcionales: Mailpit para correo local y claves RSA para los JWT.

No cubre la contratación de un servidor ni de un dominio, ni la configuración detallada de un proxy inverso; el ajuste del proxy se limita a las reglas indicadas en la sección 15.

# 7. Requisitos mínimos

## 7.1 Requisitos del equipo (según la especificación de requisitos)

De acuerdo con los requisitos de compatibilidad de la especificación de requisitos (RNF-COM), el software se ejecuta en equipos de cómputo y dispositivos móviles en perfecto estado, con conexión a internet, almacenamiento suficiente para guardar la aplicación y la última versión disponible del sistema operativo (Cortés Parra y García Artunduaga, 2026):

- Sistema operativo de computador: Windows 10 o superior.

- Sistema operativo móvil: la última versión disponible de Android. La aplicación móvil no está disponible para iPhone.

- Navegador web actualizado: Chrome, Firefox, Safari o Edge. La vinculación del dispositivo por Bluetooth solo está disponible en Chrome o Edge de escritorio y en Chrome para Android, y exige una conexión segura (HTTPS o `localhost`).

- Conexión a internet (los servicios se comunican mediante protocolos estándar HTTP/HTTPS).

- Para el dispositivo de medición: una placa ESP32 DevKit con Wi-Fi y Bluetooth, un sensor PZEM-004T V3, una fuente HLK-PM03 y un cable USB.

## 7.2 Recursos de hardware recomendados para ejecutar el sistema

Más allá de los requisitos del equipo indicados en la especificación de requisitos, el equipo donde se instalen los componentes debe contar con los siguientes recursos mínimos para una experiencia sin cuelgues ni errores de memoria. Las versiones exactas de software se detallan en la tabla de la sección 7.3.

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

Las versiones mínimas verificadas sobre el repositorio del backend, de los dos frontends y de los dispositivos son:

| **Herramienta**                | **Versión mínima**                   | **Para qué se usa**                                                          |
|--------------------------------|--------------------------------------|------------------------------------------------------------------------------|
| Java JDK                       | 21 o superior                        | Compilar y ejecutar el backend (Spring Boot 4.1.1).                          |
| Node.js                        | 22 LTS (mín. 20.19)                  | Tooling de React 19/Vite 8 y de Expo SDK 57.                                 |
| npm                            | incluido con Node                    | Gestión de dependencias del web y del móvil.                                 |
| Docker Desktop                 | última estable                       | Levantar MySQL 8.4, el broker Mosquitto y Mailpit.                           |
| Docker Compose                 | v2, incluido en Docker Desktop       | Crear el esquema y el usuario de base de datos y levantar el broker.         |
| OpenSSL y bash                 | cualquier versión reciente           | Generar el par de claves RSA, el pepper y los certificados de desarrollo.    |
| Git                            | última estable                       | Clonar los repositorios y, en Windows, trae OpenSSL embebido.                |
| Expo Go                        | app de la tienda                     | Probar la app móvil sin emulador en un dispositivo físico.                   |
| arduino-cli o Arduino IDE 2    | última estable                       | Compilar y cargar el firmware del dispositivo (núcleo esp32 de Espressif 3.x).|

Todas estas herramientas se distribuyen con documentación oficial en línea (Oracle, s. f.; Docker Inc., s. f.; Node.js Foundation, s. f.; Git, s. f.; OpenSSL, s. f.; Expo, s. f.; Arduino, s. f.).

# 8. Variables de entorno que el instalador debe conocer

El backend nunca guarda credenciales en el código: todo se inyecta por variables de entorno o por archivos ignorados por Git (`.env`, `application-local.yaml`). Los nombres que siguen son los exactos de `.env.example` del backend, de la web y de la aplicación móvil, y de `application.yaml` cuando se indica (Spring, s. f.).

> *El archivo `.env` se lee como `.properties`: escribe los valores sin comillas y sin `$`. Un valor con `$` se interpreta como un marcador y la aplicación no arranca.*

| **Variable / archivo**                                         | **Para qué sirve**                                                                                                                | **Dónde anotarla**                                |
|----------------------------------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------|---------------------------------------------------|
| MYSQL_PASSWORD / MYSQL_ROOT_PASSWORD                           | Contraseñas de la base de datos que crea Docker Compose.                                                                          | `.env` (raíz de energy-monitor-backend)           |
| SPRING_DATASOURCE_PASSWORD                                     | Misma contraseña que MYSQL_PASSWORD; la usan las pruebas del perfil `test`.                                                       | `.env`                                            |
| MAIL_HOST, MAIL_PORT, MAIL_USERNAME, MAIL_PASSWORD, MAIL_FROM  | SMTP para enviar los códigos de verificación y recuperación y los correos de alerta.                                              | `.env`                                            |
| SECURITY_RESET_PEPPER                                          | Pepper largo y aleatorio (HMAC) para el código de recuperación.                                                                   | `.env`                                            |
| SECURITY_CORS_ALLOWED_ORIGINS                                  | Orígenes permitidos para el navegador (p. ej. http://localhost:5173).                                                             | `.env` o `application-local.yaml`                 |
| SECURITY_CORS_EXPOSED_HEADERS                                  | Cabeceras que el navegador puede leer. Por defecto `Allow` y `Retry-After`.                                                       | Variable de entorno (opcional)                    |
| SECURITY_JWT_PRIVATE_KEY_PATH / SECURITY_JWT_PUBLIC_KEY_PATH   | Rutas absolutas a las claves PEM del token JWT.                                                                                   | Variables de entorno o `application-local.yaml`   |
| GOOGLE_CLIENT_ID / GOOGLE_CLIENT_SECRET                        | Credenciales OAuth («Aplicación web») para iniciar sesión con Google. Si están vacías, el botón de Google responde `503`.         | `.env`                                            |
| MQTT_BROKER_URL                                                | Dirección del listener interno del broker (`tcp://localhost:1883`).                                                               | `.env`                                            |
| MQTT_BACKEND_USER / MQTT_BACKEND_PASS                          | Credenciales del backend en el broker. Cualquier valor aleatorio sirve (`openssl rand -hex 24`).                                  | `.env`                                            |
| MQTT_CLIENT_ID                                                 | Identificador del backend en el broker; debe ser distinto en cada instancia.                                                      | `.env`                                            |
| MQTT_AUTH_ALLOWED_NETWORKS                                     | Redes desde las que se puede llamar a `/internal/mqtt/auth/**` (por defecto `127.0.0.1/32,::1/128,172.16.0.0/12`).               | Variable de entorno (opcional)                    |
| DEVICE_MQTT_PUBLIC_HOST / DEVICE_MQTT_PUBLIC_PORT              | Dirección del broker que la web escribe en el dispositivo al vincularlo. Nunca `localhost`: en desarrollo, la IP del PC en la red local; en producción, el dominio del broker. Puerto `8883`. | `.env`                  |
| DEVICE_OFFLINE_AFTER                                           | Tiempo de silencio tras el cual un dispositivo se marca sin conexión (por defecto `PT150S`).                                      | Variable de entorno (opcional)                    |
| NOTIFICATION_WEB_URL                                           | Dirección de la web que usan los enlaces de los correos de alerta (`http://localhost:5173`).                                      | `.env`                                            |
| NOTIFICATION_VAPID_PUBLIC_KEY / NOTIFICATION_VAPID_PRIVATE_KEY | Par de claves VAPID de las notificaciones push del navegador. Sin ellas se desactiva el push y el correo sigue funcionando.      | `.env`                                            |
| NOTIFICATION_VAPID_SUBJECT                                     | Contacto asociado a las claves VAPID (por defecto un `mailto:` de soporte).                                                       | Variable de entorno (opcional)                    |
| ALERT_LIMIT_ZONE                                               | Zona horaria en la que empiezan el día y el mes de las alertas de límite (por defecto `America/Bogota`).                          | Variable de entorno (opcional)                    |
| RECOMMENDATION_EVALUATE_INTERVAL / RECOMMENDATION_ZONE         | Cada cuánto se evalúan las reglas de ahorro (`PT1H`) y la zona horaria de las reglas por franja del día (`America/Bogota`).       | Variables de entorno (opcionales)                 |
| VITE_API_BASE_URL                                              | URL del backend para la app web (por defecto http://localhost:8080).                                                              | `.env.local` en Energy-Monitor-application        |
| VITE_GOOGLE_CLIENT_ID                                          | El mismo ID de cliente de GOOGLE_CLIENT_ID. Vacío: el botón de Google no se muestra.                                              | `.env.local` en Energy-Monitor-application        |
| EXPO_PUBLIC_API_BASE_URL                                       | URL del backend para la app móvil, sin `/api/v1` (sección 11.2).                                                                  | `.env` en Energy-Monitor-Mobile                   |

# 9. Instalación del backend

Repositorio: `energy-monitor-backend` (Java 21, Spring Boot 4.1.1, Spring Modulith, MySQL 8.4, Liquibase, JWT RS256). La aplicación escucha en `http://localhost:8080` (Spring, s. f.).

## 9.1 Obtener el código

1.  Clonar o copiar la carpeta del repositorio en el equipo: git clone \<url\> o descomprimir el entregable.

2.  Verifica que contiene `mvnw`, `pom.xml`, `docker-compose.yml` y la carpeta `src/`.

## 9.2 Definir credenciales de base de datos (.env)

Copia el archivo de ejemplo y reemplaza cada `CHANGE_ME` por valores propios. El archivo `.env` está en `.gitignore` y no debe subirse al repositorio:

```bash
cp .env.example .env
# En Windows PowerShell:
Copy-Item .env.example .env
```

Valores mínimos a completar:

| **Variable**               | **Ejemplo (cámbialo)**         |
|----------------------------|--------------------------------|
| MYSQL_PASSWORD             | Un valor fuerte de tu elección |
| MYSQL_ROOT_PASSWORD        | Otro valor fuerte              |
| SPRING_DATASOURCE_PASSWORD | El mismo que MYSQL_PASSWORD    |

El resto de las variables (broker, dispositivos, notificaciones, Google) se describen en la sección 8 y se completan al instalar cada pieza.

## 9.3 Levantar MySQL con Docker Compose

```bash
docker compose up -d
docker compose ps
```

Espera a que el contenedor `energy-monitor-mysql` reporte «healthy». Docker crea el esquema `energy_monitor` y el usuario `energy_app` con los valores de `.env` (Docker Inc., s. f.).

## 9.4 Crear el perfil de aplicación local

```bash
cp src/main/resources/application-local.yaml.example src/main/resources/application-local.yaml
# Windows PowerShell:
Copy-Item src\main\resources\application-local.yaml.example src\main\resources\application-local.yaml
```

Edita el nuevo archivo y coloca: la misma contraseña de `MYSQL_PASSWORD` en `spring.datasource.password`, las rutas absolutas de las dos claves PEM (sección 9.5) y el pepper (sección 9.6).

## 9.5 Generar el par de claves RSA (tokens JWT)

Los tokens de acceso se firman con RS256 y la aplicación necesita un par de claves. En Linux y macOS:

```bash
mkdir -p ~/.energy-monitor/keys
chmod 700 ~/.energy-monitor/keys
openssl genpkey -algorithm RSA -out ~/.energy-monitor/keys/private.pem -pkeyopt rsa_keygen_bits:2048
openssl rsa -in ~/.energy-monitor/keys/private.pem -pubout -out ~/.energy-monitor/keys/public.pem
```

En Windows (PowerShell o Git Bash), con la misma carpeta oculta `.energy-monitor`:

```powershell
New-Item -ItemType Directory -Force "$HOME\.energy-monitor\keys"
openssl genpkey -algorithm RSA -out "$HOME\.energy-monitor\keys\private.pem" -pkeyopt rsa_keygen_bits:2048
openssl rsa -in "$HOME\.energy-monitor\keys\private.pem" -pubout -out "$HOME\.energy-monitor\keys\public.pem"
```

Indica las rutas en `application-local.yaml` (absolutas; Java no expande `~`):

```yaml
security:
  jwt:
    private-key-path: C:\Users\tu-usuario\.energy-monitor\keys\private.pem
    public-key-path: C:\Users\tu-usuario\.energy-monitor\keys\public.pem
```

> *Las claves `.pem` son secretos: nunca se commitean, ni se copian a un chat.*

## 9.6 Generar el pepper de recuperación

```bash
openssl rand -hex 32
```

Copia el resultado en `application-local.yaml` bajo `security.reset.pepper`. Sin pepper la aplicación se niega a iniciar, del mismo modo que sin las claves RSA. Cambiarlo invalida los códigos de recuperación ya emitidos.

## 9.7 Decidir el correo de recuperación

Elige una de dos opciones: usar Mailpit en local (recomendado para desarrollo) o usar una cuenta Gmail real con una «app password» de 16 caracteres generada en la cuenta de Google (Google, s. f.). Para Mailpit usa `MAIL_HOST=localhost`, `MAIL_PORT=587`, `MAIL_USERNAME=dev`, `MAIL_PASSWORD=dev`; los mensajes se leen en `http://localhost:8025` (sección 16).

## 9.8 Compilar y arrancar

```bash
./mvnw clean test
./mvnw spring-boot:run
# Windows:
.\mvnw.cmd clean test
.\mvnw.cmd spring-boot:run
```

En Windows se usa `mvnw.cmd`. El repositorio incluye el Maven Wrapper, por lo que no hace falta instalar Maven por separado (Node.js solo se necesita para los frontends y no instala Maven). La API queda en `http://localhost:8080`; la documentación OpenAPI en `/v3/api-docs` y Swagger UI en `/swagger-ui.html` (Spring, s. f.). El estado de salud está en `/actuator/health`.

> *Las pruebas no son herméticas: las de persistencia corren contra la base de datos real (Liquibase crea el esquema). Por eso la sección 9.3 debe estar sana antes de `mvnw test`.*

# 10. Instalación del frontend web

Repositorio: `Energy-Monitor-application` (React 19 + Vite 8). Dev server en `http://localhost:5173` (Vite, s. f.).

## 10.1 Dependencias

Con Node 22 y npm en el PATH, desde la carpeta del repositorio:

```bash
npm install
```

## 10.2 Configurar la URL del backend

Copia el ejemplo y ajusta si el backend no usa el puerto por defecto:

```bash
cp .env.example .env.local
# Windows PowerShell:
Copy-Item .env.example .env.local
```

```
VITE_API_BASE_URL=http://localhost:8080
VITE_GOOGLE_CLIENT_ID=
```

`VITE_GOOGLE_CLIENT_ID` debe ser el mismo ID de cliente que `GOOGLE_CLIENT_ID` del backend. Si queda vacío, el botón de Google no se muestra.

## 10.3 Arrancar en desarrollo

```bash
npm run dev
```

Abre `http://localhost:5173`. Para compilar para producción: `npm run build`; para revisar el código: `npm run lint`; para las pruebas: `npm test` (Vite, s. f.).

## 10.4 Permiso de CORS en el backend

El navegador solo permitirá llamadas del frontend si el backend lo lista como origen permitido. Debe definirse en la misma configuración de instalación del backend (`SECURITY_CORS_ALLOWED_ORIGINS` en `.env` o `application-local.yaml`):

```
SECURITY_CORS_ALLOWED_ORIGINS=http://localhost:5173,http://127.0.0.1:5173
```

> *Sin esa variable el navegador bloquea las peticiones aunque el backend responda bien. El backend ya expone por defecto las cabeceras `Allow` y `Retry-After`, de modo que el frontend lee la espera exacta del límite de intentos; solo hace falta `SECURITY_CORS_EXPOSED_HEADERS` si se quiere cambiar esa lista.*

# 11. Instalación de la aplicación móvil

Repositorio: `Energy-Monitor-Mobile` (Expo SDK 57, React Native 0.86.3, TypeScript, solo Android). La aplicación está integrada con la API del backend, por lo que el backend debe estar en ejecución para iniciar sesión y consultar datos (Expo, s. f.).

## 11.1 Dependencias

```bash
npm install
```

## 11.2 Configurar la URL del backend

Copia el ejemplo y define la dirección del backend, sin `/api/v1`:

```bash
cp .env.example .env
# Windows PowerShell:
Copy-Item .env.example .env
```

```
EXPO_PUBLIC_API_BASE_URL=http://10.0.2.2:8080
```

El valor depende de dónde se ejecute la aplicación:

| **Dónde corre la app** | **Valor de EXPO_PUBLIC_API_BASE_URL**                                  |
|------------------------|------------------------------------------------------------------------|
| Emulador de Android    | `http://10.0.2.2:8080` (`localhost` sería el propio emulador)          |
| Teléfono físico        | `http://<IP del PC en la red local>:8080`, por ejemplo `http://192.168.1.50:8080` |
| Navegador (`npm run web`) | `http://localhost:8080`                                             |

Expo incorpora las variables `EXPO_PUBLIC_*` al compilar: tras cambiar el valor, reinicia `npx expo start`. CORS no aplica a React Native, por lo que no hay que agregar la app a `SECURITY_CORS_ALLOWED_ORIGINS`.

## 11.3 Arrancar Expo

```bash
npm start
# o directamente:
npx expo start
```

El CLI de Expo muestra un código QR y menús interactivos. Elige la plataforma:

- Android físico o emulador: `npm run android` (o presiona `a`).

- Navegador, solo como apoyo en desarrollo: `npm run web` (o presiona `w`).

## 11.4 Probar en un dispositivo físico

Instala Expo Go desde Google Play, asegúrate de que el equipo y el móvil estén en la misma red, y escanea el QR desde Expo Go. Es la vía recomendable en Windows porque no requiere Android Studio (Expo, s. f.). Para que el teléfono llegue al backend, permite el puerto 8080 en el firewall del equipo.

## 11.5 Notas de compatibilidad

- Node 22 es el entorno de referencia para el tooling de Expo SDK 57.

- La aplicación móvil no está disponible para iPhone: el manual no cubre iOS ni Xcode.

- Si el QR no abre, comprueba el firewall o inicia con `npx expo start --tunnel`.

# 12. Broker MQTT

El broker es Eclipse Mosquitto con el complemento `mosquitto-go-auth`: cada dispositivo se autentica contra el backend y solo puede publicar en sus dos tópicos. Se levanta desde la carpeta `devices/docker/` del repositorio de dispositivos, cuyo `docker-compose.yml` de desarrollo inicia únicamente el broker; el backend se ejecuta en el equipo y el contenedor lo alcanza mediante `host-gateway` (Eclipse Foundation, s. f.).

Los repositorios deben estar uno junto al otro (`backend/energy-monitor-backend`, `frontend/energy-monitor-web` y `devices`). Pasos:

1.  Define en el `.env` del backend `MQTT_BACKEND_USER`, `MQTT_BACKEND_PASS`, `DEVICE_MQTT_PUBLIC_HOST` (la IP del PC en la red local; en Linux, `hostname -I`) y `DEVICE_MQTT_PUBLIC_PORT=8883` (sección 8).

2.  Arranca el backend (sección 9.8) y comprueba que `http://localhost:8080/actuator/health` responde `UP`. El broker consulta al backend para autenticar a todos los clientes, incluido el propio backend.

3.  Genera el certificado TLS de desarrollo (una sola vez):

```bash
cd devices/docker
./mosquitto/generate-dev-certs.sh
```

El script crea una autoridad certificadora de desarrollo y el certificado del broker en `mosquitto/certs/`, válido para las IP del equipo, y escribe `esp32-pzem-monitor/dev_ca.h` para que el firmware confíe en esa autoridad. Si la IP del equipo cambia, vuelve a ejecutarlo: la autoridad se reutiliza y los dispositivos no necesitan reprogramarse.

4.  En Linux con ufw, permite que el contenedor llame al backend (una sola vez), solo desde las redes de Docker:

```bash
sudo ufw allow from 172.16.0.0/12 to any port 8080 proto tcp comment 'mosquitto -> backend'
```

5.  Con el backend en ejecución, inicia el broker:

```bash
docker compose up -d
docker compose logs -f mosquitto
```

El backend aparece como `energy-monitor-backend-telemetry` y `energy-monitor-backend-status`. Los dispositivos usan el puerto 8883 con TLS; el 1883 es interno, solo para el backend, y Docker lo publica únicamente en el bucle local.

> *Si el contenedor se cierra con un error de permisos sobre `server.key`, el contenedor lee el archivo como el usuario 1000: ejecuta `sudo chown 1000 docker/mosquitto/certs/server.key`.*

# 13. Firmware del dispositivo de medición

El firmware (versión 1.3.8) está en `devices/esp32-pzem-monitor/` y se compila con arduino-cli o con Arduino IDE 2 (Arduino, s. f.). No hay actualización por aire (OTA): el firmware se carga siempre por USB.

## 13.1 Cableado

| PZEM-004T V3 | ESP32                  | Nota                              |
|--------------|------------------------|-----------------------------------|
| TX           | GPIO16 (RX2)           | Se cruzan TX y RX                 |
| RX           | GPIO17 (TX2)           |                                   |
| VCC          | 3V3                    | Si no responde, usar 5 V desde VIN |
| GND          | GND                    | Común con el PZEM                 |

La comunicación es Modbus RTU a 9600 baudios, 8N1, con dirección `0xF8`. La fuente HLK-PM03 alimenta la placa desde el mismo circuito que se mide.

> **Advertencia de seguridad:** el PZEM-004T no está aislado galvánicamente. No toques el lado conectado a la red eléctrica ni manipules el cableado de la red con el prototipo energizado.

## 13.2 Herramientas y librerías

```bash
arduino-cli core update-index
arduino-cli core install esp32:esp32
arduino-cli lib install PZEM004Tv30 PubSubClient ArduinoJson WiFiManager NimBLE-Arduino
```

| **Librería**   | **Autor**       | **Versión probada** |
|----------------|-----------------|---------------------|
| PZEM004Tv30    | Jakub Mandula   | 1.2.1               |
| PubSubClient   | Nick O'Leary    | 2.8                 |
| ArduinoJson    | Benoit Blanchon | 7.x                 |
| WiFiManager    | tzapu           | 2.0.17              |
| NimBLE-Arduino | h2zero          | 2.5.1               |

No confundas `PZEM004Tv30` con la librería antigua `PZEM-004T` (v1/v2).

## 13.3 Preparación

```bash
cd devices/esp32-pzem-monitor
cp secrets.h.example secrets.h
```

Define al menos `PORTAL_PASSWORD` (8 caracteres o más) en `secrets.h`: protege el punto de acceso `EnergyMonitor-Setup`. El archivo `dev_ca.h` lo genera `generate-dev-certs.sh` (sección 12). Ambos archivos están ignorados por Git.

## 13.4 Compilar y cargar por USB

El esquema de particiones debe ser **Minimal SPIFFS (1.9 MB APP with OTA)**, porque con Bluetooth el binario (~1,4 MB) no cabe en el esquema por defecto.

```bash
arduino-cli compile --fqbn esp32:esp32:esp32:PartitionScheme=min_spiffs .
arduino-cli upload  --fqbn esp32:esp32:esp32:PartitionScheme=min_spiffs -p /dev/ttyUSB0 .
arduino-cli monitor -p /dev/ttyUSB0 -c baudrate=115200
```

En Arduino IDE: placa **ESP32 Dev Module** y *Partition Scheme* **Minimal SPIFFS**. Cierra cualquier otro monitor serie antes de cargar, o el puerto estará ocupado. En Linux, el usuario necesita acceso al puerto (`sudo usermod -aG dialout $USER` y volver a iniciar sesión).

## 13.5 Modo de configuración

Un dispositivo nuevo, sin configuración, entra solo en el modo de configuración. Para activarlo en uno ya configurado, mantén pulsado el botón **BOOT** (GPIO0) durante 3 segundos. Sin red conocida durante 5 minutos, el dispositivo ofrece la configuración por Bluetooth por sí solo.

# 14. Vincular el primer dispositivo

El dispositivo se registra desde la aplicación web; no hace falta insertar nada a mano en la base de datos. Requisitos: backend, base de datos y broker en ejecución, y la web abierta en Chrome o Edge de escritorio, o Chrome en Android, mediante HTTPS o `localhost`.

1.  En la web: hogar → **Dispositivos** → **Vincular dispositivo** (solo el propietario).

2.  Mantén pulsado **BOOT** en la ESP32 durante 3 segundos hasta que el LED parpadee (un dispositivo nunca vinculado ya está en modo de configuración tras cargarlo).

3.  **Buscar dispositivo**: el navegador muestra `EnergyMonitor-<código>`; selecciónalo.

4.  Elige el electrodoméstico, el nombre y la habitación.

5.  Elige la red Wi-Fi del hogar de la lista (son las redes que ve la ESP32) y escribe su contraseña.

6.  La web registra el dispositivo en el backend, que genera su `api_key`, y envía al dispositivo por Bluetooth la red, la contraseña, el servidor (`DEVICE_MQTT_PUBLIC_HOST`), el identificador y la clave.

7.  Al conectarse al broker, el dispositivo sale solo del modo de configuración.

**Alternativa sin Bluetooth** (Safari, Firefox u otros navegadores): conéctate a la red Wi-Fi `EnergyMonitor-Setup` con la clave `PORTAL_PASSWORD`, abre `http://192.168.4.1` → **Configure WiFi** y completa la red, el servidor MQTT (la IP del PC o el dominio del broker), el puerto 8883, el identificador, el código y la clave del dispositivo. Esta vía exige que el dispositivo ya exista en el backend con su clave.

> *En Linux, Chrome trae Web Bluetooth desactivado: habilita `chrome://flags/#enable-experimental-web-platform-features` y reinicia el navegador. Web Bluetooth solo funciona en `https://` o `http://localhost`.*

Salida esperada en el monitor serie:

```
=== EnergyMonitor ESP32 + PZEM-004T V3 fw=1.3.8 ===
[PZEM] UART2 ready at 9600 baud, addr=0xf8
[WiFi] connected to 'MiHogar', IP=192.168.1.37
[MQTT] connected as esp32-DEV0000042
[MQTT] telemetry published
```

# 15. Producción

Esta sección resume el despliegue en un servidor con Docker Compose: broker, backend y MySQL (`devices/docker/docker-compose.prod.yml`). El `mosquitto.conf` es el mismo archivo que en desarrollo (Let's Encrypt, s. f.).

1.  **Servidor y dominio.** Un servidor con Docker y un dominio para el broker (por ejemplo `mqtt.ejemplo.com`) apuntando a él. Puede ser el mismo dominio de la API.

2.  **Certificado.** Obtén un certificado de Let's Encrypt para ese dominio (`certbot certonly --standalone -d mqtt.ejemplo.com`), copia `fullchain.pem` como `server.crt` y `privkey.pem` como `server.key` en la carpeta `MQTT_CERT_DIR` (legible por el usuario 1000) y repite la copia en cada renovación con un `--deploy-hook` que reinicie el contenedor `mosquitto`. El firmware no cambia: confía en las raíces de Let's Encrypt.

3.  **Configuración.** `cp docker/.env.prod.example docker/.env.prod` y complétalo: contraseñas de MySQL, pepper, carpeta del par de claves JWT, un `MQTT_BACKEND_PASS` aleatorio, `DEVICE_MQTT_PUBLIC_HOST` con el dominio del broker, `NOTIFICATION_WEB_URL` y las claves VAPID. La clave privada JWT debe ser legible por `BACKEND_UID` (10001 por defecto).

4.  **Arranque.**

```bash
cd docker
docker compose -f docker-compose.prod.yml --env-file .env.prod up -d --build
```

5.  **Imagen del backend.** El `Dockerfile` del backend compila con Temurin 21 (JDK) y ejecuta con Temurin 21 (JRE) como usuario sin privilegios (UID 10001), en el puerto 8080.

6.  **Cortafuegos.** Abre los puertos 443 (proxy HTTPS) y 8883 (MQTT con TLS). Mantén cerrados el 1883, el 3306 y el 8080.

7.  **Proxy inverso HTTPS** (Caddy o nginx) delante de `127.0.0.1:8080` para la API y la web. El proxy **debe bloquear `/internal/**`**: detrás del proxy toda solicitud parece local. La web debe servirse por HTTPS, porque Web Bluetooth y el push lo exigen.

8.  **Dirección real del cliente.** Detrás de un proxy, el límite de intentos de recuperación de contraseña se comparte entre todos los usuarios. Para evitarlo, agrega `server.forward-headers-strategy=native` en la configuración del perfil de despliegue (`prod`), no en `application.yaml`. El archivo de ese perfil no está versionado en el repositorio: `[POR CONFIRMAR]` su ubicación.

Las claves VAPID se generan una sola vez por entorno (cambiarlas invalida todas las suscripciones push); los comandos están en `.env.example` y en la sección 7 del README de dispositivos. Varias instancias del backend necesitan valores distintos de `MQTT_CLIENT_ID`.

# 16. Correo local con Mailpit (opcional)

Mailpit es un servidor SMTP de desarrollo: acepta cualquier mensaje y los muestra en una bandeja web sin enviar nada por internet real. Se levanta con Docker (Mailpit, s. f.):

```bash
docker run -d --name mailpit -p 587:1025 -p 8025:8025 \
  -e MP_SMTP_AUTH_ACCEPT_ANY=1 -e MP_SMTP_AUTH_ALLOW_INSECURE=1 axllent/mailpit
```

Usa el puerto 587 del equipo (en lugar del 1025 por defecto de Mailpit) para que al alternar entre Mailpit y Gmail solo cambie `MAIL_HOST` y las credenciales. En `.env`:

```
MAIL_HOST=localhost
MAIL_PORT=587
MAIL_USERNAME=dev
MAIL_PASSWORD=dev
MAIL_FROM=Energy Monitor <no-reply@localhost>
```

Bandeja de entrada del desarrollo: `http://localhost:8025`. Cuando uses Gmail, el código de recuperación llega al buzón real asociado a la cuenta con la «app password».

# 17. Verificación de la instalación

Ejecuta esta lista antes de dar por terminada la instalación. Cada comando debe producir el resultado indicado:

| **Componente**   | **Comprobación**                                                                  | **Resultado esperado**                                                                           |
|------------------|-----------------------------------------------------------------------------------|--------------------------------------------------------------------------------------------------|
| Docker           | `docker compose ps` en energy-monitor-backend                                     | `energy-monitor-mysql` en estado `healthy`.                                                      |
| Backend          | Abrir `http://localhost:8080/actuator/health`                                     | Responde `UP`.                                                                                   |
| Backend          | Abrir `http://localhost:8080/swagger-ui.html`                                     | Swagger UI lista la documentación de `/api/v1`.                                                  |
| OpenAPI          | `GET http://localhost:8080/v3/api-docs`                                           | JSON con la especificación de OpenAPI.                                                           |
| Broker           | `docker compose ps` y `docker compose logs -f mosquitto` en `devices/docker`      | `energy-monitor-mqtt` activo con el puerto 8883 publicado; el backend aparece conectado como `energy-monitor-backend-telemetry` y `-status`. |
| Dispositivo      | Monitor serie del ESP32                                                           | `[MQTT] connected as esp32-<identificador>` y `[MQTT] telemetry published`.                      |
| Primera medición | Panel del hogar en la web                                                         | La primera medición del dispositivo aparece en la web en aproximadamente un minuto.              |
| Web              | `npm run dev` en Energy-Monitor-application                                       | Vite en `http://localhost:5173` muestra la pantalla de acceso.                                   |
| Móvil            | `npm start` en Energy-Monitor-Mobile                                              | Expo muestra el QR y, al abrirlo con Expo Go, la app permite iniciar sesión contra el backend.   |
| Correo           | `MAIL_HOST=localhost` + Mailpit en :587                                           | Los códigos de recuperación aparecen en `http://localhost:8025`.                                 |

Para una comprobación de punta a punta: registra un usuario desde la web, verifica su correo (Mailpit o Gmail), inicia sesión, crea un hogar, vincula el dispositivo (sección 14) y confirma que su primera medición aparece (Cortés Parra y García Artunduaga, 2026).

# 18. Resumen de comandos de instalación

| **Paso**      | **Backend (energy-monitor-backend)** | **Web (Energy-Monitor-application)** | **Móvil (Energy-Monitor-Mobile)**       |
|---------------|--------------------------------------|--------------------------------------|-----------------------------------------|
| Variables     | Copy-Item .env.example .env          | Copy-Item .env.example .env.local    | Copy-Item .env.example .env             |
| Deps.         | (wrapper Maven incluido)             | npm install                          | npm install                             |
| Base de datos | docker compose up -d                 | —                                    | —                                       |
| Claves/pepper | openssl … (sección 9.5, 9.6)         | —                                    | —                                       |
| Pruebas       | .\mvnw.cmd test                      | npm test                             | npm run lint                            |
| Ejecutar      | .\mvnw.cmd spring-boot:run           | npm run dev                          | npm start                               |
| URL típica    | http://localhost:8080                | http://localhost:5173                | QR / localhost:8081 (Expo)              |

Broker (`devices/docker`): `./mosquitto/generate-dev-certs.sh` y `docker compose up -d`. Firmware (`devices/esp32-pzem-monitor`): `arduino-cli compile` y `arduino-cli upload` (sección 13).

# 19. Solución de problemas habituales

| **Síntoma**                                                 | **Causa probable**                                                                                      | **Acción correctiva**                                                                                           |
|-------------------------------------------------------------|---------------------------------------------------------------------------------------------------------|-----------------------------------------------------------------------------------------------------------------|
| El backend no arranca y nombra un path de clave o el pepper | Faltan `SECURITY_JWT_PRIVATE_KEY_PATH`, `SECURITY_JWT_PUBLIC_KEY_PATH` o `SECURITY_RESET_PEPPER`.       | Definirlas en `.env` o en `application-local.yaml` (rutas absolutas) y reintentar.                              |
| `Access denied for user 'energy_app'` al correr tests       | No está `SPRING_DATASOURCE_PASSWORD` en `.env`.                                                         | Copiarla del valor de `MYSQL_PASSWORD` en `.env` (sección 9.2).                                                 |
| El backend no arranca por un valor del `.env`               | Un valor con comillas o con `$` (el archivo se lee como `.properties`).                                 | Escribir los valores sin comillas y sin `$`.                                                                    |
| El navegador bloquea las llamadas desde la web              | Falta `SECURITY_CORS_ALLOWED_ORIGINS` con el origen de Vite.                                            | Agregarlo en `.env` del backend (sección 10.4).                                                                 |
| No llegan los códigos de recuperación                       | Correo mal configurado; el error queda en la tabla `notification`.                                      | Revisar `MAIL_*`, usar Mailpit y leer la bandeja en `:8025` (sección 16).                                       |
| Mailpit/Docker no arranca                                   | Docker Desktop detenido o puerto ocupado.                                                               | Iniciar Docker Desktop; cambiar puerto con `-p 1587:1025` si 587 está en uso.                                   |
| El dispositivo no conecta (`[MQTT] connection failed, rc=-2`) | `DEVICE_MQTT_PUBLIC_HOST` apunta a `localhost` o a una IP equivocada, el certificado no es válido para la IP actual o la red lo bloquea. | Usar la IP del PC en la red local, volver a ejecutar `generate-dev-certs.sh`, reiniciar el broker y revisar el firewall. |
| El dispositivo muestra `rc=4` o `rc=5`                      | El código o la clave no coinciden con los del backend, o el backend está detenido.                      | Iniciar el backend y volver a vincular el dispositivo.                                                          |
| El broker registra `context deadline exceeded` al llamar a `backend:8080` | El backend está detenido o falta la regla de ufw.                                          | Iniciar el backend y aplicar la regla de la sección 12.                                                         |
| La web no encuentra el dispositivo por Bluetooth            | El dispositivo no está en modo de configuración, el navegador no es compatible o la página no es segura. | Mantener BOOT 3 s hasta que parpadee el LED, usar Chrome o Edge y abrir la web en `localhost` o por HTTPS; en Linux habilitar el flag de la sección 14. |
| Falla `wifi_failed:auth` al vincular                        | Contraseña de Wi-Fi incorrecta.                                                                         | Volver a vincular con la contraseña correcta.                                                                   |
| El push del navegador no llega                              | Faltan las claves VAPID o el usuario no concedió el permiso de notificaciones; el navegador exige HTTPS o `localhost`. | Definir `NOTIFICATION_VAPID_PUBLIC_KEY` y `NOTIFICATION_VAPID_PRIVATE_KEY`, reiniciar el backend y activar las notificaciones en el navegador. |
| El botón de Google responde `503`                           | `GOOGLE_CLIENT_ID` y `GOOGLE_CLIENT_SECRET` están vacíos en el backend.                                 | Configurar ambas con las credenciales OAuth («Aplicación web») y reiniciar el backend.                          |
| Falla la carga del firmware: puerto ocupado                 | Otro programa usa el puerto serie.                                                                      | Cerrar el monitor serie de Arduino IDE o cualquier otro programa que use `/dev/ttyUSB0`.                        |
| «Sketch too big» al compilar                                | El esquema de particiones no es Minimal SPIFFS.                                                         | Compilar con `PartitionScheme=min_spiffs` (sección 13.4).                                                       |
| «~» no se expande en la ruta de claves                      | Java no expande el símbolo en `private-key-path`.                                                       | Usar la ruta absoluta completa (`C:\Users\tu-usuario\.energy-monitor\keys\`).                                   |
| La app móvil no llega al backend                            | `EXPO_PUBLIC_API_BASE_URL` apunta a `localhost`, que en el emulador o en el teléfono es el propio equipo. | Usar `10.0.2.2` en el emulador o la IP del PC en el teléfono y reiniciar Expo (sección 11.2).                 |
| Expo no conecta con Expo Go                                 | Distinta red, firewall, o QR inválido.                                                                  | Usar `npx expo start --tunnel`; verificar scripts de red corporativa.                                           |
| `npm install` falla en móvil/web                            | Versión de Node distinta a la del software de referencia.                                               | Actualizar a Node 22 LTS (mín. 20.19) y reinstalar.                                                             |

# 20. Referencias (formato APA, 7.ª edición)

Las citas en el texto y la lista de referencias siguen el estilo de la séptima edición del manual de publicaciones de la APA (American Psychological Association \[APA\], 2020).

American Psychological Association. (2020). Publication manual of the American Psychological Association (7.ª ed.). https://www.apa.org/

Arduino. (s. f.). Arduino CLI. https://arduino.github.io/arduino-cli/

Cortés Parra, J. E. y García Artunduaga, M. A. (2026). *Especificación de requisitos de software de EnergyMonitor* (versión 2.0). SENA – CIES Regional Huila.

Docker Inc. (s. f.). Docker documentation. https://docs.docker.com/

Eclipse Foundation. (s. f.). Eclipse Mosquitto. https://mosquitto.org/

Expo. (s. f.). Expo documentation. https://docs.expo.dev/

Git. (s. f.). Git for Windows. https://git-scm.com/download/win

Google. (s. f.). Cómo crear una contraseña de aplicación. https://myaccount.google.com/apppasswords

Let's Encrypt. (s. f.). Documentation. https://letsencrypt.org/docs/

Mailpit. (s. f.). Mailpit: Web and API based SMTP testing. https://mailpit.axllent.org/

MySQL. (2026). MySQL 8.4 reference manual. https://dev.mysql.com/doc/refman/8.4/en/

Node.js Foundation. (s. f.). Node.js. https://nodejs.org/

OpenSSL. (s. f.). OpenSSL cryptography and SSL/TLS toolkit. https://www.openssl.org/

Oracle. (s. f.). Java SE downloads. https://www.oracle.com/java/technologies/downloads/

Spring. (s. f.). Spring Boot reference documentation. https://docs.spring.io/spring-boot/index.html

Vite. (s. f.). Vite guide. https://vite.dev/guide/
