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

En esta sección se incluyen los SEO Tags y Meta Tags junto con los valores que se asignarán en las principales páginas de la experiencia, tanto a nivel del sitio web (Landing Page) como de la Web Application.

Con el objetivo de mejorar la visibilidad en motores de búsqueda, atraer nuevos usuarios y proporcionar información relevante sobre la landing page y la aplicación web, se incorporarán los siguientes "Meta Tags" como etiquetas HTML en las páginas principales de nuestra plataforma:

<title>ElectroLink - Tu conexión segura a la electricidad</title>

<meta name="description" content="ElectroLink es una plataforma que conecta proveedores de servicios y componentes eléctricos con clientes que requieren asesoramiento o asistencia para mantenimiento en hogares u oficinas.">

<meta name="keywords" content="seguridad, ahorro eléctrico, mantenimiento, asesoramiento">

<meta name="author" content="ElectroLink">


### 4.2.4. Searching Systems

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


### 4.2.5. Navigation Systems

Los sistemas de navegación de ElectroLink han sido diseñados para guiar de forma intuitiva a los usuarios a través del Landing Page y la aplicación, facilitando la exploración del contenido y el acceso a las funcionalidades clave.

La estructura sigue una lógica clara que permite a cada tipo de usuario (hogares, oficinas y proveedores) encontrar rápidamente la información o servicios que necesita mediante menús jerárquicos, enlaces destacados y botones de acción visibles.

![](assets/img/cap5/navigationsystems.png)

## 5.3 Landing Page UI Design.

En esta sección, el equipo de Hampcoders presenta el Diseño de Interfaz de Usuario del sitio web de negocio.

### 5.3.1. Landing Page Wireframe.

#### Sección "Cómo Funciona?" y "Resolvemos Problemas Reales"

![](assets/img/cap5/landingpage-seccion1.png)

#### Sección de "Testimonios" y "Por qué usar Electrolink?"

![](assets/img/cap5/landingpage-seccion2.png)

#### Sección de "Sobre Nosotros"

![](assets/img/cap5/landingpage-seccion3.png)

#### Sección de "Contacto"

![](assets/img/cap5/landingpage-seccion4.png)

### 5.3.2. Landing Page Mock-up.

#### Sección "Cómo Funciona?" y "Resolvemos Problemas Reales"

![](assets/img/cap5/landingpagemockup-1.png)

#### Sección de "Testimonios" y "Por qué usar Electrolink?"

![](assets/img/cap5/landingpagemockup-2.png)


## 5.4 Landing Page UI Design.

### 5.4.1. Applications Wireframes.
### 5.4.2. Applications Wireflow Diagrams.
### 5.4.2. Applications Mock-ups.
### 5.4.3. Applications User Flow Diagrams.

## 5.5. Applications Prototyping.

## 5.6. IoT Device Design.



