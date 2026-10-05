# 06. Sound the Alarm: Detection and Response
> Course focus: Understand the incident response lifecycle and practice using tools to detect and respond to cybersecurity incidents.
> Study notes covering alert analysis, documentation, the phishing playbook, incident triage, business continuity, and SIEM tools (Splunk and Google SecOps).


---

## 1. The 5 W's of Incident Investigation

Capture the 5 W's of an incident:

- **Who** caused the incident?
- **What** happened?
- **When** did the incident occur?
- **Where** did the incident happen?
- **Why** did the incident happen?

---

## 4. CSIRT & SOC

Incident response is a team effort. Two structures organize that work:

- **CSIRT** (Computer Security Incident Response Team) — core technical goals: manage incidents, prevent future incidents, and provide resources for response/recovery
- **SOC** (Security Operations Center) — organized into tiers

### SOC Tier Structure

| Tier | Role | Responsibilities |
|---|---|---|
| **L1** | Triage / frontline | *(covered in earlier reading)* |
| **L2** | Deeper investigation | • Receives escalated tickets from L1<br>• Conducts deeper investigations<br>• Configures and refines security tools<br>• Reports to the SOC Lead |
| **L3** | SOC Lead | • Manages team operations<br>• Performs advanced detection techniques (malware & forensic analysis)<br>• Reports to the SOC Manager |
| — | **SOC Manager** | • Hires, trains, and evaluates the SOC team<br>• Creates performance metrics and manages team performance<br>• Develops incident, compliance, and audit reports<br>• Communicates findings to stakeholders (e.g. executive management) |

### Other Specialized Roles

- **Forensic investigators** (usually L2/L3): collect, preserve, and analyze digital evidence to determine what happened
- **Threat hunters** (usually L3): detect, analyze, and defend against new/advanced threats using threat intelligence

> Note: SOC structure — like CSIRT structure — varies by organization.

**Key takeaway:** as a security analyst you'll collaborate both within your team and beyond it. Understanding CSIRT/SOC structure clarifies how incidents move through the lifecycle and who owns what — useful for responding creatively under pressure.

---

## 5. Incident Handler's Journal — Template

Reusable template for logging findings during labs/activities:

```
Date:
Entry #:

Description:
  Brief summary of the entry.

Tool(s) used:
  - 

5 W's:
  Who caused the incident?
  What happened?
  When did the incident occur?
  Where did the incident happen?
  Why did the incident happen?
  
```

---

## 6. Knowledge Check — Answers & Rationale

| # | Question Topic | Correct Answer | Rationale |
|---|---|---|---|
| 1 | Phases *after* Preparation | Detection and Analysis / Containment, Eradication, and Recovery / Post-Incident Activity | The 4 NIST phases: Preparation → Detection & Analysis → Containment, Eradication & Recovery → Post-Incident Activity |
| 2 | Nature of the lifecycle | Cyclical | Response feeds lessons back into preparation for the next incident |
| 3 | Occurrence with no policy violation | Event | An incident is an event that crosses into policy violation / threat |
| 4 | Elements of the 5 W's tested | Who / When / Where the incident took place | "Which type" isn't a separate W — it's covered under "What" |

---

## 7. Knowledge Check — CSIRT/SOC Roles

| # | Question Topic | Correct Answer | Rationale |
|---|---|---|---|
| 1 | CSIRT's core goals | Manage incidents / prevent future incidents / provide response-recovery resources | PR is a side function — the technical core is management, recovery, and prevention |
| 2 | What guides detection/response | Incident Response Plan (IRP) | A set of instructions helping IT staff detect, respond to, and recover from network security incidents |
| 3 | Common role filling front-line duties | Security analysts | — |
| 4 | Project-manager-style role in an incident | Incident coordinator | Keeps communication flowing across technical, management, and legal teams — distinct from the technical lead, who owns remediation |

---

## 8. Detection Tools — IDS vs. IPS vs. EDR

