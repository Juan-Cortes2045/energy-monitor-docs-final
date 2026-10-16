# Test Evidence

Run date: October 14, 2026.

| Repository | Branch | Commit | Note |
|---|---|---|---|
| energy-monitor-backend | dev | `d03bbd3` | Core service; the tree has 4 new uncommitted test classes |
| energy-monitor-web | dev | `e6edc12` | Web application |
| energy-monitor-mobile | dev | `a3f6875` | No automated tests |
| devices | dev | `57417a7` | The tree has 2 modified uncommitted files |
| energy-monitor-docs-final | dev | `2af09ab` | Final documentation (this document) |

- [backend/surefire-summary.md](backend/surefire-summary.md)
- [backend/log-run-without-db.md](backend/log-run-without-db.md)
- [backend/log-new-tests.md](backend/log-new-tests.md)
- [web/vitest-summary.md](web/vitest-summary.md)

There is no coverage evidence: JaCoCo is not in the backend `pom.xml` and the web defines no coverage script.

Spanish version: [../ES/README.md](../ES/README.md)
