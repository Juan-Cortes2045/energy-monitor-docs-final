# Enlaces de Repositorios GitHub

Actualizado el 20 de octubre de 2026.

## Sistema en producción

- Aplicación web y API: [https://energy-monitor.tech](https://energy-monitor.tech)
- Broker MQTT de los medidores: `mqtt.energy-monitor.tech:8883` (MQTT con TLS; no se abre en el navegador)

## Repositorios vigentes

| Componente | Repositorio | Contenido | Rama de producción | Rama de integración |
|---|---|---|---|---|
| Backend | [energy-monitor-backend](https://github.com/Juan-Cortes2045/energy-monitor-backend) | API REST Spring Boot y firmware del ESP32 (`firmware/`) | `main` | `dev` |
| Frontend web | [Energy-Monitor-application](https://github.com/Juan-Cortes2045/Energy-Monitor-application) | Aplicación web React + Vite | `main` | `dev` |
| Frontend móvil | [Energy-Monitor-Mobile](https://github.com/angelgarcia0/Energy-Monitor-Mobile) | Aplicación Android (Expo / React Native) | `main` | `dev` |
| Infraestructura | [energy-monitor-infra](https://github.com/Juan-Cortes2045/energy-monitor-infra) | Docker Compose (local, desarrollo y producción), broker Mosquitto, scripts de despliegue y CI/CD | `main` | — (las ramas de trabajo van directo a `main` por PR) |
| Documentación | [energy-monitor-docs-final](https://github.com/Juan-Cortes2045/energy-monitor-docs-final) | Documentación final bilingüe (ES/EN) | `main` | `dev` |

## Integración y despliegue continuos

Backend, web e infraestructura tienen el workflow `.github/workflows/ci-cd.yml` (GitHub Actions):

| Repositorio | Check obligatorio en los PR | Despliegue |
|---|---|---|
| energy-monitor-backend | `Tests` | Desde `main`, con aprobación del environment `production` |
| Energy-Monitor-application | `Tests and build` | Desde `main`, con aprobación del environment `production` |
| energy-monitor-infra | `Validate compose and scripts` | Desde `main`, con aprobación del environment `production` |

## Versión desplegada

Commit en `main` desplegado por GitHub Actions el 19 de octubre de 2026:

| Repositorio | Commit |
|---|---|
| energy-monitor-backend | `90ae5ef` |
| Energy-Monitor-application | `2b41cb3` |
| energy-monitor-infra | `7324110` |

## Repositorio retirado

- [energy-monitor-devices](https://github.com/Juan-Cortes2045/energy-monitor-devices): ya no se usa. El firmware del ESP32 pasó a `energy-monitor-backend/firmware/` y la configuración del broker Mosquitto a `energy-monitor-infra/mosquitto/`. Se conserva solo como historial.
