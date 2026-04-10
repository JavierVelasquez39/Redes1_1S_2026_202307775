# Manual Técnico — Práctica 2  
## Red del Aeropuerto Internacional La Aurora

**Curso:** Redes de Computadoras 1  
**Universidad:** Universidad de San Carlos de Guatemala  
**Proyecto:** Diseño e implementación de una red LAN segmentada con VLANs para distintas áreas operativas del Aeropuerto Internacional La Aurora  
**Carné:** 202307775  
**Estudiante:** Javier Andrés Velásquez Bonilla  
**Software utilizado:** Cisco Packet Tracer 8.2.2  

---

# 1. Introducción

El presente manual técnico documenta el diseño, implementación y validación de una topología de red LAN segmentada con VLANs para distintas áreas operativas del Aeropuerto Internacional La Aurora. El objetivo del proyecto fue construir una red organizada, con separación lógica por áreas, uso de VLAN nativa, VLAN de seguridad, redundancia de enlaces, EtherChannel y Rapid PVST+, cumpliendo con los lineamientos establecidos en el enunciado de la práctica.

La solución implementada fue diseñada con un enfoque jerárquico, utilizando una capa central de conmutación (`CORE1` y `CORE2`), switches principales por área y switches de acceso donde fue necesario por densidad de hosts. Se trabajó con segmentación por VLAN, propagación centralizada de VLANs mediante VTP, enlaces troncales, agregación de enlaces con LACP y puertos no utilizados protegidos mediante VLAN Blackhole 999.

---

# 2. Objetivos del laboratorio

## 2.1 Objetivo general

Diseñar e implementar una red LAN segmentada para el Aeropuerto Internacional La Aurora, utilizando switches Cisco 2960 y configuraciones de VLAN, VTP, STP, EtherChannel y direccionamiento IPv4, de forma organizada, funcional y documentada.

## 2.2 Objetivos específicos

- Identificar 8 áreas operativas del aeropuerto y asignar una VLAN a cada una.
- Crear una VLAN nativa 99 para tráfico troncal entre switches.
- Crear una VLAN 999 de tipo Blackhole para puertos no utilizados.
- Implementar VTP con dominio asociado al carné estudiantil.
- Implementar Rapid PVST+ para evitar bucles de capa 2.
- Configurar EtherChannel con LACP en enlaces críticos.
- Realizar subnetting eficiente por VLAN.
- Asignar direcciones IP a todas las computadoras utilizadas en la simulación.
- Verificar conectividad dentro de la misma VLAN y aislamiento entre VLANs.

---

# 3. Alcance de la solución implementada

La solución implementada contempla:

- 8 áreas del aeropuerto, cada una con una VLAN propia.
- 2 switches centrales (`CORE1` y `CORE2`).
- 8 switches principales de área.
- 4 switches de acceso adicionales para áreas de mayor tamaño.
- 20 PCs distribuidas entre las 8 áreas para pruebas funcionales.
- EtherChannel LACP en enlaces críticos.
- Troncales con VLAN nativa 99.
- VLAN 999 para puertos no utilizados y apagados.
- Rapid PVST+ con root bridge por VLAN en el switch principal de cada área.

---

# 4. Áreas seleccionadas del aeropuerto

Las 8 áreas seleccionadas para representar la operación del aeropuerto son las siguientes:

| Área | Nombre del área | VLAN |
|---|---|---:|
| Área 1 | Check-in / Pasajeros | 15 |
| Área 2 | Puertas de Embarque | 25 |
| Área 3 | Administración | 35 |
| Área 4 | Manejo de Equipaje | 45 |
| Área 5 | Operaciones de Pista | 55 |
| Área 6 | Seguridad / CCTV | 65 |
| Área 7 | Mantenimiento | 75 |
| Área 8 | Red de Invitados | 85 |

La numeración de las VLANs se determinó usando el último dígito del carné (`5`), siguiendo la regla del enunciado: `1X`, `2X`, `3X`, ..., `8X`.

---

# 5. Descripción de la topología

## 5.1 Estructura general

La topología fue diseñada en tres niveles lógicos:

- **Core Layer:** `CORE1`, `CORE2`
- **Distribution Layer:** `SW-A1` a `SW-A8`
- **Access Layer:** `SW-A1-ACC1`, `SW-A1-ACC2`, `SW-A3-ACC1`, `SW-A6-ACC1`

## 5.2 Equipos utilizados

### Switches de núcleo
- `CORE1`
- `CORE2`

### Switches principales
- `SW-A1`
- `SW-A2`
- `SW-A3`
- `SW-A4`
- `SW-A5`
- `SW-A6`
- `SW-A7`
- `SW-A8`

