# Cisco Packet Tracer: VLAN Segmentation & Device Hardening

This implementation focuses on micro-segmentation using 802.1Q VLAN tagging to isolate student and faculty traffic across a multi-switch topology, alongside fundamental Cisco IOS device hardening.

## Network Topology
<img width="581" height="637" alt="Screenshot 2026-06-21 203123" src="https://github.com/user-attachments/assets/0d92c8a8-bdb0-48e2-bceb-bfc3a0752fdf" />


## Addressing Table
<img width="467" height="247" alt="Screenshot 2026-06-21 202016" src="https://github.com/user-attachments/assets/f086fd18-bad9-489b-99e7-53fb7c7cafaa" />

---

## Project Objectives & Architecture
* **Traffic Isolation (Layer 2):** Created VLAN 64 (`STUDENTS`) and VLAN 128 (`FACULTY`) to structurally segment network broadcast domains.
* **802.1Q Trunking:** Configured IEEE 802.1Q trunk links between switches (`Fa0/2`) and upward to the router interface (`Fa0/3`) to support multi-VLAN data streams over a single physical connection.
* **Infrastructure Hardening:** Implemented strict AAA baselines, including encrypted line security and absolute credential storage.

---

## Device Configurations

### Switch S1 Configuration
```text
! Basic Security Hardening
hostname S1
no ip domain-lookup
enable secret class
banner motd "Unauthorized Access is Forbidden"

! Line Management & AAA Baselines
line console 0
 password cyber
 login
exit
line vty 0 4
 password cyber
 login
exit
service password-encryption

! Interface Assignments & Trunking
interface fastethernet 0/3
 description Connection to Router
 switchport mode trunk
exit
interface fa0/2
 switchport mode trunk
exit

interface fa0/5
 switchport mode access
 switchport access vlan 64
 no shutdown
exit
interface fa0/10
 switchport mode access
 switchport access vlan 128
 no shutdown
exit

! SVI Management
interface vlan 1
 description Management IP
 ip address 200.10.25.194 255.255.255.192
 no shutdown
```
### Switch S2 Configuration
```text
hostname S2
no ip domain-lookup
enable secret class
banner motd "Unauthorized Access is Forbidden"

line console 0
 password cyber
 login
exit
line vty 0 4
 password cyber
 login
exit
service password-encryption

interface fa0/2
 switchport mode trunk
exit

interface fa0/5
 switchport mode access
 switchport access vlan 64
 no shutdown
exit
interface fa0/10
 switchport mode access
 switchport access vlan 128
 no shutdown
exit

interface vlan 1
 description Management IP
 ip address 200.10.25.195 255.255.255.192
 no shutdown
```
