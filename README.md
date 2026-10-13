
# SOHO Network Lab — Cisco Packet Tracer

A complete Small Office / Home Office (SOHO) network built in Cisco Packet Tracer, 
demonstrating real-world IT support and network administration skills.

![Network Topology](topology/soho-network-topology.png)

---

## 📖 Project Overview

This project simulates a small business network — the kind you'd find in a clinic, 
law office, retail shop, or branch location. It includes every feature a real SOHO 
network needs:

- Segmented VLANs for Staff, Guest, and Management traffic
- Inter-VLAN routing via Router-on-a-Stick
- Wireless access points mapped to specific VLANs
- Centralized DHCP from the Cisco router
- Secure remote management over SSH v2
- Layer 2 hardening with port security
- Configuration backup and disaster recovery via TFTP
- Time synchronization using NTP
- Centralized logging with Syslog

Built as part of ongoing IT support and networking skill development.

---

## 🎯 Skills Demonstrated

| Skill Area | What Was Implemented |
|------------|---------------------|
| **Network Design** | Multi-VLAN topology with trunking and 802.1Q |
| **Inter-VLAN Routing** | Router-on-a-Stick with sub-interfaces |
| **IP Services** | Per-VLAN DHCP scopes, SVIs, default gateways |
| **Wireless** | Separate SSIDs mapped to Staff and Guest VLANs |
| **Security** | SSH v2, RSA keys, ACL-restricted VTY access, port security |
| **Operations** | NTP time sync, Syslog centralized logging |
| **Disaster Recovery** | TFTP config backup and full restore workflow |
| **Documentation** | Topology diagram, IP plan, VLAN plan, setup guide |

---

## 🏗️ Network Design

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

### Physical Topology
### Physical Topology

```
                    [ISP / WAN]
                         |
                  [Router ISR4321]
                         |
                  Gig0/0/0 (Trunk)
                         |
            +------------+------------+
            |                         |
       [Switch 1] ---- Trunk ---- [Switch 2]
       (SW1-Staff)                 (SW2-Guest)
            |                         |
    +-------+-------+           +-----+-----+
    |       |       |           |     |     |
 Server  Staff PCs AP-1       Guest  PC3   AP-2
 Printer            (Staff)     PC          (Guest)
```
---

## 🛠️ Equipment Simulated

| Device | Model | Role |
|--------|-------|------|
| Router | Cisco ISR4321 | Inter-VLAN routing, DHCP, NTP client, Syslog client |
| Switch 1 | Cisco 2960-24TT | Staff access layer, trunk to router and SW2 |
| Switch 2 | Cisco 2960-24TT | Guest access layer, trunk to SW1 |
| Access Points | Cisco AP-PT | Wireless for Staff and Guest SSIDs |
| Server | Server-PT | TFTP, NTP, Syslog, DNS services |
| End Devices | PC-PT, Laptop-PT, Printer-PT | User devices |

---

## 🔧 Configuration Highlights

### Router-on-a-Stick
The router uses a single physical interface (`Gig0/0/0`) with three sub-interfaces 
for VLANs 10, 20, and 99 — each tagged with 802.1Q encapsulation.
### SSH Access
SSH v2 with RSA keys and local user authentication. VTY lines restricted 
with an ACL so only Staff VLAN hosts can connect.
### Port Security
Every access port allows exactly one MAC address (sticky learning) with 
err-disable enforcement on violation.
### TFTP Backup + Disaster Recovery
All three devices back up their running config to the TFTP server. The recovery 
workflow was tested by erasing SW2's startup-config, rebooting, and restoring 
the config from the TFTP server — proving the backup was usable.

### NTP Time Sync
All devices synchronize to the Server-PT NTP service, ensuring consistent 
timestamps across the network.
### Syslog Centralized Logging
All devices send logs to the Server-PT syslog service. Combined with NTP, 
this allows cross-device event correlation.
---

## 🧪 Verification & Testing

The following tests confirmed the network was fully functional:

| Test | Command | Result |
|------|---------|--------|
| Local gateway (Staff) | `ping 192.168.10.1` from Staff PC | ✅ Reply |
| Local gateway (Guest) | `ping 192.168.20.1` from Guest PC | ✅ Reply |
| Local gateway (Mgmt) | `ping 192.168.99.1` from Mgmt PC | ✅ Reply |
| Inter-VLAN routing | `ping 192.168.20.x` from Staff PC | ✅ Reply |
| SSH access (Staff) | `ssh -l admin 192.168.99.10` | ✅ Login succeeded |
| SSH access (Guest) | `ssh -l admin 192.168.99.10` | ❌ Blocked by ACL (as intended) |
| Port security violation | Swap PC on secured port | ✅ Port err-disabled |
| NTP sync | `show ntp status` | ✅ Clock is synchronized |
| Syslog reception | Server-PT Syslog panel | ✅ Logs from all 3 devices |
| TFTP restore | `copy tftp: running-config` | ✅ Full config restored |

---

## 📸 Screenshots

<img width="1728" height="1117" alt="Screenshot 2026-10-13 at 1 56 28 PM" src="https://github.com/user-attachments/assets/c1de3008-00f1-4378-a410-c679259aa81c" />


<img width="1728" height="1117" alt="Screenshot 2026-10-13 at 11 01 13 AM" src="https://github.com/user-attachments/assets/8fe04bd3-5411-41b3-9224-b80b6b844467" />


<img width="1728" height="1117" alt="Screenshot 2026-10-13 at 11 29 08 AM" src="https://github.com/user-attachments/assets/2210773f-268b-4eda-aa6e-679d0e1991f5" />


<img width="1728" height="1117" alt="Screenshot 2026-10-13 at 12 01 07 PM" src="https://github.com/user-attachments/assets/c8b668ab-9cd2-4fe5-910c-1562f31f3637" />


---

## 📂 Repository Structure
soho-network-lab/
├── README.md
├── packet-tracer/
│   └── soho-network.pkt
├── configs/
│   ├── SOHO-Router.cfg
│   ├── SW1-Staff.cfg
│   └── SW2-Guest.cfg
├── docs/
│   ├── ip-addressing.md
│   ├── vlan-plan.md
│   ├── setup-guide.md
│   └── troubleshooting.md
├── topology/
│   └── soho-network-topology.png
└── screenshots/
├── 01-topology.png
├── 02-ntp-synced.png
├── 03-syslog-entries.png
└── 04-disaster-recovery.png

## 🚀 How to Open This Lab

1.⁠ ⁠Install *Cisco Packet Tracer 8.x*
2.⁠ ⁠Clone this repo: git clone https://github.com/Ontobias/SOHO-NETWORK-LAB.git cd SOHO-NETWORK-LAB
3.⁠Open ⁠ packet-tracer/soho-network.pkt ⁠
4.⁠ ⁠Wait ~30 seconds for devices to boot
5.⁠ ⁠Test connectivity from any Staff PC:
ping 192.168.10.1
ping 192.168.20.1
ping 192.168.99.1

---

## 🎓 Certifications This Project Supports

- **CompTIA A+** — Networking fundamentals
- **CompTIA Network+** — VLANs, routing, DHCP, security, monitoring
- **CompTIA Security+** — ACLs, port security, hardening
- **Cisco CCNA** — Full coverage of switching and routing topics

---

## Author


**[OSCAR TOBIAS]**
- LinkedIn: https://www.linkedin.com/in/oscar-tobias-7821ba361/
- GitHub: [@Ontobias](https://github.com/Ontobias)