### Switches de acceso
- `SW-A1-ACC1`
- `SW-A1-ACC2`
- `SW-A3-ACC1`
- `SW-A6-ACC1`

### Hosts
- 20 PCs distribuidas por área

## 5.3 Capturas de la topología

### 5.3.1 Construcción de la topología (secuencia)

![Construcción 1](Imagenes/Construccion1.png)
![Construcción 2](Imagenes/Construccion2.png)
![Construcción 3](Imagenes/Construccion3.png)
![Construcción 4](Imagenes/Construccion4.png)
![Construcción 5](Imagenes/Construccion5.png)
![Construcción 6](Imagenes/Construccion6.png)
![Construcción 7](Imagenes/Construccion7.png)
![Construcción 8](Imagenes/Construccion8.png)
![Construcción 9](Imagenes/Construccion9.png)
![Construcción 10](Imagenes/Construccion10.png)
![Construcción 11](Imagenes/Construccion11.png)
![Construcción 12](Imagenes/Construccion12.png)
![Construcción 13](Imagenes/Construccion13.png)
![Construcción 14](Imagenes/Construccion14.png)
![Construcción 15](Imagenes/Construccion15.png)


---

# 6. Justificación del diseño

## 6.1 Razón de la forma de interconexión

Se eligió una topología jerárquica porque facilita:

- administración centralizada,
- segmentación lógica por áreas,
- crecimiento ordenado,
- redundancia controlada,
- mejor documentación y comprensión visual.

## 6.2 Alta disponibilidad

Las áreas consideradas más críticas en la operación del aeropuerto fueron:

- Área 1: Check-in / Pasajeros
- Área 6: Seguridad / CCTV

Estas áreas poseen enlaces redundantes hacia la capa central, y además cuentan con EtherChannel en enlaces principales para incrementar disponibilidad y ancho de banda. Esto responde a la exigencia de alta disponibilidad planteada por el enunciado.

## 6.3 Dominios de colisión

Cada puerto switch constituye un dominio de colisión independiente. Dado que la topología usa switches en toda la red, se minimizan colisiones y se delimita claramente el tráfico de cada dispositivo conectado.

La segmentación por switches y puertos access permite que:

- cada PC opere en su propio dominio de colisión,
- cada área esté lógicamente separada por VLAN,
- los enlaces troncales y EtherChannels transporten múltiples VLANs sin mezclar los dominios de broadcast de usuario.

## 6.4 Carga de tráfico

Las áreas con mayor carga esperada son:

- Check-in / Pasajeros
- Seguridad / CCTV
- Administración

Por esa razón se utilizaron switches de acceso adicionales en áreas con más densidad o importancia, y EtherChannel en enlaces críticos, evitando cuellos de botella.

## 6.5 Tipo de cableado

Se utilizaron:

- **Copper Cross-Over** para enlaces switch a switch
- **Copper Straight-Through** para enlaces PC a switch

Esto responde a prácticas típicas de capa 2 y a lo solicitado en la rúbrica sobre uso correcto del cableado.

---

# 7. Tabla de VLANs por área

| Área | VLAN | Nombre | Hosts requeridos por enunciado |
|---|---:|---|---:|
| Área 1 | 15 | AREA1 | 74 |
| Área 2 | 25 | AREA2 | 25 |
| Área 3 | 35 | AREA3 | 32 |
| Área 4 | 45 | AREA4 | 5 |
| Área 5 | 55 | AREA5 | 26 |
| Área 6 | 65 | AREA6 | 47 |
| Área 7 | 75 | AREA7 | 8 |
| Área 8 | 85 | AREA8 | 20 |
| Nativa | 99 | NATIVA | N/A |
| Blackhole | 999 | BLACKHOLE | N/A |

Los requerimientos de hosts corresponden a la tabla del enunciado.

---

# 8. Tabla de subnetting organizada

Se empleó **VLSM** para asignar una subred eficiente a cada VLAN. La distribución propuesta aprovecha mejor el espacio IPv4 y mantiene subredes contiguas.

## 8.1 Tabla de subnetting por VLAN

