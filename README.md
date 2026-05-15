
# 🖥️ Virtual Home Lab Project — Windows + Linux Infrastructure

![Windows Server](https://img.shields.io/badge/Windows%20Server-2022-blue)
![Ubuntu](https://img.shields.io/badge/Ubuntu-22.04-orange)
![VMware](https://img.shields.io/badge/VMware-Workstation%20Pro-green)
![Status](https://img.shields.io/badge/Project-Completed-brightgreen)

---

## 📌 Overview
This project demonstrates the end-to-end setup of a virtual enterprise IT environment using Windows and Linux systems.

---

## 🏗️ Architecture Diagram

```
                +----------------------+
                |   Windows Server     |
                |   DC1 (AD, DNS, DHCP)|
                |   192.168.247.10     |
                +----------+-----------+
                           |
        -----------------------------------------
        |                                       |
+----------------------+            +----------------------+
| Ubuntu Server        |            | Windows 11 Client    |
| Log1 (Samba Server)  |            | Win1 (Domain Joined) |
| 192.168.247.20       |            | DHCP Assigned        |
+----------------------+            +----------------------+
```

---

## 🚀 Key Features
- Active Directory setup
- DNS & DHCP configuration
- Linux Samba file sharing
- Windows/Linux integration

---

## 📂 Screenshots (Add yours here)

![AD Setup](./screenshots/ad.png)
![Samba Share](./screenshots/samba.png)
![Domain Join](./screenshots/domain.png)

---

## ⚙️ Technologies
- Windows Server 2022  
- Ubuntu Server 22.04  
- Windows 11  
- VMware Workstation Pro  

---

## 🧠 Skills Demonstrated
- Virtualization  
- Active Directory  
- Linux Administration  
- Networking (DNS/DHCP)  
- Security Hardening  

---

## 💼 Resume Highlights
- Built a multi-VM lab environment  
- Configured AD DS, DNS, DHCP  
- Integrated Linux Samba with Windows domain  

---

## 📎 Author
**Onyinye Ilechukwu**
