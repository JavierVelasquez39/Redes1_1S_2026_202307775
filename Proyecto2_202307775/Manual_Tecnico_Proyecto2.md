# Manual Técnico - Proyecto 2 Redes de Computadoras 1

## Red Nacional de Coordinación SE-CONRED

**Universidad:** Universidad de San Carlos de Guatemala  
**Facultad:** Facultad de Ingeniería  
**Curso:** Redes de Computadoras 1  
**Proyecto:** Proyecto 2  
**Carnet:** 202307775  
**Herramienta:** Cisco Packet Tracer 8.2.2  

---

## 2. Introducción

El presente manual técnico documenta el diseño e implementación de una red multisede para la **SE-CONRED**, integrando segmentación lógica, enrutamiento y mecanismos de redundancia. La solución incorpora VLANs, VTP, trunking 802.1Q, Router-on-a-Stick, OSPF, EIGRP, RIP, rutas estáticas, redistribución de rutas, EtherChannel, HSRP, STP/Rapid PVST+ y subnetting con VLSM. El objetivo es asegurar conectividad total entre sedes y alta disponibilidad en los servicios críticos.

---

## 3. Objetivos

### Objetivo general

Diseñar, implementar y documentar una infraestructura de red multisede funcional para SE-CONRED, garantizando conectividad entre sedes, segmentación lógica, redundancia y disponibilidad.

### Objetivos específicos

- Implementar VLANs por sede.
- Configurar enlaces troncales 802.1Q.
- Implementar VTP en las sedes con más de un switch.
- Configurar enrutamiento inter-VLAN mediante Router-on-a-Stick.
- Implementar protocolos dinámicos OSPF, EIGRP y RIP.
- Implementar un segmento con rutas estáticas.
- Configurar HSRP en Sede Oriente.
- Configurar EtherChannel en el backbone.
- Validar conectividad mediante pruebas de ping.
- Documentar decisiones técnicas, direccionamiento IP y configuraciones relevantes.

---

## 4. Reglas de identificación

```text
Carnet: 202307775
XX = 75
Y = 5
```

Por lo tanto, los IDs de VLAN quedan:

- 1Y = 15
- 2Y = 25
- 3Y = 35
- 4Y = 45
- 5Y = 55
- 6Y = 65
- 7Y = 75
- 8Y = 85

---

## 5. Topología general implementada

![Topología final del proyecto](./Imagenes/BACKBONE.png)

![Construcción de topología - Etapa 1](./Imagenes/Construccion1.png)
![Construcción de topología - Etapa 2](./Imagenes/Construccion2.png)
![Construcción de topología - Etapa 3](./Imagenes/Construccion3.png)

La red se divide en:

- Backbone nacional
- Sede Occidente
- Sede Norte
- Sede Oriente
- Sede Central

Los routers de borde forman el límite entre cada LAN interna y el backbone nacional, permitiendo el intercambio de rutas entre sedes.

---

## 6. Diseño del Backbone

### 6.1 Dispositivos principales

| Dispositivo | Rol                           |
| ----------- | ----------------------------- |
| R-CORE1     | Router principal del backbone |
| R-CORE2     | Router principal del backbone |
| SW-CORE1    | Switch core para EtherChannel |
| SW-CORE2    | Switch core para EtherChannel |
| R-OCCIDENTE | Router de borde Occidente     |
| R-NORTE     | Router de borde Norte         |
| R-ORIENTE1  | Router de borde Oriente 1     |
| R-ORIENTE2  | Router de borde Oriente 2     |
| R-CENTRAL   | Router de borde Central       |

### 6.2 Tecnologías usadas en backbone

- OSPF en el segmento central.
- EIGRP 75 en Occidente.
- RIP v2 en Norte.
- Rutas estáticas en Oriente.
- EtherChannel entre SW-CORE1 y SW-CORE2.
- Redistribución de rutas entre protocolos para interconexión completa.

### 6.3 Direccionamiento del backbone

