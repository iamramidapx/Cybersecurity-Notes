# Assets, Threats, and Vulnerabilities — Study Summary

Personal study notes on core security-analyst concepts: how to identify what needs protecting, what can go wrong, and how organizations reduce risk. Written in my own words as a quick reference.

## Contents

1. [Risk, Threats, and Vulnerabilities](#1-risk-threats-and-vulnerabilities)
2. [Asset Management and Classification](#2-asset-management-and-classification)
3. [Cloud Security](#3-cloud-security)
4. [NIST Cybersecurity Framework (CSF)](#4-nist-cybersecurity-framework-csf)
5. [Risk Registers and Risk Scoring](#5-risk-registers-and-risk-scoring)
6. [Least Privilege and Access Auditing](#6-least-privilege-and-access-auditing)
7. [Data Lifecycle, Governance, and Privacy](#7-data-lifecycle-governance-and-privacy)
8. [Cryptography: Encryption and Hashing](#8-cryptography-encryption-and-hashing)
9. [Authentication: SSO and MFA](#9-authentication-sso-and-mfa)
10. [CI/CD Pipeline Security](#10-cicd-pipeline-security)
11. [OWASP Top 10 and OSINT](#11-owasp-top-10-and-osint)
12. [Vulnerability Management](#12-vulnerability-management)
13. [Threat Actors and Attack Vectors](#13-threat-actors-and-attack-vectors)
14. [Brute Force Attacks and Defenses](#14-brute-force-attacks-and-defenses)

---

## 1. Risk, Threats, and Vulnerabilities

| Term | Meaning |
|---|---|
| **Risk** | Anything that can affect the confidentiality, integrity, or availability (CIA) of an asset |
| **Threat** | A circumstance or event that can harm assets |
| **Vulnerability** | A weakness a threat can exploit |

- **Risk = Likelihood × Impact.** Analysts mostly influence the *likelihood* side by addressing threats and vulnerabilities.
- **Threats** are intentional (e.g., a hacker targeting a misconfigured app) or unintentional (e.g., an employee holding a door open for a stranger).
- **Vulnerabilities** are technical (e.g., misconfigured software) or human (e.g., a lost access card).

## 2. Asset Management and Classification

*You can only protect what you know you have.* Asset management means tracking assets and the risks that affect them.

- **Asset types:** digital (data), information systems, physical, and intangible (brand, IP).
- **Classification** labels assets by sensitivity and importance, which requires knowing what the asset is, where it is, who owns it, and how important it is.
- **Common scheme (highest to lowest):** Restricted → Confidential → Internal-only → Public. Governments often call their top tier "confidential."
- **Challenges:** ownership can be unclear (e.g., a work laptop with personal photos), and one piece of information can carry several classifications at once.

## 3. Cloud Security

- **Service models:**
  - **SaaS**: finished apps used in a browser (Gmail, Slack, Zoom)
  - **PaaS**: back-end tools for building and deploying apps
  - **IaaS**: remote servers, storage, and networking
- **Shared responsibility model:** the provider secures its side; the customer remains responsible for identity and access management, resource configuration, and data handling. The split shifts depending on SaaS/PaaS/IaaS.
- **Common challenges:** misconfiguration (especially relying on defaults), difficulty monitoring access, and regulatory compliance (HIPAA, PCI DSS, GDPR).

## 4. NIST Cybersecurity Framework (CSF)

A voluntary, flexible, industry-agnostic framework, first released in 2014.

- **Core:** desired security outcomes in six functions: **Identify, Protect, Detect, Respond, Recover, Govern** (Govern was added in 2024).
- **Tiers:** a 1–4 scale of how mature an organization's security program is.
- **Profiles:** tailored templates for a specific industry or organization.
- **Implementation outline (CISA guidance):** create a current profile → assess risk → analyze and prioritize gaps → implement an action plan.
- Regulations are mandatory rules; frameworks are optional resources.

## 5. Risk Registers and Risk Scoring

A **risk register** is a central record of risks to assets. Each risk gets a likelihood and a severity score (1–3), and **priority = likelihood × severity** (range 1–9).

| | Low severity (1) | Moderate (2) | Catastrophic (3) |
|---|:-:|:-:|:-:|
| **Certain (3)** | 3 | 6 | 9 |
| **Likely (2)** | 2 | 4 | 6 |
| **Rare (1)** | 1 | 2 | 3 |

Considerations when scoring: the operating environment (location, headcount, remote workers, third parties), financial and reputational harm, operational impact, and regulatory exposure. Higher scores get attention first.

## 6. Least Privilege and Access Auditing

**Principle of least privilege (PoLP):** give users only the access they need for their job. It limits exposure, reduces accidental changes, and aids monitoring. Related: **separation of duties**.

- **Account types:** guest, user, service, privileged.
- **Audit types:**
  - *Usage audits*: what is each account doing?
  - *Privilege audits*: has access crept beyond what's needed (**privilege creep**)?
  - *Account change audits*: are directory changes suspicious?

## 7. Data Lifecycle, Governance, and Privacy

- **Data lifecycle:** Collect → Store → Use → Archive → Destroy.
- **Governance roles:** *data owner* (decides access), *data custodian* (handles storage and transport safely), *data steward* (implements governance policy).
- **Sensitive data types:** **PII** (identifies a person), **SPII** (sensitive PII with stricter handling), **PHI** (health information, regulated by HIPAA).
- **Privacy vs. security:** privacy is about people's control over their data; security is about protecting that data in all states.
- **Key regulations:** **GDPR** (EU, applies to anyone handling EU residents' data), **PCI DSS** (payment cards), **HIPAA** (US health data).
- **Audits vs. assessments:** an audit checks controls against expectations (less frequent, often external); an assessment tests resilience to threats (more frequent, often internal).

## 8. Cryptography: Encryption and Hashing

**Encryption**
- **Symmetric**: one shared secret key (fast). Examples: AES (128/192/256-bit), 3DES.
- **Asymmetric**: public/private key pair. Examples: RSA, DSA.
- Real systems combine both. Longer keys resist brute force but cost more processing time.
- **Kerckhoffs's principle / "obscurity is not security":** a cryptosystem should stay secure even if its design is public. Avoid homemade algorithms.
- OpenSSL is a common key-generation tool; its 2014 Heartbleed bug is a reminder to patch.

**Hashing**
- A one-way function that turns input into a fixed-size digest, used for integrity checks and password storage.
- **MD5** (128-bit) is weak and prone to **collisions**; the **SHA family** (SHA-1, -224, -256, -384, -512) offers longer digests.
- **Rainbow tables** are precomputed hash lookups; **salting** (adding random data before hashing) defeats them and makes identical passwords hash differently.

## 9. Authentication: SSO and MFA

- **Single sign-on (SSO):** one login for many services via trusted third-party tokens (commonly LDAP/SAML). Better usability, lower management cost, fewer attack points, but a single point of failure.
- **Multi-factor authentication (MFA):** combines two or more of: something you **know**, something you **have**, something you **are**.
- Using SSO and MFA together improves security without hurting usability.

## 10. CI/CD Pipeline Security

CI/CD automates building, testing, and releasing software (Continuous Integration, Continuous Delivery, Continuous Deployment). Automated checks such as SAST, DAST, and compliance scans can be built in.

| Weakness | Mitigation |
|---|---|
| Insecure third-party dependencies | Scan and update regularly (SCA) |
| Misconfigured permissions | RBAC and least privilege |
| No automated security testing | Add SAST and DAST to the pipeline |
| Hardcoded secrets | Use a secrets manager or vault |
| Unsecured build environments | Harden; use containers or VMs |

Overall approach: **DevSecOps** and defense in depth.

## 11. OWASP Top 10 and OSINT

**OWASP** is a nonprofit that publishes the **Top 10** web application risks. Categories covered:

1. Broken access control
2. Cryptographic failures
3. Injection
4. Insecure design
5. Security misconfiguration
6. Vulnerable and outdated components
7. Identification and authentication failures
8. Software and data integrity failures (e.g., supply chain attacks like SolarWinds, 2020)
9. Security logging and monitoring failures
10. Server-side request forgery (SSRF)

**OSINT** (open-source intelligence) is analyzing publicly available information to produce actionable *intelligence*. Information is raw data; intelligence is analyzed, contextualized information that supports decisions.

## 12. Vulnerability Management

- **Vulnerability scanners** compare systems against known weaknesses (e.g., CVE list). Scan types: external vs. internal, authenticated vs. unauthenticated, limited vs. comprehensive.
- **Remediation:** apply patches and updates, replace **end-of-life** software, and improve procedures and training. Update strategies include manual and automatic, each with trade-offs.
- **Penetration testing:** authorized simulated attacks. Strategies vary by how much the tester knows up front. **Bug bounty programs** pay freelancers to report flaws.
- **Vulnerability assessment report:** system description, scope, purpose, risk assessment, approach, and remediation strategy.
- **NIST SP 800-30** provides a method for assessing risk: threat sources → threat events → likelihood → severity.

## 13. Threat Actors and Attack Vectors

**Threat actor categories (by motive):** competitors, state actors, criminal syndicates, insider threats, and shadow IT.

**Hacker types:** unauthorized (malicious), authorized (ethical), and semi-authorized (e.g., hacktivists).

**Advanced persistent threat (APT):** an actor who maintains long-term, stealthy access, often state-linked.

**Common attack vectors:** direct physical access, removable media (USB), social media, email, on-premises wireless, cloud services, and supply chains.

*Takeaway: think like an attacker, and match motives to the access points they would likely target.*

## 14. Brute Force Attacks and Defenses

**Attack styles:** simple brute force, dictionary attacks, reverse brute force, credential stuffing, and exhaustive key search. Tools include Hashcat, John the Ripper, THC Hydra, Aircrack-ng, and Ophcrack; defenders use them for testing too.

**Defenses:** hashing and salting, MFA, CAPTCHA, and strong password policies (length and complexity).

---

## Hands-On Practice

- **Linux/Bash labs:** used `ls`, `cat`, and hidden files to break a Caesar cipher, and generated and compared file hashes to detect tampering.
- **Risk register exercise:** scored risks for a fictional bank (likelihood × severity).
- **Reflection and scenario work:** attack surfaces of a home network and a USB-drive ("parking lot") exercise.

## Source

Based on coursework from the Google Cybersecurity Certificate (Course 5: *Assets, Threats, and Vulnerabilities*). This summary is an independent paraphrase for personal study and is not affiliated with or endorsed by Google or Coursera.
