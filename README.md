# 🛡 Department of Pasco Security (DPS) Home Lab

![Status](https://img.shields.io/badge/Status-Foundation%20Phase-blue)
![Build](https://img.shields.io/badge/Build-Preparation-yellow)
![Focus](https://img.shields.io/badge/Current%20Focus-Help%20Desk%20%26%20Core%20IT-informational)
![Platform](https://img.shields.io/badge/Platform-Apple%20Silicon%20M2-lightgrey)
![Hypervisor](https://img.shields.io/badge/Hypervisor-VMware%20Fusion%2013.x-critical)

---

## 🚀 Project Overview

The **Department of Pasco Security (DPS)** is a long-term enterprise security simulation project designed to model real-world infrastructure, network segmentation, and security operations.

The original implementation began with a dual-homed **OPNsense** firewall protecting a **Windows 11 Pro** internal segment on Apple Silicon (M2).

After initial deployment, I intentionally paused expansion of DPS to strengthen foundational IT competencies (Help Desk operations, Active Directory fundamentals, networking basics).

DPS will resume structured infrastructure expansion once defined readiness milestones are met.

This repository serves as:

- Architectural blueprint  
- Technical reference documentation  
- Long-term enterprise roadmap  

---

# 🧱 Current Development Phase

### Foundation Development Phase (Active)

I am currently building operational discipline through:

- Help Desk troubleshooting documentation
- Active Directory user lifecycle practice
- Networking fundamentals (IP, DNS, DHCP)
- Structured ticketing workflows
- Database-backed IT asset modeling
- Foundational systems programming

This ensures DPS evolves from competence — not configuration experimentation.

---

# 🧭 DPS Enterprise Architecture Vision

The DPS lab utilizes a **Zone-Based Security Model** to isolate untrusted traffic from internal assets.

Below is the original logical topology.

<p align="center">
<img width="443" height="697" alt="DPS_Network_Topology drawio" src="https://github.com/user-attachments/assets/4da46afe-7670-4e38-aa9a-9a6f49ee2d2f" />
</p>

---

# 🔮 Future Architecture Expansion (Planned)

            [ External Network ]
                    |
               [ Firewall ]
                    |
          -----------------------
          |                     |
   [ Domain Controller ]   [ Linux Server ]
          |                     |
    [ Windows Clients ]    [ Log Server ]
          |
     [ Database Server ]

Planned enhancements:

- VLAN segmentation
- Centralized logging
- SIEM integration
- SOC workflow simulation
- Threat detection lab scenarios

---

# 📊 Measurable Milestones for DPS Reactivation

DPS infrastructure build will resume once the following competencies are completed and documented:

---

## 🎯 Milestone 1 – Help Desk Operational Readiness

- 20+ fully documented troubleshooting tickets
- Active Directory user creation → modification → deprovision lifecycle completed
- Group Policy Object configured and tested
- Root cause analysis written for at least 5 incidents

---

## 🎯 Milestone 2 – Networking Mastery

- Subnetting exercises completed and documented
- Static vs DHCP configuration validated in lab
- DNS resolution path documented
- Packet capture analysis performed and explained
- Successful LAN isolation validation test

---

## 🎯 Milestone 3 – Systems & Data Layer

- Functional MySQL IT asset schema implemented
- Ticket lifecycle modeled in database
- 10+ meaningful SQL queries written and explained
- Database integrated into internal lab workflow

---

Once all milestones are met:

Foundation Phase → Infrastructure Core Build Phase

---

# 🛠 Infrastructure & Isolation Strategy

- Dedicated macOS user account for lab containment
- VMware Fusion 13.x (Pro) hypervisor
- OPNsense 24.x (arm64)
- Windows 11 Pro (ARM)
- External 2TB SSD for VM storage isolation
- Manual network segmentation configuration

---

# 🔧 Technical Milestones Achieved (Phase 0 Build)

### A. Hypervisor Provisioning
Configured VMware Fusion with custom network segments to support dual-homed firewall deployment.

### B. Storage Engineering
Converted OPNsense raw image using:

qemu-img convert -f raw -O vmdk [source_image].img OPNsense-DPS-Core.vmdk

Resolved NVMe incompatibility by switching Virtual Disk Bus to SATA.

### C. Dual-Homed Network Architecture
Provisioned:

- WAN (Bridged Mode)
- LAN (Custom Host-Only Network)

Implemented internal gateway: 10.0.50.1/24  
Configured DHCP scope: 10.0.50.100 – 10.0.50.200

### D. Secure Management
Generated self-signed SHA256 SSL certificate for encrypted Web GUI access.

---

# 📂 Phase Documentation

<details>
<summary><b>Click to expand: Phase 0 – Firewall & Initial Segmentation Build</b></summary>

(Insert your detailed technical documentation here.)

</details>

---

# 🏛 Engineering Philosophy

- Foundations before complexity
- Enterprise security requires operational maturity
- Document before advancing
- Build intentionally, not reactively

---

# 📌 Project Status

Current Phase: Foundation Development  
Next Activation Target: Networking Milestone Completion  

DPS expansion resumes upon milestone validation.

---

Enterprise security built on operational discipline — not configuration shortcuts.
