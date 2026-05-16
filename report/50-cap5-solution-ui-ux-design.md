# Capítulo V: Solution UI/UX Design

## 5.1 Style Guidelines.

En esta sección, el equipo establece las bases para contar con un repositorio centralizado y organizado de uso común, que incluye recursos como assets, tipografías, entre otros elementos. El objetivo es mantener una presentación consistente y coherente en toda la propuesta. Asimismo, se incorporan secciones para General Style Guidelines y Web Style Guidelines.

### 5.1.1. General Style Guidelines.

En esta sección se explican las decisiones y referencias visuales relacionadas con conceptos fundamentales como branding, tipografía, colores y espaciado, los cuales sirven como base para la identidad visual del proyecto.

![](assets/img/cap5/styleGuidelinesElectrolink.png)

### 5.1.2. Web, Mobile and IoT Style Guidelines.

En esta sección se explican las decisiones sobre los estándares visuales y de interacción para interfaces web responsivas, así como para interfaces de aplicaciones móviles y de aplicaciones IoT.

Respecto al diseño responsivo, se han definido media queries específicos para distintos breakpoints de la aplicación, siguiendo los estándares establecidos.

| Media Query | Dispositivo       |
|------------|------------------|
| 1024px     | Desktop          |
| 760px      | Tablets          |
| 720px      | Teléfono         |
| 320px      | Teléfono pequeño |

Con estas convenciones nos hemos guiado para determinar en qué momentos se deben realizar cambios significativos en la interfaz.

## 5.2 Information Architecture.

### 5.2.1. Organization Systems.

En esta sección, el equipo define los sistemas de organización de la información que se aplicarán a los distintos grupos de contenido del proyecto, considerando enfoques como la organización jerárquica (visual hierarchy), secuencial (step-by-step) y matricial, según corresponda.

También se establecen los esquemas de categorización de contenido a utilizar.

En el caso de la landing page, se ha utilizado principalmente una organización jerárquica, iniciando con la sección hero y finalizando con la sección de reclutamiento (sin incluir el footer). Esta decisión busca mantener una estructura clara y evitar la sobrecarga de información al usuario, priorizando una navegación más enfocada en el servicio.

![](assets/img/cap5/landingorganization.png)


Para la aplicación web, implementamos un sistema de widgets con estilo matricial para estructurar los datos, lo que permite categorizar la información según nuestros segmentos ya que poseen requerimientos distintos; así, el dueño visualiza métricas de consumo y gestión de inmuebles, mientras que el técnico accede a su agenda y stock de materiales. De esta forma, logramos que la experiencia de usuario sea específica para cada rol, manteniendo también un flujo de suscripción organizado y eficiente.

![](assets/img/cap5/dashboardownerMockup.png)


### 5.2.2. Labeling Systems.

En esta sección se especifican las etiquetas que se utilizarán para representar los distintos conjuntos de información y las relaciones entre ellos. Para el desarrollo de las aplicaciones, se ha optado por el uso de etiquetas simples que faciliten la navegación y la comprensión por parte de los usuarios.

Las etiquetas han sido diseñadas para ser intuitivas y fáciles de recordar, permitiendo que los usuarios encuentren rápidamente la información que necesitan. A continuación, se presentan las etiquetas que se utilizarán en las aplicaciones.

* **Dashboard:** Panel de control organizado mediante un sistema de widgets con estilo matricial, que permite categorizar la información según nuestros segmentos ya que no poseen las mismas necesidades operativas.
* **Services:** Sección destinada a la selección y programación de trabajos técnicos, como instalaciones o mantenimientos.
* **Properties:** Módulo para la gestión y visualización en mapa de los activos e inmuebles registrados.
* **Analytics:** Espacio donde se presentan análisis estadísticos de consumo y métricas de rendimiento.
* **Inventory:** Apartado para el control de existencias y alertas de stock de materiales.
* **Subscription:** Gestión de niveles de cuenta que, bajo un modelo inspirado en Spotify, redirige a la web para transacciones de planes y revisión de facturación.

Respecto a la experiencia de usuario en la aplicación móvil, se ha definido que esta no tenga acceso a realizar pagos, redirigiendo en su lugar al usuario a la versión web para asegurar una gestión centralizada y segura de las suscripciones.

### 5.2.3. SEO Tags and Meta Tags

En esta sección se incluyen los SEO Tags y Meta Tags utilizados en las principales páginas de la plataforma (Landing Page y Web Application).

Con el objetivo de mejorar la visibilidad en motores de búsqueda, atraer nuevos usuarios y proporcionar información relevante sobre la plataforma, se definen los siguientes valores:

