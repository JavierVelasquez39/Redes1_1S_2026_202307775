Aquí tienes el contenido del documento convertido a formato Markdown (.md), estructurado y con las citas correspondientes según el texto proporcionado:

# [cite_start]Redes de Computadoras 1 [cite: 1]
## [cite_start]Universidad de San Carlos de Guatemala [cite: 2]
### [cite_start]Facultad de Ingeniería [cite: 3, 6]
[cite_start]**Ingeniería en Ciencias y Sistemas** [cite: 3]

---

[cite_start]**Proyecto 2: Red Nacional de Coordinación SE-CONRED** [cite: 8, 9]
* [cite_start]**Ponderación:** 33 pts [cite: 10]
* [cite_start]**Tiempo estimado:** 35 hrs/min [cite: 11]

---

## [cite_start]1. MARCO FORMATIVO [cite: 16]

### 1.1. [cite_start]Valor [cite: 16]
| Nombre del valor | ¿Cómo se aplica en tu laboratorio? |
| :--- | :--- |
| **Responsabilidad técnica** | [cite_start]Se aplica al diseñar una solución de red coherente y funcional, documentar decisiones, verificar conectividad y mantener configuraciones limpias y consistentes[cite: 17]. |

### 1.2. [cite_start]Competencia(s) [cite: 19]
* [cite_start]**Competencia General:** Capacidad de diseñar, implementar y evaluar redes en capas física, enlace y red del modelo OSI, aplicando cableado, direccionamiento IP, VLANs y enrutamiento[cite: 20].
* [cite_start]**Competencia Específica:** Diseñar e implementar una infraestructura multisede para SE-CONRED, integrando VLANs, VTP, enrutamiento inter-VLAN, VLSM/FLSM, protocolos dinámicos/estáticos, EtherChannel y redundancia[cite: 20].

### 1.3. [cite_start]Objetivo SMART [cite: 22]
| SMART | Definición | Objetivo redactado |
| :--- | :--- | :--- |
| **Específico** | [cite_start]Objetivo concreto y tangible[cite: 23]. | [cite_start]Diseñar, configurar y documentar una infraestructura multisede para SE-CONRED con backbone predefinido y topologías internas para sedes Occidente, Norte, Oriente y Central[cite: 23]. |
| **Medible** | [cite_start]Medida objetiva de éxito[cite: 25]. | [cite_start]Lograr conectividad funcional, propagación de VLANs vía VTP, enrutamiento operativo y pruebas de redundancia[cite: 25]. |
| **Alcanzable / Realista** | [cite_start]Posible con recursos disponibles; contribuye a metas amplias[cite: 25]. | [cite_start]Uso de Cisco Packet Tracer (CLI), subnetting y protocolos de enrutamiento para fortalecer habilidades prácticas en un escenario institucional[cite: 25]. |
| **A Tiempo** | [cite_start]Fecha límite o cronograma[cite: 25]. | [cite_start]Cumplir con implementación y documentación dentro del plazo del cronograma[cite: 25]. |

---

## [cite_start]2. Resumen Ejecutivo [cite: 27]
[cite_start]Este proyecto consiste en el diseño y simulación de una red multisede para la **SE-CONRED** en Guatemala[cite: 28]. [cite_start]La solución se basa en un **backbone predefinido** que interconecta las sedes de Occidente, Norte, Oriente y Central[cite: 30]. [cite_start]Cada estudiante debe diseñar la topología interna, incorporando VLANs, enlaces troncales, VTP y subnetting eficiente[cite: 31]. [cite_start]Se integran tecnologías de enrutamiento dinámico/estático, redundancia y agregación de enlaces para asegurar la continuidad del servicio[cite: 33].

---

## [cite_start]3. Enunciado del Proyecto [cite: 37]

### [cite_start]Objetivos Específicos [cite: 41]
* [cite_start]Creación de una topología de red funcional[cite: 43].
* [cite_start]Dominio de enrutamiento inter-VLAN (Router on a Stick e interfaces virtuales)[cite: 44, 45].
* [cite_start]Aplicación de VLSM y FLSM para distribución de IP[cite: 46].
* [cite_start]Configuración de protocolos de enrutamiento estático y dinámico[cite: 47].
* [cite_start]Configuración de redundancia (EtherChannel, HSRP, VRRP)[cite: 48].

