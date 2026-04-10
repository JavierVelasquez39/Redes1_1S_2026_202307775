# Informe de Desarrollo — Práctica 2
## Red del Aeropuerto Internacional La Aurora

**Curso:** Redes de Computadoras 1  
**Universidad:** Universidad de San Carlos de Guatemala  
**Carné:** 202307775  
**Estudiante:** Javier Andrés Velásquez Bonilla  
**Software:** Cisco Packet Tracer 8.2.2  
**Fecha:** 9 de abril de 2026

---

# 1. Descripción general de la implementación

La práctica consistió en diseñar e implementar una red LAN segmentada por VLANs para distintas áreas del Aeropuerto Internacional La Aurora. La solución se construyó con un enfoque jerárquico de tres capas (Core, Distribución y Acceso), incorporando VTP para propagación de VLANs, troncales con VLAN nativa 99, EtherChannel con LACP para enlaces críticos, Rapid PVST+ para evitar bucles de capa 2, y VLAN 999 como Blackhole para puertos no utilizados.

Las VLANs asignadas fueron: 15, 25, 35, 45, 55, 65, 75 y 85, cada una asociada a un área operativa. El direccionamiento se planificó mediante VLSM para optimizar el uso del espacio IPv4 y garantizar aislamiento entre dominios de broadcast, sin enrutamiento inter‑VLAN.

---

# 2. Proceso de implementación

## 2.1 Construcción de la topología

Se comenzó con el armado progresivo de la topología, verificando enlaces físicos y jerarquía entre switches.

![Construcción 1](Imagenes/Construccion1.png)
![Construcción 6](Imagenes/Construccion6.png)
![Construcción 12](Imagenes/Construccion12.png)
![Construcción 15](Imagenes/Construccion15.png)

## 2.2 Configuración lógica principal

- **VTP:** CORE1 como servidor y el resto como clientes (dominio 202307775).  
- **VLANs:** creación y propagación desde CORE1.  
- **Troncales:** configuración en enlaces entre switches y VLAN nativa 99.  
- **EtherChannel:** LACP entre CORE1–CORE2, CORE1–SW-A1 y CORE2–SW-A6.  
- **Rapid PVST+:** habilitado en toda la topología.  
- **Root Bridge:** cada switch principal configurado como root de su VLAN.  
- **Seguridad:** VLAN 999 en puertos no usados.

---

# 3. Problemas encontrados y soluciones

## 3.1 Fallos iniciales de ping (ARP)
**Problema:** En Packet Tracer, los primeros pings entre hosts de la misma VLAN fallaron parcialmente.  
**Solución:** Se repitieron las pruebas tras el aprendizaje de ARP y MAC; la conectividad se normalizó.

## 3.2 VTP sin propagación
**Problema:** Algunas VLANs no se propagaban a todos los switches.  
**Solución:** Se verificó que el dominio, modo y contraseña VTP coincidieran en todos los equipos.

## 3.3 Inconsistencias en troncales
**Problema:** Algunos enlaces no transportaban todas las VLANs debido a configuración incompleta.  
**Solución:** Se estandarizó la configuración de trunk y se aplicó la VLAN nativa 99.

## 3.4 Puerto no utilizado en VLAN 1
**Problema:** Se detectó un puerto libre en VLAN 1 (SW-A5 Fa0/9).  
**Solución:** Se reasignó a VLAN 999 y se apagó administrativamente.

---

# 4. Evidencias de correcto funcionamiento

## 4.1 Verificación de VLANs y troncales

![VLANs CORE1](Imagenes/VLANCore1.png)
![Trunk CORE1](Imagenes/CORE1Trunk.png)
![VLAN 99 CORE1](Imagenes/CORE1VLAN99.png)

## 4.2 EtherChannel y STP

![EtherChannel CORE1](Imagenes/EthernetChannelCORE1.png)
![Rapid PVST CORE1](Imagenes/RapidCORE1.png)

## 4.3 Pruebas de conectividad

**Ping exitoso dentro de la misma VLAN:**

![PING 1](Imagenes/PING1.png)
![PING 2](Imagenes/PING2.png)

**Ping fallido entre VLANs (aislamiento esperado):**

![PING 5](Imagenes/PING5.png)

---

# 5. Resumen

La red implementada cumple con los requisitos de segmentación por VLAN, seguridad básica de puertos, redundancia y control de bucles. Se validó conectividad dentro de cada VLAN y aislamiento entre áreas, confirmando el correcto comportamiento de la topología. La documentación y las evidencias presentadas respaldan la implementación y su funcionamiento en el entorno simulado.