| Área | VLAN | Hosts requeridos | Prefijo | Máscara | Red | Gateway | Rango útil | Broadcast |
|---|---:|---:|---|---|---|---|---|---|
| Área 1 | 15 | 74 | /25 | 255.255.255.128 | 192.168.75.0 | 192.168.75.1 | 192.168.75.1 - 192.168.75.126 | 192.168.75.127 |
| Área 6 | 65 | 47 | /26 | 255.255.255.192 | 192.168.75.128 | 192.168.75.129 | 192.168.75.129 - 192.168.75.190 | 192.168.75.191 |
| Área 3 | 35 | 32 | /26 | 255.255.255.192 | 192.168.75.192 | 192.168.75.193 | 192.168.75.193 - 192.168.75.254 | 192.168.75.255 |
| Área 5 | 55 | 26 | /27 | 255.255.255.224 | 192.168.76.0 | 192.168.76.1 | 192.168.76.1 - 192.168.76.30 | 192.168.76.31 |
| Área 2 | 25 | 25 | /27 | 255.255.255.224 | 192.168.76.32 | 192.168.76.33 | 192.168.76.33 - 192.168.76.62 | 192.168.76.63 |
| Área 8 | 85 | 20 | /27 | 255.255.255.224 | 192.168.76.64 | 192.168.76.65 | 192.168.76.65 - 192.168.76.94 | 192.168.76.95 |
| Área 7 | 75 | 8 | /28 | 255.255.255.240 | 192.168.76.96 | 192.168.76.97 | 192.168.76.97 - 192.168.76.110 | 192.168.76.111 |
| Área 4 | 45 | 5 | /29 | 255.255.255.248 | 192.168.76.112 | 192.168.76.113 | 192.168.76.113 - 192.168.76.118 | 192.168.76.119 |

## 8.2 Justificación del subnetting

Se utilizó VLSM debido a que las áreas no requieren la misma cantidad de hosts. Esta técnica permite:

- reducir desperdicio de direcciones IP,
- asignar tamaños de red acordes a la necesidad real,
- mantener orden lógico en la documentación,
- facilitar la planificación de crecimiento.

---

# 9. Tabla completa de asignación de direcciones IP

## 9.1 Hosts de la simulación

| PC | Área | VLAN | IP | Máscara | Gateway |
|---|---|---:|---|---|---|
| PC1-A1 | Área 1 | 15 | 192.168.75.10 | 255.255.255.128 | 192.168.75.1 |
| PC2-A1 | Área 1 | 15 | 192.168.75.11 | 255.255.255.128 | 192.168.75.1 |
| PC3-A1 | Área 1 | 15 | 192.168.75.12 | 255.255.255.128 | 192.168.75.1 |
| PC4-A1 | Área 1 | 15 | 192.168.75.13 | 255.255.255.128 | 192.168.75.1 |
| PC1-A2 | Área 2 | 25 | 192.168.76.34 | 255.255.255.224 | 192.168.76.33 |
| PC2-A2 | Área 2 | 25 | 192.168.76.35 | 255.255.255.224 | 192.168.76.33 |
| PC1-A3 | Área 3 | 35 | 192.168.75.194 | 255.255.255.192 | 192.168.75.193 |
| PC2-A3 | Área 3 | 35 | 192.168.75.195 | 255.255.255.192 | 192.168.75.193 |
| PC3-A3 | Área 3 | 35 | 192.168.75.196 | 255.255.255.192 | 192.168.75.193 |
| PC1-A4 | Área 4 | 45 | 192.168.76.114 | 255.255.255.248 | 192.168.76.113 |
| PC2-A4 | Área 4 | 45 | 192.168.76.115 | 255.255.255.248 | 192.168.76.113 |
| PC1-A5 | Área 5 | 55 | 192.168.76.2 | 255.255.255.224 | 192.168.76.1 |
| PC2-A5 | Área 5 | 55 | 192.168.76.3 | 255.255.255.224 | 192.168.76.1 |
| PC1-A6 | Área 6 | 65 | 192.168.75.130 | 255.255.255.192 | 192.168.75.129 |
| PC2-A6 | Área 6 | 65 | 192.168.75.131 | 255.255.255.192 | 192.168.75.129 |
| PC3-A6 | Área 6 | 65 | 192.168.75.132 | 255.255.255.192 | 192.168.75.129 |
| PC1-A7 | Área 7 | 75 | 192.168.76.98 | 255.255.255.240 | 192.168.76.97 |
| PC2-A7 | Área 7 | 75 | 192.168.76.99 | 255.255.255.240 | 192.168.76.97 |
| PC1-A8 | Área 8 | 85 | 192.168.76.66 | 255.255.255.224 | 192.168.76.65 |
| PC2-A8 | Área 8 | 85 | 192.168.76.67 | 255.255.255.224 | 192.168.76.65 |


- Área 1

    ![Área 1](Imagenes/PCA1.png)
- Área 2

    ![Área 2](Imagenes/PCA2.png)
- Área 3

    ![Área 3](Imagenes/PCA3.png)
- Área 4

    ![Área 4](Imagenes/PCA4.png)
- Área 5

    ![Área 5](Imagenes/PCA5.png)
- Área 6

    ![Área 6](Imagenes/PCA6.png)
- Área 7

    ![Área 7](Imagenes/PCA7.png)
- Área 8

    ![Área 8](Imagenes/PCA8.png)

---

# 10. Detalle de la topología física implementada

