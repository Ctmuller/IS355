# IS355: Tutorial 9 - Cyber Security Management

---

[Course Journal](Main.md) | [Tutorial 8](Tutorial8.md) | [Tutorial 10](Tutorial_10.md) | [Group Project Repository](https://github.com/Ctmuller/IS355_GroupProject)

## Task 1 — Select Security Objectives

For this tutorial, I continued using our group's manufacturing company network design. The company operates from one site with an attached office and factory, and our network separates management, office computers, factory equipment, cameras, servers, staff Wi-Fi, and guest Wi-Fi into seven VLANs.

I selected four sub-categories from the **NIST Cybersecurity Framework (CSF) 2.0**: two from the Protect function and two from the Detect function, each from a different category.

| Function | Category | Sub-category | Objective |
|---|---|---|---|
| Protect (PR) | Technology Infrastructure Resilience (PR.IR) | **PR.IR-01** | Networks and environments are protected from unauthorized logical access and usage. |
| Protect (PR) | Data Security (PR.DS) | **PR.DS-11** | Backups of data are created, protected, maintained, and tested. |
| Detect (DE) | Continuous Monitoring (DE.CM) | **DE.CM-01** | Networks and network services are monitored to find potentially adverse events. |
| Detect (DE) | Adverse Event Analysis (DE.AE) | **DE.AE-03** | Information is correlated from multiple sources. |

### Objective 1 — PR.IR-01: Protecting Networks from Unauthorized Access

**Function:** Protect | **Category:** Technology Infrastructure Resilience (PR.IR) | **Sub-category:** PR.IR-01

**Why it is important for our project:**  
Network segmentation is the foundation of our design. The firewall is the default gateway for every VLAN, so all traffic between the office, factory, cameras, servers, and wireless networks has to pass through its rules. This objective makes sure those boundaries are actually enforced instead of just existing on paper.

**Attack/vulnerability it mitigates:**  
**Lateral movement after a compromise.** If an employee opens a phishing email and malware infects an office PC on VLAN 20, segmentation stops the malware from spreading directly to the PLCs and HMIs on VLAN 30 or the cameras on VLAN 40. It also stops guest Wi-Fi users on VLAN 70 from reaching any internal system.

### Objective 2 — PR.DS-11: Creating, Protecting, and Testing Backups

**Function:** Protect | **Category:** Data Security (PR.DS) | **Sub-category:** PR.DS-11

**Why it is important for our project:**  
The company's engineering designs, financial records, and PLC programs are difficult or impossible to recreate. Our design sends encrypted backups to a cloud backup server over the site-to-site VPN. This objective goes further by requiring the backups to be protected and **tested**, because a backup that has never been restored might not work when it is needed.

**Attack/vulnerability it mitigates:**  
**Ransomware.** In Tutorial 8, I identified ransomware as one of the biggest threats to the company. If ransomware encrypts the file server, the company can restore from an off-site backup instead of paying the attackers. The backups need to be immutable (cannot be changed or deleted for a set period) so the ransomware cannot encrypt or delete them too.

### Objective 3 — DE.CM-01: Monitoring the Network

**Function:** Detect | **Category:** Continuous Monitoring (DE.CM) | **Sub-category:** DE.CM-01

**Why it is important for our project:**  
Our FortiGate firewall inspects all traffic between VLANs and to the internet, which makes it the best place to watch for suspicious activity. Its intrusion prevention system (IPS) and traffic logs can show when something on the network is behaving abnormally.

**Attack/vulnerability it mitigates:**  
**Compromised IoT devices, such as the security cameras.** IP cameras often run outdated firmware and are a common target. A camera on VLAN 40 should only ever talk to the NVR. If monitoring shows a camera trying to reach the file server or an unknown internet address, that is a sign it has been compromised. Network monitoring also helps detect denial-of-service traffic like the Cisco NX-OS issue I researched in Tutorial 8.

### Objective 4 — DE.AE-03: Correlating Information from Multiple Sources

**Function:** Detect | **Category:** Adverse Event Analysis (DE.AE) | **Sub-category:** DE.AE-03

**Why it is important for our project:**  
One log on its own often does not look suspicious. Combining logs from the firewall, Active Directory, the file server, and endpoint protection gives a much clearer picture of what is happening. For a company with a small IT team, a central log system (SIEM) that correlates these sources automatically is more realistic than someone checking each device by hand.

**Attack/vulnerability it mitigates:**  
**Stolen credentials from phishing.** If an attacker steals an employee's password, every individual login may look normal. Correlation can reveal the pattern: for example, a successful VPN login from an unusual country, followed by that same account accessing engineering design files it has never opened before, followed by a large upload to the internet. Together, these events point to account takeover and data theft.

### My Analysis

I chose these four objectives because they connect directly to the two threats I considered most serious in Tutorial 8: ransomware and phishing.

The Protect objectives try to **stop or limit** an attack. Segmentation limits how far an attacker can spread, and tested backups mean ransomware cannot permanently destroy the company's data.

The Detect objectives assume that some attacks will still get through. Monitoring and correlating logs help the company notice an attack early, before it reaches the factory or before large amounts of data are stolen.

**Source**

- [NIST Cybersecurity Framework (CSF) 2.0](https://www.nist.gov/cyberframework)
- [NIST CSF 2.0 Reference Tool](https://csrc.nist.gov/Projects/cybersecurity-framework/Filters#/csf/filters)

---

## Task 2 — Create Asset Inventory

In Tutorial 8, I listed the most important assets in our network and the CIA protections they need. For this task, I expanded that list to cover all six asset types: **people, procedures, data, software, hardware, and networking**.

**Note:** Our company is fictional, so the hostnames, serial numbers, MAC addresses, license keys, and public IP addresses below are made-up values for illustration. The internal IP addresses follow our design's 10.10.0.0/16 scheme, where the third octet matches the VLAN number.

### 2.1 People Assets

| Asset ID | Role | Approx. Number | Systems Accessed | Access Level | Security Notes |
|---|---|---|---|---|---|
| PPL-01 | IT Administrator | 1–2 | All systems, management VLAN 10 | Privileged (admin) | Highest-value target for attackers; requires MFA and separate admin accounts |
| PPL-02 | Office Staff | 30–40 | Office PCs, file server, email, ERP | Standard user | Main phishing target; needs security awareness training |
| PPL-03 | Engineers | 5–8 | CAD software, engineering design files | Standard user + design share | Handle the company's most valuable intellectual property |
| PPL-04 | Finance and HR Staff | 3–5 | Accounting software, financial and HR records | Standard user + restricted shares | Targets for invoice fraud and payroll phishing |
| PPL-05 | Factory Operators and Supervisors | 20–30 | HMIs, production schedules | Limited (factory VLAN only) | Physical access to machinery; should not use HMIs for email or browsing |
| PPL-06 | Management / Executives | 3–5 | Email, financial reports, file server | Standard user + reporting | Targets for business email compromise ("whaling") |
| PPL-07 | Contractors and Vendors | Varies | Remote support for PLCs or NVR | Temporary, limited | Access granted only when needed and removed after the job |
| PPL-08 | Visitors / Guests | Varies | Guest Wi-Fi only | Internet only (VLAN 70) | No access to internal systems |

### 2.2 Procedure Assets

| Asset ID | Procedure | Owner | Frequency | Assets Supported | Document Location |
|---|---|---|---|---|---|
| PRC-01 | Backup and restore testing | IT Administrator | Daily backups, monthly restore test | File server, AD, PLC programs, configs | \\FS-01\IT\Procedures\Backup.docx |
| PRC-02 | Patch and firmware management | IT Administrator | Monthly, or immediately for critical CVEs | All servers, PCs, switches, firewall, cameras | \\FS-01\IT\Procedures\Patching.docx |
| PRC-03 | User onboarding and offboarding | IT Administrator + HR | When staff join or leave | AD accounts, email, building access | \\FS-01\IT\Procedures\Accounts.docx |
| PRC-04 | Incident response plan | IT Administrator + Management | Reviewed yearly; used when needed | Entire network | \\FS-01\IT\Procedures\IncidentResponse.docx |
| PRC-05 | Network change management | IT Administrator | Before any firewall or switch change | Firewall rules, VLANs, switch configs | \\FS-01\IT\Procedures\ChangeMgmt.docx |
| PRC-06 | PLC program change control | Factory Engineering Lead | Before any PLC/HMI change | PLCs, HMIs, PLC program files | \\FS-01\Engineering\Procedures\PLC-Changes.docx |
| PRC-07 | Guest Wi-Fi password rotation | IT Administrator | Monthly | Guest SSID (VLAN 70) | \\FS-01\IT\Procedures\GuestWiFi.docx |
| PRC-08 | CCTV footage retention and release | Facilities Manager | Footage kept 30 days | NVR, cameras | \\FS-01\Facilities\CCTV-Policy.docx |
| PRC-09 | Security awareness training | IT Administrator + HR | Yearly, plus phishing tests | All staff | \\FS-01\HR\Training\Security.pptx |

### 2.3 Data Assets

Each data asset is classified by **access** (who is allowed to see it) and **value** (how much harm its loss or exposure would cause).

**Access classification levels:**

- **Public:** Anyone may see it.
- **Internal:** All employees may see it, but not outsiders.
- **Confidential:** Only specific departments or roles may see it.
- **Restricted:** Only a small, named group may see it; exposure would cause serious harm.

| Asset ID | Data Asset | Location | Owner | Access Classification | Value | CIA Priority | Important Protections |
|---|---|---|---|---|---|---|---|
| DAT-01 | Engineering designs and prototype files | FS-01 (\\FS-01\Engineering) | Engineering Lead | Restricted | High | **C**, **I** | Engineers-only share permissions, encryption at rest, access logging, encrypted cloud backup |
| DAT-02 | Financial records | FS-01 + accounting database | Finance Manager | Restricted | High | **C**, **I** | Finance-only permissions, MFA, audit logs, encrypted backup |
| DAT-03 | Employee and payroll (HR) records | FS-01 (\\FS-01\HR) | HR Manager | Restricted | High | **C** | HR-only permissions, encryption, limited retention |
| DAT-04 | Active Directory accounts and password hashes | DC-01 | IT Administrator | Restricted | High | **C**, **I**, **A** | Admin-only access, MFA for admins, DC on server VLAN only, backup of AD |
| DAT-05 | Network device configurations and firewall rules | FW-01, switches, config backups on FS-01 | IT Administrator | Restricted | High | **C**, **I** | Management VLAN only, change management (PRC-05), encrypted config backups |
| DAT-06 | PLC programs and production recipes | PLCs + \\FS-01\Engineering\PLC | Factory Engineering Lead | Confidential | High | **I**, **A** | PLC change control (PRC-06), factory VLAN isolation, offline copies |
| DAT-07 | Customer and order data | ERP database on FS-01 | Sales Manager | Confidential | Medium–High | **C**, **I**, **A** | Role-based access, encrypted backup |
| DAT-08 | Email and internal communications | Cloud email service | IT Administrator | Confidential | Medium | **C**, **A** | MFA, email filtering, phishing protection |
| DAT-09 | CCTV video footage | NVR-01 | Facilities Manager | Confidential | Medium | **I**, **A**, C | Camera VLAN isolation, NVR access limited to security staff, 30-day retention (PRC-08) |
| DAT-10 | Security and system logs | FW-01, DC-01, SIEM | IT Administrator | Internal | Medium | **I**, **A** | Logs sent to central log server, read-only for most users, retained 90 days |
| DAT-11 | Cloud backup copies | Cloud backup server | IT Administrator | Restricted | High | **C**, **I**, **A** | Encrypted in transit (VPN) and at rest, immutable storage, restore testing (PRC-01) |
| DAT-12 | Public website content | Cloud web server | Marketing | Public | Low–Medium | **I**, **A** | Admin MFA, CMS updates, web application firewall, website backup |

### 2.4 Software Assets

| Asset ID | Software | Version | Installed On | License Key (fake) | Purpose |
|---|---|---|---|---|---|
| SW-01 | Windows Server 2022 Standard | 21H2 | DC-01, FS-01 | WS22-7HK3P-QX9M2-4TRV8-LJ6YD | Server operating system |
| SW-02 | Active Directory Domain Services, DNS, DHCP | Windows Server 2022 roles | DC-01 | Included with SW-01 | User authentication, name resolution, IP assignment |
| SW-03 | Windows 11 Enterprise | 24H2 | Office PCs and laptops | W11E-VOLUME-KMS-0042 | Employee operating system |
| SW-04 | Microsoft 365 Apps | Current Channel | Office PCs and laptops | M365-TENANT-LPS-55210 | Email, documents, spreadsheets |
| SW-05 | Google Chrome | Latest stable (auto-update) | Office PCs and laptops | Free | Web browser |
| SW-06 | Microsoft Defender for Endpoint | Plan 2 | All PCs and servers | MDE-P2-55210 | Antivirus and endpoint monitoring |
| SW-07 | FortiOS | 7.4.x | FW-01 | FGT-FC-10-0100F-950-02-36 | Firewall, IPS, VPN, web filtering |
| SW-08 | Cisco IOS XE | 17.x | Core and access switches | C9300-DNA-A-24-3Y | Switch operating system |
| SW-09 | CAD / engineering design software | 2026 | Engineering workstations | CAD-NL-8841-2207 | Creating engineering and prototype designs |
| SW-10 | Accounting / ERP software | 2026 | FS-01 + finance PCs | ERP-ENT-30U-77341 | Financial records, orders, inventory |
| SW-11 | PLC programming and HMI software | v35 | Engineering workstation, HMIs | PLC-ENG-4410-3381 | Programming and operating factory machinery |
| SW-12 | NVR video management software | 5.x | NVR-01 | VMS-32CH-91827 | Recording and viewing camera footage |
| SW-13 | Backup software | 12.x | FS-01 → cloud backup server | BKP-ENT-10WL-66120 | Scheduled encrypted backups |
| SW-14 | Website content management system (CMS) | 6.x | Cloud web server | Open source | Public company website |
| SW-15 | SIEM / central log collection | 2026 | Log server (VM on FS-01 host) | SIEM-SMB-5GB-40318 | Collecting and correlating logs (supports DE.AE-03) |

### 2.5 Hardware Assets

| Asset ID | Device | Hostname | Model | Serial Number | MAC Address | IP Address / VLAN | Location |
|---|---|---|---|---|---|---|---|
| HW-01 | Domain controller (AD/DNS/DHCP) | DC-01 | Dell PowerEdge R660 | 7XK9PL3 | 00:1A:2B:50:00:10 | 10.10.50.10 / VLAN 50 | Server rack |
| HW-02 | File server | FS-01 | Dell PowerEdge R660 | 7XK9PL4 | 00:1A:2B:50:00:20 | 10.10.50.20 / VLAN 50 | Server rack |
| HW-03 | Network Video Recorder | NVR-01 | Enterprise 32-channel NVR | NVR3220A8812 | 00:1A:2B:50:00:30 | 10.10.50.30 / VLAN 50 | Server rack |
| HW-04 | Uninterruptible power supply | UPS-01, UPS-02 | APC Smart-UPS 3000 rack-mount | AS2611004417, AS2611004418 | 00:1A:2B:10:00:40–41 | 10.10.10.40–.41 / VLAN 10 | Server rack |
| HW-05 | Office desktops (×30) | PC-001 to PC-030 | Dell OptiPlex 7020 | 5QW8R01 to 5QW8R30 | 00:1A:2B:20:01:01 – :30 | DHCP 10.10.20.100–.199 / VLAN 20 | Office |
| HW-06 | Laptops (×10) | LT-001 to LT-010 | Dell Latitude 5450 | 9HT2M01 to 9HT2M10 | 00:1A:2B:60:01:01 – :10 | DHCP 10.10.60.x / VLAN 60 | Office / mobile |
| HW-07 | Engineering workstations (×5) | ENG-01 to ENG-05 | Dell Precision 3680 | 3LP7V01 to 3LP7V05 | 00:1A:2B:20:02:01 – :05 | 10.10.20.50–.54 / VLAN 20 | Engineering office |
| HW-08 | Network printers (×3) | PRN-01 to PRN-03 | HP LaserJet Enterprise | VNB3K11401–03 | 00:1A:2B:20:03:01 – :03 | 10.10.20.30–.32 / VLAN 20 | Office |
| HW-09 | PLCs (×10) | PLC-01 to PLC-10 | Industrial PLC controller | PLC5580-0001 to -0010 | 00:1A:2B:30:01:01 – :10 | 10.10.30.10–.19 / VLAN 30 | Factory floor |
| HW-10 | HMI touch panels (×5) | HMI-01 to HMI-05 | Industrial HMI panel | HMI7000-0001 to -0005 | 00:1A:2B:30:02:01 – :05 | 10.10.30.50–.54 / VLAN 30 | Factory floor |
| HW-11 | IP security cameras (×30) | CAM-01 to CAM-30 | PoE dome/bullet cameras | IPC4K-26001 to -26030 | 00:1A:2B:40:01:01 – :30 | 10.10.40.10–.39 / VLAN 40 | Office, factory, exterior |

### 2.6 Networking Assets

| Asset ID | Device / Service | Hostname | Model | Serial Number | MAC Address | Management IP / VLAN | Location |
|---|---|---|---|---|---|---|---|
| NET-01 | Next-generation firewall (gateway for all VLANs) | FW-01 | Fortinet FortiGate 100F | FG100FTK26001234 | 00:09:0F:10:00:01 | 10.10.10.1 / VLAN 10 | Server rack |
| NET-02 | Core Layer 3 switch | CORE-SW-01 | Cisco Catalyst 9300-24P | FOC2611A1BC | 00:1A:2B:10:00:02 | 10.10.10.2 / VLAN 10 | Server rack |
| NET-03 | Office access switches (stacked) | OFF-SW-01, OFF-SW-02 | Cisco Catalyst 9200-48P | FOC2612A2BC, FOC2612A2BD | 00:1A:2B:10:00:11–12 | 10.10.10.11–.12 / VLAN 10 | Office comms closet |
| NET-04 | Industrial factory switch | FAC-SW-01 | Cisco IE3300 | FOC2613A3BC | 00:1A:2B:10:00:13 | 10.10.10.13 / VLAN 10 | Factory enclosure |
| NET-05 | Camera PoE+ switch | CAM-SW-01 | 48-port PoE+ switch | SW48P-260014 | 00:1A:2B:10:00:14 | 10.10.10.14 / VLAN 10 | Server rack |
| NET-06 | Access point PoE+ switch | AP-SW-01 | 24-port PoE+ switch | SW24P-260015 | 00:1A:2B:10:00:15 | 10.10.10.15 / VLAN 10 | Server rack |
| NET-07 | Server switch | SRV-SW-01 | 24-port gigabit switch | SW24G-260016 | 00:1A:2B:10:00:16 | 10.10.10.16 / VLAN 10 | Server rack |
| NET-08 | Wi-Fi 6 access points (×10) | AP-01 to AP-10 | Enterprise Wi-Fi 6 AP | WAP6-26101 to -26110 | 00:1A:2B:10:01:01 – :10 | 10.10.10.101–.110 / VLAN 10 | Office and factory ceilings |
| NET-09 | ISP router / business fiber connection | ISP-RTR | ISP-provided | ISP-88412290 | 00:1A:2B:00:00:01 | Public 203.0.113.10 | Server rack |
| NET-10 | Site-to-site IPsec VPN tunnel | VPN-CLOUD | Configured on FW-01 | N/A | N/A | 203.0.113.10 ↔ cloud gateway 198.51.100.20 | Firewall ↔ cloud |
| NET-11 | Cloud web server (VM) | WEB-01 | Cloud provider VM | N/A | N/A | Public 198.51.100.25 | Cloud provider |
| NET-12 | Cloud backup server / storage | BKP-01 | Cloud provider storage | N/A | N/A | Private, reached through VPN-CLOUD | Cloud provider |
| NET-13 | VLANs and subnets | VLAN 10–70 | 10.10.10.0/24 to 10.10.70.0/24 | N/A | N/A | Gateways (.1) on FW-01 | Logical |

### My Analysis

Expanding the inventory showed me that the company's assets are much more than just computers and switches.

The **data** assets are what attackers actually want, but they depend on every other asset type to stay protected. For example, the engineering designs (DAT-01) are only as safe as the people who can access them (PPL-03), the file server they are stored on (HW-02), and the backup procedure that protects them (PRC-01).

**People** were the asset type I had not thought about much before. Most of the threats I identified in Tutorial 8, such as phishing and disgruntled employees, involve people rather than technology. This is why security awareness training (PRC-09) and onboarding/offboarding (PRC-03) are listed as procedures.

Creating the inventory also made the network design easier to manage. Because the third octet of every IP address matches the VLAN number, it is easy to tell which network a device belongs to just from its address. Having serial numbers and MAC addresses recorded would also help the IT administrator identify unknown devices that appear on the network, which connects back to the monitoring objective DE.CM-01 in Task 1.

---

## Task 4 — Conduct a Risk Analysis

Our group's risk analysis was completed using the risk assessment template spreadsheet and is included in our group project submission, as the tutorial instructions specify.

- [Group Project Repository](https://github.com/Ctmuller/IS355_GroupProject)

---

## Tutorial 9 Reflection

This tutorial connected the security concepts from Tutorial 8 to a more organized management approach.

In Tutorial 8, I identified important assets and threats. In this tutorial, the NIST Cybersecurity Framework gave me a structured way to decide **which security objectives matter most** for those assets, and the asset inventory showed **exactly what** needs to be protected.

One of the biggest things I learned is that the Protect and Detect functions work together. No set of protections is perfect, so the company also needs a way to notice when an attack gets through. Segmentation and backups reduce the damage of an attack, while monitoring and log correlation help the company find it quickly.

I also learned that a complete asset inventory is the starting point for risk management. A company cannot protect, monitor, or assess the risk of an asset it does not know it has. The inventory from this tutorial will feed directly into our group's risk assessment, since each risk needs to be tied to a specific asset.
