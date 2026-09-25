# 06. Sound the Alarm: Detection and Response
> Course focus: Understand the incident response lifecycle and practice using tools to detect and respond to cybersecurity incidents.
Preparation: the planning and training process — the closest thing to hands-on SOC work in the Google Cybersecurity Certificate.

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

## Key Takeaway

This course is where the program gets closest to real SOC work that shows up in almost every real-world security job description. Worth the most detailed notes of the whole certificate.
