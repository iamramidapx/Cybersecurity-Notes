# MITRE Frameworks

A summary of MITRE's cyber security resources: ATT&CK, CAR, D3FEND, and newer projects (Emulation Plans, Caldera, AADAPT, ATLAS).

**MITRE** is a not-for-profit R&D organization (cyber security, AI, healthcare, space systems) whose mission is "to solve problems for a safer world." Its cyber frameworks give red and blue teams a shared way to understand adversary behavior, build detections, and strengthen defenses.

**Learning objectives:** understand ATT&CK's purpose and structure; see how professionals use it; profile threats with CTI and the ATT&CK Matrix; learn about CAR and D3FEND.

---

## 1. MITRE ATT&CK

A globally accessible knowledge base of adversary **tactics and techniques** based on real-world observations. Started in 2013 to document the TTPs of APT groups.

| Term | Meaning |
|---|---|
| **Tactic** | The adversary's goal — the "why" |
| **Technique** | How the goal is achieved |
| **Procedure** | The specific implementation of a technique |

- **Matrices:** Enterprise (Windows, macOS, Linux, cloud, …), Mobile, and ICS.
- **Matrix structure:** tactics across the top → techniques beneath → expandable sub-techniques.
  *Example:* Reconnaissance (tactic) → Active Scanning (technique) → Scanning IP Blocks / Vulnerability Scanning / Wordlist Scanning (sub-techniques).
- Each technique page includes an ID, description, procedure examples (groups, software, campaigns), mitigations, detections, and references.
- **ATT&CK Navigator** is a tool for annotating and exploring matrices.

**Quick answers:** Phishing → *Initial Access*. Create Account → *T1136* (Persistence).

### Why it matters
- Provides a **standard language and unique IDs**, so incidents and data can be compared across the community.
- Bridges **threat intelligence and defense**: mapping activity to TTPs turns reports into detection logic, queries, and playbooks.

### Who uses it

| Role | Goal | How they use ATT&CK |
|---|---|---|
| CTI teams | Improve security posture through threat info | Map observed behavior to TTPs to build actionable profiles |
| SOC analysts | Triage alerts | Link activity to techniques for context and prioritization |
| Detection engineers | Build/improve detections | Map SIEM/EDR rules to ATT&CK to find coverage gaps |
| Incident responders | Investigate incidents | Map timelines to tactics/techniques to visualize the attack |
| Red & purple teams | Emulate adversaries | Build emulation plans aligned to known group techniques |

---

## 2. Mapping in Action — Mustang Panda (G0129)

A China-based, state-sponsored espionage group (active since at least 2012) targeting governments, military, and NGOs, especially in Southeast Asia, Europe, and the US. Attribution rests on government statements, tracking by security firms (aliases: Twill Typhoon, Bronze President, TEMP.Hex), and shared tooling such as custom **PlugX** RAT variants.

**Typical behavior:** phishing for initial access, persistence via scheduled tasks, file obfuscation for defense evasion, ingress tool transfer for C2.

| Question | Answer |
|---|---|
| Reconnaissance technique | **T1598** — Phishing for Information (sub-technique T1598.003, Spearphishing Link; web bugs profile targets and fingerprint user-agents before payload delivery) |
| Software used for Access Token Manipulation (T1134) | **Cobalt Strike** — `steal_token` (T1134.001), `make_token` (T1134.003), and `ppid` parent-PID spoofing (T1134.004) |

---

## 3. Applying ATT&CK — Aviation / Cloud Scenario

Scenario: an aviation-sector analyst migrating to the cloud uses ATT&CK Groups, Navigator layers, and technique pages to find relevant threats and coverage gaps.

| Question | Answer |
|---|---|
| APT group targeting aviation, active since ≥2013 | **APT33** (Iran-linked; seeks aerospace IP, military/geopolitical intelligence, and supply-chain footholds) |
| Sub-technique of concern for Office 365 | **T1078.004 — Valid Accounts: Cloud Accounts** (often obtained via password spraying, T1110.003) |
| Linked tool | **Ruler** — abuses Outlook mail rules to execute payloads on endpoints |
| Mitigation for inactive/unused accounts | **User Account Management** (M1018) |
| Detection Strategy ID for abused cloud accounts | **DET0546** |

**How to find this:** use the group's profile page ("Techniques Used" table; sub-techniques have a decimal ID), a `site:attack.mitre.org` search, or select the group in the Navigator to highlight all its techniques.

---

## 4. CAR — Cyber Analytics Repository

A knowledge base of **ready-made detection analytics** built on ATT&CK. Each analytic describes how to detect a behavior, with a data model, pseudocode, and tool-specific implementations (e.g., Splunk, LogPoint, EQL), and sometimes unit tests. It has its own Navigator layer.

| Question | Answer |
|---|---|
| Tactic for CAR-2019-07-001 | **Defense Evasion** |
| Analytic type for Access Permission Modification | **Situational Awareness** |

---

## 5. D3FEND

*Detection, Denial, and Disruption Framework Empowering Network Defense* — the defensive counterpart to ATT&CK: a common language for how security controls work, organized into **seven tactics** with techniques and IDs. Pages explain how a defense works, implementation considerations, and related digital artifacts and ATT&CK techniques. Example: **Credential Rotation (D3-CRO)**.

| Question | Answer |
|---|---|
| Sub-technique of User Behavior Analysis (D3-UBA) for logon geolocation | **User Geolocation Logon Pattern Analysis** (D3-UGLPA) |
| Digital artifact it analyzes | **Network Traffic** |

---

## 6. Other MITRE Projects

- **Adversary Emulation Library** (maintained mainly by the Center for Threat Informed Defense): free, step-by-step plans that mimic real threat groups.
- **Caldera:** automated adversary emulation tool for red/blue exercises, testing detections and incident response using ATT&CK.
- **AADAPT:** matrix of adversary tactics/techniques against digital asset payment technologies (blockchains, smart contracts, wallets). Scrape Blockchain Data → **ADT3025**.
- **ATLAS:** matrix of threats, real-world attacks, and mitigations for AI/ML systems.

---

## Key Takeaways

1. ATT&CK = attacker behavior (tactics → techniques → sub-techniques → procedures).
2. CAR = detection analytics; D3FEND = defensive countermeasures; together with ATT&CK they cover the attack–detect–defend loop.
3. Mapping threat-group activity and your own detections to ATT&CK exposes coverage gaps and gives teams a shared vocabulary.
4. Emulation Plans and Caldera let you test those defenses; AADAPT and ATLAS extend the approach to blockchain and AI.