| Enlace              | Red           | Dispositivo A | IP A       | Dispositivo B | IP B       | Protocolo       |
| ------------------- | ------------- | ------------- | ---------- | ------------- | ---------- | --------------- |
| CORE1 - OCCIDENTE   | 10.75.0.4/30  | R-CORE1       | 10.75.0.5  | R-OCCIDENTE   | 10.75.0.6  | EIGRP           |
| CORE2 - NORTE       | 10.75.0.8/30  | R-CORE2       | 10.75.0.9  | R-NORTE       | 10.75.0.10 | RIP             |
| CORE1 - CENTRAL     | 10.75.0.12/30 | R-CORE1       | 10.75.0.13 | R-CENTRAL     | 10.75.0.14 | OSPF            |
| CORE1 - ORIENTE1    | 10.75.0.16/30 | R-CORE1       | 10.75.0.17 | R-ORIENTE1    | 10.75.0.18 | Estático        |
| CORE2 - ORIENTE2    | 10.75.0.20/30 | R-CORE2       | 10.75.0.21 | R-ORIENTE2    | 10.75.0.22 | Estático        |
| ORIENTE1 - ORIENTE2 | 10.75.0.24/30 | R-ORIENTE1    | 10.75.0.25 | R-ORIENTE2    | 10.75.0.26 | Enlace interno  |
| CORE1 - CORE2       | 10.75.0.32/29 | R-CORE1       | 10.75.0.33 | R-CORE2       | 10.75.0.34 | OSPF / Ethernet |

Si aplica administración en switches core:

- SW-CORE1: 10.75.0.35
- SW-CORE2: 10.75.0.36

### 6.4 Justificación del backbone

Se diseñó un backbone con dos routers principales para cumplir criterios de redundancia, escalabilidad e interconexión multisede. El uso de EtherChannel agrega capacidad de enlace y resiliencia ante fallas físicas. Los distintos protocolos de enrutamiento responden al requisito de dominios diferenciados y la redistribución permite el intercambio de rutas entre todos los segmentos.

### 6.5 Configuraciones relevantes del backbone

![Verificación de backbone - R-CORE1](./Imagenes/R-CORE1Comprobacion.png)
![Verificación de backbone - R-CORE2](./Imagenes/R-CORE2Comprobacion.png)
![Ping backbone - R-CORE1](./Imagenes/R-CORE1Ping.png)
![Ping backbone - R-CORE2](./Imagenes/R-CORE2Ping.png)
![Rutas y conectividad core](./Imagenes/PingsCore1.png)
![Comprobaciones switches core](./Imagenes/SwitchesCoreComprobaciones.png)

#### Configuración relevante - R-CORE1


![Interfaces R-CORE1](./Imagenes/InterfaceCore1.png)
![OSPF R-CORE1](./Imagenes/OSPFR-CORE1.png)

#### Configuración relevante - R-CORE2


![Interfaces R-CORE2](./Imagenes/InterfaceCore2.png)

#### Configuración relevante - R-CENTRAL

![Interfaces R-CENTRAL](./Imagenes/InterfaceCentral.png)

#### Configuración relevante - EIGRP (R-CORE1 / R-OCCIDENTE)


![EIGRP](./Imagenes/EIGRP.png)

#### Configuración relevante - RIP (R-CORE2 / R-NORTE)


![RIP](./Imagenes/RIP.png)

#### Configuración relevante - Rutas estáticas Oriente


![Rutas estáticas](./Imagenes/Static.png)
![Pruebas rutas estáticas](./Imagenes/PingsSTATIC.png)

#### Configuración relevante - Redistribución de rutas


![OSPF](./Imagenes/OSPF.png)

#### Configuración relevante - EtherChannel (SW-CORE1 / SW-CORE2)


![EtherChannel comprobación](./Imagenes/EtherChannelComprobacion.png)
![Interface SW-CORE1](./Imagenes/SW-CORE1Interface.png)

---

## 7. Sede Occidente

### 7.1 Descripción

