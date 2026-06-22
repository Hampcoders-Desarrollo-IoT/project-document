### 6.2.2. Sprint 2

### 6.2.2.1. Sprint Planning 2.
En esta sección se describe el avance del proyecto ElectroLink durante el Sprint 2, tanto a nivel de desarrollo del producto como del trabajo colaborativo del equipo. En este sprint, el enfoque principal está en el desarrollo de la aplicación web, el avance inicial de la aplicación móvil y la construcción del primer prototipo IoT utilizando Wokwi, alineado con la propuesta de valor de monitoreo eléctrico en tiempo real.

| Campo | Detalle |
|------|--------|
| **Sprint #** | Sprint 2 |
| **Sprint Planning Background** | En este Sprint 2, el equipo se enfoca en el desarrollo funcional del sistema ElectroLink, incluyendo la implementación de la aplicación web, avances iniciales de la aplicación móvil y la construcción de un prototipo IoT en Wokwi para el monitoreo eléctrico en tiempo real. |
| **Date** | 2025-05-26 |
| **Time** | 08:00 AM |
| **Location** | Vía presencial y virtual (reunión híbrida mediante Google Meet) |
| **Prepared By** | Italo Sanchez Manrique  |
| **Attendees (to planning meeting)** | Ethan Matias Aliaga /  Alessandra Becerra / José Mathias Meza / Leandro Contreras / Ivo Machado / Italo Sanchez  |
| **Sprint 1 Review Summary** | Durante el Sprint 1 se desarrolló el flujo principal de visualización de datos en la aplicación web, así como una primera versión funcional de la landing page. El equipo logró definir la estructura base del sistema y validar parcialmente la propuesta de valor del producto. |
| **Sprint 1 Retrospective Summary** | Se identificó la necesidad de mejorar el diseño de los flujos para alinearlos mejor con las user stories. Además, se planteó optimizar la organización del trabajo y la claridad en la distribución de tareas para los siguientes sprints. |
| **Sprint Goal & User Stories** | Implementar las funcionalidades base del sistema, incluyendo registro, autenticación y gestión de perfiles en la aplicación web, avances iniciales de la aplicación móvil y la simulación del monitoreo eléctrico mediante un prototipo IoT en Wokwi. |
| **Sprint 2 Goal** | Our focus is on desarrollar las funcionalidades base de la aplicación web, iniciar la app móvil y construir un prototipo IoT funcional.<br>We believe it delivers validación temprana del sistema y permite a usuarios monitorear su consumo eléctrico y conectarse con técnicos.<br>This will be confirmed when exista una versión funcional del sistema web, avances navegables en la app móvil y un prototipo IoT simulando datos en tiempo real. |
| **Sprint 2 Velocity** | 20 Story Points |
| **Sum of Story Points** | 19 – 20 Story Points |

### 6.2.2.2. Aspect Leader and Collaborators.
En esta sección se presenta la matriz de liderazgo y colaboración (Leadership and Collaboration Matrix - LACX), la cual define los roles de cada integrante del equipo dentro de los distintos aspectos considerados en el Sprint.  

Los aspectos definidos corresponden a los *bounded contexts* del sistema, los cuales representan subconjuntos funcionales clave como gestión de identidad, suscripciones, perfiles, servicios, activos e IoT.  

Para cada aspecto se ha asignado un **líder (L)** responsable principal de su desarrollo y uno o más **colaboradores (C)** que apoyan en la implementación. Esta organización permite mejorar la comunicación, distribución del trabajo y eficiencia del equipo durante el Sprint.

---

### Leadership and Collaboration Matrix (LACX)

| Team Member (Last Name, First Name) | GitHub Username | Identity and Access Management (IAM) | Subscription and Payments | Profiles and Preferences | Service Design and Planning | Service Operation and Monitoring | Assets and Resource Management | IoT Monitoring and Edge Processing |
|------------------------------------|----------------|-------------------------------------|---------------------------|--------------------------|-----------------------------|----------------------------------|--------------------------------|------------------------------------|
| Aliaga Aguirre, Ethan Matias       | ethanaliaga    | L | C | C | C | C | C | C |
| Becerra Tejeda, Alessandra Nicole  | alessandrabecerra | C | L | C | C | C | C | C |
| Cabanillas Meza, José Mateo        | josecabanillas | C | C | C | C | C | C | L |
| Contreras López, Leandro Saul      | leandrocontreras | C | C | L | C | C | C | C |
| Sanchez Manrique, Italo Ludwing    | italosanchez   | C | C | C | L | C | C | C |
| Machado Bracamonte, Ivo Marcelo    | ivomachado     | C | C | C | C | L | L | C |

