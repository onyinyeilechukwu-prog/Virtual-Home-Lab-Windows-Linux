# 🖥️ Virtual Home Lab — Windows + Linux Infrastructure

<p align="center">
  <img src="https://img.shields.io/badge/VMware-Workstation_Pro-607078?style=for-the-badge&logo=vmware&logoColor=white"/>
  <img src="https://img.shields.io/badge/Windows_Server-2022-0078D4?style=for-the-badge&logo=windows&logoColor=white"/>
  <img src="https://img.shields.io/badge/Ubuntu_Server-22.04_LTS-E95420?style=for-the-badge&logo=ubuntu&logoColor=white"/>
  <img src="https://img.shields.io/badge/Active_Directory-Domain_Services-0078D4?style=for-the-badge&logo=microsoft&logoColor=white"/>
  <img src="https://img.shields.io/badge/Status-Completed-2ea44f?style=for-the-badge"/>
</p>

---

## 📌 Executive Summary

This project documents the end-to-end design, deployment, and administration of a virtualized enterprise IT lab environment. Using VMware Workstation, three virtual machines were built and integrated to simulate a production-ready network: a **Windows Server 2022 Domain Controller**, an **Ubuntu Server 22.04 file server**, and a **Windows 11 client workstation**.

The project covers the full system administration lifecycle — from OS installation and security hardening, through infrastructure service configuration (Active Directory, DNS, DHCP, Samba), to cross-platform domain integration and file sharing. Each phase reflects tasks performed by real-world IT administrators in enterprise environments.

> **Domain:** `ONYINYELAB.COM` &nbsp;|&nbsp; **Network:** `192.168.247.0/24` &nbsp;|&nbsp; **Phases:** 17

---

## 🌐 Network Topology

```
┌──────────────────────────────────────────────────────────────────┐
│                       VMware Virtual Network                      │
│                        192.168.247.0 / 24                         │
│                                                                    │
│   ┌─────────────────┐   ┌─────────────────┐   ┌───────────────┐  │
│   │      DC1        │   │      Log1       │   │     Win1      │  │
│   │  Windows Server │   │  Ubuntu Server  │   │  Windows 11   │  │
│   │     2022        │◄──►     22.04       │◄──►   Client      │  │
│   │                 │   │                 │   │               │  │
│   │ 192.168.247.10  │   │ 192.168.247.20  │   │  DHCP Lease   │  │
│   │    (Static)     │   │    (Static)     │   │  .100 – .200  │  │
│   └────────┬────────┘   └─────────────────┘   └───────┬───────┘  │
│            │                                           │           │
│            └──────────── ONYINYELAB.COM ───────────────┘           │
└──────────────────────────────────────────────────────────────────┘
```

| VM | Role | IP Address | OS |
|:---|:---|:---|:---|
| **DC1** | Domain Controller / DNS / DHCP | `192.168.247.10` (Static) | Windows Server 2022 |
| **Log1** | Samba File Server | `192.168.247.20` (Static) | Ubuntu Server 22.04 LTS |
| **Win1** | Domain Client Workstation | `192.168.247.100–200` (DHCP) | Windows 11 |

---

## 🖥️ Virtual Machine Specifications

<details>
<summary><strong>DC1 — Windows Server 2022 (Domain Controller)</strong></summary>

| Setting | Value |
|:---|:---|
| Role | Domain Controller / DNS Server / DHCP Server |
| IP Address | 192.168.247.10 (Static) |
| Disk | 60 GB |
| RAM | 4 GB |
| CPUs | 4 vCPUs |
| Network Adapter | NAT |

</details>

<details>
<summary><strong>Log1 — Ubuntu Server 22.04 (File Server)</strong></summary>

| Setting | Value |
|:---|:---|
| Role | Samba File Server |
| IP Address | 192.168.247.20 (Static) |
| Disk | 50 GB |
| RAM | 4 GB |
| CPUs | 4 vCPUs |
| Network Adapter | NAT |

</details>

<details>
<summary><strong>Win1 — Windows 11 (Domain Client)</strong></summary>

| Setting | Value |
|:---|:---|
| Role | Domain-Joined Workstation |
| IP Address | DHCP — 192.168.247.100–200 |
| Disk | 60 GB |
| RAM | 2 GB |
| CPUs | 2 vCPUs |
| Network Adapter | NAT |

</details>

---

## 📋 Project Phases

### Phase 1 — Environment Setup & ISO Downloads
Installed VMware Workstation Pro and downloaded all required operating system images:
- Windows Server 2022 (Microsoft Evaluation Center)
- Ubuntu Server 22.04 LTS (Canonical)
- Windows 11 (Microsoft)

📸 *Screenshot: VMware dashboard with loaded ISOs*

---

