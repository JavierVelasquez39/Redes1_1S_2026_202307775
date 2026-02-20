# MANUAL TÉCNICO: DISEÑO E IMPLEMENTACIÓN DE RED LAN
## PRIMERA PRÁCTICA DEL LABORATORIO



## 1. Datos del estudiante
* **Estudiante:** Javier Andrés Velásquez Bonilla 
* **Carnet:** 202307775 
* **Curso:** Redes de Computadoras 1 
* **Institución:** Universidad de San Carlos de Guatemala 
* **Facultad:** Ingeniería

---

## 2. Introducción
El presente documento detalla el diseño y la configuración de una infraestructura de red LAN para la empresa Constructiva S.A. La red se distribuye en tres niveles, garantizando la interconexión entre 10 departamentos y sus respectivos servidores mediante switches Cisco 2960. Se aplicó el estándar de cableado TIA/EIA-568B y direccionamiento IPv4 estático para asegurar una comunicación eficiente y organizada.

---

## 3. Tabla de Direccionamiento IP
*En esta sección se detallan las direcciones IP estáticas asignadas a cada dispositivo final (PCs, Laptops y Servidores) bajo la red base 192.178.75.0/24.*

### Tabla de Direccionamiento IP Combinada

| Piso | Nombre | Tipo | Departamento | IP asignada | Máscara de Subred |
| :--- | :--- | :--- | :--- | :--- | :--- |
| 1 | Recep_Lap_01 | Laptop | Recep | 192.178.75.1 | 255.255.255.0 |
| 1 | Recep_Lap_02 | Laptop | Recep | 192.178.75.2 | 255.255.255.0 |
| 1 | Recep_PC_01 | PC | Recep | 192.178.75.3 | 255.255.255.0 |
| 1 | Recep_PC_02 | PC | Recep | 192.178.75.4 | 255.255.255.0 |
| 1 | Recep_PC_03 | PC | Recep | 192.178.75.5 | 255.255.255.0 |
| 1 | Recep_PC_04 | PC | Recep | 192.178.75.6 | 255.255.255.0 |
| 1 | Recep_Lap_03 | Laptop | Recep | 192.178.75.7 | 255.255.255.0 |
| 1 | Recep_PC_05 | PC | Recep | 192.178.75.8 | 255.255.255.0 |
| 1 | Recep_Lap_04 | Laptop | Recep | 192.178.75.9 | 255.255.255.0 |
| 1 | Recep_PC_06 | PC | Recep | 192.178.75.10 | 255.255.255.0 |
| 1 | Recep_PC_07 | PC | Recep | 192.178.75.11 | 255.255.255.0 |
| 1 | Recep_Server_01 | Server | Recep | 192.178.75.12 | 255.255.255.0 |
| 1 | Conta_Lap_01 | Laptop | Conta | 192.178.75.20 | 255.255.255.0 |
| 1 | Conta_PC_02 | PC | Conta | 192.178.75.21 | 255.255.255.0 |
| 1 | Conta_Lap_02 | Laptop | Conta | 192.178.75.22 | 255.255.255.0 |
| 1 | Conta_PC_02 | PC | Conta | 192.178.75.23 | 255.255.255.0 |
| 1 | Conta_Lap_03 | Laptop | Conta | 192.178.75.24 | 255.255.255.0 |
| 1 | Conta_PC_03 | PC | Conta | 192.178.75.25 | 255.255.255.0 |
| 1 | Conta_PC_04 | PC | Conta | 192.178.75.26 | 255.255.255.0 |
| 1 | Conta_PC_05 | PC | Conta | 192.178.75.27 | 255.255.255.0 |
| 1 | Legal_PC_01 | PC | Legal | 192.178.75.30 | 255.255.255.0 |
| 1 | Legal_PC_02 | PC | Legal | 192.178.75.31 | 255.255.255.0 |
| 1 | Legal_PC_03 | PC | Legal | 192.178.75.32 | 255.255.255.0 |
| 1 | Legal_Lap_01 | Laptop | Legal | 192.178.75.33 | 255.255.255.0 |
| 1 | Legal_Lap_02 | Laptop | Legal | 192.178.75.34 | 255.255.255.0 |
| 1 | Reu_Lap_01 | Laptop | Reuniones | 192.178.75.40 | 255.255.255.0 |
| 1 | Reu_Lap_02 | Laptop | Reuniones | 192.178.75.41 | 255.255.255.0 |
| 1 | Reu_Lap_03 | Laptop | Reuniones | 192.178.75.42 | 255.255.255.0 |
| 1 | Reu_Lap_04 | Laptop | Reuniones | 192.178.75.43 | 255.255.255.0 |
| 1 | Reu_Lap_05 | Laptop | Reuniones | 192.178.75.44 | 255.255.255.0 |
| 2 | Arqui_PC_01 | PC | Arquitectura | 192.178.75.50 | 255.255.255.0 |
| 2 | Arqui_PC_02 | PC | Arquitectura | 192.178.75.51 | 255.255.255.0 |
| 2 | Arqui_PC_03 | PC | Arquitectura | 192.178.75.52 | 255.255.255.0 |
| 2 | Arqui_PC_04 | PC | Arquitectura | 192.178.75.53 | 255.255.255.0 |
| 2 | Arqui_PC_05 | PC | Arquitectura | 192.178.75.54 | 255.255.255.0 |
| 2 | Arqui_PC_06 | PC | Arquitectura | 192.178.75.55 | 255.255.255.0 |
| 2 | Urba_Lap_01 | Laptop | Urbanismo | 192.178.75.60 | 255.255.255.0 |
| 2 | Urba_Lap_02 | Laptop | Urbanismo | 192.178.75.61 | 255.255.255.0 |
| 2 | Urba_Lap_03 | Laptop | Urbanismo | 192.178.75.62 | 255.255.255.0 |
| 2 | Urba_Lap_04 | Laptop | Urbanismo | 192.178.75.63 | 255.255.255.0 |
| 2 | Urba_Lap_05 | Laptop | Urbanismo | 192.178.75.64 | 255.255.255.0 |
| 2 | Urba_Server_01 | Server | Urbanismo | 192.178.75.65 | 255.255.255.0 |
| 2 | Planos_PC_01 | PC | Planos | 192.178.75.70 | 255.255.255.0 |
| 2 | Planos_PC_02 | PC | Planos | 192.178.75.71 | 255.255.255.0 |
| 2 | Planos_PC_03 | PC | Planos | 192.178.75.72 | 255.255.255.0 |
| 2 | Planos_Lap_01 | Laptop | Planos | 192.178.75.73 | 255.255.255.0 |
| 2 | Planos_Lap_02 | Laptop | Planos | 192.178.75.74 | 255.255.255.0 |
| 3 | Direccion_Lap_01 | Laptop | Direccion | 192.178.75.80 | 255.255.255.0 |
| 3 | Direccion_Lap_02 | Laptop | Direccion | 192.178.75.81 | 255.255.255.0 |
| 3 | Direccion_Lap_03 | Laptop | Direccion | 192.178.75.82 | 255.255.255.0 |
| 3 | Direccion_Lap_04 | Laptop | Direccion | 192.178.75.83 | 255.255.255.0 |
| 3 | Ingenieria_PC_01 | PC | Ingenieria | 192.178.75.90 | 255.255.255.0 |
| 3 | Ingenieria_PC_02 | PC | Ingenieria | 192.178.75.91 | 255.255.255.0 |
| 3 | Ingenieria_PC_03 | PC | Ingenieria | 192.178.75.92 | 255.255.255.0 |
| 3 | Ingenieria_PC_04 | PC | Ingenieria | 192.178.75.93 | 255.255.255.0 |
| 3 | Ingenieria_PC_05 | PC | Ingenieria | 192.178.75.94 | 255.255.255.0 |
| 3 | Ingenieria_Server_01 | Servidor | Ingenieria | 192.178.75.95 | 255.255.255.0 |
| 3 | Servidores_Server_01 | Servidor | Servidores | 192.178.75.100 | 255.255.255.0 |
| 3 | Servidores_Server_02 | Servidor | Servidores | 192.178.75.101 | 255.255.255.0 |
| 3 | Servidores_Server_03 | Servidor | Servidores | 192.178.75.102 | 255.255.255.0 |