Occidente representa una sede regional logística. Se implementó una topología tipo estrella extendida/jerárquica con un switch principal y switches de acceso, facilitando la administración centralizada y el crecimiento ordenado.

### 7.2 Topología

![Topología Sede Occidente](./Imagenes/OCCIDENTE.png)

### 7.3 VLANs

| VLAN           | Nombre         | ID | Hosts requeridos | Red             |
| -------------- | -------------- | -: | ---------------: | --------------- |
| Operaciones    | OPERACIONES    | 15 |               40 | 10.75.10.64/26  |
| Administración | ADMINISTRACION | 25 |               18 | 10.75.10.128/27 |
| Seguridad      | SEGURIDAD      | 35 |                8 | 10.75.10.160/28 |
| Inventario     | INVENTARIO     | 45 |               55 | 10.75.10.0/26   |

### 7.4 Subneteo

| VLAN | Red             | Máscara         | Gateway      | Rango utilizable            | Broadcast    |
| ---- | --------------- | --------------- | ------------ | --------------------------- | ------------ |
| 45   | 10.75.10.0/26   | 255.255.255.192 | 10.75.10.1   | 10.75.10.2 - 10.75.10.62    | 10.75.10.63  |
| 15   | 10.75.10.64/26  | 255.255.255.192 | 10.75.10.65  | 10.75.10.66 - 10.75.10.126  | 10.75.10.127 |
| 25   | 10.75.10.128/27 | 255.255.255.224 | 10.75.10.129 | 10.75.10.130 - 10.75.10.158 | 10.75.10.159 |
| 35   | 10.75.10.160/28 | 255.255.255.240 | 10.75.10.161 | 10.75.10.162 - 10.75.10.174 | 10.75.10.175 |

### 7.5 PCs configuradas

| PC | VLAN | IP | Máscara | Gateway |
|---|---:|---|---|---|
| PC-OCC-INV1 | 45 | 10.75.10.2 | 255.255.255.192 | 10.75.10.1 |
| PC-OCC-INV2 | 45 | 10.75.10.3 | 255.255.255.192 | 10.75.10.1 |
| PC-OCC-OP1 | 15 | 10.75.10.66 | 255.255.255.192 | 10.75.10.65 |
| PC-OCC-OP2 | 15 | 10.75.10.67 | 255.255.255.192 | 10.75.10.65 |
| PC-OCC-ADM1 | 25 | 10.75.10.130 | 255.255.255.224 | 10.75.10.129 |
| PC-OCC-ADM2 | 25 | 10.75.10.131 | 255.255.255.224 | 10.75.10.129 |
| PC-OCC-SEG1 | 35 | 10.75.10.162 | 255.255.255.240 | 10.75.10.161 |

![IP Config PC Operaciones](./Imagenes/IPConfigPCOP.png)
![IP Config PC Administración](./Imagenes/IPConfigADM.png)
![IP Config PC Seguridad](./Imagenes/IPConfigPCSEG.png)
![IP Config PC Inventario](./Imagenes/IPConfigPCINV.png)

### 7.6 Configuraciones relevantes

![Interfaces R-OCCIDENTE](./Imagenes/InterfaceOccidente.png)
![Switch core Occidente](./Imagenes/SW-OCC-CORE.png)
![Switch acceso Occidente 1](./Imagenes/SW-OCC-ACC1.png)
![Switch acceso Occidente 3](./Imagenes/SW-OCC-ACC3.png)

```bash
! VTP server en SW-OCC-CORE
! VTP client en switches de acceso
! Trunks
! Puertos access
! Subinterfaces de R-OCCIDENTE
! EIGRP 75
```

### 7.7 Pruebas

![Pruebas de conectividad Occidente](./Imagenes/PingsOccidente.png)

---

## 8. Sede Norte

### 8.1 Descripción

Norte requiere continuidad de servicio y caminos alternos, por lo que se implementó una topología redundante con dos switches de distribución, switches de acceso, Rapid PVST+ y enlaces alternos.

