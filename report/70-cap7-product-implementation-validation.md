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
\

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
\
![Web - SignUp](assets/img/cap7/signUp_front.png)

##### Pantalla de Inicio de Sesión

Se comprobó el funcionamiento de la pantalla de autenticación, la cual permite validar credenciales e interceptar el token JWT en el frontend para autorizar de manera automática las peticiones posteriores del sistema.
\
![Web - LogIn](assets/img/cap7/logIn_front.png)

##### Gestión de Perfil de Usuario 

Se verificó la carga de datos en el módulo de administración del perfil una vez activo en la base de datos. La vista recupera de forma dinámica la información personal del usuario.
\
![Web - Profile](assets/img/cap7/profile_front.png)

##### Panel de Inventario Personal de Activos

Se validó la correcta visualización del módulo de control de stock técnico, el cual unifica indicadores analíticos de ítems totales, unidades globales y estados de alerta.
\
![Web - Inventory](assets/img/cap7/inventory_front.png)

##### Catálogo General de Componentes

Se verificó el funcionamiento del Catálogo de Componentes, el cual consolida las métricas de activos físicos y ofrece capacidades dinámicas para paginación, ordenamiento y filtrado por texto.
\
![Web - Catalog](assets/img/cap7/component_catalog_front.png)

##### Formulario Modal para Registro de Nuevos Componentes

Se comprobó la inserción de nuevos activos mediante un formulario. La interfaz procesa campos obligatorios como el nombre y la descripción del material eléctrico antes de enviar la orden de creación.
\
![Web - New Component](assets/img/cap7/newcomponent_front.png)


##### Gestión de Tipos de Componentes

Se verificó el despliegue del listado de tipos de componentes, el cual lee del backend  (ej. la familia 'Relay') junto con su estado lógico y opciones CRUD.
\
![Web - Component Type](assets/img/cap7/component_types_front.png)

##### Formulario de Nuevo Tipo de Componentes

Se validó la ventana para la expansión del catálogo, la cual permite añadir nuevas categorías y descripciones para la normalización del inventario.
\
![Web - New Component Type](assets/img/cap7/newcomponent_type_front.png)


#### Aplicación Mobile

\
![Mobile - Login](assets/img/cap7/dashboard.jpeg)
\
![Mobile - Login](assets/img/cap7/mob-regsitro.jpeg)
\
![Mobile - Login](assets/img/cap7/m-login.jpeg)
\
![Mobile - Login](assets/img/cap7/mobile-vista-empresa.jpeg)
\
![Mobile - Login](assets/img/cap7/mobile-vista-empresa2.jpeg)
\
![Mobile - Login](assets/img/cap7/mobile-vista-empresa3.jpeg)
\

### 6.2.2.7. Services Documentation Evidence for Sprint Review.

### 6.2.2.8. Software Deployment Evidence for Sprint Review.

Durante el Sprint 2, el equipo realizó el despliegue de la aplicaicon web y mobile de ElectroLink, enfocándose en la infraestructura en la nube y el entorno productivo del Web Service / Backend de la aplicación y mobile. A continuación se describen los procesos de despliegue llevados a cabo.

### Despliegue del Web Service / Backend – Render

El Backend de ElectroLink fue desplegado utilizando la plataforma de hosting en la nube **Render**, conectada directamente al repositorio del proyecto para permitir un flujo de Integración y Despliegue Continuo (CI/CD). Los pasos realizados fueron los siguientes:

**1. Creación del repositorio y estructura del Backend**
Se consolidó el código fuente de la API de ElectroLink en el repositorio público `electrolink-backend` bajo la organización `open-source-4341` en GitHub. Este repositorio contiene toda la arquitectura de software (controladores, servicios, repositorios y configuraciones de base de datos) lista para entornos cloud.

 *Captura: Estructura del repositorio GitHub con el código fuente del Web Service (Backend)*
 \
![Repositorio GitHub - Backend](assets/img/cap6/backend-deployment.jpeg)

