# SIEM, Firewalls, IDS/Snort, Vulnerability Scanning

## 1. SIEM Basics

**SIEM** = Security Information and Event Management. It is the core tool of a SOC analyst. It collects, normalizes, and correlates logs from many sources.

### Log source types
| Type | What it records | Sources | Examples |
|---|---|---|---|
| **Host-centric** | Events on or about a host | Windows, Linux, servers | File access, auth attempts, process execution, registry changes, PowerShell execution |
| **Network-centric** | Host-to-host and internet traffic | Firewalls, IDS/IPS, routers | SSH, FTP, web traffic, VPN access, file sharing |

### Problems without a SIEM
- **Too many log sources**: hundreds of events per second, scattered across devices.
- **No centralization**: you have to SSH or RDP into each machine.
- **Limited context**: single logs look harmless, and the story only appears when they are correlated.
- **Limited analysis**: humans can't review everything manually.
- **Format issues**: every source uses a different format.

### Core SIEM features
| Feature | Meaning |
|---|---|
| **Centralized collection** | Agents or APIs pull logs from all sources into one place |
| **Normalization** | **Parsing** breaks a log into fields. **Normalization** puts all logs in one consistent format |
| **Correlation** | Links events across sources to find patterns (see example below) |
| **Real-time alerting** | Rules fire alerts when conditions are met. Analysts can write custom rules |
| **Dashboards / reporting** | Alert highlights, failed logins, events ingested, rules triggered, top domains visited, health alerts |

**Correlation example (5-minute window):** VPN login from a new IP → shared-drive document access → PowerShell script run → outbound connection. Each event looks fine alone, but together they suggest **data exfiltration with compromised VPN credentials**.

### Where logs live
**Windows**
- Event Viewer. Each event type has a unique **Event ID**.

**Linux**
| Path | Contents |
|---|---|
| `/var/log/httpd` | HTTP requests, responses, and errors |
| `/var/log/cron` | Cron job events |
| `/var/log/auth.log`, `/var/log/secure` | Authentication logs |
| `/var/log/kern` | Kernel events |

**Web server (Apache)**
- `/var/log/apache` or `/var/log/httpd`
- Sample line: `192.168.21.200 - - [21/March/2022:10:17:10 -0300] "GET /cgi-bin/try/ HTTP/1.0" 200 3395 "-" "Mozilla/5.0 ..."`

### Log ingestion methods
1. **Agent / Forwarder**: lightweight tool on the endpoint (Splunk calls it a "forwarder").
2. **Syslog**: widely used protocol that sends real-time data to a central destination.
3. **Manual upload**: ingest offline data (Splunk, ELK).
4. **Port forwarding**: the SIEM listens on a port and endpoints send data to it.

### Detection rules
Rules are logical expressions over **field-value pairs**, which is why normalized logs matter.

Example rules:
- 5 failed logins in 10 seconds → "Multiple Failed Login Attempts"
- Successful login after multiple failures → "Successful Login After Multiple Failures"
- USB plugged in (if restricted by policy) → alert
- Outbound traffic > 25 MB → possible exfiltration (threshold depends on policy)

**Use case 1: Event log cleared**
```
IF LogSource = WinEventLog AND EventID = 104
THEN alert "Event Log Cleared"
```
Adversaries clear logs after exploitation to cover their tracks.

**Use case 2: `whoami` execution**
```
IF LogSource = WinEventLog AND EventCode = 4688 AND NewProcessName contains "whoami"
THEN alert "WHOAMI command execution detected"
```
4688 is process creation.

**Key Event IDs**
| ID | Meaning |
|---|---|
| 104 | Event log cleared |
| 4688 | Process creation (use with `NewProcessName`) |

### Alert investigation workflow
1. Alert triggers → check the associated events and which rule conditions matched.
2. Decide **True Positive** or **False Positive**.
3. **False Positive** → tune the rule so it doesn't recur.
4. **True Positive** → investigate further:
   - Contact the asset owner.
   - Isolate the infected host if the activity is confirmed.
   - Block the suspicious IP.

---

## 2. Firewalls

A firewall controls network traffic based on rules, usually at the network boundary.

### Types
| Type | OSI layer | Key points |
|---|---|---|
| **Stateless** | 3–4 | Matches each packet against rules, with no memory of connections. Basic filtering, fast, efficient on high-speed networks |
| **Stateful** | 3–4 | Tracks connections in a **state table** and can apply complex rules |
| **Proxy (application-level gateway)** | 7 | Sits between the private network and the internet. Inspects packet contents, filters content, controls applications, can decrypt and inspect SSL/TLS, and masks internal clients |
| **NGFW (next-gen)** | 3–7 | Deep packet inspection, built-in IPS, heuristic analysis, SSL/TLS inspection, advanced threat protection |

