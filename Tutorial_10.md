# IS355: Tutorial 10 - Cyber Security Controls

---

[Course Journal](Main.md) | [Tutorial 9](Tutorial9.md) | [Tutorial 11](Tutorial_11.md) | [Group Project Repository](https://github.com/Ctmuller/IS355_GroupProject)

## Table of Contents

- [Task 1 — Essential Eight Mitigation Strategies](#task-1--essential-eight-mitigation-strategies)
- [Task 2 — Explore and Select NIST Controls](#task-2--explore-and-select-nist-controls)
- [Tasks 3–5 — Optional Tasks](#tasks-35--optional-tasks)
- [Tutorial 10 Reflection](#tutorial-10-reflection)

---

## Task 1 — Essential Eight Mitigation Strategies

I continued using our group's manufacturing company scenario: one site with an attached office and factory, seven VLANs (management, office, factory OT, cameras, servers, staff Wi-Fi, guest Wi-Fi), local servers for confidential files, and cloud-hosted web and backup servers. The asset IDs below (PPL, DAT, SW, HW, NET) come from the asset inventory I built in [Tutorial 9](Tutorial9.md), and the risk IDs (R1–R10) come from our group's [risk register](https://github.com/Ctmuller/IS355_GroupProject/blob/main/Risk_Assessment.md).

The **Essential Eight**, published by the Australian Cyber Security Centre (ACSC), is split into three groups:

| Group | Strategies |
|---|---|
| Prevent malware delivery and execution | Application control, Patch applications, Configure Microsoft Office macro settings, User application hardening |
| Limit the extent of cyber security incidents | Restrict administrative privileges, Patch operating systems, Multi-factor authentication |
| Recover data and system availability | Regular backups |

I selected four strategies from all three groups:

| # | Strategy | Group | Main Risks Addressed |
|---|---|---|---|
| 1 | **Application control** | Prevent malware delivery and execution | R1 Ransomware, R3 Malware spreading to factory systems |
| 2 | **Restrict administrative privileges** | Limit the extent of cyber security incidents | R2 Stolen credentials, R4 Network device compromise, R5 Insider theft |
| 3 | **Multi-factor authentication** | Limit the extent of cyber security incidents | R2 Stolen credentials, R7 Contractor access misuse |
| 4 | **Regular backups** | Recover data and system availability | R1 Ransomware, R6 Site outage |

### Strategy 1 — Application Control

**Group:** Prevent malware delivery and execution

**How I would apply it in our project:**  
Application control means only approved programs, scripts, and installers are allowed to run. Everything else is blocked by default. I would roll it out in two stages:

1. **Factory HMIs (HW-10) and the engineering workstations (HW-07) first.** These are fixed-function machines. An HMI panel only ever needs to run the HMI runtime from the PLC/HMI software suite (SW-11), and an engineering workstation only needs the CAD software (SW-09), the PLC programming software, and Microsoft 365 (SW-04). Because the list of approved software is short and rarely changes, an allowlist is easy to maintain here, and it gives the most protection to the machines closest to the production line.
2. **Office desktops and laptops (HW-05, HW-06) and the servers (DC-01, FS-01) second.** I would use Windows Defender Application Control (WDAC) or AppLocker, managed through Active Directory Group Policy from DC-01, with rules based on publisher certificates (for example, allow anything signed by Microsoft, Autodesk, or the ERP vendor) instead of individual file hashes, so normal software updates do not break the rules.

Blocked execution attempts would be logged and forwarded to the SIEM (SW-15), so the IT Administrator (PPL-01) can see when someone tries to run something unapproved.

**Why it matters for our scenario:**  
Most ransomware (R1) arrives as an executable or script that a user is tricked into running from an email attachment or download folder. With application control, that file simply does not run, even if the user clicks it. It also stops malware from launching on HMIs and engineering workstations if it reaches them, which directly reduces R3.

### Strategy 2 — Restrict Administrative Privileges

**Group:** Limit the extent of cyber security incidents

**How I would apply it in our project:**

- **Separate admin accounts.** The IT Administrator (PPL-01) gets two accounts: a normal account for email, web browsing, and daily work, and a separate privileged account used only for administration. The privileged account is blocked from email and internet browsing.
- **No local admin rights for staff.** Office staff, engineers, finance, and factory operators (PPL-02 to PPL-06) use standard accounts. Software installs go through IT.
- **Administration only from the management network.** The firewall (FW-01), core switch (CORE-SW-01), access switches, and access points can only be managed from VLAN 10, and only from the IT Administrator's admin device. This matches the "Unauthorized devices → Management VLAN 10: Denied" rule in our network design.
- **Contractors get just-in-time access.** Vendor accounts for PLC or NVR support (PPL-07) are disabled by default. They are enabled for a specific job, routed through the factory jump host, and disabled again afterward (procedure PRC-03).
- **Quarterly review.** The IT Administrator and HR review the membership of Domain Admins and other privileged groups every quarter and remove anything that is no longer needed.

**Why it matters for our scenario:**  
If an attacker steals a normal office user's password (R2), the damage is limited to what that user can access. They cannot install software, change firewall rules, or turn off Defender. If the stolen account were an administrator account, the attacker would effectively own the whole network, including the PLCs and backups. Restricting admin rights also reduces insider risk (R5), since engineers cannot grant themselves access to finance or HR shares.

### Strategy 3 — Multi-Factor Authentication

**Group:** Limit the extent of cyber security incidents

**How I would apply it in our project:**  
MFA would be required for, in priority order:

1. **All privileged accounts** — the IT Administrator's admin account on Active Directory (DC-01), FortiGate (FW-01), switch management, and the cloud provider console that controls the web server and backup server (NET-11, NET-12).
2. **Remote access** — the FortiGate remote-access VPN for staff working from home and for contractors (PPL-07) connecting to the factory jump host.
3. **Internet-facing services** — Microsoft 365 email (DAT-08) and the website CMS admin login (SW-14).
4. **Sensitive internal data** — access to the finance and HR shares (DAT-02, DAT-03) and the ERP system (SW-10).

I would use phishing-resistant methods where possible: FIDO2 security keys for the IT Administrator, and an authenticator app with number matching for other staff, instead of SMS codes.

**Why it matters for our scenario:**  
Phishing is the most likely way into the company (R2 is scored High with likelihood 4). MFA means a stolen password on its own is not enough to log in to email, the VPN, or the cloud console. It is especially important for the contractor VPN (R7), because a vendor's laptop is outside the company's control.

### Strategy 4 — Regular Backups

**Group:** Recover data and system availability

**How I would apply it in our project:**

| What | Frequency | Where | Protection |
|---|---|---|---|
| File server shares: engineering, finance, HR (DAT-01 to DAT-03) | Daily incremental, weekly full | Local backup repository + cloud backup server (NET-12) | Encrypted on-site before upload (SW-13), sent over the site-to-site VPN |
| Active Directory system state (DC-01, DAT-04) | Daily | Local + cloud | Encrypted |
| PLC programs and recipes (DAT-06) | After every approved change (PRC-06) and weekly | `\\FS-01\Engineering\PLC` + cloud | Versioned so an older working program can be restored |
| Firewall and switch configurations (DAT-05) | After every change (PRC-05) and weekly | FS-01 + cloud | Encrypted |
| Public website (DAT-12) | Daily | Cloud provider snapshots | Separate from the main backup account |

The cloud backups would be **immutable** for 30 days, so they cannot be changed or deleted, even by an administrator account. Backup administration would use a dedicated account that is separate from the normal domain admin account. Following procedure PRC-01, I would run a **monthly restore test**: restore a sample engineering folder and one PLC program to a test location and confirm they open correctly.

**Why it matters for our scenario:**  
Ransomware (R1) is our only Critical risk. Prevention can fail, so the company needs a guaranteed way to recover without paying. Modern ransomware groups specifically look for backups and delete them first, which is why immutability and a separate backup account matter. Backups of PLC programs and switch configurations also help with R6: after a hardware failure, the replacement device can be restored quickly instead of being reconfigured from memory.

### Why I Chose These Four (and Not the Others)

I chose these four because they map most directly to our highest risks. Between them, they cover all three Essential Eight groups and the full timeline of an attack: stop the malware from running (application control), limit what an attacker can do if they get in (admin restrictions and MFA), and recover if everything else fails (backups). Three of them also line up with controls already in our group's risk assessment (C1 MFA, C2 backups, C7 least privilege), which keeps my journal and the project consistent.

| Strategy Not Selected | Why |
|---|---|
| **Patch applications** and **Patch operating systems** | Both are very important, and our project already covers them through procedure PRC-02 and control C4. I chose application control instead because it adds protection that patching does not: it blocks unknown malware even when no patch exists yet. Patching is also harder on the factory side, where PLC and HMI updates need vendor approval and planned downtime. |
| **Configure Microsoft Office macro settings** | Relevant because the company uses Microsoft 365, but it addresses one specific delivery method. Application control and MFA cover a wider range of phishing attacks. It would be my next choice if I could pick a fifth. |
| **User application hardening** | Useful for office browsers, but it mainly protects office PCs (Medium-value assets), while my chosen strategies also protect the servers, factory systems, and backups. |

**Sources**

- [Australian Cyber Security Centre — Essential Eight](https://www.cyber.gov.au/resources-business-and-government/essential-cyber-security/essential-eight)
- [ACSC — Essential Eight Maturity Model](https://www.cyber.gov.au/resources-business-and-government/essential-cyber-security/essential-eight/essential-eight-maturity-model)

---

## Task 2 — Explore and Select NIST Controls

Using the NIST SP 800-53 Rev. 5 control catalog spreadsheet from Moodle, I selected **six base controls from six different families**. I ignored the control enhancements, as the task instructions specify.

| # | Control | Family | Related Essential Eight / Project Control |
|---|---|---|---|
| 1 | **AC-6** Least Privilege | Access Control (AC) | Restrict admin privileges; C7 |
| 2 | **IA-2** Identification and Authentication (Organizational Users) | Identification and Authentication (IA) | MFA; C1 |
| 3 | **SC-7** Boundary Protection | System and Communications Protection (SC) | C3 segmentation |
| 4 | **CP-9** System Backup | Contingency Planning (CP) | Regular backups; C2 |
| 5 | **SI-2** Flaw Remediation | System and Information Integrity (SI) | Patching; C4 |
| 6 | **AU-6** Audit Record Review, Analysis, and Reporting | Audit and Accountability (AU) | C5 SIEM |

### Control 1 — AC-6 Least Privilege

**Family:** Access Control (AC)

**What it requires:** Users and processes get only the access they need to do their job, and nothing more.

**Why it is important for our project:**  
Our most valuable data (engineering designs, financial records, HR records) all sits on one file server, FS-01. Without least privilege, any employee account could read all of it, so one compromised account or one disgruntled employee could expose everything. This control is the main defense against insider theft of engineering designs (R5).

**How I would implement it:**

- Create Active Directory security groups by role: `GRP-Engineering`, `GRP-Finance`, `GRP-HR`, `GRP-Factory`.
- Set share permissions so `\\FS-01\Engineering` is only accessible to `GRP-Engineering`, `\\FS-01\Finance` only to `GRP-Finance`, and `\\FS-01\HR` only to `GRP-HR`.
- Factory operator accounts (PPL-05) can only log in to HMIs on VLAN 30 and have no access to the office file shares.
- Combined with the Essential Eight admin restrictions in Task 1: no local admin rights for staff, and separate admin accounts for IT.

### Control 2 — IA-2 Identification and Authentication (Organizational Users)

**Family:** Identification and Authentication (IA)

**What it requires:** Every user is uniquely identified and authenticated before accessing company systems. The enhancements (which I am not counting as separate controls) add multi-factor authentication for privileged and network access.

**Why it is important for our project:**  
Shared logins make it impossible to tell who did what. In a factory this is a real concern, because HMIs and engineering workstations are often left logged in under one shared account. If a PLC program is changed incorrectly, the company needs to know which person made the change.

**How I would implement it:**

- Every employee gets their own Active Directory account. No shared accounts, including on the HMIs (HW-10); operators log in with their own credentials or badge.
- Contractors (PPL-07) get individual named accounts, never a shared "vendor" login.
- MFA is required on the VPN, Microsoft 365, the cloud console, and all admin accounts, as described in Task 1.
- Default passwords on the IP cameras (HW-11), access points, and switches are changed during installation, because those are also accounts that authenticate users.

### Control 3 — SC-7 Boundary Protection

**Family:** System and Communications Protection (SC)

**What it requires:** Monitor and control communications at the external boundary of the system and at key internal boundaries, and connect to external networks only through managed interfaces.

**Why it is important for our project:**  
Our design depends on boundaries: between the internet and the company, between the office and the factory, and between the guest Wi-Fi and everything else. SC-7 is the control that makes those boundaries enforceable. It directly reduces malware spreading from the office to the factory (R3) and guest users reaching internal systems (R10).

**How I would implement it:**

- **External boundary:** The FortiGate firewall (FW-01) is the only path to the internet and to the cloud servers, with IPS and web filtering enabled. Inbound connections from the internet to internal VLANs are denied.
- **Internal boundaries:** Deny-by-default rules between VLANs, allowing only the flows in our traffic rules table. For example, Office VLAN 20 → Factory VLAN 30 is denied, Camera VLAN 40 can reach only the NVR, and Guest VLAN 70 can reach only the internet.
- **Managed interface for the factory:** All access to PLCs and HMIs goes through the factory jump host, so there is one controlled, logged entry point into VLAN 30.
- **Cloud connection:** The backup server is reached only through the site-to-site IPsec VPN (NET-10). The web server sits behind a web application firewall.

### Control 4 — CP-9 System Backup

**Family:** Contingency Planning (CP)

**What it requires:** Back up user data, system data, and system documentation regularly, and protect the confidentiality, integrity, and availability of the backups.

**Why it is important for our project:**  
Ransomware (R1) is our only Critical risk, and backups are the control that decides whether the company recovers in hours or loses its engineering designs permanently. CP-9 also requires protecting the backups themselves, which matters because the backup copies (DAT-11) are themselves a Restricted asset containing all of the company's IP.

**How I would implement it:**

- Follow the backup schedule from Task 1: daily file server and AD backups, PLC programs and network configs after every change.
- Encrypt backups on-site before upload, so the cloud provider never sees unencrypted data.
- Keep immutable copies for 30 days in the cloud backup server, plus a local copy for fast restores.
- Back up **documentation** too, since CP-9 explicitly includes it: the procedures in `\\FS-01\IT\Procedures`, the network diagram, and the asset inventory. Without these, rebuilding after a disaster would be much slower.

### Control 5 — SI-2 Flaw Remediation

**Family:** System and Information Integrity (SI)

**What it requires:** Identify, report, and correct system flaws, test updates before installing them, and install security-relevant updates within defined time frames.

**Why it is important for our project:**  
In Tutorial 8, I researched recent CVEs in Cisco network devices and Chrome, which showed how often vulnerabilities appear in the exact products our design uses. An unpatched firewall or switch could be exploited remotely (R4), and outdated camera firmware or an old website CMS are common entry points (R8, R9).

**How I would implement it:**

| Asset Type | Patch Window | Notes |
|---|---|---|
| Firewall, VPN, internet-facing services (FW-01, WEB-01, CMS) | Within 48 hours for critical CVEs | Highest exposure to the internet |
| Servers, PCs, Microsoft 365, Chrome | Monthly cycle (Patch Tuesday), critical within 2 weeks | Test on a small pilot group first |
| Switches, access points, cameras, NVR | Monthly review of vendor advisories | Often forgotten, so tracked in the asset inventory |
| PLCs and HMIs | During planned maintenance windows, after vendor approval | Production cannot stop without planning; compensating control is SC-7 isolation |

The asset inventory from Tutorial 9 makes this possible, because it records the software version on each device (for example, FortiOS 7.4.x and Cisco IOS XE 17.x), so the IT Administrator can quickly check whether a new advisory affects the company.

### Control 6 — AU-6 Audit Record Review, Analysis, and Reporting

**Family:** Audit and Accountability (AU)

**What it requires:** Review and analyze audit records regularly for signs of inappropriate or unusual activity, and report findings to the right people.

**Why it is important for our project:**  
Logs are only useful if someone looks at them. Our design already collects logs on the firewall and servers, but with a small IT team, nobody will read them by hand. AU-6 supports the detection objectives from Tutorial 9 (DE.CM-01 and DE.AE-03) and helps catch stolen-credential attacks (R2) and data theft (R5) that look normal one event at a time.

**How I would implement it:**

- Forward logs from FW-01, DC-01, FS-01, Microsoft Defender (SW-06), and the jump host to the SIEM (SW-15).
- Set alerts for specific patterns: multiple failed logins followed by a success, a VPN login from an unusual country, an account downloading an unusually large number of engineering files, a camera trying to reach anything other than the NVR, and any change to firewall rules or Domain Admins membership.
- The IT Administrator reviews SIEM alerts daily and produces a short monthly summary for management.
- Serious findings trigger the incident response plan (PRC-04).

### My Analysis

The NIST controls and the Essential Eight overlap a lot, but they work at different levels. The Essential Eight is a short, practical list of technical actions, while SP 800-53 is a large catalog that also covers process and accountability. For example, the Essential Eight says "make backups"; CP-9 adds that the backups must be protected and must include documentation.

Choosing controls from six families helped me see that a good security program needs more than one type of control. AC-6, IA-2, and SC-7 **prevent** access, SI-2 **removes** vulnerabilities before they are used, AU-6 **detects** what gets through, and CP-9 makes sure the company can **recover**. That follows the same Protect → Detect → Recover thinking from the NIST CSF in Tutorial 9.

**Sources**

- [NIST SP 800-53 Rev. 5 — Security and Privacy Controls for Information Systems and Organizations](https://csrc.nist.gov/pubs/sp/800/53/r5/upd1/final)
- NIST SP 800-53 Control Catalog spreadsheet (provided on Moodle)

---

## Tasks 3–5 — Optional Tasks

Tasks 3 (encrypt a file with 7-Zip), 4 (view password information in `/etc/shadow`), and 5 (key-based SSH authentication) are optional and were not attempted this week.

---

## Tutorial 10 Reflection

This tutorial moved from identifying risks to choosing specific controls. The biggest lesson for me was that controls are most useful when they are tied to a specific asset and a specific risk. "Use MFA" is general advice, but "require MFA on the contractor VPN that leads to the factory jump host, because of risk R7" is something a company could actually implement and test.

I also noticed how much the factory changes the picture. Many standard controls, such as monthly patching, are hard to apply to PLCs and HMIs because production cannot just stop. In those cases, the answer is usually a compensating control, like stronger network isolation (SC-7) and application control on the HMIs, rather than skipping the protection altogether.

Finally, working through this tutorial helped me check our group project for consistency. The controls I chose here map directly to C1–C8 in our risk assessment, which gives each project control a reference to a recognized standard.
