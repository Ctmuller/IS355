# IS355: Tutorial 11 - Managing IT Services

---

[Course Journal](Main.md) | [Tutorial 10](Tutorial10.md) | [Tutorial 12](Tutorial12.md) | [Group Project Repository](https://github.com/Ctmuller/IS355_GroupProject)

## Table of Contents

- [Task 2 — Feedback on Project Report](#task-2--feedback-on-project-report)
  - [Project Status Before Feedback](#project-status-before-feedback)
  - [Self-Review: Issues I Found](#self-review-issues-i-found)
  - [Tutor Feedback](#tutor-feedback)
  - [Open Questions and Follow-Up](#open-questions-and-follow-up)
  - [Action Plan](#action-plan)
- [Tutorial 11 Reflection](#tutorial-11-reflection)

---

## Task 2 — Feedback on Project Report

### Project Status Before Feedback

Before the feedback session, I reviewed every section of our [group project repository](https://github.com/Ctmuller/IS355_GroupProject) so that I could ask specific questions instead of general ones.

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

### Tutor Feedback

I was absent on the day the tutor reviewed the projects, so the feedback below was passed on to me by my group members.

The tutor looked through our GitHub repository and said the project looked great overall. He told us to continue with our current approach and to contact him with any questions. He did not ask for any specific changes to any section.

| # | Area | Tutor's Feedback | My Response / Planned Change |
|---|---|---|---|
| 1 | Network design | Covered by the overall comment that the project looks good. No specific changes requested. | Keep the design as it is, but still fix the addressing and gateway inconsistencies I found (issues 1 and 2). |
| 2 | Cloud services | No specific comments. | Finish the missing backup approaches and add the Azure screenshot (issues 4 and 5). |
| 3 | Risk assessment and controls | No specific comments. | Move the risk register into the course template (issue 6) and align control C3 with the network design. |
| 4 | Diagrams and evidence | No specific comments. | Fix the broken links and add the new security components to the diagram (issues 3 and 7). |
| 5 | Formatting / structure | The repository structure was easy to review. | Keep the same structure and add the missing reflection section (issue 8). |

Because the feedback was positive but general, it confirms that we are on the right track, but it does not answer the detailed questions I prepared. The issues from my self-review are still real problems, so I am treating them as the main source of improvements, and I am following up with the tutor directly on my questions.

### Open Questions and Follow-Up

Since I could not ask my questions in person, I am sending them to the tutor directly. For each one, I have recorded the decision we will use if we do not hear back before the deadline, so the work is not blocked.

| # | Question | Default Decision if No Reply | Status |
|---|---|---|---|
| 1 | Is using `54.x.x.x` as the addressing prefix the correct interpretation of the student ID rule, given that it is a public range? Should we explain what we would use in a real deployment? | Keep `54.10.x.0/24` to follow the rule, and expand the existing note to say a real deployment would use a private RFC 1918 range such as `10.0.0.0/8`. | Open — sent to tutor |
| 2 | For inter-VLAN routing, should the gateways stay on the Layer 3 core switch (with ACLs) or move to the firewall for deeper inspection? | Keep the gateways on the Layer 3 core switch, since that is what the network design and diagram already show. Rewrite control C3 to match, and route only the most sensitive traffic (such as access to the factory network) through the firewall. | Open — sent to tutor |
| 3 | Is the risk assessment the right depth (9 assets, 10 risks, 8 controls, against a minimum of 6 assets and 6 controls)? | Keep the current scope, since it exceeds the minimum. Focus on quality and consistency rather than adding more items. | Open — sent to tutor |
| 4 | Is mapping each control to NIST CSF 2.0 and CIS Controls v8 enough, or should we also reference SP 800-53? | Add an SP 800-53 reference column to the controls table, reusing the controls I selected in Tutorial 10. It is a small change that strengthens the justification. | Open — sent to tutor |
| 5 | How should we present individual contributions when some members have fewer commits than others? | Base the contribution statement on the tasks each member completed (sections written, research, diagrams, reviews), not commit counts, since some work was done outside GitHub. Each member will confirm their own entry. | Open — sent to tutor |

### Action Plan

| Priority | Action | Section | Target |
|---|---|---|---|
| 1 | Send my five questions to the tutor | All | Week 12 (start) |
| 2 | Make the IP addressing and gateway location consistent across all sections (issues 1 and 2) | Network Design, Risk Assessment | Week 12 |
| 3 | Fix broken diagram links and add the missing Azure screenshot (issues 3 and 4) | Network Design, Cloud Services | Week 12 |
| 4 | Add the SIEM, jump host, and WAF to the network diagram (issue 7) | Diagrams | Week 12 |
| 5 | Enter the risk register into the course spreadsheet template and link it (issue 6) | Risk Assessment | Week 12 |
| 6 | Collect each member's backup approach (issue 5) | Cloud Services | Week 12 |
| 7 | Apply the tutor's answers, or use the default decisions above if there is no reply | All | Week 13 |
| 8 | Write the project reflection and contribution statement (issue 8) | Reflection | Week 13 |

---

## Tutorial 11 Reflection

This week's topic was managing IT services, and preparing for feedback felt like a small version of that: reviewing what has been delivered, checking it against the requirements, and planning improvements.

The most useful part was reading the whole report in one sitting. Each section looked fine on its own, but together they had contradictions, mainly the IP addressing and where the default gateway sits. These happened because the sections were written weeks apart and the design changed in between. In a real company, this is the reason for change management (procedure PRC-05 in my asset inventory): when the design changes, every document that depends on it has to be updated too.

Missing the feedback session also taught me something. The tutor's comment that the project looked great was encouraging, but a general review of a repository will not catch every detail, such as two sections using different IP ranges. Positive feedback tells us the overall direction is right, but it is not the same as checking that everything is consistent. This is similar to IT service management, where a satisfied customer does not mean there are no underlying problems. Regular internal reviews are still needed to find issues before they cause failures.

Because I could not ask my questions in person, I recorded a default decision for each one. This means the group can keep working if the tutor does not reply in time, instead of waiting on an answer. It is a simple way of managing a dependency so it does not block delivery.