## 10.1 Enlaces entre switches

### Core a Core
- `CORE1 Fa0/1` ↔ `CORE2 Fa0/1`
- `CORE1 Fa0/2` ↔ `CORE2 Fa0/2`
- `CORE1 Fa0/3` ↔ `CORE2 Fa0/3`

### Área 1
- `CORE1 Fa0/4` ↔ `SW-A1 Fa0/1`
- `CORE1 Fa0/5` ↔ `SW-A1 Fa0/2`
- `CORE1 Fa0/6` ↔ `SW-A1 Fa0/3`
- `CORE2 Fa0/4` ↔ `SW-A1 Fa0/4`
- `SW-A1 Fa0/5` ↔ `SW-A1-ACC1 Fa0/1`
- `SW-A1 Fa0/6` ↔ `SW-A1-ACC2 Fa0/1`

### Área 2
- `CORE1 Fa0/7` ↔ `SW-A2 Fa0/1`
- `CORE2 Fa0/5` ↔ `SW-A2 Fa0/2`

### Área 3
- `CORE1 Fa0/8` ↔ `SW-A3 Fa0/1`
- `SW-A3 Fa0/2` ↔ `SW-A3-ACC1 Fa0/1`

### Área 4
- `CORE1 Fa0/9` ↔ `SW-A4 Fa0/1`
- `CORE2 Fa0/6` ↔ `SW-A4 Fa0/2`

### Área 5
- `CORE1 Fa0/10` ↔ `SW-A5 Fa0/1`

### Área 6
- `CORE2 Fa0/7` ↔ `SW-A6 Fa0/1`
- `CORE2 Fa0/8` ↔ `SW-A6 Fa0/2`
- `CORE2 Fa0/9` ↔ `SW-A6 Fa0/3`
- `CORE1 Fa0/11` ↔ `SW-A6 Fa0/4`
- `SW-A6 Fa0/5` ↔ `SW-A6-ACC1 Fa0/1`

### Área 7
- `CORE2 Fa0/10` ↔ `SW-A7 Fa0/1`

### Área 8
- `CORE2 Fa0/11` ↔ `SW-A8 Fa0/1`

---

# 11. Configuración base aplicada a todos los switches

La configuración base utilizada en todos los switches fue la siguiente, cambiando únicamente el nombre del equipo:

```plaintext
enable
configure terminal

hostname NOMBRE_DEL_SWITCH

no ip domain-lookup

enable secret 202307775

line console 0
password 202307775
login
exit

line vty 0 4
password 202307775
login
exit

banner motd #Acceso restringido - Red Aeropuerto#

end
write memory
```

---

# 12. Configuración de VTP

> **Nota importante:** La implementación realizada en Packet Tracer utilizó un esquema VTP centralizado con `CORE1` como servidor y el resto de switches como clientes. Este enfoque fue adoptado por simplicidad operativa en la simulación.

## 12.1 CORE1 como VTP Server

```plaintext
enable
configure terminal

vtp mode server
vtp domain 202307775
vtp password area1

end
write memory
```

**Captura de VTP Server (CORE1):**

![VTP Server CORE1](Imagenes/Core1VTPServer.png)

## 12.2 Resto de switches como VTP Client

```plaintext
enable
configure terminal

vtp mode client
vtp domain 202307775
vtp password area1

end
write memory
```

**Capturas de VTP Client (muestras por capa):**

- Core 2

    ![VTP CORE2](Imagenes/CORE2VTP.png)
- Distribución

    ![VTP SW-A1](Imagenes/SWA1VTP.png)
    ![VTP SW-A2](Imagenes/SWA2VTP.png)
    ![VTP SW-A3](Imagenes/SWA3VTP.png)
    ![VTP SW-A4](Imagenes/SWA4VTP.png)
    ![VTP SW-A5](Imagenes/SWA5VTP.png)
    ![VTP SW-A6](Imagenes/SWA6VTP.png)
    ![VTP SW-A7](Imagenes/SWA7VTP.png)
    ![VTP SW-A8](Imagenes/SWA8VTP.png)

- Acceso

    ![VTP SW-A1-ACC1](Imagenes/SWA1ACC1VTP.png)
    ![VTP SW-A1-ACC2](Imagenes/SWA1ACC2VTP.png)
    ![VTP SW-A3-ACC1](Imagenes/SWA3ACC1VTP.png)
    ![VTP SW-A6-ACC1](Imagenes/SWA6ACC1VTP.png)

---

# 13. Configuración de VLANs

Las VLANs fueron creadas en `CORE1` y propagadas a la red mediante VTP.

