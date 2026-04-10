## Asignación final área ↔ VLAN ↔ hosts


Área	Nombre	VLAN	Hosts requeridos
1	Terminal de Pasajeros	15	74
2	Migración	25	25
3	Aduana	35	32
4	Torre de Control	45	5
5	Operaciones de Aerolíneas / Check-in	55	26
6	Carga Aérea	65	47
7	Seguridad Aeroportuaria	75	8
8	Administración / DGAC	85	20

Vamos a hacer una topología tipo núcleo-distribución/acceso simplificada, muy ordenada para Packet Tracer:

2 switches centrales:
CORE1
CORE2
8 switches principales, uno por área:
SW-A1
SW-A2
SW-A3
SW-A4
SW-A5
SW-A6
SW-A7
SW-A8
switches de acceso adicionales solo en áreas que lo necesiten por cantidad de hosts.
Áreas críticas con alta disponibilidad

Las áreas críticas serán:

Área 4: Torre de Control
Área 2: Migración
Área 6: Carga Aérea
Área 1: Terminal de Pasajeros

Justificación breve:

Torre de Control: crítica para coordinación operativa.
Migración: crítica para flujo legal de pasajeros.
Carga Aérea: crítica por logística y operación comercial.
Terminal de Pasajeros: alta concentración de usuarios y servicios.

A estas áreas las conectaremos a CORE1 y CORE2, para cumplir redundancia.

Switches que vas a colocar

Usa Cisco 2960 en todos, porque la rúbrica lo menciona.

Coloca estos:

Core
2 switches 2960:
CORE1
CORE2
Principales por área
8 switches 2960:
SW-A1
SW-A2
SW-A3
SW-A4
SW-A5
SW-A6
SW-A7
SW-A8
Switches adicionales de acceso

Para que los hosts quepan cómodamente:

Área 1 (74 hosts): 2 switches extra
SW-A1-ACC1
SW-A1-ACC2
Área 3 (32 hosts): 1 switch extra
SW-A3-ACC1
Área 6 (47 hosts): 1 switch extra
SW-A6-ACC1

Con eso tendrás una topología realista y fácil de documentar.