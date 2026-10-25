# Enlaces de Repositorios GitHub

Actualizado el 24 de octubre de 2026.

## Sistema en producción

- Aplicación web y API: [https://energy-monitor.tech](https://energy-monitor.tech) (la API entra siempre por el API gateway)
- Broker MQTT de los medidores: `mqtt.energy-monitor.tech:8883` (MQTT con TLS; no se abre en el navegador)

## Repositorios vigentes

Desde el 24 de octubre de 2026 la API está repartida en un API gateway, tres microservicios (seguridad, dispositivos y mediciones) y el backend, que sigue siendo un monolito modular con los módulos home, alert, recommendation y notification. La migración de esos cuatro módulos a microservicios está en curso.

| Componente | Repositorio | Contenido | Rama de producción | Rama de integración |
|---|---|---|---|---|
| API gateway | [en-monitor-api-gateway](https://github.com/Juan-Cortes2045/en-monitor-api-gateway) | Única entrada de la API (Spring Cloud Gateway, puerto 8080) | `main` | `dev` |
| Servicio de seguridad | [en-monitor-security-service](https://github.com/Juan-Cortes2045/en-monitor-security-service) | Cuentas, sesiones, JWT, Google, RBAC y administración de usuarios (puerto 8082) | `main` | `dev` |
| Servicio de dispositivos | [en-monitor-device-service](https://github.com/Juan-Cortes2045/en-monitor-device-service) | Vinculación y credenciales de los dispositivos, autenticación del broker, comandos y conectividad (puerto 8084) | `main` | `dev` |
| Servicio de mediciones | [en-monitor-measurement-service](https://github.com/Juan-Cortes2045/en-monitor-measurement-service) | Telemetría, historial, estadísticas, consumo y niveles de consumo (puerto 8085) | `main` | `dev` |
| Backend | [energy-monitor-backend](https://github.com/Juan-Cortes2045/energy-monitor-backend) | Monolito modular Spring Modulith con home, alert, recommendation y notification (puerto 8083), y firmware del ESP32 (`firmware/`) | `main` | `dev` |
| Frontend web | [Energy-Monitor-application](https://github.com/Juan-Cortes2045/Energy-Monitor-application) | Aplicación web React 19 + Vite 8 | `main` | `dev` |
| Frontend móvil | [Energy-Monitor-Mobile](https://github.com/angelgarcia0/Energy-Monitor-Mobile) | Aplicación Android (Expo 57 / React Native) | `main` | `dev` |
| Infraestructura | [energy-monitor-infra](https://github.com/Juan-Cortes2045/energy-monitor-infra) | Docker Compose (local, desarrollo y producción), RabbitMQ, broker Mosquitto, scripts de despliegue, pruebas de punta a punta (`e2e/`) y CI/CD | `main` | `dev` |
| Documentación | [energy-monitor-docs-final](https://github.com/Juan-Cortes2045/energy-monitor-docs-final) | Documentación final bilingüe (ES/EN) | `main` | `dev` |

En todos los repositorios los *pull requests* van a `dev` y de `dev` a `main`. Otros repositorios de documentación del proyecto: `en-monitor-docs` (code-sena) y `Energy-Monitor-Documentation`.

## Integración y despliegue continuos

El gateway, los tres servicios, el backend, la web y la infraestructura tienen el workflow `.github/workflows/ci-cd.yml` (GitHub Actions). Cada repositorio tiene su environment `production` con los secretos `DEPLOY_HOST`, `DEPLOY_USER`, `DEPLOY_SSH_KEY` y `DEPLOY_KNOWN_HOSTS`, y despliega su componente con `infra/deploy/deploy.sh`. La aplicación móvil no tiene pipeline.

| Repositorio | Jobs de CI | Componente de `deploy.sh` | Despliegue |
|---|---|---|---|
| en-monitor-api-gateway | `Tests` | `gateway` | Desde `main`, con aprobación del environment `production` |
| en-monitor-security-service | `Tests` | `security` | Desde `main`, con aprobación del environment `production` |
| en-monitor-device-service | `Tests` y `Docker image` | `device` | Desde `main`, con aprobación del environment `production` |
| en-monitor-measurement-service | `Tests` y `Docker image` | `measurement` | Desde `main`, con aprobación del environment `production` |
| energy-monitor-backend | `Tests` | `backend` | Desde `main`, con aprobación del environment `production` |
| Energy-Monitor-application | `Tests and build` | `frontend` | Desde `main`, con aprobación del environment `production` |
| energy-monitor-infra | `Validate compose and scripts` | `infra` | Desde `main`, con aprobación del environment `production` |

## Versión desplegada

Commit en `main` en producción el 24 de octubre de 2026:

| Repositorio | Commit |
|---|---|
| en-monitor-api-gateway | `c070cec` |
| en-monitor-security-service | `334691a` |
| en-monitor-device-service | `f8b8526` |
| en-monitor-measurement-service | `ee2cb81` |
| energy-monitor-backend | `aa9a713` |
| Energy-Monitor-application | `e59bece` |
| Energy-Monitor-Mobile (no se despliega en el servidor) | `894b65a` |
| energy-monitor-infra | `eaf961b` |

## Repositorio retirado

- [energy-monitor-devices](https://github.com/Juan-Cortes2045/energy-monitor-devices): ya no se usa. El firmware del ESP32 pasó a `energy-monitor-backend/firmware/` y la configuración del broker Mosquitto a `energy-monitor-infra/mosquitto/`. Se conserva solo como historial.
