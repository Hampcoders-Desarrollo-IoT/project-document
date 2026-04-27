# Capítulo IV: Solution Software Design

## 4.1 Strategic-Level Domain-Driven Design.
### 4.1.1. Design-Level EventStorming.

#### 4.1.1.1 Candidate Context Discovery.
#### 4.1.1.2 Domain Message Flows Modeling.
#### 4.1.1.3 Bounded Context Canvases.

En esta sección se presentan los bounded contexts identificados para la solución ElectroLink, definidos a partir del análisis del dominio y siguiendo un enfoque de Domain-Driven Design (DDD). Cada contexto delimita responsabilidades claras, lenguaje ubicuo y reglas de negocio específicas, permitiendo una adecuada separación de preocupaciones y escalabilidad del sistema.

## 1. Identity and Access Management (IAM)
Gestión de autenticación, autorización y control de acceso de usuarios al sistema, incluyendo registro, inicio de sesión y manejo de roles.

![](assets/img/cap4/IAM-bd.PNG)

---

## 2. Subscription and Payments
Gestión de planes, facturación y control de acceso a funcionalidades.

---

![](assets/img/cap4/bd-subscription.PNG)


---

## 3. Profiles and Preferences
Administración de perfiles de usuarios, técnicos y configuración personalizada.

---

![](assets/img/cap4/bd-profiles.PNG)


---

## 4. Service Design and Planning
Orquestación de servicios, solicitudes y asignación inteligente de técnicos.

---

![](assets/img/cap4/service-desing-bd.PNG)

---

## 5. Service Operation and Monitoring
Ejecución, seguimiento y cierre de servicios con evidencia y evaluación.

---

![](assets/img/cap4/bd-service-operation.PNG)


---

## 6. Assets and Resource Management
Gestión de propiedades, dispositivos IoT e inventario de técnicos.

---

![](assets/img/cap4/bd-assets.PNG)

---

## 7. IoT Monitoring and Edge Processing
Procesamiento de datos en tiempo real y detección de anomalías eléctricas.

---

![](assets/img/cap4/bd-iot.PNG)

---

## 8. Analytics
Visualización, reportes e insights a partir de datos históricos y en tiempo real.

---

![](assets/img/cap4/bd-analytics.PNG)


---

#### 4.1.2. Context Mapping.
\
El Context Mapping es una técnica esencial en el diseño de ElectroLink que nos permite visualizar las relaciones estructurales y de comunicación entre los ocho Bounded Contexts identificados en el dominio de la gestión eléctrica inteligente. A través de esta técnica, hemos identificado las interacciones, dependencias y posibles puntos de integración entre los contextos, asegurando que el flujo de información desde los sensores hasta la toma de decisiones proactivas sea consistente.
\
En el desarrollo de nuestro proyecto, el proceso se estructuró siguiendo las fases metodológicas del diseño guiado por el dominio:
\
**Identificación de Relaciones:** Se comenzó por definir las interdependencias entre contextos, estableciendo roles de Upstream (U) y Downstream (D). Un ejemplo crítico es la relación entre IoT Monitoring (Upstream) y Service Design (Downstream), donde los eventos de anomalías dictan el comportamiento proactivo del sistema.

**Anticorruption Layer (ACL):** Aplicada en Service Design para proteger el algoritmo de asignación técnica de cambios en los modelos de activos o perfiles.

**Shared Kernel:** Utilizado entre Service Operation y Assets para gestionar el estado compartido de los dispositivos instalados en tiempo real.

**Open Host Service (OHS):** El contexto de IoT Monitoring expone una interfaz estandarizada para el control seguro de relés eléctricos.

**Customer/Supplier:** Establecido entre Profiles e IoT Monitoring, donde los umbrales configurados por el cliente guían la detección de anomalías.

**Conformist:** El BC de Analytics se adhiere a los contratos de datos de telemetría impuestos por la ingesta de dispositivos para garantizar reportes precisos.

A continuación, se presenta el Context Map elegido que resume visualmente estas relaciones y sirve como hoja de ruta para la implementación técnica de la solución:

![](assets/img/cap4/Context-Mapping-Electrolink.jpg)

#### 4.1.3. Software Architecture.

##### 4.1.3.1. Candidate Context Discovery.
![](assets/img/cap2/CCD.png)

##### 4.1.3.1. Software Architecture System Landscape Diagram.

![](assets/img/cap4/c4-model/SystemContext.png)

##### 4.1.3.2. Software Architecture Context Level Diagrams.

![](assets/img/cap4/c4-model/SystemContext.png)

##### 4.1.3.3. Software Architecture Container Level Diagrams.

![](assets/img/cap4/c4-model/Containers.png)

##### 4.1.3.4. Software Architecture Deployment Diagrams.

## 4.2. Tactical-Level Domain-Driven Design

### 4.2.1. Bounded Context: Identity & Access Management
#### 4.2.1.1. Domain Layer.

En esta capa se describen las clases que representan el núcleo del dominio del contexto de Identity and Access Management. Se incluyen el agregado raíz, los objetos de valor, los comandos y consultas bajo el patrón CQRS, así como los contratos para servicios de dominio e infraestructura.

**Value Objects**

Representan conceptos descriptivos del dominio sin identidad conceptual propia, garantizando la validación e inmutabilidad de los datos.

| Objeto de Valor | Descripción |
|-----------------|-------------|
| `UserId` | Identificador único de la cuenta de usuario, generado internamente (UUID v4). |
| `Username` | Encapsula la validación de formato de correo electrónico para el nombre de usuario. |
| `HashedPassword` | Encapsula el hash de la contraseña y garantiza el cumplimiento de la política de seguridad (longitud, caracteres especiales). |
| `VerificationToken` | Token único con tiempo de vida (TTL) de 24 horas para la validación del correo electrónico. |
| `PasswordResetToken` | Token único con tiempo de vida (TTL) de 1 hora para la recuperación de contraseña. |
| `UserStatus` | Enumeración que define el estado del ciclo de vida de la cuenta (`PendingVerification`, `Verified`). |
| `FailedLoginAttempts` | Encapsula el conteo de intentos fallidos de inicio de sesión de manera inmutable. |

**Aggregates**

**User**

Actúa como el único *Aggregate Root* de este contexto. Gestiona la identidad digital del usuario, sus credenciales, el ciclo de vida de la cuenta y los tokens de seguridad asociados.

| Atributo | Tipo |
|----------|------|
| Id | UserId |
| Username | Username |
| PasswordHash | HashedPassword |
| Status | UserStatus |
| VerificationToken | VerificationToken |
| PasswordResetToken | PasswordResetToken |
| FailedAttempts | FailedLoginAttempts |
| LockedUntil | DateTime? |

| Método | Descripción |
|--------|-------------|
| `Register` | Factory method que inicializa un usuario, validando políticas de seguridad y generando el token de verificación. |
| `VerifyEmail` | Valida el token proporcionado y transiciona el estado de la cuenta a `Verified`. |
| `RecordSuccessfulLogin` | Restablece los contadores de intentos fallidos tras una autenticación exitosa. |
| `RecordFailedLogin` | Incrementa los intentos fallidos y bloquea temporalmente la cuenta si se excede el límite máximo permitido. |
| `IsLockedOut` | Evalúa si la cuenta se encuentra en periodo de bloqueo temporal. |
| `RequestPasswordReset` | Genera un token de recuperación para usuarios verificados. |
| `ResetPassword` | Restablece la contraseña validando el token de recuperación emitido. |
| `ChangePassword` | Modifica la contraseña actual requiriendo autenticación previa. |

**Commands**

Representan la intención de modificar el estado del sistema.

| Clase | Descripción |
|-------|-------------|
| `RegisterUserCommand` | Representa la intención de registrar un nuevo usuario en el sistema. |
| `VerifyEmailCommand` | Representa la intención de validar el correo electrónico de una cuenta pendiente. |
| `LoginCommand` | Representa la intención de autenticarse en el sistema. |
| `LogoutCommand` | Representa la intención de cerrar la sesión activa de un usuario. |
| `RequestPasswordResetCommand` | Representa la intención de solicitar la recuperación de contraseña. |
| `ResetPasswordCommand` | Representa la intención de establecer una nueva contraseña mediante un token válido. |
| `ChangePasswordCommand` | Representa la intención de modificar la contraseña desde una sesión autenticada. |

**Queries**

Representan la intención de consultar el estado del sistema sin producir efectos secundarios.

| Clase | Descripción |
|-------|-------------|
| `GetUserByIdQuery` | Consulta para recuperar los datos de un usuario mediante su identificador único. |
| `GetUserByUsernameQuery` | Consulta para recuperar los datos de un usuario mediante su nombre de usuario (correo electrónico). |
| `GetAllUsersQuery` | Consulta para listar todos los usuarios registrados en el sistema. |

**Domain Services (Interfaces)**

| Interfaz | Descripción |
|----------|-------------|
| `IUserCommandService` | Define el contrato para la ejecución de operaciones que alteran el estado del agregado `User`. |
| `IUserQueryService` | Define el contrato para la ejecución de consultas sobre el agregado `User`. |

**Repositories (Interfaces)**

| Interfaz | Descripción |
|----------|-------------|
| `IUserRepository` | Contrato de persistencia para el agregado `User`, definiendo métodos de acceso y búsqueda específicos. |

---

#### 4.2.1.2. Interface Layer.

En esta capa se definen las clases responsables de manejar la interacción externa mediante solicitudes HTTP, transformando los datos recibidos (Resources) en comandos estructurados y viceversa.

**Resources**

Actúan como Objetos de Transferencia de Datos (DTOs) que estructuran el contrato de comunicación con los clientes de la API.

| Clase | Descripción |
|-------|-------------|
| `RegisterUserResource` | Recibe los datos necesarios para el registro de un nuevo usuario. |
| `VerifyEmailResource` | Recibe el identificador del usuario y el token para la verificación del correo. |
| `LoginResource` | Recibe las credenciales para la autenticación en el sistema. |
| `AuthenticatedUserResource` | Devuelve los datos del usuario autenticado junto con su token de sesión (JWT). |
| `RequestPasswordResetResource` | Recibe el nombre de usuario para iniciar el flujo de recuperación de contraseña. |
| `ResetPasswordResource` | Recibe el token de recuperación y la nueva contraseña confirmada. |
| `ChangePasswordResource` | Recibe la contraseña actual y la nueva contraseña a establecer. |
| `UserResource` | Devuelve la información pública y segura de una cuenta de usuario. |

**Transforms / Assemblers**

Se encargan de mapear los recursos de entrada hacia los comandos del dominio, y las entidades del dominio hacia recursos de salida.

| Clase | Descripción |
|-------|-------------|
| `RegisterUserCommandFromResourceAssembler` | Transforma un `RegisterUserResource` en un `RegisterUserCommand`. |
| `LoginCommandFromResourceAssembler` | Transforma un `LoginResource` en un `LoginCommand`. |
| `ChangePasswordCommandFromResourceAssembler` | Transforma un `ChangePasswordResource` en un `ChangePasswordCommand`. |
| `AuthenticatedUserResourceFromEntityAssembler` | Mapea la entidad `User` y el token generado hacia un `AuthenticatedUserResource`. |
| `UserResourceFromEntityAssembler` | Mapea la entidad `User` hacia un `UserResource` para exposición de lectura. |

**Anti-Corruption Layer (ACL) / Facade**

| Interfaz | Implementación | Descripción |
|----------|----------------|-------------|
| `IIamContextFacade` | `IamContextFacade` | Expone un contrato estricto para que otros Bounded Contexts (ej. Profiles) consulten datos de IAM sin acoplarse a su modelo interno. |

**Controllers**

Controladores REST que exponen los endpoints de forma pública o privada según la política de autorización.

**AuthenticationController**

| Ruta Específica | Método | Descripción |
|-----------------|--------|-------------|
| `/api/v1/authentication/sign-up` | `POST` | Gestiona el registro de nuevos usuarios en el sistema. |
| `/api/v1/authentication/verify-email` | `POST` | Gestiona la validación del correo electrónico mediante token. |
| `/api/v1/authentication/sign-in` | `POST` | Autentica al usuario y retorna un token JWT válido. |
| `/api/v1/authentication/sign-out` | `POST` | Cierra la sesión activa del usuario autenticado. |
| `/api/v1/authentication/forgot-password`| `POST` | Inicia el proceso de recuperación de contraseña. |
| `/api/v1/authentication/reset-password` | `POST` | Confirma y aplica el restablecimiento de una contraseña olvidada. |

**UsersController**

| Ruta Específica | Método | Descripción |
|-----------------|--------|-------------|
| `/api/v1/users/{id}` | `GET` | Retorna los detalles de la cuenta de un usuario específico. |
| `/api/v1/users/{id}/password` | `PUT` | Permite a un usuario autenticado cambiar su contraseña actual. |

---

#### 4.2.1.3. Application Layer.

Esta capa orquesta el flujo de las reglas de negocio, coordinando los repositorios, la publicación de eventos de dominio y la delegación de responsabilidades a los servicios externos.

**Command Services**

| Clase | Interfaz Implementada | Descripción |
|-------|-----------------------|-------------|
| `UserCommandService` | `IUserCommandService` | Gestiona la ejecución transaccional de comandos (registro, login, validación), la publicación de eventos en el mediador y la persistencia mediante Unit of Work. |

**Query Services**

| Clase | Interfaz Implementada | Descripción |
|-------|-----------------------|-------------|
| `UserQueryService` | `IUserQueryService` | Implementa el manejo de consultas aislando la lectura de datos para evitar impactos en el estado del agregado. |

**Outbound Services (Interfaces)**

Puertos definidos en la capa de aplicación que serán implementados por la infraestructura para integraciones externas.

| Interfaz | Descripción |
|----------|-------------|
| `IHashingService` | Contrato para el cifrado y validación de contraseñas (implementado mediante BCrypt). |
| `ITokenService` | Contrato para la generación y validación de tokens JSON Web Token (JWT). |
| `IEmailNotificationService` | Contrato para el envío de notificaciones por correo (verificación, restablecimiento de contraseña, alertas de bloqueo). |

---

#### 4.2.1.4. Infrastructure Layer.

En esta capa se ubican las implementaciones técnicas que permiten la persistencia de datos y la comunicación con servicios de terceros.

**Persistencia y Configuración (Entity Framework Core)**

| Clase | Descripción |
|-------|-------------|
| `UserConfiguration` | Define el mapeo objeto-relacional (ORM) del agregado `User` hacia la tabla `iam_users`. Establece la conversión de *Value Objects* a tipos primitivos y configura tokens como entidades en propiedad (*Owned Entities*) mapeadas en la misma tabla. |

**Implementación de Repositorios**

| Clase | Interfaz Implementada | Descripción |
|-------|-----------------------|-------------|
| `UserRepository` | `IUserRepository` | Implementa las consultas hacia la base de datos utilizando Entity Framework Core, resolviendo la búsqueda por nombre de usuario, tokens específicos y persistencia general del agregado. |

---

#### 4.2.1.5. Bounded Context Software Architecture Component Level Diagrams.
#### 4.2.1.6. Bounded Context Software Architecture Code Level Diagrams.

##### 4.2.1.6.1. Bounded Context Domain Layer Class Diagrams.

A continuación, se presenta el diagrama de clases de la capa de dominio generado en PlantUML, mostrando el agregado principal y sus objetos de valor correspondientes.

##### 4.2.1.6.2. Bounded Context Database Design Diagram.

A continuación, se presenta el diagrama Entidad-Relación (ERD) físico de la tabla gestionada en este Bounded Context. Se emplea el prefijo iam_ para garantizar el aislamiento a nivel de base de datos.

### 4.2.2. Bounded Context: Profiles & Preferences Management

#### 4.2.2.1. Domain Layer.

En esta capa se definen las clases que constituyen el núcleo del dominio del Bounded Context de Profiles & Preferences. Se incluyen el Aggregate Root, las entidades internas, los Value Objects, los contratos de servicio de dominio y las interfaces de repositorio. El diseño sigue el patrón CQRS (Command Query Responsibility Segregation) y los principios de Domain-Driven Design.

---

### Aggregate Root

#### `Profile`

Representa el perfil de negocio de un usuario registrado en el sistema IAM. Es el punto de entrada único al Bounded Context y el responsable de coordinar el ciclo de vida del perfil: `INCOMPLETE → ACTIVE → DEACTIVATED`. Contiene, como entidades internas, a `TechnicianProfile` o `ClientProfile` según el rol de negocio asignado al completar el perfil. El `Profile` nunca conoce credenciales ni lógica de autenticación; su única referencia a IAM es el Value Object `UserId`.

**Atributos**

| Atributo             | Tipo                   | Descripción                                                                 |
|----------------------|------------------------|-----------------------------------------------------------------------------|
| `Id`                 | `ProfileId`            | Identificador único del perfil con prefijo `prof-`.                         |
| `UserId`             | `UserId`               | Referencia lógica al usuario en IAM BC. Sin JOIN directo.                   |
| `Status`             | `ProfileStatus`        | Estado del ciclo de vida: `Incomplete`, `Active` o `Deactivated`.           |
| `BusinessRole`       | `BusinessRole?`        | Rol de negocio: `Technician`, `Homeowner` o `Company`. Nulo hasta completar.|
| `PersonalData`       | `PersonalData?`        | Value Object con los datos personales. Nulo hasta completar el perfil.      |
| `TechnicianProfile`  | `TechnicianProfile?`   | Entidad interna para perfiles de tipo Técnico. Mutuamente exclusiva con `ClientProfile`. |
| `ClientProfile`      | `ClientProfile?`       | Entidad interna para perfiles de tipo Cliente. Mutuamente exclusiva con `TechnicianProfile`. |
| `CreatedAt`          | `DateTime`             | Fecha y hora UTC de creación del perfil.                                    |
| `CompletedAt`        | `DateTime?`            | Fecha y hora UTC en que el perfil fue completado.                           |
| `DeactivatedAt`      | `DateTime?`            | Fecha y hora UTC de desactivación. Nulo si el perfil está activo.           |
| `DeactivationReason` | `string?`              | Motivo de la desactivación.                                                 |

**Métodos de dominio**

| Método                                                         | Descripción                                                                                                                           |
|----------------------------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------|
| `Create(userId: string): Profile`                              | Factory method estático. Crea un perfil en estado `Incomplete` y emite `ProfileCreatedAsIncompleteEvent`.                            |
| `CompleteAsTechnician(personalData, technicianData)`           | Transiciona el perfil a `Active` asignando el rol `Technician`. Valida que el estado sea `Incomplete`. Emite `ProfileCompletedEvent`. |
| `CompleteAsHomeowner(personalData, alertPreferences, emergencyContact)` | Transiciona a `Active` con rol `Homeowner`. Emite `ProfileCompletedEvent`.                                                  |
| `CompleteAsCompany(personalData, alertPreferences)`            | Transiciona a `Active` con rol `Company`. Valida canales IoT si las alertas están habilitadas. Emite `ProfileCompletedEvent`.         |
| `UpdatePersonalData(updatedData: PersonalData)`                | Actualiza los campos mutables de `PersonalData` (teléfono, dirección). Preserva los inmutables (email, DNI, RUC). Requiere estado `Active`. |
| `Deactivate(reason: string, notes: string?)`                   | Transiciona a `Deactivated`. Captura si el técnico tenía certificación IoT activa. Emite `ProfileDeactivatedEvent`.                  |
| `Reactivate()`                                                 | Transiciona desde `Deactivated` a `Active`. Emite `ProfileReactivatedEvent`.                                                         |

---

### Entidades Internas

#### `TechnicianProfile`

Entidad interna del Aggregate `Profile` que encapsula los datos operativos, profesionales y de certificación IoT de un técnico. Solo tiene sentido en el contexto de un `Profile` con `BusinessRole = Technician`.

**Atributos**