### 8.2 Topología

![Topología Sede Norte](./Imagenes/NORTE.png)

### 8.3 VLANs

| VLAN           | Nombre         | ID | Hosts requeridos | Red            |
| -------------- | -------------- | -: | ---------------: | -------------- |
| Operaciones    | OPERACIONES    | 15 |               30 | 10.75.20.0/27  |
| Administración | ADMINISTRACION | 25 |               12 | 10.75.20.64/28 |
| Seguridad      | SEGURIDAD      | 35 |               10 | 10.75.20.80/28 |
| Inventario     | INVENTARIO     | 45 |               15 | 10.75.20.32/27 |

### 8.4 Subneteo

| VLAN | Red            | Máscara         | Gateway     | Rango utilizable          | Broadcast   |
| ---- | -------------- | --------------- | ----------- | ------------------------- | ----------- |
| 15   | 10.75.20.0/27  | 255.255.255.224 | 10.75.20.1  | 10.75.20.2 - 10.75.20.30  | 10.75.20.31 |
| 45   | 10.75.20.32/27 | 255.255.255.224 | 10.75.20.33 | 10.75.20.34 - 10.75.20.62 | 10.75.20.63 |
| 25   | 10.75.20.64/28 | 255.255.255.240 | 10.75.20.65 | 10.75.20.66 - 10.75.20.78 | 10.75.20.79 |
| 35   | 10.75.20.80/28 | 255.255.255.240 | 10.75.20.81 | 10.75.20.82 - 10.75.20.94 | 10.75.20.95 |

### 8.5 PCs configuradas

| PC          | VLAN | IP          | Máscara         | Gateway     |
| ----------- | ---: | ----------- | --------------- | ----------- |
| PC-NOR-OP1  |   15 | 10.75.20.2  | 255.255.255.224 | 10.75.20.1  |
| PC-NOR-OP2  |   15 | 10.75.20.3  | 255.255.255.224 | 10.75.20.1  |
| PC-NOR-ADM1 |   25 | 10.75.20.66 | 255.255.255.240 | 10.75.20.65 |
| PC-NOR-ADM2 |   25 | 10.75.20.67 | 255.255.255.240 | 10.75.20.65 |
| PC-NOR-SEG1 |   35 | 10.75.20.82 | 255.255.255.240 | 10.75.20.81 |
| PC-NOR-INV1 |   45 | 10.75.20.34 | 255.255.255.224 | 10.75.20.33 |
| PC-NOR-INV2 |   45 | 10.75.20.35 | 255.255.255.224 | 10.75.20.33 |

### 8.6 Configuraciones relevantes

![Interfaces R-NORTE](./Imagenes/InterfaceNorte.png)
![SW-NOR-DIST1](./Imagenes/SW-NOR-DIST1.png)
![SW-NOR-DIST2](./Imagenes/SW-NOR-DIST2.png)
![SW-NOR-ACC2](./Imagenes/SW-NOR-ACC2.png)

```bash
! VTP
! Rapid PVST+
! Root Bridge principal SW-NOR-DIST1
! Trunks
! Router-on-a-Stick en R-NORTE
! RIP v2
```

### 8.7 Pruebas

![Pruebas Norte](./Imagenes/PingsNorte.png)
![Ping adicional Norte](./Imagenes/PingNORTE.png)

---

## 9. Sede Oriente

### 9.1 Descripción

Oriente requiere alta disponibilidad del gateway, por lo que se implementaron dos routers de borde con HSRP. R-ORIENTE1 actúa como router activo y R-ORIENTE2 como respaldo.

### 9.2 Topología

![Topología Sede Oriente](./Imagenes/ORIENTE.png)

### 9.3 VLANs

