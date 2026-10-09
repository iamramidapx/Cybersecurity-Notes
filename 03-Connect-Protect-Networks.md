# Botium Toys Security Audit & Network Fundamentals — Summary

A summary of a cybersecurity course document (*Connect and Protect: Networks and Network Security*). It has two parts: a security controls and compliance audit for a fictional company, **Botium Toys**, and a primer on network devices, network diagrams, and cloud computing.

---

## Part 1: Botium Toys Audit

### Controls assessment

| Control | In place? | Notes |
|---|---|---|
| Least privilege | ❌ | All employees can access customer data |
| Disaster recovery plans | ❌ | None exist; needed for business continuity |
| Password policies | ❌ | Requirements are minimal |
| Separation of duties | ❌ | The CEO runs daily operations and payroll |
| Firewall | ✅ | Blocks traffic using a well-defined rule set |
| Intrusion detection system (IDS) | ❌ | Needed to identify intrusions |
| Backups | ❌ | No backups of critical data |
| Antivirus software | ✅ | Installed and monitored by IT |
| Legacy system monitoring and maintenance | ❌ | No regular schedule; unclear intervention procedures |
| Encryption | ❌ | Not used |
| Password management system | ❌ | None in place |
| Locks (offices, storefront, warehouse) | ✅ | Sufficient |
| CCTV surveillance | ✅ | Installed and functioning |
| Fire detection/prevention | ✅ | Functioning |

**Result:** 5 of 14 controls are in place. The physical and perimeter controls are fine. The administrative and data-protection controls are largely missing.

### Compliance checklist

| Standard | Met | Not met |
|---|---|---|
| **PCI DSS** | — | Access limited to authorized users; credit card data handled securely; encryption of card data; secure password management (0 of 4 met) |
| **GDPR** | 72-hour breach notification plan; privacy policies enforced | EU customer data kept secure (no encryption); data classification (assets inventoried but not classified) (2 of 4 met) |
| **SOC 1 / SOC 2** | Data integrity | User access policies (no least privilege or separation of duties); PII/SPII confidentiality (no encryption); data availability limited to authorized users (1 of 4 met) |

### Recommendations

- Implement least privilege, separation of duties, and encryption. These close the biggest compliance gaps.
- Create disaster recovery plans and backups.
- Strengthen password policies and add a password management system.
- Deploy an IDS.
- Set a regular schedule and clear procedures for legacy system maintenance.
- Classify assets so any further needed controls can be identified.

---

## Part 2: Network Fundamentals

### Network devices

- **Devices/computers:** Each has a unique MAC address and IP address, plus a network interface that sends and receives data packets.
- **Firewall:** Monitors and filters incoming and outgoing traffic using organization-defined rules. It is a first line of defense, not the only one.
- **Server:** Provides information and services to clients in the client-server model. Examples are DNS, file, and mail servers.
- **Hub:** Repeats all data to every port, which makes it vulnerable to eavesdropping. It is rarely used today.
- **Switch:** Forwards packets only to the intended device, using a MAC address table. It works at the data link layer and improves performance and security.
- **Router:** Connects networks and forwards packets by destination IP address. It works at the network layer and often includes firewall features.
- **Modem:** Connects a home or office to the ISP and converts signals into a format the local network can use.
- **Wireless access point:** Creates a Wi-Fi network over radio waves.

### Network diagrams

Network diagrams are maps of the devices on a network and how they connect. Security analysts use them to understand an organization's architecture and to develop and refine strategies for securing it.

### Cloud computing

- **On-premise networks** keep all equipment at a company-owned location.
- **Cloud computing** uses remote servers, applications, and network services hosted by a cloud service provider (CSP). Customers use them through the CSP's API or web console.
- **Service models:**
  - **SaaS:** CSP-operated software used remotely.
  - **IaaS:** Virtual compute and storage resources.
  - **PaaS:** Tools for building custom cloud applications.

---

*The Botium Toys scenario is a training exercise. The company and its audit results are fictional.*
