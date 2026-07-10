# Capítulo VI: Product Implementation, Validation & Deployment
---
## 6.1. Software Configuration Management.

Software Configuration Management (SCM) o Gestión de Configuración de Software es el conjunto de prácticas y herramientas usadas para controlar cambios en un proyecto de software.

Su objetivo principal es que un equipo pueda desarrollar software sin perder control sobre:
versiones del código,
cambios realizados,
errores introducidos,
configuraciones del entorno,
y colaboración entre desarrolladores.

### 6.1.1. Software Development Environment Configuration.

**Project Requirements Management**

**Jira:**
Permite al equipo gestionar los avances del proyecto en sprints garantizando trabajo y entregas continuas. 

**Link de referencia:** [Acceder a Jira](https://www.atlassian.com/es/software/jira)

**Product UX/UI Design:**

**Figma:** 
Herramienta de diseño fundamental para generar los mockups y prototipos de los diseños realizados para nuestra solución.

**Link de referencia:** [Acceder a Figma](https://www.figma.com/)

**Software Development**

**Visual Studio Code:**
Editor de codigo gratuito y de codigo abierto, siendo de preferencia por muchos desarrolladores. Para el proyecto, el equipo utilizo esta herramienta de desarrollo garantizado una colaboracion e integracion eficiente durante el proyecto.

**Link de referencia:** [Acceder a VS Code](https://code.visualstudio.com/)

**Software Deployment:**

**Render:**
Esta aplicación permite desplegar bases de datos, aplicaciones web, sitios estáticos o backend para gestionar el despliegue de servidores y configuraciones complejas.

**Link de referencia:** [Acceder a Render](https://render.com/)

**Git:**
Sistema de control de versiones que nos permite tener un seguimiento fluido y colaborativo sobre el proyecto, generando integridad y continuedad en el desarrollo de nuestro proyecto.

[Acceder a Git](https://git-scm.com/)

**Software Documentation and Project Management**

**Github:** 
Plataforma en la nube para alojar codigo de software colaborativo mediante repositorios basados en Git. En esta plataforma tenemos alojado el desarrollo de nuestro proyecto, tanto documentacion, codigo de frontend, mobile y backend.

[Acceder a Github](https://github.com/)

### 6.1.2. Source Code Management.

**Repositorios de Github** 

 - Enlace del [codigo Landing Page]()
 - Enlace del [codigo Frontend](https://github.com/Hampcoders-Desarrollo-IoT/electrolink-frontend-web)
 - Enlace del [codigo Backend](https://github.com/Hampcoders-Desarrollo-IoT/electrolink-backend-api)
\
![Gitflow Graphic](assets/img/cap6/git-flow.png)

Gitflow es un modelo de ramificación para Git que organiza el desarrollo de software mediante ramas estandarizadas, facilitando la colaboración y el control del código en equipo. En Glottia, utilizamos este modelo para gestionar de forma eficiente los repositorios de nuestros microservicios.

La rama main contiene las versiones estables y listas para producción de cada microservicio, previamente validadas y etiquetadas para mantener un control de las releases.

La rama develop almacena la versión en desarrollo del proyecto, integrando las funcionalidades completadas durante el sprint antes de pasar a producción.

Por otro lado, las ramas feature se utilizan para desarrollar funcionalidades o tareas específicas de manera independiente, siguiendo la convención feature/nombre-descriptivo y permitiendo integrar los cambios mediante Pull Requests.

### 6.1.3. Source Code Style Guide & Conventions.

### 6.1.4. Software Deployment Configuration.

## 6.2. Landing Page, Services & Applications Implementation.

### 6.2.1. Sprint 1
#### 6.2.1.1. Sprint Planning 1

En esta seccion se detalle la planificacion y distribucion llevada a cabo para abarcar el contenido a abarcar en este presente sprint. Nos centramos en enfocar el diseño de la Landing Page, el Backend y una implementación parcial del frontend.

| Campo | Detalle |
| :--- | :--- |
| **Sprint #** | Sprint 1 |
| **Date** | 10-05-2026 |
| **Time** | 12:00 PM |
| **Location** | Virtual – Discord |
| **Prepared By** | Leandro Contreras |
| **Attendees (to planning meeting)** | Leandro Contreras, Alessandra Becerra, Ivo Machado, Ethan Aliaga, Italo Sanchez, Mateo Cabanillas |
| **Sprint n-1 Review Summary** | Este es el primer Sprint, por lo que este campo aún no es aplicable. |
| **Sprint n-1 Retrospective Summary** | Este es el primer Sprint, por lo que este campo aún no es aplicable. |
| **Sprint 1 Goal** | Nuestro objetivo es desarrollar e implementar una Landing Page responsiva y accesible vinculada al catálogo de planes comerciales, en paralelo con la construcción, despliegue en la nube y puesta en marcha del entorno del Backend. Esto abarca la creación de los primeros endpoints funcionales para los Bounded Contexts de **Identity and Access Management (IAM)**, **Profiles and Preferences**, y **Subscription and Payments**. El resultado final se validará mediante la publicación del software funcional en producción y el análisis de la interacción inicial de los usuarios. |
| **Sprint 1 Velocity** | 16 Story Points |
| **Sum of Story Points** | 16 Story Points |


#### 6.2.1.2. Aspect Leaders and Collaborators.

En este Sprint el equipo se enfocó en construir la Landing Page de ElectroLink, asimismo una implementación parcial del Frontend Web Application. Es de suma importancia tener asignado un rol y especificar las US a llevar a cabo para fomentar un flujo de trabajo continuo, responsable y efectivo.

| Team Member (Last Name, First Name) | GitHub Username | UX/UI & Dev: Landing Page  | BC: IAM & Auth - API Endpoints | BC: Profiles - Estructura DB  | BC: Subscriptions - Catálogo |
| :--- | :--- | :---: | :---: | :---: | :---: |
| Becerra Tejeda, Alessandra Nicole | aleeBecerra | **L** *(Form. Contacto)* | | **C** | |
| Contreras López, Leandro Saul | WiDDsito | **L** *(Responsive Móvil)* | **C** | | |
| Aliaga Aguirre, Ethan Matías | MatFragg | **L** *(Hero & Estilos)* | | | |
| Machado Bracamonte, Ivo Marcelo | ivommb11 | **C** *(Header Fijo)* | | **L** | |
| Cabanillas Meza, José Mateo | marckszz | | **L** *(JWT & Sign-up)* | | **C** |
| Sanchez Manrique, Italo Ludwing | ItaloSanche | | **C** | **C** | **L** *(GET/Plans)* |


#### 6.2.1.3. Sprint Backlog 1.

El principal objetivo del Sprint Backlog en este ciclo es habilitar la infraestructura en la nube y disponibilizar el núcleo de autenticación y visualización de planes de ElectroLink. Esto permite sincronizar la propuesta de valor presentada en la Landing Page con la persistencia real del Web Service.

La siguiente tabla detalla la descomposición de las Historias de Usuario asignadas en Work-Items/Tasks específicas para su ejecución y control de estado:

| Sprint # | User Story Id | User Story Title | Work-Item / Task Id | Work-Item / Task Title | Description | Estimation (Hours) | Assigned To | Status (To-do / In Process / To Review / Done) |
| :--- | :--- | :--- | :--- | :--- | :--- | :---: | :--- | :--- |
| **Sprint 1** | US-60 | Información institucional de la startup | TS1-01 | Maquetación HTML/CSS del Hero Section | Diseñar y codificar la sección principal de la Landing con la identidad de la marca. | 4 | Ethan Aliaga | Done |
| **Sprint 1** | US-61 | Navegación fluida y adaptativa | TS1-02 | Configuración Responsive & Media Queries | Adaptar los contenedores y el Header fijo para una correcta visualización en móviles y tablets. | 5 | Leandro Contreras | Done |
| **Sprint 1** | US-58 | Características y beneficios en Landing Page | TS1-03 | Componente de beneficios y Formulario | Desarrollar la sección de ventajas competitivas para técnicos y propietarios. | 6 | Alessandra Becerra | Done |
| **Sprint 1** | US-01 | Registro de cuenta como Propietario | TS1-04 | Endpoint POST `/api/v1/authentication/sign-up` | Implementar lógica de negocio, DTOs y persistencia en Base de Datos para el registro de usuarios. | 8 | Mateo Cabanillas | Done |
| **Sprint 1** | US-04 | Autenticación de usuario registrado | TS1-05 | Endpoint POST `/api/v1/authentication/sign-in` | Desarrollar el proceso de login seguro mediante la generación y validación de tokens JWT. | 8 | Italo Sanchez | Done |
| **Sprint 1** | US-08 | Visualización del perfil de propietario | TS1-06 | Endpoint GET `/api/v1/profiles/{id}` | Implementar controlador y servicio para recuperar los datos específicos de perfil según el ID. | 6 | Ivo Machado | Done |
| **Sprint 1** | US-59 | Visualización de planes de suscripción | TS1-07 | Endpoint GET `/api/v1/plans` | Desarrollar el listado transaccional en el Backend de los tipos de planes vigentes (Free, Premium). | 4 | Italo Sanchez | Done |
| **Sprint 1** | *N/A* | *Constraint General* | TS1-08 | Pipeline CI/CD & Despliegue en Render / GitHub | Configurar las variables de entorno de producción y automatizar despliegues con la rama main. | 6 | Leandro Contreras | Done |

#### 6.2.1.4. Development Evidence for Sprint Review.

Durante el Sprint 1 se consiguió un avance parcial en el despliegue de la landing page. Actualmente, el sitio ya dispone de varias secciones funcionales que brindan información relevante sobre los servicios y el equipo de Hampcoders.
[Visita y conoce acerca de nuestra plataforma ElectroLink aquí](https://open-source-4341.github.io/Landing-Page/)

- **Sección Hero:** presenta la propuesta de valor principal enfocada en conectar propietarios y pequeñas y medianas empresas con Tecnicos, resaltando beneficios como cortocircuitos en las instalaciones, asegurando conexión entre dispositivos y resolviendo problemas electricos.
(assets/img/cap6/home.png)

- **Sección ¿Cómo funciona?** Explica el flujo de uso de la plataforma tanto para Tecnicos electricistas como para PYMEs y Propietarios de Hogar, mostrando el proceso de registro, búsqueda de encuentros y ofrecer dispositivos IoT para monitorear el flujo electrico.
(assets/img/cap6/how-it-works.png)

- **Sección Nuestra Solución** describe los principales pilares de la plataforma, como la comunidad activa, el soporte tecnico y el respaldo práctico. 
(assets/img/cap6/our-solution.png)

- **Seccion Electrolink en accion** El usuario puede visualizar una demostración práctica de la plataforma a través de un video interactivo que muestra la interfaz de la aplicación en funcionamiento. 
(assets/img/cap6/electrolink-in-action.png)

- **Seccion Mision, Vision y Valores** Para que los usuarios sientan conexión desde el primer momento conociendo nuestros alcances, los valores de nuestra startup y lo que queremos como equipo para satisfacer las necesidades de nuestros clientes potenciales.
(assets/img/cap6/goals.png)

#### 6.2.1.5. Testing Suite Evidence for Sprint Review.

#### Test Unitarios del Bounded Context de Analytics

\vspace{1em}

En esta imagen se puede visualizar las pruebas unitarias del queryservice del bounded context de analytics.
\
![Analytics unit test 1](assets/img/cap6/unittest/analytics/analytics-queryservice-ut.png)

Este es el servicio principal del contexto. Las pruebas verifican que las tres analíticas principales generen las matemáticas, agrupaciones y recuentos de manera correcta.

#### Test Unitarios del Bounded Context de Assets

\vspace{1em}

Pruebas para el servicio de componentes (`ComponentCommandServiceImpl`).
\
![Assets unit test 1](assets/img/cap6/unittest/assets/assets-componentcommand-ut.png)

Pruebas para el servicio de tipos de componentes (`ComponentTypeCommandServiceImpl`).
\
![Assets unit test 2](assets/img/cap6/unittest/assets/assets-ctypecommand-ut.png)



#### Test Unitarios del Bounded Context de Iam

\vspace{1em}

Pruebas para el servicio de comandos de roles (`RoleCommandServiceImpl`).
\
![Iam unit test 1](assets/img/cap6/unittest/iam/iam-rolecommand-ut.png)

Pruebas para el servicio de comandos de usuarios (`UserCommandServiceImpl`).
\
![Iam unit test 2](assets/img/cap6/unittest/iam/iam-usercommand-ut.png)


#### Test Unitarios del Bounded Context de Monitoring

\vspace{1em}

Pruebas para la creación y actualización de calificaciones (ratings).

![Monitoring unit test 1](assets/img/cap6/unittest/monitoring/monitoring-ratingcommand-ut.png)

Pruebas para la generación y gestión de estado de los reportes.

![Monitoring unit test 2](assets/img/cap6/unittest/monitoring/monitoring-reportcommand-ut.png)

Pruebas para adjuntar y eliminar fotos de evidencia en los reportes.

![Monitoring unit test 3](assets/img/cap6/unittest/monitoring/monitoring-rphotocommand-ut.png)


#### Test Unitarios del Bounded Context de Profiles

\vspace{1em}

Pruebas para el servicio de comandos de perfiles (`ProfileCommandServiceImpl`). Este servicio maneja la persistencia y la validación de la información de los usuarios (ya sean `HomeOwner` o `Technician`).

![Profiles unit test 1](assets/img/cap6/unittest/profiles/profiles-profilecommand-ut.png)

Pruebas para el servicio de consultas de perfiles (`ProfileQueryServiceImpl`).

![Profiles unit test 2](assets/img/cap6/unittest/profiles/profiles-profilequery-ut.png)

#### Test Unitarios del Bounded Context de Service Delivery Process

\vspace{1em}

Pruebas para la creación, actualización y gestión de las solicitudes de servicio realizadas por el cliente.

![Sdp unit test 1](assets/img/cap6/unittest/sdp/sdp-requestcommand-ut.png)

Pruebas para la creación y actualización de los horarios de disponibilidad del técnico.

![Sdp unit test 2](assets/img/cap6/unittest/sdp/sdp-schedulecommand-ut.png)

Pruebas para gestionar la entidad del servicio base.

![sdp unit test 3](assets/img/cap6/unittest/sdp/sdp-servicecommand-ut.png)



#### Test Unitarios del Bounded Context de Subscription

\vspace{1em}

Pruebas para garantizar que la creación de planes (Basic, Premium, etc.) asigne correctamente los precios y nombres, y se persista en la base de datos sin duplicados.

![Subscription unit test 1](assets/img/cap6/unittest/subscription/sub-plancommand-ut.png)

Pruebas sobre la gestión de suscripciones de los usuarios, incluyendo verificaciones de renovaciones y creaciones iniciales.

![Subscription unit test 2](assets/img/cap6/unittest/subscription/sub-subcommand-ut.png)

Verifica la funcionalidad de búsqueda de un plan mediante su ID o su tipo.

![Subscription unit test 3](assets/img/cap6/unittest/subscription/sub-planquery-ut.png)


#### 6.2.1.6. Execution Evidence for Sprint Review.

Durante el Sprint 1, el equipo de Hampcoders logró implementar y desplegar la Landing Page de ElectroLink, junto con una versión parcial del Frontend Web Application. El objetivo principal fue construir el primer punto de contacto con los usuarios potenciales, presentando de forma clara y atractiva la propuesta de valor de la plataforma: conectar a propietarios de hogares y pequeñas y medianas empresas con técnicos eléctricos de confianza, permitiendo además el monitoreo del consumo eléctrico en tiempo real mediante dispositivos inteligentes.

A continuación se detallan las principales vistas implementadas durante este Sprint:

#### Landing Page – Sección Hero

La sección principal de la Landing Page presenta la propuesta de valor central de ElectroLink. Se comunica de manera directa el beneficio de conectar usuarios con técnicos electricistas confiables, destacando la capacidad de la plataforma para detectar problemas eléctricos como cortocircuitos antes de que se conviertan en fallas graves.

/
![Landing Page – Sección Hero](assets/img/cap6/home.png)


#### Landing Page – Sección ¿Cómo funciona?

Esta sección explica el flujo de uso de la plataforma, diferenciando la experiencia para dos tipos de usuario: Técnicos Electricistas y PYMEs / Propietarios de Hogar. Se ilustra el proceso de registro, búsqueda de técnicos y la integración con dispositivos IoT para el monitoreo del flujo eléctrico en tiempo real.

/
![Landing Page – Sección Hero](assets/img/cap6/como-funciona.png)


#### Landing Page – Sección ElectroLink en Acción

El usuario puede visualizar una demostración práctica mediante un video interactivo embebido en la página, que muestra la interfaz de la aplicación en funcionamiento real. Esto permite a los visitantes comprender de forma intuitiva cómo opera la plataforma antes de registrarse.

/
![Landing Page – Sección Hero](assets/img/cap6/solucion-landing.png)

###### Enlace a la Landing Page desplegada

Visita y conoce nuestra plataforma ElectroLink aquí:

[Ver Landing](https://hampcoders.github.io/Landing-Page/) 

#### 6.2.1.7. Services Documentation Evidence for Sprint Review.

Durante el Sprint 1, el equipo avanzó con la implementación del backend de ElectroLink bajo una arquitectura de Domain-Driven Design (DDD), estructurada en Bounded Contexts independientes. Se implementaron y documentaron los principales endpoints REST de la API, con documentación generada mediante OpenAPI / Swagger.
La documentación se encuentra disponible de forma local en https://electrolink-backend.onrender.com/swagger/index.html durante este Sprint, dado que el despliegue del backend en producción está previsto para un sprint posterior.
A continuación se presenta la relación de endpoints documentados por Bounded Context:

### API Documentation - ElectroLink

### Bounded Context: IAM (Gestión de Identidad y Acceso)

| Endpoint | Verbo HTTP | Descripción | Parámetros | Response ejemplo |
| :--- | :--- | :--- | :--- | :--- |
| `/api/v1/authentication/sign-in` | **POST** | Autenticación de usuario | `{ "username": "string", "password": "string" }` | `{ "token": "jwt_string", "userId": 1 }` |
| `/api/v1/authentication/sign-up` | **POST** | Registro de nuevo usuario | `{ "username": "string", "password": "string", "roles": ["ROLE_USER"] }` | `{ "id": 1, "username": "string" }` |
| `/api/v1/roles` | **GET** | Lista todos los roles disponibles | — | `[{ "id": 1, "name": "ROLE_USER" }]` |
| `/api/v1/users/{id}` | **GET** | Obtiene un usuario por ID | `id` (path param) | `{ "id": 1, "username": "string" }` |

#### Captura de interacción con Swagger UI – Endpoint POST /sign-up con datos de prueba

/
![Landing Page – Sección Hero](assets/img/cap6/IAM-Swagger.png)

---

## Bounded Context: Profiles

| Endpoint | Verbo HTTP | Descripción | Parámetros | Response ejemplo |
| :--- | :--- | :--- | :--- | :--- |
| `/api/v1/profiles` | **POST** | Crea un perfil de usuario (HomeOwner o Technician) | `{ "firstName": "string", "lastName": "string", "email": "string", "profileType": "HOMEOWNER" }` | `{ "id": 1, "fullName": "John Doe" }` |
| `/api/v1/profiles/{id}` | **GET** | Obtiene el perfil por ID | `id` (path param) | `{ "id": 1, "email": "...", "profileType": "HOMEOWNER" }` |
| `/api/v1/profiles/{id}` | **PUT** | Actualiza datos del perfil | `id` (path), body con campos a actualizar | `{ "id": 1, "email": "nuevo@email.com" }` |


#### Captura de interacción con Swagger UI – Endpoint GET /profiles/{id}
/
![Swagger Profiles](assets/img/cap6/profiles-endpoints.png)

---

## Bounded Context: Assets (Componentes IoT)

| Endpoint | Verbo HTTP | Descripción | Parámetros | Response ejemplo |
| :--- | :--- | :--- | :--- | :--- |
| `/api/v1/components` | **POST** | Registra un nuevo componente IoT | `{ "name": "string", "componentTypeId": 1 }` | `{ "id": 1, "name": "Smart Meter X1" }` |
| `/api/v1/component-types` | **GET** | Lista todos los tipos de componente | — | `[{ "id": 1, "name": "Medidor de Consumo" }]` |
| `/api/v1/components/{id}` | **DELETE** | Elimina un componente por ID | `id` (path param) | `{ "deleted": true }` |

#### Captura de interacción con Swagger UI – Endpoint POST /components con datos de prueba
/
![Swagger Assets](assets/img/cap6/assets-endpoints.png)

---

## Bounded Context: Monitoring (Reportes y Calificaciones)

| Endpoint | Verbo HTTP | Descripción | Parámetros | Response ejemplo |
| :--- | :--- | :--- | :--- | :--- |
| `/api/v1/reports` | **POST** | Crea un reporte de falla eléctrica | `{ "description": "string", "profileId": 1 }` | `{ "id": 1, "status": "OPEN" }` |
| `/api/v1/reports/{id}/photos` | **POST** | Adjunta foto de evidencia al reporte | `id` (path), form-data imagen | `{ "photoUrl": "https://..." }` |
| `/api/v1/ratings` | **POST** | Registra una calificación del servicio | `{ "score": 5, "comment": "string", "serviceId": 1 }` | `{ "id": 1, "score": 5 }` |

#### Captura de interacción con Swagger UI – Endpoint POST /reports con datos de prueba
![Swagger Monitoring](assets/img/cap6/reports-swagger.png)

---

## Bounded Context: Service Delivery Process (SDP)

| Endpoint | Verbo HTTP | Descripción | Parámetros | Response ejemplo |
| :--- | :--- | :--- | :--- | :--- |
| `/api/v1/service-requests` | **POST** | Crea solicitud de servicio técnico | `{ "description": "string", "profileId": 1, "scheduleId": 1 }` | `{ "id": 1, "status": "PENDING" }` |
| `/api/v1/schedules` | **POST** | Registra disponibilidad del técnico | `{ "technicianId": 1, "availableFrom": "2026-05-10T09:00", "availableTo": "2026-05-10T17:00" }` | `{ "id": 1, "status": "AVAILABLE" }` |
| `/api/v1/services/{id}` | **PATCH** | Actualiza el estado del servicio | `id` (path), `{ "status": "COMPLETED" }` | `{ "id": 1, "status": "COMPLETED" }` |

#### Captura de interacción con Swagger UI – Endpoint POST /service-requests
/
![Swagger SDP](assets/img/cap6/swagger/sdp-endpoints.png)

---

## Bounded Context: Subscription

| Endpoint | Verbo HTTP | Descripción | Parámetros | Response ejemplo |
| :--- | :--- | :--- | :--- | :--- |
| `/api/v1/plans` | **GET** | Lista todos los planes disponibles | — | `[{ "id": 1, "name": "Basic", "price": 0.0 }, { "id": 2, "name": "Premium", "price": 29.99 }]` |
| `/api/v1/plans/{id}` | **GET** | Obtiene un plan por ID o tipo | `id` (path param) | `{ "id": 2, "name": "Premium", "price": 29.99 }` |
| `/api/v1/subscriptions` | **POST** | Crea una suscripción para un usuario | `{ "userId": 1, "planId": 2 }` | `{ "id": 1, "status": "ACTIVE", "renewalDate": "2026-06-10" }` |

#### Captura de interacción con Swagger UI – Endpoint GET /plans
/
![Swagger Subscription](assets/img/cap6/subscriptions.png)

---

## Repositorio de Web Services

| Repositorio | URL |
| :--- | :--- |
| **Backend – ElectroLink API** | [https://github.com/Hampcoders-Desarrollo-IoT/electrolink-backend-api](https://github.com/Hampcoders-Desarrollo-IoT/electrolink-backend-api) |


#### 6.2.1.8. Software Deployment Evidence for Sprint Review.

Durante el Sprint 1, el equipo realizó el despliegue de los primeros productos digitales de ElectroLink, enfocándose en la infraestructura en la nube y el entorno productivo del Web Service / Backend de la aplicación. A continuación se describen los procesos de despliegue llevados a cabo.

### Despliegue del Web Service / Backend – Render

El Backend de ElectroLink fue desplegado utilizando la plataforma de hosting en la nube **Render**, conectada directamente al repositorio del proyecto para permitir un flujo de Integración y Despliegue Continuo (CI/CD). Los pasos realizados fueron los siguientes:

**1. Creación del repositorio y estructura del Backend**
Se consolidó el código fuente de la API de ElectroLink en el repositorio público `electrolink-backend` bajo la organización `open-source-4341` en GitHub. Este repositorio contiene toda la arquitectura de software (controladores, servicios, repositorios y configuraciones de base de datos) lista para entornos cloud.

 *Captura: Estructura del repositorio GitHub con el código fuente del Web Service (Backend)*
![Repositorio GitHub - Backend](assets/img/cap6/backend-deployment.jpeg)

**2. Configuración del servicio web y rama de despliegue**
En el panel de control de Render, se vinculó el repositorio de GitHub y se creó un nuevo *Web Service*. Se configuraron las variables de entorno necesarias (conexión a la base de datos, credenciales secretas, etc.) y se seleccionó la rama principal (`main`) como la fuente oficial de despliegue automatizado. Cada nuevo cambio subido a esta rama gatilla un *build* automático.

 *Captura: Configuración del pipeline de despliegue y rama main en la plataforma cloud*
![Configuración del despliegue en Render](assets/img/cap6/deployment-main.png)

**3. Verificación y estado del despliegue en producción**
Una vez concluido exitosamente el proceso de compilación (*build*) y ejecución (*deploy*) en los servidores de Render, se comprobó que el servicio se encontrara en estado *Live*. Se verificó la disponibilidad del Backend accediendo a su URL pública de producción y realizando peticiones de prueba a los endpoints expuestos de la API:
[https://github.com/Hampcoders-Desarrollo-IoT/electrolink-backend-api](https://github.com/Hampcoders-Desarrollo-IoT/electrolink-backend-api)

 *Captura: Confirmación de despliegue exitoso (Live) en el dashboard de Render*
![Verificación del despliegue en Render](assets/img/cap6/render-backend.jpeg)


Entorno de Desarrollo Local – Backend (Spring Boot)
Durante este Sprint el backend de ElectroLink fue desarrollado y ejecutado en entorno local. El stack tecnológico utilizado fue:

Lenguaje: Java 17
Framework: Spring Boot 3.x
Base de datos: postgresql (local)
Documentación API: Springdoc OpenAPI (Swagger UI)

Pasos realizados:

Se configuró el proyecto Spring Boot con las dependencias necesarias (Spring Web, Spring Data JPA, Spring Security, Springdoc OpenAPI).
Se configuró la base de datos postgresql local con las credenciales de entorno en application.properties.

Se accedió a la documentación Swagger en https://electrolink-backend.onrender.com/swagger/index.html para validar los endpoints implementados.


 Captura: Swagger UI corriendo en entorno local con los endpoints listados
 /
![Swagger Subscription](assets/img/cap6/swagger-general.png)



#### 6.2.1.9. Team Collaboration Insights during Sprint.