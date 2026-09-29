# AlwaysOn Tech — Cisco Enterprise Network

![Cisco](https://img.shields.io/badge/Cisco-Packet%20Tracer-blue?logo=cisco)
![OSPF](https://img.shields.io/badge/Routing-OSPF-green)
![HSRP](https://img.shields.io/badge/Redundancy-HSRP-orange)
![VTP](https://img.shields.io/badge/VLANs-VTP-purple)

> Design and implementation project of a complete enterprise network for the fictional company **AlwaysOn Tech**, built in Cisco Packet Tracer as part of the subject *2509_ASIX_0370 — Network Planning and Administration (Planificació i administració de xarxes) — 1PM HOSPITALET*.

**Author:** David Alvarez

---

## Description

AlwaysOn Tech is an IT consulting company based in a two-floor office. The network serves five internal departments and external consultants who connect remotely over the Internet.

The goal of the project was to design and implement an enterprise network infrastructure with **full redundancy**, **per-department security** and complete **corporate services**.

---

## Topology

```
Internet (ISP1/ISP2)
        |
   [R0] — [R1]          ← Edge routers running OSPF
    |  \  / |
  Core0 — Core1         ← L3 core switches with HSRP
    |         |
  Dist0 — Dist1         ← Distribution switches
    |         |
  SW-RRHH  SW-VENTAS  SW-OPER  SW-FINANZ  SW-IT  SW-SERVERS

Consultants → Router-ISP1 → R0 → Internal network
```

> Device names (`SW-RRHH`, `SW-VENTAS`, `SW-OPER`, `SW-FINANZ`, `SW-IT`, `SW-SERVERS`) match the names used in the Packet Tracer file: HR, Sales, Operations, Finance, IT and Servers.

---

## Technologies and protocols

| Layer | Protocol/Technology | Function |
|-------|---------------------|----------|
| Routing | OSPF area 0 | Dynamic routing between routers and Cores |
| L3 redundancy | HSRP | Virtual IP shared between Core0 and Core1 |
| VLAN management | VTP | Automatic VLAN propagation from Core0 |
| L2 redundancy | STP | Loop prevention on access switches |
| IP translation | NAT/PAT | Internet access from the internal network |
| Security | Extended ACLs | Isolation between departments |
| Remote access | SSH v2 | Secure device administration |
| Time sync | NTP | Unified clock across all devices |
| Logging | Syslog | Centralized logs on SRV-SYSLOG |
| IP assignment | DHCP | Automatic IPs per department |
| Name resolution | DNS | Internal name resolution |

---

## Repository structure

```
AlwaysOnTech-Red-Empresarial/
│
├── README.md                          ← This file
├── AlwaysOnTech.pkt                   ← Cisco Packet Tracer simulation
│
├── documentacion/
│   ├── AlwaysOnTech_documento_completo.docx     ← Project report
│   ├── AlwaysOnTech_guia_comandos_completa.docx ← Full command guide
│   └── AlwaysOnTech_VLSM_v2.xlsx                ← IP addressing tables
```

---

## IP addressing

| Zone | Range | Function |
|------|-------|----------|
| WAN ISP1 | 200.0.0.0/30 | Router-ISP1 ↔ R0 link |
| WAN ISP2 | 201.0.0.0/30 | Router-ISP2 ↔ R1 link |
| LAN ISP1 | 172.16.0.0/29 | Internal ISP1 network and consultants |
| LAN ISP2 | 172.16.1.0/29 | Internal ISP2 network |
| Consultants | 192.168.100.0/29 | Private consultant network (VPN) |
| Infrastructure | 10.0.0.0/8 | L3 links between routers and Cores |
| Users | 192.168.0.0/16 (VLSM) | Department and service VLANs |

### User VLANs

| VLAN | Name | Subnet | HSRP Gateway |
|------|------|--------|--------------|
| VLAN 10 | HR | 192.168.0.216/29 | 192.168.0.217 |
| VLAN 20 | Sales | 192.168.0.192/28 | 192.168.0.193 |
| VLAN 30 | Operations | 192.168.0.208/29 | 192.168.0.209 |
| VLAN 40 | Finance | 192.168.0.224/29 | 192.168.0.225 |
| VLAN 50 | IT | 192.168.0.232/29 | 192.168.0.233 |
| VLAN 60 | Servers | 192.168.0.176/28 | 192.168.0.177 |
| VLAN 70 | VoIP | 192.168.0.240/29 | 192.168.0.241 |
| VLAN 80 | Corporate WiFi | 192.168.0.0/26 | 192.168.0.1 |
| VLAN 90 | Guest WiFi | 192.168.0.128/27 | 192.168.0.129 |
| VLAN 99 | Management | 192.168.0.160/28 | 192.168.0.161 |
| VLAN 100 | Consultants VPN | 192.168.0.64/26 | 192.168.0.65 |
| VLAN 999 | Ghost (native) | — | — |

---

## Implemented features

- [x] **OSPF** — Dynamic routing in area 0 with unique router IDs
- [x] **HSRP** — Gateway redundancy on all 11 user VLANs
- [x] **VTP** — VLAN propagation from Core0 to all switches
- [x] **STP** — Redundancy management with PortFast and BPDU Guard on access ports
- [x] **NAT/PAT** — IP translation on R0 and R1 towards ISP1 and ISP2
- [x] **ACLs** — Isolation between departments with controlled access to servers
- [x] **DHCP** — Per-VLAN pools with ip helper-address on Core0
- [x] **DNS** — Internal name resolution (alwaysontech.local)
- [x] **FTP** — File server with three user levels
- [x] **HTTP** — Corporate intranet accessible from all departments
- [x] **SSH v2** — Secure remote administration on all devices
- [x] **NTP** — Centralized clock synchronization
- [x] **Syslog** — Centralized logging for all devices
- [x] **TFTP** — Configuration backups
- [x] **VoIP** — VLAN 70 with IP phone support
- [x] **WiFi** — Separate VLANs for employees and guests
- [x] **DHCP Snooping** — Protection against rogue DHCP servers
- [x] **ISP redundancy** — Two Internet providers (ISP1 and ISP2)
- [x] **External consultants** — Controlled access to FTP and intranet

---

## Known limitations of Cisco Packet Tracer

During the implementation we ran into the following limitations of the simulation tool:

1. **Port-security sticky** — Saved MAC addresses are lost or change when the simulation is restarted. The correct configuration is documented, but it does not persist between sessions.

2. **ACLs on Catalyst 3650 SVIs** — The `show ip interface vlan X` command does not show the applied ACLs even though they work. `show running-config | include access-group` does not show them either, but the traffic is filtered correctly.

3. **HSRP virtual IP does not answer ping** — The HSRP virtual IP does not reply to pings from devices in the same VLAN. This is a known Packet Tracer behavior.

4. **Multi-hop NAT** — Pinging the Internet (Router-ISP) from internal PCs returns request timed out even though NAT translations are created correctly. This is a simulator limitation with NAT across multiple hops.

5. **VTP revision number** — After restarting the simulation, some switches may end up with a higher VTP revision number than Core0 and refuse its updates. Fix: temporarily change the VTP mode to transparent and then back to client.

6. **DHCP pools with HSRP** — Packet Tracer's DHCP server uses the real IP of the relay agent (Core0 SVI) to select the pool, not the HSRP virtual IP. Pool gateways must match Core0's real IP on each VLAN.

---

## Author

| Name | GitHub |
|------|--------|
| David Alvarez | [@misteralva](https://github.com/misteralva) |

---

## Subject

**2509_ASIX_0370** — Network Planning and Administration (*Planificació i administració de xarxes*)
**Program:** CFGS in Network Systems Administration – Cybersecurity (ASIX)