| Atributo                   | Tipo                                        | Descripción                                                                 |
|----------------------------|---------------------------------------------|-----------------------------------------------------------------------------|
| `Id`                       | `TechnicianProfileId`                       | Identificador único con prefijo `tech-`.                                    |
| `Specialties`              | `IReadOnlyList<Specialty>`                  | Lista de especialidades técnicas (enum).                                    |
| `Languages`                | `IReadOnlyList<string>`                     | Idiomas que domina el técnico.                                              |
| `ServiceArea`              | `ServiceArea`                               | Área geográfica de cobertura (GeoJSON Polygon/MultiPolygon).                |
| `ExperienceYears`          | `int`                                       | Años de experiencia declarados.                                             |
| `AboutMe`                  | `string?`                                   | Descripción profesional del técnico.                                        |
| `PhotoUrl`                 | `string?`                                   | URL de la fotografía de perfil.                                             |
| `AverageRating`            | `decimal`                                   | Calificación promedio calculada externamente. Solo lectura desde este BC.   |
| `ActiveServiceCount`       | `int`                                       | Cantidad de servicios activos. Solo lectura desde este BC.                  |
| `IsIoTCertified`           | `bool`                                      | Indica si el técnico posee certificación IoT activa.                        |
| `IoTCertifiedAt`           | `DateTime?`                                 | Fecha y hora UTC del último otorgamiento de certificación IoT.              |
| `IoTCertificationRevokedAt`| `DateTime?`                                 | Fecha y hora UTC de la última revocación.                                   |
| `Certifications`           | `IReadOnlyList<ExternalCertification>`      | Certificaciones externas registradas por el técnico.                        |
| `IoTCertificationHistory`  | `IReadOnlyList<IoTCertificationHistoryEntry>`| Historial append-only de otorgamientos y revocaciones IoT.                 |

**Métodos de dominio**

| Método                                               | Descripción                                                                                                    |
|------------------------------------------------------|----------------------------------------------------------------------------------------------------------------|
| `Create(data: TechnicianData): TechnicianProfile`    | Factory method interno. Inicializa el perfil técnico con `IsIoTCertified = false`.                             |
| `UpdateData(data: TechnicianData)`                   | Actualiza especialidades, área de servicio, idiomas, descripción y foto.                                       |
| `AddCertification(cert: ExternalCertification)`      | Agrega una certificación externa. Valida que no exista un duplicado por nombre e institución.                  |
| `UpdateCertification(certId, updatedCert)`           | Actualiza los datos de una certificación existente por su `CertificationId`.                                   |
| `GrantIoTCertification(grantedByAdminId, notes?)`    | Establece `IsIoTCertified = true` y registra la entrada en el historial IoT. Solo un administrador puede ejecutarlo. |
| `RevokeIoTCertification(revokedByAdminId, reason, notes?)` | Establece `IsIoTCertified = false` y registra la revocación en el historial.                            |

---

#### `ClientProfile`

Entidad interna del Aggregate `Profile` que encapsula las preferencias de notificación, la configuración de alertas IoT y los umbrales de consumo eléctrico de un cliente (propietario residencial o empresa).

**Atributos**

| Atributo               | Tipo                              | Descripción                                                                           |
|------------------------|-----------------------------------|---------------------------------------------------------------------------------------|
| `Id`                   | `ClientProfileId`                 | Identificador único con prefijo `cli-`.                                               |
| `AccountType`          | `AccountType`                     | Tipo de cuenta: `Individual` (propietario) o `Company` (empresa).                     |
| `AlertPreferences`     | `AlertPreferences`                | Value Object con preferencias generales e IoT de notificación.                        |
| `EmergencyContact`     | `EmergencyContact?`               | Contacto de emergencia. Solo disponible para cuentas `Individual`.                    |
| `ConsumptionThresholds`| `IReadOnlyList<CircuitThreshold>` | Lista de umbrales personalizados por circuito. Solo disponible para cuentas `Company`.|

**Métodos de dominio**

| Método                                                            | Descripción                                                                                                              |
|-------------------------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------|
| `CreateForHomeowner(alertPreferences, emergencyContact?)`         | Factory method interno para cuentas `Individual`.                                                                        |
| `CreateForCompany(alertPreferences)`                              | Factory method interno para cuentas `Company`.                                                                           |
| `UpdateAlertPreferences(newPreferences: AlertPreferences)`        | Reemplaza las preferencias de alerta. Valida que al menos un canal general esté activo y que los canales IoT sean consistentes si están habilitados. |
| `UpdateIoTAlertPreferences(enabled: bool, channels)`             | Actualiza exclusivamente la configuración IoT de las preferencias de alerta.                                             |
| `SetConsumptionThresholds(thresholds: IEnumerable<CircuitThreshold>)` | Reemplaza la lista completa de umbrales (upsert). Valida que no existan `CircuitId` duplicados y que cada umbral sea válido. Solo disponible para `Company`. |

---

### Value Objects

| Value Object              | Descripción                                                                                                   |
|---------------------------|---------------------------------------------------------------------------------------------------------------|
| `ProfileId`               | Identificador tipado del Aggregate `Profile`. Formato: `prof-{GUID}`.                                         |
| `TechnicianProfileId`     | Identificador tipado de la entidad `TechnicianProfile`. Formato: `tech-{GUID}`.                               |
| `ClientProfileId`         | Identificador tipado de la entidad `ClientProfile`. Formato: `cli-{GUID}`.                                    |
| `CertificationId`         | Identificador tipado de la entidad `ExternalCertification`. Formato: `cert-{GUID}`.                           |
| `UserId`                  | Referencia lógica al usuario en el IAM BC. Sin prefijo propio.                                                |
| `ProfileStatus`           | Enum de ciclo de vida: `Incomplete`, `Active`, `Deactivated`.                                                 |
| `BusinessRole`            | Enum de rol de negocio: `Technician`, `Homeowner`, `Company`.                                                 |
| `AccountType`             | Enum de tipo de cuenta: `Individual`, `Company`.                                                              |
| `PersonalData`            | Encapsula datos personales. `Email` y `Dni`/`Ruc` son inmutables tras el onboarding. Incluye validación de formato de teléfono y edad mínima de 18 años. |
| `Address`                 | Dirección postal compuesta por calle, distrito, ciudad, país y código postal.                                 |
| `TechnicianData`          | Agregación de atributos profesionales del técnico usada como parámetro de entrada en métodos de dominio.      |
| `AlertPreferences`        | Preferencias de notificación: `SmsNotifications`, `EmailNotifications`, `PushNotifications`, `PreferredContactTime`, `IoTAlertsEnabled`, `IoTAlertChannels?`. |
| `IoTAlertChannels`        | Canales de notificación IoT organizados por nivel de severidad (`Critical`, `High`, `Medium`, `Low`). Valida que los niveles `Critical` y `High` tengan al menos un canal si IoT está habilitado. |
| `AlertChannel`            | Enum de canal de alerta: `Push`, `Sms`, `Email`.                                                              |
| `AlertSeverity`           | Enum de nivel de severidad: `Critical`, `High`, `Medium`, `Low`.                                              |
| `ServiceArea`             | Área geográfica de cobertura del técnico representada como GeoJSON (Polygon o MultiPolygon).                  |
| `Specialty`               | Enum de especialidades técnicas disponibles en el sistema.                                                    |
| `ExternalCertification`   | Certificación profesional externa con campos: `Id`, `Name`, `IssuingOrganization`, `IssueDate`, `ExpirationDate?`, `CredentialId?`, `CredentialUrl?`. |
| `CircuitThreshold`        | Umbral de consumo por circuito: `CircuitId`, `CircuitLabel`, `MaxWatts`, `MaxAmps`, `EvaluationWindow`. Con método `Validate()`. |
| `EmergencyContact`        | Contacto de emergencia: `Name`, `Relationship`, `PhoneNumber`.                                                |
| `PreferredContactTime`    | Enum de preferencia de horario de contacto: `Morning`, `Afternoon`, `Evening`.                                |
| `RevocationReason`        | Enum de motivos de revocación de certificación IoT: `Misconduct`, `TrainingExpired`, entre otros.             |

---

### Interfaces de Repositorio

| Interfaz                         | Extiende                                         | Métodos específicos                                                                                   |
|----------------------------------|--------------------------------------------------|-------------------------------------------------------------------------------------------------------|
| `IProfileRepository`             | `IBaseRepository<Profile, ProfileId>`            | `FindByUserIdAsync`, `ExistsByUserIdAsync`, `ExistsByDniAsync`, `ExistsByEmailAsync`                  |
| `ITechnicianProfileRepository`   | `IBaseRepository<TechnicianProfile, TechnicianProfileId>` | `FindByIdAsync`, `FindIoTCertifiedAsync`                                                    |
| `IClientProfileRepository`       | `IBaseRepository<ClientProfile, ClientProfileId>` | `FindByIdAsync`                                                                                       |

---

### Interfaces de Servicio de Dominio

**`IProfileCommandService`**

Define los contratos de los comandos que modifican el estado del Aggregate `Profile`.

| Método `Handle`                              | Comando                                  | Descripción                                                    |
|----------------------------------------------|------------------------------------------|----------------------------------------------------------------|
| `Handle(CreateProfileCommand)`               | `CreateProfileCommand`                   | Crea un perfil en estado `Incomplete` al detectar un registro de usuario. |
| `Handle(CompleteProfileAsTechnicianCommand)` | `CompleteProfileAsTechnicianCommand`     | Completa el perfil asignando el rol `Technician`.              |
| `Handle(CompleteProfileAsHomeownerCommand)`  | `CompleteProfileAsHomeownerCommand`      | Completa el perfil asignando el rol `Homeowner`.               |
| `Handle(CompleteProfileAsCompanyCommand)`    | `CompleteProfileAsCompanyCommand`        | Completa el perfil asignando el rol `Company`.                 |
| `Handle(UpdateProfilePersonalDataCommand)`   | `UpdateProfilePersonalDataCommand`       | Actualiza teléfono y dirección del perfil activo.              |
| `Handle(UpdateTechnicianDataCommand)`        | `UpdateTechnicianDataCommand`            | Actualiza los datos profesionales de un técnico.               |
| `Handle(AddCertificationCommand)`            | `AddCertificationCommand`                | Agrega una certificación externa al perfil técnico.            |
| `Handle(UpdateAlertPreferencesCommand)`      | `UpdateAlertPreferencesCommand`          | Actualiza las preferencias generales de alerta de un cliente.  |
| `Handle(UpdateIoTAlertPreferencesCommand)`   | `UpdateIoTAlertPreferencesCommand`       | Actualiza exclusivamente la configuración de alertas IoT.      |
| `Handle(DeactivateProfileCommand)`           | `DeactivateProfileCommand`               | Desactiva el perfil registrando el motivo.                     |
| `Handle(ReactivateProfileCommand)`           | `ReactivateProfileCommand`               | Reactiva un perfil previamente desactivado.                    |
| `Handle(GrantIoTCertificationCommand)`       | `GrantIoTCertificationCommand`           | Otorga la certificación IoT a un técnico (solo administrador). |
| `Handle(RevokeIoTCertificationCommand)`      | `RevokeIoTCertificationCommand`          | Revoca la certificación IoT de un técnico (solo administrador).|
| `Handle(SetConsumptionThresholdsCommand)`    | `SetConsumptionThresholdsCommand`        | Reemplaza los umbrales de consumo de una cuenta `Company`.     |

**`IProfileQueryService`**

Define los contratos de las consultas que leen el estado del Aggregate `Profile` sin modificarlo.

| Método `Handle`                              | Query                              | Tipo de retorno                        | Descripción                                    |
|----------------------------------------------|------------------------------------|----------------------------------------|------------------------------------------------|
| `Handle(GetProfileByUserIdQuery)`            | `GetProfileByUserIdQuery`          | `Task<Profile?>`                       | Obtiene el perfil completo dado un `UserId`.   |
| `Handle(GetProfileByIdQuery)`                | `GetProfileByIdQuery`              | `Task<Profile?>`                       | Obtiene el perfil completo dado un `ProfileId`.|
| `Handle(GetTechnicianProfileByIdQuery)`      | `GetTechnicianProfileByIdQuery`    | `Task<TechnicianProfile?>`             | Obtiene el sub-perfil de un técnico.           |
| `Handle(GetClientProfileByIdQuery)`          | `GetClientProfileByIdQuery`        | `Task<ClientProfile?>`                 | Obtiene el sub-perfil de un cliente.           |
| `Handle(GetIoTCertificationStatusQuery)`     | `GetIoTCertificationStatusQuery`   | `Task<TechnicianProfile?>`             | Obtiene el estado de certificación IoT.        |
| `Handle(GetConsumptionThresholdsQuery)`      | `GetConsumptionThresholdsQuery`    | `Task<IReadOnlyList<CircuitThreshold>>`| Obtiene los umbrales de consumo de un cliente. |

---

#### 4.2.2.2. Interface Layer.

Interface Layer

Esta capa define el contrato de comunicación entre el exterior y el Bounded Context, tanto para los consumidores REST como para otros Bounded Contexts a través del Anti-Corruption Layer (ACL).

---

### Resources (Entrada)

Los recursos de entrada encapsulan los datos provenientes de las solicitudes HTTP y son transformados en comandos por los Assemblers correspondientes.

| Recurso                                | Atributos principales                                                                                                   | Propósito                                                 |
|----------------------------------------|-------------------------------------------------------------------------------------------------------------------------|-----------------------------------------------------------|
| `CompleteProfileAsTechnicianResource`  | `ProfileId`, `FirstName`, `LastName`, `Email`, `PhoneNumber`, `Dni`, `DateOfBirth`, `Address`, `Specialties`, `Languages`, `ExperienceYears`, `ServiceArea`, `AboutMe?`, `PhotoUrl?` | Completar perfil como técnico. |
| `CompleteProfileAsHomeownerResource`   | `ProfileId`, `FirstName`, `LastName`, `Email`, `PhoneNumber`, `Dni`, `DateOfBirth`, `Address`, `SmsNotifications`, `EmailNotifications`, `PushNotifications`, `PreferredContactTime`, `EmergencyContact?` | Completar perfil como propietario residencial. |
| `CompleteProfileAsCompanyResource`     | `ProfileId`, `CompanyName`, `Ruc`, `Email`, `PhoneNumber`, `Address`, `SmsNotifications`, `EmailNotifications`, `PushNotifications`, `IoTAlertsEnabled`, `IoTAlertChannels?` | Completar perfil como empresa. |
| `UpdateProfilePersonalDataResource`    | `PhoneNumber?`, `Address?`                                                                                              | Actualizar datos personales mutables.                     |
| `UpdateTechnicianDataResource`         | `Specialties?`, `ServiceArea?`, `Languages?`, `AboutMe?`, `PhotoUrl?`                                                   | Actualizar datos profesionales del técnico.               |
| `AddCertificationResource`             | `Name`, `IssuingOrganization`, `IssueDate`, `ExpirationDate?`, `CredentialId?`, `CredentialUrl?`                       | Agregar una certificación externa.                        |
| `UpdateAlertPreferencesResource`       | `SmsNotifications`, `EmailNotifications`, `PushNotifications`, `PreferredContactTime`, `IoTAlertsEnabled`, `IoTAlertChannels?` | Actualizar preferencias de alerta del cliente.     |
| `UpdateIoTAlertPreferencesResource`    | `IoTAlertsEnabled`, `IoTAlertChannels`                                                                                  | Actualizar únicamente la configuración IoT del cliente.   |
| `DeactivateProfileResource`            | `Reason`, `Notes?`                                                                                                      | Solicitar la desactivación del perfil.                    |
| `GrantIoTCertificationResource`        | `TechnicianProfileId`, `Notes?`                                                                                         | Otorgar certificación IoT (solo administrador).           |
| `RevokeIoTCertificationResource`       | `TechnicianProfileId`, `Reason`, `Notes?`                                                                               | Revocar certificación IoT (solo administrador).           |
| `SetConsumptionThresholdsResource`     | `Thresholds: IEnumerable<CircuitThresholdResource>`                                                                     | Reemplazar umbrales de consumo (solo empresas).           |

**Recursos anidados compartidos**

| Recurso                      | Atributos                                                            |
|------------------------------|----------------------------------------------------------------------|
| `AddressResource`            | `Street`, `District`, `City`, `Country`, `PostalCode`               |
| `ServiceAreaResource`        | `Type`, `Coordinates`                                               |
| `EmergencyContactResource`   | `Name`, `Relationship`, `PhoneNumber`                               |
| `IoTAlertChannelsResource`   | `Critical`, `High`, `Medium`, `Low` (colecciones de strings)        |
| `CircuitThresholdResource`   | `CircuitId`, `CircuitLabel`, `MaxWatts`, `MaxAmps`, `EvaluationWindow` |

---

### Resources (Salida)

| Recurso                                    | Atributos principales                                                                                                                  | Propósito                                                        |
|--------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------------|
| `ProfileStatusResponseResource`            | `ProfileId`, `UserId`, `Status`, `CompletionPercentage`                                                                                | Retornar el estado y porcentaje de completitud del perfil.       |
| `TechnicianProfileResponseResource`        | `ProfileId`, `UserId`, `BusinessRole`, `Status`, `PersonalData`, `Technician`                                                          | Retornar el perfil completo de un técnico.                       |
| `ClientProfileResponseResource`            | `ProfileId`, `UserId`, `BusinessRole`, `AccountType`, `Status`, `PersonalData`, `AlertPreferences`                                     | Retornar el perfil completo de un cliente.                       |
| `TechnicianDataResponseResource`           | `TechnicianProfileId`, `Specialties`, `Certifications`, `ExperienceYears`, `ServiceArea`, `IsIoTCertified`, `IoTCertifiedAt?`, `AverageRating`, `ActiveServiceCount`, `AboutMe?` | Datos profesionales del técnico. |
| `PersonalDataResponseResource`             | `FirstName?`, `LastName?`, `CompanyName?`, `Email`, `PhoneNumber`, `Dni?`, `Address`                                                   | Datos personales del perfil.                                     |
| `AlertPreferencesResponseResource`         | `SmsNotifications`, `EmailNotifications`, `PushNotifications`, `PreferredContactTime`, `IoTAlertsEnabled`, `IoTAlertChannels?`         | Preferencias de notificación del cliente.                        |
| `IoTCertificationStatusResponseResource`   | `TechnicianProfileId`, `IsIoTCertified`, `CertifiedAt?`, `CertificationHistory`                                                       | Estado completo de la certificación IoT de un técnico.           |
| `ConsumptionThresholdsResponseResource`    | `ProfileId`, `ClientProfileId`, `AccountType`, `Thresholds`                                                                           | Umbrales de consumo configurados por la empresa.                 |
| `ExternalCertificationResponseResource`    | `Id`, `Name`, `IssuingOrganization`, `IssueDate`, `ExpirationDate?`, `CredentialId?`                                                  | Datos de una certificación externa del técnico.                  |

---

### Assemblers (Transforms)

Los Assemblers son clases estáticas que implementan la conversión bidireccional entre recursos HTTP y comandos o entidades de dominio.

**Assemblers de Comando (Resource → Command)**

| Clase                                                          | Método principal            | Propósito                                                              |
|----------------------------------------------------------------|-----------------------------|------------------------------------------------------------------------|
| `CompleteProfileAsTechnicianCommandFromResourceAssembler`      | `ToCommandFromResource`     | Construye `CompleteProfileAsTechnicianCommand` desde el resource HTTP. |
| `CompleteProfileAsHomeownerCommandFromResourceAssembler`       | `ToCommandFromResource`     | Construye `CompleteProfileAsHomeownerCommand`.                         |
| `CompleteProfileAsCompanyCommandFromResourceAssembler`         | `ToCommandFromResource`     | Construye `CompleteProfileAsCompanyCommand`.                           |
| `UpdateProfilePersonalDataCommandFromResourceAssembler`        | `ToCommandFromResource`     | Construye `UpdateProfilePersonalDataCommand`.                          |
| `UpdateTechnicianDataCommandFromResourceAssembler`             | `ToCommandFromResource`     | Construye `UpdateTechnicianDataCommand`.                               |
| `AddCertificationCommandFromResourceAssembler`                 | `ToCommandFromResource`     | Construye `AddCertificationCommand`.                                   |
| `UpdateAlertPreferencesCommandFromResourceAssembler`           | `ToCommandFromResource`     | Construye `UpdateAlertPreferencesCommand`.                             |
| `UpdateIoTAlertPreferencesCommandFromResourceAssembler`        | `ToCommandFromResource`     | Construye `UpdateIoTAlertPreferencesCommand`.                          |
| `GrantIoTCertificationCommandFromResourceAssembler`            | `ToCommandFromResource`     | Construye `GrantIoTCertificationCommand` incluyendo el ID del admin.   |
| `RevokeIoTCertificationCommandFromResourceAssembler`           | `ToCommandFromResource`     | Construye `RevokeIoTCertificationCommand` incluyendo el ID del admin.  |
| `SetConsumptionThresholdsCommandFromResourceAssembler`         | `ToCommandFromResource`     | Construye `SetConsumptionThresholdsCommand`.                           |