### [cite_start]Backbone Core [cite: 89]
[cite_start]Es la infraestructura principal para interconectar las áreas[cite: 90].
* **Requerimientos:**
    * [cite_start]Al menos dos dispositivos de Capa 3 en el núcleo central[cite: 94].
    * [cite_start]Redundancia de enlace mediante agregación de enlaces entre dispositivos del núcleo[cite: 95, 96].
    * [cite_start]**Segmentos de enrutamiento:** OSPF, EIGRP, RIP y un segmento con rutas estáticas[cite: 99, 100, 101, 102].
    * [cite_start]Mínimo dos puntos de redistribución de rutas[cite: 103].
    * [cite_start]**Medios físicos:** Al menos un enlace de fibra óptica, uno Ethernet y uno serial[cite: 109, 110, 111].

---

## 4. Requerimientos por Sede

### [cite_start]1. Sede Occidente (Logística) [cite: 126]
* [cite_start]**Topología sugerida:** Estrella extendida o árbol jerárquico[cite: 135].
* [cite_start]**VLANs (ID = nY):** Operaciones (1Y, 40 equipos), Administración (2Y, 18 equipos), Seguridad (3Y, 8 equipos), Inventario (4Y, 55 equipos)[cite: 138].
* [cite_start]**Técnico:** Al menos 1 switch principal, aplicación de VTP y enlaces troncales[cite: 141, 143].

### [cite_start]2. Sede Norte (Monitoreo) [cite: 147]
* [cite_start]**Requisito clave:** Redundancia interna y caminos alternos[cite: 153, 156].
* [cite_start]**VLANs (ID = nY):** Operaciones (1Y, 30 equipos), Administración (2Y, 12 equipos), Seguridad (3Y, 10 equipos), Inventario (4Y, 15 equipos)[cite: 159].
* [cite_start]**Técnico:** Implementar **Rapid PVST+** y redundancia interna demostrable[cite: 163, 164].

### [cite_start]3. Sede Oriente (Coordinación) [cite: 170]
* [cite_start]**Requisito clave:** Alta disponibilidad para el gateway[cite: 177].
* [cite_start]**VLANs (ID = nY):** Atención Regional (5Y, 28 equipos), Administración (2Y, 19 equipos), Seguridad (3Y, 8 equipos), Inventario (4Y, 21 equipos)[cite: 180].
* [cite_start]**Técnico:** Uso de dos routers de borde con protocolos **HSRP o VRRP** para redundancia de gateway[cite: 178, 183].

### [cite_start]4. Sede Central (Sede Principal) [cite: 195]
* [cite_start]**Topología sugerida:** Malla parcial o estructura jerárquica redundante[cite: 203].
* [cite_start]**VLANs (ID = nY):** Administración (2Y, 20), Seguridad (3Y, 9), Monitoreo (6Y, 45), Soporte (7Y, 10), Servicios Críticos (8Y, 16)[cite: 205].
* [cite_start]**Técnico:** Redundancia física obligatoria, identificar el **Root Bridge** y documentar el gateway interno[cite: 210, 216, 217].

---

## 5. Reglas Generales de Diseño
* [cite_start]**Cálculo de Y:** $Y$ es el último dígito del carné[cite: 221]. [cite_start]Ejemplo: Carné $20200124 \rightarrow Y=4$[cite: 222].
* [cite_start]**Dispositivos:** Mínimo 7 dispositivos finales por área[cite: 223].
* [cite_start]**VTP:** Obligatorio en áreas con más de un switch[cite: 66].
* [cite_start]**STP:** Implementar Rapid PVST+ donde haya bucles de capa 2[cite: 67].

---

## [cite_start]6. Entregables [cite: 238]
Se entrega un enlace a un **repositorio privado de GitHub** con:
1.  [cite_start]**Documentación Técnica (Markdown):** Capturas de pantalla, justificaciones, extractos de configuración, tablas de subneteo y pruebas de conectividad[cite: 240].
2.  [cite_start]**Archivo .pkt:** Topología funcional de Packet Tracer[cite: 240].
3.  [cite_start]**Acceso:** Agregar al auxiliar correspondiente como colaborador (JosselineM7, PabloR-RC1, RoVas97, CFerSazo7 o sneeg123)[cite: 241, 247, 248, 249, 251, 252].

---

## [cite_start]7. Cronograma [cite: 308]
| Tipo | Fecha Inicio | Fecha Fin |
| :--- | :--- | :--- |
| Asignación | 12/04/2026 | [cite_start]13/04/2026 [cite: 309] |
| Elaboración | 13/04/2026 | [cite_start]29/04/2026 [cite: 309] |
| Calificación | 30/04/2026 | [cite_start]30/04/2026 [cite: 309] |