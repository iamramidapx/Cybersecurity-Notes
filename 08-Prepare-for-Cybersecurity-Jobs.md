# 08: Put It to Work: Prepare for Cybersecurity Jobs
Course 8 of the Google Cybersecurity Certificate (Coursera). Covers incident classification, escalation, stakeholder communication, community engagement, AI in cybersecurity, and interview prep.


## Log Analysis Exercise: `system_activity_log_2025-07-30.txt`

**Most suspicious entry (requires immediate escalation):**
John.Doe copied `/finance/budget_2026_final.xlsx` to `/public_share` at 08:20:20. This is a classic data-exfiltration red flag — a sensitive financial file was moved somewhere network-wide accessible. Unlike the failed login attempts, this is a *completed* action that compromises confidentiality.

**Potential account compromise / insider threat (combined entries):**
1. Admin login from external IP `203.0.113.25` at an unusual location (08:30:40)
2. John.Doe's file copy to a public folder (08:20:20)

Together these suggest either a compromised admin account or a malicious insider moving data while access controls are weakened.

**Brute-force attempt:**
User `guest` from IP `10.0.0.100` had 3 consecutive failed logins between 08:15:00–08:15:10 — a signature of brute-force/credential-stuffing activity.

## Data Classification

| Type | Description | Examples |
|---|---|---|
| **Public** | Open to the public; minimal risk if exposed, but still needs basic protection | Press releases, job descriptions, marketing materials |
| **Private** | Should stay internal; unauthorized access = serious risk | Company emails, employee IDs, research data |
| **Sensitive** | Must be restricted to authorized users only; includes PII, SPII, PHI | Bank account numbers, SSNs, passwords, passport numbers, medical records |
| **Confidential** | Critical to ongoing operations; often protected by NDAs | Trade secrets, financial records, sensitive government data |

## Asset Classification
Assets are labeled low- to high-level based on sensitivity and business impact:
- **Low-level**: e.g., a company's public website address — minimal harm if compromised
- **High-level**: e.g., internal emails discussing trade secrets — major harm if leaked (loss of competitive edge, reputation, customer trust)

Every organization has its own data classification policy; understanding it helps prioritize what needs the strongest protection.

## Key Quiz Takeaways

- **Security mindset** enables analysts to: evaluate risks/identify breaches, and recognize what they're defending.
- **Least impactful asset if compromised**: Guest Wi-Fi network (isolated from core systems, no sensitive data).
- **Cultivating a security mindset**: staying current on the latest vulnerabilities and threat trends (NDAs are legal tools, not mindset-builders).
- **Examples of a security mindset in action**: exercising suspicion before opening email attachments, and reporting suspicious emails (Zero Trust, proactive defense).


## 1. Log Analysis Activity (system_activity_log_2025-07-30)

| Question | Answer | Why |
|---|---|---|
| Most suspicious entry needing immediate escalation | `08:20:20` John.Doe copied `/finance/budget_2026_final.xlsx` to `/public_share` | Completed action exposing sensitive financial data (possible exfiltration); failed logins were unsuccessful |
| Two entries that together suggest compromise or insider threat | Admin login from external IP `203.0.113.25` + John.Doe's file copy | Unusual-location admin access plus data movement to a public folder |
| User and IP with repeated failed logins | `guest` from `10.0.0.100` (3 attempts in 10 seconds) | Pattern typical of brute force or credential stuffing |

---

## 2. Data and Asset Classification

- **Public:** minimal risk if shared (press releases, job descriptions, product announcements).
- **Private:** should be kept from the public (internal emails, employee IDs, research data).
- **Sensitive:** PII, SPII, PHI (bank accounts, passwords, passport numbers, medical info).
- **Confidential:** limited access, often under NDA (trade secrets, financial records).
- **Asset classification:** public data is a low-level asset; sensitive and confidential data are high-level assets. Examples: guest Wi-Fi is lowest impact; intellectual property and customer data are highest.
- **Security mindset:** proactively evaluate risk, recognize what you are defending, stay current on vulnerabilities, be suspicious of attachments, and report suspicious emails.

## 3. Business Continuity and Disaster Recovery

- Four-step process: identify assets, identify threats, implement detection tools and processes, then create BC/DR plans.
- **Business continuity plan:** sustain operations during and after disruption. Steps: business impact analysis, recovery steps for critical functions, form a BC team, train the team.
- **Disaster recovery plan:** minimize impact and restore software and hardware, and identify affected applications and data.

## 4. Incident Types, Terminology and Escalation

