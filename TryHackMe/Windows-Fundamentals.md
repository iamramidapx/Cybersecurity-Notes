# Windows Fundamentals

Study notes covering Windows editions, built-in utilities, Windows security features, and Active Directory / Group Policy (based on the TryHackMe *Windows Fundamentals* series).

---

## 1. Windows Editions & Quick Exam Facts

- **BitLocker (full-disk encryption) is only in Pro and higher** (Enterprise/Education), not Home. Microsoft uses *feature segmentation*: Home = basic features, Pro = advanced control and security. Some Home devices have a limited, automatic **Device Encryption** instead.
- **Taskbar:** hide the Search box via the *Hidden* option; Task View button is removed by unchecking *Show Task View button*.
- **Notification area** (besides Clock and Network) shows **Action Center**. Use official Windows labels (e.g. *Volume*, *Action Center*).
- **Windows folder:** `C:\Windows` normally, but not guaranteed; the system environment variable **`%windir%`** always points to it.
- **Local User and Group Management:** `lusrmgr.msc` (Run dialog).
- **UAC (User Account Control):** introduced in Vista. Admin sessions do not run elevated by default; Windows prompts before privileged operations. By default it does **not** apply to the built-in Administrator account.

---

## 2. Built-in Utilities

| Tool | Command | Purpose |
|---|---|---|
| System Information | `msinfo32` | Hardware Resources, Components, Software Environment (incl. Environment Variables, network connections); has a search bar |
| Resource Monitor | `resmon` | Per-process CPU, Memory, Disk, Network usage; real-time graphs; for advanced troubleshooting |
| Registry Editor | `regedit` | Central hierarchical database of config (user profiles, installed apps, hardware, ports). **Advanced users only — changes can break the system** |
| System Configuration | `msconfig` | Launcher for many of the above tools |
| Windows Defender Firewall | `wf.msc` | Advanced firewall configuration |
| Windows Update (Control Panel) | `control /name Microsoft.WindowsUpdate` | Open Windows Update from Run/CMD |

**Environment variables** store OS info (OS path, processor count, temp folder locations). View via msinfo32, *Control Panel > System and Security > System > Advanced system settings*, or *Settings > System > About > Advanced system settings*.

### Command Prompt essentials

- `hostname` – computer name
- `whoami` – logged-in user
- `ipconfig` – network address settings
- `netstat` – protocol statistics and current TCP/IP connections (parameters like `-a`, `-b`, `-e`)
- `net` – manage network resources (sub-commands: `user`, `localgroup`, `use`, `share`, `session`)
- `cls` – clear screen
- **Help:** `command /?` (e.g. `ipconfig /?`), but for `net` use `net help <subcommand>` (e.g. `net help user`)

---

## 3. Windows Security Features

### Windows Update
- Provides security updates, feature enhancements and patches (also for Defender).
- **Patch Tuesday** = 2nd Tuesday of each month; urgent fixes may ship out-of-band.
- Since Windows 10, updates can only be **postponed**, not ignored indefinitely; restarts can be scheduled.
- "Managed" update settings are typical in enterprise environments, not on home PCs.

### Windows Security (Settings) — protection areas
Virus & threat protection · Firewall & network protection · App & browser control · Device security

Status icons: **Green** = protected, **Yellow** = recommendation, **Red** = needs immediate attention.

### Virus & threat protection
- **Scan options:** Quick (common threat locations), Full (all files/programs; can exceed 1 hour), Custom.
- **Threat history:** last scan, quarantined threats (isolated), allowed threats (only allow if 100% sure).
- **Settings:** Real-time protection, Cloud-delivered protection, Automatic sample submission, Controlled folder access, Exclusions, Notifications.
- **Controlled folder access** = ransomware protection; requires Real-time protection; only trusted apps can modify protected folders.
- **Exclusions** reduce false positives but can hide real threats.
- Right-click any file/folder → *Scan with Microsoft Defender*.

### Firewall
A firewall controls what may pass through network ports (like a security guard checking IDs). Three profiles:
- **Domain** – host can authenticate to a domain controller
- **Private** – user-assigned, for home/private networks
- **Public** – default; for coffee shops, airports, etc.

Per profile you can turn the firewall on/off and **block all incoming connections**; you can also allow specific apps. Leave it enabled unless you are certain.

### App & browser control
- **Microsoft Defender SmartScreen** – protects against phishing/malware sites and malicious downloads; modes: *Warn*, *Block*, *Off*.
- **Exploit protection** – built-in defences against attacks; leave defaults.

### Device security
- **Core isolation → Memory integrity:** prevents malicious code being injected into high-security processes.
- **TPM (Trusted Platform Module):** tamper-resistant hardware crypto-processor.
- **BitLocker:** drive encryption protecting against data theft from lost/stolen/decommissioned machines; strongest when paired with TPM 1.2+.

