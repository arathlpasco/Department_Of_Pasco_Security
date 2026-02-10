# Department of Pasco Security (DPS) Home Lab

## 🚀 Project Overview
A simulated enterprise security environment built on **Apple Silicon (M2)** using **VMware Fusion**. This project demonstrates the deployment of a dual-homed **OPNsense** firewall protecting a **Windows 11 Pro** internal segment, providing a sandbox for network security testing, traffic analysis, and firewall policy management.

---

## 🛠️ Infrastructure & Isolation Strategy

* **Dedicated Lab Environment**: To maintain system hygiene and protect academic data, I created a **dedicated macOS User Account** specifically for this project. This ensures that virtualization experiments and large VM files remain isolated from my primary school documents and personal files.
* **Hypervisor**: VMware Fusion 13.x (Pro).
* **Gateway/Firewall**: OPNsense 24.x (Architecture: arm64/aarch64).
* **Internal Client**: Windows 11 Pro on ARM.
* **Storage**: All Virtual Machine assets are hosted on an external **2TB SSD** to manage disk space and improve performance.

---

## 🔧 Technical Milestones

### **A. Hypervisor Provisioning**
Installed VMware Fusion to facilitate local virtualization on macOS. I configured custom virtual network segments to ensure a "Dual-Homed" architecture, separating the lab's internal traffic from the host machine's network.

### **B. Storage Engineering: Disk Conversion**
The OPNsense raw image required conversion to be compatible with VMware Fusion. I utilized the `qemu-img` utility via the macOS Terminal to transform the image into a VMware-recognized format.
* **Command**: `qemu-img convert -f raw -O vmdk [source_image].img OPNsense-DPS-Core.vmdk`
* **Troubleshooting**: Resolved a "No Operating System Found" error by manually adjusting the Virtual Disk Bus type from **NVMe to SATA** in the VM settings to ensure the FreeBSD kernel could initialize the storage.

### **C. Dual-Homed Network Architecture**
Provisioned two distinct virtual network interface cards (vNICs) to establish the "Two-Door" security model:
1. **WAN (em0)**: Set to **Bridged Mode** to provide the firewall with internet access via the host's Wi-Fi.
2. **LAN (em1)**: Mapped to a **Custom Host-Only Network** (`DPS_INTERNAL_NETWORK\`), creating a private segment isolated from the internet for internal assets.

### **D. Gateway Logic & DHCP Configuration**
Initialized the OPNsense kernel and performed the manual interface assignment.
* **Static Gateway**: Established the internal gateway at **`10.0.50.1/24`**.
* **DHCP Scope**: Configured a stateful DHCP pool ranging from **`10.0.50.100` to `10.0.50.200`** to automate addressing for internal clients.
* **Security**: Generated a **Self-Signed SSL Certificate** for the Web GUI to ensure encrypted management access from the internal segment.

---

## 🏛️ Project Reflection & Troubleshooting
During Phase 1, I successfully navigated several "real-world" IT hurdles:
* **HID Redirection**: Resolved console input issues by utilizing specific scancodes (`fn + Return`) to communicate with the virtual hardware.
* **Cold Boot Syncing**: Identified that VMware hardware changes (like adding vNICs) require a full VM shutdown to be recognized by the OPNsense kernel.
* **Protocol Validation**: Corrected a configuration loop by identifying and resolving an input error where IPv4 parameters were erroneously entered into IPv6 fields.