- **Title:** ElectroLink - Tu conexión segura a la electricidad  
- **Meta description:** ElectroLink es una plataforma que conecta proveedores de servicios y componentes eléctricos con clientes que requieren asesoramiento o asistencia para mantenimiento en hogares u oficinas.  
- **Meta keywords:** seguridad, ahorro eléctrico, mantenimiento, asesoramiento  
- **Meta author:** ElectroLink  


### 5.2.4. Searching Systems

ElectroLink dispone de un sistema de búsqueda avanzada que facilita a los usuarios la localización de servicios y productos eléctricos de manera rápida y precisa mediante diversos criterios de filtrado:

| Filtro | Descripción |
|--------|-------------|
| Tipo de servicio | Permite filtrar proveedores según el servicio requerido, como instalación, mantenimiento, reparación o auditorías eléctricas. |
| Ubicación | Identifica técnicos o empresas disponibles en una zona geográfica cercana al usuario. |
| Disponibilidad | Filtra profesionales según horarios y fechas disponibles para agendar una atención. |
| Certificación | Muestra únicamente proveedores con certificaciones vigentes en el ámbito eléctrico. |
| Rango de precio | Permite ajustar la búsqueda según el presupuesto del usuario, desde opciones básicas hasta servicios premium. |
| Calificación de proveedores | Prioriza técnicos con mejores valoraciones y reseñas de otros usuarios. |
| Categoría de producto | Facilita la búsqueda de productos eléctricos específicos dentro del catálogo (ej. interruptores, medidores, luminarias). |
| Consumo energético | Permite analizar o filtrar datos de consumo eléctrico por periodos o dispositivos. |
| Planes de suscripción | Compara opciones de suscripción según beneficios como monitoreo, soporte o prioridad en atención. |


### 5.2.5. Navigation Systems

Los sistemas de navegación de ElectroLink han sido diseñados para guiar de forma intuitiva a los usuarios a través del Landing Page y la aplicación, facilitando la exploración del contenido y el acceso a las funcionalidades clave.

La estructura sigue una lógica clara que permite a cada tipo de usuario (hogares, oficinas y proveedores) encontrar rápidamente la información o servicios que necesita mediante menús jerárquicos, enlaces destacados y botones de acción visibles.

![](assets/img/cap5/navigationsystems.png)

## 5.3 Landing Page UI Design.

En esta sección, el equipo de Hampcoders presenta el Diseño de Interfaz de Usuario del sitio web de negocio.

### 5.3.1. Landing Page Wireframe.

#### Sección "Cómo Funciona?" y "Resolvemos Problemas Reales" 


![](assets/img/cap5/landingpage/landingpage-seccion1.png)


#### Sección de "Testimonios" y "Por qué usar Electrolink?"


![](assets/img/cap5/landingpage/landingpage-seccion2.png)


#### Sección de "Sobre Nosotros"


![](assets/img/cap5/landingpage/landingpage-seccion3.png)


#### Sección de "Contacto"


![](assets/img/cap5/landingpage/landingpage-seccion4.png)


### 5.3.2. Landing Page Mock-up.

#### Sección "Cómo Funciona?" y "Resolvemos Problemas Reales"


![](assets/img/cap5/landingpage/landingpagemockup-1.png)


#### Sección de "Testimonios" y "Por qué usar Electrolink?"


![](assets/img/cap5/landingpage/landingpagemockup-2.png)



## 5.4 Applications UX/UI Design.

En esta sección, el equipo de Hampcoders presenta el Diseño de Interfaz de Usuario de la aplicación Front-End


### 5.4.1. Applications Wireframes.

Se muestran los wireframes de ElectroLink, los cuales representan la estructura general de la interfaz y la disposición de sus elementos. Estos permiten definir una base clara de navegación y organización antes del diseño visual final, asegurando una experiencia de usuario intuitiva, funcional y accesible.

Los wireframes contemplan distintos perfiles de usuario, incluyendo propietarios, técnicos y proveedores, así como usuarios con necesidades de accesibilidad (TEA, ansiedad social o discapacidades físicas).

#### Elementos principales del diseño

**Arquitectura de la información:**  
Organización de funciones como historial de servicios, consumo energético, gestión de dispositivos y solicitudes de mantenimiento.

**Estructura de la interfaz:**  
Distribución estratégica de botones, menús y tarjetas para facilitar acciones como contacto con proveedores, edición de dispositivos y visualización de métricas.

