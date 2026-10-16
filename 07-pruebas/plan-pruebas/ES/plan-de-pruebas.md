# Plan y Reporte de Pruebas de EnergyMonitor

Juan Esteban Cortés Parra y Miguel Ángel García Artunduaga

Servicio Nacional de Aprendizaje (SENA)

Tecnólogo en Análisis y Desarrollo de Software (ADSO), ficha 3145556

14 de octubre de 2026

## Control de versiones

**Tabla 1**

*Control de versiones del documento*

| Versión | Fecha      | Descripción del cambio                                                                    | Responsables                                  |
|---------|------------|-------------------------------------------------------------------------------------------|-----------------------------------------------|
| 1.0     | 14/10/2026 | Primera versión: inventario, trazabilidad y resultados de la ejecución sin base de datos. | Cortés Parra, J. E.; García Artunduaga, M. A. |

## Resumen

Este documento es el plan y reporte de pruebas de EnergyMonitor. Inventaría las 129 clases de prueba automatizadas del servicio central (1097 métodos de prueba declarados) y las 20 suites de la aplicación web (141 pruebas), traza los 107 requisitos funcionales y no funcionales de la especificación contra esas pruebas, y registra la ejecución del 14 de octubre de 2026. La suite del servicio central sin base de datos ejecutó 835 pruebas, con 833 aprobadas y 2 fallidas; la web ejecutó 141 pruebas, todas aprobadas. Las 37 clases que necesitan MySQL no se ejecutaron porque falta la variable SPRING_DATASOURCE_PASSWORD. La aplicación móvil no tiene pruebas automatizadas. Se escribieron 4 clases de prueba nuevas y se registraron 2 defectos y 1 observación documental.

*Palabras clave:* pruebas de software, trazabilidad, JUnit, Vitest, Spring Modulith, EnergyMonitor

## Introducción

### Propósito

Documentar las pruebas que ya existen y se ejecutan en EnergyMonitor, y mostrar qué requisitos cubren y cuáles no antes de la entrega del 16 de octubre de 2026. La estructura se adapta de IEEE 829-2008 e ISO/IEC/IEEE 29119-3.

### Alcance

Se prueba el servicio central (siete módulos: security, home, device, measurement, alert, recommendation y notification), la aplicación web, la ingesta MQTT y la autorización del broker. El firmware del dispositivo, el broker Mosquitto, Bluetooth, las notificaciones push y la aplicación móvil Android quedan fuera de las pruebas automatizadas y se declaran como vacíos conocidos. No se prueban el rendimiento, la carga ni la accesibilidad (WCAG), que la especificación deja como `[POR CONFIRMAR]`.

### Referencias del documento

La trazabilidad usa los códigos de la especificación de requisitos (RF-SEC, RF-HOM, RF-DEV, RF-MED, RF-ALE, RF-REC, RF-NOT, RF-CLI y RNF) y las historias HU-01 a HU-15 (Cortés Parra y García Artunduaga, 2026). Los IDs, los nombres de clases y los códigos son idénticos en las versiones en español e inglés.

## Elementos bajo prueba

La Tabla 2 lista los elementos, la rama y el commit con el que se ejecutó cada prueba.

**Tabla 2**

*Elementos bajo prueba, versión, rama y commit*

| Elemento                  | Versión                         | Rama | Commit  | Observación                                                              |
|---------------------------|---------------------------------|------|---------|--------------------------------------------------------------------------|
| energy-monitor-backend    | Java 21 · Spring Boot 4.1.1     | dev  | d03bbd3 | Servicio central; el árbol tiene 4 clases de prueba nuevas sin confirmar |
| energy-monitor-web        | React 19 · Vite 8               | dev  | e6edc12 | Aplicación web                                                           |
| energy-monitor-mobile     | Expo SDK 57 · React Native 0.86 | dev  | a3f6875 | Sin pruebas automatizadas                                                |
| devices                   | Firmware 1.3.8 · Mosquitto 2    | dev  | 57417a7 | El árbol tiene 2 archivos modificados sin confirmar                      |
| energy-monitor-docs-final | Markdown ES/EN                  | dev  | 2af09ab | Documentación final (este documento)                                     |

## Estrategia de pruebas

### Niveles de prueba

**Tabla 3**

*Niveles de prueba y su estado en el proyecto*

| N.º | Nivel                                   | Clases/suites | Métodos de prueba | Clases ejecutadas |
|-----|-----------------------------------------|---------------|-------------------|-------------------|
| 1   | Dominio                                 | 20            | 337               | 20                |
| 2   | Casos de uso y adaptadores sin contexto | 50            | 276               | 50                |
| 3   | Web (controladores)                     | 10            | 144               | 7                 |
| 4   | Seguridad                               | 18            | 195               | 12                |
| 5   | Integración con MySQL real              | 27            | 126               | 0                 |
| 6   | Arquitectura                            | 1             | 2                 | 0                 |
| 7   | Eventos entre módulos                   | 1             | 1                 | 1                 |
| 8   | MQTT y broker                           | 2             | 16                | 2                 |
| 9   | Frontend web (Vitest)                   | 20            | 141               | 20                |

Todos los niveles de la lista existen en el proyecto. El nivel 9 corresponde a las suites de la web. La aplicación móvil no tiene pruebas automatizadas.

### Técnicas y herramientas

Se combinan pruebas de caja blanca (dominio y casos de uso con puertos simulados) y de caja negra (contratos HTTP), partición de equivalencia y valores límite (por ejemplo los niveles de potencia en 500, 1500 y 3000 W, los umbrales y los 150 segundos de silencio). Las pruebas nuevas siguen el estilo AAA y las convenciones del proyecto (nombres descriptivos con @DisplayName).

**Tabla 4**

*Herramientas de prueba y versiones verificadas*

| Herramienta                                    | Versión                    | Uso                                                                  |
|------------------------------------------------|----------------------------|----------------------------------------------------------------------|
| Spring Boot / spring-boot-starter-test         | 4.1.1                      | Pruebas de contexto, MockMvc y @MockitoBean (nunca @MockBean)        |
| JUnit Jupiter                                  | 6.0.3                      | Ejecución y pruebas parametrizadas                                   |
| Mockito                                        | 5.x [POR CONFIRMAR]        | Puertos simulados (5.20.0 y 5.23.0 en el repositorio local de Maven) |
| AssertJ                                        | 3.27.7                     | Aserciones                                                           |
| Spring Modulith (spring-modulith-starter-test) | 2.1.1                      | ModularityTests: límites de módulo                                   |
| Vitest / Testing Library (React)               | 5.0.3 / 16.3.3             | Pruebas de la aplicación web (jsdom)                                 |
| Java / Node.js / Docker                        | 21.0.12 / 22.22.2 / 29.8.2 | Entorno de ejecución                                                 |
| JaCoCo                                         | No está en el pom.xml      | Sin reporte de cobertura (ver recomendaciones)                       |

## Entorno de pruebas

Las pruebas de persistencia y las de contexto completo corren contra MySQL 8.4 real (contenedor energy-monitor-mysql), no contra H2. Liquibase es el dueño del esquema e Hibernate está en ddl-auto: validate; una base H2 o un @AutoConfigureTestDatabase(replace = ANY) cambiarían el dialecto y anularían la validación del esquema real. El costo es que esas pruebas no son herméticas: necesitan MySQL activo, el perfil test y la variable SPRING_DATASOURCE_PASSWORD; el usuario por defecto es energy_app.

**Tabla 5**

*Componentes del entorno*

| Componente       | Detalle                                                                        |
|------------------|--------------------------------------------------------------------------------|
| MySQL 8.4        | Docker, energy-monitor-mysql (healthy)                                         |
| Perfil de Spring | test (src/test/resources/application-test.yaml); recommendation.enabled: false |

## Criterios de entrada, salida, suspensión y reanudación

**Tabla 6**

*Criterios de las pruebas*

| Criterio    | Condición                                                                                                                                    |
|-------------|----------------------------------------------------------------------------------------------------------------------------------------------|
| Entrada     | Código compilable en dev; MySQL activo y SPRING_DATASOURCE_PASSWORD definida para las pruebas con contexto; npm install ejecutado en la web. |
| Salida      | Todas las pruebas automatizadas aprobadas, ningún defecto de severidad alta abierto y las clases con MySQL ejecutadas.                       |
| Suspensión  | MySQL no disponible, falta de credenciales o fallo de compilación.                                                                           |
| Reanudación | Entorno restablecido; se repite desde la clase que se detuvo.                                                                                |

## Inventario de pruebas automatizadas

Cada tabla corresponde a un módulo del servicio central. «Estado» es Aprobada, Falla (con su defecto) o «Por ejecutar (MySQL)» para las clases que levantan contexto con base de datos. Las clases nuevas están marcadas con NUEVA en su descripción.

**Tabla 7**

*Pruebas del módulo security*

