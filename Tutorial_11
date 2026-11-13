**Networking and Cyber Security — Tutorial 11: Managing IT Services**  

---

[Course Journal](Main.md) | [Tutorial 10](Tutorial10.md) | [Tutorial 12](Tutorial12.md) | [Group Project Repository](https://github.com/Ctmuller/IS355_GroupProject)

## Table of Contents

- [Task 2 — Feedback on Project Report](#task-2--feedback-on-project-report)
  - [Project Status Before Feedback](#project-status-before-feedback)
  - [Self-Review: Issues I Found](#self-review-issues-i-found)
  - [Questions I Asked My Tutor](#questions-i-asked-my-tutor)
  - [Tutor Feedback](#tutor-feedback)
  - [Action Plan](#action-plan)
- [Tutorial 11 Reflection](#tutorial-11-reflection)

---

## Task 2 — Feedback on Project Report

### Project Status Before Feedback

Before meeting with my tutor, I reviewed every section of our [group project repository](https://github.com/Ctmuller/IS355_GroupProject) so I could ask specific questions instead of general ones.

| Report Section | File | Status |
|---|---|---|
| Project Plan | `Project_Plan.md` | Complete |
| Network Design | `Project_Draft.md` | Draft (v0.2) — open questions still listed |
| Network Diagram | `diagrams/network-architecture.drawio` / `.png` | Complete, but needs the new security components added |
| Cloud Services | `Cloud_Services.md` | Nearly complete — pricing and recommendation done |
| Security Risk Assessment and Controls | `Risk_Assessment.md` | Draft — 9 assets, 10 risks, 8 controls, residual risk |
| Project Reflection and Contribution Statement | Not started | To do |

### Self-Review: Issues I Found

Reviewing the whole report at once showed several places where sections written at different times no longer match each other.

| # | Issue | Where | Why It Matters |
|---|---|---|---|
| 1 | **IP addressing is inconsistent.** The network design uses the `54.10.x.0/24` scheme from the student ID rule, but the risk assessment (Section 6) uses `10.10.50.40` and `10.10.10.50` for the new SIEM and jump host. | `Project_Draft.md` §4 vs `Risk_Assessment.md` §6 | A marker comparing sections will see two different networks. |
| 2 | **Default gateway location is inconsistent.** The network design puts the VLAN gateways and ACLs on the Layer 3 core switch, but the risk assessment (control C3) says "the firewall as the gateway." | `Project_Draft.md` §3.2 vs `Risk_Assessment.md` §5 | Affects where segmentation is enforced and how much inter-VLAN traffic the firewall can inspect. |
| 3 | **Broken diagram links.** Section 7 of the network design links to `../diagrams/network-diagram.drawio` and `.png`, which do not exist. The real files are `diagrams/network-architecture.*`. | `Project_Draft.md` §7 | Images will not display on GitHub. |
| 4 | **Missing Azure screenshot.** The cloud services section links to `diagrams/Azure_Estimate.png`, but that file is not in the repository. | `Cloud_Services.md` §1.3 | Required pricing evidence is incomplete. |
| 5 | **Group members' backup approaches are incomplete.** Section 3 still has placeholder text for some members. | `Cloud_Services.md` §3 | Required by the project specification. |
| 6 | **Risk register not yet in the course template.** The risk assessment is written in Markdown but has not been entered into the required spreadsheet. | `Risk_Assessment.md` §7 | Required deliverable. |
| 7 | **Network diagram does not show the new security components** (SIEM, factory jump host, WAF). | `diagrams/` | The project requires the design to be updated where controls change the architecture. |
| 8 | **Project reflection and contribution statement not started.** | — | Required section. |

### Questions I Asked My Tutor

1. Is using `54.x.x.x` as the addressing prefix the correct interpretation of the student ID rule, given that it is a public range? Should we add a note explaining what we would use in a real deployment, or is the existing note enough?
2. For inter-VLAN routing, is it better to keep the gateways on the Layer 3 core switch (with ACLs) or move them to the firewall for deeper inspection? Which option fits the marking criteria better?
3. Is our risk assessment the right depth? We have 9 assets, 10 risks, and 8 controls, where the minimum is 6 assets and 6 controls.
4. Is mapping each control to both the NIST CSF 2.0 and the CIS Controls v8 enough justification, or should we also reference SP 800-53 controls like the ones I selected in Tutorial 10?
5. How should we present individual contributions in the reflection, given that some group members have made fewer commits than others?

### Tutor Feedback

> **To complete after the meeting:** record the tutor's feedback below in your own words.

| # | Area | Tutor's Feedback | My Response / Planned Change |
|---|---|---|---|
| 1 | Network design | | |
| 2 | Cloud services | | |
| 3 | Risk assessment and controls | | |
| 4 | Diagrams and evidence | | |
| 5 | Formatting / structure | | |

### Action Plan

| Priority | Action | Section | Target |
|---|---|---|---|
| 1 | Make the IP addressing and gateway location consistent across all sections (issues 1 and 2) | Network Design, Risk Assessment | Week 12 |
| 2 | Fix broken diagram links and add the missing Azure screenshot (issues 3 and 4) | Network Design, Cloud Services | Week 12 |
| 3 | Add the SIEM, jump host, and WAF to the network diagram (issue 7) | Diagrams | Week 12 |
| 4 | Enter the risk register into the course spreadsheet template and link it (issue 6) | Risk Assessment | Week 12 |
| 5 | Collect each member's backup approach (issue 5) | Cloud Services | Week 12 |
| 6 | Apply tutor feedback from the table above | All | Week 13 |
| 7 | Write the project reflection and contribution statement (issue 8) | Reflection | Week 13 |

---

## Tutorial 11 Reflection

This week's topic was managing IT services, and preparing for feedback felt like a small version of that: reviewing what has been delivered, checking it against the requirements, and planning improvements.

The most useful part was reading the whole report in one sitting. Each section looked fine on its own, but together they had contradictions, mainly the IP addressing and where the default gateway sits. These happened because the sections were written weeks apart and the design changed in between. In a real company, this is the reason for change management (procedure PRC-05 in my asset inventory): when the design changes, every document that depends on it has to be updated too.

Going in with specific questions also made the feedback more useful than asking "is this good?" would have.