---

## 4. Comandos de Configuración de Switches
Para cumplir con los requisitos de seguridad y gestión, se configuró cada switch mediante la Interfaz de Línea de Comandos (CLI).

### Bloque de comandos estándar utilizado:
A continuación se muestra el ejemplo de los comandos aplicados a cada uno de los switches (Sustituir `[NOMBRE_SWITCH]` por el nombre correspondiente como `SW_L1`, `Recep`, `Conta`, etc.):

```bash
# Entrar a modo privilegiado
enable

# Entrar a modo de configuración global
configure terminal

# Configuración de Hostname oficial
hostname [NOMBRE_SWITCH]

# Configuración de contraseña de acceso (Número de Carnet)
enable password 202307775

# Salir y guardar configuración en la NVRAM
exit
write memory
```


#### Configuración de diversos switches de niveles y departamentos:


![switch1](/Imagenes/SwitchArqui.png)

![switch2](/Imagenes/SwitchDireccion.png)

![switch3](/Imagenes/SwitchIngenieria.png)

![switch4](/Imagenes/SwitchPiso1.png)

![switch5](/Imagenes/SwitchPiso2.png)

![switch6](/Imagenes/SwitchPiso3.png)

![switch7](/Imagenes/SwitchServidores.png)

## 5. Manual de Conectividad y Pruebas
Para validar el funcionamiento de la red, se realizaron pruebas de conectividad total y análisis de protocolos de Capa 2 y Capa 3.


