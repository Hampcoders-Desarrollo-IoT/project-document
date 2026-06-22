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

### 6.2.2.6. Execution Evidence for Sprint Review.

#### Aplicación Web


#### Aplicación Mobile

### 6.2.2.7. Services Documentation Evidence for Sprint Review.

### 6.2.2.8. Software Deployment Evidence for Sprint Review.

### 6.2.2.9. Team Collaboration Insights during Sprint.

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