**Assemblers de Respuesta (Entity → Resource)**

| Clase                                                          | Método principal            | Propósito                                                              |
|----------------------------------------------------------------|-----------------------------|------------------------------------------------------------------------|
| `ProfileStatusResponseResourceFromEntityAssembler`             | `ToResourceFromEntity`      | Mapea `Profile` a `ProfileStatusResponseResource`.                     |
| `TechnicianProfileResponseResourceFromEntityAssembler`         | `ToResourceFromEntity`      | Mapea `Profile` + `TechnicianProfile` a `TechnicianProfileResponseResource`. |
| `ClientProfileResponseResourceFromEntityAssembler`             | `ToResourceFromEntity`      | Mapea `Profile` + `ClientProfile` a `ClientProfileResponseResource`.   |
| `IoTCertificationStatusResponseResourceFromEntityAssembler`    | `ToResourceFromEntity`      | Mapea `TechnicianProfile` a `IoTCertificationStatusResponseResource`.  |
| `ConsumptionThresholdsResponseResourceFromEntityAssembler`     | `ToResourceFromEntity`      | Mapea `Profile` + thresholds a `ConsumptionThresholdsResponseResource`.|

---

### Controllers

El Bounded Context expone tres controladores REST independientes, segregados por tipo de actor, lo que facilita la aplicación de políticas de autorización y la documentación en Swagger.

---

#### `ProfilesController`

**Ruta base:** `api/v1/profiles`  
**Descripción:** Gestiona las operaciones comunes del ciclo de vida del perfil: consulta de estado, completitud inicial y desactivación/reactivación.

| Método HTTP | Ruta                              | Descripción                                          | Respuestas HTTP          |
|-------------|-----------------------------------|------------------------------------------------------|--------------------------|
| `GET`       | `/{profileId}/status`             | Consulta el estado y porcentaje de completitud.       | 200, 404                 |
| `GET`       | `/by-user/{userId}`               | Obtiene el perfil completo asociado a un `UserId`.    | 200, 404                 |
| `PATCH`     | `/{profileId}/complete/technician`| Completa el perfil como técnico.                      | 200, 400                 |
| `PATCH`     | `/{profileId}/complete/homeowner` | Completa el perfil como propietario residencial.      | 200, 400                 |
| `PATCH`     | `/{profileId}/complete/company`   | Completa el perfil como empresa.                      | 200, 400                 |
| `PATCH`     | `/{profileId}/personal-data`      | Actualiza teléfono y/o dirección.                     | 200, 400                 |
| `PATCH`     | `/{profileId}/deactivate`         | Desactiva el perfil.                                  | 200, 400                 |
| `PATCH`     | `/{profileId}/reactivate`         | Reactiva el perfil.                                   | 200, 400                 |

---

#### `TechniciansController`

**Ruta base:** `api/v1/technicians`  
**Descripción:** Gestiona los endpoints exclusivos para perfiles de tipo técnico, incluyendo certificaciones externas y la gestión de la certificación IoT (operación administrativa).

| Método HTTP | Ruta                                              | Descripción                                              | Respuestas HTTP   |
|-------------|---------------------------------------------------|----------------------------------------------------------|-------------------|
| `GET`       | `/{profileId}`                                    | Obtiene el perfil completo del técnico.                   | 200, 404          |
| `PATCH`     | `/{profileId}/technician-data`                    | Actualiza datos profesionales del técnico.                | 200, 400          |
| `POST`      | `/{profileId}/certifications`                     | Agrega una certificación externa.                         | 201, 400          |
| `GET`       | `/{technicianProfileId}/iot-certification`        | Consulta el estado de la certificación IoT.               | 200, 404          |
| `POST`      | `/{technicianProfileId}/iot-certification/grant`  | Otorga la certificación IoT (solo administrador).         | 200, 400, 403     |
| `POST`      | `/{technicianProfileId}/iot-certification/revoke` | Revoca la certificación IoT (solo administrador).         | 200, 400, 403     |

---

#### `ClientsController`

**Ruta base:** `api/v1/clients`  
**Descripción:** Gestiona los endpoints exclusivos para perfiles de tipo cliente (propietarios residenciales y empresas), incluyendo preferencias de alerta y umbrales de consumo.

| Método HTTP | Ruta                                  | Descripción                                                    | Respuestas HTTP |
|-------------|---------------------------------------|----------------------------------------------------------------|-----------------|
| `PATCH`     | `/{profileId}/alert-preferences`      | Actualiza las preferencias generales de alerta.                | 200, 400        |
| `PATCH`     | `/{profileId}/iot-alert-preferences`  | Actualiza exclusivamente la configuración de alertas IoT.      | 200, 400        |
| `PUT`       | `/{profileId}/consumption-thresholds` | Reemplaza los umbrales de consumo (solo cuentas `Company`).    | 200, 400        |
| `GET`       | `/{profileId}/consumption-thresholds` | Consulta los umbrales de consumo configurados.                 | 200, 404        |

---

#### 4.2.2.3. Application Layer

Esta capa orquesta los casos de uso del Bounded Context coordinando el dominio, los repositorios y la publicación de eventos de dominio. Implementa las interfaces definidas en la capa de dominio.

---

### `ProfileCommandService`

**Ubicación:** `Profiles/Application/Internal/CommandServices/ProfileCommandService.cs`  
**Implementa:** `IProfileCommandService`  
**Dependencias:** `IProfileRepository`, `ITechnicianProfileRepository`, `IUnitOfWork`, `IMediator`, `ILogger<ProfileCommandService>`

Este servicio orquesta todos los comandos de escritura del BC. Por cada comando ejecutado, el flujo estándar consiste en: (1) recuperar el Aggregate desde el repositorio, (2) invocar el método de dominio correspondiente, (3) persistir los cambios mediante `IUnitOfWork.CompleteAsync()`, y (4) publicar los eventos de dominio generados a través de `IMediator`.

| Comando manejado                           | Descripción de orquestación                                                                                            |
|--------------------------------------------|------------------------------------------------------------------------------------------------------------------------|
| `CreateProfileCommand`                     | Verifica idempotencia por `UserId`. Invoca `Profile.Create()`. Persiste y publica `ProfileCreatedAsIncompleteEvent`.  |
| `CompleteProfileAsTechnicianCommand`       | Valida unicidad de DNI y email. Construye `PersonalData` y `TechnicianData`. Invoca `CompleteAsTechnician()`.         |
| `CompleteProfileAsHomeownerCommand`        | Valida unicidad de DNI y email. Construye `PersonalData` y `AlertPreferences`. Invoca `CompleteAsHomeowner()`.        |
| `CompleteProfileAsCompanyCommand`          | Valida unicidad de email. Construye `PersonalData` y `AlertPreferences` con configuración IoT. Invoca `CompleteAsCompany()`. |
| `UpdateProfilePersonalDataCommand`         | Construye el parche de datos personales y delega en `Profile.UpdatePersonalData()`.                                    |
| `UpdateTechnicianDataCommand`              | Valida que `BusinessRole == Technician`. Delega en `TechnicianProfile.UpdateData()`.                                  |
| `AddCertificationCommand`                  | Construye `ExternalCertification` y delega en `TechnicianProfile.AddCertification()`.                                 |
| `UpdateAlertPreferencesCommand`            | Construye `AlertPreferences` y delega en `ClientProfile.UpdateAlertPreferences()`.                                    |
| `UpdateIoTAlertPreferencesCommand`         | Valida que `BusinessRole != Technician`. Construye `IoTAlertChannels` y delega en `ClientProfile.UpdateIoTAlertPreferences()`. |
| `DeactivateProfileCommand`                 | Invoca `Profile.Deactivate()`. El evento `ProfileDeactivatedEvent` incluye `wasIoTCertified` para notificar a Service Design BC. |
| `ReactivateProfileCommand`                 | Invoca `Profile.Reactivate()`.                                                                                         |
| `GrantIoTCertificationCommand`             | Resuelve el `TechnicianProfile` por ID. Invoca `GrantIoTCertification()`. Publica `IoTCertificationGrantedEvent`.     |
| `RevokeIoTCertificationCommand`            | Resuelve el `TechnicianProfile` por ID. Invoca `RevokeIoTCertification()`. Publica `IoTCertificationRevokedEvent`.    |
| `SetConsumptionThresholdsCommand`          | Construye la lista de `CircuitThreshold`. Delega en `ClientProfile.SetConsumptionThresholds()`. Publica `ConsumptionThresholdsUpdatedEvent`. |

---

### `ProfileQueryService`

**Ubicación:** `Profiles/Application/Internal/QueryServices/ProfileQueryService.cs`  
**Implementa:** `IProfileQueryService`  
**Dependencias:** `IProfileRepository`, `ITechnicianProfileRepository`, `IClientProfileRepository`

Este servicio orquesta todas las operaciones de lectura. No modifica el estado de ninguna entidad ni publica eventos.

| Query manejada                       | Descripción                                                                                                    |
|--------------------------------------|----------------------------------------------------------------------------------------------------------------|
| `GetProfileByUserIdQuery`            | Delega en `IProfileRepository.FindByUserIdAsync()`. Incluye `TechnicianProfile` y `ClientProfile` por eager loading. |
| `GetProfileByIdQuery`                | Delega en `IProfileRepository.FindByIdAsync()`.                                                                |
| `GetTechnicianProfileByIdQuery`      | Delega en `ITechnicianProfileRepository.FindByIdAsync()`. Incluye `Certifications` e `IoTCertificationHistory`. |
| `GetClientProfileByIdQuery`          | Delega en `IClientProfileRepository.FindByIdAsync()`. Incluye `ConsumptionThresholds`.                        |
| `GetIoTCertificationStatusQuery`     | Delega en `ITechnicianProfileRepository.FindByIdAsync()` para obtener el estado y el historial IoT completo.  |
| `GetConsumptionThresholdsQuery`      | Delega en `IProfileRepository.FindByIdAsync()` y proyecta `ClientProfile.ConsumptionThresholds`.              |

---

### Puerto de Salida: `IIamContextFacade`

**Ubicación:** `Profiles/Application/Internal/OutboundServices/IIamContextFacade.cs`  
**Propósito:** Puerto hacia el Bounded Context de IAM. Permite que el BC de Profiles consulte datos de identidad sin acceder directamente a las tablas de IAM.

---

#### 4.2.2.4. Infrastructure Layer.

Esta capa proporciona las implementaciones concretas de los repositorios definidos en el dominio y las configuraciones de mapeo objeto-relacional mediante Entity Framework Core.

---

### Implementación de Repositorios

| Clase                          | Interfaz implementada              | Descripción                                                                                                                      |
|--------------------------------|------------------------------------|----------------------------------------------------------------------------------------------------------------------------------|
| `ProfileRepository`            | `IProfileRepository`               | Extiende `BaseRepository<Profile, ProfileId>`. Implementa `FindByUserIdAsync` con eager loading de `TechnicianProfile` y `ClientProfile`. Proporciona `ExistsByDniAsync` y `ExistsByEmailAsync` para validaciones de unicidad. |
| `TechnicianProfileRepository`  | `ITechnicianProfileRepository`     | Extiende `BaseRepository<TechnicianProfile, TechnicianProfileId>`. Implementa `FindByIdAsync` con eager loading de `Certifications` e `IoTCertificationHistory`. Implementa `FindIoTCertifiedAsync` para consultas de pool IoT. |
| `ClientProfileRepository`      | `IClientProfileRepository`         | Extiende `BaseRepository<ClientProfile, ClientProfileId>`. Implementa `FindByIdAsync` con eager loading de `ConsumptionThresholds`. |

---

### Configuraciones de Persistencia (EF Core)

#### `ProfileConfiguration`

**Tabla:** `profiles`

| Columna                  | Tipo SQL        | Restricciones               | Mapeo                                                   |
|--------------------------|-----------------|-----------------------------|---------------------------------------------------------|
| `id`                     | `VARCHAR(60)`   | `PK`, `NOT NULL`            | `Profile.Id` (Value Object `ProfileId`)                 |
| `user_id`                | `VARCHAR(60)`   | `NOT NULL`, `UNIQUE`        | `Profile.UserId` (Value Object `UserId`)                |
| `status`                 | `VARCHAR(30)`   | `NOT NULL`                  | `Profile.Status` (almacenado como string)               |
| `business_role`          | `VARCHAR(20)`   | Nullable                    | `Profile.BusinessRole` (almacenado como string)         |
| `first_name`             | `VARCHAR(100)`  | Nullable                    | `Profile.PersonalData.FirstName` (Owned Entity)         |
| `last_name`              | `VARCHAR(100)`  | Nullable                    | `Profile.PersonalData.LastName`                         |
| `company_name`           | `VARCHAR(200)`  | Nullable                    | `Profile.PersonalData.CompanyName`                      |
| `ruc`                    | `VARCHAR(20)`   | Nullable                    | `Profile.PersonalData.Ruc`                              |
| `email`                  | `VARCHAR(255)`  | Unique (índice parcial)     | `Profile.PersonalData.Email`                            |
| `phone_number`           | `VARCHAR(20)`   | Nullable                    | `Profile.PersonalData.PhoneNumber`                      |
| `dni`                    | `VARCHAR(15)`   | Nullable                    | `Profile.PersonalData.Dni`                              |
| `date_of_birth`          | `DATE`          | Nullable                    | `Profile.PersonalData.DateOfBirth`                      |
| `address_street`         | `VARCHAR(200)`  | Nullable                    | `Profile.PersonalData.Address.Street` (Owned → Owned)   |
| `address_district`       | `VARCHAR(100)`  | Nullable                    | `Profile.PersonalData.Address.District`                 |
| `address_city`           | `VARCHAR(100)`  | Nullable                    | `Profile.PersonalData.Address.City`                     |
| `address_country`        | `VARCHAR(100)`  | Nullable                    | `Profile.PersonalData.Address.Country`                  |
| `address_postal_code`    | `VARCHAR(20)`   | Nullable                    | `Profile.PersonalData.Address.PostalCode`               |
| `created_at`             | `TIMESTAMPTZ`   | `NOT NULL`, `DEFAULT now()` | `Profile.CreatedAt`                                     |
| `completed_at`           | `TIMESTAMPTZ`   | Nullable                    | `Profile.CompletedAt`                                   |
| `deactivated_at`         | `TIMESTAMPTZ`   | Nullable                    | `Profile.DeactivatedAt`                                 |
| `deactivation_reason`    | `VARCHAR(50)`   | Nullable                    | `Profile.DeactivationReason`                            |

`PersonalData` y `Address` se mapean como **Owned Entities aplanadas** (sin tabla separada). `TechnicianProfile` y `ClientProfile` se configuran como relaciones `1:0..1` con clave foránea `profile_id` en sus respectivas tablas.

---

#### `TechnicianProfileConfiguration`

**Tabla:** `technician_profiles`

| Columna                         | Tipo SQL        | Restricciones      | Mapeo                                                          |
|---------------------------------|-----------------|--------------------|----------------------------------------------------------------|
| `id`                            | `VARCHAR(60)`   | `PK`, `NOT NULL`   | `TechnicianProfile.Id`                                         |
| `profile_id`                    | `VARCHAR(60)`   | `FK → profiles.id` | Clave foránea al Aggregate Root                                |
| `specialties`                   | `JSONB`         | `NOT NULL`         | `IReadOnlyList<Specialty>` serializado como JSON               |
| `languages`                     | `JSONB`         | `NOT NULL`         | `IReadOnlyList<string>` serializado como JSON                  |
| `service_area`                  | `JSONB`         | `NOT NULL`         | `ServiceArea` serializado como GeoJSON                         |
| `experience_years`              | `INTEGER`       |                    | `TechnicianProfile.ExperienceYears`                            |
| `about_me`                      | `VARCHAR(2000)` | Nullable           | `TechnicianProfile.AboutMe`                                    |
| `photo_url`                     | `VARCHAR(500)`  | Nullable           | `TechnicianProfile.PhotoUrl`                                   |
| `average_rating`                | `DECIMAL(4,2)`  | `DEFAULT 0`        | `TechnicianProfile.AverageRating`                              |
| `active_service_count`          | `INTEGER`       | `DEFAULT 0`        | `TechnicianProfile.ActiveServiceCount`                         |
| `is_iot_certified`              | `BOOLEAN`       | `DEFAULT false`    | `TechnicianProfile.IsIoTCertified`                             |
| `iot_certified_at`              | `TIMESTAMPTZ`   | Nullable           | `TechnicianProfile.IoTCertifiedAt`                             |
| `iot_certification_revoked_at`  | `TIMESTAMPTZ`   | Nullable           | `TechnicianProfile.IoTCertificationRevokedAt`                  |

Las colecciones `Certifications` e `IoTCertificationHistory` se mapean a tablas separadas (`profiles_external_certifications` y `profiles_iot_certification_history`) con clave foránea `technician_profile_id`.

---

#### `ClientProfileConfiguration`

**Tabla:** `client_profiles`

| Columna                          | Tipo SQL        | Restricciones      | Mapeo                                                                  |
|----------------------------------|-----------------|--------------------|------------------------------------------------------------------------|
| `id`                             | `VARCHAR(60)`   | `PK`, `NOT NULL`   | `ClientProfile.Id`                                                     |
| `profile_id`                     | `VARCHAR(60)`   | `FK → profiles.id` | Clave foránea al Aggregate Root                                        |
| `account_type`                   | `VARCHAR(20)`   | `NOT NULL`         | `ClientProfile.AccountType` (almacenado como string)                   |
| `sms_notifications`              | `BOOLEAN`       | `DEFAULT false`    | `ClientProfile.AlertPreferences.SmsNotifications` (Owned Entity)       |
| `email_notifications`            | `BOOLEAN`       | `DEFAULT true`     | `ClientProfile.AlertPreferences.EmailNotifications`                    |
| `push_notifications`             | `BOOLEAN`       | `DEFAULT true`     | `ClientProfile.AlertPreferences.PushNotifications`                     |
| `preferred_contact_time`         | `VARCHAR(20)`   | Nullable           | `ClientProfile.AlertPreferences.PreferredContactTime`                  |
| `iot_alerts_enabled`             | `BOOLEAN`       | `DEFAULT false`    | `ClientProfile.AlertPreferences.IoTAlertsEnabled`                      |
| `iot_alert_channels`             | `JSONB`         | Nullable           | `ClientProfile.AlertPreferences.IoTAlertChannels` serializado como JSON|
| `emergency_contact_name`         | `VARCHAR(200)`  | Nullable           | `ClientProfile.EmergencyContact.Name` (Owned Entity nullable)          |
| `emergency_contact_relationship` | `VARCHAR(100)`  | Nullable           | `ClientProfile.EmergencyContact.Relationship`                          |
| `emergency_contact_phone`        | `VARCHAR(20)`   | Nullable           | `ClientProfile.EmergencyContact.PhoneNumber`                           |

`AlertPreferences` y `EmergencyContact` se mapean como **Owned Entities aplanadas**. `IoTAlertChannels` se persiste como columna de tipo `JSONB`. La colección `ConsumptionThresholds` se mapea a la tabla `profiles_consumption_thresholds` con clave foránea `client_profile_id`.

---