### 5.1 Pruebas de Ping (ICMP)
Se ejecutaron pruebas de conectividad entre diferentes niveles del edificio para asegurar que los enlaces troncales y el direccionamiento son correctos.

* **Comando:** `ping [IP_DESTINO]`
* **Resultado esperado:** 0% packet loss (Conectividad exitosa).

#### Aquí se presentan 10 ejemplos distintos de PING extiosos en la práctica:

![ping1](/Imagenes/PING1.png)

![ping2](/Imagenes/PING2.png)

![ping3](/Imagenes/PING3.png)

![ping4](/Imagenes/PING4.png)

![ping5](/Imagenes/PING5.png)

![ping6](/Imagenes/PING6.png)

![ping7](/Imagenes/PING7.png)

![ping8](/Imagenes/PING8.png)

![ping9](/Imagenes/PING9.png)

![ping10](/Imagenes/PING10.png)



### 5.2 Análisis de Protocolos (ARP e ICMP)
Utilizando el modo simulación de Cisco Packet Tracer, se verificó el flujo de paquetes:

1. **Protocolo ARP:** Proceso de resolución de direcciones MAC para identificar el destino físico en la LAN.

![arp](/Imagenes/ARP.png)

2. **Protocolo ICMP:** Intercambio de mensajes Echo Request y Echo Reply para verificar la comunicación.

![simula1](/Imagenes/Simulacion1.png)


#### Aquí hay 10 ejemplos de configuraciones de VPCs

![VCP](/Imagenes/IPConfig_Arqui1.png)

![VCP](/Imagenes/IPConfig_Conta1.png)

![VCP](/Imagenes/IPConfig_Ingenieria1.png)

![VCP](/Imagenes/IPConfig_Legal1.png)

![VCP](/Imagenes/IPConfig_Planos.png)

![VCP](/Imagenes/IPConfig_Recep1.png)

![VCP](/Imagenes/IPConfig_Reuniones1.png)

![VCP](/Imagenes/IPConfig_Servidores1.png)

![VCP](/Imagenes/IPConfig_Urba1.png)

![VCP](/Imagenes/IPConfig_Ingenieria2.png)

### 5.3 Verificación de Tablas de Direcciones MAC
En los switches, se utilizó el siguiente comando para verificar que el dispositivo aprendió correctamente las direcciones físicas en sus puertos tras el tráfico generado:

```bash
show mac-address-table
```
![mac](/Imagenes/MACAdress.png)


## Un vistazo a los pisos y su correcta simulación

Al no especificarse la topología que se debía aplicar en el enunciado, se tomó la deicisón de aplicar la topología de estrella extendida, para lograr un cableado más ordenado y comprensible.

### Piso 1

![simula1](/Imagenes/Simulacion1.png)


### Piso 2

![simula2](/Imagenes/Simulacion2.png)

### Piso 3

![simula3](/Imagenes/Simulacion3.png)