**2. Configuración del servicio web y rama de despliegue**
En el panel de control de Render, se vinculó el repositorio de GitHub y se creó un nuevo *Web Service*. Se configuraron las variables de entorno necesarias (conexión a la base de datos, credenciales secretas, etc.) y se seleccionó la rama principal (`main`) como la fuente oficial de despliegue automatizado. Cada nuevo cambio subido a esta rama gatilla un *build* automático.

 *Captura: Configuración del pipeline de despliegue y rama main en la plataforma cloud*
 \
![Configuración del despliegue en Render](assets/img/cap6/deployment-main.png)

**3. Verificación y estado del despliegue en producción**
Una vez concluido exitosamente el proceso de compilación (*build*) y ejecución (*deploy*) en los servidores de Render, se comprobó que el servicio se encontrara en estado *Live*. Se verificó la disponibilidad del Backend accediendo a su URL pública de producción y realizando peticiones de prueba a los endpoints expuestos de la API:
\
[https://github.com/Hampcoders-Desarrollo-IoT/electrolink-backend-api](https://github.com/Hampcoders-Desarrollo-IoT/electrolink-backend-api)

 *Captura: Confirmación de despliegue exitoso (Live) en el dashboard de Render*
 \
![Verificación del despliegue en Render](assets/img/cap6/render-backend.jpeg)

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

# Entrevistas por Segmento - Plataforma ElectroLink

## Segmento: Técnicos

1. Al ver el gráfico de la falla (picos de corriente) antes de llegar al lugar, ¿esto realmente le habría cambiado cómo aborda el servicio?
2. ¿La calibración hecha con el asistente de IA le pareció confiable, o preferiría ajustar los límites usted mismo?
3. ¿El corte automático del relé le parece una ventaja para su trabajo, o le preocupa no tener control manual en ese momento?
4. Después de ver el registro fotográfico y técnico digital, ¿lo usaría para respaldarse ante reclamos de clientes?
5. ¿Este pre-diagnóstico justificaría cobrar una tarifa mayor que un técnico sin esta información?
6. ¿Aceptaría trabajos asignados automáticamente por la plataforma con base en estas alertas, o prefiere elegir manualmente?
7. ¿Qué le faltó ver en la demo para confiar en usar esto como su canal principal de trabajo?


---

## Segmento: PYMEs

1. Después de ver la demo, ¿el dashboard le dio la visibilidad que necesita sobre el consumo y estado de sus circuitos?
2. ¿La alerta que recibió ante la sobrecarga simulada le pareció clara y con tiempo suficiente para actuar?
3. Al ver que el firmware corta el relé automáticamente sin depender del servidor, ¿esto le genera más confianza en la protección de su equipo?
4. ¿Cambiaría su forma actual de gestionar mantenimientos después de ver esto funcionando?
5. ¿Qué le generó dudas durante la demo: el costo, la instalación, o algo del funcionamiento mismo?
6. Con lo que vio, ¿pagaría por este sistema hoy? ¿Qué lo haría decidirse o no?


---


### 6.3.1. Diseño de Entrevistas.

## Segmento: Técnicos

1. Piero Tenorio Medina 
  \
   Enlace: [https://youtu.be/Epc6R8F4hjE ](https://youtu.be/Epc6R8F4hjE )
   Duración: 7:53 min
   Empieza: 00:00
   
*Resumen de la opinión del entrevistado sobre la aplicación Electrolink*

1. **Comprensión general de la plataforma**:

   * Piero comprendió rápidamente el proposito de la aplicación, lo cual es el punto deseado..

2. **Experiencia de usuario (UX)**:

   * Destaca que la interfaz le transmite confianza, lo cual es clave para cualquier plataforma online. Aunque si reforzaria en lo que la organización visual de algunos elementos.

3. **Información clave para el usuario**:

   * Menciona que las secciones para manejar sus componentes electricos son bastante buenas pero considera mejorar la paleta de colores.

4. **Funcionalidad destacada**:

   * Encuentra muy útil la sección para agregar componetes a su inventario..

5. **Sugerencias de mejora**:

   * Sugiere mejorar la organización visual para poder evitar la fatiga al momento de buscar entre secciones.Asimismo, piensa que es muy importante utilizar mostrar la vista de componentes mediante tablas.

---

**Análisis general**

Piero Tenorio Medina refleja el perfil de un **usuario potencialmente interesado** en usar la aplicación. Su retroalimentación valida que:

* El **objetivo principal se comunica con claridad**.
* Hay un **mínimo viable funcional** con buen potencial.
* **La confianza en la plataforma es positiva**, aunque aún hay espacio para mejorar la presentación visual.

Además, sus comentarios demuestran que valora:

* La **usabilidad práctica (disponibilidad, localización)**.
* La **comunicación directa (chat)**.
* Una **presentación atractiva (imágenes e información enriquecida)**.

---

## Segmento: PYMEs

## Segmento: Técnicos

1. Martin Castillo
  \
   Enlace: []()
   Duración: 2:30 min
   Empieza: 00:00
   
*Resumen de la opinión del entrevistado sobre la aplicación Electrolink*


1. **Comprensión general de la plataforma**:

   * El entrevistado comprendió rápidamente la propuesta de valor de ElectroLink y destacó que el sistema permite monitorear el consumo eléctrico y el estado de los circuitos desde un único lugar, facilitando la gestión de sus instalaciones.

2. **Experiencia de usuario (UX)**:

   * Consideró que el **dashboard principal es claro, intuitivo y fácil de utilizar**, ya que presenta la información más relevante para la toma de decisiones. Sin embargo, sugirió realizar ligeras mejoras en el diseño visual para hacer la interfaz aún más atractiva y organizada.

3. **Información clave para el usuario**:

   * Comentó que le gustaría contar con una funcionalidad para registrar las propiedades de sus clientes y disponer de un **dashboard adicional** que le permita visualizar quiénes son sus clientes, dónde se encuentran ubicados y el estado de cada instalación.

4. **Funcionalidad destacada**:

   * Valoró especialmente el dashboard de monitoreo, indicando que la visualización del consumo, las alertas y el estado de los dispositivos le brindan mayor confianza para realizar un mantenimiento preventivo y tomar decisiones oportunas.

5. **Sugerencias de mejora**:

   * Mencionó que estaría dispuesto a pagar por el sistema, siempre que los mecanismos de monitoreo, detección y protección alcancen una confiabilidad cercana al **99 %**. Asimismo, indicó que sería importante conversar sobre los diferentes planes de precios, futuras mejoras y nuevas funcionalidades que podrían incorporarse a la plataforma.

---

## Análisis general

El entrevistado refleja el perfil de una **PYME interesada en adoptar soluciones tecnológicas para la gestión y monitoreo eléctrico**. Su retroalimentación valida que:

* El **dashboard principal aporta valor**, ofreciendo información clara sobre el consumo y el estado de los dispositivos.
* Existe **interés en adoptar la plataforma**, siempre que garantice un alto nivel de confiabilidad en sus mecanismos de monitoreo y protección.
* Hay oportunidades de ampliar la solución incorporando herramientas para la **gestión de clientes y propiedades**, permitiendo administrar múltiples instalaciones desde un solo lugar.

Además, sus comentarios demuestran que valora:

* La **visualización clara de la información mediante dashboards**.
* La **confiabilidad del sistema** para proteger los equipos eléctricos.
* La **escalabilidad de la plataforma**, incorporando nuevos módulos para la gestión de clientes y sedes.
* Un **modelo de precios acorde al valor ofrecido**, acompañado de mejoras continuas y nuevas funcionalidades.


### 6.3.3. Evaluaciones según heurísticas.

Evaluación Heurística de ElectroLink

**Carrera:** Ingeniería de Software  
**Curso:** Aplicaciones Web  
**Auditor:** ElectroLink  
**Plataforma evaluada:** ElectroLink – Plataforma Web  

---

## Tareas evaluadas

- Comprender el propósito del sitio al ingresar  
- Navegar y entender la propuesta de valor tanto para propietarios como para proveedores  
- Visualizar e interactuar con el catálogo de servicios  
- Acceder a testimonios, valores, misión y visión de la empresa  
- Evaluar la visual jerárquica de acciones clave (registrarse, buscar técnicos, mostrar perfil)  
- Interacción de proveedores con sus servicios e inventario  
- Mostrar el trabajo realizado (visibilidad a clientes)  
- Comparar perfiles técnicos y ver reseñas  
- Reportar un servicio finalizado (cliente y proveedor)  
- Evaluar accesibilidad visual e inclusividad  

---

## Tabla resumen de problemas detectados

| #  | Problema detectado                                                                 | Severidad | Heurística/Principio violado                                       |
|----|-------------------------------------------------------------------------------------|-----------|-------------------------------------------------------------------|
| 1  | Falta un botón de regreso rápido al inicio en páginas extensas                     | 2         | Control del usuario                                               |
| 2  | Íconos e imágenes no tienen descripciones accesibles (sin `alt`)                   | 3         | Inclusive Design – Experiencias comparables                      |
| 3  | Jerarquía visual poco clara en botones de acción principal                         | 2         | Visibilidad y jerarquía visual                                    |
| 4  | No hay diferenciación visual clara entre botones de cliente y proveedor            | 2         | Consistencia y estándares                                         |
| 5  | No se explicita claramente el beneficio tangible de publicar un servicio           | 2         | Reconocer en lugar de recordar                                   |
| 6  | En vistas de gestión, no se resalta lo más urgente (como “nuevas solicitudes”)     | 3         | Visibilidad del estado del sistema                               |
| 7  | No hay retroalimentación visual luego de acciones (ej. guardar inventario)         | 3         | Visibilidad del estado del sistema                               |
| 8  | No hay ayudas contextuales (tooltips o descripciones) en íconos de servicios       | 2         | Ayuda y documentación                                             |
| 9  | No se destacan los beneficios diferenciales para PYMEs respecto a clientes comunes | 2         | Reconocer en lugar de recordar / Personalización del contenido    |
| 10 | No se ofrecen opciones de configuración accesibles (contraste, tamaños)            | 3         | Diseño inclusivo                                                  |
| 11 | La opción “Mostrar tu trabajo” no guía claramente cómo se verá al cliente          | 2         | Correspondencia entre sistema y el mundo real                     |
| 12 | El flujo de registro y rol no se valida con confirmación clara al usuario          | 3         | Prevención de errores / Control del usuario                       |

---

## Descripción de problemas clave

### Problema #2: Falta de etiquetas accesibles en íconos e imágenes  
**Severidad:** 3  
**Heurística violada:** Inclusive Design  
**Descripción:** Los íconos que representan funcionalidades como “servicio garantizado”, “componentes”, “perfiles”, etc., no tienen `alt` ni descripciones para lectores de pantalla.  
**Recomendación:** Añadir `alt`, `aria-label` o tooltips en cada ícono o imagen decorativa relevante.

---

### Problema #6: No se resalta lo más urgente para el proveedor  
**Severidad:** 3  
**Heurística violada:** Visibilidad del estado del sistema  
**Descripción:** En el panel del proveedor, las nuevas solicitudes o acciones pendientes no están resaltadas con prioridad visual.  
**Recomendación:** Usar badges, resaltado en rojo o secciones tipo “acciones recientes”.

---

### Problema #7: No hay retroalimentación visual tras acciones clave  
**Severidad:** 3  
**Heurística violada:** Visibilidad del estado del sistema  
**Descripción:** Al guardar componentes, aceptar solicitudes o subir fotos, el usuario no recibe un mensaje inmediato o animación de confirmación.  
**Recomendación:** Mostrar mensajes toast, iconos animados de éxito o loaders donde aplique.

### 6.4. Video About-the-Product.