**Pantallas clave:**  
- Dashboard de usuario  
- Panel de proveedores  
- Formularios de mantenimiento  
- Perfil de usuario  

**Principios de diseño:**  
- Simplicidad para reducir la carga cognitiva  
- Consistencia en estilos y componentes  
- Accesibilidad con opciones como texto ajustable, alto contraste, navegación por teclado y diseño responsive  

Estos wireframes sirven como guía para el desarrollo del frontend, asegurando una plataforma ordenada, usable e inclusiva.

#### Vista de inicio de sesión


![](assets/img/cap5/wireframes/signInWireframe.png)


#### Vista de Crear Cuenta


![](assets/img/cap5/wireframes/signUpWireframe.png)


#### Vista de Dashboard Dueño de Hogar


![](assets/img/cap5/wireframes/homeOwnerDashboardWireframe.png)


#### Vista de editar perfil


![](assets/img/cap5/wireframes/HomeOwnerProfileWireframe.png)


#### Vista la sección de propiedades


![](assets/img/cap5/wireframes/propertyWireframe.png)



### 5.4.2. Applications Wireflow Diagrams.

En esta sección, el equipo de Hampcoders define los wireflows diagrams para la aplicación Web

#### Wireflow para el Usuario se registre dentro de la plataforma


![](assets/img/cap5/wireframes/wireflowregistro.png)


#### Wireflow para el usuario busque tecnicos


![](assets/img/cap5/wireframes/wireflowlocalizacion.png)


#### Wireflow para añadir propiedades


![](assets/img/cap5/wireframes/wireflowpropiedad.png)


[https://lucid.app/lucidchart/30f4fbe6-f0ef-44da-88e3-46020aca7a0f/edit?view_items=AAr.UGv3QC3F&page=0_0&invitationId=inv_420b895d-1a79-4a72-baab-d4f42e8e3f6f](https://lucid.app/lucidchart/30f4fbe6-f0ef-44da-88e3-46020aca7a0f/edit?view_items=AAr.UGv3QC3F&page=0_0&invitationId=inv_420b895d-1a79-4a72-baab-d4f42e8e3f6f)

### 5.4.2. Applications Mock-ups.

#### Vista de Inicio de Sesión


![](assets/img/cap5/mockups/SignInMockup.png)


#### Vista de Registro


![](assets/img/cap5/mockups/signUpMockup.png)


#### Vista de Dashboard de Técnico


![](assets/img/cap5/mockups/dasboardtechnician.png)


#### Vista de Dashboard de Dueño de hogar


![](assets/img/cap5/mockups/dashboardowner.png)


#### Vista de Portafolio de propiedades


![](assets/img/cap5/mockups/propertyPortfolioMockup.png)


#### Vista de Solicitud de servicio


![](assets/img/cap5/mockups/servicerequestMockup.png)


#### Vista de Suscripciones


![](assets/img/cap5/mockups/subscriptionsMockup.png)


### 5.4.3. Applications User Flow Diagrams.

En esta sección se presenta la propuesta de User Flows del sistema ElectroLink, los cuales representan las distintas rutas de interacción que los usuarios realizan dentro de la plataforma.

Cada flujo ha sido diseñado a partir de un objetivo específico del usuario (User Goal), considerando los perfiles de User Persona definidos en el alcance del proyecto.

#### User Flow diagrams para el Usuario se registre dentro de la plataforma


![](assets/img/cap5/userflows/userflow.png)


#### User Flow diagrams para el Usuario acceda desde la pagina principal al inventario


![](assets/img/cap5/userflows/userflow2.png)


## 5.5. Applications Prototyping.

Esta sección presenta los prototipos de interfaz de usuario, que incluyen simulaciones de interacción y navegación dentro del sistema. Las decisiones de diseño de interacción se basan en criterios fundamentales como la facilidad de uso, la accesibilidad y la adaptación a distintos dispositivos.

![](assets/img/cap5/prototype.png)


[https://www.figma.com/proto/gtumIjnhLJ1rDqlntXsJJS/Electrolink-EXP?node-id=4-5&t=VaQk4h0RNODs3pVx-1&scaling=min-zoom&content-scaling=fixed&page-id=0%3A1&starting-point-node-id=4%3A5](https://www.figma.com/proto/gtumIjnhLJ1rDqlntXsJJS/Electrolink-EXP?node-id=4-5&t=VaQk4h0RNODs3pVx-1&scaling=min-zoom&content-scaling=fixed&page-id=0%3A1&starting-point-node-id=4%3A5)


## 5.6. IoT Device Design.