| VLAN              | Nombre            | ID | Hosts requeridos | Red            |
| ----------------- | ----------------- | -: | ---------------: | -------------- |
| Atención Regional | ATENCION_REGIONAL | 55 |               28 | 10.75.30.0/27  |
| Administración    | ADMINISTRACION    | 25 |               19 | 10.75.30.64/27 |
| Seguridad         | SEGURIDAD         | 35 |                8 | 10.75.30.96/28 |
| Inventario        | INVENTARIO        | 45 |               21 | 10.75.30.32/27 |

### 9.4 Subneteo

| VLAN | Red            | Máscara         | Gateway virtual HSRP | Rango utilizable           | Broadcast    |
| ---- | -------------- | --------------- | -------------------- | -------------------------- | ------------ |
| 55   | 10.75.30.0/27  | 255.255.255.224 | 10.75.30.1           | 10.75.30.2 - 10.75.30.30   | 10.75.30.31  |
| 45   | 10.75.30.32/27 | 255.255.255.224 | 10.75.30.33          | 10.75.30.34 - 10.75.30.62  | 10.75.30.63  |
| 25   | 10.75.30.64/27 | 255.255.255.224 | 10.75.30.65          | 10.75.30.66 - 10.75.30.94  | 10.75.30.95  |
| 35   | 10.75.30.96/28 | 255.255.255.240 | 10.75.30.97          | 10.75.30.98 - 10.75.30.110 | 10.75.30.111 |

### 9.5 HSRP

| VLAN | Grupo HSRP | IP Virtual  | R-ORIENTE1  | R-ORIENTE2  | Activo     |
| ---: | ---------: | ----------- | ----------- | ----------- | ---------- |
|   55 |          1 | 10.75.30.1  | 10.75.30.2  | 10.75.30.3  | R-ORIENTE1 |
|   45 |          4 | 10.75.30.33 | 10.75.30.34 | 10.75.30.35 | R-ORIENTE1 |
|   25 |          2 | 10.75.30.65 | 10.75.30.66 | 10.75.30.67 | R-ORIENTE1 |
|   35 |          3 | 10.75.30.97 | 10.75.30.98 | 10.75.30.99 | R-ORIENTE1 |

### 9.6 PCs configuradas

| PC          | VLAN | IP           | Máscara         | Gateway     |
| ----------- | ---: | ------------ | --------------- | ----------- |
| PC-ORI-AT1  |   55 | 10.75.30.4   | 255.255.255.224 | 10.75.30.1  |
| PC-ORI-AT2  |   55 | 10.75.30.5   | 255.255.255.224 | 10.75.30.1  |
| PC-ORI-ADM1 |   25 | 10.75.30.68  | 255.255.255.224 | 10.75.30.65 |
| PC-ORI-ADM2 |   25 | 10.75.30.69  | 255.255.255.224 | 10.75.30.65 |
| PC-ORI-SEG1 |   35 | 10.75.30.100 | 255.255.255.240 | 10.75.30.97 |
| PC-ORI-INV1 |   45 | 10.75.30.36  | 255.255.255.224 | 10.75.30.33 |
| PC-ORI-INV2 |   45 | 10.75.30.37  | 255.255.255.224 | 10.75.30.33 |

### 9.7 Configuraciones relevantes

![Interfaces R-ORIENTE1](./Imagenes/InterfaceOriente1.png)
![Interfaces R-ORIENTE2](./Imagenes/InterfaceOriente2.png)
![Switch core Oriente](./Imagenes/SWIORICORE.png)
![Switch acceso Oriente 1](./Imagenes/SWORIACC1.png)
![Switch acceso Oriente 2](./Imagenes/SWORIACC2.png)

```bash
! VTP y trunks
! Router-on-a-Stick en R-ORIENTE1 y R-ORIENTE2
! HSRP
! Rutas estáticas por defecto hacia el backbone
! Rutas estáticas desde el CORE hacia 10.75.30.0/24
```

### 9.8 Pruebas

![Pruebas Oriente - Router 1](./Imagenes/PingsOriente1.png)
![Pruebas Oriente - Router 2](./Imagenes/PingsOriente2.png)
![Pruebas PC Oriente](./Imagenes/PingsPCORIAT1.png)
![IP Config PC Oriente Atención](./Imagenes/IPConfigPCORIAT1.png)
![IP Config PC Oriente Inventario](./Imagenes/IPConfigPCORIINV2.png)