| Nivel                  | Clase de prueba                       | Qué verifica                                                                                  | Métodos | Requisitos que cubre                                                                    | Estado               |
|------------------------|---------------------------------------|-----------------------------------------------------------------------------------------------|---------|-----------------------------------------------------------------------------------------|----------------------|
| Dominio                | AuditLogTest                          | Reglas e invariantes de AuditLog                                                              | 9       | RF-SEC-12                                                                               | Aprobada             |
| Dominio                | PasswordResetTokenTest                | Reglas e invariantes de PasswordResetToken                                                    | 19      | RF-SEC-06                                                                               | Aprobada             |
| Dominio                | RbacTest                              | Reglas e invariantes de Rbac                                                                  | 14      | RF-SEC-09                                                                               | Aprobada             |
| Dominio                | RefreshTokenTest                      | Reglas e invariantes de RefreshToken                                                          | 41      | RF-SEC-04, RNF-SEG-02                                                                   | Aprobada             |
| Dominio                | SecuritySupportModelTest              | Reglas e invariantes de SecuritySupportModel                                                  | 28      | RF-SEC-12                                                                               | Falla (DEF-01)       |
| Dominio                | UserSessionTest                       | Reglas e invariantes de UserSession                                                           | 45      | RF-SEC-04                                                                               | Aprobada             |
| Dominio                | UserTest                              | Reglas e invariantes de User                                                                  | 21      | RF-SEC-01                                                                               | Aprobada             |
| Dominio                | ValueObjectTest                       | Reglas e invariantes de ValueObject                                                           | 14      | RF-SEC-01                                                                               | Aprobada             |
| Casos de uso y adaptad | AccountLifecycleServiceTest           | Consulta de cuenta, edición de perfil y cambio de estado de la cuenta                         | 9       | RF-SEC-08                                                                               | Aprobada             |
| Casos de uso y adaptad | AuthenticateUserServiceTest           | Estado de cuenta y credenciales clasificados; todo fallo queda en el registro                 | 6       | RF-SEC-03                                                                               | Aprobada             |
| Casos de uso y adaptad | ChangePasswordServiceTest             | Cambio de contraseña con verificación de la actual y política                                 | 3       | RF-SEC-07                                                                               | Aprobada             |
| Casos de uso y adaptad | DeleteAccountServiceTest              | Eliminar la cuenta valida la contraseña y anuncia AccountDeleted                              | 2       | RF-SEC-11                                                                               | Aprobada             |
| Casos de uso y adaptad | EmailVerificationServiceTest          | Código de verificación de seis dígitos válido al menos 15 minutos                             | 6       | RF-SEC-02                                                                               | Aprobada             |
| Casos de uso y adaptad | PasswordResetServiceTest              | Recuperación: código de un solo uso, ventana de validez y presupuesto de intentos             | 16      | RF-SEC-06                                                                               | Aprobada             |
| Casos de uso y adaptad | RefreshSessionServiceTest             | Renovación: uso único, linaje, identidad de sesión y escalada de reutilización                | 20      | RF-SEC-04, RNF-SEG-01, RNF-SEG-02                                                       | Aprobada             |
| Casos de uso y adaptad | RegisterUserServiceTest               | Registro: hash de contraseña, persona y usuario, rechazo de duplicados y política             | 7       | RF-SEC-01, RF-SEC-12                                                                    | Aprobada             |
| Casos de uso y adaptad | SignInWithGoogleServiceTest           | Inicio de sesión con Google y creación de la cuenta                                           | 7       | RF-SEC-05, RNF-INT-03                                                                   | Aprobada             |
| Casos de uso y adaptad | UserAdministrationServiceTest         | Administración de cuentas: un administrador nunca actúa sobre su propia cuenta                | 18      | —                                                                                       | Aprobada             |
| Casos de uso y adaptad | UserProfileQueryServiceTest           | Nombre y correo de las cuentas conocidas                                                      | 1       | RF-SEC-08                                                                               | Aprobada             |
| Casos de uso y adaptad | UserSessionServiceTest                | Apertura de sesión con refresh token opaco y cierre                                           | 6       | RF-SEC-04                                                                               | Aprobada             |
| Web (controladores)    | AdminControllerWebTest                | Endpoints de administración /api/v1/admin con permisos RBAC y 409 al administrarse a sí mismo | 15      | —                                                                                       | Por ejecutar (MySQL) |
| Web (controladores)    | AuthControllerWebTest                 | Contrato HTTP de autenticación, cuenta, contraseña, perfil, Google y permisos (401, 403, 429) | 66      | RF-SEC-01, RF-SEC-02, RF-SEC-03, RF-SEC-05, RF-SEC-07, RF-SEC-08, RF-SEC-09, RNF-SEG-06 | Por ejecutar (MySQL) |
| Seguridad              | AuthRefreshEndpointWebTest            | Contrato HTTP de POST /api/v1/auth/refresh                                                    | 18      | RF-SEC-04                                                                               | Por ejecutar (MySQL) |
| Seguridad              | BCryptPasswordHasherAdapterTest       | BCryptPasswordHasherAdapter                                                                   | 10      | RF-SEC-03                                                                               | Aprobada             |
| Seguridad              | InMemoryResetAttemptLimiterTest       | Límite de 10 intentos por IP cada 15 minutos                                                  | 8       | RNF-SEG-06                                                                              | Aprobada             |
| Seguridad              | JwtTokenIssuerTest                    | Emisión y validación de JWT RS256 (15 minutos)                                                | 18      | RF-SEC-03, RNF-SEG-01                                                                   | Aprobada             |
| Seguridad              | PemRsaKeysTest                        | Qué claves PEM se aceptan y qué mensaje se da al rechazarlas                                  | 19      | —                                                                                       | Aprobada             |
| Seguridad              | PepperedHmacResetCodeHasherTest       | PepperedHmacResetCodeHasher                                                                   | 15      | RF-SEC-06, RNF-SEG-05                                                                   | Aprobada             |
| Seguridad              | PoolSaturatedLimiterTest              | El límite por IP se aplica sin esperar una conexión del pool (429)                            | 2       | RNF-SEG-06                                                                              | Por ejecutar (MySQL) |
| Seguridad              | RbacServiceTest                       | Asignar y revocar roles con auditoría y verificación de permisos                              | 17      | RF-SEC-09                                                                               | Aprobada             |
| Seguridad              | ResetAttemptLimiterBeforeDatabaseTest | Un canje rechazado por el límite no abre transacción                                          | 3       | RNF-SEG-06                                                                              | Por ejecutar (MySQL) |
| Seguridad              | SecureIdentifierGeneratorAdapterTest  | SecureIdentifierGeneratorAdapter                                                              | 5       | —                                                                                       | Aprobada             |
| Seguridad              | SecureTokenGeneratorAdapterTest       | SecureTokenGeneratorAdapter                                                                   | 13      | —                                                                                       | Aprobada             |
| Seguridad              | SecurityCorsConfigurationTest         | Guardas de la política CORS sin levantar contexto                                             | 11      | RNF-SEG-03                                                                              | Aprobada             |
| Seguridad              | SecurityCorsWebTest                   | Contrato CORS: orígenes respondidos y respuesta del preflight                                 | 23      | RNF-SEG-03                                                                              | Por ejecutar (MySQL) |
| Seguridad              | SecurityLoginFlowTest                 | Orden del intercambio de inicio de sesión y lo que rechaza                                    | 11      | RF-SEC-03                                                                               | Aprobada             |
| Seguridad              | Sha256TokenHasherTest                 | Sha256TokenHasher                                                                             | 9       | RF-SEC-04                                                                               | Aprobada             |
| Integración con MySQL  | ConfigurationPolicyPersistenceTest    | Ida y vuelta de ConfigurationPolicy contra MySQL real                                         | 6       | RF-SEC-12                                                                               | Por ejecutar (MySQL) |
| Integración con MySQL  | LogPersistenceTest                    | Ida y vuelta de Log contra MySQL real                                                         | 5       | RF-SEC-12                                                                               | Por ejecutar (MySQL) |
| Integración con MySQL  | PasswordPolicySeedTest                | Política de contraseñas sembrada por el changeset security-019                                | 5       | RF-SEC-12                                                                               | Por ejecutar (MySQL) |
| Integración con MySQL  | PasswordResetTokenReservationTest     | Reserva de intentos de canje contra MySQL                                                     | 6       | RF-SEC-06                                                                               | Por ejecutar (MySQL) |
| Integración con MySQL  | RbacAssignmentPersistenceTest         | Ida y vuelta de RbacAssignment contra MySQL real                                              | 2       | RF-SEC-09                                                                               | Por ejecutar (MySQL) |
| Integración con MySQL  | RefreshLifecycleIntegrationTest       | Ciclo completo: abrir sesión, rotar, detectar reutilización y revocar la familia              | 15      | RF-SEC-04, RNF-SEG-02                                                                   | Por ejecutar (MySQL) |
| Integración con MySQL  | RefreshSessionTransactionTest         | La rotación de refresh token es atómica contra la BD real                                     | 6       | RF-SEC-04                                                                               | Por ejecutar (MySQL) |
| Integración con MySQL  | RefreshTokenPersistenceTest           | Ida y vuelta de RefreshToken contra MySQL real                                                | 10      | RF-SEC-04                                                                               | Por ejecutar (MySQL) |
| Integración con MySQL  | SecurityTransactionBoundaryTest       | Dónde queda el límite transaccional y que Spring lo respeta                                   | 5       | —                                                                                       | Por ejecutar (MySQL) |
| Integración con MySQL  | SecurityTransactionRollbackTest       | La transacción hace rollback de verdad                                                        | 3       | —                                                                                       | Por ejecutar (MySQL) |
| Integración con MySQL  | TokenSessionPersistenceTest           | Ida y vuelta de TokenSession contra MySQL real                                                | 6       | RF-SEC-04                                                                               | Por ejecutar (MySQL) |
| Integración con MySQL  | UserPersonPersistenceTest             | Ida y vuelta de UserPerson contra MySQL real                                                  | 5       | RF-SEC-01, RNF-MAN-03                                                                   | Por ejecutar (MySQL) |

**Tabla 8**

*Pruebas del módulo home*