```plaintext
enable
configure terminal

vlan 15
name AREA1
exit

vlan 25
name AREA2
exit

vlan 35
name AREA3
exit

vlan 45
name AREA4
exit

vlan 55
name AREA5
exit

vlan 65
name AREA6
exit

vlan 75
name AREA7
exit

vlan 85
name AREA8
exit

vlan 99
name NATIVA
exit

vlan 999
name BLACKHOLE
exit

end
write memory
```

**Capturas de VLANs configuradas:**

- VLANs en CORE1

    ![VLANs CORE1](Imagenes/VLANCore1.png)

- VLAN 15 (Área 1)

    ![VLAN 15](Imagenes/VLAN15.png)

- VLAN 45 (Área 4)

    ![VLAN 45](Imagenes/VLAN45.png)

- VLAN 65 (Área 6)

    ![VLAN 65](Imagenes/VLAN65.png)

- VLAN 999 (Blackhole)

    ![VLAN 999 - 1](Imagenes/VLAN9991.png)
    ![VLAN 999 - 2](Imagenes/VLAN9992.png)

---

# 14. Configuración de puertos troncales

Los enlaces entre switches fueron configurados como troncales, y posteriormente se estableció la VLAN nativa 99.

## 14.1 Trunks

En la primera fase se configuraron puertos troncales entre switches.

Ejemplo de configuración general:

```plaintext
enable
configure terminal
interface range fa0/1 - 24
switchport mode trunk
end
write memory
```

**Capturas de enlaces troncales (muestras por capa):**

- Core

    ![Trunk CORE1](Imagenes/CORE1Trunk.png)
    ![Trunk CORE2](Imagenes/CORE2Trunk.png)
- Distribución

    ![Trunk SW-A1](Imagenes/SWA1Trunk.png)
    ![Trunk SW-A2](Imagenes/SWA2Trunk.png)
    ![Trunk SW-A3](Imagenes/SWA3Trunk.png)
    ![Trunk SW-A4](Imagenes/SWA4Trunk.png)
    ![Trunk SW-A5](Imagenes/SWA5Trunk.png)
    ![Trunk SW-A6](Imagenes/SWA6Trunk.png)
    ![Trunk SW-A7](Imagenes/SWA7Trunk.png)
    ![Trunk SW-A8](Imagenes/SWA8Trunk.png)
- Acceso

    ![Trunk SW-A1-ACC1](Imagenes/SWA1ACC1Trunk.png)
    ![Trunk SW-A1-ACC2](Imagenes/SWA1ACC2Trunk.png)
    ![Trunk SW-A3-ACC1](Imagenes/SWA3ACC1Trunk.png)
    ![Trunk SW-A6-ACC1](Imagenes/SWA6ACC1Trunk.png)

## 14.2 VLAN nativa 99

Ejemplo aplicado a enlaces troncales:

```plaintext
enable
configure terminal
interface range fa0/1 - 11
switchport trunk native vlan 99
end
write memory
```

**Capturas de VLAN nativa 99 aplicada:**

![VLAN 99 CORE1](Imagenes/CORE1VLAN99.png)
![VLAN 99 CORE2](Imagenes/CORE2VLAN99.png)
![VLAN 99 SW-A1-ACC2](Imagenes/SWA1ACC2VLAN99.png)
![VLAN 99 SW-A4](Imagenes/SWA4VLAN99.png)
![VLAN 99 SW-A7](Imagenes/SWA7VLAN99.png)

En los switches de acceso y distribución, se aplicó el mismo criterio únicamente a los puertos utilizados como uplinks.

---

# 15. Configuración de EtherChannel con LACP

## 15.1 EtherChannel entre CORE1 y CORE2

### CORE1

```plaintext
enable
configure terminal
interface range fa0/1 - 3
channel-group 1 mode active
end
write memory

configure terminal
interface port-channel 1
switchport mode trunk
switchport trunk native vlan 99
end
write memory
```

**Capturas de EtherChannel CORE1/CORE2:**

![EtherChannel CORE1](Imagenes/EthernetChannelCORE1.png)
![EtherChannel CORE2](Imagenes/EthernetChannelCORE2.png)

### CORE2

```plaintext
enable
configure terminal
interface range fa0/1 - 3
channel-group 1 mode active
end
write memory

configure terminal
interface port-channel 1
switchport mode trunk
switchport trunk native vlan 99
end
write memory
```

## 15.2 EtherChannel entre CORE1 y SW-A1

### CORE1

```plaintext
enable
configure terminal
interface range fa0/4 - 6
channel-group 2 mode active
end
write memory

configure terminal
interface port-channel 2
switchport mode trunk
switchport trunk native vlan 99
end
write memory
```

**Captura de EtherChannel SW-A1:**

![EtherChannel SW-A1](Imagenes/EthernetChannelSWA1.png)

### SW-A1

