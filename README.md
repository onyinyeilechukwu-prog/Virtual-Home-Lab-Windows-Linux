
**Virtual Home Lab Project**

Windows Server 2022 + Ubuntu 22.04 Enterprise Infrastructure

Domain: ONYINYELAB.COM | 17 Phases | Virtualization & Active Directory | DNS/DHCP | Linux File Sharing

**EXECUTIVE SUMMARY**

This project demonstrates the end-to-end design, deployment, and administration of a virtualized enterprise IT lab environment. Using VMware Workstation, three virtual machines were built and integrated to simulate a production-ready network: a Windows Server 2022 Domain Controller, an Ubuntu Server 22.04 file server, and a Windows 11 client workstation.

The project covers the full system administration lifecycle — from OS installation and security hardening, through infrastructure service configuration (Active Directory, DNS, DHCP, Samba), to cross-platform domain integration and file sharing. Each phase reflects tasks performed by real-world IT administrators in enterprise environments.

**PROJECT DETAILS**

|     |     |
| --- | --- |
| **Project Title** | Virtual Home Lab – Windows + Linux |
| **Domain** | ONYINYELAB.COM |
| **Hypervisor** | VMware Workstation Pro |
| **Number of Phases** | 17 Phases |
| **Network Range** | 192.168.247.0/24 |
| **Project Type** | IT Infrastructure / Home Lab |
| **Operating Systems** | Windows Server 2022, Ubuntu 22.04 LTS, Windows 11 |

**VIRTUAL MACHINE SPECIFICATIONS**

**DC1 — Windows Server 2022 (Domain Controller)**

|     |     |
| --- | --- |
| **Role** | Domain Controller / DNS Server / DHCP Server |
| **IP Address** | 192.168.247.10 (Static) |
| **Disk** | 60 GB |
| **RAM** | 4 GB |
| **CPUs** | 4 vCPUs |
| **Network** | NAT |

**Log1 — Ubuntu Server 22.04 (File Server)**

|     |     |
| --- | --- |
| **Role** | Samba File Server |
| **IP Address** | 192.168.247.20 (Static) |
| **Disk** | 50 GB |
| **RAM** | 4 GB |
| **CPUs** | 4 vCPUs |
| **Network** | NAT |

**Win1 — Windows 11 (Domain Client)**

|     |     |
| --- | --- |
| **Role** | Domain-Joined Workstation |
| **IP Address** | DHCP: 192.168.247.100–200 |
| **Disk** | 60 GB |
| **RAM** | 2 GB |
| **CPUs** | 2 vCPUs |
| **Network** | NAT |

**PROJECT PHASES — DETAILED BREAKDOWN**

**Phase 1: Environment Setup & ISO Downloads**

Installed VMware Workstation Pro. Downloaded official ISO images for all three operating systems:

- Windows Server 2022 (Microsoft Evaluation Center)
- Ubuntu Server 22.04 LTS (Canonical)
- Windows 11 (Microsoft)

**Phase 2: Virtual Network Configuration**

Created a custom VMnet network using VMware's Virtual Network Editor to isolate the lab:

- Adapter Type: Host-Only
- Subnet IP: 10.10.10.0
- Subnet Mask: 255.255.255.0

**Phase 3: Windows Server 2022 VM Creation**

Provisioned the DC1 VM with 60 GB disk, 4 GB RAM, 4 vCPUs, NAT adapter. Installed Windows Server 2022 Standard (Desktop Experience).

**Phase 4: Windows Server Security Hardening**

- Applied all Windows Updates via Windows Update
- Disabled Guest account via Local Security Policy
- Verified Windows Defender Antivirus is active and updated
- Enabled Windows Firewall on Domain, Private, and Public profiles

**Phase 5: Static IP Configuration — DC1**

Configured static IPv4 on the server NIC to ensure reliable DNS and AD DS:

- IP: 192.168.247.10 | Subnet: 255.255.255.0 | Gateway: 192.168.247.2 | DNS: 127.0.0.1

**Phase 6: Promote Server to Domain Controller**

- Installed AD DS, DNS, and DHCP roles via Server Manager
- Ran the AD DS Configuration Wizard
- Created new forest with root domain: ONYINYELAB.COM
- Server successfully promoted and restarted as Domain Controller

