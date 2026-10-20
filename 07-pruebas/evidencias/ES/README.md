# Evidencias de pruebas

## Versión 1.1

Fecha de ejecución: 20 de octubre de 2026.

| Repositorio | Rama | Commit | Nota |
|---|---|---|---|
| energy-monitor-backend | main | `90ae5ef` | Servicio central y firmware; suite completa contra MySQL (1131 pruebas, 0 fallos), también en el *check* `Tests` de GitHub Actions |
| energy-monitor-web | main | `2b41cb3` | Aplicación web; 141 pruebas aprobadas, también en el *check* `Tests and build` |
| energy-monitor-mobile | dev | `4ebccfb` | Sin pruebas automatizadas |
| energy-monitor-infra | main | `7324110` | Compose, scripts y plantilla de Nginx validados en el *check* `Validate compose and scripts` |
| energy-monitor-docs-final | dev | `00806c6` | Documentación final (este documento) |

- [backend/resumen-suite-completa.md](backend/resumen-suite-completa.md)
- [web/resumen-vitest.md](web/resumen-vitest.md) (sección «Ejecución de la versión 1.1»)

## Versión 1.0 (histórica)

Fecha de ejecución: 14 de octubre de 2026.

| Repositorio | Rama | Commit | Nota |
|---|---|---|---|
| energy-monitor-backend | dev | `d03bbd3` | Servicio central; el árbol tenía 4 clases de prueba nuevas sin confirmar |
| energy-monitor-web | dev | `e6edc12` | Aplicación web |
| energy-monitor-mobile | dev | `a3f6875` | Sin pruebas automatizadas |
| devices | dev | `57417a7` | Repositorio ya retirado: el firmware pasó al backend y el broker a `energy-monitor-infra` |
| energy-monitor-docs-final | dev | `2af09ab` | Documentación final (este documento) |

- [backend/resumen-surefire.md](backend/resumen-surefire.md)
- [backend/log-ejecucion-sin-bd.md](backend/log-ejecucion-sin-bd.md)
- [backend/log-pruebas-nuevas.md](backend/log-pruebas-nuevas.md)
- [web/resumen-vitest.md](web/resumen-vitest.md)

No hay evidencia de cobertura: JaCoCo no está en el `pom.xml` del backend y la web no define un script de cobertura.

Versión en inglés: [../EN/README.md](../EN/README.md)