```plaintext
enable
configure terminal
interface range fa0/1 - 3
channel-group 2 mode active
end
write memory

configure terminal
interface port-channel 2
switchport mode trunk
switchport trunk native vlan 99
end
write memory
```

## 15.3 EtherChannel entre CORE2 y SW-A6

### CORE2

```plaintext
enable
configure terminal
interface range fa0/7 - 9
channel-group 3 mode active
end
write memory

configure terminal
interface port-channel 3
switchport mode trunk
switchport trunk native vlan 99
end
write memory
```

**Captura de EtherChannel SW-A6:**

![EtherChannel SW-A6](Imagenes/EthernetChannelSWA6.png)

### SW-A6

```plaintext
enable
configure terminal
interface range fa0/1 - 3
channel-group 3 mode active
end
write memory

configure terminal
interface port-channel 3
switchport mode trunk
switchport trunk native vlan 99
end
write memory
```

---

# 16. Configuración de Rapid PVST+

La siguiente configuración fue aplicada en todos los switches:

```plaintext
enable
configure terminal
spanning-tree mode rapid-pvst
end
write memory
```

**Capturas de Rapid PVST+:**

![Rapid PVST CORE1](Imagenes/RapidCORE1.png)
![Rapid PVST SW-A3](Imagenes/RapidSWA3.png)
![Rapid PVST SW-A3-ACC1](Imagenes/RapidSWA3ACC1.png)

---

# 17. Configuración de root bridge por VLAN

Cada switch principal de área fue configurado como root bridge de su VLAN.

## 17.1 Comandos aplicados

### SW-A1
```plaintext
enable
configure terminal
spanning-tree vlan 15 root primary
end
write memory
```

### SW-A2
```plaintext
enable
configure terminal
spanning-tree vlan 25 root primary
end
write memory
```

### SW-A3
```plaintext
enable
configure terminal
spanning-tree vlan 35 root primary
end
write memory
```

### SW-A4
```plaintext
enable
configure terminal
spanning-tree vlan 45 root primary
end
write memory
```

### SW-A5
```plaintext
enable
configure terminal
spanning-tree vlan 55 root primary
end
write memory
```

### SW-A6
```plaintext
enable
configure terminal
spanning-tree vlan 65 root primary
end
write memory
```

### SW-A7
```plaintext
enable
configure terminal
spanning-tree vlan 75 root primary
end
write memory
```

### SW-A8
```plaintext
enable
configure terminal
spanning-tree vlan 85 root primary
end
write memory
```

**Capturas de root bridge (ejemplos):**

![Root Bridge SW-A2](Imagenes/RootBridgeSWA2.png)
![Root Bridge SW-A5](Imagenes/RootBridgeSWA5.png)
![Root Bridge SW-A7](Imagenes/RootBridgeSWA7.png)

---

# 18. Configuración de puertos access por VLAN

## 18.1 Switches principales

### SW-A1 — VLAN 15
```plaintext
enable
configure terminal
interface range fa0/7 - 12
switchport mode access
switchport access vlan 15
end
write memory
```

### SW-A2 — VLAN 25
```plaintext
enable
configure terminal
interface range fa0/3 - 8
switchport mode access
switchport access vlan 25
end
write memory
```

### SW-A3 — VLAN 35
```plaintext
enable
configure terminal
interface range fa0/3 - 10
switchport mode access
switchport access vlan 35
end
write memory
```

### SW-A4 — VLAN 45
```plaintext
enable
configure terminal
interface range fa0/3 - 6
switchport mode access
switchport access vlan 45
end
write memory
```

### SW-A5 — VLAN 55
```plaintext
enable
configure terminal
interface range fa0/2 - 8
switchport mode access
switchport access vlan 55
end
write memory
```

### SW-A6 — VLAN 65
```plaintext
enable
configure terminal
interface range fa0/6 - 12
switchport mode access
switchport access vlan 65
end
write memory
```

### SW-A7 — VLAN 75
```plaintext
enable
configure terminal
interface range fa0/2 - 6
switchport mode access
switchport access vlan 75
end
write memory
```

### SW-A8 — VLAN 85
```plaintext
enable
configure terminal
interface range fa0/2 - 6
switchport mode access
switchport access vlan 85
end
write memory
```

## 18.2 Switches de acceso

### SW-A1-ACC1 — VLAN 15
```plaintext
enable
configure terminal
interface range fa0/2 - 10
switchport mode access
switchport access vlan 15
end
write memory
```

### SW-A1-ACC2 — VLAN 15
```plaintext
enable
configure terminal
interface range fa0/2 - 10
switchport mode access
switchport access vlan 15
end
write memory
```

### SW-A3-ACC1 — VLAN 35
```plaintext
enable
configure terminal
interface range fa0/2 - 10
switchport mode access
switchport access vlan 35
end
write memory
```