Se realizó apagado de R-ORIENTE1 y la conectividad se mantuvo mediante R-ORIENTE2.

---

## 10. Sede Central

### 10.1 Descripción

Central es la sede principal; se implementó una topología jerárquica redundante con dos switches de distribución, switches de acceso, STP y caminos alternos.

### 10.2 Topología

![Topología Sede Central](./Imagenes/CENTRAL.png)

### 10.3 VLANs

| VLAN                | Nombre             | ID | Hosts requeridos | Red             |
| ------------------- | ------------------ | -: | ---------------: | --------------- |
| Administración      | ADMINISTRACION     | 25 |               20 | 10.75.40.64/27  |
| Seguridad           | SEGURIDAD          | 35 |                9 | 10.75.40.144/28 |
| Monitoreo y Control | MONITOREO          | 65 |               45 | 10.75.40.0/26   |
| Soporte             | SOPORTE            | 75 |               10 | 10.75.40.128/28 |
| Servicios Críticos  | SERVICIOS_CRITICOS | 85 |               16 | 10.75.40.96/27  |

### 10.4 Subneteo

| VLAN | Red             | Máscara         | Gateway      | Rango utilizable            | Broadcast    |
| ---- | --------------- | --------------- | ------------ | --------------------------- | ------------ |
| 65   | 10.75.40.0/26   | 255.255.255.192 | 10.75.40.1   | 10.75.40.2 - 10.75.40.62    | 10.75.40.63  |
| 25   | 10.75.40.64/27  | 255.255.255.224 | 10.75.40.65  | 10.75.40.66 - 10.75.40.94   | 10.75.40.95  |
| 85   | 10.75.40.96/27  | 255.255.255.224 | 10.75.40.97  | 10.75.40.98 - 10.75.40.126  | 10.75.40.127 |
| 75   | 10.75.40.128/28 | 255.255.255.240 | 10.75.40.129 | 10.75.40.130 - 10.75.40.142 | 10.75.40.143 |
| 35   | 10.75.40.144/28 | 255.255.255.240 | 10.75.40.145 | 10.75.40.146 - 10.75.40.158 | 10.75.40.159 |

### 10.5 PCs configuradas

| PC          | VLAN | IP           | Máscara         | Gateway      |
| ----------- | ---: | ------------ | --------------- | ------------ |
| PC-CEN-MON1 |   65 | 10.75.40.2   | 255.255.255.192 | 10.75.40.1   |
| PC-CEN-MON2 |   65 | 10.75.40.3   | 255.255.255.192 | 10.75.40.1   |
| PC-CEN-ADM1 |   25 | 10.75.40.66  | 255.255.255.224 | 10.75.40.65  |
| PC-CEN-SEG1 |   35 | 10.75.40.146 | 255.255.255.240 | 10.75.40.145 |
| PC-CEN-SOP1 |   75 | 10.75.40.130 | 255.255.255.240 | 10.75.40.129 |
| PC-CEN-SVC1 |   85 | 10.75.40.98  | 255.255.255.224 | 10.75.40.97  |
| PC-CEN-SVC2 |   85 | 10.75.40.99  | 255.255.255.224 | 10.75.40.97  |

### 10.6 Root Bridge

```text
Root Bridge principal: SW-CEN-DIST1
Root Bridge secundario: SW-CEN-DIST2
Gateway interno de VLANs: R-CENTRAL mediante Router-on-a-Stick
```

### 10.7 Configuraciones relevantes

![SW-CEN-DIST1](./Imagenes/SWCENDIST1.png)
![SW-CEN-DIST2](./Imagenes/SWCENDIST2.png)
![SW-CEN-ACC1](./Imagenes/SWCENACC1.png)
![SW-CEN-ACC2](./Imagenes/SWCENACC2.png)

