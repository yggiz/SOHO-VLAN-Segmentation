# SOHO-VLAN-Segmentation

> **Isolating a Small Office/Home Office Network with Cisco VLANs**  
> Author: Ziggy Dolce | Portfolio Lab Project

## Overview

This lab implements **VLAN-based network segmentation** for a Small Office/Home Office (SOHO) environment using Cisco IOS. The design separates traffic into logical network segments to improve security, reduce broadcast domains, and enforce access boundaries — all on commodity Cisco hardware commonly found in small business deployments.

The project demonstrates how a single physical switch can host multiple isolated network segments, each with its own IP subnet and access policy, mirroring real-world small-enterprise network architecture.

---

## The Problem

In most SOHO and small office environments, all devices share a single flat network. A laptop, a NAS, a printer, IoT devices, and guest WiFi all live on the same subnet. This means:

- A compromised device can scan and reach every other device on the LAN
- A misconfigured IoT device can generate broadcast storms that degrade the whole network
- There is no security boundary between sensitive and untrusted devices

---

## The Solution

**VLAN segmentation** divides the single physical network into isolated logical segments. Inter-VLAN routing is controlled at the router/Layer 3 switch, giving centralized visibility into all traffic crossing security boundaries.

---

## Network Design

```
                    ┌─────────────┐
                    │     R1      │  Cisco Router
                    │             │  Inter-VLAN Routing (Router-on-a-Stick)
                    └──────┬──────┘
                           │ Trunk (802.1Q)
                    ┌──────┴──────┐
                    │    SW1      │  Cisco Catalyst 2960
                    │             │  Access + Trunk Ports
                    └──┬───┬───┬──┘
                       │   │   │
               ┌───────┘   │   └────────┐
               │           │            │
         ┌─────┴────┐ ┌────┴─────┐ ┌───┴──────┐
         │ VLAN 10  │ │ VLAN 20  │ │ VLAN 30  │
         │  Work    │ │  IoT     │ │  Guest   │
         └──────────┘ └──────────┘ └──────────┘
```

---

## VLAN & IP Addressing

| VLAN | Name        | Subnet             | Gateway        | Purpose                           |
|------|-------------|--------------------|----------------|-----------------------------------|
| 10   | Work        | 192.168.10.0/24    | 192.168.10.1   | Workstations, laptops, printers   |
| 20   | IoT         | 192.168.20.0/24    | 192.168.20.1   | Smart devices, cameras, sensors   |
| 30   | Guest       | 192.168.30.0/24    | 192.168.30.1   | Guest WiFi, untrusted devices     |

---

## Key Configurations

### VLAN Definition (Switch)
```cisco
vlan 10
 name Work
vlan 20
 name IoT
vlan 30
 name Guest
```

### Trunk Port (Switch → Router)
```cisco
interface GigabitEthernet0/1
 switchport mode trunk
 switchport trunk allowed vlan 10,20,30
```

### Access Ports
```cisco
interface FastEthernet0/1
 switchport mode access
 switchport access vlan 10

interface FastEthernet0/2
 switchport mode access
 switchport access vlan 20
```

### Sub-Interfaces (Router-on-a-Stick)
```cisco
interface GigabitEthernet0/0.10
 encapsulation dot1Q 10
 ip address 192.168.10.1 255.255.255.0

interface GigabitEthernet0/0.20
 encapsulation dot1Q 20
 ip address 192.168.20.1 255.255.255.0

interface GigabitEthernet0/0.30
 encapsulation dot1Q 30
 ip address 192.168.30.1 255.255.255.0
```

---

## Verification

```cisco
! Confirm VLANs are active
show vlan brief

! Verify trunk is carrying all VLANs
show interfaces trunk

! Check routing table for all three subnets
show ip route

! Test connectivity between VLANs (should succeed via router)
ping 192.168.20.1 source 192.168.10.1
```

---

## Security Considerations

- **IoT devices are isolated**: A compromised smart device cannot initiate connections to workstations
- **Guest VLAN is untrusted**: Guest devices can only reach the internet, not internal resources
- **ACLs can be added** to the router sub-interfaces to enforce stricter inter-VLAN policies (see [ACL-Segmentation-PoC](https://github.com/yggiz/ACL-Segmentation-PoC) for a full ACL implementation)

---

## MITRE ATT&CK Relevance

| Technique | ID | How VLANs Help |
|-----------|----|----------------|
| Network Service Discovery | T1046 | Limits scan visibility across VLANs |
| Lateral Movement | T1021 | Workstations and IoT on separate broadcast domains |
| Internal Spearphishing | T1534 | Guest VLAN cannot reach internal hosts |

---

## Tools Used

- **Cisco Packet Tracer** — Network simulation
- **Cisco IOS CLI** — Switch and router configuration

---

## Repository Structure

```
├── README.md
├── configs/
│   ├── R1_config.txt          # Router running configuration
│   └── SW1_config.txt         # Switch running configuration
└── diagrams/
    └── topology.png           # Network topology diagram
```

---

## Related Projects

- **[ACL-Segmentation-PoC](https://github.com/yggiz/ACL-Segmentation-PoC)** — Extends VLAN segmentation with extended ACLs for a healthcare environment
- **[Multi-Site-Enterprise-OSPF](https://github.com/yggiz/Multi-Site-Enterprise-OSPF)** — Scales routing concepts to a multi-site enterprise with OSPF

---

## Author

**Ziggy Dolce** — Network & Security Engineer  
Miami Dade College, B.S. Cybersecurity (May 2026)  
CompTIA Network+ · CompTIA Security+ · CCNA (in progress)  
dolce.ziggy12@gmail.com | [github.com/yggiz](https://github.com/yggiz)

---

## License

Shared for educational and portfolio purposes.