### Volume Shadow Copy Service (VSS) / System Protection
- Creates point-in-time snapshots, stored in `System Volume Information`.
- Enables: create restore point, system restore, configure settings, delete restore points.
- **Security note:** malware (especially ransomware) hunts and deletes shadow copies — keep **offline/off-site backups**.

---

## 4. Windows Domains & Active Directory

### Why domains?
Managing a few PCs by hand works; hundreds of PCs and users across offices does not. A **Windows domain** centralises administration:
- **Active Directory (AD)** – the central repository
- **Domain Controller (DC)** – the server running AD services
- Benefits: **centralised identity management** and **centralised security policies**.
- Real-world example: campus logins work on any machine because authentication is forwarded to AD; policies stop you accessing Control Panel.

### AD objects (Active Directory Domain Services)
- **Users** – security principals (can be authenticated and granted privileges). Represent **people** or **services** (e.g. IIS, MSSQL service accounts with minimal privileges).
- **Machines** – every domain-joined computer gets an account named `COMPUTERNAME$` (e.g. `DC01$`); local admin on its own computer; password auto-rotated, ~120 random characters.
- **Security Groups** – grant access to resources; can contain users, machines and other groups; members inherit privileges.

Key default groups:

| Group | Purpose |
|---|---|
| Domain Admins | Admin over the entire domain, including DCs |
| Server Operators | Administer DCs (cannot change admin group memberships) |
| Backup Operators | Access any file regardless of permissions (for backups) |
| Account Operators | Create/modify other accounts |
| Domain Users | All user accounts |
| Domain Computers | All computers |
| Domain Controllers | All DCs |

### Tools & structure
- **Active Directory Users and Computers** – manage users, groups, machines; reset passwords.
- **Organizational Units (OUs)** – containers for classifying users/machines, usually mirroring the business structure (IT, Management, Marketing, R&D, Sales). **A user can be in only one OU.**
- Default containers: **Builtin**, **Computers** (default landing spot for joined machines), **Domain Controllers**, **Users**, **Managed Service Accounts**.

### OUs vs Security Groups
- **OUs → apply policies** (a user belongs to one).
- **Security Groups → grant permissions** to resources like shares/printers (a user can belong to many).

### Admin tasks covered
1. **Delete an OU:** enable *Advanced Features* (View menu), untick *Protect object from accidental deletion* (Object tab), then delete (removes everything beneath it). Create/delete users to match the org chart.
2. **Delegation:** right-click OU → *Delegate Control* to let e.g. IT support reset passwords for Sales/Marketing/Management without being Domain Admin. Delegated users may lack access to the GUI, so use PowerShell:
   ```powershell
   Set-ADAccountPassword sophie -Reset -NewPassword (Read-Host -AsSecureString -Prompt 'New Password') -Verbose
   Set-ADUser -ChangePasswordAtLogon $true -Identity sophie -Verbose
   ```
3. **Organise machines:** don't leave everything in *Computers*. Split into **Workstations** (daily-use; should never have privileged users signed in), **Servers** (provide services), and **Domain Controllers** (most sensitive — hold password hashes for all accounts).

---

## 5. Group Policy (GPO)

- A **Group Policy Object** is a collection of settings applied to OUs (user and/or computer configuration).
- Manage with **Group Policy Management**: create the GPO under *Group Policy Objects*, then **link** it to an OU. A GPO applies to the linked OU **and all child OUs**.
- **Scope** = where it's linked; **Security Filtering** defaults to *Authenticated Users*; **Settings** tab shows contents.
- *Default Domain Policy* is linked to the whole domain (password and account lockout policies, etc.). Example: set minimum password length to 10 via *Computer Configuration → Policies → Windows Settings → Security Settings → Account Policies → Password Policy*.
- Use the **Explain** tab on a policy to read what it does.
- **Distribution:** GPOs are shared through the **SYSVOL** network share on the DC (`C:\Windows\SYSVOL\sysvol\`). Clients can take up to **2 hours** to sync; force it with:
  ```powershell
  gpupdate /force
  ```

### Example GPOs
1. **Restrict Control Panel Access** – *User Configuration* policy *Prohibit access to Control Panel and PC settings*; linked to Marketing, Management and Sales OUs (not IT).
2. **Auto Lock Screen** – *Computer Configuration* policy for machine inactivity limit = **5 minutes**; linked to the **root domain** so Workstations, Servers and DCs inherit it (user-only OUs ignore computer settings).

Verify by logging in as a restricted user: Control Panel should be denied by the administrator.

---

## 6. Network Authentication (intro)

In a domain, all credentials live on the Domain Controllers. When a user authenticates to a service, the service asks the DC to validate the credentials. Windows domains use two protocols for network authentication: **Kerberos** and **NetNTLM**.

---

## Quick Reference — Commands & Shortcuts

`lusrmgr.msc` · `msinfo32` · `resmon` · `regedit` · `msconfig` · `wf.msc` · `hostname` · `whoami` · `ipconfig` · `netstat` · `net` · `net help <cmd>` · `cls` · `gpupdate /force` · `control /name Microsoft.WindowsUpdate` · `%windir%`
