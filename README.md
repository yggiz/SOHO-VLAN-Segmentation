# SOHO-Network-Design
Simulate a Small Office/Home Office network using routing, VLANs, and basic ACLs for segmenting traffic.

# SOHO Network Design using Cisco Packet Tracer

## Overview
This project simulates a small office/home office (SOHO) netwotk using Cisco Pavket Tracer . It includes VLAN segmentation, static routing, and access control lists (ACLs) for basic security and traffic control. 

## Topology Diagram
![SOHO Topology](diagram/soho_network.drawio) 

## IP Subnets
| Device     | Interface           | IP Address   | VLAN   |
|------------|---------------------|--------------|--------|
| Router     | G0/0.10             | 192.168.10.1 | 10     |
| Router     | G0/0.20             | 192.168.20.1 | 20     |
| Switch     | VLAN1               | 192.168.1.2  | Mgmt   |
| Admin PC   | FasterEthernet0     | 192.168.10.2 | 10     |
| Guest PC   | FasterEthernet1     | 192.168.20.2 | 20     |

## Features Implemented 
- VLAN Configuration (Admin and Guest)
- Inter-VLAN Routing via Router-on-a-Stick
- Static Routing
- ACLs to block Guest VLAN from accessing Admin VLAN
- Ping tests and routing table verifications

## Sample Configs
See the 'configs/' folder for full configuration files.

## Testing
- Verified ping between Admin devices
- Confirmed Guest devices cannot access Admin resources
- Routing tables show correct routes for each subnet 
