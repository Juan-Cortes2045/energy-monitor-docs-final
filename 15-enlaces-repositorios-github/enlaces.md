# Enlaces de Repositorios GitHub

Actualizado el 28 de octubre de 2026.

## Sistema en producción

- Aplicación web y API: [https://energy-monitor.tech](https://energy-monitor.tech) (la API entra siempre por el API gateway)
- Broker MQTT de los medidores: `mqtt.energy-monitor.tech:8883` (MQTT con TLS; no se abre en el navegador)

## Repositorios vigentes

Desde el 28 de octubre de 2026 la API está repartida en un API gateway y siete microservicios (seguridad, dispositivos, mediciones, hogares, notificaciones, alertas y recomendaciones). El backend monolítico (`energy-monitor-backend`) se retiró.

| Componente | Repositorio | Contenido | Rama de producción | Rama de integración |
|---|---|---|---|---|
| API gateway | [en-monitor-api-gateway](https://github.com/Juan-Cortes2045/en-monitor-api-gateway) | Única entrada de la API (Spring Cloud Gateway, puerto 8080) | `main` | `dev` |
| Servicio de seguridad | [en-monitor-security-service](https://github.com/Juan-Cortes2045/en-monitor-security-service) | Cuentas, registro con verificación de correo, términos, sesiones, JWT, Google, RBAC y administración de usuarios (puerto 8082) | `main` | `dev` |
| Servicio de dispositivos | [en-monitor-device-service](https://github.com/Juan-Cortes2045/en-monitor-device-service) | Vinculación y credenciales de los dispositivos, autenticación del broker, comandos y conectividad (puerto 8084) | `main` | `dev` |
| Servicio de mediciones | [en-monitor-measurement-service](https://github.com/Juan-Cortes2045/en-monitor-measurement-service) | Telemetría, historial, estadísticas, consumo y niveles de consumo (puerto 8085) | `main` | `dev` |
| Servicio de hogares | [en-monitor-home-service](https://github.com/angelgarcia0/en-monitor-home-service) | Hogares, miembros, roles, favoritos, umbrales, eliminación y transferencia de propiedad (puerto 8086) | `main` | `dev` |
| Servicio de notificaciones | [en-monitor-notification-service](https://github.com/angelgarcia0/en-monitor-notification-service) | Entregas, preferencias, correo, Web Push, push móvil, avisos in-app y actividad (puerto 8087) | `main` | `dev` |
| Servicio de alertas | [en-monitor-alert-service](https://github.com/angelgarcia0/en-monitor-alert-service) | Alertas de consumo y de conectividad (puerto 8088) | `main` | `dev` |
| Servicio de recomendaciones | [en-monitor-recommendation-service](https://github.com/angelgarcia0/en-monitor-recommendation-service) | Reglas de ahorro y recomendaciones (puerto 8089) | `main` | `dev` |
| Frontend web | [Energy-Monitor-application](https://github.com/Juan-Cortes2045/Energy-Monitor-application) | Aplicación web React 19 + Vite 8 | `main` | `dev` |
| Frontend móvil | [Energy-Monitor-Mobile](https://github.com/angelgarcia0/Energy-Monitor-Mobile) | Aplicación Android (Expo 57 / React Native) | `main` | `dev` |
| Infraestructura | [energy-monitor-infra](https://github.com/Juan-Cortes2045/energy-monitor-infra) | Docker Compose (local, desarrollo y producción), RabbitMQ, broker Mosquitto, scripts de despliegue, pruebas de punta a punta (`e2e/`) y CI/CD | `main` | `dev` |
| Documentación | [energy-monitor-docs-final](https://github.com/Juan-Cortes2045/energy-monitor-docs-final) | Documentación final bilingüe (ES/EN) | `main` | `dev` |

En todos los repositorios los *pull requests* van a `dev` y de `dev` a `main`. Otros repositorios de documentación del proyecto: `en-monitor-docs` (code-sena) y `Energy-Monitor-Documentation`.

## Integración y despliegue continuos

El gateway, los siete servicios, la web y la infraestructura tienen el workflow `.github/workflows/ci-cd.yml` (GitHub Actions). Cada repositorio tiene su environment `production` con los secretos `DEPLOY_HOST`, `DEPLOY_USER`, `DEPLOY_SSH_KEY` y `DEPLOY_KNOWN_HOSTS`, y despliega su componente con `infra/deploy/deploy.sh`. La aplicación móvil no tiene pipeline ni se despliega en el servidor, y el firmware del medidor y los repositorios de documentación tampoco tienen pipeline porque no tienen build ni pruebas automáticas.

| Repositorio | Jobs de CI | Componente de `deploy.sh` | Despliegue |
|---|---|---|---|
| en-monitor-api-gateway | `Tests` | `gateway` | Desde `main`, con aprobación del environment `production` |
| en-monitor-security-service | `Tests` | `security` | Desde `main`, con aprobación del environment `production` |
| en-monitor-device-service | `Tests` y `Docker image` | `device` | Desde `main`, con aprobación del environment `production` |
| en-monitor-measurement-service | `Tests` y `Docker image` | `measurement` | Desde `main`, con aprobación del environment `production` |
| en-monitor-home-service | `Tests` y `Docker image` | `home` | Desde `main`, con aprobación del environment `production` |
| en-monitor-notification-service | `Tests` y `Docker image` | `notification` | Desde `main`, con aprobación del environment `production` (pipeline incorporado el 28 de octubre de 2026) |
| en-monitor-alert-service | `Tests` y `Docker image` | `alert` | Desde `main`, con aprobación del environment `production` |
| en-monitor-recommendation-service | `Tests` y `Docker image` | `recommendation` | Desde `main`, con aprobación del environment `production` |
| Energy-Monitor-application | `Tests and build` | `frontend` | Desde `main`, con aprobación del environment `production` |
| energy-monitor-infra | `Validate compose and scripts` | `infra` | Desde `main`, con aprobación del environment `production` |

## Versión desplegada

Commit en `main` de cada repositorio al cierre del 28 de octubre de 2026. El equipo confirmó ese día que todos los servicios y la web estaban en producción y que todos los repositorios, salvo el móvil, tienen CI/CD. Los cambios de la última jornada (consumo inusual, fotos de perfil, ayuda en Ajustes y novedades) ya están en `main` de seguridad, hogares, alertas, notificaciones y la web; su despliegue depende de la aprobación del *environment* `production` de cada repositorio.

| Repositorio | Commit | Estado en producción |
|---|---|---|
| en-monitor-api-gateway | `0b2c164` | Desplegado |
| en-monitor-security-service | `de52c95` | En `main` (perfil interno con foto); despliegue por confirmar |
| en-monitor-device-service | `06c1bdc` | Desplegado |
| en-monitor-measurement-service | `8a9b544` | Desplegado |
| en-monitor-home-service | `6a4e396` | En `main` (miembros con foto); despliegue por confirmar |
| en-monitor-notification-service | `ecdbc0a` | En `main` (texto de la alerta de consumo inusual); despliegue por confirmar |
| en-monitor-alert-service | `0e65430` | En `main` (alerta de consumo inusual); despliegue por confirmar |
| en-monitor-recommendation-service | `aeeeec5` | Desplegado |
| Energy-Monitor-application | `27ad9f0` | En `main` (modales de ayuda, visor de fotos y novedades); despliegue por confirmar |
| energy-monitor-infra | `ceb1543` | Desplegado |
| Energy-Monitor-Mobile (no se despliega en el servidor) | `894b65a` en `main`; `8b5edc0` en `dev` (188 commits por fusionar) | No aplica |

## Repositorios retirados

- [energy-monitor-backend](https://github.com/Juan-Cortes2045/energy-monitor-backend): backend monolítico (Spring Modulith) retirado el 28 de octubre de 2026, cuando sus módulos pasaron a servicios propios. Se conserva como historial y porque contiene el firmware del ESP32 (`firmware/`).
- [energy-monitor-devices](https://github.com/Juan-Cortes2045/energy-monitor-devices): ya no se usa. El firmware del ESP32 pasó a `energy-monitor-backend/firmware/` y la configuración del broker Mosquitto a `energy-monitor-infra/mosquitto/`. Se conserva solo como historial.
