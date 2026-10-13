
# SOHO Network Lab — Cisco Packet Tracer

A complete Small Office / Home Office (SOHO) network built in Cisco Packet 
Tracer, demonstrating real-world IT support and network administration skills.



## Project Overview

This project simulates a small business network — the kind you'd find in a 
clinic, law office, retail shop, or branch location. It includes every feature 
a real SOHO network needs:

- Segmented VLANs for Staff, Guest, and Management traffic
- Inter-VLAN routing via Router-on-a-Stick
- Wireless access points mapped to specific VLANs
- Centralized DHCP from the Cisco router
- Secure remote management over SSH v2
- Layer 2 hardening with port security
- Configuration backup and disaster recovery via TFTP
- Time synchronization using NTP
- Centralized logging with Syslog

---

##  Skills Demonstrated

| Skill Area | What Was Implemented |
|------------|---------------------|
| Network Design | Multi-VLAN topology with trunking and 802.1Q |
| Inter-VLAN Routing | Router-on-a-Stick with sub-interfaces |
| IP Services | Per-VLAN DHCP scopes, SVIs, default gateways |
| Wireless | Separate SSIDs mapped to Staff and Guest VLANs |
| Security | SSH v2, RSA keys, ACL-restricted VTY, port security |
| Operations | NTP time sync, Syslog centralized logging |
| Disaster Recovery | TFTP config backup and full restore workflow |
| Documentation | Topology diagram, IP plan, VLAN plan, setup guide |

---

##  Network Design

### VLAN Architecture

| VLAN | Name | Subnet | Purpose |
|------|------|--------|---------|
| 10 | Staff | 192.168.10.0/24 | Employee wired + wireless devices |
| 20 | Guest | 192.168.20.0/24 | Visitor devices (isolated) |
| 99 | Management | 192.168.99.0/24 | Network infrastructure + admin access |

### IP Addressing

| Device | Interface | IP Address | Purpose |
|--------|-----------|------------|---------|
| Router | Gig0/0/0.10 | 192.168.10.1 | Staff gateway |
| Router | Gig0/0/0.20 | 192.168.20.1 | Guest gateway |
| Router | Gig0/0/0.99 | 192.168.99.1 | Management gateway |
| Switch 1 | Vlan99 | 192.168.99.10 | SW1 management SVI |
| Switch 2 | Vlan99 | 192.168.99.11 | SW2 management SVI |
| Server-PT | FastEthernet0 | 192.168.10.8 | DHCP, TFTP, NTP, Syslog |

---



| Device | Model | Role |
|--------|-------|------|
| Router | Cisco ISR4321 | Inter-VLAN routing, DHCP, NTP/Syslog client |
| Switch 1 | Cisco 2960-24TT | Staff access layer |
| Switch 2 | Cisco 2960-24TT | Guest access layer |
| Access Points | Cisco AP-PT | Wireless for Staff and Guest |
| Server | Server-PT | TFTP, NTP, Syslog, DNS |
| End Devices | PC-PT, Laptop-PT, Printer-PT | User devices |

---



---

## Verification & Testing

| Test | Result |
|------|--------|
| Local gateway (Staff / Guest / Mgmt) | ✅ Reply |
| Inter-VLAN routing | ✅ Reply |
| SSH from Staff PC | ✅ Login succeeded |
| SSH from Guest PC | ❌ Blocked by ACL (as designed) |
| Port security violation | ✅ Port err-disabled |
| NTP sync | ✅ Clock is synchronized |
| Syslog reception | ✅ Logs from all 3 devices |
| TFTP restore | ✅ Full config restored |


 NTP Synchronized

 Syslog Centralized Logging


 Disaster Recovery — Config Restored from TFTP


---

