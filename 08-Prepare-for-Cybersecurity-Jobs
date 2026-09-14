# 08: Prepare for Cybersecurity Jobs

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