### Phase 2 — Virtual Network Configuration (VMnet)
Created a custom isolated VMnet using VMware's Virtual Network Editor:

| Setting | Value |
|:---|:---|
| Adapter Type | Host-Only |
| Subnet IP | 10.10.10.0 |
| Subnet Mask | 255.255.255.0 |

📸 *Screenshot: VMware Virtual Network Editor*

---

### Phase 3 — Windows Server 2022 VM Creation
Provisioned the DC1 virtual machine and installed Windows Server 2022 Standard (Desktop Experience).

📸 *Screenshot: DC1 hardware settings in VMware*

---

### Phase 4 — Harden Windows Server 2022
Applied baseline security hardening before promoting to Domain Controller:
- ✅ Applied all Windows Updates
- ✅ Disabled the Guest account via Local Security Policy
- ✅ Confirmed Windows Defender Antivirus is active
- ✅ Enabled Windows Firewall on Domain, Private, and Public profiles

📸 *Screenshot: Windows Security Center — Defender and Firewall active*

---

### Phase 5 — Static IP Configuration (DC1)
Assigned a static IPv4 address to ensure stable DNS and AD DS operation:

| Setting | Value |
|:---|:---|
| IP Address | 192.168.247.10 |
| Subnet Mask | 255.255.255.0 |
| Default Gateway | 192.168.247.2 |
| Preferred DNS | 127.0.0.1 |

📸 *Screenshot: Network Adapter IPv4 settings on DC1*

---

### Phase 6 — Promote Windows Server to Domain Controller
Installed AD DS, DNS, and DHCP roles then promoted the server to a DC:
- Installed **Active Directory Domain Services**, **DNS Server**, and **DHCP Server** roles via Server Manager
- Ran the AD DS Configuration Wizard to create a new forest
- Set root domain name: **`ONYINYELAB.COM`**
- Server restarted and successfully joined the domain

📸 *Screenshot: Active Directory Users and Computers — ONYINYELAB.COM*

---

### Phase 7 — DNS Server Configuration
Configured DNS to support both forward and reverse name resolution:
- Created **Forward Lookup Zone** → `ONYINYELAB.COM`
- Created **Reverse Lookup Zone** → `192.168.247.x`
- Verified DNS resolution using `nslookup` and `Resolve-DnsName` in PowerShell

📸 *Screenshot: DNS Manager with forward and reverse lookup zones*

---

### Phase 8 — DHCP Server Configuration
Created and activated a DHCP scope to automatically assign IPs to domain clients:

| Setting | Value |
|:---|:---|
| Scope Name | Onyinyab |
| Start IP | 192.168.247.100 |
| End IP | 192.168.247.200 |
| Default Gateway | 192.168.247.2 |
| DNS Server | 192.168.10.10 |
| DNS Domain | ONYINYELAB.COM |

📸 *Screenshot: DHCP Manager — active scope and lease assignments*

---

### Phase 9 — Organizational Units & User Accounts
Structured Active Directory with OUs and domain user accounts:
- Created Organizational Units to reflect department structure
- Created domain user accounts with names, passwords, and OU placement
- Assigned users to security groups for resource access control

📸 *Screenshot: Active Directory Users and Computers — OUs and users*

---

### Phase 10 — Ubuntu Server 22.04 VM Creation
Provisioned the Log1 virtual machine and performed a base Ubuntu Server installation.

📸 *Screenshot: Log1 hardware summary in VMware*

---

### Phase 11 — Harden Ubuntu Server 22.04
Applied security hardening before configuring services:
- ✅ Updated OS: `sudo apt update && sudo apt upgrade -y`
- ✅ Disabled guest account
- ✅ Reviewed and restricted SSH access in `sshd_config`

📸 *Screenshot: Terminal output — system fully updated*

---

### Phase 12 — Static IP Configuration (Log1)
Configured a static IP address via Netplan:

| Setting | Value |
|:---|:---|
| IP Address | 192.168.247.20 |
| Subnet Mask | 255.255.255.0 (/24) |
| Default Gateway | 192.168.247.2 |
| DNS Server | 192.168.10.10 |

```yaml
# /etc/netplan/00-installer-config.yaml
network:
  ethernets:
    ens33:
      dhcp4: no
      addresses: [192.168.247.20/24]
      gateway4: 192.168.247.2
      nameservers:
        addresses: [192.168.10.10]
  version: 2
```

📸 *Screenshot: `ip addr` output confirming static IP on Log1*

---

### Phase 13 — Samba File Sharing Setup
Installed and configured Samba to provide SMB/CIFS file sharing accessible from Windows clients:

```bash
# Install Samba
sudo apt install samba -y

# Create shared directory
sudo mkdir -p /srv/samba/shared
sudo chmod 777 /srv/samba/shared
```