| Nivel                  | Clase de prueba                  | Qué verifica                                                                                  | Métodos | Requisitos que cubre                                                                      | Estado               |
|------------------------|----------------------------------|-----------------------------------------------------------------------------------------------|---------|-------------------------------------------------------------------------------------------|----------------------|
| Dominio                | HomeTest                         | Reglas e invariantes de Home                                                                  | 16      | RF-HOM-01                                                                                 | Aprobada             |
| Dominio                | HomeThresholdsTest               | Reglas e invariantes de HomeThresholds                                                        | 17      | RF-HOM-08                                                                                 | Aprobada             |
| Dominio                | HomeTypeTest                     | Reglas e invariantes de HomeType                                                              | 8       | —                                                                                         | Aprobada             |
| Dominio                | MemberPermissionsTest            | Reglas e invariantes de MemberPermissions                                                     | 3       | RF-HOM-05                                                                                 | Aprobada             |
| Dominio                | OwnerPermissionsTest             | Reglas e invariantes de OwnerPermissions                                                      | 3       | RF-HOM-05                                                                                 | Aprobada             |
| Dominio                | RoleTest                         | Reglas e invariantes de Role                                                                  | 3       | RF-HOM-05                                                                                 | Aprobada             |
| Dominio                | UserHomeTest                     | Reglas e invariantes de UserHome                                                              | 26      | RF-HOM-05                                                                                 | Aprobada             |
| Casos de uso y adaptad | CreateHomeServiceTest            | Caso de uso CreateHome: reglas de negocio y errores con puertos simulados                     | 6       | RF-HOM-01                                                                                 | Aprobada             |
| Casos de uso y adaptad | DeleteHomeServiceTest            | Solo el propietario elimina el hogar; anuncia HomeDeleted; miembro recibe 403                 | 4       | RF-HOM-09, RNF-MAN-03                                                                     | Aprobada             |
| Casos de uso y adaptad | GetHomeThresholdsServiceTest     | Caso de uso GetHomeThresholds: reglas de negocio y errores con puertos simulados              | 2       | RF-HOM-08                                                                                 | Aprobada             |
| Casos de uso y adaptad | GetHomeTypeServiceTest           | Caso de uso GetHomeType: reglas de negocio y errores con puertos simulados                    | 2       | —                                                                                         | Aprobada             |
| Casos de uso y adaptad | JoinHomeServiceTest              | Caso de uso JoinHome: reglas de negocio y errores con puertos simulados                       | 4       | RF-HOM-03                                                                                 | Aprobada             |
| Casos de uso y adaptad | LeaveHomeServiceTest             | Caso de uso LeaveHome: reglas de negocio y errores con puertos simulados                      | 4       | RF-HOM-06                                                                                 | Aprobada             |
| Casos de uso y adaptad | ListHomeTypesServiceTest         | Caso de uso ListHomeTypes: reglas de negocio y errores con puertos simulados                  | 2       | —                                                                                         | Aprobada             |
| Casos de uso y adaptad | ListHomesServiceTest             | Caso de uso ListHomes: reglas de negocio y errores con puertos simulados                      | 5       | RF-HOM-02                                                                                 | Aprobada             |
| Casos de uso y adaptad | ListMembersServiceTest           | Caso de uso ListMembers: reglas de negocio y errores con puertos simulados                    | 4       | RF-HOM-04                                                                                 | Aprobada             |
| Casos de uso y adaptad | RemoveDeletedAccountServiceTest  | Cuenta eliminada: borra los hogares propios y rechaza si es único propietario con miembros    | 2       | RF-SEC-11                                                                                 | Aprobada             |
| Casos de uso y adaptad | RemoveUserServiceTest            | Caso de uso RemoveUser: reglas de negocio y errores con puertos simulados                     | 5       | RF-HOM-06                                                                                 | Aprobada             |
| Casos de uso y adaptad | ToggleFavoriteServiceTest        | Caso de uso ToggleFavorite: reglas de negocio y errores con puertos simulados                 | 2       | RF-HOM-07                                                                                 | Aprobada             |
| Casos de uso y adaptad | TransferHomeOwnershipServiceTest | Transferencia de propiedad: roles intercambiados, un solo propietario, destinos inválidos     | 6       | RF-HOM-10                                                                                 | Aprobada             |
| Casos de uso y adaptad | UpdateHomeThresholdsServiceTest  | Caso de uso UpdateHomeThresholds: reglas de negocio y errores con puertos simulados           | 7       | RF-HOM-08                                                                                 | Aprobada             |
| Web (controladores)    | HomeControllerTest               | Endpoints de hogares: crear, listar, unirse, favorito, expulsar, salir, eliminar y transferir | 26      | RF-HOM-01, RF-HOM-02, RF-HOM-03, RF-HOM-04, RF-HOM-06, RF-HOM-07, RF-HOM-08, RF-HOM-09, … | Aprobada             |
| Web (controladores)    | HomeExceptionHandlerTest         | Respuesta del módulo cuando la solicitud no puede atribuirse a un usuario                     | 2       | RF-SEC-10                                                                                 | Aprobada             |
| Integración con MySQL  | HomePersistenceTest              | Ida y vuelta de Home contra MySQL real                                                        | 6       | RF-HOM-01, RNF-MAN-03                                                                     | Por ejecutar (MySQL) |
| Integración con MySQL  | HomeThresholdsPersistenceTest    | Ida y vuelta de HomeThresholds contra MySQL real                                              | 3       | RF-HOM-08                                                                                 | Por ejecutar (MySQL) |
| Integración con MySQL  | HomeTypePersistenceTest          | Ida y vuelta de HomeType contra MySQL real                                                    | 3       | —                                                                                         | Por ejecutar (MySQL) |
| Integración con MySQL  | HomeTypeSeedTest                 | El catálogo contiene exactamente los seis tipos de hogar sembrados                            | 1       | RF-HOM-01                                                                                 | Por ejecutar (MySQL) |
| Integración con MySQL  | UserHomePersistenceTest          | Ida y vuelta de UserHome contra MySQL real                                                    | 7       | RF-HOM-05                                                                                 | Por ejecutar (MySQL) |

**Tabla 9**

*Pruebas del módulo device*

| Nivel                  | Clase de prueba               | Qué verifica                                                                                                      | Métodos | Requisitos que cubre                                  | Estado   |
|------------------------|-------------------------------|-------------------------------------------------------------------------------------------------------------------|---------|-------------------------------------------------------|----------|
| Casos de uso y adaptad | BrokerAccessServiceTest       | Autenticación del broker con la clave en claro contra el hash y autorización de tópicos                           | 8       | RF-DEV-07, RNF-SEG-09                                 | Aprobada |
| Casos de uso y adaptad | DeviceConnectivityServiceTest | Estado en línea y sin conexión, bitácora de cambios y 150 s de silencio                                           | 6       | RF-ALE-02, RF-MED-05, RF-MED-06, RNF-DIS-02           | Aprobada |
| Casos de uso y adaptad | HomeDeviceServiceTest         | Vincular, listar, editar, reemitir credenciales y desvincular con reglas de propietario                           | 9       | RF-DEV-02, RF-DEV-03, RF-DEV-04, RF-DEV-05, RF-DEV-06 | Aprobada |
| Web (controladores)    | HomeDeviceControllerTest      | NUEVA. Endpoints de dispositivos del hogar: vincular, editar, credenciales, desvincular y errores 400/403/404/409 | 10      | RF-DEV-02, RF-DEV-03, RF-DEV-04, RF-DEV-05, RF-DEV-06 | Aprobada |
| Seguridad              | ApiKeyHashTest                | Hash SHA-256 de la api_key del dispositivo                                                                        | 3       | RF-DEV-07, RNF-SEG-09                                 | Aprobada |
| MQTT y broker          | BrokerAuthControllerTest      | NUEVA. /internal/mqtt/auth/user y /acl: 200 concede, 403 deniega, códigos de acceso 1 a 4                         | 8       | RF-DEV-07                                             | Aprobada |

**Tabla 10**

*Pruebas del módulo measurement*

| Nivel                  | Clase de prueba                     | Qué verifica                                                                                        | Métodos | Requisitos que cubre            | Estado               |
|------------------------|-------------------------------------|-----------------------------------------------------------------------------------------------------|---------|---------------------------------|----------------------|
| Dominio                | ConsumptionLevelTest                | Niveles LOW, MEDIUM, HIGH y CRITICAL y sus límites                                                  | 14      | RF-MED-09                       | Aprobada             |
| Dominio                | MeasurementTest                     | Reglas e invariantes de Measurement                                                                 | 12      | RF-MED-02                       | Aprobada             |
| Casos de uso y adaptad | GetConsumptionStatisticsServiceTest | Caso de uso GetConsumptionStatistics: reglas de negocio y errores con puertos simulados             | 5       | RF-MED-07                       | Aprobada             |
| Casos de uso y adaptad | GetLatestMeasurementServiceTest     | Caso de uso GetLatestMeasurement: reglas de negocio y errores con puertos simulados                 | 2       | RF-MED-07                       | Aprobada             |
| Casos de uso y adaptad | HomeConsumptionUseCaseTest          | Resumen e historial de consumo del hogar                                                            | 8       | RF-MED-08                       | Aprobada             |
| Casos de uso y adaptad | ListConsumptionLevelsServiceTest    | Caso de uso ListConsumptionLevels: reglas de negocio y errores con puertos simulados                | 2       | RF-MED-09                       | Aprobada             |
| Casos de uso y adaptad | ListMeasurementsServiceTest         | Caso de uso ListMeasurements: reglas de negocio y errores con puertos simulados                     | 4       | RF-MED-07                       | Aprobada             |
| Casos de uso y adaptad | RegisterMeasurementServiceTest      | Caso de uso RegisterMeasurement: reglas de negocio y errores con puertos simulados                  | 4       | RF-MED-02                       | Aprobada             |
| Web (controladores)    | ConsumptionLevelControllerTest      | Códigos HTTP y cuerpo de ConsumptionLevelController                                                 | 1       | RF-MED-09                       | Aprobada             |
| Web (controladores)    | MeasurementControllerTest           | Códigos HTTP y cuerpo de MeasurementController                                                      | 5       | RF-MED-07                       | Aprobada             |
| Integración con MySQL  | ConsumptionLevelPersistenceTest     | Ida y vuelta de ConsumptionLevel contra MySQL real                                                  | 4       | RF-MED-09                       | Por ejecutar (MySQL) |
| Integración con MySQL  | MeasurementPersistenceTest          | Ida y vuelta de Measurement contra MySQL real                                                       | 4       | RF-MED-02                       | Por ejecutar (MySQL) |
| MQTT y broker          | TelemetryMqttConsumerTest           | NUEVA. Ingesta de telemetría: el tópico identifica al dispositivo y se descartan mensajes inválidos | 8       | RF-DEV-07, RF-MED-02, RF-MED-03 | Aprobada             |