**Phase 7: DNS Server Configuration**

- Created Forward Lookup Zone: ONYINYELAB.COM
- Created Reverse Lookup Zone for 192.168.247.x subnet
- Verified resolution using nslookup and PowerShell Resolve-DnsName

**Phase 8: DHCP Server Configuration**

- Created DHCP scope 'Onyinyab': range 192.168.247.100–200
- Configured router option: 192.168.247.2
- Configured DNS option: 192.168.10.10 with domain ONYINYELAB.COM
- Activated scope and authorized DHCP server in AD

**Phase 9: Organizational Units & User Accounts**

- Created Organizational Units (OUs) to reflect department structure
- Created domain user accounts with appropriate names, passwords, and OU placement
- Assigned users to security groups for resource access control

**Phase 10: Ubuntu Server 22.04 VM Creation**

Provisioned Log1 VM with 50 GB disk, 4 GB RAM, 4 vCPUs, NAT adapter. Performed base Ubuntu Server install.

**Phase 11: Ubuntu Server Security Hardening**

- Updated OS: sudo apt update && sudo apt upgrade -y
- Disabled guest account and restricted unnecessary services
- Reviewed sshd_config for secure remote access settings

**Phase 12: Static IP Configuration — Log1**

Configured static IP via Netplan configuration file:

- IP: 192.168.247.20 | Subnet: /24 | Gateway: 192.168.247.2 | DNS: 192.168.10.10
- Applied config with: sudo netplan apply

**Phase 13: Samba File Sharing Setup**

- Installed Samba: sudo apt install samba
- Created shared directory: /srv/samba/shared
- Configured /etc/samba/smb.conf with share name, path, and access controls
- Added Samba users and restarted smbd service
- Configured UFW firewall to allow Samba traffic

**Phase 14: Windows 11 VM Creation**

Provisioned Win1 with 60 GB disk, 2 GB RAM, 2 vCPUs. Installed Windows 11 Home/Pro.

**Phase 15: Windows 11 Security Hardening**

- Applied all Windows Updates
- Confirmed Windows Defender Antivirus active and scanning
- Verified Windows Firewall enabled on all network profiles

**Phase 16: Join Windows 11 to Domain**

- Verified DHCP assigned IP in range 192.168.247.100–200
- Navigated to System > Advanced System Settings > Computer Name > Change
- Entered domain name: ONYINYELAB.COM
- Authenticated with Active Directory user account created in Phase 9
- Restarted Win1 and logged in with domain credentials

**Phase 17: Access Samba Share from Domain Client**

- Opened File Explorer on Win1 > Map Network Drive
- Entered UNC path: \\\\192.168.247.20\\Shared
- Authenticated with Samba user account credentials
- Successfully accessed, read, and wrote files to the shared folder

**TECHNICAL SKILLS DEMONSTRATED**

|     |     |
| --- | --- |
| **Skill** | **Description** |
| **Virtualization** | Deployed and managed 3 VMs using VMware Workstation Pro; configured virtual hardware, networking, and storage |
| **Network Segmentation** | Designed isolated virtual networks using VMnet Host-Only adapters; planned IP address scheme |
| **Windows Server Admin** | Installed, configured, and hardened Windows Server 2022; managed server roles and features |
| **Linux Administration** | Installed and managed Ubuntu Server 22.04; used CLI for configuration, networking, and services |
| **Active Directory** | Promoted server to DC; created OUs, user accounts, and managed group memberships |
| **DNS Configuration** | Configured forward and reverse lookup zones; verified resolution via nslookup and PowerShell |
| **DHCP Configuration** | Created and activated DHCP scopes; configured IP pools, gateway, DNS, and domain options |
| **File Sharing (Samba)** | Configured cross-platform SMB/CIFS file sharing between Ubuntu and Windows systems |
| **Domain Integration** | Joined Windows 11 workstation to Active Directory domain using authorized user credentials |
| **Security Hardening** | Applied OS updates, disabled accounts, configured firewalls on both Windows and Linux systems |


_This document was prepared as part of a hands-on IT career development portfolio. All configurations, screenshots, and configurations represent work completed independently in a personal virtual lab environment._
**Onyinye Ilechukwu**
