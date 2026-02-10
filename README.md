# 🛡️ Department of Pasco Security (DPS) Home Lab

![Status](https://img.shields.io/badge/Status-Active-success) ![Platform](https://img.shields.io/badge/Platform-VMware%20Fusion-blue) ![OS](https://img.shields.io/badge/OS-OPNsense%20%7C%20FreeBSD-red)

**Author:** Arath Luis Pasco  
**Date:** February 9, 2026  

---

## 1. Project Overview & System Isolation
To ensure academic integrity and system hygiene, this project utilizes a strict **Host-Guest Isolation Strategy**. A dedicated macOS User Profile was established to quarantine virtualization experiments from primary system resources and personal files.

### **Storage Infrastructure**
*   **Medium:** External 2TB SSD.
*   **Format:** APFS (Encrypted) for optimal Apple Silicon throughput and data security.

> **Note:** All virtual machine assets are persistent on the external volume to prevent internal drive wear and ensure portability.

![Disk Utility formatting the LaCie drive as APFS Encrypted](Images/Arath_Driver_Format.png)

---

## 2. Virtual Engineering & Preparation

### **Image Provisioning**
The OPNsense raw image was converted to the VMware-native `.vmdk` format using the `qemu-img` utility to ensure hypervisor compatibility.

### **Hardware Compatibility Patch**
**Issue:** Initial boot failure caused by a driver mismatch with the default NVMe controller.
**Resolution:** Reconfigured the Virtual Disk Bus to **SATA**, allowing the FreeBSD kernel to successfully initialize the storage volume.

---

## 3. Network Architecture: Dual-Homed Perimeter
The lab implements a **Dual-Homed** configuration to physically segregate WAN traffic from the private internal network.

| Interface | VMware Mapping | Subnet Designation | Function |
| :--- | :--- | :--- | :--- |
| **WAN (em0)** | Bridged (Autodetect) | `DHCP (ISP)` | Internet uplink via host Wi-Fi. |
| **LAN (em1)** | Custom VNet | `10.0.50.1/24` | Private gateway for `DPS_INTERNAL_NETWORK`. |

### **Virtual Hardware Validation**

**1. Perimeter Uplink (WAN - em0)**  
*Bridge mode allows the firewall to pull a valid IP directly from the host network.*  
![VMware settings showing Network Adapter 1 set to Bridged Autodetect](Images/FreeBSD_14_Network_Adapter_Config.png)

**2. Internal Gateway (LAN - em1)**  
*Strict traffic isolation is enforced via a custom virtual switch.*  
![VMware settings showing Network Adapter 2 set to Custom DPS_INTERNAL_NETWORK](Images/FreeBSD_14_Network_Adapter_2_Config.png)

**3. Hypervisor Subnet Definition**  
*The logical definition of the private 10.0.50.0/24 block.*  
![VMware Network Preferences showing DPS_INTERNAL_NETWORK defined as 10.0.50.0](Images/DPS_Internal_Network_Config.png)

---

## 4. Console Configuration
The OPNsense kernel was initialized via the serial console, transitioning the LAN interface from the factory default (`192.168.1.1`) to the custom lab subnet.

![OPNsense console showing LAN (em0) successfully set to 10.0.50.1/24](Images/OPNsense_LAN_WAN.png)

### **Security & Services**
*   **DHCP Scope:** Established a stateful lease pool: `10.0.50.100` – `10.0.50.200`.
*   **Management Encryption:** Generated a self-signed **SHA256 SSL certificate** to enforce HTTPS for web administration.

---

## 5. Client Integration (Windows 11)
The Windows 11 client was bridged to the `DPS_INTERNAL_NETWORK`. Upon boot, the client successfully negotiated a DHCP lease with the OPNsense gateway.

**Verification:**  
![Windows 11 terminal showing initial assigned IP](Images/Worl_Station_Conection_DPS_INTERNAL_NETWORK.png)  
*(Validated transition to the 10.0.50.x range post-initialization.)*

---

## 🏛️ Project Reflection & Troubleshooting

### **Phase 1: Key Challenges Resolved**
| Challenge | Technical Solution |
| :--- | :--- |
| **HID Redirection** | Utilized specific scancodes (`fn + Return`) to force console input capture within the virtual environment. |
| **Hardware Abstraction** | Identified that vNIC additions require a **Cold Boot** (Full Shutdown) for the FreeBSD HAL to register new devices. |
| **Protocol Validation** | Debugged a configuration loop caused by erroneously entering IPv4 parameters into an IPv6 field. |