#### 4.2.2.5. Bounded Context Software Architecture Component Level Diagrams.
#### 4.2.2.6. Bounded Context Software Architecture Code Level Diagrams.
##### 4.2.2.6.1. Bounded Context Domain Layer Class Diagrams.

El siguiente diagrama representa las clases de la capa de dominio del Bounded Context de Profiles & Preferences, incluyendo el Aggregate Root, las entidades internas y los principales Value Objects, junto con sus relaciones de composición y dependencia.

##### 4.2.2.6.2. Bounded Context Database Design Diagram.

El siguiente diagrama representa el modelo físico de la base de datos del Bounded Context de Profiles & Preferences, incluyendo las tablas, claves primarias, claves foráneas y los tipos de columna aplicados.

### 4.2.3. Bounded Context: Assets & Resources Management
#### 4.2.3.1. Domain Layer.

Domain Layer

Este Bounded Context implementa el núcleo del dominio bajo el patrón DDD (Domain-Driven Design) con CQRS (Command Query Responsibility Segregation). La capa de dominio concentra los Aggregates, Entities, Value Objects, contratos de repositorio e interfaces de servicios de dominio, sin dependencias de infraestructura o frameworks externos.

---

### Aggregates

El BC está compuesto por seis Aggregate Roots independientes, cada uno protegiendo sus propias invariantes de negocio.

---

#### `ComponentType`

Representa el catálogo maestro de tipos de componentes eléctricos administrado por el System Admin. Ningún `Component` puede existir sin referencia a un `ComponentType` activo.

**Atributos:**

| Atributo    | Tipo              | Descripción                                              |
|-------------|-------------------|----------------------------------------------------------|
| `Id`        | `ComponentTypeId` | Identificador único con prefijo `ctype-{Guid}`           |
| `Name`      | `string`          | Nombre del tipo de componente. No puede estar vacío.     |
| `Description` | `string`        | Descripción del tipo de componente.                      |
| `IsActive`  | `bool`            | Indica si el tipo de componente está activo en el sistema |

**Métodos de negocio:**

| Método       | Descripción                                                                                           |
|--------------|-------------------------------------------------------------------------------------------------------|
| `Create(name, description)` | Factory method. Valida que `name` no sea vacío, inicializa `IsActive = true` y publica `ComponentTypeCreatedEvent`. |
| `Update(name, description)` | Actualiza nombre y descripción. Publica `ComponentTypeUpdatedEvent`.                     |
| `Deactivate()` | Marca el tipo como inactivo de manera idempotente. No elimina el registro físicamente.              |

**Invariantes:** El nombre es único por categoría y modelo. El precio estándar debe ser mayor a cero. No puede desactivarse si existen componentes activos de este tipo vinculados a inventarios.

---

#### `Component`

Representa un ítem de catálogo concreto (por ejemplo, "Interruptor termomagnético 20A"). Es la referencia que los técnicos incorporan a su inventario.

**Atributos:**

| Atributo      | Tipo              | Descripción                                              |
|---------------|-------------------|----------------------------------------------------------|
| `Id`          | `ComponentId`     | Identificador único con prefijo `comp-{Guid}`            |
| `TypeId`      | `ComponentTypeId` | Referencia al tipo de componente al que pertenece        |
| `Name`        | `string`          | Nombre del componente específico                         |
| `Description` | `string`          | Descripción detallada del componente                     |
| `IsActive`    | `bool`            | Indica si el componente está activo en el catálogo       |

**Métodos de negocio:**

| Método                             | Descripción                                                                   |
|------------------------------------|-------------------------------------------------------------------------------|
| `Create(typeId, name, description)` | Factory method. Valida `typeId` y `name`. Publica `ComponentCreatedEvent`.   |
| `UpdateInfo(name, description, isActive)` | Actualiza los atributos del componente. Publica `ComponentUpdatedEvent`. |
| `Activate()`                       | Activa el componente de manera idempotente. Publica `ComponentActivatedEvent`. |
| `Deactivate()`                     | Desactiva el componente de manera idempotente. Publica `ComponentDeactivatedEvent`. |

---

#### `TechnicianInventory`

Gestiona el stock de componentes de un técnico y el ciclo de vida de reservas vinculadas a servicios. Es el núcleo de la lógica de aprovisionamiento.

**Atributos:**

| Atributo      | Tipo                       | Descripción                                                        |
|---------------|----------------------------|--------------------------------------------------------------------|
| `Id`          | `TechnicianInventoryId`    | Identificador único con prefijo `inv-{Guid}`                       |
| `TechnicianId`| `TechnicianId`             | Referencia al técnico propietario (único por inventario)           |
| `Status`      | `EInventoryStatus`         | Estado del inventario (`Empty`, `Active`, `Inactive`)              |
| `StockItems`  | `IReadOnlyCollection<ComponentStock>` | Colección de ítems de stock (entidades internas)         |
| `Reservations`| `IReadOnlyCollection<ComponentReservation>` | Colección de reservas activas                      |

**Métodos de negocio:**

| Método                                        | Descripción                                                                                   |
|-----------------------------------------------|-----------------------------------------------------------------------------------------------|
| `Create(technicianId)`                        | Factory method. Inicializa inventario vacío. Publica `TechnicianInventoryCreatedEvent`.       |
| `AddStock(componentId, quantity, alertThreshold)` | Añade un nuevo ítem de stock al inventario. Lanza excepción si ya existe stock para ese componente. |
| `IncreaseStock(componentId, amount)`          | Incrementa la cantidad disponible. Publica `ComponentStockIncreasedEvent`.                   |
| `DecreaseStock(componentId, amount)`          | Decrementa la cantidad disponible. Publica `ComponentStockDecreasedEvent` y, si corresponde, `ComponentStockLowEvent`. |
| `ReserveComponentsForService(serviceId, items)` | Reserva atómicamente todos los ítems requeridos para un servicio con TTL de 72 horas. Publica `ComponentsReservedForServiceEvent`. |
| `ConsumeComponentsForService(serviceId)`      | Consume los componentes reservados para un servicio. Publica `ComponentsConsumedEvent`.      |
| `ReleaseReservation(serviceId, reason)`       | Libera una reserva activa y restaura el stock disponible. Publica `ComponentReservationReleasedEvent`. |

**Invariantes:** Un técnico puede tener a lo sumo un inventario. No se reserva si `AvailableForReservation < quantityRequired`, con validación atómica sobre todos los ítems. Al consumir, los componentes deben estar previamente reservados para el `serviceId` indicado. Las reservas tienen un tiempo de vida de 72 horas.

---

#### `Property`

Representa un inmueble registrado en el sistema. Mantiene dirección, geolocalización obligatoria y, tras la integración IoT, la lista de dispositivos instalados.

**Atributos:**

| Atributo                 | Tipo                          | Descripción                                                              |
|--------------------------|-------------------------------|--------------------------------------------------------------------------|
| `Id`                     | `PropertyId`                  | Identificador único con prefijo `prop-{Guid}`                            |
| `OwnerId`                | `OwnerId`                     | Referencia al propietario del inmueble                                   |
| `Address`                | `Address`                     | Value Object con campos de dirección postal (Shared Kernel)              |
| `Geolocation`            | `Geolocation`                 | Value Object con latitud, longitud, precisión y fuente                   |
| `Region`                 | `Region`                      | Value Object de región geográfica                                        |
| `District`               | `District`                    | Value Object de distrito geográfico                                      |
| `Status`                 | `EPropertyStatus`             | Estado del inmueble (`Created`, `Active`, `Inactive`, `Archived`)        |
| `IsActive`               | `bool`                        | Indica si la propiedad está activa                                       |
| `InstalledDeviceIds`     | `IReadOnlyCollection<string>` | Lista serializada de IDs de dispositivos IoT asignados o instalados      |
| `HasActiveIoTMonitoring` | `bool`                        | Calculado automáticamente según `InstalledDeviceIds`                     |

**Métodos de negocio:**

| Método                                              | Descripción                                                                                  |
|-----------------------------------------------------|----------------------------------------------------------------------------------------------|
| `Create(ownerId, address, geolocation, region, district)` | Factory method. Valida todos los parámetros obligatorios. Publica `PropertyCreatedEvent`. |
| `UpdateAddress(newAddress)`                         | Actualiza la dirección de manera idempotente. Publica `PropertyAddressUpdatedEvent`.        |
| `UpdateGeolocation(newGeolocation)`                 | Actualiza la geolocalización. Publica `PropertyGeolocationUpdatedEvent`.                    |
| `Archive(reason)`                                   | Archiva la propiedad. Lanza `InvalidOperationException` si hay dispositivos IoT asignados. Publica `PropertyArchivedEvent`. |
| `RecordMaintenance(serviceId, summary, completedAt)`| Registra un evento de mantenimiento. Publica `PropertyMaintenanceRecordedEvent`.            |
| `AddInstalledDevice(deviceId)`                      | Añade un dispositivo a la lista de manera idempotente. Actualiza `HasActiveIoTMonitoring`. |
| `RemoveInstalledDevice(deviceId)`                   | Elimina un dispositivo de la lista. Recalcula `HasActiveIoTMonitoring` automáticamente.    |

**Invariantes (IoT):** Una propiedad no puede archivarse si tiene dispositivos en estado `Assigned` o `Installed`. `HasActiveIoTMonitoring` se recalcula automáticamente al añadir o eliminar un `deviceId`.

---

#### `PropertyPortfolio`

Agrupa las propiedades de un propietario o empresa con metadatos de gestión de portafolio.

**Atributos:**

| Atributo  | Tipo                              | Descripción                                                        |
|-----------|-----------------------------------|--------------------------------------------------------------------|
| `Id`      | `PropertyPortfolioId`             | Identificador único con prefijo `portf-{Guid}`                     |
| `OwnerId` | `OwnerId`                         | Referencia al propietario (único por portafolio)                   |
| `Status`  | `EPortfolioStatus`                | Estado del portafolio (`Empty`, `Active`)                          |
| `Entries` | `IReadOnlyCollection<PortfolioEntry>` | Entradas que vinculan propiedades al portafolio               |

**Métodos de negocio:**

| Método                                              | Descripción                                                                          |
|-----------------------------------------------------|--------------------------------------------------------------------------------------|
| `Create(ownerId)`                                   | Factory method. Inicializa portafolio vacío. Publica `PropertyPortfolioCreatedEvent`. |
| `AddProperty(propertyId, nickname, isPrimary, occupancyStatus)` | Añade una propiedad validando unicidad de `propertyId` y `nickname`. Publica `PropertyAddedToPortfolioEvent`. |
| `RemoveProperty(propertyId, reason)`                | Elimina la entrada. Si queda vacío, cambia estado a `Empty`. Publica `PropertyRemovedFromPortfolioEvent`. |

---

#### `IoTDevice`

Representa el ciclo de vida completo de un dispositivo IoT físico de ElectroLink desde que sale de bodega hasta su decomisionamiento. Es el mecanismo de autenticación contra el Edge API mediante `apiKeyHash`. El dispositivo siempre pertenece a ElectroLink y nunca se transfiere al cliente.

**Ciclo de vida:** `InStock → Assigned → Installed → Maintenance → Installed` (retorno) o `[Cualquiera] → Decommissioned`.

**Atributos:**

| Atributo                   | Tipo                   | Descripción                                                               |
|----------------------------|------------------------|---------------------------------------------------------------------------|
| `Id`                       | `IoTDeviceId`          | Identificador único con prefijo `dev-{Guid}`                              |
| `SerialNumber`             | `SerialNumber`         | Número de serie físico. Único e inmutable tras el registro                |
| `ApiKeyHash`               | `ApiKeyHash`           | Hash BCrypt de la clave de API. El texto plano nunca persiste             |
| `FirmwareVersion`          | `string`               | Versión de firmware actualmente instalada en el dispositivo               |
| `Status`                   | `EDeviceStatus`        | Estado del ciclo de vida (`InStock`, `Assigned`, `Installed`, `Maintenance`, `Decommissioned`) |
| `AssignedPropertyId`       | `PropertyId?`          | Referencia a la propiedad asignada. Nulo si está en bodega                |
| `InstallationRequestId`    | `InstallationRequestId?` | Referencia cruzada a la solicitud de instalación en Service Design BC   |
| `InstalledByTechnicianId`  | `TechnicianId?`        | Técnico que realizó la instalación física                                 |
| `InstalledAt`              | `DateTime?`            | Fecha y hora de instalación física confirmada                             |
| `ConnectionStatus`         | `EConnectionStatus`    | Estado de conectividad (`Connected`, `Disconnected`)                      |
| `LastReadingAt`            | `DateTime?`            | Fecha de la última lectura registrada por el Edge API                     |
| `MaintenanceReason`        | `string?`              | Motivo del retiro a mantenimiento                                         |
| `ExpectedReturnDate`       | `DateTime?`            | Fecha estimada de retorno del mantenimiento                               |

**Métodos de negocio:**

| Método                                                 | Descripción                                                                                     |
|--------------------------------------------------------|-------------------------------------------------------------------------------------------------|
| `Register(serialNumber, apiKeyHash, firmwareVersion)`  | Factory method. Inicializa dispositivo con `Status = InStock`. Publica `IoTDeviceRegisteredEvent`. |
| `AssignToProperty(propertyId, installationRequestId)` | Transición `InStock → Assigned`. Valida estado previo. Publica `DeviceAssignedToPropertyEvent`. |
| `RecordInstallation(technicianId, propertyId, firmwareVersion, installedAt)` | Transición `Assigned → Installed`. Valida estado y consistencia de `propertyId`. Publica `DeviceInstalledEvent`. |
| `UpdateConnectionStatus(newStatus, lastReadingAt)`    | Actualiza estado de conectividad solo si el dispositivo está en `Installed` o `Maintenance`. Publica `DeviceConnectionStatusUpdatedEvent`. |
| `SendToMaintenance(reason, expectedReturnDate)`        | Transición `Installed → Maintenance`. Fuerza `ConnectionStatus = Disconnected`. Publica `DeviceSentToMaintenanceEvent`. |
| `Reinstall(technicianId, firmwareVersion, reinstalledAt)` | Transición `Maintenance → Installed`. Limpia campos de mantenimiento. Publica `DeviceReinstalledEvent`. |
| `Decommission(reason)`                                 | Transición `[Cualquiera] → Decommissioned`. Idempotente. Invalida el `apiKeyHash` efectivamente. Publica `DeviceDecommissionedEvent`. |

**Invariantes:** `SerialNumber` es único e inmutable. El `apiKeyHash` es generado una única vez y nunca se expone en texto plano tras el registro. Las transiciones de estado deben seguir el grafo válido definido. `ConnectionStatus` solo puede modificarse en estado `Installed` o `Maintenance`. Los dispositivos en `MAINTENANCE` cuentan para facturación (decisión MVP, Hotspot C-4).

---

### Entities

Las entidades son objetos con identidad que viven dentro de un Aggregate y cuya existencia está subordinada a su raíz.

#### `ComponentStock`

Registra la cantidad disponible y reservada de un componente específico dentro del inventario de un técnico.

| Atributo                | Tipo                    | Descripción                                            |
|-------------------------|-------------------------|--------------------------------------------------------|
| `Id`                    | `ComponentStockId`      | Identificador con prefijo `stock-{Guid}`               |
| `TechnicianInventoryId` | `TechnicianInventoryId` | Referencia al inventario contenedor                    |
| `ComponentId`           | `ComponentId`           | Referencia al componente catalogado                    |
| `QuantityAvailable`     | `int`                   | Cantidad total disponible en stock                     |
| `ReservedQuantity`      | `int`                   | Cantidad actualmente reservada para servicios          |
| `AlertThreshold`        | `int`                   | Umbral mínimo que dispara alerta de bajo stock         |
| `LastUpdated`           | `DateTime`              | Marca temporal de la última modificación               |
| `AvailableForReservation` | `int` (calculado)    | `QuantityAvailable - ReservedQuantity`                 |

#### `ComponentReservation`

Agrupa los ítems reservados para un servicio específico con tiempo de vida de 72 horas.

| Atributo      | Tipo                    | Descripción                                               |
|---------------|-------------------------|-----------------------------------------------------------|
| `Id`          | `ComponentReservationId`| Identificador con prefijo `res-{Guid}`                    |
| `InventoryId` | `TechnicianInventoryId` | Inventario al que pertenece la reserva                    |
| `ServiceId`   | `ServiceId`             | Referencia al servicio para el que se efectúa la reserva  |
| `ExpiresAt`   | `DateTime`              | Fecha y hora de expiración de la reserva (72 horas)       |
| `IsConsumed`  | `bool`                  | Indica si los componentes ya fueron consumidos            |
| `IsReleased`  | `bool`                  | Indica si la reserva fue liberada                         |
| `ConsumedAt`  | `DateTime?`             | Fecha de consumo efectivo                                 |
| `ReleasedAt`  | `DateTime?`             | Fecha de liberación                                       |
| `Items`       | `IReadOnlyCollection<ReservationItem>` | Ítems detallados de la reserva             |

#### `ReservationItem`

Detalla la cantidad reservada de un componente individual dentro de una `ComponentReservation`.

| Atributo       | Tipo                    | Descripción                                            |
|----------------|-------------------------|--------------------------------------------------------|
| `Id`           | `ReservationItemId`     | Identificador con prefijo `ritem-{Guid}`               |
| `ReservationId`| `ComponentReservationId`| Referencia a la reserva contenedora                   |
| `ComponentId`  | `ComponentId`           | Componente reservado                                   |
| `Quantity`     | `int`                   | Cantidad reservada de ese componente                   |

#### `PortfolioEntry`

Representa la asociación entre un portafolio y una propiedad, con metadatos propios del vínculo.

| Atributo         | Tipo                  | Descripción                                              |
|------------------|-----------------------|----------------------------------------------------------|
| `Id`             | `PortfolioEntryId`    | Identificador con prefijo `pentry-{Guid}`                |
| `PortfolioId`    | `PropertyPortfolioId` | Referencia al portafolio                                 |
| `PropertyId`     | `PropertyId`          | Referencia a la propiedad                                |
| `Nickname`       | `string`              | Alias único dentro del portafolio (ej. "Casa Principal") |
| `IsPrimary`      | `bool`                | Indica si es la propiedad principal del portafolio       |
| `OccupancyStatus`| `OccupancyStatus`     | Estado de ocupación del inmueble                         |

---

### Value Objects

| Value Object           | Descripción                                                                                                |
|------------------------|------------------------------------------------------------------------------------------------------------|
| `ComponentTypeId`      | ID tipado con prefijo `ctype-{Guid}`. Inmutable y validado en construcción.                               |
| `ComponentId`          | ID tipado con prefijo `comp-{Guid}`.                                                                       |
| `TechnicianInventoryId`| ID tipado con prefijo `inv-{Guid}`.                                                                        |
| `ComponentStockId`     | ID tipado con prefijo `stock-{Guid}`.                                                                      |
| `ComponentReservationId`| ID tipado con prefijo `res-{Guid}`.                                                                       |
| `ReservationItemId`    | ID tipado con prefijo `ritem-{Guid}`.                                                                      |
| `PropertyId`           | ID tipado con prefijo `prop-{Guid}`.                                                                       |
| `PropertyPortfolioId`  | ID tipado con prefijo `portf-{Guid}`.                                                                      |
| `PortfolioEntryId`     | ID tipado con prefijo `pentry-{Guid}`.                                                                     |
| `IoTDeviceId`          | ID tipado con prefijo `dev-{Guid}`.                                                                        |
| `TechnicianId`         | ID de referencia cruzada hacia el BC de Profiles.                                                          |
| `OwnerId`              | ID de referencia cruzada hacia el BC de Profiles.                                                          |
| `ServiceId`            | ID de referencia cruzada hacia el BC de Service Design.                                                    |
| `InstallationRequestId`| ID de referencia cruzada hacia el BC de Service Design (solicitud de instalación IoT).                     |
| `SerialNumber`         | Protege la invariante de no-vacío y límite de longitud del número de serie del dispositivo.                |
| `ApiKeyHash`           | Encapsula el proceso de hashing con BCrypt y la verificación. Nunca expone el texto plano.                 |
| `Geolocation`          | Contiene latitud (−90 a 90), longitud (−180 a 180), precisión y fuente (`GPS`, `MANUAL`, `GEOCODING`).    |
| `Region`               | Nombre de la región geográfica.                                                                            |
| `District`             | Nombre del distrito geográfico.                                                                            |
| `Money`                | Monto y moneda para precios de componentes.                                                                |
| `EComponentStatus`     | Enumeración: `Active`, `Inactive`.                                                                         |
| `EInventoryStatus`     | Enumeración: `Empty`, `Active`, `Inactive`.                                                                |
| `EPortfolioStatus`     | Enumeración: `Empty`, `Active`.                                                                            |
| `EPropertyStatus`      | Enumeración: `Created`, `Active`, `Inactive`, `Archived`.                                                  |
| `EDeviceStatus`        | Enumeración: `InStock`, `Assigned`, `Installed`, `Maintenance`, `Decommissioned`.                          |
| `EConnectionStatus`    | Enumeración: `Connected`, `Disconnected`.                                                                  |
| `OccupancyStatus`      | Estado de ocupación del inmueble.                                                                          |

