# SmartTech Network Infrastructure & Simulation Documentation

> **Project Author:** Odunuga Fatai Olayinka  
> **Simulation Environment:** Cisco Packet Tracer (`SmartTech_Network.pkt`)  
> **Primary Network Subnet:** `192.168.10.0/24`  
> **Default Gateway:** `192.168.10.1` (Cisco 1941 Router)  
> **Central Server:** `192.168.10.2` (Multi-Service Server Room)  

---

## 📋 Table of Contents
1. [Project Overview](#-project-overview)
2. [Network Architecture & Topology](#-network-architecture--topology)
   - [Physical & Logical Topology](#physical--logical-topology)
   - [Mermaid Network Topology Diagram](#mermaid-network-topology-diagram)
3. [IP Addressing Scheme](#-ip-addressing-scheme)
4. [Centralized Enterprise Network Services](#-centralized-enterprise-network-services)
   - [1. Dynamic Host Configuration Protocol (DHCP)](#1-dynamic-host-configuration-protocol-dhcp)
   - [2. Domain Name System (DNS)](#2-domain-name-system-dns)
   - [3. Hypertext Transfer Protocol (HTTP) Web Server](#3-hypertext-transfer-protocol-http-web-server)
   - [4. File Transfer Protocol (FTP) & Role-Based Access Control (RBAC)](#4-file-transfer-protocol-ftp--role-based-access-control-rbac)
   - [5. Email Services](#5-email-services)
   - [6. Smart Building & Internet of Things (IoT) Integration](#6-smart-building--internet-of-things-iot-integration)
5. [Verification & Connectivity Testing](#-verification--connectivity-testing)
6. [Troubleshooting & Resolved Issues](#-troubleshooting--resolved-issues)
7. [Repository & Project File Structure](#-repository--project-file-structure)
8. [Getting Started & Simulation Execution](#-getting-started--simulation-execution)
9. [Conclusion](#-conclusion)

---

## 🏢 Project Overview

The **SmartTech Network Simulation Project** is an end-to-end enterprise network design engineered to support the operational demands of **SmartTech**. Built using **Cisco Packet Tracer**, this simulation unifies core infrastructure routing, department-level switching, centralized server application services, and smart building automation into a single cohesive network framework.

### Core Objectives
* **Centralized Server Infrastructure:** Host all key network application services (DHCP, DNS, HTTP, FTP, Email, and IoT Management) on a dedicated central server (`192.168.10.2`).
* **Department Segmentation:** Connect three operational departments—**Admin**, **IT**, and **Management**—along with an **IoT Smart Infrastructure branch**.
* **Role-Based Access Control (RBAC):** Enforce privilege segregation for corporate file access on the FTP service.
* **Smart Office Automation:** Integrate smart sensors, smart locks, automated lighting, and CCTV monitoring into the enterprise network via wired switches and a wireless Home Gateway.

---

## 🌐 Network Architecture & Topology

### Physical & Logical Topology

The network employs a **Star Topology** structured around a central **Multilayer Switch** connected to dedicated departmental switches and an edge gateway router:

* **Edge Gateway Router (`192.168.10.1`):** A Cisco router acting as the default gateway for the entire enterprise.
* **Server Room (`192.168.10.2`):** Directly connected to the central switch, hosting all enterprise services.
* **Department Switches:** Three dedicated switches serving:
  * **Admin Department:** 4 Workstation PCs (`192.168.10.24`, etc.)
  * **IT Department:** 4 Workstation PCs / Laptops (`192.168.10.17`, etc.)
  * **Management Department:** 4 Workstation PCs / Laptops (`192.168.10.21`, etc.)
* **IoT Infrastructure Branch:** An IoT switch connected to wired smart devices (Motion Detector, Smart Lock, Smart Lighting, IP Camera) and a **Wireless Home Gateway** for wireless smart endpoints.

> [!NOTE]
> **Logical Architecture:** The network currently operates on a single flat `192.168.10.0/24` subnet without VLAN segregation. Inter-department communication occurs natively via the central switch layer, with routing managed via `192.168.10.1`.

![SmartTech Network Topology](assets/images/image1.jpeg)

### Mermaid Network Topology Diagram

```mermaid
flowchart TD
    subgraph WAN_Edge ["WAN Edge / Gateway"]
        R1["Cisco Router<br/><b>192.168.10.1</b><br/>Default Gateway"]
    end

    subgraph Core_Layer ["Core Switch Layer"]
        MLS["Central Multilayer Switch"]
    end

    subgraph Server_Room ["Server Room"]
        SRV["Central Multi-Service Server<br/><b>192.168.10.2</b><br/>(DHCP, DNS, HTTP, FTP, Mail, IoT)"]
    end

    subgraph Departments ["Department Access Layer"]
        SW_ADMIN["Admin Switch"]
        SW_IT["IT Switch"]
        SW_MGMT["MGMT Switch"]
        SW_IOT["IoT Switch"]

        PC_ADMIN["Admin Dept PCs (4)<br/><i>e.g., 192.168.10.24</i>"]
        PC_IT["IT Dept PCs/Laptops (4)<br/><i>e.g., 192.168.10.17</i>"]
        PC_MGMT["Management PCs (4)<br/><i>e.g., 192.168.10.21</i>"]
    end

    subgraph Smart_Building ["Smart Building / IoT Branch"]
        HG["Home Gateway<br/>(Wireless AP)"]
        IOT_WIRED["Wired Devices<br/>(Motion Sensor, Smart Lock, Lighting)"]
        IOT_CAM["CCTV / IP Camera"]
        IOT_WIRELESS["Wireless Devices<br/>(Smart Sensors)"]
    end

    R1 <--> MLS
    MLS <--> SRV
    MLS <--> SW_ADMIN
    MLS <--> SW_IT
    MLS <--> SW_MGMT
    MLS <--> SW_IOT

    SW_ADMIN --- PC_ADMIN
    SW_IT --- PC_IT
    SW_MGMT --- PC_MGMT

    SW_IOT --- IOT_WIRED
    SW_IOT --- IOT_CAM
    SW_IOT --- HG
    HG -.- IOT_WIRELESS
```

---

## 🔢 IP Addressing Scheme

All connected devices resolve under the `192.168.10.0/24` IP pool with a subnet mask of `255.255.255.0`.

| Device / Group | Role / Function | IP Address | Subnet Mask | Default Gateway | DNS Server | Assignment Type |
| :--- | :--- | :--- | :--- | :--- | :--- | :--- |
| **Router** | Edge Gateway | `192.168.10.1` | `255.255.255.0` | *N/A* | *N/A* | **Static** |
| **Central Server** | Multi-Service Host | `192.168.10.2` | `255.255.255.0` | `192.168.10.1` | `192.168.10.2` | **Static** |
| **Admin 1 PC** | Admin Workstation | `192.168.10.24` | `255.255.255.0` | `192.168.10.1` | `192.168.10.2` | **DHCP** |
| **IT 3 PC** | IT Workstation | `192.168.10.17` | `255.255.255.0` | `192.168.10.1` | `192.168.10.2` | **DHCP** |
| **Management 3 PC** | Management Workstation | `192.168.10.21` | `255.255.255.0` | `192.168.10.1` | `192.168.10.2` | **DHCP** |
| **Other Endpoints** | Department & IoT Endpoints | `192.168.10.10 - 192.168.10.69` | `255.255.255.0` | `192.168.10.1` | `192.168.10.2` | **DHCP** |

![IP Configuration Table Overview](assets/images/image2.jpeg)

---

## 🛠️ Centralized Enterprise Network Services

### 1. Dynamic Host Configuration Protocol (DHCP)
* **Host Server:** Central Server (`192.168.10.2`)
* **Address Range:** `192.168.10.10` to `192.168.10.69`
* **Default Gateway Provided:** `192.168.10.1`
* **DNS Server Provided:** `192.168.10.2`
* **Function:** Automated IP lease distribution across Admin, IT, Management PCs, and wired/wireless IoT controllers.

| Management PC DHCP Lease | Admin PC DHCP Lease | IT PC DHCP Lease |
| :---: | :---: | :---: |
| ![Management DHCP](assets/images/image6.jpeg) | ![Admin DHCP](assets/images/image7.jpeg) | ![IT DHCP](assets/images/image5.jpeg) |

---

### 2. Domain Name System (DNS)
* **Host Server:** Central Server (`192.168.10.2`)
* **Record Type:** `A Record`
* **Configured Hostnames:**
  * `www.smarttech.local` ➔ `192.168.10.2` (Web Portal)
  * `ftp.smarttech.local` ➔ `192.168.10.2` (File Repository)
  * `mail.smarttech.local` ➔ `192.168.10.2` (Email Service)

> [!TIP]
> All DNS hostnames were verified using `nslookup` and ICMP `ping` execution directly from departmental workstations.

| DNS Server Setup | `nslookup www.smarttech.local` | `nslookup ftp.smarttech.local` | `nslookup mail.smarttech.local` |
| :---: | :---: | :---: | :---: |
| ![DNS Configuration](assets/images/image10.jpeg) | ![HTTP DNS Lookup](assets/images/image11.jpeg) | ![FTP DNS Lookup](assets/images/image12.jpeg) | ![Mail DNS Lookup](assets/images/image13.jpeg) |

---

### 3. Hypertext Transfer Protocol (HTTP) Web Server
* **Service:** HTTP / HTTPS turned ON on Central Server (`192.168.10.2`).
* **Customization:** Edited `index.html` to display a customized SmartTech welcome portal highlighting corporate news and departmental links.
* **Testing:** Verified by accessing `http://192.168.10.2` and `http://www.smarttech.local` via workstation web browsers.

| HTTP Service Status | Customized index.html | Web Browser Verification |
| :---: | :---: | :---: |
| ![HTTP Enable](assets/images/image14.jpeg) | ![HTML Editor](assets/images/image15.jpeg) | ![Browser Page](assets/images/image16.jpeg) |

---

### 4. File Transfer Protocol (FTP) & Role-Based Access Control (RBAC)
* **Service:** FTP Server enabled on `192.168.10.2`.
* **Security & Access Control (RBAC):**
  * **Administrator Account:** Granted full privileges — Read (`R`), Write (`W`), Delete (`D`), List (`L`).
  * **Employee Accounts:** Restricted privileges — Read (`R`), List (`L`) only.
* **File Operations Verified:**
  * Authenticated via FTP prompt on workstation command line.
  * Transferred sample files using `get` and verified remote inventory using `dir`.

| FTP RBAC Configuration | CLI Login & File Download (`get`) | Server Directory Confirmation (`dir`) |
| :---: | :---: | :---: |
| ![FTP RBAC](assets/images/image17.jpeg) | ![FTP Download](assets/images/image18.jpeg) | ![FTP Dir](assets/images/image19.jpeg) |

---

### 5. Email Services
* **Domain Name:** `smarttech.local`
* **Configured Mailboxes:**
  * `admin@smarttech.local`
  * `it@smarttech.local`
  * Additional departmental user mailboxes.
* **Verification:** Sent and received test emails between `it@smarttech.local` and `admin@smarttech.local` via Packet Tracer Email Client.

| Mail Server Configuration | Email Sent/Received (`it@smarttech.local`) | Email Sent/Received (`admin@smarttech.local`) |
| :---: | :---: | :---: |
| ![Mail Service](assets/images/image20.jpeg) | ![IT Mail Test](assets/images/image21.jpeg) | ![Admin Mail Test](assets/images/image22.jpeg) |

---

### 6. Smart Building & Internet of Things (IoT) Integration
* **IoT Controller:** Central IoT Server running on `192.168.10.2`.
* **Device Onboarding:**
  * **Wired Devices:** Connected to the IoT switch, acquiring DHCP addresses and registering to the IoT Server.
  * **Wireless Devices:** Connected via **Home Gateway** (WPA/DHCP), registered to central IoT management credentials.
* **Smart Automation Rule:**
  * **Condition:** Motion Detector triggered (`Alt + Click` simulation).
  * **Action:** Smart Office Lighting automatically switches ON upon motion detection.
* **CCTV Surveillance:** Real-time IP Camera video feed monitoring accessible via IoT Web Dashboard.

| IoT Server Setup | Web Dashboard Registration | Automation Rule Setup | CCTV Video Stream |
| :---: | :---: | :---: | :---: |
| ![IoT Setup](assets/images/image23.png) | ![IoT Dashboard](assets/images/image28.jpeg) | ![Automation Rule](assets/images/image29.jpeg) | ![CCTV Stream](assets/images/image33.jpeg) |

---

## 🧪 Verification & Connectivity Testing

Comprehensive connectivity and protocol verification tests were conducted across all departments:

1. **IP Allocation & Local Gateway Reachability:**
   * Command executed: `ipconfig /all` on Admin, IT, and Management workstations.
   * Command executed: `ping 192.168.10.1` (Gateway) and `ping 192.168.10.2` (Server).
2. **Inter-Departmental ICMP Testing:**
   * Executed ICMP pings between Admin PC (`192.168.10.24`), IT PC (`192.168.10.17`), and Management PC (`192.168.10.21`). All tests succeeded with 0% packet loss.
3. **Application Protocol Verification:**
   * **DNS & HTTP:** Resolved `www.smarttech.local` and rendered web pages in PC browsers.
   * **FTP:** Connected via CLI, listed directory, downloaded test files.
   * **SMTP/POP3:** Transferred messages successfully across departmental email accounts.

![Connectivity Test Aggregation](assets/images/image9.jpeg)

---

## 🔍 Troubleshooting & Resolved Issues

During implementation, two notable technical challenges arose and were systematically resolved:

### Issue 1: Wireless Laptop DHCP Pool Misdirection
* **Symptom:** An IT Department Laptop unexpectedly acquired an IP address outside the enterprise `192.168.10.0/24` subnet.
* **Root Cause:** The laptop's wireless card was enabled and automatically associated with the **Home Gateway** wireless network, taking a DHCP address from the Home Gateway's localized pool prior to wired link initialization.
* **Remediation:** Disabled the wireless adapter on the IT laptop, forcing it to communicate strictly over the wired switch port and acquire its proper lease (`192.168.10.x`) from the central server pool.

![Troubleshooting Laptop Wireless](assets/images/image34.jpeg)

---

### Issue 2: Cisco Packet Tracer Email Domain Constraint
* **Symptom:** Packet Tracer rejected user account creation when using `.local` as the mail domain suffix in certain interface wizards.
* **Root Cause:** Simulator-specific validation rules restricting string parsing for top-level domains during account registration.
* **Remediation:** Applied `.loc` as the internal user registration alias while retaining the fully qualified `smarttech.local` domain in DNS records, enabling successful account registration and seamless mail transmission.

![Troubleshooting Email Domain](assets/images/image35.jpeg)

---

## 📁 Repository & Project File Structure

```
SmartTech Docs/
├── README.md                           # Comprehensive project documentation (This file)
├── SmartTech Documentation.docx        # Original Word documentation report by Odunuga Fatai Olayinka
├── SmartTech_Network.pkt               # Cisco Packet Tracer network simulation file
└── assets/
    └── images/                         # Network topology screenshots, service configs & test results
        ├── image1.jpeg                 # Network Topology Diagram
        ├── image2.jpeg                 # IP Addressing Table
        ├── image3.png - image9.jpeg    # IP Configuration & Ping Connectivity Verification
        ├── image10.jpeg - image13.jpeg # DNS Server Configuration & nslookup tests
        ├── image14.jpeg - image16.jpeg # HTTP Web Server Configuration & Browser test
        ├── image17.jpeg - image19.jpeg # FTP Service, RBAC, CLI file transfer & dir checks
        ├── image20.jpeg - image22.jpeg # Mail Server Setup & Email client tests
        ├── image23.png - image33.jpeg  # IoT Server, Smart Lighting automation & CCTV monitoring
        └── image34.jpeg - image35.jpeg # Troubleshooting screenshots (Wireless DHCP & Mail domain fix)
```

---

## 🚀 Getting Started & Simulation Execution

To open, inspect, and run the SmartTech network simulation:

1. **Prerequisites:**
   * Install **Cisco Packet Tracer** (v8.0 or higher recommended).
2. **Opening the Simulation:**
   * Launch Cisco Packet Tracer.
   * Navigate to `File` > `Open...` and select `SmartTech_Network.pkt` located in this directory.
3. **Verifying Services:**
   * **Ping Test:** Open any departmental PC (e.g., Admin PC), open the Command Prompt, and run:
     ```bash
     ping 192.168.10.2
     ping www.smarttech.local
     ```
   * **Web Browsing:** Open PC Web Browser and visit `http://www.smarttech.local`.
   * **FTP Access:** In PC Command Prompt, type `ftp ftp.smarttech.local`, log in using `admin` credentials, and run `dir` or `get sample.txt`.
   * **IoT Motion Automation:** Toggle simulation mode or hold `Alt + Click` on the **Motion Detector** to observe the **Smart Light** turning on automatically.

---

## 📌 Conclusion

The **SmartTech Network Simulation Project** successfully demonstrates a fully functional, integrated enterprise IT infrastructure. By establishing a centralized multi-service server room, enforcing Role-Based Access Control, connecting key business departments, and integrating smart building IoT automation, the design delivers robust performance, security, and scalability. The documented troubleshooting workflows further illustrate practical network diagnostics and simulator adaptation techniques.

---
*Documentation prepared for SmartTech | Original Work by Odunuga Fatai Olayinka*
