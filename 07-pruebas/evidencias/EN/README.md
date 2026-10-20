# Test Evidence

## Version 1.1

Run date: October 20, 2026.

| Repository | Branch | Commit | Note |
|---|---|---|---|
| energy-monitor-backend | main | `90ae5ef` | Core service and firmware; full suite against MySQL (1131 tests, 0 failures), also in the `Tests` check of GitHub Actions |
| energy-monitor-web | main | `2b41cb3` | Web application; 141 tests passed, also in the `Tests and build` check |
| energy-monitor-mobile | dev | `4ebccfb` | No automated tests |
| energy-monitor-infra | main | `7324110` | Compose files, scripts and Nginx template validated in the `Validate compose and scripts` check |
| energy-monitor-docs-final | dev | `00806c6` | Final documentation (this document) |

- [backend/full-suite-summary.md](backend/full-suite-summary.md)
- [web/vitest-summary.md](web/vitest-summary.md) (section “Version 1.1 Run”)

## Version 1.0 (historical)

Run date: October 14, 2026.

| Repository | Branch | Commit | Note |
|---|---|---|---|
| energy-monitor-backend | dev | `d03bbd3` | Core service; the tree had 4 new uncommitted test classes |
| energy-monitor-web | dev | `e6edc12` | Web application |
| energy-monitor-mobile | dev | `a3f6875` | No automated tests |
| devices | dev | `57417a7` | Repository now retired: the firmware moved to the backend and the broker to `energy-monitor-infra` |
| energy-monitor-docs-final | dev | `2af09ab` | Final documentation (this document) |

- [backend/surefire-summary.md](backend/surefire-summary.md)
- [backend/log-run-without-db.md](backend/log-run-without-db.md)
- [backend/log-new-tests.md](backend/log-new-tests.md)
- [web/vitest-summary.md](web/vitest-summary.md)

There is no coverage evidence: JaCoCo is not in the backend `pom.xml` and the web defines no coverage script.

Spanish version: [../ES/README.md](../ES/README.md)
