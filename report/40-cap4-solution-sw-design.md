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

## 2. Subscription and Payments
Gestión de planes, facturación y control de acceso a funcionalidades.
![](assets/img/cap4/bd-subscription.PNG)

## 3. Profiles and Preferences
Administración de perfiles de usuarios, técnicos y configuración personalizada.
![](assets/img/cap4/bd-profiles.PNG)

## 4. Service Design and Planning
Orquestación de servicios, solicitudes y asignación inteligente de técnicos.
![](assets/img/cap4/service-desing-bd.PNG)

## 5. Service Operation and Monitoring
Ejecución, seguimiento y cierre de servicios con evidencia y evaluación.
![](assets/img/cap4/db-service-operation.PNG)

## 6. Assets and Resource Management
Gestión de propiedades, dispositivos IoT e inventario de técnicos.
![](assets/img/cap4/bd-assets.PNG)

## 7. IoT Monitoring and Edge Processing
Procesamiento de datos en tiempo real y detección de anomalías eléctricas.
![](assets/img/cap4/bd-iot.PNG)

## 8. Analytics
Visualización, reportes e insights a partir de datos históricos y en tiempo real.
![](assets/img/cap4/bd-analytics.PNG)

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

##### 4.1.3.2. Domain Message Flows Modeling.
##### 4.1.3.3. Bounded Context Canvases.
##### 4.1.3.4. Software Architecture Deployment Diagrams.

## 4.2. Tactical-Level Domain-Driven Design

### 4.2.1. Bounded Context: Identity & Access Management
#### 4.2.1.1. Domain Layer.
#### 4.2.1.2. Interface Layer.
#### 4.2.1.3. Application Layer.
#### 4.2.1.4. Infrastructure Layer.
#### 4.2.1.5. Bounded Context Software Architecture Component Level Diagrams.
#### 4.2.1.6. Bounded Context Software Architecture Code Level Diagrams.
##### 4.2.1.6.1. Bounded Context Domain Layer Class Diagrams.
##### 4.2.1.6.2. Bounded Context Database Design Diagram.

### 4.2.2. Bounded Context: Profiles & Preferences Management
#### 4.2.2.1. Domain Layer.
#### 4.2.2.2. Interface Layer.
#### 4.2.2.3. Application Layer.
#### 4.2.2.4. Infrastructure Layer.
#### 4.2.2.5. Bounded Context Software Architecture Component Level Diagrams.
#### 4.2.2.6. Bounded Context Software Architecture Code Level Diagrams.
##### 4.2.2.6.1. Bounded Context Domain Layer Class Diagrams.
##### 4.2.2.6.2. Bounded Context Database Design Diagram.

### 4.2.3. Bounded Context: Assets & Resources Management
#### 4.2.3.1. Domain Layer.
#### 4.2.3.2. Interface Layer.
#### 4.2.3.3. Application Layer.
#### 4.2.3.4. Infrastructure Layer.
#### 4.2.3.5. Bounded Context Software Architecture Component Level Diagrams.
#### 4.2.3.6. Bounded Context Software Architecture Code Level Diagrams.
##### 4.2.3.6.1. Bounded Context Domain Layer Class Diagrams.
##### 4.2.3.6.2. Bounded Context Database Design Diagram.

### 4.2.4. Bounded Context: Service Design & Planning
#### 4.2.4.1. Domain Layer.
#### 4.2.4.2. Interface Layer.
#### 4.2.4.3. Application Layer.
#### 4.2.4.4. Infrastructure Layer.
#### 4.2.4.5. Bounded Context Software Architecture Component Level Diagrams.
#### 4.2.4.6. Bounded Context Software Architecture Code Level Diagrams.
##### 4.2.4.6.1. Bounded Context Domain Layer Class Diagrams.
##### 4.2.4.6.2. Bounded Context Database Design Diagram.

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