---

### Domain Services (Interfaces)

Las interfaces de servicios de dominio definen los contratos que implementa la capa de Application. Siguen el patrón CQRS, separando responsabilidades de escritura (Command Services) y lectura (Query Services).

#### Command Services

| Interfaz                          | Descripción                                                                                          |
|-----------------------------------|------------------------------------------------------------------------------------------------------|
| `IComponentTypeCommandService`    | Define las operaciones de creación, actualización y desactivación sobre el agregado `ComponentType`. |
| `IComponentCommandService`        | Define las operaciones de creación, actualización, activación y desactivación de `Component`.        |
| `ITechnicianInventoryCommandService` | Define las operaciones sobre el inventario: creación, gestión de stock, reservas y consumos.      |
| `IPropertyCommandService`         | Define las operaciones sobre propiedades: creación, actualización, archivado y registro de mantenimiento. |
| `IPropertyPortfolioCommandService`| Define las operaciones sobre portafolios: creación, adición y eliminación de propiedades.            |
| `IIoTDeviceCommandService`        | Define el ciclo de vida completo del dispositivo IoT: registro, asignación, instalación, mantenimiento, reinstalación y decomisionamiento. |

#### Query Services

| Interfaz                          | Descripción                                                                                          |
|-----------------------------------|------------------------------------------------------------------------------------------------------|
| `IComponentTypeQueryService`      | Define consultas sobre el catálogo de tipos de componentes.                                          |
| `IComponentQueryService`          | Define consultas individuales y por tipo sobre componentes.                                          |
| `ITechnicianInventoryQueryService`| Define consultas sobre inventarios por técnico.                                                      |
| `IPropertyQueryService`           | Define consultas sobre propiedades por ID y por propietario.                                         |
| `IPropertyPortfolioQueryService`  | Define consultas sobre portafolios por propietario.                                                  |
| `IIoTDeviceQueryService`          | Define consultas por ID, estado, propiedad y conteo de dispositivos activos para facturación.        |

---

### Repositories (Interfaces)

Los contratos de repositorio definen las operaciones de persistencia que la capa de Infrastructure implementa mediante EF Core.

| Interfaz                       | Descripción                                                                                          |
|--------------------------------|------------------------------------------------------------------------------------------------------|
| `IComponentTypeRepository`     | Contrato para persistencia y consultas sobre la tabla `arm_component_types`.                         |
| `IComponentRepository`         | Contrato para persistencia y consultas sobre la tabla `arm_components`.                              |
| `ITechnicianInventoryRepository`| Contrato para persistencia y consultas sobre inventarios, stocks y reservas.                        |
| `IPropertyRepository`          | Contrato para persistencia y consultas sobre la tabla `arm_properties`.                              |
| `IPropertyPortfolioRepository` | Contrato para persistencia y consultas sobre portafolios y entradas.                                 |
| `IIoTDeviceRepository`         | Contrato para persistencia y consultas especializadas sobre `arm_iot_devices`. Incluye métodos: `FindBySerialNumberAsync`, `FindByStatusAsync`, `FindByPropertyIdAsync`, `CountActiveByOwnerIdAsync` y `FindFirstInStockAsync`. |

---

#### 4.2.3.2. Interface Layer.

Interface Layer

La capa de interfaces expone los casos de uso del Bounded Context a través de una API RESTful. Está compuesta por Resources (entrada y salida), Assemblers que transforman entre representaciones, y Controllers que orquestan las solicitudes HTTP.

---

### Resources de Entrada (Command Resources)

#### Gestión de Tipos de Componentes

| Clase                       | Atributo      | Tipo     | Descripción                                   |
|-----------------------------|---------------|----------|-----------------------------------------------|
| `CreateComponentTypeResource` | `Name`      | `string` | Nombre del nuevo tipo de componente           |
|                              | `Description` | `string` | Descripción del tipo de componente            |
| `UpdateComponentTypeResource` | `Name`      | `string` | Nuevo nombre del tipo de componente           |
|                              | `Description` | `string` | Nueva descripción                             |

#### Gestión de Componentes

| Clase                    | Atributo      | Tipo     | Descripción                                        |
|--------------------------|---------------|----------|----------------------------------------------------|
| `CreateComponentResource` | `TypeId`     | `string` | ID del tipo de componente al que pertenece         |
|                          | `Name`        | `string` | Nombre del componente                              |
|                          | `Description` | `string` | Descripción del componente                         |
| `UpdateComponentResource` | `Name`       | `string` | Nuevo nombre del componente                        |
|                          | `Description` | `string` | Nueva descripción                                  |
|                          | `IsActive`    | `bool`   | Estado de activación del componente                |

#### Gestión de Inventarios

| Clase                         | Atributo         | Tipo     | Descripción                                      |
|-------------------------------|------------------|----------|--------------------------------------------------|
| `AddStockToInventoryResource` | `ComponentId`    | `string` | ID del componente a incorporar al stock          |
|                               | `Quantity`       | `int`    | Cantidad inicial en stock                        |
|                               | `AlertThreshold` | `int`    | Umbral de alerta de bajo stock                   |
| `UpdateComponentStockResource`| `Quantity`       | `int`    | Nueva cantidad disponible                        |
|                               | `AlertThreshold` | `int`    | Nuevo umbral de alerta                           |

#### Gestión de Propiedades

| Clase                       | Atributo      | Tipo     | Descripción                                       |
|-----------------------------|---------------|----------|---------------------------------------------------|
| `CreatePropertyResource`    | `OwnerId`     | `string` | ID del propietario del inmueble                   |
|                             | `Street`      | `string` | Calle de la dirección postal                      |
|                             | `City`        | `string` | Ciudad de la dirección postal                     |
|                             | `Country`     | `string` | País de la dirección postal                       |
|                             | `Latitude`    | `double` | Latitud geográfica                                |
|                             | `Longitude`   | `double` | Longitud geográfica                               |
|                             | `Region`      | `string` | Región geográfica                                 |
|                             | `District`    | `string` | Distrito geográfico                               |
| `UpdateGeolocationResource` | `Latitude`    | `double` | Nueva latitud                                     |
|                             | `Longitude`   | `double` | Nueva longitud                                    |
|                             | `Accuracy`    | `double` | Precisión de la medición                          |
|                             | `Source`      | `string` | Fuente de la geolocalización (`GPS`, `MANUAL`, etc.) |

#### Gestión de Dispositivos IoT

| Clase                                | Atributo                | Tipo        | Descripción                                                                 |
|--------------------------------------|-------------------------|-------------|-----------------------------------------------------------------------------|
| `RegisterIoTDeviceResource`          | `SerialNumber`          | `string`    | Número de serie físico del dispositivo                                      |
|                                      | `ApiKey`                | `string`    | Clave de API en texto plano. Se hashea en el servicio; nunca persiste.      |
|                                      | `FirmwareVersion`       | `string`    | Versión de firmware del dispositivo                                         |
| `AssignDeviceToPropertyResource`     | `DeviceId`              | `string`    | ID del dispositivo a asignar                                                |
|                                      | `PropertyId`            | `string`    | ID de la propiedad destino                                                  |
|                                      | `InstallationRequestId` | `string`    | ID de la solicitud de instalación en Service Design BC                      |
| `RecordDeviceInstallationResource`   | `DeviceId`              | `string`    | ID del dispositivo instalado físicamente                                    |
|                                      | `PropertyId`            | `string`    | ID de la propiedad donde se realizó la instalación                         |
|                                      | `TechnicianId`          | `string`    | ID del técnico que ejecutó la instalación                                   |
|                                      | `InstallationRequestId` | `string`    | ID de la solicitud de instalación                                           |
|                                      | `FirmwareVersion`       | `string`    | Versión de firmware al momento de la instalación                            |
|                                      | `InstalledAt`           | `DateTime`  | Fecha y hora de instalación física                                          |
| `UpdateDeviceConnectionStatusResource` | `DeviceId`            | `string`    | ID del dispositivo                                                          |
|                                      | `NewConnectionStatus`   | `string`    | Nuevo estado: `"Connected"` o `"Disconnected"`                              |
|                                      | `LastReadingAt`         | `DateTime?` | Fecha de la última lectura recibida                                         |
| `SendDeviceToMaintenanceResource`    | `DeviceId`              | `string`    | ID del dispositivo a enviar a mantenimiento                                 |
|                                      | `Reason`                | `string`    | Motivo del retiro a mantenimiento                                           |
|                                      | `ExpectedReturnDate`    | `DateTime?` | Fecha estimada de retorno                                                   |
| `ReinstallDeviceResource`            | `DeviceId`              | `string`    | ID del dispositivo reinstalado                                              |
|                                      | `TechnicianId`          | `string`    | ID del técnico que realizó la reinstalación                                 |
|                                      | `FirmwareVersion`       | `string`    | Versión de firmware instalada en la reinstalación                           |
|                                      | `ReinstalledAt`         | `DateTime`  | Fecha y hora de reinstalación                                               |
| `DecommissionDeviceResource`         | `DeviceId`              | `string`    | ID del dispositivo a decomisionar                                           |
|                                      | `Reason`                | `string`    | Motivo del decomisionamiento                                                |

---

### Resources de Salida (Response Resources)

| Clase                       | Atributos principales                                                                                  |
|-----------------------------|--------------------------------------------------------------------------------------------------------|
| `ComponentTypeResource`     | `ComponentTypeId`, `Name`, `Description`, `IsActive`                                                   |
| `ComponentResource`         | `ComponentId`, `TypeId`, `Name`, `Description`, `IsActive`                                             |
| `TechnicianInventoryResource` | `InventoryId`, `TechnicianId`, `Status`, colección de stock con cantidades                           |
| `PropertyResource`          | `PropertyId`, `OwnerId`, campos de dirección, campos de geolocalización, `Status`, `IsActive`, `HasActiveIoTMonitoring` |
| `PropertyPortfolioResource` | `PortfolioId`, `OwnerId`, `Status`, colección de `PortfolioEntry`                                      |
| `IoTDeviceResource`         | `DeviceId`, `SerialNumber`, `FirmwareVersion`, `Status`, `ConnectionStatus`, `AssignedPropertyId?`, `InstallationRequestId?`, `InstalledByTechnicianId?`, `InstalledAt?`, `LastReadingAt?`, `MaintenanceReason?`, `ExpectedReturnDate?` |
| `ActiveDeviceCountResource` | `OwnerId`, `ActiveDeviceCount`                                                                         |

---

### Assemblers (Transforms)

Los Assemblers implementan la transformación bidireccional entre Resources y objetos de dominio, manteniendo desacopladas ambas representaciones.

| Clase                                              | Método                   | Descripción                                                                  |
|----------------------------------------------------|--------------------------|------------------------------------------------------------------------------|
| `CreateComponentTypeCommandFromResourceAssembler`  | `ToCommandFromResource`  | Transforma `CreateComponentTypeResource` en `CreateComponentTypeCommand`.    |
| `ComponentTypeResourceFromEntityAssembler`         | `ToResourceFromEntity`   | Transforma la entidad `ComponentType` en `ComponentTypeResource`.            |
| `CreateComponentCommandFromResourceAssembler`      | `ToCommandFromResource`  | Transforma `CreateComponentResource` en `CreateComponentCommand`.            |
| `ComponentResourceFromEntityAssembler`             | `ToResourceFromEntity`   | Transforma la entidad `Component` en `ComponentResource`.                    |
| `AddStockToInventoryCommandFromResourceAssembler`  | `ToCommandFromResource`  | Transforma `AddStockToInventoryResource` en `AddStockToInventoryCommand`.    |
| `TechnicianInventoryResourceFromEntityAssembler`   | `ToResourceFromEntity`   | Transforma el agregado `TechnicianInventory` en `TechnicianInventoryResource`. |
| `CreatePropertyCommandFromResourceAssembler`       | `ToCommandFromResource`  | Transforma `CreatePropertyResource` en `CreatePropertyCommand`.              |
| `UpdatePropertyCommandFromResourceAssembler`       | `ToCommandFromResource`  | Transforma `UpdatePropertyResource` en `UpdatePropertyCommand`.              |
| `PropertyResourceFromEntityAssembler`              | `ToResourceFromEntity`   | Transforma el agregado `Property` en `PropertyResource`.                     |
| `PropertyPortfolioResourceFromEntityAssembler`     | `ToResourceFromEntity`   | Transforma el agregado `PropertyPortfolio` en `PropertyPortfolioResource`.   |
| `RegisterIoTDeviceCommandFromResourceAssembler`    | `ToCommandFromResource`  | Transforma `RegisterIoTDeviceResource` en `RegisterIoTDeviceCommand`.        |
| `RecordInstallationCommandFromResourceAssembler`   | `ToCommandFromResource`  | Transforma `RecordDeviceInstallationResource` en `RecordDeviceInstallationCommand`. |
| `IoTDeviceResourceFromEntityAssembler`             | `ToResourceFromEntity`   | Transforma el agregado `IoTDevice` en `IoTDeviceResource`.                   |

---

### Controllers

#### `ComponentTypesController`

Ruta base: `/api/v1/component-types`

| Método HTTP | Ruta                     | Descripción                                          |
|-------------|--------------------------|------------------------------------------------------|
| `POST`      | `/`                      | Crea un nuevo tipo de componente en el catálogo      |
| `GET`       | `/`                      | Obtiene todos los tipos de componentes activos       |
| `GET`       | `/{componentTypeId}`     | Obtiene un tipo de componente por su ID              |
| `PUT`       | `/{componentTypeId}`     | Actualiza un tipo de componente existente            |
| `DELETE`    | `/{componentTypeId}`     | Desactiva un tipo de componente                      |

#### `ComponentsController`

Ruta base: `/api/v1/components`

| Método HTTP | Ruta                       | Descripción                                                |
|-------------|----------------------------|------------------------------------------------------------|
| `POST`      | `/`                        | Crea un nuevo componente referenciando un tipo             |
| `GET`       | `/`                        | Obtiene todos los componentes del catálogo                 |
| `GET`       | `/{componentId}`           | Obtiene un componente por su ID                            |
| `GET`       | `/by-type/{typeId}`        | Obtiene todos los componentes de un tipo específico        |
| `PUT`       | `/{componentId}`           | Actualiza la información de un componente                  |
| `DELETE`    | `/{componentId}`           | Desactiva un componente del catálogo                       |

#### `TechnicianInventoriesController`

Ruta base: `/api/v1/technician-inventories`

| Método HTTP | Ruta                                              | Descripción                                            |
|-------------|---------------------------------------------------|--------------------------------------------------------|
| `GET`       | `/by-technician/{technicianId}`                   | Obtiene el inventario de un técnico                    |
| `POST`      | `/{inventoryId}/stock`                            | Añade un nuevo ítem de stock al inventario             |
| `PUT`       | `/{inventoryId}/stock/{componentId}/increase`     | Incrementa la cantidad disponible de un componente     |
| `PUT`       | `/{inventoryId}/stock/{componentId}/decrease`     | Decrementa la cantidad disponible de un componente     |
| `DELETE`    | `/{inventoryId}/stock/{componentId}`              | Elimina un ítem de stock del inventario                |

#### `PropertiesController`

Ruta base: `/api/v1/properties`

| Método HTTP | Ruta                                | Descripción                                                       |
|-------------|-------------------------------------|-------------------------------------------------------------------|
| `POST`      | `/`                                 | Registra una nueva propiedad para un propietario                  |
| `GET`       | `/{propertyId}`                     | Obtiene una propiedad por su ID                                   |
| `GET`       | `/by-owner/{ownerId}`               | Obtiene todas las propiedades de un propietario                   |
| `PUT`       | `/{propertyId}`                     | Actualiza los datos generales de una propiedad                    |
| `PUT`       | `/{propertyId}/address`             | Actualiza la dirección postal de una propiedad                    |
| `PUT`       | `/{propertyId}/geolocation`         | Actualiza la geolocalización de una propiedad                     |
| `DELETE`    | `/{propertyId}`                     | Archiva una propiedad. Retorna `409 Conflict` si tiene dispositivos IoT activos. |

#### `PropertyPortfoliosController`

Ruta base: `/api/v1/property-portfolios`

| Método HTTP | Ruta                                          | Descripción                                          |
|-------------|-----------------------------------------------|------------------------------------------------------|
| `GET`       | `/by-owner/{ownerId}`                         | Obtiene el portafolio de propiedades de un propietario |
| `POST`      | `/{portfolioId}/properties`                   | Añade una propiedad al portafolio                    |
| `DELETE`    | `/{portfolioId}/properties/{propertyId}`      | Elimina una propiedad del portafolio                 |

#### `IoTDevicesController`

Ruta base: `/api/v1/iot-devices`

| Método HTTP | Ruta                                         | Descripción                                                                                      |
|-------------|----------------------------------------------|--------------------------------------------------------------------------------------------------|
| `POST`      | `/`                                          | Registra un nuevo dispositivo IoT. Retorna `201 Created` con el API key en texto plano (única vez). |
| `GET`       | `/`                                          | Lista todos los dispositivos IoT del sistema (rol Admin)                                         |
| `GET`       | `/{deviceId}`                                | Obtiene un dispositivo por su ID                                                                 |
| `GET`       | `/by-property/{propertyId}`                  | Lista los dispositivos asignados a una propiedad                                                 |
| `GET`       | `/by-status/{status}`                        | Filtra dispositivos por estado del ciclo de vida                                                 |
| `GET`       | `/active-count/by-owner/{ownerId}`           | Retorna el conteo de dispositivos activos para facturación empresarial                           |
| `POST`      | `/assign`                                    | Asigna un dispositivo a una propiedad (`InStock → Assigned`)                                    |
| `POST`      | `/record-installation`                       | Registra la instalación física de un dispositivo (`Assigned → Installed`)                        |
| `PUT`       | `/{deviceId}/connection-status`              | Actualiza el estado de conectividad del dispositivo                                              |
| `POST`      | `/{deviceId}/send-to-maintenance`            | Envía un dispositivo a mantenimiento (`Installed → Maintenance`)                                 |
| `POST`      | `/{deviceId}/reinstall`                      | Reinstala un dispositivo desde mantenimiento (`Maintenance → Installed`)                         |
| `DELETE`    | `/{deviceId}`                                | Decomisiona un dispositivo (`[Cualquiera] → Decommissioned`)                                    |

---

#### 4.2.3.3. Application Layer.

La capa de aplicación orquesta los casos de uso del Bounded Context implementando las interfaces de servicio definidas en el dominio. Opera bajo el patrón CQRS mediante Command Services y Query Services, sin contener lógica de negocio propia. Adicionalmente, gestiona los Event Handlers para la integración con otros Bounded Contexts.

---

### Command Services

