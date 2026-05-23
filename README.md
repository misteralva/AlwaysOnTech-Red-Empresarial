# AlwaysOn Tech — Red Empresarial Cisco

![Cisco](https://img.shields.io/badge/Cisco-Packet%20Tracer-blue?logo=cisco)
![OSPF](https://img.shields.io/badge/Routing-OSPF-green)
![HSRP](https://img.shields.io/badge/Redundancia-HSRP-orange)
![VTP](https://img.shields.io/badge/VLANs-VTP-purple)

> Proyecto de diseño e implementación de una red empresarial completa para la empresa ficticia **AlwaysOn Tech**, realizado en Cisco Packet Tracer como parte de la asignatura *2509_ASIX_0370_Planificació i administració de xarxes — 1PM HOSPITALET*.

**Autores:** Alex Naranjo — David Alvarez

---

## Descripción

AlwaysOn Tech es una empresa de consultoría informática con sede en una oficina de dos plantas. La red da servicio a cinco departamentos internos y a consultores externos que acceden de forma remota a través de Internet.

El objetivo del proyecto ha sido diseñar e implementar una infraestructura de red empresarial con **redundancia total**, **seguridad por departamentos** y **servicios corporativos** completos.

---

## Topología

```
Internet (ISP1/ISP2)
        |
   [R0] — [R1]          ← Routers de borde con OSPF
    |  \  / |
  Core0 — Core1         ← Switches núcleo L3 con HSRP
    |         |
  Dist0 — Dist1         ← Switches de distribución
    |         |
  SW-RRHH  SW-VENTAS  SW-OPER  SW-FINANZ  SW-IT  SW-SERVERS

Consultores → Router-ISP1 → R0 → Red interna
```

---

## Tecnologías y protocolos

| Capa | Protocolo/Tecnología | Función |
|------|---------------------|---------|
| Enrutamiento | OSPF área 0 | Enrutamiento dinámico entre routers y Cores |
| Redundancia L3 | HSRP | IP virtual compartida entre Core0 y Core1 |
| Gestión VLANs | VTP | Propagación automática de VLANs desde Core0 |
| Redundancia L2 | STP | Gestión de bucles en switches de acceso |
| Traducción IPs | NAT/PAT | Salida a Internet desde red interna |
| Seguridad | ACLs extendidas | Aislamiento entre departamentos |
| Acceso remoto | SSH v2 | Administración segura de dispositivos |
| Sincronización | NTP | Reloj unificado en todos los dispositivos |
| Registro | Syslog | Centralización de logs en SRV-SYSLOG |
| Asignación IPs | DHCP | IPs automáticas por departamento |
| Resolución nombres | DNS | Resolución de nombres internos |

---

## Estructura del repositorio

```
AlwaysOnTech-Red-Empresarial/
│
├── README.md                          ← Este archivo
├── AlwaysOnTech.pkt                   ← Simulación Cisco Packet Tracer
│
├── documentacion/
│   ├── AlwaysOnTech_documento_completo.docx    ← Memoria del proyecto
│   ├── AlwaysOnTech_guia_comandos_completa.docx ← Guía de comandos
│   └── AlwaysOnTech_VLSM_v2.xlsx              ← Tablas de direccionamiento IP
```

---

## Direccionamiento IP

| Zona | Rango | Función |
|------|-------|---------|
| WAN ISP1 | 200.0.0.0/30 | Enlace Router-ISP1 ↔ R0 |
| WAN ISP2 | 201.0.0.0/30 | Enlace Router-ISP2 ↔ R1 |
| LAN ISP1 | 172.16.0.0/29 | Red interna ISP1 y consultores |
| LAN ISP2 | 172.16.1.0/29 | Red interna ISP2 |
| Consultores | 192.168.100.0/29 | Red privada consultores VPN |
| Infraestructura | 10.0.0.0/8 | Enlaces L3 entre routers y Cores |
| Usuarios | 192.168.0.0/16 (VLSM) | VLANs de departamentos y servicios |

### VLANs de usuario

| VLAN | Nombre | Subred | Gateway HSRP |
|------|--------|--------|-------------|
| VLAN 10 | RRHH | 192.168.0.216/29 | 192.168.0.217 |
| VLAN 20 | Ventas | 192.168.0.192/28 | 192.168.0.193 |
| VLAN 30 | Operaciones | 192.168.0.208/29 | 192.168.0.209 |
| VLAN 40 | Finanzas | 192.168.0.224/29 | 192.168.0.225 |
| VLAN 50 | IT | 192.168.0.232/29 | 192.168.0.233 |
| VLAN 60 | Servidores | 192.168.0.176/28 | 192.168.0.177 |
| VLAN 70 | VoIP | 192.168.0.240/29 | 192.168.0.241 |
| VLAN 80 | WiFi Corp | 192.168.0.0/26 | 192.168.0.1 |
| VLAN 90 | WiFi Guest | 192.168.0.128/27 | 192.168.0.129 |
| VLAN 99 | Gestión | 192.168.0.160/28 | 192.168.0.161 |
| VLAN 100 | VPN Consultores | 192.168.0.64/26 | 192.168.0.65 |
| VLAN 999 | Fantasma (nativa) | — | — |

---

## Funcionalidades implementadas

- [x] **OSPF** — Enrutamiento dinámico en área 0 con router-ids únicos
- [x] **HSRP** — Redundancia de gateway en las 11 VLANs de usuario
- [x] **VTP** — Propagación de VLANs desde Core0 a todos los switches
- [x] **STP** — Gestión de redundancia con PortFast y BPDU Guard en acceso
- [x] **NAT/PAT** — Traducción de IPs en R0 y R1 hacia ISP1 e ISP2
- [x] **ACLs** — Aislamiento entre departamentos con acceso controlado a servidores
- [x] **DHCP** — Pools por VLAN con ip helper-address en Core0
- [x] **DNS** — Resolución de nombres internos (alwaysontech.local)
- [x] **FTP** — Servidor de archivos con tres niveles de usuario
- [x] **HTTP** — Intranet corporativa accesible desde todos los departamentos
- [x] **SSH v2** — Administración remota segura en todos los dispositivos
- [x] **NTP** — Sincronización de reloj centralizada
- [x] **Syslog** — Centralización de logs de todos los dispositivos
- [x] **TFTP** — Copias de seguridad de configuraciones
- [x] **VoIP** — VLAN 70 con soporte de teléfonos IP
- [x] **WiFi** — VLANs separadas para empleados y visitantes
- [x] **DHCP Snooping** — Protección contra servidores DHCP no autorizados
- [x] **Redundancia ISP** — Dos proveedores de Internet (ISP1 e ISP2)
- [x] **Consultores externos** — Acceso controlado a FTP e intranet

---

## Limitaciones conocidas de Cisco Packet Tracer

Durante la implementación encontramos las siguientes limitaciones de la herramienta de simulación:

1. **Port-security sticky** — Las MACs guardadas se pierden o cambian al reiniciar la simulación. Se documenta la configuración correcta pero no se mantiene entre sesiones.

2. **ACLs en SVIs del Catalyst 3650** — El comando `show ip interface vlan X` no muestra las ACLs aplicadas aunque sí funcionan. El comando `show running-config | include access-group` tampoco las muestra, pero el tráfico sí es filtrado.

3. **HSRP virtual IP no responde ping** — La IP virtual HSRP no responde a pings desde dispositivos en la misma VLAN. Es un comportamiento conocido de Packet Tracer.

4. **NAT multisalto** — El ping desde PCs internos hacia Internet (Router-ISP) devuelve request timed out aunque las traducciones NAT se crean correctamente. Es una limitación del simulador con NAT en múltiples saltos.

5. **VTP revision number** — Al reiniciar la simulación algunos switches pueden tener un número de revisión VTP mayor que Core0 y no aceptar las actualizaciones. Solución: cambiar temporalmente el dominio VTP a transparent y volver a client.

6. **DHCP pools con HSRP** — El servidor DHCP de Packet Tracer usa la IP real del relay agent (Core0 SVI) para determinar el pool, no la IP virtual HSRP. Los gateways de los pools deben coincidir con la IP real de Core0 en cada VLAN.

---

## Autor

| Nombre | GitHub |
|--------|--------|
| David Alvarez | [@misteralva](https://github.com) |

---

## Asignatura

**2509_ASIX_0370** — Planificació i administració de xarxes   
**Cicle:** CFGS Administració de Sistemes Informàtics en Xarxa - Ciberseguridad (ASIX)
