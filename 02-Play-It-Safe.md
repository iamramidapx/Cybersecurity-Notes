Play It Safe: Manage Security Risks — Study Summary
Personal study notes for **Course 2** of the Google Cybersecurity Certificate (Coursera), covering Modules 1–4. This is my own condensed summary, not the official course material.

## Module 1 — Security domains

The CISSP framework organizes security work into **eight domains**:

| # | Domain | Focus |
|---|--------|-------|
| 1 | Security & risk management | Security posture, compliance, business continuity, ethics, regulations (e.g. GDPR) |
| 2 | Asset security | Storing, retaining and destroying data; backups and recovery plans |
| 3 | Security architecture & engineering | Shared responsibility; design principles such as least privilege, defense in depth, zero trust |
| 4 | Communication & network security | Securing on-site, remote and cloud networks |
| 5 | Identity & access management (IAM) | Authentication, authorization, least privilege |
| 6 | Security assessment & testing | Audits, pen testing, control testing |
| 7 | Security operations | Investigations, SIEM, log management, incident management, playbooks, forensics |
| 8 | Software development security | Building security into every phase of the software development life cycle |

### Threats, risks, vulnerabilities
- **Asset**: anything of value (digital data such as SSNs, or physical items such as servers).
- **Threat**: an event or circumstance that can harm assets (e.g. insider threats, advanced persistent threats).
- **Risk**: anything that can affect the confidentiality, integrity or availability of an asset (external, internal, legacy systems, multiparty, licensing/compliance).
- **Vulnerability**: a weakness a threat can exploit (e.g. Log4Shell, ProxyLogon, ZeroLogon, PetitPotam, SSRF, insufficient logging).
- **Risk strategies**: acceptance, avoidance, transference, mitigation.
- The **NIST Risk Management Framework (RMF)** gives a step-by-step process for managing risk, ending in formal system authorization and ongoing monitoring.

## Module 2 — Frameworks and controls

- **Frameworks** are guidelines for building plans that reduce risk and support compliance (e.g. NIST CSF, ISO/IEC 27001, the Cyber Threat Framework).
- **Controls** are safeguards that reduce specific risks. Categories: **administrative**, **technical**, **physical**. Types: **preventative**, **detective**, **corrective**, **deterrent**.

| Category | Examples |
|----------|----------|
| Administrative | Least privilege, separation of duties, password and access policies, disaster recovery plans |
| Technical | Firewalls, IDS/IPS, MFA, encryption, backups, antivirus |
| Physical | Locks, CCTV, lighting, fire detection, locking network cabinets |

### CIA triad
- **Confidentiality** — only authorized users can access data.
- **Integrity** — data is accurate, authentic and unaltered (e.g. via cryptography).
- **Availability** — authorized users can reach data and systems when needed.

### OWASP security principles
Minimize attack surface · least privilege · defense in depth · separation of duties · keep security simple · fix issues correctly (root cause) · establish secure defaults · fail securely · don't trust third-party services · avoid security by obscurity.

### Security audits
An audit reviews controls, policies and procedures against internal and external standards. Typical checklist: define **scope and goals → complete a risk assessment → conduct the audit (controls and compliance assessment) → create a mitigation plan → communicate results to stakeholders**.

## Module 3 — Introduction to cybersecurity tools

- **SIEM** (Security Information and Event Management) tools collect and analyze log data for real-time monitoring and alerting. Deployment models: **self-hosted, cloud-hosted, cloud-native, hybrid**. **SOAR** adds automated response.
- **Logs → normalization → correlation → alerts** is the basic SIEM pipeline; risk scoring and correlation rules help analysts prioritize.
- **Open-source vs proprietary**: open-source tools (Linux, Suricata) are free and community-driven, and are not inherently less secure; proprietary tools are paid and vendor-controlled.
- **Splunk** dashboards: security posture, executive summary, incident review, risk analysis.
- **Google Chronicle** dashboards: enterprise insights, data ingestion and health, IOC matches, main, rule detections, user sign-in overview.

## Module 4 — Using playbooks to respond to incidents

- A **playbook** is a living document of predefined steps and responsibilities for responding to incidents or vulnerabilities (e.g. ransomware, BEC, vishing). It is updated after incidents and as threats change.
- SIEM and playbooks work together: the SIEM detects, the playbook says how to respond.
- **NIST incident response lifecycle**:
  1. Preparation
  2. Detection & analysis
  3. Containment (always before eradication)
  4. Eradication & recovery
  5. Post-incident activity (lessons learned)

## Course logistics (brief)

Passing each graded quiz needs 80%+. The full certificate spans 9 courses and is self-paced.

---