| Clase                              | Interfaz Implementada              | Descripción                                                                                                 |
|------------------------------------|------------------------------------|-------------------------------------------------------------------------------------------------------------|
| `ComponentTypeCommandService`      | `IComponentTypeCommandService`     | Orquesta la creación, actualización y desactivación de tipos de componentes. Delega al agregado y persiste mediante el repositorio y la unidad de trabajo. |
| `ComponentCommandService`          | `IComponentCommandService`         | Orquesta la creación y actualización de componentes, validando la existencia del tipo referenciado.         |
| `TechnicianInventoryCommandService`| `ITechnicianInventoryCommandService` | Orquesta la inicialización de inventarios, la gestión de stock y las operaciones de reserva y consumo. Valida disponibilidad atómica antes de reservar. |
| `PropertyCommandService`           | `IPropertyCommandService`          | Orquesta el registro, actualización y archivado de propiedades. Verifica la invariante de archivado IoT antes de proceder. |
| `PropertyPortfolioCommandService`  | `IPropertyPortfolioCommandService` | Orquesta la gestión del portafolio de propiedades por propietario.                                          |
| `IoTDeviceCommandService`          | `IIoTDeviceCommandService`         | Orquesta el ciclo de vida completo del dispositivo IoT. En `AssignDeviceToPropertyCommand` actualiza simultáneamente `IoTDevice` y `Property` en la misma transacción. En `DecommissionDeviceCommand` elimina el dispositivo de la propiedad antes de decomisionar. Publica eventos de dominio tras confirmar la persistencia. |

---

### Query Services

| Clase                             | Interfaz Implementada             | Descripción                                                                                     |
|-----------------------------------|-----------------------------------|-------------------------------------------------------------------------------------------------|
| `ComponentTypeQueryService`       | `IComponentTypeQueryService`      | Recupera tipos de componentes por ID o listado completo.                                        |
| `ComponentQueryService`           | `IComponentQueryService`          | Recupera componentes por ID, por tipo o por lista de IDs.                                       |
| `TechnicianInventoryQueryService` | `ITechnicianInventoryQueryService`| Recupera el inventario de un técnico incluyendo ítems de stock y reservas activas.              |
| `PropertyQueryService`            | `IPropertyQueryService`           | Recupera propiedades por ID, por propietario o consultas de dirección.                          |
| `PropertyPortfolioQueryService`   | `IPropertyPortfolioQueryService`  | Recupera el portafolio de propiedades de un propietario.                                        |
| `IoTDeviceQueryService`           | `IIoTDeviceQueryService`          | Recupera dispositivos por ID, estado o propiedad. Implementa `GetActiveDeviceCountByOwnerIdQuery` que agrega propiedades del propietario y cuenta dispositivos en estado `Installed` o `Maintenance`. |

---

### Event Handlers

La capa de aplicación contiene Event Handlers para dos categorías de eventos: eventos de dominio internos y eventos externos provenientes de otros Bounded Contexts.

#### Handlers de Eventos Externos (Inbound Cross-BC)

| Clase                                           | Evento Consumido                        | Acción Orquestada                                                                          |
|-------------------------------------------------|-----------------------------------------|--------------------------------------------------------------------------------------------|
| `ProfileCompletedEventHandler`                  | `ProfileCompleted` (Profiles BC)        | Crea `TechnicianInventory` si el perfil es de tipo técnico, o `PropertyPortfolio` si es propietario o empresa. |
| `IoTInstallationServiceScheduledEventHandler`   | `IoTInstallationServiceScheduled` (Service Design BC) | Selecciona el primer dispositivo `InStock` disponible (FIFO) y ejecuta `AssignDeviceToPropertyCommand`. |
| `ServiceCompletedIoTInstallationEventHandler`   | `ServiceCompleted` con `serviceType == IOT_INSTALLATION` (Service Operation BC) | Ejecuta `RecordDeviceInstallationCommand` y registra el mantenimiento en la propiedad. |
| `DeviceDisconnectedEventHandler`                | `DeviceDisconnected` (IoT Monitoring BC)| Ejecuta `UpdateDeviceConnectionStatusCommand` con `EConnectionStatus.Disconnected`.        |
| `DeviceReconnectedEventHandler`                 | `DeviceReconnected` (IoT Monitoring BC) | Ejecuta `UpdateDeviceConnectionStatusCommand` con `EConnectionStatus.Connected`.           |

#### Handlers de Eventos de Dominio Internos (Outbound)

| Clase                                        | Evento Publicado                   | Descripción                                                                        |
|----------------------------------------------|------------------------------------|------------------------------------------------------------------------------------|
| `DeviceInstalledEventHandler`                | `DeviceInstalledEvent`             | Registra en log la necesidad de activación de plan en Subscriptions BC e inicio de stream en IoT Monitoring BC. |
| `DeviceDecommissionedEventHandler`           | `DeviceDecommissionedEvent`        | Registra en log la invalidación del `apiKeyHash` y la necesidad de ajuste de facturación en Subscriptions BC. |
| `ComponentStockLowEventHandler`              | `ComponentStockLowEvent`           | Registra en log la alerta de bajo stock para notificación al técnico.              |
| `PropertyGeolocationUpdatedEventHandler`     | `PropertyGeolocationUpdatedEvent`  | Registra en log la necesidad de recalcular técnicos cercanos en Service Design BC. |

---

#### 4.2.3.4. Infrastructure Layer

La capa de infraestructura provee las implementaciones concretas de los repositorios y las configuraciones de mapeo objeto-relacional mediante Entity Framework Core. Todas las clases de configuración heredan de `IEntityTypeConfiguration<T>` y se registran en `AppDbContext` a través de `ModelBuilderExtensions`.

---

### Implementación de los Repositories

| Clase                          | Interfaz Implementada            | Descripción                                                                                             |
|--------------------------------|----------------------------------|---------------------------------------------------------------------------------------------------------|
| `ComponentTypeRepository`      | `IComponentTypeRepository`       | Implementa consultas y persistencia sobre la tabla `arm_component_types`.                               |
| `ComponentRepository`          | `IComponentRepository`           | Implementa consultas y persistencia sobre la tabla `arm_components`.                                    |
| `TechnicianInventoryRepository`| `ITechnicianInventoryRepository` | Implementa consultas sobre inventarios con carga de `StockItems` y `Reservations` mediante `Include`.   |
| `PropertyRepository`           | `IPropertyRepository`            | Implementa consultas sobre propiedades con soporte para filtrado por `OwnerId`.                         |
| `PropertyPortfolioRepository`  | `IPropertyPortfolioRepository`   | Implementa consultas sobre portafolios con carga de `Entries` mediante `Include`.                       |
| `IoTDeviceRepository`          | `IIoTDeviceRepository`           | Implementa todos los métodos especializados: `FindBySerialNumberAsync`, `FindByStatusAsync`, `FindByPropertyIdAsync`, `CountActiveByOwnerIdAsync` (incluye `Installed` y `Maintenance`) y `FindFirstInStockAsync` (selección FIFO por ID). |

---

### Configuraciones de Persistencia (EF Core)

#### `IoTDeviceConfiguration`

Tabla: `arm_iot_devices`

| Columna                     | Tipo SQL        | Restricciones                              | Observaciones                                        |
|-----------------------------|-----------------|--------------------------------------------|------------------------------------------------------|
| `id`                        | `VARCHAR(60)`   | `PK`, `NOT NULL`                           | Prefijo `dev-{Guid}`                                 |
| `serial_number`             | `VARCHAR(100)`  | `NOT NULL`, `UNIQUE`                       | Convertidor: `SerialNumber ↔ string`                 |
| `api_key_hash`              | `VARCHAR(255)`  | `NOT NULL`                                 | Convertidor: `ApiKeyHash ↔ string`                   |
| `firmware_version`          | `VARCHAR(50)`   | `NOT NULL`                                 |                                                      |
| `status`                    | `VARCHAR(30)`   | `NOT NULL`                                 | Convertidor: `EDeviceStatus` como string             |
| `assigned_property_id`      | `VARCHAR(60)`   | Nullable                                   | Convertidor: `PropertyId? ↔ string?`                 |
| `installation_request_id`   | `VARCHAR(60)`   | Nullable                                   | Convertidor: `InstallationRequestId? ↔ string?`      |
| `installed_by_technician_id`| `VARCHAR(60)`   | Nullable                                   | Convertidor: `TechnicianId? ↔ string?`               |
| `installed_at`              | `DATETIME2`     | Nullable                                   |                                                      |
| `connection_status`         | `VARCHAR(20)`   | `NOT NULL`                                 | Convertidor: `EConnectionStatus` como string         |
| `last_reading_at`           | `DATETIME2`     | Nullable                                   |                                                      |
| `maintenance_reason`        | `VARCHAR(500)`  | Nullable                                   |                                                      |
| `expected_return_date`      | `DATETIME2`     | Nullable                                   |                                                      |

Índices adicionales: `idx_arm_iot_devices_status` sobre `status`, `idx_arm_iot_devices_property` sobre `assigned_property_id`.

#### `PropertyConfiguration` (extensión IoT)

Tabla: `arm_properties`

| Columna adicional             | Tipo SQL         | Restricciones               | Observaciones                                              |
|-------------------------------|------------------|-----------------------------|-------------------------------------------------------------|
| `installed_device_ids`        | `NVARCHAR(MAX)`  | `NOT NULL`, default `'[]'`  | Serializado como JSON. Convertidor: `List<string> ↔ JSON`  |
| `has_active_iot_monitoring`   | `BIT`            | `NOT NULL`, default `0`     | Derivado del contenido de `installed_device_ids`            |

#### Registro de Configuraciones

Todas las configuraciones son aplicadas mediante `ModelBuilderExtensions.ApplyAssetsConfigurations()`:

```csharp
modelBuilder.ApplyConfiguration(new ComponentTypeConfiguration());
modelBuilder.ApplyConfiguration(new ComponentConfiguration());
modelBuilder.ApplyConfiguration(new TechnicianInventoryConfiguration());
modelBuilder.ApplyConfiguration(new ComponentStockConfiguration());
modelBuilder.ApplyConfiguration(new ComponentReservationConfiguration());
modelBuilder.ApplyConfiguration(new ReservationItemConfiguration());
modelBuilder.ApplyConfiguration(new PropertyConfiguration());
modelBuilder.ApplyConfiguration(new PropertyPortfolioConfiguration());
modelBuilder.ApplyConfiguration(new PortfolioEntryConfiguration());
modelBuilder.ApplyConfiguration(new IoTDeviceConfiguration());
```

---

#### 4.2.3.5. Bounded Context Software Architecture Component Level Diagrams.
#### 4.2.3.6. Bounded Context Software Architecture Code Level Diagrams.
##### 4.2.3.6.1. Bounded Context Domain Layer Class Diagrams.

El siguiente diagrama representa las clases de la capa de dominio del Bounded Context Assets & Resources Management, incluyendo los Aggregate Roots, Entities y Value Objects con sus relaciones de composición y agregación.

##### 4.2.3.6.2. Bounded Context Database Design Diagram.

El siguiente diagrama ERD físico representa el esquema de base de datos del Bounded Context Assets & Resources Management.

### 4.2.4. Bounded Context: Service Design & Planning
#### 4.2.4.1. Domain Layer.

La capa de dominio del *Bounded Context* **Service Design and Planning** concentra las entidades, agregados, objetos de valor, comandos, eventos de dominio y los contratos de repositorio que protegen las invariantes del negocio. El modelo sigue los principios de **Domain-Driven Design (DDD)** bajo el patrón **CQRS** (*Command Query Responsibility Segregation*).

---

### Aggregates y Entities

#### ServiceCatalog *(Aggregate Root)*

**Responsabilidad:** Gestionar el catálogo de servicios de un técnico. Protege las invariantes de creación y modificación de *recipes*. Valida la certificación IoT cuando el *recipe* lo requiere

**Máquina de estados:** `EMPTY` → `ACTIVE` → `INACTIVE`

| Atributo | Tipo | Descripción |
|---|---|---|
| `CatalogId` | `CatalogId` | Identificador único del catálogo (prefijo `cat-`) |
| `TechnicianId` | `TechnicianId` | Referencia al técnico propietario (cross-BC) |
| `Status` | `ECatalogStatus` | Estado actual del catálogo (`Empty`, `Active`, `Inactive`) |
| `Recipes` | `IReadOnlyList<ServiceRecipe>` | Colección de recetas de servicio (entidades internas) |

| Método | Descripción |
|---|---|
| `Create(technicianId)` | Fábrica estática; crea un catálogo vacío y emite `ServiceCatalogCreatedEvent` |
| `AddRecipe(...)` | Agrega un nuevo *recipe*; valida unicidad del nombre, certificación IoT y tipos de componentes |
| `UpdateRecipe(...)` | Actualiza un *recipe* existente; aplica reglas de negocio sobre servicios activos |
| `DeactivateRecipe(recipeId, reason, inProgressCount)` | Desactiva un *recipe*; verifica que no haya servicios en ejecución |
| `DeactivateIoTRecipes(reason)` | Desactiva masivamente todos los *recipes* con `RequiresIoTCertification = true` |
| `ReactivateRecipe(recipeId)` | Reactiva un *recipe* previamente desactivado |

**Invariantes de dominio:**
- Un técnico solo puede tener un catálogo.
- Los nombres de *recipes* activos deben ser únicos dentro del catálogo.
- Las categorías `IOT_INSTALLATION` e `IOT_MAINTENANCE` siempre requieren `RequiresIoTCertification = true`.
- No se puede crear un *recipe* IoT si el técnico no posee certificación vigente.

---

#### ServiceRecipe *(Entity)*

**Responsabilidad:** Representar la definición técnica de un servicio ofrecido por el técnico. Entidad hija de `ServiceCatalog`.

**Máquina de estados:** `ACTIVE` → `INACTIVE` → `ACTIVE`

| Atributo | Tipo | Descripción |
|---|---|---|
| `Id` | `RecipeId` | Identificador único del *recipe* (prefijo `recipe-`) |
| `CatalogId` | `CatalogId` | Referencia al catálogo contenedor |
| `TechnicianId` | `TechnicianId` | Referencia al técnico propietario |
| `ServiceName` | `string` | Nombre único del servicio (máx. 200 caracteres) |
| `ServiceDescription` | `string` | Descripción técnica del servicio |
| `ServiceCategory` | `EServiceCategory` | Categoría del servicio |
| `RequiresIoTCertification` | `bool` | Indica si el servicio requiere técnico certificado IoT |
| `ComponentRequirements` | `IReadOnlyList<ComponentRequirementItem>` | Componentes requeridos con cantidades |
| `EstimatedDuration` | `EstimatedDuration` | Duración estimada en horas y minutos |
| `Pricing` | `ServicePricing` | Estructura de precios (materiales, mano de obra, total) |
| `Prerequisites` | `IReadOnlyList<string>` | Condiciones previas requeridas |
| `Deliverables` | `IReadOnlyList<string>` | Entregables comprometidos |
| `WarrantyPeriod` | `WarrantyPeriod` | Período de garantía en meses |
| `IsActive` | `bool` | Estado de activación del *recipe* |
| `TimesRequested` | `int` | Contador de veces solicitado |

| Método | Descripción |
|---|---|
| `Create(...)` | Fábrica estática; inicializa el *recipe* con todos sus atributos |
| `Update(...)` | Actualiza atributos modificables; valida reglas sobre servicios activos y aumento de precio |
| `Deactivate(reason, inProgressCount)` | Desactiva el *recipe*; verifica que no haya servicios en ejecución |
| `Reactivate()` | Reactiva el *recipe*; valida que esté inactivo |
| `IncrementTimesRequested()` | Incrementa el contador de solicitudes |

---

#### ServiceRequest *(Aggregate Root)*

**Responsabilidad:** Orquestar el asistente (*wizard*) de solicitud del propietario o empresa. Soporta tres tipos de solicitud: `STANDARD`, `IOT_INSTALLATION` y `SUGGESTED_BY_SYSTEM`.

**Máquina de estados:** `DRAFT` → `PROPERTY_SELECTED` → `CATEGORY_SELECTED` → `READY_TO_CONFIRM` → `PENDING_ASSIGNMENT` → `ASSIGNED` | `CANCELLED` | `EXPIRED`

| Atributo | Tipo | Descripción |
|---|---|---|
| `RequestId` | `RequestId` | Identificador único de la solicitud (prefijo `req-`) |
| `HomeownerId` | `HomeownerId?` | Referencia al propietario individual (cross-BC) |
| `CompanyId` | `CompanyId?` | Referencia a la empresa (cross-BC) |
| `ClientType` | `EClientType` | Tipo de cliente (`Individual`, `Company`) |
| `Status` | `ERequestStatus` | Estado actual de la solicitud |
| `RequestType` | `ERequestType` | Tipo de solicitud (`Standard`, `IotInstallation`, `SuggestedBySystem`) |
| `PropertyId` | `PropertyId?` | Propiedad seleccionada (cross-BC) |
| `Geolocation` | `Geolocation?` | Coordenadas de la propiedad |
| `HasIoTDevice` | `bool` | Indica si la propiedad tiene dispositivo IoT instalado |
| `InstalledDeviceId` | `DeviceId?` | Referencia al dispositivo IoT instalado (cross-BC) |
| `RequestedCategory` | `EServiceCategory?` | Categoría de servicio seleccionada |
| `RequiresIoTCertifiedTechnician` | `bool` | Indica si el técnico asignado debe ser certificado IoT |
| `Preferences` | `RequestPreferences?` | Preferencias de horario y descripción del problema |
| `ReceiptData` | `ReceiptData?` | Datos de consumo eléctrico (manual o desde sensor IoT) |
| `IsPriority` | `bool` | Indicador de solicitud prioritaria |
| `IoTContextSnapshot` | `IoTContextSnapshot?` | Instantánea inmutable del estado del sensor al confirmar |
| `RecipeSnapshot` | `RecipeSnapshot?` | Instantánea inmutable del *recipe* al asignar |
| `AssignedTechnicianId` | `TechnicianId?` | Técnico asignado por el algoritmo de *matching* |
| `AssignmentId` | `AssignmentId?` | Referencia a la asignación creada |
| `OriginSuggestionId` | `SuggestionId?` | Referencia a la sugerencia proactiva origen |
| `OriginSubscriptionId` | `SubscriptionId?` | Referencia a la suscripción empresarial origen |
| `CancellationReason` | `CancellationReason?` | Motivo de cancelación |

| Método | Descripción |
|---|---|
| `Initiate(homeownerId, ...)` | Fábrica para propietario individual; estado inicial `DRAFT` |
| `InitiateForCompany(companyId, ...)` | Fábrica para empresa; estado inicial `DRAFT` |
| `CreateForIoTInstallation(...)` | Fábrica para onboarding IoT empresarial; nace directamente en `PENDING_ASSIGNMENT` |
| `InitiateFromSuggestion(...)` | Fábrica para flujo proactivo; categoría pre-poblada desde la sugerencia |
| `SelectProperty(propertyId, geolocation, hasIoTDevice, deviceId)` | Avanza a `PROPERTY_SELECTED` |
| `SelectCategory(category)` | Avanza a `CATEGORY_SELECTED` |
| `AddDetails(preferences, receiptData, isPriority)` | Avanza a `READY_TO_CONFIRM` |
| `Confirm(frozenIoTSnapshot, recipeRequiresIoTCertification)` | Congela el `IoTContextSnapshot` y avanza a `PENDING_ASSIGNMENT` |
| `MarkAsAssigned(assignmentId, technicianId, snapshot)` | Registra la asignación y avanza a `ASSIGNED` |
| `Cancel(reason)` | Cancela la solicitud; no permitido en estado `ASSIGNED` o `EXPIRED` |
| `Expire()` | Marca la solicitud como expirada desde `PENDING_ASSIGNMENT` |

---

#### ServiceAssignment *(Aggregate Root)*

**Responsabilidad:** Registrar el resultado del algoritmo de *matching*. El `MatchingCriteria` incluye el filtro de certificación IoT.