### Rule components
**Source address · Destination address · Port · Protocol · Action · Direction**

### Actions
| Action | Effect | Example |
|---|---|---|
| **Allow** | Permit the traffic | Allow `192.168.1.0/24` → Any, TCP 80, outbound |
| **Deny** | Block the traffic | Deny Any → `192.168.1.0/24`, TCP 22, inbound |
| **Forward** | Redirect to another segment (gateway and routing firewalls) | Forward Any → `192.168.1.8`, TCP 80, inbound |

### Direction
- **Inbound**: incoming traffic only. Example: allow port 80 to the web server.
- **Outbound**: outgoing traffic only. Example: block outgoing SMTP (port 25) from everything except the mail server.
- **Forward**: moves traffic within the network. Example: send incoming HTTP to the web server.

### Windows Defender Firewall
- On by default. It picks a profile using **Network Location Awareness (NLA)**.
- **Profiles**: *Private* (home) and *Guest/Public* (untrusted networks such as cafés). Settings can differ per profile.
- Allow or block apps per profile from the main page.
- **Custom rule** (example: block outbound HTTP/HTTPS):
  1. Advanced Settings → **Outbound Rules** → **New Rule**
  2. **Custom** → **All programs**
  3. Protocol **TCP**, Remote port = **Specific ports** → `80,443` (comma-separated, no spaces)
  4. Scope: leave as is
  5. Action: **Block the connection**
  6. Profile: tick all
  7. Name the rule → **Finish**

### Linux firewalls
All are built on **Netfilter**, the kernel framework for packet filtering, NAT, and connection tracking.
- **iptables**: the most widely used.
- **nftables**: successor to iptables, with better filtering and NAT.
- **firewalld**: predefined rule sets and zone-based.
- **ufw**: Uncomplicated Firewall, a beginner-friendly front end to iptables.

**ufw commands**
```bash
sudo ufw status                    # check status
sudo ufw enable                    # enable (also on startup)
sudo ufw disable                   # disable
sudo ufw default allow outgoing    # default policy: allow all outgoing
sudo ufw deny 22/tcp               # block incoming SSH
sudo ufw status numbered           # list rules with numbers
sudo ufw delete 2                  # delete rule #2
```

---

## 3. IDS (Intrusion Detection System) and Snort

**Firewall vs IDS:** a firewall is the gatekeeper at the door. An IDS is the surveillance camera that catches bad actors who already got inside.

### Deployment modes
| | HIDS | NIDS |
|---|---|---|
| Scope | One host | Whole network |
| Pros | Detailed host visibility | Centralized view of all traffic |
| Cons | Resource-intensive, hard to manage at scale | No per-host depth |

### Detection modes
| Mode | How it works | Notes |
|---|---|---|
| **Signature-based** | Matches known attack patterns (signatures) | Fast, but only catches known attacks. Good for small threat surfaces |
| **Anomaly-based** | Learns a baseline, then flags deviations | Can catch **zero-days**, but has higher processing overhead |
| **Hybrid** | Signatures for known threats, anomaly detection for new ones | Combines both strengths |

### Snort
- Open-source IDS, developed in 1998. It uses signature and anomaly detection, defined in rule files.
- Config directory: `/etc/snort/`
  - `snort.conf`: main config (enabled rule files, network ranges)
  - `rules/`: rule files, including `local.rules` for custom rules
  - Others: `classification.config`, `reference.config`, `threshold.conf`, `gen-msg.map`, `community-sid-msg.map`, `unicode.map`

**Snort modes**
| Mode | Function |
|---|---|
| **Packet sniffer** | Reads and displays packets with no analysis (useful for troubleshooting) |
| **Packet logging** | Logs traffic as PCAP for later analysis |
| **NIDS** | Real-time monitoring that matches traffic against rules and alerts. This is the main IDS mode |

**Rule format**
```
alert icmp any any -> $HOME_NET any (msg:"Ping Detected"; sid:10001; rev:1;)
```
| Part | Meaning |
|---|---|
| `alert` | Action |
| `icmp` | Protocol |
| `any any` | Source IP and source port |
| `->` | Direction |
| `$HOME_NET any` | Destination IP and port. `$HOME_NET` is a variable set in `snort.conf` |
| `msg` | Alert message |
| `sid` | Unique signature ID |
| `rev` | Revision number (increment on each change) |