**Tabla 11**

*Pruebas del módulo alert*

| Nivel                  | Clase de prueba                      | Qué verifica                                                                       | Métodos | Requisitos que cubre | Estado               |
|------------------------|--------------------------------------|------------------------------------------------------------------------------------|---------|----------------------|----------------------|
| Dominio                | AlertTest                            | Reglas e invariantes de Alert                                                      | 16      | RF-ALE-03, RF-ALE-04 | Aprobada             |
| Casos de uso y adaptad | GetAlertServiceTest                  | Caso de uso GetAlert: reglas de negocio y errores con puertos simulados            | 2       | RF-ALE-05            | Aprobada             |
| Casos de uso y adaptad | ListAlertsServiceTest                | Caso de uso ListAlerts: reglas de negocio y errores con puertos simulados          | 3       | RF-ALE-05            | Aprobada             |
| Casos de uso y adaptad | RegisterDeviceLinkedAlertServiceTest | Aviso de dispositivo vinculado con la primera lectura                              | 4       | RF-ALE-01            | Aprobada             |
| Casos de uso y adaptad | RegisterLimitAlertServiceTest        | Alertas de límite diario o mensual                                                 | 4       | RF-ALE-04            | Aprobada             |
| Casos de uso y adaptad | RegisterThresholdAlertServiceTest    | Alertas de consumo alto o crítico por nivel de potencia                            | 7       | RF-ALE-03            | Aprobada             |
| Casos de uso y adaptad | ResolveAlertServiceTest              | Caso de uso ResolveAlert: reglas de negocio y errores con puertos simulados        | 4       | RF-ALE-05            | Aprobada             |
| Web (controladores)    | AlertControllerTest                  | Endpoints de alertas: listar, consultar, leer, eliminar y 404                      | 7       | RF-ALE-05            | Aprobada             |
| Integración con MySQL  | AlertPersistenceTest                 | Ida y vuelta de Alert contra MySQL real                                            | 4       | RF-ALE-05            | Por ejecutar (MySQL) |
| Eventos entre módulos  | MeasurementRecordedListenerTest      | El evento MeasurementRecorded dispara el registro de alertas de umbral y de límite | 1       | RF-ALE-03, RF-ALE-06 | Aprobada             |

**Tabla 12**

*Pruebas del módulo recommendation*

| Nivel                  | Clase de prueba                    | Qué verifica                                                                                          | Métodos | Requisitos que cubre                                                                   | Estado               |
|------------------------|------------------------------------|-------------------------------------------------------------------------------------------------------|---------|----------------------------------------------------------------------------------------|----------------------|
| Dominio                | RecommendationRulesTest            | Cinco reglas de ahorro: hora punta, reposo, comparación semanal, proyección y aumento por dispositivo | 8       | RF-REC-01, RF-REC-02, RF-REC-03, RF-REC-04, RF-REC-05, RF-REC-06, RF-REC-07, RF-REC-09 | Aprobada             |
| Casos de uso y adaptad | EvaluateRecommendationsServiceTest | Evaluación de reglas por hogar y no repetir en 7 días                                                 | 4       | RF-REC-01, RF-REC-07                                                                   | Aprobada             |
| Casos de uso y adaptad | RecommendationInboxServiceTest     | Caso de uso RecommendationInbox: reglas de negocio y errores con puertos simulados                    | 4       | RF-REC-08                                                                              | Aprobada             |
| Web (controladores)    | RecommendationControllerTest       | NUEVA. Bandeja de recomendaciones: listar, leer, eliminar, 404 y 409                                  | 7       | RF-REC-08                                                                              | Falla (DEF-02)       |
| Integración con MySQL  | RecommendationPersistenceTest      | Ida y vuelta de Recommendation contra MySQL real                                                      | 3       | RF-REC-08                                                                              | Por ejecutar (MySQL) |

**Tabla 13**

*Pruebas del módulo notification*

| Nivel                  | Clase de prueba                       | Qué verifica                                                                              | Métodos | Requisitos que cubre                                   | Estado               |
|------------------------|---------------------------------------|-------------------------------------------------------------------------------------------|---------|--------------------------------------------------------|----------------------|
| Dominio                | NotificationTest                      | Reglas e invariantes de Notification                                                      | 20      | RF-NOT-01, RF-NOT-05                                   | Aprobada             |
| Casos de uso y adaptad | AbandonedDeliverySweeperTest          | Barrido de entregas abandonadas al iniciar                                                | 4       | RF-NOT-05                                              | Aprobada             |
| Casos de uso y adaptad | AdminNotificationsTest                | Notificaciones de administrador: quién recibe cada evento y lecturas acotadas al llamante | 5       | —                                                      | Aprobada             |
| Casos de uso y adaptad | EmailPasswordResetDeliveryAdapterTest | Envío del código de recuperación; nada sale si la transacción hace rollback               | 5       | RF-NOT-06, RF-SEC-06                                   | Aprobada             |
| Casos de uso y adaptad | EmailSenderTest                       | Registro del intento antes del envío y envío del correo                                   | 9       | RF-NOT-01, RF-NOT-05, RF-NOT-06, RF-SEC-02, RNF-INT-02 | Aprobada             |
| Casos de uso y adaptad | NotifyAlertTest                       | Entrega de cada alerta a los miembros por correo y push según preferencias                | 4       | RF-NOT-01, RF-NOT-07                                   | Aprobada             |
| Casos de uso y adaptad | SendPasswordResetEmailTest            | Mensaje de recuperación de contraseña y su registro                                       | 11      | RF-NOT-06, RF-SEC-06                                   | Aprobada             |
| Casos de uso y adaptad | WebPushSenderTest                     | Cifrado del mensaje push (RFC 8291) como lo descifra un navegador                         | 2       | RF-NOT-01, RF-NOT-04                                   | Aprobada             |
| Web (controladores)    | AdminNotificationsControllerWebTest   | Endpoints de notificaciones de administrador                                              | 5       | —                                                      | Por ejecutar (MySQL) |
| Integración con MySQL  | NotificationPersistenceTest           | Bitácora de entregas contra el esquema real                                               | 4       | RF-NOT-05                                              | Por ejecutar (MySQL) |

**Tabla 14**

*Pruebas del módulo transversales*

| Nivel                  | Clase de prueba                      | Qué verifica                                                                                 | Métodos | Requisitos que cubre  | Estado               |
|------------------------|--------------------------------------|----------------------------------------------------------------------------------------------|---------|-----------------------|----------------------|
| Seguridad              | AdminRbacIntegrationTest             | Rol ADMIN sembrado, códigos USER.READ, USER.MANAGE y ROLE.MANAGE y búsquedas del directorio  | 6       | RF-SEC-09             | Por ejecutar (MySQL) |
| Seguridad              | HomeCallerIdentityIntegrationTest    | Identidad de quien llama a /homes por el filtro real y un JWT RS256; X-User-Id se ignora     | 4       | RF-SEC-10             | Por ejecutar (MySQL) |
| Integración con MySQL  | AccountDeletionIntegrationTest       | Borrado de cuenta entre security y home contra el esquema real                               | 3       | RF-SEC-11             | Por ejecutar (MySQL) |
| Integración con MySQL  | DeviceApiKeyStorageIntegrationTest   | Solo el hash SHA-256 de la api_key llega a la tabla; el acceso al broker sigue funcionando   | 3       | RF-DEV-02, RNF-SEG-09 | Por ejecutar (MySQL) |
| Integración con MySQL  | EnergyMonitorBackendApplicationTests | Arranque del contexto completo de la aplicación                                              | 1       | —                     | Por ejecutar (MySQL) |
| Integración con MySQL  | HomeDeletionIntegrationTest          | Eliminar un hogar borra membresías y umbrales y desvincula dispositivos; un miembro no puede | 2       | RF-DEV-06, RF-HOM-09  | Por ejecutar (MySQL) |
| Integración con MySQL  | HomeOwnershipTransferIntegrationTest | Intercambio atómico de roles OWNER y MEMBER contra el esquema real                           | 4       | RF-HOM-05, RF-HOM-10  | Por ejecutar (MySQL) |
| Arquitectura           | ModularityTests                      | ApplicationModules.verify(): los límites de los siete módulos se respetan                    | 2       | RNF-MAN-01            | Por ejecutar (MySQL) |

**Tabla 15**

*Pruebas de la aplicación web (Vitest)*