| Atributo | Tipo | Descripción |
|---|---|---|
| `AssignmentId` | `AssignmentId` | Identificador único de la asignación (prefijo `assign-`) |
| `RequestId` | `RequestId` | Referencia a la solicitud asignada |
| `TechnicianId` | `TechnicianId?` | Técnico asignado |
| `RecipeSnapshot` | `RecipeSnapshot?` | Instantánea del *recipe* al momento de la asignación |
| `MatchingCriteria` | `MatchingCriteria?` | Criterios aplicados por el algoritmo de *matching* |
| `Status` | `EAssignmentStatus` | Estado de la asignación (`Assigned`, `Failed`) |
| `FailureReason` | `string?` | Motivo del fallo (si aplica) |
| `RetryCount` | `int` | Número de reintentos realizados |

| Método | Descripción |
|---|---|
| `Assign(requestId, technicianId, snapshot, criteria)` | Fábrica para asignación exitosa; emite `ServiceAutomaticallyAssignedEvent` |
| `Fail(requestId, reason, retryCount)` | Fábrica para asignación fallida; emite `ServiceAssignmentFailedEvent` |

---

#### ServiceSuggestion *(Aggregate Root)*

**Responsabilidad:** Encapsular una sugerencia proactiva generada a partir de una anomalía IoT. Gestiona su ciclo de vida y protege las invariantes de unicidad por propiedad y el *cooling-off period*.

**Máquina de estados:** `PENDING_REVIEW` → `ACCEPTED` | `DISMISSED` | `EXPIRED`

| Atributo | Tipo | Descripción |
|---|---|---|
| `SuggestionId` | `SuggestionId` | Identificador único de la sugerencia (prefijo `sugg-`) |
| `ClientId` | `string` | Identificador del propietario o empresa afectada |
| `ClientType` | `EClientType` | Tipo de cliente (`Individual`, `Company`) |
| `PropertyId` | `PropertyId` | Propiedad con la anomalía detectada |
| `AnomalyEventId` | `AnomalyEventId` | Referencia al evento de anomalía origen (cross-BC) |
| `SuggestedServiceCategory` | `EServiceCategory` | Categoría de servicio sugerida según el tipo de anomalía |
| `AnomalySeverity` | `EAnomalySeverity` | Severidad de la anomalía (`Low`, `Medium`, `High`, `Critical`) |
| `Status` | `ESuggestionStatus` | Estado actual de la sugerencia |
| `CreatedAt` | `DateTime` | Fecha y hora de creación |
| `ExpiresAt` | `DateTime` | Fecha de expiración (`CreatedAt + 48h`) |
| `DerivedRequestId` | `RequestId?` | Referencia a la solicitud creada al aceptar |

| Método | Descripción |
|---|---|
| `Create(clientId, clientType, propertyId, anomalyEventId, suggestedCategory, severity)` | Fábrica; calcula `ExpiresAt` y emite `ServiceSuggestionCreatedEvent` |
| `Accept(clientId)` | Avanza a `ACCEPTED`; genera un `RequestId` derivado |
| `Dismiss(clientId, reason)` | Avanza a `DISMISSED` con el motivo indicado |
| `Expire()` | Avanza a `EXPIRED`; invocado por el programador de tareas |

---

### Value Objects

| Value Object | Atributos Principales | Descripción |
|---|---|---|
| `CatalogId` | `Value: string` | ID del catálogo (prefijo `cat-`); inmutable |
| `RecipeId` | `Value: string` | ID del *recipe* (prefijo `recipe-`); inmutable |
| `RequestId` | `Value: string` | ID de la solicitud (prefijo `req-`); inmutable |
| `AssignmentId` | `Value: string` | ID de la asignación (prefijo `assign-`); inmutable |
| `SuggestionId` | `Value: string` | ID de la sugerencia (prefijo `sugg-`); inmutable |
| `TechnicianId` | `Value: string` | Referencia cross-BC al técnico |
| `HomeownerId` | `Value: string` | Referencia cross-BC al propietario individual |
| `CompanyId` | `Value: string` | Referencia cross-BC a la empresa |
| `PropertyId` | `Value: string` | Referencia cross-BC a la propiedad |
| `DeviceId` | `Value: string` | Referencia cross-BC al dispositivo IoT |
| `AnomalyEventId` | `Value: string` | Referencia cross-BC al evento de anomalía |
| `SubscriptionId` | `Value: string` | Referencia cross-BC a la suscripción |
| `Money` | `Amount: decimal`, `Currency: ECurrency` | Valor monetario inmutable con moneda |
| `ServicePricing` | `MaterialsEstimate`, `LaborCost`, `TotalPrice: Money` | Estructura de precios del servicio |
| `EstimatedDuration` | `TotalMinutes: int` | Duración estimada del servicio |
| `WarrantyPeriod` | `Months: int` | Período de garantía en meses |
| `Geolocation` | `Latitude: decimal`, `Longitude: decimal` | Coordenadas geográficas de la propiedad |
| `ComponentRequirementItem` | `ComponentTypeId`, `Quantity`, `IsRequired` | Componente requerido con cantidad mínima |
| `RecipeSnapshot` | `RecipeId`, `ServiceName`, `RequiresIoTCertification`, `Pricing`, `EstimatedDuration` | Instantánea inmutable del *recipe* al asignar |
| `MatchingCriteria` | `Geolocation`, `IsPriority`, `RequiresIoTCertifiedTechnician`, `PreferredTechnicianId` | Criterios del algoritmo de *matching* |
| `RequestPreferences` | `ProblemDescription`, `PreferredDates`, `TimePreference` | Preferencias del cliente para la visita |
| `ReceiptData` | `ConsumptionKwh`, `AmountPaid`, `DataSource: EConsumptionDataSource` | Datos del recibo eléctrico o lectura del sensor |
| `IoTContextSnapshot` | `DeviceId`, `CapturedAt`, `StreamStatus`, `RecentReadings`, `ActiveAnomalies` | Instantánea inmutable del sensor al confirmar la solicitud |
| `IoTReadingEntry` | `Timestamp`, `Voltage`, `Current`, `PowerFactor` | Lectura individual del sensor IoT |
| `IoTAnomalyEntry` | `AnomalyId`, `AnomalyType`, `Severity`, `DetectedAt` | Anomalía activa al momento de la instantánea |
| `CancellationReason` | `Value: string` | Motivo de cancelación de la solicitud |
| `DeactivationReason` | `Value: string` | Motivo de desactivación del *recipe* |

---

### Domain Services (Interfaces de Contrato)

#### Command Services

| Interfaz | Descripción |
|---|---|
| `IServiceCatalogCommandService` | Define operaciones que modifican el agregado `ServiceCatalog` mediante comandos |
| `IServiceRequestCommandService` | Define operaciones que modifican el agregado `ServiceRequest` mediante comandos |
| `IServiceAssignmentCommandService` | Define la operación de ejecución del algoritmo de *matching* |
| `IServiceSuggestionCommandService` | Define operaciones que modifican el agregado `ServiceSuggestion` mediante comandos |

#### Query Services

| Interfaz | Descripción |
|---|---|
| `IServiceDesignQueryService` | Define todas las consultas de solo lectura del *Bounded Context* |

---

### Repository Interfaces

| Interfaz | Descripción |
|---|---|
| `IServiceCatalogRepository` | Contrato de persistencia y consulta sobre catálogos y *recipes* |
| `IServiceRequestRepository` | Contrato de persistencia y consulta sobre solicitudes de servicio |
| `IServiceAssignmentRepository` | Contrato de persistencia y consulta sobre asignaciones |
| `IServiceSuggestionRepository` | Contrato de persistencia y consulta sobre sugerencias proactivas IoT |

**Métodos principales de `IServiceCatalogRepository`:**

| Método | Descripción |
|---|---|
| `FindByTechnicianIdAsync(technicianId)` | Obtiene el catálogo de un técnico incluyendo sus *recipes* |
| `ExistsByTechnicianIdAsync(technicianId)` | Verifica si ya existe catálogo para el técnico |
| `FindActiveRecipeByCategoryAndTechnicianAsync(category, technicianId)` | Busca un *recipe* activo por categoría y técnico |

**Métodos principales de `IServiceRequestRepository`:**

| Método | Descripción |
|---|---|
| `FindByClientIdAsync(clientId)` | Obtiene todas las solicitudes de un cliente |
| `FindPendingAssignmentAsync()` | Obtiene solicitudes en cola de *matching* ordenadas por prioridad y fecha |
| `HasActiveRequestForPropertyAsync(propertyId)` | Verifica si hay una solicitud activa para la propiedad |
| `FindPendingIoTAssignedToTechnicianAsync(technicianId)` | Obtiene solicitudes IoT asignadas a un técnico específico |

**Métodos principales de `IServiceSuggestionRepository`:**

| Método | Descripción |
|---|---|
| `FindActiveSuggestionByPropertyIdAsync(propertyId)` | Busca sugerencia activa (`PENDING_REVIEW`) para una propiedad |
| `FindByClientIdAsync(clientId, status?)` | Obtiene sugerencias de un cliente con filtro opcional de estado |
| `FindExpiredPendingAsync()` | Obtiene sugerencias caducadas pendientes de expirar |

---

#### 4.2.4.2. Interface Layer.

La capa de interfaces define los recursos de entrada y salida, los ensambladores de transformación y los controladores REST que exponen las capacidades del *Bounded Context* hacia los consumidores externos.

---

### Resources (Recursos de Entrada y Salida)

#### Recursos de Entrada (Comandos de Cliente)

| Clase | Atributos Principales | Descripción |
|---|---|---|
| `CreateServiceRecipeResource` | `ServiceName`, `ServiceDescription`, `ServiceCategory`, `RequiresIoTCertification`, `ComponentRequirements`, `EstimatedDurationHours`, `EstimatedDurationMinutes`, `Pricing`, `Prerequisites`, `Deliverables`, `WarrantyMonths` | Datos para crear un nuevo *recipe* en el catálogo |
| `UpdateServiceRecipeResource` | `ServiceName?`, `ServiceDescription?`, `ComponentRequirements?`, `EstimatedDuration?`, `Pricing?`, `Prerequisites?`, `Deliverables?`, `WarrantyMonths?` | Datos para actualizar un *recipe* existente |
| `DeactivateServiceRecipeResource` | `Reason: string` | Motivo de desactivación del *recipe* |
| `SelectPropertyResource` | `PropertyId: string` | Identificador de la propiedad seleccionada |
| `SelectServiceCategoryResource` | `Category: string` | Categoría de servicio seleccionada |
| `AddServiceDetailsResource` | `ProblemDescription`, `ConsumptionKwh`, `ConsumptionDataSource`, `AmountPaid?`, `BillingPeriod?`, `ReceiptNumber?`, `IsPriority`, `PreferredDates`, `TimePreference` | Detalles del servicio y datos del recibo eléctrico |
| `CancelServiceRequestResource` | `Reason: string` | Motivo de cancelación de la solicitud |
| `AcceptServiceSuggestionResource` | `ClientId: string` | Identificador del cliente que acepta la sugerencia |
| `DismissServiceSuggestionResource` | `ClientId: string`, `Reason: string` | Motivo del descarte de la sugerencia |

#### Recursos de Salida (Respuestas al Cliente)

| Clase | Atributos Principales | Descripción |
|---|---|---|
| `ServiceCatalogResource` | `CatalogId`, `TechnicianId`, `Status`, `Recipes: IEnumerable<ServiceRecipeDetailResource>` | Catálogo completo con sus *recipes* |
| `ServiceRecipeDetailResource` | `RecipeId`, `ServiceName`, `ServiceCategory`, `RequiresIoTCertification`, `ComponentRequirements`, `Pricing`, `EstimatedDuration`, `WarrantyMonths`, `IsActive` | Detalle de un *recipe* individual |
| `ServiceRequestSummaryResource` | `RequestId`, `ClientId`, `ClientType`, `Status`, `RequestType`, `RequiresIoTCertifiedTechnician`, `PropertyId`, `Geolocation`, `HasIoTDevice`, `InstalledDeviceId`, `RequestedCategory`, `IsPriority`, `IoTContext?`, `AssignedTechnicianId?`, `AssignmentId?`, `CreatedAt` | Resumen del estado actual de la solicitud |
| `AvailableServiceResource` | `RecipeId`, `ServiceName`, `ServiceCategory`, `TotalPrice`, `Currency`, `EstimatedHours`, `RequiresIoTCertification` | Servicio disponible para seleccionar en el *wizard* |
| `IoTContextResource` | `DeviceId`, `StreamStatus`, `CapturedAt`, `LastReading?`, `ActiveAnomalies` | Estado actual del sensor IoT de la propiedad |
| `ServiceSuggestionResource` | `SuggestionId`, `PropertyId`, `SuggestedServiceCategory`, `AnomalySeverity`, `Status`, `CreatedAt`, `ExpiresAt` | Sugerencia proactiva del sistema |

---

### Assemblers (Transformadores)

Los ensambladores implementan el patrón **Assembler** para desacoplar la capa de interfaz de la capa de dominio, transformando recursos en comandos y entidades en recursos de respuesta.

| Clase | Descripción |
|---|---|
| `CreateServiceRecipeCommandFromResourceAssembler` | Transforma `CreateServiceRecipeResource` en `CreateServiceRecipeCommand`, mapeando precios, duraciones y la bandera `RequiresIoTCertification` |
| `ServiceCatalogResourceFromEntityAssembler` | Transforma el agregado `ServiceCatalog` y su colección de `ServiceRecipe` en `ServiceCatalogResource` |
| `ServiceRequestSummaryResourceFromEntityAssembler` | Transforma el agregado `ServiceRequest` en `ServiceRequestSummaryResource`, incluyendo el mapeo del `IoTContextSnapshot` |
| `ServiceSuggestionResourceFromEntityAssembler` | Transforma el agregado `ServiceSuggestion` en `ServiceSuggestionResource` |
| `IoTContextResourceFromValueObjectAssembler` | Transforma el *value object* `IoTContextSnapshot` en `IoTContextResource`, seleccionando la última lectura y las anomalías activas |

---

### Controllers (Controladores REST)

#### ServiceCatalogController

| Método HTTP | Ruta | Descripción |
|---|---|---|
| `POST` | `/api/v1/technicians/{technicianId}/catalog` | Crea el catálogo de servicios del técnico |
| `GET` | `/api/v1/technicians/{technicianId}/catalog` | Obtiene el catálogo con todos sus *recipes* |
| `POST` | `/api/v1/technicians/{technicianId}/catalog/recipes` | Agrega un nuevo *recipe* al catálogo |
| `PUT` | `/api/v1/technicians/{technicianId}/catalog/recipes/{recipeId}` | Actualiza un *recipe* existente |
| `PATCH` | `/api/v1/technicians/{technicianId}/catalog/recipes/{recipeId}/deactivate` | Desactiva un *recipe* |
| `PATCH` | `/api/v1/technicians/{technicianId}/catalog/recipes/{recipeId}/reactivate` | Reactiva un *recipe* |
| `GET` | `/api/v1/technicians/{technicianId}/catalog/recipes/{recipeId}` | Obtiene el detalle de un *recipe* específico |

#### ServiceRequestController

| Método HTTP | Ruta | Descripción |
|---|---|---|
| `POST` | `/api/v1/service-requests` | Inicia el asistente de solicitud (estado `DRAFT`) |
| `PUT` | `/api/v1/service-requests/{requestId}/property` | Selecciona la propiedad (avanza a `PROPERTY_SELECTED`) |
| `GET` | `/api/v1/service-requests/{requestId}/iot-context` | Obtiene el contexto IoT actual de la propiedad (solo lectura) |
| `GET` | `/api/v1/service-requests/{requestId}/available-services` | Lista los servicios disponibles para la categoría y zona |
| `PUT` | `/api/v1/service-requests/{requestId}/category` | Selecciona la categoría de servicio (avanza a `CATEGORY_SELECTED`) |
| `PUT` | `/api/v1/service-requests/{requestId}/details` | Agrega detalles del problema y preferencias (avanza a `READY_TO_CONFIRM`) |
| `POST` | `/api/v1/service-requests/{requestId}/confirm` | Confirma la solicitud, congela el snapshot IoT y dispara el *matching* |
| `DELETE` | `/api/v1/service-requests/{requestId}` | Cancela la solicitud |
| `GET` | `/api/v1/service-requests/{requestId}` | Obtiene el resumen actualizado de la solicitud |

#### ServiceAssignmentController

| Método HTTP | Ruta | Descripción |
|---|---|---|
| `GET` | `/api/v1/service-requests/matching-queue` | Obtiene la cola de solicitudes pendientes de asignación (uso interno) |

#### ServiceSuggestionController

| Método HTTP | Ruta | Descripción |
|---|---|---|
| `GET` | `/api/v1/suggestions` | Obtiene las sugerencias del cliente autenticado (filtro opcional por estado) |
| `POST` | `/api/v1/suggestions/{suggestionId}/accept` | Acepta la sugerencia y crea una solicitud derivada |
| `POST` | `/api/v1/suggestions/{suggestionId}/dismiss` | Descarta la sugerencia con un motivo |

---

#### 4.2.4.3. Application Layer.

La capa de aplicación orquesta los casos de uso del *Bounded Context*, coordinando la interacción entre el dominio, los repositorios y los servicios externos. Implementa las interfaces de servicio definidas en la capa de dominio.

---

### Command Services

#### ServiceCatalogCommandService

Implementa `IServiceCatalogCommandService`. Coordina la creación, modificación y desactivación de catálogos y *recipes*, verificando la certificación IoT mediante el puerto de salida `IProfilesContextFacade`.

| Comando Manejado | Descripción |
|---|---|
| `CreateServiceCatalogCommand` | Valida que el técnico no tenga catálogo previo y crea uno nuevo |
| `CreateServiceRecipeCommand` | Verifica la certificación IoT si `RequiresIoTCertification = true`, luego delega en `ServiceCatalog.AddRecipe(...)` |
| `UpdateServiceRecipeCommand` | Delega la actualización en el agregado; aplica reglas de negocio sobre servicios activos |
| `DeactivateServiceRecipeCommand` | Consulta servicios en ejecución mediante `IServiceOperationContextFacade` antes de desactivar |
| `ReactivateServiceRecipeCommand` | Reactiva el *recipe* indicado en el catálogo del técnico |
| `DeactivateIoTRecipesDueToRevocationCommand` | Desactiva masivamente todos los *recipes* IoT al revocarse la certificación del técnico |

#### ServiceRequestCommandService

Implementa `IServiceRequestCommandService`. Coordina el asistente de solicitud completo, consultando propiedades (Assets BC), planes de suscripción (Subscriptions BC) y el estado del dispositivo IoT (IoT Monitoring BC).

| Comando Manejado | Descripción |
|---|---|
| `InitiateServiceRequestCommand` | Verifica la elegibilidad del plan, crea el agregado `ServiceRequest` y persiste en estado `DRAFT` |
| `SelectPropertyForRequestCommand` | Obtiene la geolocalización y consulta el dispositivo IoT instalado en la propiedad |
| `SelectServiceCategoryCommand` | Avanza el estado del asistente a `CATEGORY_SELECTED` |
| `AddServiceDetailsCommand` | Construye `RequestPreferences` y `ReceiptData`; avanza a `READY_TO_CONFIRM` |
| `ConfirmServiceRequestCommand` | Congela el `IoTContextSnapshot` mediante `IIoTMonitoringContextFacade`; determina `RequiresIoTCertifiedTechnician` desde el *recipe* y dispara el *matching* |
| `CancelServiceRequestCommand` | Cancela la solicitud con el motivo indicado |
| `ScheduleIoTInstallationServiceCommand` | Crea un `ServiceRequest` de tipo `IOT_INSTALLATION` directamente en `PENDING_ASSIGNMENT` y dispara el *matching* con filtro IoT |

#### ServiceAssignmentCommandService

Implementa `IServiceAssignmentCommandService`. Ejecuta el algoritmo de *matching* coordinando datos de tres *Bounded Contexts* externos.