Detection tools work like a home security system for a network: continuous monitoring, with an alert the moment something looks off.

| Capability | IDS | IPS | EDR |
|---|---|---|---|
| Detects malicious activity | ✓ | ✓ | ✓ |
| Prevents intrusions | — | ✓ | ✓ |
| Logs activity | ✓ | ✓ | ✓ |
| Generates alerts | ✓ | ✓ | ✓ |
| Performs behavioral analysis | — | — | ✓ |

**IDS (Intrusion Detection System)**
Monitors and alerts only — does not block anything. A security professional has to act on the alert manually.

**Detection categories (for IDS alerts):**
- **True positive** — real attack, correctly flagged
- **True negative** — no attack, no alert (correct)
- **False positive** — flagged, but not actually malicious (wastes analyst time)
- **False negative** — real attack, missed entirely (the dangerous one)

**IPS (Intrusion Prevention System)**
Does everything an IDS does, plus takes action to stop the activity — e.g. modifying a router's access control list to block traffic. Many tools (Suricata, Snort, Sagan) can run as either IDS or IPS.

**EDR (Endpoint Detection and Response)**
Installed on endpoints (laptops, phones, tablets — any networked device). Uses **behavioral analysis** (ML/AI) to spot unusual activity and can **auto-respond** without a human in the loop — e.g. killing a process that shouldn't be running.
*Examples: Open EDR, Bitdefender EDR, FortiEDR.*

> Note: SIEM tools also have detection capabilities — covered next.

**Key takeaway:** IDS, IPS, and EDR each play a different role — detect, log, alert, and (for IPS/EDR) stop malicious activity.

---

## 9. Knowledge Check — Detection Tools

| # | Question Topic | Correct Answer |
|---|---|---|
| 1 | Types of documentation | Playbooks / Final reports / Policies |
| 2 | Ticketing system example | Jira |
| 3 | Monitors + alerts on possible intrusions | Intrusion Detection System (IDS) |
| 4 | Actions an IPS performs | Detect abnormal activity / Monitor activity / Stop intrusive activity |

---

## 10. SIEM (Security Information and Event Management)

A SIEM collects and analyzes log data across an organization so security teams aren't manually checking every device.

**Advantages**
- **Access to event data** — ingests activity from potentially hundreds of connected systems/devices, including real-time data
- **Monitoring, detecting, alerting** — continuously analyzes data against detection rules; a match triggers an alert
- **Log storage** — retains historical data for later investigation (retention period depends on org requirements)

**The SIEM process — 3 steps**

1. **Collect & aggregate data** — pulls logs (event records with timestamps, IPs, etc.) from firewalls, servers, routers, and more into one centralized place, removing the need to check each source individually.
   - Includes **parsing**: mapping a raw log line into labeled fields. E.g.:
     ```
     April 3 11:01:21 server sshd[1088]: Failed password for user nuhara from 218.124.14.105 port 5023
     ```
     becomes:
     ```
     host = server
     process = sshd
     source_user = nuhara
     source ip = 218.124.14.105
     source port = 5023
     ```
2. **Normalize data** — converts logs from many different source formats (a firewall log looks nothing like a server log) into one standard, structured, searchable format.
3. **Analyze data** — applies detection logic (rules/conditions) to the normalized data; a match fires an alert to the security team.
   - Includes **correlation**: comparing multiple log events together to spot patterns that a single log wouldn't reveal on its own.

---

## 11. Detection and alerts (quiz recap)

- Detection tools have limitations. Attackers keep developing evasion techniques, and a tool only detects what it is configured or programmed to recognize.
- Analysts refine alert rules to improve detection accuracy and to reduce false positives.
- **Analysis** is the investigation and validation of alerts.
- High alert volumes are caused by broad detection rules and misconfigured alert settings.

## 12. Security documentation

Documentation is recorded content used to support investigations, complete tasks, and communicate findings.

**Benefits**