```bash
! VTP server en SW-CEN-DIST1
! VTP clients
! Trunks
! STP priority 4096 en DIST1
! STP priority 8192 en DIST2
! Router-on-a-Stick en R-CENTRAL
! Ruta por defecto en R-CENTRAL
! Ruta estática en R-CORE1 hacia 10.75.40.0/24
```

### 10.8 Pruebas

![Pruebas Central](./Imagenes/PingsCentral.png)
![Ping PC Central MON1](./Imagenes/PingPCCENMON1.png)
![PC Central SVC2](./Imagenes/PCCENSVC2.png)
![IP Config PC Central MON1](./Imagenes/IPConfigPCCENMON1.png)

Al desconectar el enlace principal de un switch de acceso, STP habilitó el enlace alterno y la conectividad se recuperó.

---

## 11. Tabla global de VLANs

| Sede      | VLAN | Nombre             | Red             | Gateway      |
| --------- | ---: | ------------------ | --------------- | ------------ |
| Occidente |   15 | OPERACIONES        | 10.75.10.64/26  | 10.75.10.65  |
| Occidente |   25 | ADMINISTRACION     | 10.75.10.128/27 | 10.75.10.129 |
| Occidente |   35 | SEGURIDAD          | 10.75.10.160/28 | 10.75.10.161 |
| Occidente |   45 | INVENTARIO         | 10.75.10.0/26   | 10.75.10.1   |
| Norte     |   15 | OPERACIONES        | 10.75.20.0/27   | 10.75.20.1   |
| Norte     |   25 | ADMINISTRACION     | 10.75.20.64/28  | 10.75.20.65  |
| Norte     |   35 | SEGURIDAD          | 10.75.20.80/28  | 10.75.20.81  |
| Norte     |   45 | INVENTARIO         | 10.75.20.32/27  | 10.75.20.33  |
| Oriente   |   55 | ATENCION_REGIONAL  | 10.75.30.0/27   | 10.75.30.1   |
| Oriente   |   25 | ADMINISTRACION     | 10.75.30.64/27  | 10.75.30.65  |
| Oriente   |   35 | SEGURIDAD          | 10.75.30.96/28  | 10.75.30.97  |
| Oriente   |   45 | INVENTARIO         | 10.75.30.32/27  | 10.75.30.33  |
| Central   |   25 | ADMINISTRACION     | 10.75.40.64/27  | 10.75.40.65  |
| Central   |   35 | SEGURIDAD          | 10.75.40.144/28 | 10.75.40.145 |
| Central   |   65 | MONITOREO          | 10.75.40.0/26   | 10.75.40.1   |
| Central   |   75 | SOPORTE            | 10.75.40.128/28 | 10.75.40.129 |
| Central   |   85 | SERVICIOS_CRITICOS | 10.75.40.96/27  | 10.75.40.97  |

---

## 12. Pruebas de conectividad global

### 12.1 Pruebas intra-VLAN

Se validaron pings entre PCs de la misma VLAN para comprobar conectividad dentro del mismo dominio de broadcast.

### 12.2 Pruebas inter-VLAN

Se validaron pings entre VLANs distintas dentro de una misma sede, verificando el funcionamiento del Router-on-a-Stick.

### 12.3 Pruebas entre sedes

Se realizaron pings entre sedes para comprobar el enrutamiento extremo a extremo:

- Occidente → Norte
- Occidente → Central
- Norte → Oriente
- Central → Oriente
- Central → Norte
- Oriente → Occidente

### 12.4 Pruebas de redundancia

- HSRP Oriente: apagado de R-ORIENTE1 y continuidad con R-ORIENTE2.
- STP Central: caída de enlace principal y recuperación por enlace alterno.
- EtherChannel backbone: evidencia de port-channel funcionando.

![Pruebas Backbone - R-CORE1](./Imagenes/R-CORE1Ping.png)
![Pruebas Backbone - R-CORE2](./Imagenes/R-CORE2Ping.png)
![EtherChannel comprobación](./Imagenes/EtherChannelComprobacion.png)

