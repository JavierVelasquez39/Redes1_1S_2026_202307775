# Manual Técnico - Tarea 3: Configuración de VLANs y VTP
**Estudiante:** Javier Andrés Velásquez Bonilla  
**Carnet:** 202307775  
**Curso:** Redes de Computadoras 1  

---

## 1. Diseño de la Topología
Se construyó una red en Cisco Packet Tracer utilizando cuatro switches 2960 y seis PCs distribuidas en tres departamentos: ADMIN, MERCA y VENTAS.

![Topologia](/Imagenes/topologia.png)

---

## 2. Scripts de Configuración

### 2.1. Configuración de Enlaces Troncales (Trunk)
Para permitir el paso de información de VTP y el tráfico de múltiples VLANs, se configuraron los puertos de interconexión entre switches como troncales.

**En Switch0:**
```ios
interface range fa0/1-3
 switchport mode trunk
```
![Switch0Trunk](/Imagenes/SwitchTronk.png)


**En Switches Clientes (ADMIN, MERCA, VENTAS):**
```
interface fa0/1
 switchport mode trunk
```

![trunkAdmin](/Imagenes/trunkAdmin.png)

![trunkMerca](/Imagenes/trunkMerca.png)

![trunkVentas](/Imagenes/trunkVentas.png)

### 2.2. Configuración de VTP (VLAN Trunking Protocol)

Se estableció el Switch0 como servidor para centralizar la creación de VLANs.

**Configuración en Switch0 (Servidor):**

```
vtp mode server
vtp domain USAC
vtp password redes1
```

![vtpSwitch0](/Imagenes/vtpSwitch0.png)

**Configuración en Clientes:**

```
vtp mode client
vtp domain USAC
vtp password redes1
```

![vtpAdmin](/Imagenes/vtpAdmin.png)

![vtpMerca](/Imagenes/vtpMerca.png)

### 2.3. Creación y Asignación de VLANs

Se crearon las VLANs 10, 20 y 30 en el servidor y se asignaron los puertos de acceso en los clientes.

**Creación en Switch0:**

```
vlan 10
 name ADMIN
vlan 20
 name MERCA
vlan 30
 name VENTAS
```

![creacionVLANS](/Imagenes/creacionVLANS.png)

**Asignación en Clientes:**

```
interface range fa0/2-4
 switchport mode access
 switchport access vlan 30
```

![vlan30](/Imagenes/configVENTAS.png)

![vlan20](/Imagenes/configMERCA.png)

![vlan10](/Imagenes/configADMIN.png)

## 3. Verificación de Configuración


### 3.1 Estado de VTP (show vtp status)

Comprobación de que el dominio es "USAC" y los modos son correctos.

![vtpShowStatus1](/Imagenes/showAdmin.png)

![vtpShowStatus2](/Imagenes/showMerca.png)

![vtpShowStatus3](/Imagenes/showVentas.png)

### 3.2 Tabla de VLANs (show vlan brief)

Evidencia de que las VLANs se propagaron y los puertos están correctamente asignados.

![showVLANBriefAdmin](/Imagenes/showVLANBriefAdmin.png)

![showVLANBriefMerca](/Imagenes/ShowVLANbriefMerca.png)

![showVLANBriefVentas](/Imagenes/showvlanbrief.png)

## 4. Pruebas de Conectividad (Ping)

### Tabla de Direccionamiento IP

| Dispositivo | VLAN | Dirección IP | Máscara de Subred |
| :--- | :--- | :--- | :--- |
| **PC0** | ADMIN (10) | 192.168.10.10 | 255.255.255.0 |
| **PC2** | MERCA (20) | 192.168.20.10 | 255.255.255.0 |
| **PC3** | MERCA (20) | 192.168.20.11 | 255.255.255.0 |
| **PC4** | VENTAS (30) | 192.168.30.10 | 255.255.255.0 |
| **PC5** | VENTAS (30) | 192.168.30.11 | 255.255.255.0 |
| **PC6** | VENTAS (30) | 192.168.30.12 | 255.255.255.0 |

Se realizaron pruebas para verificar la segmentación de la red.

![pingVentas](/Imagenes/pingVentas.png)

![pingMERCA](/Imagenes/pingMERCA.png)

![pingFail](/Imagenes/pingFail.png)

![pingAdmin](/Imagenes/pingAdmin.png)