### SW-A6-ACC1 — VLAN 65
```plaintext
enable
configure terminal
interface range fa0/2 - 10
switchport mode access
switchport access vlan 65
end
write memory
```

---

# 19. Configuración de VLAN 999 Blackhole

La VLAN 999 fue usada para asignar puertos no utilizados y apagarlos administrativamente.

## 19.1 Ejemplo general

```plaintext
enable
configure terminal
interface range fa0/13 - 24
switchport mode access
switchport access vlan 999
shutdown
end
write memory
```

## 19.2 Aplicación por switch

### CORE1
```plaintext
enable
configure terminal
interface range fa0/12 - 24
switchport mode access
switchport access vlan 999
shutdown
end
write memory
```

### CORE2
```plaintext
enable
configure terminal
interface range fa0/12 - 24
switchport mode access
switchport access vlan 999
shutdown
end
write memory
```

### SW-A1
```plaintext
enable
configure terminal
interface range fa0/13 - 24
switchport mode access
switchport access vlan 999
shutdown
end
write memory
```

### SW-A2
```plaintext
enable
configure terminal
interface range fa0/5 - 24
switchport mode access
switchport access vlan 999
shutdown
end
write memory
```

### SW-A3
```plaintext
enable
configure terminal
interface range fa0/11 - 24
switchport mode access
switchport access vlan 999
shutdown
end
write memory
```

### SW-A4
```plaintext
enable
configure terminal
interface range fa0/7 - 24
switchport mode access
switchport access vlan 999
shutdown
end
write memory
```

### SW-A5
```plaintext
enable
configure terminal
interface range fa0/10 - 24
switchport mode access
switchport access vlan 999
shutdown
end
write memory
```

### SW-A6
```plaintext
enable
configure terminal
interface range fa0/10 - 24
switchport mode access
switchport access vlan 999
shutdown
end
write memory
```

### SW-A7
```plaintext
enable
configure terminal
interface range fa0/4 - 24
switchport mode access
switchport access vlan 999
shutdown
end
write memory
```

### SW-A8
```plaintext
enable
configure terminal
interface range fa0/4 - 24
switchport mode access
switchport access vlan 999
shutdown
end
write memory
```

### SW-A1-ACC1
```plaintext
enable
configure terminal
interface range fa0/4 - 24
switchport mode access
switchport access vlan 999
shutdown
end
write memory
```

### SW-A1-ACC2
```plaintext
enable
configure terminal
interface range fa0/4 - 24
switchport mode access
switchport access vlan 999
shutdown
end
write memory
```

### SW-A3-ACC1
```plaintext
enable
configure terminal
interface range fa0/4 - 24
switchport mode access
switchport access vlan 999
shutdown
end
write memory
```

### SW-A6-ACC1
```plaintext
enable
configure terminal
interface range fa0/4 - 24
switchport mode access
switchport access vlan 999
shutdown
end
write memory
```

---

# 20. Configuración de PCs

Cada PC fue conectada con cable `Copper Straight-Through` a su puerto access correspondiente y configurada manualmente desde:

**Desktop → IP Configuration**

Los nombres utilizados para las PCs fueron:

- `PC1-A1`, `PC2-A1`, `PC3-A1`, `PC4-A1`
- `PC1-A2`, `PC2-A2`
- `PC1-A3`, `PC2-A3`, `PC3-A3`
- `PC1-A4`, `PC2-A4`
- `PC1-A5`, `PC2-A5`
- `PC1-A6`, `PC2-A6`, `PC3-A6`
- `PC1-A7`, `PC2-A7`
- `PC1-A8`, `PC2-A8`

Esta nomenclatura facilita el orden visual y el troubleshooting.

---

# 21. Pruebas de conectividad

## 21.1 Pruebas exitosas dentro de la misma VLAN

Se verificó conectividad correcta entre hosts de la misma área:

| Área | Prueba exitosa |
|---|---|
| Área 1 | `PC1-A1` → `192.168.75.11` |
| Área 2 | `PC1-A2` → `192.168.76.35` |
| Área 3 | `PC1-A3` → `192.168.75.195` |
| Área 4 | `PC1-A4` → `192.168.76.115` |
| Área 5 | `PC1-A5` → `192.168.76.3` |
| Área 6 | `PC1-A6` → `192.168.75.131` |
| Área 7 | `PC1-A7` → `192.168.76.99` |
| Área 8 | `PC1-A8` → `192.168.76.67` |

**Capturas de pings exitosos y fallidos desde área 1 a la 4:**

![PING 1](Imagenes/PING1.png)
![PING 2](Imagenes/PING2.png)
![PING 3](Imagenes/PING3.png)
![PING 4](Imagenes/PING4.png)