- **Incident types:** malware infection, unauthorized access (digital or physical, without permission), improper usage (acceptable use policy violation).
- **Key terms:** a *vulnerability* is a weakness, a *threat* is a potential danger, an *asset* is what you protect, and a *security incident* is an event that actually harms confidentiality, integrity or availability.
- **Escalation:** identify a potential incident, triage it, and hand it to a more experienced person.
- **Prioritization:**
  - Incidents affecting business-critical assets come first.
  - Incidents involving customer PII are escalated with higher urgency.
  - Malicious code execution is escalated, while unauthorized software alone is mainly a policy issue.
  - One failed login is low-level; 15 in 30 minutes needs escalation.
- **Tips:** learn the company's escalation policy, follow it, and ask questions.
- **Breach notification laws** apply to PII exposure, so know the local rules.

### Roles in escalation

| Role | Responsibility |
|---|---|
| Data owner | Decides access, use and destruction; accountable for classification |
| Data controller | Decides how and why personal data is processed; ensures regulatory compliance |
| Data processor | Processes data on the controller's behalf (often a vendor) |
| Data custodian | Grants and revokes access; implements security controls |
| Data protection officer (DPO) | Monitors internal compliance with data protection rules |

Entry-level analysts typically escalate to their direct supervisor.

---

## 5. Stakeholder Communication

| Stakeholder | Focus |
|---|---|
| Risk manager | Identify and manage risks; enforce IT policies |
| Operations manager | Day-to-day security operations; first line of response (analysts work with them most) |
| Legal counsel | Regulations, litigation, penalties and fines |
| CFO | Financial cost of incidents and of security tools |
| CEO | Financial and operational impact |
| CISO | Security architecture, risk analysis, audits, continuity plans |

**Communication best practices:**
- Be clear and concise, and avoid unnecessary jargon.
- Before communicating, know what the person needs to know, why it matters, when to act, and how to explain it non-technically.
- Follow company protocols and handle sensitive data with care.
- Match the channel to the message: chat or phone for simple items, email or meeting for complex ones, charts for data.
- If a stakeholder doesn't respond, follow up by email and then by phone or another channel.

**Visual dashboards:** use charts and graphs (e.g., Google Sheets, OpenOffice) for numeric data such as phishing clicks by department. Use a detailed report with a timeline for in-depth incident analysis (Juliana's case study: dashboard for lockout data, detailed document for the CISO).

---

## 6. Engaging with the Cybersecurity Community

- Join organizations and conferences that match your interests, found via web search, LinkedIn and mailing lists (e.g., CISA's threat and vulnerability lists).
- Beware of social engineering on social media.
- **Worksheet:** interests were cloud security, incident response, sensitive data protection and security awareness. The Cloud Security Alliance (CSA) was the best fit; the CISO Executive Network is executive-level and NCSA is general awareness.
- **Resources quizzed:** OWASP Top 10 for web application risks, Krebs on Security, and follow CISOs and peers on social media.

---

## 7. AI in Cybersecurity

- **Generative AI uses:** create content (e.g., test data), summarize reports, answer threat questions, and do first-pass phishing checks.
- **TCREI prompting framework:** Task, Context, References, Evaluate, Iterate.
- **Prompt exercise:** a Senior Security Analyst persona writing a 500-word phishing and malware guide for non-technical staff, with context, references (OWASP principles), and bulleted format at an 8th-grade reading level. Evaluating and iterating made the draft simpler, less alarming and more visual.
- **Phishing red flags:** urgency, mismatched URLs, suspicious attachments, and lookalike sender addresses.
- **Responsible use:** review outputs for accuracy, disclose AI use, avoid entering sensitive data, keep a human in the loop, and follow company policy.
- **Double-edged sword:** attackers use AI too, and AI systems are themselves targets.

---

## 8. Technical Interview Prep

- Expect questions on Python basics (and possibly whiteboarding pseudocode), NIST frameworks (CSF), and network security.
- **Common questions:**
  - TCP/IP model: how data is organized and transmitted across a network.
  - OSI model: seven layers of network communication.
  - SIEM tools: identify and analyze threats, risks and vulnerabilities.
- Tip: write the full question down first to cover every part.

## 9. Career Roadmap: Transitioning to Security Analyst

1. **Job search:** use niche boards (NinjaJobs, CyberSecJobs), target healthcare/biotech/pharma where a microbiology background is an asset, and network via OWASP, ISACA and BSides.
2. **Applications:** hybrid resume with a skills matrix (SIEM, Python, NIST) plus hands-on projects (home labs, TryHackMe); frame science experience as analytical discipline, data integrity and SOP adherence.
3. **Interviews:** master the CIA triad, OSI model and NIST incident response lifecycle; think aloud; explain technical issues to non-technical audiences.
4. **Practice:** mock interviews, plus a GitHub or blog with technical write-ups as proof of work.