**Create and test a rule**
```bash
sudo nano /etc/snort/rules/local.rules
# add (don't delete existing rules):
alert icmp any any -> 127.0.0.1 any (msg:"Loopback Ping Detected"; sid:10003; rev:1;)

# run Snort live (replace 'lo' with your interface name if different)
sudo snort -q -l /var/log/snort -i lo -A console -c /etc/snort/snort.conf

# trigger it
ping 127.0.0.1
```

**Run Snort on a PCAP file** (forensics on historical traffic)
```bash
sudo snort -q -l /var/log/snort -r Task.pcap -A console -c /etc/snort/snort.conf
```

**Snort flags**
| Flag | Meaning |
|---|---|
| `-q` | Quiet |
| `-l <dir>` | Log directory |
| `-i <iface>` | Live interface |
| `-r <file>` | Read PCAP |
| `-A console` | Print alerts to the console |
| `-c <file>` | Config file |

---

## 4. Vulnerability Scanning

- **Vulnerability**: a weakness in a system. **Patching**: fixing it by changing software or the system.
- Scanning is often required for compliance, typically **quarterly or yearly**.
- Automated scanners beat manual scanning, especially on large networks.

### Scan types
| Authenticated | Unauthenticated |
|---|---|
| Needs host credentials | Needs only an IP address |
| Deeper: config and installed apps | Less resource-intensive, simple setup |
| Finds what an attacker **with host access** could exploit | Finds what an **external attacker** could exploit |
| Example: internal database scan | Example: public website scan |

| Internal | External |
|---|---|
| Run from inside the network | Run from outside the network |
| Finds what is exposed once an attacker is inside | Finds what is exposed to outsiders |

Authenticated scans are typically used for **internal** scanning, and unauthenticated scans for **external** scanning.

### Scanner comparison
| Tool | Vendor / Origin | Model | Notes |
|---|---|---|---|
| **Nessus** | Open source in 1998, bought by Tenable in 2005 | Proprietary, **on-prem**, free (limited) and paid | Widely used by large enterprises |
| **Qualys** | 1999 | Subscription, **cloud-based** | Continuous scanning, compliance checks, asset management, auto alerts. No hardware to manage |
| **Nexpose** | Rapid7, 2005 | Subscription, on-prem or hybrid | Continuous asset discovery, risk scores by asset value and impact, compliance checks |
| **OpenVAS** | Greenbone | **Open source** | Basic features, less extensive. Good for small orgs and individuals |

Choose a scanner based on scope, resources, and depth of analysis. Most produce exportable reports with risk scores and descriptions, and some include remediation advice.

### CVE and CVSS
**CVE** (Common Vulnerabilities and Exposures) is a unique ID for each vulnerability, run by **MITRE**.
- Format: `CVE-YEAR-NNNN+`. The year is when it was discovered, followed by 4 or more digits (e.g., `CVE-2024-9374`).

**CVSS** (Common Vulnerability Scoring System) gives a severity score from 0 to 10.
| Score | Severity |
|---|---|
| 0.0–3.9 | Low |
| 4.0–6.9 | Medium |
| 7.0–8.9 | High |
| 9.0–10 | Critical |

### OpenVAS quick start
```bash
sudo apt install docker.io
sudo docker run -d -p 443:443 --name openvas immauss/openvas
```
Open `https://127.0.0.1` and log in.

**Run a scan**
1. **Scans → Tasks** → star icon → **New Task**
2. Name the task and click the **Scan Targets** option
3. Enter the target name and IP → **Create**
4. Choose the scan option → **Create**
5. Click the **play** button in Actions
6. Wait for status **Done**. Click the task name for details, then the vulnerability count for the full list and each item's severity
7. Export the report from the Tasks dashboard in your preferred format

---

## Quick Reference

| Concept | One-liner |
|---|---|
| Parsing | Splitting a log into fields |
| Normalization | Putting logs from all sources into one format |
| Correlation | Linking events across sources to expose an attack |
| True / False Positive | Real threat vs. false alarm (tune the rule on a false positive) |
| Stateless vs Stateful | No connection memory vs. tracks connections in a state table |
| NGFW | Layers 3–7 with DPI, IPS, and TLS inspection |
| HIDS / NIDS | One host vs. whole network |
| Signature vs Anomaly | Known patterns vs. deviation from baseline (catches zero-days) |
| CVE / CVSS | Vulnerability ID / severity score (0–10) |
