# IS355: Tutorial 12 - NICE Self-Assessment, Cyber Security Tool Kit, and Journal Wrap-Up

---

[Course Journal](Main.md) | [Tutorial 11](Tutorial11.md) | [Group Project Repository](https://github.com/Ctmuller/IS355_GroupProject)

## Table of Contents

- [Task 1 — NIST NICE Self-Assessment](#task-1--nist-nice-self-assessment)
- [Task 2 — Cyber Security Tool Kit](#task-2--cyber-security-tool-kit)
- [Task 3 — Finalize GitHub Journal](#task-3--finalize-github-journal)
- [Tutorial 12 and Course Reflection](#tutorial-12-and-course-reflection)

---

## Task 1 — NIST NICE Self-Assessment

The **NICE Cybersecurity Workforce Framework** (NIST SP 800-181) gives employers and students a common language for cybersecurity work. It groups the work into seven **Categories** (for example, Securely Provision, Operate and Maintain, and Oversee and Govern), divides those into **Specialty Areas**, and then into specific **Work Roles**. Each Work Role is defined by its Tasks and by the **Knowledge, Skills, and Abilities (KSAs)** needed to do them.

The course self-assessment spreadsheet lists 116 KSAs (82 Knowledge and 34 Skills) drawn from three Work Roles in the Operate and Maintain category:

| Work Role | NICE ID | Description |
|---|---|---|
| Network Operations Specialist | OM-NET-001 | Plans, implements, and operates network services/systems, including hardware and virtual environments |
| System Administrator | OM-ADM-001 | Sets up and maintains a system or specific components of a system |
| Systems Security Analyst | OM-ANA-001 | Analyzes and develops the integration, testing, operations, and maintenance of systems security |

I rated every KSA as **0 (None)**, **1 (Some)**, or **2 (Good)**, based on this course, my other Information Systems courses (including Systems Analysis and Design), and my work experience with databases and process automation.

- [My completed self-assessment spreadsheet](W12_nist-nice-self-assessment.xlsx)

### My Results

| Work Role | Type | Total | Good | Some | None |
|---|---|---|---|---|---|
| Network Operations Specialist | Knowledge | 39 | 14 (36%) | 20 (51%) | 5 (13%) |
| | Skills | 11 | 1 (9%) | 9 (82%) | 1 (9%) |
| System Administrator | Knowledge | 29 | 9 (31%) | 15 (52%) | 5 (17%) |
| | Skills | 14 | 2 (14%) | 3 (21%) | 9 (64%) |
| Systems Security Analyst | Knowledge | 46 | 14 (30%) | 24 (52%) | 8 (17%) |
| | Skills | 9 | 1 (11%) | 8 (89%) | 0 (0%) |

**Note on the Summary sheet:** While checking my results, I found three formula errors in the spreadsheet's Summary sheet. The figures in the table above are corrected.

- The "Some" count for **Skills** under System Administrator (cell C12) and Systems Security Analyst (cell C19) adds up the Network Operations Specialist column (column H) instead of their own columns (K and N). With my ratings, this showed System Administrator as 9 "Some" skills instead of 3, which made "Not assessed" display as −6.
- The "Not assessed" rows sum rows 2–82 instead of 2–83, so the last Knowledge item (K0622) is skipped. It did not change my results, but would if K0622 were rated 0.

### Strengths

My strongest area is **knowledge** across all three roles: at least 83% of the Knowledge items for each role are rated Some or Good. The items I rated Good line up with the work I did this semester:

- **Networking fundamentals and protocols** (K0001, K0061, K0332, K0111, K0113): TCP/IP, ARP, DNS, and DHCP from Tutorials 2–5, and packet analysis in Wireshark in Tutorial 7.
- **Network security architecture and design** (K0179, K0333, K0049): designing the group project's segmented, seven-VLAN network with a next-generation firewall.
- **Risk, threats, and controls** (K0002, K0005, K0297, K0287, S0147): the asset inventory, risk register, and control selection using NIST CSF, SP 800-53, the Essential Eight, and CIS Controls in Tutorials 8–10.
- **Databases** (K0024): from my Information Systems coursework and database and process automation work.

### Gaps and Next Steps

My biggest gap is **hands-on System Administrator skills**: 64% are rated None. This makes sense, because most of my work this semester was design and analysis on paper rather than running real servers. Items like recovering failed servers (S0157), installing upgrades (S0154), and maintaining Active Directory (S0043) are things I have described in the project but never done.

Across all three roles, Skills are much weaker than Knowledge. I know *what* should be done far better than I can currently *do* it.

To close these gaps, I would:

1. **Build a small home lab.** Run a Windows Server evaluation VM as a domain controller, join a client VM to it, and practice account management, Group Policy, backups, and restores. This turns the controls I wrote about in Tutorial 10 (separate admin accounts, application control, backup testing) into real experience.
2. **Complete the optional hands-on tasks** I skipped in Tutorial 10: key-based SSH, Linux user and password management in the OpenWRT VM, and file encryption.
3. **Study for CompTIA Network+ and Security+.** Both certifications map closely to the Network Operations Specialist and Systems Security Analyst knowledge areas.

Looking beyond these three roles, my career interests in business and information systems analysis fit other NICE Work Roles, such as **Data Analyst (OM-DTA-002)**, **Systems Requirements Planner (SP-SRP-001)**, **IT Project Manager (OV-PMA-002)**, and **IT Program Auditor (OV-PMA-005)**. The risk, controls, and governance knowledge from this course transfers directly to those roles.

**Sources**

- [NIST — NICE Framework Resource Center](https://www.nist.gov/itl/applied-cybersecurity/nice/nice-framework-resource-center)
- NICE Specialty Areas and Work Role KSAs and Tasks supplement (provided on Moodle)
- W12 NIST NICE Self-Assessment spreadsheet (provided on Moodle)

---

## Task 2 — Cyber Security Tool Kit

Over the semester, I used a set of tools and resources that together form a practical tool kit for networking and security work. I grouped them by what they are used for.

### Network Diagnostics and Configuration

| Tool / Command | Used In | What It Does | When I Would Use It |
|---|---|---|---|
| `Get-ComputerInfo` (PowerShell) | [Tutorial 1](Tutorial1.md) | Shows processor, memory, and OS details | Checking whether a machine meets requirements, or confirming a VM's specs |
| `Get-NetAdapter`, `Get-NetIPConfiguration` | [Tutorial 2](Tutorial2.md) | Shows network adapters, MAC addresses, IP address, gateway, and DNS servers | First step when a device cannot connect |
| `ping`, `Test-Connection`, `Test-NetConnection` | [Tutorial 2](Tutorial2.md) | Tests whether a host is reachable and whether a specific port is open | Isolating whether a problem is the network or the application |
| `Resolve-DnsName` | [Tutorial 2](Tutorial2.md) | Looks up DNS records (A, AAAA, MX, etc.) | Troubleshooting name resolution, checking a domain's mail servers |
| `arp -a` / `Get-NetNeighbor` | [Tutorial 3](Tutorial3.md) | Shows the IP-to-MAC mappings the computer has learned | Finding devices on the local network; spotting unknown devices |
| `Get-NetRoute` | [Tutorial 4](Tutorial4.md) | Shows the routing table | Checking which gateway traffic will use, especially with a VPN connected |
| `tracert` | [Tutorial 4](Tutorial4.md) | Shows each router hop between the computer and a destination | Finding where along the path latency or failures happen |
| Speed test | [Tutorial 2](Tutorial2.md) | Measures download, upload, and latency | Confirming an ISP is delivering the contracted bandwidth |
| macOS Wi-Fi details (Option-click) / router admin page | [Tutorial 6](Tutorial6.md) | Shows SSID, channel, band, signal strength, security settings | Diagnosing Wi-Fi problems and reviewing access point security |

### Network Analysis and Threat Research

| Tool / Resource | Used In | What It Does | When I Would Use It |
|---|---|---|---|
| Wireshark | [Tutorial 7](Tutorial7.md) | Captures and decodes packets layer by layer | Investigating suspicious traffic or proving where a connection fails |
| Malware-Traffic-Analysis.net | [Tutorial 7](Tutorial7.md) | Real packet captures of malware infections for practice | Learning what malicious traffic looks like before seeing it at work |
| IP geolocation lookup sites | [Tutorial 4](Tutorial4.md) | Estimates the location and owner of an IP address | Checking where a suspicious login or connection came from |
| National Vulnerability Database (NVD) | [Tutorial 8](Tutorial8.md) | CVE details, CVSS scores, and affected versions | Deciding how urgently a vulnerability needs to be patched |
| OWASP Top 10 | [Tutorial 8](Tutorial8.md) | The most critical web application security risks | Reviewing the security of a website, like our project's cloud web server |
| Submarine Cable Map, APNIC, AARNet, Hurricane Electric maps | [Tutorial 2](Tutorial2.md) | Shows the physical and logical structure of the internet | Understanding why international traffic is slower or takes certain paths |

### Security Frameworks and Standards

| Framework | Used In | What It Does | When I Would Use It |
|---|---|---|---|
| NIST Cybersecurity Framework (CSF) 2.0 | [Tutorial 9](Tutorial9.md) | Organizes security outcomes into Govern, Identify, Protect, Detect, Respond, Recover | Setting security objectives and explaining a security program to management |
| NIST SP 800-30 | Group project | Method for conducting a risk assessment | Scoring and prioritizing risks |
| NIST SP 800-53 Rev. 5 | [Tutorial 10](Tutorial10.md) | Detailed catalog of security controls | Choosing specific controls and showing they meet a recognized standard |
| ACSC Essential Eight | [Tutorial 10](Tutorial10.md) | Eight prioritized mitigation strategies with maturity levels | A practical starting point for a small company with limited IT staff |
| CIS Critical Security Controls v8 | Group project | Prioritized list of security actions | Mapping project controls to industry best practice |
| NICE Framework | [Tutorial 12](#task-1--nist-nice-self-assessment) | Describes cybersecurity work roles and skills | Career planning and identifying skills to build |

### Design and Documentation

| Tool | Used In | What It Does | When I Would Use It |
|---|---|---|---|
| draw.io (diagrams.net) | [Tutorial 3](Tutorial3.md), [Tutorial 5](Tutorial5.md), group project | Network and process diagrams | Documenting any network design or change |
| Git, GitHub, and Markdown | Whole course | Version control and documentation | Keeping a change history for configurations, scripts, and documentation |
| AWS and Azure pricing calculators | Group project | Estimates cloud costs | Comparing providers before recommending a cloud service |

### AI in Cyber Security

AI tools are quickly becoming part of every security tool kit, both for defenders and for attackers.

**How AI is used for defense:**

| Use | What It Does | Connection to My Work |
|---|---|---|
| Anomaly detection | Learns what "normal" traffic and logins look like and flags unusual behavior | The SIEM in our group project (control C5) would rely on this to spot stolen-credential logins that look normal one event at a time |
| Threat intelligence | Summarizes large volumes of advisories, CVEs, and threat reports | Would speed up the vulnerability research I did by hand in Tutorial 8 |
| Automated response | Automatically isolates an infected device or blocks a malicious IP | EDR tools like Microsoft Defender (control C8) can quarantine a PC that shows ransomware behavior |
| Productivity | Drafting reports, summarizing logs, writing scripts, and explaining unfamiliar commands | I used an AI assistant (Claude) to help structure and draft parts of this journal and the group project, and I credit it in the project's references |

**New risks that AI creates:**

- **AI-powered phishing and deepfakes.** AI can write convincing, error-free phishing emails and clone a manager's voice for a fake payment request. This makes security awareness training (control C6) and MFA (control C1) more important, because "look for spelling mistakes" is no longer reliable advice.
- **Prompt injection.** Hidden instructions in an email, document, or web page can trick an AI assistant into leaking data or taking actions the user did not intend. Any AI tool connected to company email or files needs the same least-privilege thinking as a user account (AC-6).
- **Data poisoning and adversarial attacks.** Attackers can corrupt the data a model learns from, or craft inputs that a detection model misclassifies, so AI-based defenses can be fooled.
- **Data leakage.** Pasting confidential data, like our scenario's engineering designs, into a public AI tool can expose it. Companies need a policy on which AI tools are approved and what data can go into them.

**Using AI responsibly:** The most important lesson for me is that AI output still needs to be checked. When I used AI to help with this course, I verified technical claims against the original sources (NIST, ACSC, vendor documentation) and checked that the details matched our actual project. While reviewing work in Tutorial 11, I caught inconsistencies such as mismatched IP addressing between sections. AI makes me faster, but I am still responsible for whether the result is correct.

### Top Three Tools I Will Keep Using

1. **PowerShell networking cmdlets.** At work, I use Airtable and fleet tracking systems that depend on reliable connectivity. Being able to check an IP configuration, test a port with `Test-NetConnection`, and trace a route lets me tell whether a problem is my network or the service.
2. **NIST CSF and the Essential Eight.** These give me a structured way to talk about security with management, which matters for a business analyst role, where I will need to explain *why* a control is worth the cost, not just *what* it is.
3. **Git and GitHub.** Keeping this journal in GitHub all semester showed me the value of version history. It is useful well beyond code, for documentation and process changes too.

---

## Task 3 — Finalize GitHub Journal

I reviewed the whole repository and made the following changes to finalize it:

| Change | Files |
|---|---|
| Added links to Tutorials 9–12 on the main journal page | `Main.md` |
| Added navigation links (Course Journal / previous / next tutorial) to tutorials that were missing them | `Tutorial1.md`, `Tutorial2.md` |
| Embedded the network map screenshots that were in the image folder but not shown | `Tutorial2.md` (Task 2) |
| Renamed the routing table screenshot to include its `.png` extension so it renders on GitHub, and updated the link | `IM/GetNetRoute` → `IM/GetNetRoute.png`, `Tutorial4.md` |
| Removed placeholder and duplicate files | `IM/speedtest.png` (text placeholder), `IM/Tutorial3IM`, `IM/readme.md`, `IM/screenshot-arp-table.png` (duplicate of `arp-table.png`) |
| Added Tutorials 10, 11, and 12, and the completed NICE self-assessment spreadsheet | `Tutorial10.md`, `Tutorial11.md`, `Tutorial12.md`, `W12_nist-nice-self-assessment.xlsx` |

---

## Tutorial 12 and Course Reflection

Looking back across the journal, the course built up in layers. The first half was about how networks actually work: what a computer knows about itself, how it finds other devices with ARP, how routing and DNS get traffic across the internet, and how wireless and cloud networks fit in. The second half used that foundation to think about security: what attackers want, what assets a company has, how to score risk, and which controls reduce it.

The group project tied the two halves together. Designing the manufacturing company's network first, then attacking it on paper through the risk assessment, showed me that good security decisions are mostly good network design decisions. Segmentation, a single controlled path into the factory, and a managed management network did more to reduce risk than any single product.

The other thing I will take away is the value of documentation. Writing this journal in Markdown, with screenshots and version history, made it easy to look back at earlier work and reuse it. The asset inventory from Tutorial 9 fed straight into the controls in Tutorial 10 and the self-review in Tutorial 11. I plan to keep using the same approach in my career.