| Benefit | What it gives you |
|---|---|
| Transparency | Compliance, insurance, and legal evidence. Chain of custody is an example of an audit trail. |
| Standardization | Repeatable processes, knowledge transfer, and onboarding. An incident response plan is an example. |
| Clarity | Quick access to information. Analysts document why an alert was escalated or closed. |

**Best practices**

- **Know your audience.** An incident summary for a SOC manager differs from one for a CEO.
- **Be concise.** State the purpose immediately. Executive summaries should be short enough to skim.
- **Update regularly.** Review documentation after incidents and as new threats emerge.

## 13. Phishing playbook (v1.0, for level-one SOC analysts)

**Example ticket:** A-2703, "SERVER-MAIL Phishing attempt possible download of malware", severity Medium, status Open. The email posed as a job applicant and carried a password-protected attachment, `bfsvc.exe`. A known malicious file hash was provided.

**Steps**

1. **Receive the phishing alert.**
2. **Evaluate the alert.** Check severity, receiver and sender details (email and IP), subject line, message body, and attachments or links.
   - Low: no escalation.
   - Medium: may need escalation.
   - High: escalate immediately.
3. **Check for links or attachments.**
   - If there are none, go to Step 4.
   - If there are, do not open them outside an authorized, isolated environment.
   - Check their reputation by hash (for example, VirusTotal).
   - If they are not malicious, go to Step 4.
   - If they are malicious, summarize your findings, set the ticket to **Escalated**, and notify a level-two analyst.
4. **Close the ticket** when there are no links or attachments, or when they are confirmed non-malicious. Include a brief summary of your findings and the reason for closing.

## 14. Incident triage

Triage is the prioritizing of incidents by importance or urgency. It has three steps:

1. **Receive and assess.** Verify the alert is valid. Ask whether it is a false positive, whether it has happened before, whether it is tied to a known vulnerability, and how severe it is.
2. **Assign priority.** Weigh these three factors:
   - Functional impact on systems and services.
   - Information impact on confidentiality, integrity, and availability.
   - Recoverability, meaning whether recovery is possible and worth the cost.
3. **Collect and analyze.** Gather evidence, research externally, and document the investigation. Escalate to a level-two analyst or manager if needed.

**Benefits:** resource management (focus on urgent threats) and a standardized approach through playbooks.

## 15. Business continuity planning

- A **business continuity plan (BCP)** outlines how to sustain operations during and after a major disruption. Entry-level analysts usually don't write BCPs, but they should understand them.
- A BCP is not the same as a **disaster recovery plan**, which covers restoring information systems after a major disaster.
- Ransomware can cripple critical infrastructure such as healthcare by encrypting records.
- **Resilience** is the ability to prepare for, respond to, and recover from disruptions.

**Recovery sites (site resilience)**

| Site | Description |
|---|---|
| Hot | Fully operational duplicate of the primary environment, available immediately. |
| Warm | Fully updated and configured, but not live. Can be made operational quickly. |
| Cold | Some of the required infrastructure. Needs additional work before use. |

## 16. SIEM and search tools

- **SIEM process steps:** collect and process data, then normalize it (for example, to UDM) so it can be read and analyzed.
- **Splunk** uses **SPL** (Search Processing Language). `*` is the wildcard character.
- **Google SecOps (Chronicle)** uses **raw log search** for unstructured, unparsed logs. UDM is used for structured data.
- Useful search skills include contextual awareness (for example, telling web traffic from sensitive `vendor_sales` logs) and granular filtering to find specific events, such as a single unauthorized login attempt.

## Key takeaways

- Document your reasoning for every alert you escalate or close.
- Follow the playbook in order, and never open suspicious attachments outside an isolated environment.
- Triage so the most critical incidents get attention first.
- Plan for continuity before an incident, not after.

This course is where the program gets closest to real SOC work that shows up in almost every real-world security job description. Worth the most detailed notes of the whole certificate.