---

### 6.2.2.3. Sprint Backlog 2.
![Sprint Backlog 2](assets/img/cap7/Sprint2-iot-backlog.png)
### 6.2.2.4. Development Evidence for Sprint Review.

En esta sección se presentan las evidencias de desarrollo correspondientes al Sprint 2, incluyendo los commits realizados en los repositorios de GitHub para las aplicaciones web, mobile y el prototipo IoT. Estos commits reflejan el progreso, nuevas funcionalidades implementadas, correcciones de errores y mejoras realizadas durante el sprint.

| Repository | Branch | Commit Id | Commit Message | Commit Message Body | Committed on (Date) |
|------------|--------|-----------|----------------|----------------------|----------------------|
| https://github.com/Hampcoders-Desarrollo-IoT/electrolink-web-app | develop | - | feat: include first version of client dashboard | | Jun 21, 2026 |
| https://github.com/Hampcoders-Desarrollo-IoT/electrolink-web-app | develop | - | feat: add new section for create a property | | Jun 21, 2026 |
| https://github.com/Hampcoders-Desarrollo-IoT/electrolink-web-app | develop | - | feat: add analytics | | Jun 21, 2026 |
| https://github.com/Hampcoders-Desarrollo-IoT/electrolink-web-app | develop | - | feat: analytics & request operation for homeowner added | | Jun 21, 2026 |
| https://github.com/Hampcoders-Desarrollo-IoT/electrolink-web-app | develop | - | feat: subscriptions and dashboard refactor | | Jun 21, 2026 |
| https://github.com/Hampcoders-Desarrollo-IoT/electrolink-web-app | develop | - | Realizando las vistas de login | | Jun 21, 2026 |
| https://github.com/Hampcoders-Desarrollo-IoT/electrolink-web-app | develop | - | Realizando el dashboard del técnico (diferentes vistas) | | Jun 21, 2026 |
| https://github.com/Hampcoders-Desarrollo-IoT/electrolink-web-app | develop | - | Merge branch 'develop' into feat/dashboard-cliente | | Jun 21, 2026 |
| https://github.com/Hampcoders-Desarrollo-IoT/electrolink-web-app | develop | - | Merge pull request #1 feat/create-account | | Jun 21, 2026 |
| https://github.com/Hampcoders-Desarrollo-IoT/electrolink-mobile-app | feat/dashboard-tecnico | - | feat: add empty project for mobile app | | Jun 20, 2026 |
| https://github.com/Hampcoders-Desarrollo-IoT/electrolink-mobile-app | feat/dashboard-tecnico | - | fix: enhance property management flow | | Jun 21, 2026 |
| https://github.com/Hampcoders-Desarrollo-IoT/electrolink-mobile-app | feat/dashboard-tecnico | - | fix: profiles bug | | Jun 21, 2026 |
| https://github.com/Hampcoders-Desarrollo-IoT/electrolink-mobile-app | feat/dashboard-tecnico | - | fix: enhance profiles bounded context | | Jun 21, 2026 |
| https://github.com/Hampcoders-Desarrollo-IoT/electrolink-backend | develop | - | fix: resolve issues while using IAM | | Jun 20, 2026 |


### 6.2.2.5. Testing Suite Evidence for Sprint Review.

### 6.2.2.6. Execution Evidence for Sprint Review

\vspace{1em}

Esta sección resume la evidencia de ejecución del Sprint 2, donde se verificó el correcto funcionamiento de los endpoints del backend en .NET integrados con la base de datos relacional PostgreSQL con extensión espacial PostGIS. Las solicitudes fueron validadas localmente verificando que la gestión de sesiones, enrutamiento HTTP y la autenticación mediante tokens JWT (TokenSettings) operan de forma centralizada.

#### Aplicación Web

##### Pantalla de Registro de Usuarios

Se verificó el correcto funcionamiento de la pantalla de creación de cuentas, la cual recopila los datos del usuario nuevo y se conecta asíncronamente con el servicio de identidad del backend para su persistencia segura.

![Web - SignUp](assets/img/cap7/signUp_front.png)

Pantalla de Inicio de Sesión

Se comprobó el funcionamiento de la pantalla de autenticación, la cual permite validar credenciales e interceptar el token JWT en el frontend para autorizar de manera automática las peticiones posteriores del sistema.

![Web - LogIn](assets/img/cap7/logIn_front.png)

Gestión de Perfil de Usuario 

Se verificó la carga de datos en el módulo de administración del perfil una vez activo en la base de datos. La vista recupera de forma dinámica la información personal del usuario.

![Web - Profile](assets/img/cap7/profile_front.png)