| Suite                                                   | Pruebas | Requisitos que cubre                       | Estado   |
|---------------------------------------------------------|---------|--------------------------------------------|----------|
| components/layout/MainLayout/Sidebar/Sidebar.test.jsx   | 2       | RF-CLI-01                                  | Aprobada |
| context/HomeContext.test.jsx                            | 11      | —                                          | Aprobada |
| features/Account/Account.test.jsx                       | 6       | RF-CLI-01, RF-SEC-08, RF-SEC-11            | Aprobada |
| features/Account/AccountDeleteError.test.jsx            | 1       | RF-SEC-11                                  | Aprobada |
| features/Admin/AdminNotifications.test.jsx              | 5       | —                                          | Aprobada |
| features/Admin/AdminUsers.test.jsx                      | 9       | —                                          | Aprobada |
| features/DetailHome/Home/DeleteHome/DeleteHome.test.jsx | 5       | RF-CLI-01, RF-HOM-09                       | Aprobada |
| features/DetailHome/Users/Users.test.jsx                | 12      | RF-CLI-01, RF-HOM-04, RF-HOM-06, RF-HOM-10 | Aprobada |
| features/auth/components/RPasswordForm/recover.test.jsx | 4       | RF-CLI-01                                  | Aprobada |
| features/auth/components/RegisterForm/verify.test.jsx   | 4       | RF-CLI-01                                  | Aprobada |
| features/dashboard/pages/DashboardPage.test.jsx         | 5       | RF-CLI-01, RF-HOM-02                       | Aprobada |
| features/shared/homeTypes.test.js                       | 6       | —                                          | Aprobada |
| routes/RequirePermission.test.jsx                       | 3       | —                                          | Aprobada |
| routes/UserOnlyRoute.test.jsx                           | 4       | RF-CLI-01                                  | Aprobada |
| services/admin/adminApi.test.js                         | 4       | —                                          | Aprobada |
| services/auth/authApi.test.js                           | 4       | —                                          | Aprobada |
| services/home/homeApi.test.js                           | 15      | —                                          | Aprobada |
| services/http/errorMessages.test.js                     | 27      | —                                          | Aprobada |
| services/http/httpClient.refresh.test.js                | 3       | —                                          | Aprobada |
| services/http/httpClient.test.js                        | 11      | —                                          | Aprobada |

Las pruebas de la aplicación móvil no existen: el repositorio no tiene archivos de prueba ni script de pruebas (solo expo lint). Es una brecha declarada.

## Matriz de trazabilidad requisito → prueba

Los 107 requisitos de la especificación se cruzan con las clases de prueba. «Cobertura» distingue lo que se ejecutó de lo que depende de MySQL. Resultado: 66 con prueba automatizada ejecutada, 1 con pruebas automatizadas que dependen de MySQL y 40 sin cobertura.

**Tabla 16**

*Trazabilidad de los requisitos RF-SEC*

| Requisito | Descripción                                                                                               | Pruebas automatizadas                                                                                                                                                                      | Cobertura    |
|-----------|-----------------------------------------------------------------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|--------------|
| RF-SEC-01 | El sistema debe permitir registrar una cuenta con datos personales, correo y contraseña                   | RegisterUserServiceTest, AuthControllerWebTest, UserPersonPersistenceTest, UserTest, ValueObjectTest                                                                                       | Automatizada |
| RF-SEC-02 | El sistema debe enviar al correo registrado un código de verificación de seis dígitos y marcar el correo… | EmailVerificationServiceTest, EmailSenderTest, AuthControllerWebTest                                                                                                                       | Automatizada |
| RF-SEC-03 | El sistema debe permitir iniciar sesión de forma segura con correo y contraseña                           | AuthenticateUserServiceTest, SecurityLoginFlowTest, AuthControllerWebTest, JwtTokenIssuerTest, BCryptPasswordHasherAdapterTest                                                             | Automatizada |
| RF-SEC-04 | El sistema debe mantener la sesión del usuario sin pedirle sus credenciales continuamente, permitirle…    | RefreshSessionServiceTest, UserSessionServiceTest, UserSessionTest, RefreshTokenTest, AuthRefreshEndpointWebTest, RefreshLifecycleIntegrationTest, …                                       | Automatizada |
| RF-SEC-05 | El sistema debe permitir registrarse e iniciar sesión con una cuenta de Google                            | SignInWithGoogleServiceTest, AuthControllerWebTest                                                                                                                                         | Automatizada |
| RF-SEC-06 | El sistema debe permitir recuperar la contraseña con un código de seis dígitos enviado al correo, válido… | PasswordResetServiceTest, PasswordResetTokenTest, PepperedHmacResetCodeHasherTest, SendPasswordResetEmailTest, EmailPasswordResetDeliveryAdapterTest, PasswordResetTokenReservationTest, … | Automatizada |
| RF-SEC-07 | El sistema debe permitir al usuario con sesión iniciada cambiar su contraseña                             | ChangePasswordServiceTest, AuthControllerWebTest                                                                                                                                           | Automatizada |
| RF-SEC-08 | El sistema debe permitir consultar los datos de la cuenta y actualizar el perfil                          | AccountLifecycleServiceTest, UserProfileQueryServiceTest, AuthControllerWebTest, Account.test.jsx                                                                                          | Automatizada |
| RF-SEC-09 | El sistema debe permitir conocer los permisos del usuario                                                 | RbacServiceTest, RbacTest, RbacAssignmentPersistenceTest, AdminRbacIntegrationTest, AuthControllerWebTest                                                                                  | Automatizada |
| RF-SEC-10 | El sistema debe identificar al usuario únicamente a partir de su sesión y no debe aceptar identidades…    | HomeCallerIdentityIntegrationTest, HomeExceptionHandlerTest                                                                                                                                | Automatizada |
| RF-SEC-11 | El sistema debe permitir al usuario eliminar su cuenta confirmando su contraseña                          | DeleteAccountServiceTest, RemoveDeletedAccountServiceTest, AccountDeletionIntegrationTest, AccountDeleteError.test.jsx, Account.test.jsx                                                   | Automatizada |
| RF-SEC-12 | El sistema debe aplicar una política de contraseñas (longitud mínima de 8 y máxima de 64 caracteres) y…   | PasswordPolicySeedTest, ConfigurationPolicyPersistenceTest, LogPersistenceTest, AuditLogTest, SecuritySupportModelTest, RegisterUserServiceTest                                            | Automatizada |

**Tabla 17**

*Trazabilidad de los requisitos RF-HOM*

| Requisito | Descripción                                                                                                | Pruebas automatizadas                                                                                                                | Cobertura     |
|-----------|------------------------------------------------------------------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------|---------------|
| RF-HOM-01 | El sistema debe permitir crear un hogar eligiendo un tipo de hogar de un catálogo                          | CreateHomeServiceTest, HomeControllerTest, HomeTest, HomePersistenceTest, HomeTypeSeedTest                                           | Automatizada  |
| RF-HOM-02 | El sistema debe mostrar al usuario los hogares a los que pertenece                                         | ListHomesServiceTest, HomeControllerTest, DashboardPage.test.jsx                                                                     | Automatizada  |
| RF-HOM-03 | El sistema debe permitir unirse a un hogar con su código de acceso, con el rol de miembro                  | JoinHomeServiceTest, HomeControllerTest                                                                                              | Automatizada  |
| RF-HOM-04 | El sistema debe mostrar los miembros de un hogar                                                           | ListMembersServiceTest, HomeControllerTest, Users.test.jsx                                                                           | Automatizada  |
| RF-HOM-05 | El sistema debe distinguir los permisos generales de los usuarios de los roles de propietario y miembro…   | UserHomeTest, OwnerPermissionsTest, MemberPermissionsTest, RoleTest, UserHomePersistenceTest, HomeOwnershipTransferIntegrationTest   | Automatizada  |
| RF-HOM-06 | El propietario debe poder retirar a un miembro y cualquier miembro debe poder abandonar el hogar           | RemoveUserServiceTest, LeaveHomeServiceTest, HomeControllerTest, Users.test.jsx                                                      | Automatizada  |
| RF-HOM-07 | El usuario debe poder marcar un hogar como favorito                                                        | ToggleFavoriteServiceTest, HomeControllerTest                                                                                        | Automatizada  |
| RF-HOM-08 | El sistema debe permitir consultar, actualizar y restablecer a los valores del sistema los umbrales de un… | GetHomeThresholdsServiceTest, UpdateHomeThresholdsServiceTest, HomeThresholdsTest, HomeThresholdsPersistenceTest, HomeControllerTest | Automatizada  |
| RF-HOM-09 | El propietario debe poder eliminar el hogar mediante eliminación lógica                                    | DeleteHomeServiceTest, HomeDeletionIntegrationTest, HomeControllerTest, DeleteHome.test.jsx                                          | Automatizada  |
| RF-HOM-10 | El propietario debe poder transferir la propiedad del hogar a otro miembro                                 | TransferHomeOwnershipServiceTest, HomeOwnershipTransferIntegrationTest, HomeControllerTest, Users.test.jsx                           | Automatizada  |
| RF-HOM-11 | Pendiente: limitar el historial de un dispositivo al periodo de vinculación                                | —                                                                                                                                    | Sin cobertura |

**Tabla 18**

*Trazabilidad de los requisitos RF-DEV*

