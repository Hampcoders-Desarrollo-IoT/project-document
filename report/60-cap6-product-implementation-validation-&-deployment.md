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

![Gitflow Graphic](assets/img/cap6/git-flow.png)

Gitflow es un modelo de ramificación para Git que organiza el desarrollo de software mediante ramas estandarizadas, facilitando la colaboración y el control del código en equipo. En Glottia, utilizamos este modelo para gestionar de forma eficiente los repositorios de nuestros microservicios.

La rama main contiene las versiones estables y listas para producción de cada microservicio, previamente validadas y etiquetadas para mantener un control de las releases.

La rama develop almacena la versión en desarrollo del proyecto, integrando las funcionalidades completadas durante el sprint antes de pasar a producción.

Por otro lado, las ramas feature se utilizan para desarrollar funcionalidades o tareas específicas de manera independiente, siguiendo la convención feature/nombre-descriptivo y permitiendo integrar los cambios mediante Pull Requests.

### 6.1.3. Source Code Style Guide & Conventions.

### 6.1.4. Software Deployment Configuration.

## 6.2. Landing Page, Services & Applications Implementation.

### 6.2.1. Sprint n
#### 6.2.1.1. Sprint Planning n.

En esta seccion se detalle la planificacion y distribucion llevada a cabo para abarcar el contenido a abarcar en este presente sprint. Nos centramos en enfocar el diseño de la Landing Page, el Backend y una implementación parcial del frontend.

| Campo                                 | Detalle                                                                                                                                                                                      |
|---------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Sprint #**                          | Sprint 1                                                                                                                                                                                     |
| **Date**                              | 10-05-2026                                                                                                                                                                                   |
| **Time**                              | 12:00 PM                                                                                                                                                                                     |
| **Location**                          | Virtual – Discord                                                                                                                                                                            |
| **Prepared By**                       | Leandro Contreras                                                                                                                                                                            |
| **Attendees (to planning meeting)**   | Leandro Contreras, Alessandra Becerra, Ivo Machado, Ethan Aliaga, Italo Sanchez, Mateo Cabanillas                                                                                                                         |
| **Sprint n-1 Review Summary**         | Este es el primer Sprint, por lo que este campo aún no es aplicable                                                                                                                         |
| **Sprint n-1 Retrospective Summary**  | Este es el primer Sprint, por lo que este campo aún no es aplicable                                                                                                                         |
| **Sprint 1 Goal**                     | Nuestro objetivo es desarrollar e implementar una landing page responsiva y accesible, junto con la primera versión del frontend application, para presentar nuestra solución de manera clara a los usuarios potenciales. Esto busca generar confianza y mejorar la experiencia de los visitantes en su primer contacto con la plataforma. El resultado se validará una vez publicada la página en producción y analizado el tráfico obtenido al cierre del Sprint. |
| **Sprint 1 Velocity**                 |           |
| **Sum of Story Points**               |  |

#### 6.2.1.2. Aspect Leaders and Collaborators.

En este Sprint el equipo se enfocó en construir la Landing Page de ElectroLink, asimismo una implementación parcial del Frontend Web Application. Es de suma importancia tener asignado un rol y especificar las US a llevar a cabo para fomentar un flujo de trabajo continuo, responsable y efectivo.

| Team Member (Last Name, First Name)          | GitHub Username | Diseño visual y UX del Hero Section | Desarrollo del Formulario de contacto | Adaptación responsive para dispositivos móviles | Diseño y funcionalidad del Header fijo | Implementación de la sección de Características principales |
|----------------------------------------------|-----------------|---------------------------------------|-----------------------------------------|-------------------------------------------------|----------------------------------------|----------------------------------------------------------------|
| Becerra Tejeda, Alessandra Nicole            | aleeBecerra     |  C                                    |      L                                  |                                                |                                       |                                                               |
| Contreras López, Leandro Saul                | WiDDsito        |                                      |        C                                |                L                                |                                       |                                                               |
| Aliaga Aguirre, Ethan Matías             | MatFragg       | L                                     |                                        |                                                |                                       |                                                               |
| Machado Bracamonte, Ivo Marcelo    | ivommb11           |                                      |                                        |                                                |        L                               |                                                               |
| Cabanillas Meza, José Mateo                 | marckszz        |                                      |                                        |                                                |                C                       |                                                       L        |
|Sanchez Manrique, Italo Ludwing | ItaloSanche | 



#### 6.2.1.3. Sprint Backlog n.

#### 6.2.1.4. Development Evidence for Sprint Review.

#### 6.2.1.5. Testing Suite Evidence for Sprint Review.

#### 6.2.1.6. Execution Evidence for Sprint Review.

#### 6.2.1.7. Services Documentation Evidence for Sprint Review.

#### 6.2.1.8. Software Deployment Evidence for Sprint Review.

#### 6.2.1.9. Team Collaboration Insights during Sprint.