## 21.2 Pruebas fallidas entre VLANs

Se verificó correctamente el aislamiento entre áreas distintas, ya que no se configuró enrutamiento inter-VLAN:

| Área origen | Prueba fallida esperada |
|---|---|
| Área 1 | `PC1-A1` → `192.168.76.34` |
| Área 2 | `PC1-A2` → `192.168.75.10` |
| Área 3 | `PC1-A3` → `192.168.76.114` |
| Área 4 | `PC1-A4` → `192.168.75.194` |
| Área 5 | `PC1-A5` → `192.168.75.130` |
| Área 6 | `PC1-A6` → `192.168.76.98` |
| Área 7 | `PC1-A7` → `192.168.76.66` |
| Área 8 | `PC1-A8` → `192.168.75.10` |

**Capturas de pings exitosos y fallidos desde área 5 a la 8:**

![PING 5](Imagenes/PING5.png)
![PING 6](Imagenes/PING6.png)
![PING 7](Imagenes/PING7.png)
![PING 8](Imagenes/PING8.png)

## 21.3 Observaciones de pruebas

En Packet Tracer, los primeros pings pueden fallar parcialmente debido a:

- resolución ARP inicial,
- aprendizaje de direcciones MAC,
- convergencia temporal del simulador.

Esto es normal y no necesariamente indica error en la topología.

---

# 22. Validaciones recomendadas para anexar con capturas

Se recomienda incluir capturas de los siguientes comandos:

## 22.1 VLANs
```plaintext
show vlan brief
```

## 22.2 Trunks
```plaintext
show interfaces trunk
```

## 22.3 EtherChannel
```plaintext
show etherchannel summary
```

## 22.4 STP
```plaintext
show spanning-tree summary
show spanning-tree vlan 15
show spanning-tree vlan 25
show spanning-tree vlan 35
show spanning-tree vlan 45
show spanning-tree vlan 55
show spanning-tree vlan 65
show spanning-tree vlan 75
show spanning-tree vlan 85
```

## 22.5 VTP
```plaintext
show vtp status
```

---

# 23. Evidencia de configuraciones por dispositivo

Se anexan capturas de la configuración aplicada en cada switch para evidenciar el detalle completo por dispositivo.

## 23.1 Switches de núcleo

![Config CORE1](Imagenes/SwitchCORE1Config.png)
![Config CORE2](Imagenes/SwitchCORE2Config.png)

## 23.2 Switches principales (distribución)

![Config SW-A1](Imagenes/SwitchA1Config.png)
![Config SW-A2](Imagenes/SwitchSWA2Config.png)
![Config SW-A3](Imagenes/SwitchSWA3Config.png)
![Config SW-A4](Imagenes/SwitchSWA4Config.png)
![Config SW-A5](Imagenes/SwitchSWA5Config.png)
![Config SW-A6](Imagenes/SwitchSWA6Config.png)
![Config SW-A7](Imagenes/SwitchSWA7Config.png)
![Config SW-A8](Imagenes/SwitchSWA8Config.png)

## 23.3 Switches de acceso

![Config SW-A1-ACC1](Imagenes/SwitchSWA1ACC1.png)
![Config SW-A1-ACC2](Imagenes/SwitchA1ACC2Config.png)
![Config SW-A3-ACC1](Imagenes/SwitchSWA3ACC1Config.png)
![Config SW-A6-ACC1](Imagenes/SwitchSWA6ACC1.png)

---

# 24. Observaciones importantes de implementación

- La VLAN 99 fue usada correctamente como VLAN nativa en enlaces troncales.
- La VLAN 999 fue creada para puertos no utilizados, respondiendo al requisito de seguridad del enunciado.
- Rapid PVST+ fue activado en todos los switches, y cada switch principal fue configurado como root bridge para su VLAN respectiva.
- Se implementó EtherChannel con LACP en enlaces críticos entre núcleo y distribución.
- La conectividad validada corresponde a hosts dentro de la misma VLAN, que además coincide con el enfoque de pruebas indicado en la práctica y la rúbrica.

---

# 25. Conclusiones

La implementación desarrollada en Cisco Packet Tracer permitió construir una red segmentada y organizada para distintas áreas del Aeropuerto Internacional La Aurora. Se logró:

- separar lógicamente cada área por medio de VLANs,
- implementar una estructura jerárquica de switches,
- agregar redundancia con EtherChannel,
- proteger la red con Rapid PVST+,
- usar VLAN nativa y VLAN Blackhole,
- asignar direcciones IP conforme al subnetting definido,
- demostrar conectividad dentro de cada VLAN y aislamiento entre distintas VLANs.

El diseño final es funcional, escalable, administrable y coherente con un escenario realista de red aeroportuaria.


---