| Requisito | Descripción                                                                                               | Pruebas automatizadas                                                                        | Cobertura     |
|-----------|-----------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------|---------------|
| RF-DEV-01 | El sistema debe ofrecer un catálogo de tipos de electrodoméstico                                          | —                                                                                            | Sin cobertura |
| RF-DEV-02 | El propietario debe poder vincular un dispositivo de medición a su hogar                                  | HomeDeviceServiceTest, HomeDeviceControllerTest, DeviceApiKeyStorageIntegrationTest          | Automatizada  |
| RF-DEV-03 | Los miembros deben poder ver los dispositivos de un hogar                                                 | HomeDeviceServiceTest, HomeDeviceControllerTest                                              | Automatizada  |
| RF-DEV-04 | El propietario debe poder editar el electrodoméstico, el nombre y la habitación de un dispositivo sin…    | HomeDeviceServiceTest, HomeDeviceControllerTest                                              | Automatizada  |
| RF-DEV-05 | El propietario debe poder cambiar la red Wi-Fi de un dispositivo ya vinculado                             | HomeDeviceServiceTest, HomeDeviceControllerTest                                              | Automatizada  |
| RF-DEV-06 | El propietario debe poder desvincular un dispositivo                                                      | HomeDeviceServiceTest, HomeDeviceControllerTest, HomeDeletionIntegrationTest                 | Automatizada  |
| RF-DEV-07 | El sistema solo debe aceptar datos de dispositivos vinculados, y cada dispositivo solo puede enviar sus…  | BrokerAccessServiceTest, BrokerAuthControllerTest, ApiKeyHashTest, TelemetryMqttConsumerTest | Automatizada  |
| RF-DEV-08 | La vinculación debe poder hacerse desde la aplicación web por Bluetooth: la aplicación muestra las redes… | —                                                                                            | Sin cobertura |
| RF-DEV-09 | El dispositivo debe reconectarse sin intervención del usuario ante la pérdida de Wi-Fi o una falla…       | —                                                                                            | Sin cobertura |

**Tabla 19**

*Trazabilidad de los requisitos RF-MED*