```ini
# /etc/samba/smb.conf
[Shared]
   path = /srv/samba/shared
   browseable = yes
   read only = no
   guest ok = no
   valid users = @sambausers
```

```bash
sudo systemctl restart smbd
sudo ufw allow samba
```

📸 *Screenshot: Samba config and `systemctl status smbd` output*

---

### Phase 14 — Windows 11 VM Creation
Provisioned the Win1 virtual machine and installed Windows 11.

📸 *Screenshot: Win1 hardware settings in VMware*

---

### Phase 15 — Harden Windows 11 Client
- ✅ Applied all Windows Updates
- ✅ Confirmed Windows Defender Antivirus is active
- ✅ Verified Windows Firewall enabled on all network profiles

📸 *Screenshot: Windows Security dashboard on Win1*

---

### Phase 16 — Join Windows 11 to Domain
Joined Win1 to `ONYINYELAB.COM` using an authorized Active Directory account:
- DHCP assigned an IP in the `192.168.247.100–200` range
- Navigated to **System > Advanced System Settings > Computer Name > Change**
- Entered domain: `ONYINYELAB.COM` and authenticated with AD credentials
- Restarted and logged in with domain user account

📸 *Screenshot: System Properties — Win1 joined to ONYINYELAB.COM*

---

### Phase 17 — Access Samba Share from Domain Client
Accessed the Ubuntu Samba share from Win1 using authorized credentials:
- Opened **File Explorer → Map Network Drive**
- Entered UNC path: `\\192.168.247.20\Shared`
- Authenticated with Samba user credentials
- Successfully read and wrote files to the shared folder

📸 *Screenshot: Mapped network drive on Win1 showing Samba share*

---

## 🎯 Technical Skills Demonstrated

| Skill | What Was Done |
|:---|:---|
| **Virtualization** | Deployed and managed 3 VMs in VMware Workstation; configured virtual hardware, networking, and storage |
| **Network Segmentation** | Designed isolated virtual networks using VMnet adapters; planned and implemented IP addressing scheme |
| **Windows Server Administration** | Installed, configured, and hardened Windows Server 2022; managed server roles and features |
| **Linux Administration** | Deployed Ubuntu Server 22.04; used CLI for configuration, networking, and service management |
| **Active Directory** | Promoted server to Domain Controller; created OUs, user accounts, and managed group memberships |
| **DNS Configuration** | Configured forward and reverse lookup zones; verified name resolution via nslookup and PowerShell |
| **DHCP Configuration** | Created and activated DHCP scope; configured IP pools, gateway, DNS server, and domain options |
| **Samba / File Sharing** | Configured cross-platform SMB/CIFS file sharing between Ubuntu and Windows systems |
| **Domain Integration** | Joined Windows 11 workstation to Active Directory domain using authorized AD credentials |
| **Security Hardening** | Applied OS patching, disabled accounts, and configured firewalls on both Windows and Linux |

---

## 💼 Relevance to System Administrator Roles

This project directly maps to responsibilities commonly listed in System Administrator and IT Infrastructure job descriptions:

- **Active Directory & Identity Management** — Core skill for enterprise Windows environments
- **DNS & DHCP Administration** — Fundamental networking services managed in all corporate networks
- **Linux Server Management** — Critical for hybrid and cloud-adjacent infrastructure roles
- **Virtualization** — VMware experience is highly sought in enterprise IT environments
- **Cross-Platform Integration** — Demonstrated ability to bridge Windows and Linux ecosystems
- **Security Hardening** — Shows awareness of baseline security practices on both platforms
- **Structured Problem-Solving** — All phases were planned, documented, and executed systematically

> This lab was built entirely from scratch, reflecting the ability to independently research, plan, and implement IT infrastructure — a key attribute for junior to mid-level system administrators.

---

## 📁 Repository Structure

```
virtual-home-lab/
├── README.md
├── docs/
│   └── Virtual_Home_Lab_Project_Documentation.docx
├── screenshots/
│   ├── phase01-vmware-setup.png
│   ├── phase02-vmnet-config.png
│   ├── phase03-dc1-vm.png
│   ├── phase04-hardening.png
│   ├── phase05-static-ip-dc1.png
│   ├── phase06-active-directory.png
│   ├── phase07-dns.png
│   ├── phase08-dhcp.png
│   ├── phase09-ou-users.png
│   ├── phase10-log1-vm.png
│   ├── phase11-ubuntu-hardening.png
│   ├── phase12-static-ip-log1.png
│   ├── phase13-samba.png
│   ├── phase14-win1-vm.png
│   ├── phase15-win11-hardening.png
│   ├── phase16-domain-join.png
│   └── phase17-samba-access.png
└── configs/
    ├── netplan-log1.yaml
    └── smb.conf
```

---

*Built as part of a hands-on IT career development portfolio — demonstrating real-world system administration skills in a virtualized lab environment.*

