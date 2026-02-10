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

## 📂 Project Phases

<details>
<summary><b>Click to expand: Phase 1 - Windows 11 Pro Workstation and OPNsense Installation (Technical Documentation)</b></summary>

# Technical Documentation: Phase 1 - Windows 11 Pro Workstation and OPNsense Installation
**Project:** Department of Pasco Security (DPS) Home Lab  
**Author:** Arath Luis Pasco  
**Date:** February 9, 2026  

---

## 1. Workstation & System Isolation
To maintain system hygiene and protect academic data, a dedicated macOS User Profile was established for this project. This ensures virtualization experiments remain isolated from primary system resources and prevents accidental interference with personal school files.

### **Storage Strategy**
All assets are persistent on an external **2TB SSD**, formatted for optimal Apple Silicon throughput and encrypted for data security.

<img width="1470" height="956" alt="Arath_Driver_Format" src="https://github.com/user-attachments/assets/31248132-085a-4b3e-9528-939eff5ac17f" />

---

## 2. Image Preparation & Virtual Disk Engineering
The OPNsense raw image was converted to a VMware-compatible format (`.vmdk`) using the `qemu-img` utility.

### **Hardware Compatibility Fix**
Initial boot failed due to a driver mismatch with the default NVMe bus. I reconfigured the Virtual Disk Bus to **SATA** to ensure the FreeBSD kernel could initialize the storage successfully.

---

## 3. Network Architecture: The "Two-Door" Model
The lab utilizes a dual-homed configuration to isolate internal traffic from the internet.

| Interface | VMware Setting | Purpose |
| :--- | :--- | :--- |
| **WAN (em0)** | Bridged (Autodetect) | Internet uplink via host Wi-Fi. |
| **LAN (em1)** | Custom (`DPS_INTERNAL_NETWORK\`) | Private gateway for internal assets. |

### **Virtual Hardware Configuration**
The following screenshots confirm the precise mapping of the virtual network adapters and subnets within VMware Fusion.

**Adapter 1 (WAN - em0)** 

<img width="1920" height="1080" alt="FreeBSD_14_Network_Adapter_Config" src="https://github.com/user-attachments/assets/c47e374a-06d1-4e2e-aeab-a8005b94fa6c" />

**Adapter 2 (LAN - em1)** 

<img width="1920" height="1080" alt="FreeBSD_14_Network_Adapter_2_Config" src="https://github.com/user-attachments/assets/6ed8d11c-1e2e-4d8c-abd8-d51849cdbf9a" />

**Hypervisor Level Network (Subnet Definition)**

<img width="1920" height="1080" alt="DPS_Internal_Network_Config" src="https://github.com/user-attachments/assets/89dc12e5-2287-4b02-b6c0-85c9e0fdfc23" />


---

## 4. Console Configuration & Implementation
The OPNsense kernel was initialized through the terminal console. I successfully transitioned the LAN from the factory default (`192.168.1.1`) to the custom lab subnet (`10.0.50.1`).

**Interface Overview (Post-Provisioning)** 

<img width="1470" height="956" alt="OPNsense_LAN_WAN" src="https://github.com/user-attachments/assets/940c9fd7-06e9-4406-aaf9-234c897f381a" />

### **Key Console Commands & Security**
* **DHCP Scope**: Established a stateful range of `10.0.50.100` to `10.0.50.200`.
* **Encryption**: Generated a self-signed SHA256 SSL certificate to secure management traffic via HTTPS.

---

## 5. Client Integration (Windows 11)
The internal Windows 11 client was bridged to the `DPS_INTERNAL_NETWORK\`. Upon rebooting, the client successfully "handshaked" with the OPNsense gateway via DHCP.

**Client Network Verification (ipconfig)** 

<img width="1920" height="1080" alt="Worl_Station_Conection_DPS_INTERNAL_NETWORK" src="https://github.com/user-attachments/assets/8e90b9bb-c2ef-418e-868c-daf7231c3665" />

*(Note: Final validation confirmed the transition to the 10.0.50.100+ range after the OPNsense DHCP service was fully initialized.)*

---

## 🏛️ Project Reflection & Troubleshooting
During Phase 1, I successfully navigated several "real-world" IT hurdles:
* **HID Redirection**: Resolved console input issues by utilizing specific scancodes (`fn + Return`) to communicate with the virtual hardware.
* **Cold Boot Syncing**: Identified that VMware hardware changes (like adding vNICs) require a full VM shutdown for the OPNsense hardware abstraction layer to recognize the new device.
* **Protocol Validation**: Corrected a configuration loop by identifying and resolving an input error where IPv4 parameters were erroneously entered into IPv6 fields.

</details>