| Requisito | Descripción                                                                                                   | Pruebas automatizadas                                                                                                        | Cobertura     |
|-----------|---------------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------|---------------|
| RF-MED-01 | El dispositivo debe enviar cada 60 segundos el voltaje (V), la corriente (A), la potencia activa (W) y la…    | —                                                                                                                            | Sin cobertura |
| RF-MED-02 | El sistema debe guardar cada lectura con la fecha y hora en que se tomó. Las muestras acumuladas sin…         | RegisterMeasurementServiceTest, MeasurementTest, MeasurementPersistenceTest, TelemetryMqttConsumerTest                       | Automatizada  |
| RF-MED-03 | El sistema debe descartar los mensajes que declaren pertenecer a un dispositivo distinto del que los envía    | TelemetryMqttConsumerTest                                                                                                    | Automatizada  |
| RF-MED-04 | El dispositivo debe informar su estado cada cinco minutos y al conectarse: si está en línea o sin…            | —                                                                                                                            | Sin cobertura |
| RF-MED-05 | El sistema debe conservar los cambios de estado de cada dispositivo                                           | DeviceConnectivityServiceTest                                                                                                | Automatizada  |
| RF-MED-06 | El sistema debe marcar un dispositivo como sin conexión cuando el dispositivo avisa que se desconecta o…      | DeviceConnectivityServiceTest                                                                                                | Automatizada  |
| RF-MED-07 | El sistema debe permitir consultar las mediciones, la última medición y estadísticas de consumo               | ListMeasurementsServiceTest, GetLatestMeasurementServiceTest, GetConsumptionStatisticsServiceTest, MeasurementControllerTest | Automatizada  |
| RF-MED-08 | El sistema debe ofrecer el resumen y el historial de consumo de un hogar, con selección de un rango de fechas | HomeConsumptionUseCaseTest                                                                                                   | Automatizada  |
| RF-MED-09 | El sistema debe clasificar la potencia en los niveles LOW (0 a 500 W), MEDIUM (500 a 1500 W), HIGH (1500 a…   | ConsumptionLevelTest, ListConsumptionLevelsServiceTest, ConsumptionLevelControllerTest, ConsumptionLevelPersistenceTest      | Automatizada  |

**Tabla 20**

*Trazabilidad de los requisitos RF-ALE*

| Requisito | Descripción                                                                                                | Pruebas automatizadas                                                                                          | Cobertura    |
|-----------|------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------|--------------|
| RF-ALE-01 | El sistema debe generar un aviso de dispositivo vinculado con la primera lectura que envía un dispositivo… | RegisterDeviceLinkedAlertServiceTest                                                                           | Automatizada |
| RF-ALE-02 | El sistema debe generar una alerta de desconexión cuando un dispositivo avisa que se desconecta o pasan…   | DeviceConnectivityServiceTest                                                                                  | Automatizada |
| RF-ALE-03 | El sistema debe generar una alerta de consumo alto o crítico cuando una lectura cae en el nivel HIGH o…    | RegisterThresholdAlertServiceTest, AlertTest, MeasurementRecordedListenerTest                                  | Automatizada |
| RF-ALE-04 | El sistema debe generar una alerta de límite diario o mensual cuando la energía del hogar desde el inicio… | RegisterLimitAlertServiceTest, AlertTest                                                                       | Automatizada |
| RF-ALE-05 | El sistema debe permitir consultar las alertas, ver el detalle de una, marcarla como leída y eliminarla    | ListAlertsServiceTest, GetAlertServiceTest, ResolveAlertServiceTest, AlertControllerTest, AlertPersistenceTest | Automatizada |
| RF-ALE-06 | El sistema debe generar las alertas de consumo a partir de cada nueva lectura y las de desconexión a…      | MeasurementRecordedListenerTest                                                                                | Automatizada |

**Tabla 21**

*Trazabilidad de los requisitos RF-REC*

| Requisito | Descripción                                                                                                 | Pruebas automatizadas                                                                       | Cobertura    |
|-----------|-------------------------------------------------------------------------------------------------------------|---------------------------------------------------------------------------------------------|--------------|
| RF-REC-01 | El sistema debe evaluar cada hora la energía por hora de los hogares cuyos dispositivos reportaron en la…   | EvaluateRecommendationsServiceTest, RecommendationRulesTest                                 | Automatizada |
| RF-REC-02 | Regla de hora punta: al menos el 40 % de la energía de los últimos 7 días (mínimo 2 kWh) se consumió entre… | RecommendationRulesTest                                                                     | Automatizada |
| RF-REC-03 | Regla de consumo en reposo: un dispositivo que no sea un refrigerador consumió en promedio 15 W o más…      | RecommendationRulesTest                                                                     | Automatizada |
| RF-REC-04 | Regla de comparación semanal: los últimos 7 días superan en 25 % o más a los 7 anteriores (mínimo 1 kWh)    | RecommendationRulesTest                                                                     | Automatizada |
| RF-REC-05 | Regla de proyección de límite: al ritmo actual, el hogar superará su límite diario o mensual, el que haya…  | RecommendationRulesTest                                                                     | Automatizada |
| RF-REC-06 | Regla de aumento por dispositivo: la potencia media de un dispositivo en los últimos 7 días es 30 % o más…  | RecommendationRulesTest                                                                     | Automatizada |
| RF-REC-07 | La misma recomendación (hogar, tipo y dispositivo) no debe repetirse dentro de 7 días, aunque se haya…      | EvaluateRecommendationsServiceTest, RecommendationRulesTest                                 | Automatizada |
| RF-REC-08 | El sistema debe permitir consultar las recomendaciones de un hogar, marcarlas como leídas y eliminar las…   | RecommendationInboxServiceTest, RecommendationControllerTest, RecommendationPersistenceTest | Automatizada |
| RF-REC-09 | Las reglas requieren historial: la hora punta y el consumo en reposo funcionan tras unos días de lecturas,… | RecommendationRulesTest                                                                     | Automatizada |

**Tabla 22**

*Trazabilidad de los requisitos RF-NOT*

| Requisito | Descripción                                                                                                 | Pruebas automatizadas                                                                        | Cobertura     |
|-----------|-------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------|---------------|
| RF-NOT-01 | El sistema debe entregar cada alerta a todos los miembros del hogar por correo y por notificación del…      | NotifyAlertTest, NotificationTest, WebPushSenderTest, EmailSenderTest                        | Automatizada  |
| RF-NOT-02 | El sistema debe entregar cada recomendación nueva por los mismos canales                                    | —                                                                                            | Sin cobertura |
| RF-NOT-03 | El usuario debe poder consultar y modificar sus preferencias de notificación                                | —                                                                                            | Sin cobertura |
| RF-NOT-04 | El usuario debe poder activar y desactivar las notificaciones del navegador                                 | WebPushSenderTest                                                                            | Automatizada  |
| RF-NOT-05 | Cada intento de entrega debe quedar registrado como enviado o fallido, sin guardar el contenido del mensaje | NotificationTest, NotificationPersistenceTest, AbandonedDeliverySweeperTest, EmailSenderTest | Automatizada  |
| RF-NOT-06 | El sistema debe enviar por correo el código de verificación y el de recuperación de contraseña              | SendPasswordResetEmailTest, EmailPasswordResetDeliveryAdapterTest, EmailSenderTest           | Automatizada  |
| RF-NOT-07 | Cada correo de alerta debe incluir un enlace a la aplicación web                                            | NotifyAlertTest                                                                              | Automatizada  |

**Tabla 23**

*Trazabilidad de los requisitos RF-CLI*

| Requisito | Descripción                                                                                                 | Pruebas automatizadas                                                                                               | Cobertura     |
|-----------|-------------------------------------------------------------------------------------------------------------|---------------------------------------------------------------------------------------------------------------------|---------------|
| RF-CLI-01 | La aplicación web debe ofrecer registro, inicio de sesión (incluido Google), recuperación de contraseña,…   | Account.test.jsx, DashboardPage.test.jsx, DeleteHome.test.jsx, Users.test.jsx, verify.test.jsx, recover.test.jsx, … | Automatizada  |
| RF-CLI-02 | La aplicación web debe mostrar una insignia con las alertas pendientes, un contador en el título de la…     | —                                                                                                                   | Sin cobertura |
| RF-CLI-03 | La aplicación web debe vincular dispositivos por Bluetooth y permitir notificaciones del navegador, que…    | —                                                                                                                   | Sin cobertura |
| RF-CLI-04 | La aplicación móvil debe estar disponible solo para Android y permitir el acceso a la cuenta, los hogares,… | —                                                                                                                   | Sin cobertura |
| RF-CLI-05 | Ambas aplicaciones deben permitir elegir el idioma entre español, inglés, francés y portugués               | —                                                                                                                   | Sin cobertura |
| RF-CLI-06 | Las aplicaciones deben permitir elegir una paleta de colores con modo oscuro                                | —                                                                                                                   | Sin cobertura |

**Tabla 24**

*Trazabilidad de los requisitos RNF-SEG*

| Requisito  | Descripción                                                                                                | Pruebas automatizadas                                                                                                   | Cobertura     |
|------------|------------------------------------------------------------------------------------------------------------|-------------------------------------------------------------------------------------------------------------------------|---------------|
| RNF-SEG-01 | Cada acceso concedido a un usuario debe durar 15 minutos y renovarse automáticamente mientras la sesión…   | JwtTokenIssuerTest, RefreshSessionServiceTest                                                                           | Automatizada  |
| RNF-SEG-02 | Si se intenta reutilizar una credencial de renovación ya usada, el sistema debe invalidar la sesión        | RefreshTokenTest, RefreshSessionServiceTest, RefreshLifecycleIntegrationTest                                            | Automatizada  |
| RNF-SEG-03 | El sistema solo debe atender las solicitudes de las aplicaciones web autorizadas                           | SecurityCorsConfigurationTest, SecurityCorsWebTest                                                                      | Automatizada  |
| RNF-SEG-04 | Los datos que envían los dispositivos de medición deben viajar cifrados                                    | —                                                                                                                       | Sin cobertura |
| RNF-SEG-05 | Los códigos de recuperación deben almacenarse de forma protegida, de modo que una filtración de los datos… | PepperedHmacResetCodeHasherTest                                                                                         | Automatizada  |
| RNF-SEG-06 | El canje de códigos de recuperación debe limitarse a 10 intentos por dirección y por ventana de 15…        | InMemoryResetAttemptLimiterTest, ResetAttemptLimiterBeforeDatabaseTest, PoolSaturatedLimiterTest, AuthControllerWebTest | Automatizada  |
| RNF-SEG-07 | Las funciones internas con las que se autentica a los dispositivos solo deben ser accesibles desde la red… | —                                                                                                                       | Sin cobertura |
| RNF-SEG-08 | El sistema debe proteger la información de los usuarios conforme a la Ley 1581 de 2012 (Congreso de la…    | —                                                                                                                       | Sin cobertura |
| RNF-SEG-09 | Las credenciales de los dispositivos deben comprobarse de forma segura                                     | ApiKeyHashTest, DeviceApiKeyStorageIntegrationTest, BrokerAccessServiceTest                                             | Automatizada  |
| RNF-SEG-10 | Las contraseñas y claves del sistema no deben quedar expuestas en los archivos compartidos del proyecto    | —                                                                                                                       | Sin cobertura |

**Tabla 25**

*Trazabilidad de los requisitos RNF-REN*

| Requisito  | Descripción                                                                                                  | Pruebas automatizadas | Cobertura     |
|------------|--------------------------------------------------------------------------------------------------------------|-----------------------|---------------|
| RNF-REN-01 | Las consultas de datos y las gráficas deben responder en menos de 2 segundos                                 | —                     | Sin cobertura |
| RNF-REN-02 | El sistema debe procesar la información de cada dispositivo con una cadencia de una lectura cada 60 segundos | —                     | Sin cobertura |
| RNF-REN-03 | El envío de correos no debe dejar esperando al usuario más de 5 segundos                                     | —                     | Sin cobertura |

**Tabla 26**

*Trazabilidad de los requisitos RNF-DIS*

| Requisito  | Descripción                                                                                              | Pruebas automatizadas         | Cobertura     |
|------------|----------------------------------------------------------------------------------------------------------|-------------------------------|---------------|
| RNF-DIS-01 | [POR CONFIRMAR] porcentaje de disponibilidad objetivo                                                    | —                             | Sin cobertura |
| RNF-DIS-02 | El sistema debe tolerar la pérdida ocasional de lecturas                                                 | DeviceConnectivityServiceTest | Automatizada  |
| RNF-DIS-03 | Si el sistema no está disponible, el dispositivo debe seguir midiendo y conservar hasta 30 muestras (30… | —                             | Sin cobertura |
| RNF-DIS-04 | El servicio que recibe los datos de los dispositivos debe reintentar conectarse con el sistema cada 10…  | —                             | Sin cobertura |
| RNF-DIS-05 | Si no hay Wi-Fi durante 10 minutos, el dispositivo debe reiniciarse                                      | —                             | Sin cobertura |
| RNF-DIS-06 | El sistema debe permitir consultar su estado de salud                                                    | —                             | Sin cobertura |

**Tabla 27**

*Trazabilidad de los requisitos RNF-COM*

| Requisito  | Descripción                                                                                                | Pruebas automatizadas | Cobertura     |
|------------|------------------------------------------------------------------------------------------------------------|-----------------------|---------------|
| RNF-COM-01 | La aplicación web debe funcionar en los navegadores actuales (Chrome, Firefox, Safari y Edge)              | —                     | Sin cobertura |
| RNF-COM-02 | La vinculación por Bluetooth desde la web solo está disponible en Chrome y Edge de escritorio y en Chrome… | —                     | Sin cobertura |
| RNF-COM-03 | La aplicación móvil debe ejecutarse en dispositivos Android                                                | —                     | Sin cobertura |
| RNF-COM-04 | El dispositivo de medición debe incluir un sensor de medición eléctrica dedicado, con un solo sensor por…  | —                     | Sin cobertura |

**Tabla 28**

*Trazabilidad de los requisitos RNF-MAN*

| Requisito  | Descripción                                                                                              | Pruebas automatizadas                                                 | Cobertura                                   |
|------------|----------------------------------------------------------------------------------------------------------|-----------------------------------------------------------------------|---------------------------------------------|
| RNF-MAN-01 | El sistema debe organizarse en módulos funcionales (Gestión de Usuarios y Perfiles, Gestión de Hogares,… | ModularityTests                                                       | Automatizada (requiere MySQL, por ejecutar) |
| RNF-MAN-02 | Todo cambio en la estructura de los datos debe poder revertirse                                          | —                                                                     | Sin cobertura                               |
| RNF-MAN-03 | Los datos eliminados deben conservarse marcados como eliminados y no borrarse físicamente                | HomePersistenceTest, UserPersonPersistenceTest, DeleteHomeServiceTest | Automatizada                                |
| RNF-MAN-04 | El sistema debe contar con pruebas automáticas que verifiquen sus funciones principales                  | —                                                                     | Sin cobertura                               |

**Tabla 29**

*Trazabilidad de los requisitos RNF-I18*

| Requisito  | Descripción                                                                                  | Pruebas automatizadas | Cobertura     |
|------------|----------------------------------------------------------------------------------------------|-----------------------|---------------|
| RNF-I18-01 | La aplicación web debe estar disponible en español, inglés, francés y portugués              | —                     | Sin cobertura |
| RNF-I18-02 | La aplicación móvil debe estar disponible en español, inglés, francés y portugués            | —                     | Sin cobertura |
| RNF-I18-03 | Las alertas y las recomendaciones deben mostrarse en el idioma que haya elegido cada usuario | —                     | Sin cobertura |

**Tabla 30**

*Trazabilidad de los requisitos RNF-ESC*

| Requisito  | Descripción                                                                                                 | Pruebas automatizadas | Cobertura     |
|------------|-------------------------------------------------------------------------------------------------------------|-----------------------|---------------|
| RNF-ESC-01 | El sistema debe poder crecer en número de usuarios y de dispositivos sin afectar la experiencia del usuario | —                     | Sin cobertura |
| RNF-ESC-02 | Las cantidades objetivo de usuarios y dispositivos concurrentes son [POR CONFIRMAR]                         | —                     | Sin cobertura |

**Tabla 31**

*Trazabilidad de los requisitos RNF-USA*

| Requisito  | Descripción                                                                                               | Pruebas automatizadas | Cobertura     |
|------------|-----------------------------------------------------------------------------------------------------------|-----------------------|---------------|
| RNF-USA-01 | La interfaz debe ser clara, adaptable a distintos tamaños de pantalla y coherente, con retroalimentación… | —                     | Sin cobertura |

**Tabla 32**

*Trazabilidad de los requisitos RNF-INT*

| Requisito  | Descripción                                                                                                 | Pruebas automatizadas       | Cobertura     |
|------------|-------------------------------------------------------------------------------------------------------------|-----------------------------|---------------|
| RNF-INT-01 | Los dispositivos de medición deben comunicarse con el sistema mediante conexiones iniciadas desde el…       | —                           | Sin cobertura |
| RNF-INT-02 | El sistema debe enviar correos mediante una cuenta de correo configurada para ese fin                       | EmailSenderTest             | Automatizada  |
| RNF-INT-03 | El sistema debe permitir el acceso con cuentas de Google solicitando únicamente el nombre, el correo y la…  | SignInWithGoogleServiceTest | Automatizada  |
| RNF-INT-04 | Las notificaciones del navegador solo deben funcionar en una conexión segura y requieren que el sistema…    | —                           | Sin cobertura |
| RNF-INT-05 | La configuración del dispositivo por Bluetooth debe estar disponible hasta 10 minutos, no debe interrumpir… | —                           | Sin cobertura |

Los requisitos sin cobertura son vacíos conocidos (ver riesgos y limitaciones). La administración de cuentas no tiene requisito en la especificación (DOC-01).

## Resultados

### Backend sin base de datos

Se ejecutó `./mvnw -o test` limitado a las clases que no levantan contexto con base de datos (88 clases existentes) más las 4 clases nuevas. Los totales por módulo salen de target/surefire-reports y se detallan en [resumen-surefire.md](../../evidencias/ES/backend/resumen-surefire.md).

**Tabla 33**

*Resultados del backend por módulo*

| Módulo         | Clases | Ejecutadas | Aprobadas | Fallidas | Omitidas | Tiempo (s) |
|----------------|--------|------------|-----------|----------|----------|------------|
| alert          | 9      | 48         | 48        | 0        | 0        | 1.2        |
| device         | 6      | 48         | 48        | 0        | 0        | 2.0        |
| home           | 23     | 159        | 159       | 0        | 0        | 1.5        |
| measurement    | 11     | 67         | 67        | 0        | 0        | 0.8        |
| notification   | 8      | 60         | 60        | 0        | 0        | 1.4        |
| recommendation | 4      | 23         | 22        | 1        | 0        | 0.3        |
| security       | 31     | 430        | 429       | 1        | 0        | 18.8       |
| Total          | 92     | 835        | 833       | 2        | 0        | 26.0       |

No se ejecutaron 37 clases que necesitan MySQL (270 métodos declarados) porque la variable SPRING_DATASOURCE_PASSWORD no estaba definida en el entorno de ejecución. Son las pruebas de persistencia, las de integración, las web con contexto completo, ModularityTests y las de seguridad con contexto; su estado es «Por ejecutar (MySQL)».

### Aplicación web

Se ejecutó `npm test`: 20 archivos y 141 pruebas, todas aprobadas ([resumen-vitest.md](../../evidencias/ES/web/resumen-vitest.md)).

### Pruebas nuevas

Se escribieron 4 clases para requisitos críticos sin ninguna cobertura automatizada. Siguen el estilo de las pruebas existentes (@WebMvcTest con @MockitoBean, o Mockito puro) y la estructura AAA.

**Tabla 34**

*Pruebas nuevas y por qué se escribieron*

| Clase                        | Pruebas | Aprobadas | Fallidas | Motivo                                                                                                   | Requisitos            |
|------------------------------|---------|-----------|----------|----------------------------------------------------------------------------------------------------------|-----------------------|
| BrokerAuthControllerTest     | 12      | 12        | 0        | Autenticación y autorización de dispositivos ante el broker (seguridad); no había prueba del controlador | RF-DEV-07, RNF-SEG-09 |
| HomeDeviceControllerTest     | 10      | 10        | 0        | Vinculación y credenciales de dispositivos; solo el caso de uso tenía prueba                             | RF-DEV-02 a RF-DEV-06 |
| RecommendationControllerTest | 7       | 6         | 1        | Bandeja de recomendaciones; el controlador no tenía pruebas                                              | RF-REC-08             |
| TelemetryMqttConsumerTest    | 10      | 10        | 0        | Ingesta de lecturas y descarte de mensajes ajenos; el consumidor MQTT no tenía pruebas                   | RF-MED-02, RF-MED-03  |

### Defectos y observaciones

**Tabla 35**

*Defectos y observaciones encontrados*

| ID     | Severidad | Elemento                                                       | Descripción                                                                                                                                                                                                                                                                                                            | Estado  |
|--------|-----------|----------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|---------|
| DEF-01 | Baja      | SecuritySupportModelTest.refusesMissingNames                   | La prueba exige que Person rechace un apellido nulo, pero el código lo hizo opcional a propósito (changeset security-023: una cuenta de Google puede no tener apellido). La prueba quedó desactualizada; el código es correcto. No se modificó.                                                                        | Abierto |
| DEF-02 | Media     | RecommendationControllerTest.listRequiresHomeId (prueba nueva) | Un parámetro obligatorio ausente (por ejemplo homeId en GET /api/v1/recommendations o /api/v1/alerts) responde 500 en lugar de 400: SecurityExceptionHandler declara @ExceptionHandler(Exception.class) con alcance global y atrapa MissingServletRequestParameterException. La prueba se dejó fallando; no se ajustó. | Abierto |
| DOC-01 | Baja      | Documentación                                                  | La administración de cuentas (rutas /api/v1/admin, permisos USER.READ, USER.MANAGE y ROLE.MANAGE, pantalla /admin/users y notificaciones de administrador) existe en el código pero no tiene requisito en la especificación de requisitos ni está descrita en los manuales técnico y de administrador.                 | Abierto |

### Evidencias

Las evidencias están en [evidencias](../../evidencias/ES/README.md): resumen de Surefire, log recortado de la ejecución y resumen de Vitest. No contienen secretos.

## Riesgos, limitaciones y vacíos conocidos

**Tabla 36**

*Riesgos y limitaciones de las pruebas*

| ID   | Riesgo o limitación                                                                                                                                                                                                                                                                                                                                                 | Efecto                                                                      | Recomendación                                                           |
|------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|-----------------------------------------------------------------------------|-------------------------------------------------------------------------|
| R-01 | Pruebas no herméticas: persistencia y contexto completo necesitan MySQL real y SPRING_DATASOURCE_PASSWORD                                                                                                                                                                                                                                                           | 37 clases no se ejecutaron en esta entrega                                  | Ejecutar la suite completa antes de la entrega y evaluar Testcontainers |
| R-02 | La aplicación móvil no tiene pruebas automatizadas                                                                                                                                                                                                                                                                                                                  | Sin verificación automatizada                                               | Agregar pruebas de componentes y de servicios                           |
| R-03 | MQTT con QoS 0                                                                                                                                                                                                                                                                                                                                                      | Una lectura puede perderse; no se prueba la pérdida                         | Evaluar QoS 1 y agregar una prueba de pérdida                           |
| R-04 | Sin actualización por aire (OTA)                                                                                                                                                                                                                                                                                                                                    | El firmware se actualiza solo por USB                                       | Documentar el procedimiento de carga por USB                            |
| R-05 | Web Bluetooth limitado a Chrome, Edge y Chrome para Android, y a HTTPS o localhost                                                                                                                                                                                                                                                                                  | No se verifica de forma automatizada, tampoco el portal EnergyMonitor-Setup | Evaluar pruebas de extremo a extremo con navegador                      |
| R-06 | El contador de intentos de recuperación vive en memoria                                                                                                                                                                                                                                                                                                             | Se reinicia con el servicio y no se comparte entre instancias               | Limitador compartido si se escala                                       |
| R-07 | Sin pruebas de rendimiento ni de accesibilidad                                                                                                                                                                                                                                                                                                                      | RNF-REN y RNF-USA-01 sin cobertura                                          | Definir las metas y medirlas                                            |
| R-08 | Sin informe de cobertura (JaCoCo no está en el pom.xml)                                                                                                                                                                                                                                                                                                             | No se conoce el porcentaje de cobertura                                     | Agregar JaCoCo (no se agregó sin consultar)                             |
| R-09 | Vacíos sin prueba automatizada: StatusMqttConsumer, DeviceInactivityScheduler, HomeDeletedListener, DeviceCommandListener, MqttDeviceCommandAdapter, RecommendationScheduler, NotificationChannelsController, HomeConsumptionController, ApplianceTypeController, RegisterConnectivityAlertService, DeviceConnectivityLostListener, BrokerAuthSecurityConfiguration | Sin cobertura automatizada directa                                          | Priorizar conectividad y alertas de desconexión                         |

## Conclusiones y recomendaciones

El servicio central cuenta con una suite amplia y rápida: 835 pruebas ejecutadas sin base de datos en 26 segundos, con 2 fallas que no son del código de producción (DEF-01) o que apuntan a un defecto de manejo de errores (DEF-02). La web aprueba sus 141 pruebas. Falta ejecutar las 37 clases con MySQL para cerrar la verificación. Recomendaciones: (1) definir SPRING_DATASOURCE_PASSWORD y ejecutar `./mvnw clean test` completo; (2) corregir DEF-02 con un manejador de MissingServletRequestParameterException; (3) actualizar la prueba de DEF-01; (4) incorporar Testcontainers para hacer herméticas las pruebas de persistencia; (5) agregar pruebas a la aplicación móvil; (6) incorporar JaCoCo y una integración continua; (7) escribir los requisitos de administración y sus pruebas.

## Referencias

Cortés Parra, J. E., & García Artunduaga, M. A. (2026). *Especificación de requisitos de software de EnergyMonitor* (versión 2.0). SENA – CIES Regional Huila.

Institute of Electrical and Electronics Engineers. (2008). *IEEE standard for software and system test documentation* (IEEE Std 829-2008). https://doi.org/10.1109/IEEESTD.2008.4578383

ISO/IEC/IEEE. (2021). *Software and systems engineering — Software testing — Part 3: Test documentation* (ISO/IEC/IEEE 29119-3:2021). International Organization for Standardization.
