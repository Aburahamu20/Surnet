# Informe de Proyecto SurNet

**Instituto:** Duoc UC — Conectividad y Redes  
**Portafolio de Título**  
**Docente:** Juan Pierattini  
**Equipo:** Sebastian Fuentes · Juan Silva · Abraham Castro

---

## 1. Introducción

### 1.1 Contexto y Antecedentes de la Empresa

La empresa se dedica a la comercialización y distribución de productos y útiles de aseo, operando en un mercado con crecimiento sostenido. Como respuesta estratégica a esta expansión, la empresa separó físicamente sus operaciones:

- **Oficina administrativa:** Huérfanos 869, Piso 5, Departamento 502, Santiago (Región Metropolitana)
- **Bodega principal:** Av. Pedro Fontova 7455, Huechuraba (Región Metropolitana)

La distancia física entre ambas ubicaciones hace que la implementación de una solución de conectividad segura sea un requisito fundamental para la continuidad operacional.

### 1.2 Problemática Actual

La separación física y el bajo nivel de digitalización en la bodega generan problemáticas críticas:

- Retrasos en la preparación y despacho de pedidos por dependencia de documentación física
- Errores frecuentes en la selección de productos durante el proceso de picking
- Duplicidad de registros y pérdida o extravío de órdenes en papel
- Demoras en la actualización del inventario, dificultando la validación de existencias en tiempo real
- Escasa trazabilidad del estado de los pedidos
- Falta de coordinación efectiva entre bodega y administración

### 1.3 Justificación del Proyecto

El proyecto propone modernizar y digitalizar la infraestructura tecnológica en ambas sedes. La implementación de una nueva red interconectada mediante **VPN sitio a sitio** permitirá:

- Canal de comunicación seguro, continuo y eficiente entre sedes
- Acceso compartido en tiempo real a sistemas de gestión, bases de datos e inventario
- Digitalización del proceso logístico mediante terminales móviles con lectores de código de barras
- Red inalámbrica Wi-Fi para operaciones de picking en bodega

### 1.4 Objetivos del Proyecto

**Objetivo General:** Diseñar e implementar una infraestructura de red moderna, segura y escalable que interconecte la oficina administrativa y la bodega principal mediante una VPN sitio a sitio, integrando un sistema digital de gestión logística.

**Objetivos Específicos:**
- Implementar la infraestructura de red en la nueva oficina administrativa (cableada e inalámbrica)
- Rediseñar y modernizar la red de la bodega, incorporando cobertura Wi-Fi para terminales móviles
- Establecer una VPN sitio a sitio que garantice comunicación segura y cifrada
- Implementar sistema digital de gestión de pedidos e inventario con terminales de picking
- Mejorar la trazabilidad, precisión y velocidad del proceso logístico

### 1.5 Alcance del Proyecto

Abarca el diseño, planificación e implementación de la infraestructura tecnológica de red para ambas sedes: selección y especificación de equipos activos y pasivos, configuración de VPN sitio a sitio, despliegue de red inalámbrica en bodega e integración con el sistema de gestión logística. **No** contempla el desarrollo de software del sistema de gestión, solo su integración.

### 1.6 Distribución Física de las Sedes

**Sede Bodega — Av. Pedro Fontova 7455, Huechuraba:** Distribución de almacenamiento, zonas de recepción/despacho y pasillos de circulación. Determinó la ubicación de los puntos de acceso Wi-Fi para cobertura continua donde operan los terminales handheld de picking.

**Sede Oficina — Huérfanos 869, Piso 5, Dpto. 502, Santiago:** Disposición de estaciones de trabajo, salas y áreas comunes. Orientó el diseño del cableado estructurado y la segmentación por VLANs según áreas funcionales.

---

## 2. Vinculación con el Perfil de Egreso

El **Técnico en Redes y Telecomunicaciones** de Duoc UC está formado para implementar, mantener y dar soporte a infraestructuras de red LAN/WAN, con aplicación de técnicas de programación en redes y controles de ciberseguridad, en conformidad con normativas y estándares vigentes.

Este proyecto abarca tres competencias del perfil de egreso:
1. Diagnóstico y diseño de redes medianas con soluciones técnicas estructuradas
2. Evaluación y selección de equipos activos según criterios técnicos y presupuestarios
3. Gestión de proyectos con metodologías ágiles (Kanban) y trabajo en equipo

---

## 3. Metodología del Proyecto

### 3.1 Enfoque Metodológico: Cascada + Kanban

Se adoptó una **metodología híbrida** que combina:

- **Cascada (Waterfall):** Planificación estructurada y secuencial con entregables definidos por etapa. Garantiza que cada fase se complete antes de avanzar — fundamental en infraestructura donde errores de diseño comprometería todo el proyecto.
- **Kanban:** Incorporado exclusivamente durante la fase de ejecución como herramienta de gestión visual. Organiza tareas en tablero de estados, identifica cuellos de botella y limita el WIP (Work In Progress).

### 3.2 Fases del Proyecto (Cascada)

#### Fase 1: Análisis
Levantamiento de requerimientos, diagnóstico de infraestructura existente, evaluación de necesidades por sede, definición de objetivos técnicos.

*Entregables:* Documento de requerimientos, informe de diagnóstico, matriz de necesidades por sede.

#### Fase 2: Diseño
Topología de red, direccionamiento IP (VLSM), esquema de VLANs, diseño VPN sitio a sitio, red inalámbrica, selección de equipos, diagramas físicos y lógicos.

*Entregables:* Diagramas físico/lógico, tabla IP, esquema VLANs, diseño VPN, plano Wi-Fi, listado de equipos.

#### Fase 3: Ejecución
Instalación de cableado y equipos activos, configuración de switches/VLANs, puesta en marcha del firewall, establecimiento del túnel VPN, despliegue de APs bajo WLC, integración de terminales handheld. Gestionada con **Kanban**.

*Entregables:* Infraestructura instalada, tablero Kanban con registro de avance, bitácora de implementación.

#### Fase 4: Pruebas
- Validación de conectividad (ping, traceroute, tablas de enrutamiento)
- Pruebas de VPN (cifrado, latencia, accesibilidad inter-sede)
- Pruebas entre VLANs (aislamiento y enrutamiento inter-VLAN)
- Cobertura Wi-Fi (RSSI, SNR, throughput en puntos críticos de bodega)
- Validación de terminales handheld en red inalámbrica

*Entregables:* Informe de pruebas con resultados por subsistema, registro de correcciones.

#### Fase 5: Entrega
Documentación técnica completa, diagramas actualizados, respaldo de configuraciones, manual básico de operación y presentación formal ante evaluadores.

### 3.3 Aplicación de Kanban en la Ejecución

Tablero con 4 estados:

| Estado | Objetivo |
|---|---|
| **Planificación** | Definir, asignar y registrar todas las tareas antes de implementar |
| **Documentación Técnica** | Registrar configuraciones, ajustes e incidencias en paralelo con la ejecución |
| **En progreso** | Tareas activamente en implementación (WIP limitado) |
| **Completado** | Tareas validadas y cerradas |