---

## 13. Extractos de configuraciones importantes

![Interfaces R-CORE1](./Imagenes/InterfaceCore1.png)
![Interfaces R-CORE2](./Imagenes/InterfaceCore2.png)
![Interfaces R-NORTE](./Imagenes/InterfaceNorte.png)
![Interfaces R-OCCIDENTE](./Imagenes/InterfaceOccidente.png)
![Interfaces R-ORIENTE1](./Imagenes/InterfaceOriente1.png)
![Interfaces R-ORIENTE2](./Imagenes/InterfaceOriente2.png)
![Interfaces R-CENTRAL](./Imagenes/InterfaceCentral.png)

### 13.1 R-CORE1

```bash
! Interfaces
! OSPF
! EIGRP
! Redistribución
! Rutas estáticas
```

### 13.2 R-CORE2

```bash
! Interfaces
! OSPF
! RIP
! Redistribución
! Rutas estáticas
```

### 13.3 R-OCCIDENTE

```bash
! Subinterfaces
! EIGRP
```

### 13.4 R-NORTE

```bash
! Subinterfaces
! RIP
```

### 13.5 R-ORIENTE1 y R-ORIENTE2

```bash
! Subinterfaces
! HSRP
! Rutas por defecto
```

### 13.6 R-CENTRAL

```bash
! Subinterfaces
! Ruta por defecto
! OSPF
```

### 13.7 Switches principales

```bash
! VTP
! Trunks
! STP
! EtherChannel
```

---

## 14. Justificación técnica de topologías

### Backbone

El backbone con dos routers core y EtherChannel aporta redundancia y mayor capacidad de enlace. El uso de múltiples protocolos y redistribución permite integrar distintos dominios de enrutamiento con alta disponibilidad.

### Occidente

La topología estrella extendida/jerárquica facilita la administración y permite crecimiento ordenado de la sede logística.

### Norte

La redundancia interna con Rapid PVST+ asegura continuidad ante fallas de enlaces en una sede de monitoreo crítico.

### Oriente

HSRP garantiza alta disponibilidad del gateway, evitando interrupciones ante la caída de un router de borde.

### Central

La topología jerárquica redundante/malla parcial con STP y caminos alternos soporta la operación de la sede principal.

---

## 15. Decisiones técnicas tomadas

- Se usó 10.75.0.0/24 para backbone.
- Se usó 10.75.10.0/24 para Occidente.
- Se usó 10.75.20.0/24 para Norte.
- Se usó 10.75.30.0/24 para Oriente.
- Se usó 10.75.40.0/24 para Central.
- Se usó HSRP en Oriente.
- Se usó Router-on-a-Stick para inter-VLAN.
- Se usó EtherChannel en el backbone.
- Se usó STP/Rapid PVST+ para evitar loops en sedes redundantes.
- Se definió SW-CEN-DIST1 como Root Bridge principal de Central.
- Se definió SW-NOR-DIST1 como Root Bridge principal de Norte.

---

## 16. En conclusión...

Se logró conectividad total entre sedes y segmentación por VLAN, aplicando subnetting eficiente. Se validaron mecanismos de redundancia con HSRP y STP, y se implementaron protocolos dinámicos y rutas estáticas. El diseño cumple con los objetivos técnicos del proyecto y garantiza disponibilidad de la red multisede.

---

## 17. Anexos

![Switches core](./Imagenes/SwitchesCoreComprobaciones.png)
![SW-CORE1 Interface](./Imagenes/SW-CORE1Interface.png)
![SW-NOR-DIST1](./Imagenes/SW-NOR-DIST1.png)
![SW-CEN-DIST1](./Imagenes/SWCENDIST1.png)

- Capturas adicionales.
- Comandos completos.
- Evidencias de pruebas.
- Capturas de `show ip route`, `show standby brief`, `show interfaces trunk`, `show etherchannel summary`, `show spanning-tree`.