Panel de Inventario Personal de Activos

Se validó la correcta visualización del módulo de control de stock técnico, el cual unifica indicadores analíticos de ítems totales, unidades globales y estados de alerta.

![Web - Inventory](assets/img/cap7/inventory_front.png)

Catálogo General de Componentes

Se verificó el funcionamiento del Catálogo de Componentes, el cual consolida las métricas de activos físicos y ofrece capacidades dinámicas para paginación, ordenamiento y filtrado por texto.

![Web - Catalog](assets/img/cap7/component_catalog_front.png)

Formulario Modal para Registro de Nuevos Componentes

Se comprobó la inserción de nuevos activos mediante un formulario. La interfaz procesa campos obligatorios como el nombre y la descripción del material eléctrico antes de enviar la orden de creación.

![Web - Catalog](assets/img/cap7/newcomponent_front.png)


Gestión de Tipos de Componentes

Se verificó el despliegue del listado de tipos de componentes, el cual lee del backend  (ej. la familia 'Relay') junto con su estado lógico y opciones CRUD.

![Web - Catalog](assets/img/cap7/component_types_front.png)

Formulario de Nuevo Tipo de Componentes

Se validó la ventana para la expansión del catálogo, la cual permite añadir nuevas categorías y descripciones para la normalización del inventario.

![Web - Catalog](assets/img/cap7/newcomponent_type_front.png)


#### Aplicación Mobile

\
![Mobile - Login](assets/img/cap6/dashboard.jpeg)
\
![Mobile - Login](assets/img/cap6/mob-regsitro.jpeg)
\
![Mobile - Login](assets/img/cap6/m-login.jpeg)
\
![Mobile - Login](assets/img/cap6/mobile-vista-empresa.jpeg)
\
![Mobile - Login](assets/img/cap6/mobile-vista-empresa2.jpeg)
\
![Mobile - Login](assets/img/cap6/mobile-vista-empresa3.jpeg)
\

### 6.2.2.7. Services Documentation Evidence for Sprint Review.

### 6.2.2.8. Software Deployment Evidence for Sprint Review.

Durante el Sprint 2, el equipo realizó el despliegue de la aplicaicon web y mobile de ElectroLink, enfocándose en la infraestructura en la nube y el entorno productivo del Web Service / Backend de la aplicación y mobile. A continuación se describen los procesos de despliegue llevados a cabo.

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
![Verificación del despliegue en Render](assets/img/cap6/render-backeng.png)

### 6.2.2.9. Team Collaboration Insights during Sprint.

Finalmente, se presentan los insights de colaboración del equipo durante el Sprint 2, los cuales reflejan la coordinación, comunicación y trabajo conjunto en el desarrollo de las soluciones web y mobile del proyecto.

Web: Durante este sprint, el equipo se enfocó en el desarrollo de la aplicación web, priorizando la implementación de interfaces funcionales y una experiencia de usuario intuitiva. Se trabajó de manera colaborativa en la construcción de módulos clave como la gestión de usuarios, visualización de datos provenientes del sistema IoT, y la administración de servicios. Asimismo, se realizaron integraciones con APIs para asegurar la correcta comunicación con el backend. El equipo mantuvo una comunicación constante para resolver conflictos de diseño y asegurar la consistencia en la interfaz, además de aplicar buenas prácticas de desarrollo para garantizar escalabilidad y mantenimiento del sistema.
\
![insight-sprint2-web](assets/img/cap7/insight-sprint2.png)

Mobile: En paralelo, el equipo desarrolló la aplicación móvil, enfocándose en la accesibilidad y usabilidad en dispositivos móviles. Se implementaron funcionalidades esenciales como monitoreo en tiempo real, notificaciones y visualización simplificada de datos IoT. La colaboración fue clave para adaptar las funcionalidades del sistema web a un entorno móvil, optimizando la navegación y el rendimiento. Además, se realizaron pruebas en diferentes dispositivos para asegurar compatibilidad y una experiencia de usuario fluida. La coordinación entre los miembros permitió mantener coherencia entre ambas plataformas (web y mobile), garantizando una integración sólida del sistema.

\
![insight-sprint2-mobile](assets/img/cap7/insight-sprint2-mobile.png)

#### Kanban Board and Tasks
\
![Kanban](assets/img/cap7/Sprint2-iot-kanban1.png)
\
![Kanban](assets/img/cap7/Sprint2-iot-kanban2.png)
\
### 6.3. Validation Interviews.

### 6.3.1. Diseño de Entrevistas.

### 6.3.2. Registro de Entrevistas.

### 6.3.3. Evaluaciones según heurísticas.

### 6.4. Video About-the-Product.


