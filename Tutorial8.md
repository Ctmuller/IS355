

# IS355

**Colin Muller**  
**Networking and Cyber Security — Tutorial 8: Attacks and Vulnerabilities**  
**November 2, 2026**

---

[Course Journal](README.md) | [Tutorial 7](Tutorial7.md) | [Group Project Repository](https://github.com/Ctmuller/IS355_GroupProject)

## Task 1 — CIA Protections

For this tutorial, I used our group's manufacturing company network design. The company operates from one location with an attached office and factory. Our proposed network uses seven VLANs to separate management, office computers, factory equipment, cameras, local servers, staff Wi-Fi, and guest Wi-Fi.

The CIA Triad focuses on three main areas of cybersecurity:

- **Confidentiality:** Making sure sensitive information can only be accessed by authorized people.
- **Integrity:** Making sure data and systems cannot be changed without authorization.
- **Availability:** Making sure systems, information, and equipment are accessible when needed.

Below are the important assets I identified in our proposed network and the protections they require.

| Important Asset | Network Location | CIA Protection | Reason |
|---|---|---|---|
| Factory machinery, PLCs, and HMIs | VLAN 30 — Factory | Integrity and Availability | An unauthorized change or network failure could stop production or cause machinery to operate incorrectly. |
| Engineering designs and prototype files | VLAN 50 — Servers | Confidentiality and Integrity | These designs are valuable intellectual property. Competitors should not be able to access them, and employees need to know the files are accurate. |
| Financial records | VLAN 50 — Servers | Confidentiality and Integrity | Financial information should only be accessible to authorized employees, and the information must remain accurate for business decisions. |
| User accounts and authentication services | VLAN 50 — Servers | Confidentiality, Integrity, and Availability | Passwords and permissions need to be protected. If authentication services fail, employees could lose access to their accounts. |
| Approximately 30 security cameras | VLAN 40 — Cameras | Availability and Integrity | Cameras need to remain operational so the company can monitor its facilities. Recordings also need to remain accurate and unaltered. |
| Network Video Recorder (NVR) | VLAN 50 — Servers | Confidentiality, Integrity, and Availability | Security footage needs to be stored securely and available when an incident occurs. |
| Core switch and firewall | VLAN 10 — Management | Integrity and Availability | These devices control network traffic. Unauthorized configuration changes or failures could affect the entire network. |
| Office computers and laptops | VLAN 20 and VLAN 60 | Confidentiality and Integrity | Employees use these devices to access company information. Malware or unauthorized access could expose or change important files. |
| Employee Wi-Fi | VLAN 60 — Staff Wi-Fi | Confidentiality and Availability | Employees need reliable wireless access while preventing unauthorized users from accessing internal systems. |
| Guest Wi-Fi | VLAN 70 — Guest Wi-Fi | Integrity and Availability | Guests should have internet access without being able to reach company systems. The network's isolation settings must remain secure. |
| Encrypted cloud backups | Cloud Backup Server | Confidentiality, Integrity, and Availability | Backups must be protected from unauthorized access and remain available if local files are lost or attacked by ransomware. |
| Public company website | Cloud Web Server | Integrity and Availability | Customers need to access the website, and unauthorized users should not be able to change its content. |

### My Analysis

I think the most important assets for our manufacturing company are the factory machinery, engineering designs, and central network equipment.

Factory machinery depends heavily on availability because an outage could stop production and cost the company money. Engineering designs need confidentiality because they represent valuable company information that competitors could potentially use. The core switch and firewall are also extremely important because problems with these devices could affect communication throughout the company.

Our proposed use of VLANs, access control lists (ACLs), restricted administrative access, a next-generation firewall, and encrypted backups helps address these concerns. These are planned protections in our design, rather than systems we have already installed or tested.

---

## Task 2 — Threat Sources and Motivation

For our network project, I identified several potential attackers who could target the manufacturing company.

The company has valuable engineering designs, financial records, industrial equipment, and internet-connected systems, which could make it a target for different types of cyberattacks.

| Threat Source | Motivation | Potential Impact on Our Network |
|---|---|---|
| Ransomware groups | Make money by encrypting company files and demanding payment | Could prevent employees from accessing engineering designs, financial records, or other important files. |
| Competitors or industrial espionage groups | Steal valuable engineering designs or confidential business information | Could obtain prototype information or intellectual property that provides a competitive advantage. |
| Phishing attackers | Steal employee login credentials or financial information | Could trick employees into giving attackers access to company accounts or internal systems. |
| Disgruntled employees | Retaliation, personal gain, or disruption | Could intentionally delete company files, misuse account permissions, or change equipment settings. |
| External hackers | Find weaknesses in internet-facing systems or outdated software | Could exploit vulnerabilities in the firewall, public website, or other exposed services. |
| Unauthorized Wi-Fi users | Obtain free internet access or gain access to restricted resources | Could attempt to move from the guest wireless network into internal company networks. |
| Compromised contractors or vendors | Use stolen credentials or existing access to enter the company network | Could attempt to access machinery, servers, or management systems. |
| Attackers targeting security cameras | Disrupt surveillance or use vulnerable connected devices to access other systems | Could interfere with camera recording or attempt to access the NVR and other network resources. |

### My Analysis

I think ransomware and phishing would be two of the biggest concerns for this company.

Employees regularly use computers, email, and shared files, so one person clicking a malicious link could potentially introduce malware into the network. Because the company also stores valuable engineering designs and financial records, an attacker could try to steal that information or prevent employees from accessing it.

This is one reason our group decided to separate the network into seven VLANs.

For example:

- **VLAN 20:** Office computers have limited access to authorized servers.
- **VLAN 30:** Factory equipment is separated from ordinary office traffic.
- **VLAN 40:** Security cameras can communicate with the NVR but cannot freely access other internal networks.
- **VLAN 60:** Employee Wi-Fi provides controlled access to internal resources.
- **VLAN 70:** Guest Wi-Fi is restricted to internet-only access.
- **VLAN 10:** Network management access is limited to authorized administrators.

These restrictions are intended to reduce the damage that could occur if an attacker compromises a single device.

I think network segmentation is especially important in a manufacturing environment because an attack on an office computer should not automatically give someone access to production machinery.

---

## Task 3 — Explore Vulnerabilities

For this task, I used the National Vulnerability Database (NVD) to research three CVEs published within the past 12 months.

I selected one Critical, one High, and one Medium vulnerability. I focused on networking equipment and software that could be relevant to the types of systems used in our manufacturing company's network.

### CVE 1 — Critical Vulnerability

**CVE-2026-76471 — Cisco NX-OS Remote Code Execution Vulnerability**

| Category | Details |
|---|---|
| CVE ID | CVE-2026-76471 |
| Published Date | October 7, 2026 |
| Company | Cisco |
| Affected Product | Cisco NX-OS Software |
| Severity | Critical |
| CVSS Version | 3.1 |
| CVSS Score | 9.8 |
| Confidentiality Impact | High |
| Integrity Impact | High |
| Availability Impact | High |
| CWE | CWE-122 — Heap-based Buffer Overflow |

**Product Description**

Cisco NX-OS is an operating system used on certain Cisco Nexus network switches and related networking equipment. These devices are commonly used to manage network traffic and connect systems in enterprise networks and data centers.

**Vulnerability Description**

This vulnerability affects the NX-API feature in certain Cisco NX-OS devices.

The issue occurs because the software does not properly validate some information sent to the API. An attacker could send a specially crafted HTTP request that causes the system to process memory incorrectly.

This could allow an unauthenticated attacker to execute unauthorized code with root privileges or cause the affected device to crash and reload.

**Simple Explanation**

In simpler terms, an attacker could send malicious information to a vulnerable network switch and potentially take control of the device or cause it to stop working.

Because network switches are responsible for connecting different parts of a network, this could become a serious problem for an organization.

**Detection and Mitigation**

- Identify network equipment running affected Cisco NX-OS software versions.
- Determine whether the vulnerable NX-API feature is enabled.
- Update affected equipment to a software version that contains Cisco's security fix.
- Monitor for unusual management requests, device crashes, and unexpected reboots.
- Restrict access to management services wherever possible.

Cisco states that there is no general workaround that completely addresses this vulnerability, so installing a fixed software release is important.

**Connection to Our Project**

Our group is recommending a Cisco Catalyst 9300 Layer 3 switch or an equivalent device as the network's core switch.

The Catalyst 9300 does not run Cisco NX-OS, so this specific vulnerability does not directly apply to our proposed switch.

However, it demonstrates why network administrators need to keep switching equipment updated and restrict management access.

If a similar vulnerability affected our core switch, it could disrupt connectivity between the factory, office, servers, and other VLANs.

**Sources**

- [NIST National Vulnerability Database — CVE-2026-76471](https://nvd.nist.gov/vuln/detail/CVE-2026-76471)
- [Cisco Security Advisory](https://sec.cloudapps.cisco.com/security/center/content/CiscoSecurityAdvisory/cisco-sa-napi-rce-r2shwu2j)

### CVE 2 — High Vulnerability

**CVE-2026-9940 — Google Chrome Heap Buffer Overflow**

| Category | Details |
|---|---|
| CVE ID | CVE-2026-9940 |
| Published Date | May 28, 2026 |
| Company | Google |
| Affected Product | Google Chrome |
| Severity | High |
| CVSS Version | 3.1 |
| CVSS Score | 8.8 |
| Confidentiality Impact | High |
| Integrity Impact | High |
| Availability Impact | High |
| CWE | CWE-122 — Heap-based Buffer Overflow |

**Product Description**

Google Chrome is a web browser used to access websites, online applications, and other internet resources.

It is widely used by businesses and individuals, including employees who need access to online systems.

**Vulnerability Description**

This vulnerability involves a heap buffer overflow in Chrome's ANGLE graphics component.

An attacker could create a malicious webpage that causes the browser to improperly handle data in memory.

This memory corruption could potentially be exploited when a user visits the crafted webpage.

The vulnerability requires user interaction, such as opening a malicious website.

**Simple Explanation**

An attacker could create a webpage designed to exploit a weakness in how Chrome processes graphics.

If an employee visits that page while using an outdated version of Chrome, the vulnerability could potentially be used to compromise the browser or cause harmful behavior.

**Detection and Mitigation**

- Keep Google Chrome updated to a version containing the security fix.
- Regularly check browser versions on company computers.
- Use automatic software updates.
- Train employees to recognize suspicious links and websites.
- Monitor devices for unusual browser crashes or endpoint security alerts.

**Connection to Our Project**

Our company has approximately 30–40 office employees who use computers and laptops.

These employees would likely need web browsers for everyday tasks.

A browser vulnerability could become an entry point for an attacker, especially if an employee visits a malicious website.

This shows why endpoint security is important even when a company already has firewalls and network segmentation.

**Sources**

- [NIST National Vulnerability Database — CVE-2026-9940](https://nvd.nist.gov/vuln/detail/CVE-2026-9940)
- [Google Chrome Security Update — May 27, 2026](https://chromereleases.googleblog.com/2026/05/stable-channel-update-for-desktop_0877304591.html)

### CVE 3 — Medium Vulnerability

**CVE-2026-20173 — Cisco NX-OS Denial-of-Service Vulnerability**

| Category | Details |
|---|---|
| CVE ID | CVE-2026-20173 |
| Published Date | October 7, 2026 |
| Company | Cisco |
| Affected Product | Cisco NX-OS Software |
| Severity | Medium |
| CVSS Version | 3.1 |
| CVSS Score | 5.8 |
| Confidentiality Impact | None |
| Integrity Impact | None |
| Availability Impact | Low |
| CWE | CWE-770 — Allocation of Resources Without Limits or Throttling |

**Product Description**

Cisco NX-OS is used to operate and manage certain Cisco Nexus switches.

These devices are responsible for networking functions such as switching, routing, and communication between connected systems.

**Vulnerability Description**

This vulnerability occurs because some network protocols do not have rate limiting applied correctly.

An attacker could send a large amount of UDP or TCP traffic to a vulnerable device.

This could exhaust system resources and temporarily interrupt routing and other network control functions.

The main concern is a denial-of-service (DoS) condition.

**Simple Explanation**

An attacker could overwhelm a vulnerable network switch by sending too much traffic.

The switch could become unstable or temporarily lose the ability to handle certain network communications properly.

Unlike some other vulnerabilities, this attack is mainly concerned with disrupting availability rather than stealing or changing information.

**Detection and Mitigation**

- Monitor network traffic for unexpected spikes.
- Watch for packet loss and unusual routing behavior.
- Identify affected Cisco NX-OS versions.
- Update vulnerable equipment to a fixed software release.
- Investigate abnormal traffic patterns and network instability.

Cisco states that there is no workaround that fully fixes the vulnerability, so updating the software is recommended.

**Connection to Our Project**

Our proposed network relies on switches to connect factory equipment, office computers, security cameras, and servers.

Even a temporary network disruption could interfere with manufacturing operations or security monitoring.

While this specific vulnerability affects Cisco NX-OS rather than our proposed Catalyst 9300, it demonstrates why network availability and software maintenance are important.

**Sources**

- [NIST National Vulnerability Database — CVE-2026-20173](https://nvd.nist.gov/vuln/detail/CVE-2026-20173)
- [Cisco Security Advisory](https://sec.cloudapps.cisco.com/security/center/content/CiscoSecurityAdvisory/cisco-sa-nxos-nscpdos-SnderkC7)

### Task 3 Reflection

After researching these three vulnerabilities, I noticed that not every cyberattack has the same objective.

Some vulnerabilities allow attackers to gain control of systems or access confidential information, while others are primarily intended to disrupt services.

The Cisco vulnerabilities were particularly interesting because our group is designing a network that depends on switches to keep the factory and office connected.

The Google Chrome vulnerability also shows how an ordinary employee computer could become a security risk.

I think one of the biggest lessons is that organizations need to regularly update software, monitor their networks, and restrict access to important systems.

---

## Task 4 — Vulnerability Disclosures

I think security researchers should generally report vulnerabilities privately to the company responsible for the affected software before making the information public.

If a vulnerability becomes public before the company has time to fix it, attackers could take advantage of the weakness while customers are still using vulnerable systems.

For that reason, I think vendors should have a reasonable amount of time to investigate the issue and develop a patch.

However, I also do not think companies should be allowed to hide vulnerabilities indefinitely.

If a company knows that its software contains a serious security problem, customers deserve to know so they can take steps to protect themselves.

### How Much Time Should a Vendor Have?

I think approximately **90 days** is a reasonable starting point for coordinated vulnerability disclosure.

This gives the company time to investigate the problem, develop a fix, and notify affected customers.

However, I think the deadline should depend on the severity of the vulnerability.

For example, if attackers are already exploiting a serious security flaw, the company should provide warnings and available protections much sooner.

On the other hand, if the problem is extremely complicated and the vendor is actively working on a solution, allowing additional time might be reasonable.

### Should Researchers Disclose Vulnerabilities Without Permission?

I think researchers should first make a reasonable effort to work with the vendor.

However, if the company ignores the issue or refuses to address a serious security problem, I think the researcher may eventually be justified in making the vulnerability public.

Ideally, the researcher should provide advance warning and coordinate with a security organization when possible.

I would not support immediately publishing detailed instructions that make it easier for attackers to exploit the vulnerability.

The main goal should be protecting users rather than embarrassing the company.

### Bug Bounty Programs

I think bug bounty programs are a good idea because they encourage researchers to report vulnerabilities responsibly.

Instead of trying to exploit a vulnerability for personal gain, researchers can receive recognition or financial rewards for helping companies improve their security.

Overall, I think coordinated vulnerability disclosure is the best approach because it balances the company's need for time to fix problems with the public's need to know about security risks.

**Sources**

- [OWASP Vulnerability Disclosure Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/Vulnerability_Disclosure_Cheat_Sheet.html)
- [Microsoft Coordinated Vulnerability Disclosure](https://www.microsoft.com/en-us/msrc/cvd)

---

## Task 5 — OWASP Top 10 Vulnerabilities

### What Is the Purpose of the OWASP Top 10?

The OWASP Top 10 is a list of major security risks affecting web applications.

It is published by the Open Worldwide Application Security Project (OWASP), an organization focused on improving software security.

The purpose of the list is to help developers, cybersecurity professionals, and organizations understand common vulnerabilities that could make web applications vulnerable to attacks.

It provides guidance on areas that should receive attention when developing, testing, and maintaining web applications.

### OWASP Top 10 — 2025

The OWASP Top 10:2025 includes the following categories:

| Rank | Security Risk | Description |
|---|---|---|
| A01 | Broken Access Control | Users are able to access information or functions they should not have permission to use. |
| A02 | Security Misconfiguration | Incorrect security settings leave systems exposed to attacks. |
| A03 | Software Supply Chain Failures | Weaknesses involving software dependencies, packages, or development and distribution processes. |
| A04 | Cryptographic Failures | Sensitive information is not adequately protected through encryption and other cryptographic controls. |
| A05 | Injection | Attackers manipulate application inputs to cause unintended commands or queries to execute. |
| A06 | Insecure Design | Security problems are introduced because applications were not designed with sufficient security protections. |
| A07 | Authentication Failures | Weaknesses in login and identity verification systems allow unauthorized access. |
| A08 | Software or Data Integrity Failures | Applications fail to properly verify the integrity of software, updates, or data. |
| A09 | Security Logging and Alerting Failures | Inadequate monitoring and alerts make it harder to identify and respond to security incidents. |
| A10 | Mishandling of Exceptional Conditions | Applications respond improperly to unexpected errors or conditions, potentially creating security weaknesses. |

### Why Is the OWASP Top 10 Important?

I think the OWASP Top 10 is important because it gives developers and security professionals a clear understanding of some of the biggest risks they should consider.

Developers can use it to recognize common mistakes while building applications instead of waiting until a security incident occurs.

For example, understanding broken access control and authentication failures can help developers design systems that properly restrict access to information.

Cybersecurity professionals can use the list when performing security reviews, testing applications, and identifying potential weaknesses.

I also think it is helpful because developers and security professionals do not always approach problems in the same way.

The OWASP Top 10 provides a common reference that both groups can use when discussing security risks.

### Connection to Our Group Project

Our manufacturing company plans to host a public website through a cloud provider.

Even though our internal network will use VLANs, ACLs, and a next-generation firewall, those protections do not automatically prevent vulnerabilities in the website itself.

For example:

- Broken access control could allow unauthorized users to access restricted website functions.
- Security misconfigurations could expose information or services that should not be publicly accessible.
- Authentication failures could allow someone to gain access to an administrative account.
- Software supply chain failures could introduce vulnerabilities through outdated or compromised dependencies.

Using the OWASP Top 10 would help the company identify these problems and improve the security of its public-facing website.

### Task 5 Reflection

I think the OWASP Top 10 is a useful resource because it helps organizations focus on preventing common security problems before they become more serious.

It also reinforces that cybersecurity involves more than just network equipment.

Even a company with a well-designed internal network can still experience security incidents if its websites and applications contain vulnerabilities.

**Sources**

- [OWASP Top 10](https://owasp.org/projects/top-ten/)
- [OWASP Top 10:2025](https://top10.owasp.org/2025/0x00_2025-Introduction/)

---

## Tutorial 8 Reflection

This tutorial helped me better understand how cybersecurity relates to the network our group is designing.

Before this, most of our project work focused on equipment, IP addressing, VLANs, and connecting the different parts of the manufacturing company.

After looking at the CIA Triad and researching vulnerabilities, I can see why identifying important assets and understanding potential attackers is just as important as designing the physical network.

One of the biggest things I took away from this tutorial is that security needs to be considered throughout the entire network.

Separating factory machinery, office computers, security cameras, and servers into different VLANs is an important first step, but the company also needs software updates, monitoring, access restrictions, and backups.

I think what we covered in this tutorial will be especially useful when our group completes the risk assessment and recommends additional security controls for our final project.