| Paso del Algoritmo | Descripción |
|---|---|
| 1. Validación previa | Verifica que la solicitud tenga geolocalización y categoría definidas |
| 2. Búsqueda de técnicos | Consulta `IProfilesContextFacade.GetTechniciansInAreaAsync(lat, lon)` |
| 3. Filtrado de candidatos | Por *recipe* activo en la categoría, certificación IoT (si aplica) y stock de componentes |
| 4. Selección | Ordena candidatos por calificación descendente y selecciona el primero |
| 5. Asignación | Crea `RecipeSnapshot`, `MatchingCriteria`, `ServiceAssignment` y actualiza la solicitud |
| 6. Fallo | Si no hay candidatos, crea `ServiceAssignment` en estado `Failed` con contador de reintentos |

#### ServiceSuggestionCommandService

Implementa `IServiceSuggestionCommandService`. Orquesta la generación, aceptación y descarte de sugerencias proactivas.

| Comando Manejado | Descripción |
|---|---|
| `GenerateServiceSuggestionCommand` | Verifica condiciones anti-spam (una sugerencia activa o solicitud activa por propiedad) antes de crear la sugerencia |
| `AcceptServiceSuggestionCommand` | Acepta la sugerencia, verifica la elegibilidad del plan y delega la creación de `ServiceRequest` derivado |
| `DismissServiceSuggestionCommand` | Descarta la sugerencia con el motivo indicado por el cliente |

---

### Query Services

#### ServiceDesignQueryService

Implementa `IServiceDesignQueryService`. Proporciona todas las consultas de solo lectura del *Bounded Context*.

| Consulta Manejada | Descripción |
|---|---|
| `GetServiceCatalogQuery` | Obtiene el catálogo completo del técnico |
| `GetServiceRecipeDetailsQuery` | Obtiene el detalle de un *recipe* específico |
| `GetRequestEligibilityQuery` | Consulta la elegibilidad del cliente mediante `ISubscriptionContextFacade` |
| `GetAvailableServicesQuery` | Construye la lista de servicios disponibles cruzando técnicos en área, *recipes* activos y stock |
| `GetServiceRequestSummaryQuery` | Obtiene el estado actualizado de una solicitud |
| `GetMatchingQueueQuery` | Obtiene la cola de solicitudes pendientes de asignación |
| `GetIoTContextForPropertyQuery` | Consulta lecturas recientes y anomalías activas mediante `IIoTMonitoringContextFacade` |
| `GetServiceSuggestionsQuery` | Obtiene las sugerencias del cliente con filtro opcional de estado |

---

### Event Handlers (Políticas de Reacción)

| Manejador | Evento Disparador | Acción |
|---|---|---|
| `SubscriptionActivatedEventHandler` | `SubscriptionActivated` (Subscriptions BC) | Crea el catálogo automáticamente para técnicos Premium |
| `IoTInstallationRequiredEventHandler` | `IoTInstallationRequired` (Subscriptions BC) | Ejecuta `ScheduleIoTInstallationServiceCommand` |
| `AnomalyDetectedEventHandler` | `AnomalyDetected` (IoT Monitoring BC) | Evalúa condiciones y ejecuta `GenerateServiceSuggestionCommand` para severidades `MEDIUM`, `HIGH` o `CRITICAL` |
| `IoTCertificationGrantedEventHandler` | `IoTCertificationGranted` (Profiles BC) | Registra la habilitación del técnico para *recipes* IoT |
| `IoTCertificationRevokedEventHandler` | `IoTCertificationRevoked` (Profiles BC) | Desactiva *recipes* IoT masivamente y re-dispara el *matching* para solicitudes afectadas |

---

### Outbound Service Ports (Puertos de Salida)

| Interfaz | *Bounded Context* Destino | Propósito |
|---|---|---|
| `ISubscriptionContextFacade` | Subscriptions BC | Verificar elegibilidad del plan, límites de solicitudes y marcado como prioritaria |
| `IAssetsContextFacade` | Assets BC | Obtener geolocalización de propiedad, stock de componentes y dispositivo IoT instalado |
| `IProfilesContextFacade` | Profiles BC | Obtener técnicos en área, verificar certificación IoT, validar estado del cliente |
| `IServiceOperationContextFacade` | Service Operation BC | Contar servicios activos o en ejecución para un *recipe* específico |
| `IIoTMonitoringContextFacade` | IoT Monitoring BC | Obtener lecturas recientes y anomalías activas del dispositivo IoT instalado |

---

#### 4.2.4.4. Infrastructure Layer.

La capa de infraestructura contiene las implementaciones concretas de los repositorios y las configuraciones de persistencia mediante **Entity Framework Core**.

---

### Configuraciones EF Core

Todas las tablas del *Bounded Context* utilizan el prefijo `sdp_` (*Service Design and Planning*). Los *Value Objects* complejos se persisten como columnas de tipo `JSONB` en PostgreSQL.

#### ServiceCatalogConfiguration

| Tabla | `sdp_service_catalogs` |
|---|---|
| Clave primaria | `id` (`CatalogId`) |
| Índices | `technician_id` (único) |
| Mapeo de VO | `Status` como `string`; `TechnicianId` mediante conversión |
| Relación | `HasMany<ServiceRecipe>` con `OnDelete(Cascade)` |

#### ServiceRecipeConfiguration

| Tabla | `sdp_service_recipes` |
|---|---|
| Clave primaria | `id` (`RecipeId`) |
| Columnas relevantes | `requires_iot_certification BOOLEAN NOT NULL DEFAULT false` |
| Columnas JSONB | `component_requirements`, `prerequisites`, `deliverables` |
| Columnas embebidas | `estimated_duration_min`, `materials_estimate`, `labor_cost`, `total_price`, `currency`, `warranty_months` |
| Índices | `(catalog_id, service_name)` (único), `(requires_iot_certification, is_active)` |

#### ServiceRequestConfiguration

| Tabla | `sdp_service_requests` |
|---|---|
| Clave primaria | `id` (`RequestId`) |
| Columnas relevantes | `company_id`, `client_type`, `request_type`, `has_iot_device`, `installed_device_id`, `requires_iot_certified_tech` |
| Columnas JSONB | `receipt_data`, `preferences`, `iot_context_snapshot`, `recipe_snapshot` |
| Columnas de trazabilidad | `origin_suggestion_id`, `origin_subscription_id` |
| Índices | Cola de *matching*: `(is_priority DESC, created_at ASC) WHERE status = 'PendingAssignment'` |

#### ServiceAssignmentConfiguration

| Tabla | `sdp_service_assignments` |
|---|---|
| Clave primaria | `id` (`AssignmentId`) |
| Columnas JSONB | `recipe_snapshot`, `matching_criteria` |
| Clave foránea | `request_id → sdp_service_requests(id)` |

#### ServiceSuggestionConfiguration

| Tabla | `sdp_service_suggestions` |
|---|---|
| Clave primaria | `id` (`SuggestionId`) |
| Columnas relevantes | `client_id`, `client_type`, `property_id`, `anomaly_event_id`, `suggested_service_category`, `anomaly_severity`, `status`, `expires_at` |
| Índices | `(property_id, status)`, `(client_id, status)`, `(expires_at) WHERE status = 'PendingReview'` (parcial) |

---

### Implementación de Repositorios

| Clase | Interfaz Implementada | Descripción |
|---|---|---|
| `ServiceCatalogRepository` | `IServiceCatalogRepository` | Incluye carga ansiosa (`Include`) de *recipes*; implementa búsqueda por `TechnicianId` y por categoría activa |
| `ServiceRequestRepository` | `IServiceRequestRepository` | Implementa cola de *matching* ordenada por prioridad; consultas de solicitudes activas por propiedad y por técnico IoT |
| `ServiceAssignmentRepository` | `IServiceAssignmentRepository` | Implementa consulta por `RequestId` y conteo de reintentos |
| `ServiceSuggestionRepository` | `IServiceSuggestionRepository` | Implementa búsqueda de sugerencias activas por propiedad y consulta de caducadas para el programador de tareas |

---

#### 4.2.4.5. Bounded Context Software Architecture Component Level Diagrams.



#### 4.2.4.6. Bounded Context Software Architecture Code Level Diagrams.

##### 4.2.4.6.1. Bounded Context Domain Layer Class Diagrams.

En esta sección mostramos parte de la estructura de nuestra solución Electrolink y los datos que seran incluidos en el diagrama de clases.

![](assets/img/cap4/sdp/class.png)
\
```plantuml
@startuml Service_Design_Planning_Domain_Layer

!define AGGREGATE_COLOR #DDE5FF
!define ENTITY_COLOR #D5F5E3
!define VO_COLOR #FEF9E7
!define INTERFACE_COLOR #FDEBD0
!define ENUM_COLOR #F5EEF8

skinparam class {
  BackgroundColor White
  BorderColor #4A4A4A
  ArrowColor #4A4A4A
  FontSize 11
}

skinparam linetype ortho

' ─────────────────────────────────────────
' AGGREGATES
' ─────────────────────────────────────────

package "Aggregates" AGGREGATE_COLOR {

  class ServiceCatalog <<Aggregate Root>> AGGREGATE_COLOR {
    + CatalogId : CatalogId
    + TechnicianId : TechnicianId
    + Status : ECatalogStatus
    - _recipes : List<ServiceRecipe>
    + Recipes : IReadOnlyList<ServiceRecipe>
    --
    + {static} Create(technicianId) : ServiceCatalog
    + AddRecipe(name, desc, cat, requiresIoT, ...) : void
    + UpdateRecipe(recipeId, ...) : void
    + DeactivateRecipe(recipeId, reason, inProgress) : void
    + DeactivateIoTRecipes(reason) : IReadOnlyList<RecipeId>
    + ReactivateRecipe(recipeId) : void
  }

  class ServiceRequest <<Aggregate Root>> AGGREGATE_COLOR {
    + RequestId : RequestId
    + HomeownerId : HomeownerId?
    + CompanyId : CompanyId?
    + ClientType : EClientType
    + Status : ERequestStatus
    + RequestType : ERequestType
    + PropertyId : PropertyId?
    + Geolocation : Geolocation?
    + HasIoTDevice : bool
    + InstalledDeviceId : DeviceId?
    + RequestedCategory : EServiceCategory?
    + RequiresIoTCertifiedTechnician : bool
    + Preferences : RequestPreferences?
    + ReceiptData : ReceiptData?
    + IsPriority : bool
    + IoTContextSnapshot : IoTContextSnapshot?
    + RecipeSnapshot : RecipeSnapshot?
    + AssignedTechnicianId : TechnicianId?
    + AssignmentId : AssignmentId?
    + OriginSuggestionId : SuggestionId?
    + OriginSubscriptionId : SubscriptionId?
    + CancellationReason : CancellationReason?
    --
    + {static} Initiate(homeownerId, ...) : ServiceRequest
    + {static} InitiateForCompany(companyId, ...) : ServiceRequest
    + {static} CreateForIoTInstallation(...) : ServiceRequest
    + {static} InitiateFromSuggestion(...) : ServiceRequest
    + SelectProperty(propertyId, geo, hasIoT, deviceId) : void
    + SelectCategory(category) : void
    + AddDetails(prefs, receipt, isPriority) : void
    + Confirm(iotSnapshot, recipeRequiresIoT) : void
    + MarkAsAssigned(assignId, techId, snapshot) : void
    + Cancel(reason) : void
    + Expire() : void
  }

  class ServiceAssignment <<Aggregate Root>> AGGREGATE_COLOR {
    + AssignmentId : AssignmentId
    + RequestId : RequestId
    + TechnicianId : TechnicianId?
    + RecipeSnapshot : RecipeSnapshot?
    + MatchingCriteria : MatchingCriteria?
    + Status : EAssignmentStatus
    + FailureReason : string?
    + RetryCount : int
    --
    + {static} Assign(requestId, techId, snapshot, criteria) : ServiceAssignment
    + {static} Fail(requestId, reason, retryCount) : ServiceAssignment
  }

  class ServiceSuggestion <<Aggregate Root>> AGGREGATE_COLOR {
    + SuggestionId : SuggestionId
    + ClientId : string
    + ClientType : EClientType
    + PropertyId : PropertyId
    + AnomalyEventId : AnomalyEventId
    + SuggestedServiceCategory : EServiceCategory
    + AnomalySeverity : EAnomalySeverity
    + Status : ESuggestionStatus
    + CreatedAt : DateTime
    + ExpiresAt : DateTime
    + DerivedRequestId : RequestId?
    --
    + {static} Create(clientId, clientType, propertyId, ...) : ServiceSuggestion
    + Accept(clientId) : RequestId
    + Dismiss(clientId, reason) : void
    + Expire() : void
  }
}

@enduml
```

##### 4.2.4.6.2. Bounded Context Database Design Diagram.

En esta sección mostramos parte de la estructura de nuestra solución Electrolink y los datos que seran incluidos en el diagrama de diseño de base de datos.

![](assets/img/cap4/sdp/database.png)
\
```plantuml
@startuml Service_Design_Planning_Database

!define TABLE(name,desc) class name as "desc" << (T,#DDDDFF) >>
!define PK(x) <u>x</u>
!define FK(x) <i>x</i>

skinparam class {
  BackgroundColor #F8F9FA
  BorderColor #4A4A4A
  ArrowColor #4A4A4A
  FontSize 10
}

skinparam linetype ortho

' ─────────────────────────────────────────
' TABLES
' ─────────────────────────────────────────

TABLE(sdp_service_catalogs, "sdp_service_catalogs") {
  PK(id) VARCHAR(60) NOT NULL
  --
  technician_id VARCHAR(100) NOT NULL UNIQUE
  status VARCHAR(20) NOT NULL
  created_at TIMESTAMPTZ NOT NULL DEFAULT now()
  updated_at TIMESTAMPTZ
}

TABLE(sdp_service_recipes, "sdp_service_recipes") {
  PK(id) VARCHAR(60) NOT NULL
  --
  FK(catalog_id) VARCHAR(60) NOT NULL
  technician_id VARCHAR(100) NOT NULL
  service_name VARCHAR(200) NOT NULL
  service_description TEXT NOT NULL
  service_category VARCHAR(50) NOT NULL
  requires_iot_certification BOOLEAN NOT NULL DEFAULT false
  component_requirements JSONB NOT NULL
  estimated_duration_min INT NOT NULL
  materials_estimate DECIMAL(10,2) NOT NULL
  labor_cost DECIMAL(10,2) NOT NULL
  total_price DECIMAL(10,2) NOT NULL
  currency VARCHAR(3) NOT NULL
  prerequisites JSONB NOT NULL
  deliverables JSONB NOT NULL
  warranty_months INT NOT NULL
  is_active BOOLEAN NOT NULL DEFAULT true
  times_requested INT NOT NULL DEFAULT 0
  created_at TIMESTAMPTZ NOT NULL DEFAULT now()
  updated_at TIMESTAMPTZ
  UNIQUE(catalog_id, service_name)
}

TABLE(sdp_service_requests, "sdp_service_requests") {
  PK(id) VARCHAR(60) NOT NULL
  --
  homeowner_id VARCHAR(100)
  company_id VARCHAR(100)
  client_type VARCHAR(20) NOT NULL
  status VARCHAR(30) NOT NULL
  request_type VARCHAR(30) NOT NULL DEFAULT 'Standard'
  property_id VARCHAR(100)
  geolocation_lat DECIMAL(9,6)
  geolocation_lon DECIMAL(9,6)
  has_iot_device BOOLEAN NOT NULL DEFAULT false
  installed_device_id VARCHAR(100)
  requested_category VARCHAR(50)
  requires_iot_certified_tech BOOLEAN NOT NULL DEFAULT false
  receipt_data JSONB
  preferences JSONB
  iot_context_snapshot JSONB
  recipe_snapshot JSONB
  assigned_technician_id VARCHAR(100)
  assignment_id VARCHAR(60)
  origin_suggestion_id VARCHAR(60)
  origin_subscription_id VARCHAR(100)
  is_priority BOOLEAN NOT NULL DEFAULT false
  created_at TIMESTAMPTZ NOT NULL DEFAULT now()
  updated_at TIMESTAMPTZ
  CHECK (homeowner_id IS NOT NULL OR company_id IS NOT NULL)
}

TABLE(sdp_service_assignments, "sdp_service_assignments") {
  PK(id) VARCHAR(60) NOT NULL
  --
  FK(request_id) VARCHAR(60) NOT NULL
  technician_id VARCHAR(100)
  recipe_snapshot JSONB
  matching_criteria JSONB
  status VARCHAR(20) NOT NULL
  failure_reason VARCHAR(100)
  retry_count INT NOT NULL DEFAULT 0
  assigned_at TIMESTAMPTZ NOT NULL DEFAULT now()
}

TABLE(sdp_service_suggestions, "sdp_service_suggestions") {
  PK(id) VARCHAR(60) NOT NULL
  --
  client_id VARCHAR(100) NOT NULL
  client_type VARCHAR(20) NOT NULL
  property_id VARCHAR(100) NOT NULL
  anomaly_event_id VARCHAR(100) NOT NULL
  suggested_service_category VARCHAR(50) NOT NULL
  anomaly_severity VARCHAR(20) NOT NULL
  status VARCHAR(30) NOT NULL
  derived_request_id VARCHAR(60)
  created_at TIMESTAMPTZ NOT NULL DEFAULT now()
  expires_at TIMESTAMPTZ NOT NULL
}

TABLE(sdp_domain_events_outbox, "sdp_domain_events_outbox") {
  PK(id) UUID NOT NULL DEFAULT gen_random_uuid()
  --
  event_name VARCHAR(100) NOT NULL
  aggregate_id VARCHAR(60) NOT NULL
  payload JSONB NOT NULL
  occurred_at TIMESTAMPTZ NOT NULL
  processed_at TIMESTAMPTZ
  retry_count INT NOT NULL DEFAULT 0
  error_message TEXT
  is_published BOOLEAN NOT NULL DEFAULT false
}
@enduml
```
---

### 4.2.5. Bounded Context: Service Operation & Monitoring
#### 4.2.5.1. Domain Layer.
#### 4.2.5.2. Interface Layer.
#### 4.2.5.3. Application Layer.
#### 4.2.5.4. Infrastructure Layer.
#### 4.2.5.5. Bounded Context Software Architecture Component Level Diagrams.
#### 4.2.5.6. Bounded Context Software Architecture Code Level Diagrams.
##### 4.2.5.6.1. Bounded Context Domain Layer Class Diagrams.
##### 4.2.5.6.2. Bounded Context Database Design Diagram.

### 4.2.6. Bounded Context: Subscriptions and Payment Management
#### 4.2.6.1. Domain Layer.
#### 4.2.6.2. Interface Layer.
#### 4.2.6.3. Application Layer.
#### 4.2.6.4. Infrastructure Layer.
#### 4.2.6.5. Bounded Context Software Architecture Component Level Diagrams.
#### 4.2.6.6. Bounded Context Software Architecture Code Level Diagrams.
##### 4.2.6.6.1. Bounded Context Domain Layer Class Diagrams.
##### 4.2.6.6.2. Bounded Context Database Design Diagram.

### 4.2.7. Bounded Context: Analytics 
#### 4.2.7.1. Domain Layer.
#### 4.2.7.2. Interface Layer.
#### 4.2.7.3. Application Layer.
#### 4.2.7.4. Infrastructure Layer.
#### 4.2.7.5. Bounded Context Software Architecture Component Level Diagrams.
#### 4.2.7.6. Bounded Context Software Architecture Code Level Diagrams.
##### 4.2.7.6.1. Bounded Context Domain Layer Class Diagrams.
##### 4.2.7.6.2. Bounded Context Database Design Diagram.

### 4.2.8. Bounded Context: IoT Monitoring and Edge Processing
#### 4.2.8.1. Domain Layer.
#### 4.2.8.2. Interface Layer.
#### 4.2.8.3. Application Layer.
#### 4.2.8.4. Infrastructure Layer.
#### 4.2.8.5. Bounded Context Software Architecture Component Level Diagrams.
#### 4.2.8.6. Bounded Context Software Architecture Code Level Diagrams.
##### 4.2.8.6.1. Bounded Context Domain Layer Class Diagrams.
##### 4.2.8.6.2. Bounded Context Database Design Diagram.
