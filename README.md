# Secure SOHO Network Lab (Cisco Packet Tracer)

This lab simulates a **Secure Small Office/Home Office (SOHO)** network built using Cisco Packet Tracer. It was designed as a foundational project to demonstrate core skills in **network segmentation**, **device hardening**, **DHCP configuration**, **basic access control**, and **secure switching practices**, all of which support the career path toward becoming a **Network Security Engineer**.

## 🛠️ Lab Objectives

- Design a logically segmented network using VLANs
- Secure Layer 2 infrastructure (switches and access ports)
- Implement DHCP for IP address management
- Apply port security and trunk configurations
- Use SNMP and storm control for basic monitoring and traffic suppression
- Demonstrate early implementation of security best practices in a simulated environment

---

## 📦 Network Overview

| VLAN | Purpose       | Subnet              | Gateway IP        |
|------|---------------|---------------------|-------------------|
| 10   | Admin Devices | 192.168.10.0/24     | 192.168.10.254    |
| 20   | Guest Network | 192.168.20.0/24     | 192.168.20.254    |
| 180  | Wireless APs  | 192.168.180.0/24    | 192.168.180.254   |
| 99   | Native VLAN   | 192.168.99.0/24     | 192.168.99.254    |

---

## 🔐 Security Measures Implemented

### ✅ VLAN Segmentation
- Devices are separated logically into VLANs to reduce broadcast domains and limit lateral movement.
- VLAN 99 is used as the **native VLAN**, with no user traffic assigned to VLAN 1 (disabled where possible).

### ✅ Switch Port Security
- **Access ports** are locked down with `port-security`, using sticky MACs and a maximum of one device.
- Violation mode set to `restrict` to prevent shutdowns while still logging anomalies.

### ✅ Trunk and Native VLAN Consistency
- Trunk ports between switch and router are configured with consistent native VLANs to avoid CDP errors.
- Subinterfaces are used on the router to handle inter-VLAN routing.

### ✅ DHCP Configuration
- The router is configured to act as a DHCP server, providing IPs per VLAN.
- Excluded IPs are reserved for infrastructure devices like switches and APs.

### ✅ SNMP (Read-Only)
- Basic SNMP community string configured (`public` RO) for future integration with network monitoring tools.

### ✅ Storm Control
- Broadcast, multicast, and unicast storm control applied to mitigate broadcast storms and denial-of-service attacks.

### ✅ Errdisable Recovery Procedures
- Manual recovery demonstrated for interfaces shut down due to security violations, including removal of sticky MACs.

---

## 🧪 Testing & Verification

- Each endpoint was configured to obtain an IP via DHCP.
- Trunk links tested for native VLAN alignment.
- Ping tests verified routing across VLANs.
- Switch logs reviewed for violations and CDP alerts.
- Port security behavior validated using MAC spoofing and limit testing.

---

## 🧭 Future Enhancements

- Apply ACLs to filter inter-VLAN traffic (e.g., restrict Guest access to Admin VLAN)
- Integrate AAA using RADIUS/TACACS+ for device login control
- Configure remote logging and syslog export
- Simulate SNMP trap destinations for alerting
- Add redundant links with STP (Spanning Tree Protocol)

---

## 🧑‍💻 Author

**Ziggy Dolce**  
Aspiring Network Security Engineer  
Studying for CompTIA Network+ and Azure/AWS Cloud Security  
GitHub: Yggiz  
LinkedIn: https://linkedin.com/in/ziggy-dolce

---

## 📝 License

This project is for educational use only
