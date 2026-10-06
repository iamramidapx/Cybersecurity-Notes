# Tools of the Trade: Linux and SQL: Study Notes

My personal summary notes from the Coursera course *Tools of the Trade: Linux and SQL*, which covers foundational computing skills for security work: talking to the Linux operating system through the command line and querying databases with SQL.

> These notes cover SQL (filtering, joins, aggregate functions) first, then operating systems, Linux, and the shell.
> They are written in my own words for study purposes and are not affiliated with or endorsed by Coursera or the course authors. Quiz answers are intentionally left out, to respect the course honor code.

## Contents

1. [SQL filtering](#1-sql-filtering)
2. [SQL joins](#2-sql-joins)
3. [Aggregate functions](#3-aggregate-functions)
4. [Operating systems](#4-operating-systems)
5. [How a computer starts and completes tasks](#5-how-a-computer-starts-and-completes-tasks)
6. [Virtualization](#6-virtualization)
7. [CLI vs. GUI](#7-cli-vs-gui)
8. [Linux architecture](#8-linux-architecture)
9. [Linux distributions](#9-linux-distributions)
10. [Package managers and APT](#10-package-managers-and-apt)
11. [Shells and Bash](#11-shells-and-bash)
12. [Lab walkthroughs](#12-lab-walkthroughs)

---

## 1. SQL filtering

Filters narrow a query with a `WHERE` clause.

| Operator | Use | Example |
|---|---|---|
| `AND` | Both conditions must be true | `WHERE country = 'USA' AND state = 'NV'` |
| `OR` | Either condition can be true | `WHERE country = 'Brazil' OR country = 'Argentina'` |
| `NOT` | Excludes matching records (`!=` and `<>` also work) | `WHERE NOT status = 'successful'` |
| `BETWEEN ... AND ...` | Values within a range, with `AND` separating start and end | `WHERE date BETWEEN '2015-01-01' AND '2015-01-04'` |

Example (Chinook sample database):

```sql
SELECT firstname, lastname, country
FROM customers
WHERE country = 'Brazil' OR country = 'Argentina';
```

## 2. SQL joins

A **relational database** has tables that relate to each other. A **join** combines tables that share a common column, which is useful because security data (machines, employees, login attempts) is spread across tables. Rows and records mean the same thing here.

The table after `FROM` is the **left** table. The table after the join keyword is the **right** table. Columns are written as `table.column`.

| Join | Returns |
|---|---|
| `INNER JOIN` | Only rows that match in both tables |
| `LEFT JOIN` | All rows from the left table, plus matches from the right |
| `RIGHT JOIN` | All rows from the right table, plus matches from the left |
| `FULL OUTER JOIN` | All rows from both tables |

Rows with no match come back with `NULL` in the other table's columns.

Syntax:

```sql
SELECT *
FROM employees
INNER JOIN machines ON employees.employee_id = machines.employee_id;
```

Choosing a join: ask which table must keep all its rows. For example, to keep every record of `employees_remote` and only the matching `log_in_attempts` rows (joined on `username`), `employees_remote` must be the preserved side.

## 3. Aggregate functions

Aggregate functions calculate over multiple rows and return only the result, not the underlying data.

| Function | Returns |
|---|---|
| `COUNT` | Number of rows returned by the query |
| `AVG` | Average of the numeric values in a column |
| `SUM` | Sum of the numeric values in a column |

---

## 4. Operating systems

An operating system (OS) is the interface between computer hardware and the user. It lets people interact with the machine, runs many applications at once, and manages limited resources such as CPU and memory.

| OS | Notes |
|---|---|
| Windows | Introduced 1985. Closed source. |
| macOS | Introduced 1984. Partially open source (for example, the kernel). |
| Linux | First released 1991. Fully open source. Very important in security. |
| ChromeOS | Launched 2011. Partially open source, built on Chromium OS. Common in education. |
| Android | Mobile. Public release 2008. Open source. |
| iOS | Mobile. Introduced 2007. Partially open source. |

**Vulnerabilities.** Every OS has security issues. Legacy systems are especially risky because they no longer receive updates. Even up-to-date systems can be attacked, so analysts track published vulnerability sources such as:

- Microsoft Security Response Center (MSRC)
- Apple Security Updates
- Ubuntu CVE report
- Google Cloud Security Bulletins

Keeping systems patched is one of the main defences, but it is hard to keep everything updated, so analysts also need to understand legacy systems and their risks.

## 5. How a computer starts and completes tasks

**Booting**

1. The BIOS or UEFI chip activates. UEFI replaced BIOS on newer systems (it became common around 2007) and adds security features.
2. It runs loading instructions, such as checking hardware health.
3. Its last step starts the **bootloader**.
4. The bootloader boots the OS.

**Completing a task** follows four parts:

`User → Application → Operating system → Hardware`

The user makes a request in an application. The OS interprets it and routes it to the right hardware, for example the CPU for a calculation or the hard drive for saving a file. Results travel back up through the OS to the application.

Analogy: ordering at a restaurant. The app is the order, the hardware output is the food, and the OS is the kitchen you never see.

## 6. Virtualization

A **virtual machine (VM)** is a software-simulated computer with its own virtual CPU, storage, and other components. One physical host can run several VMs by splitting its resources (for example, 16 GB of RAM shared across the host and three VMs). Each VM runs its own OS.

**Benefits**

- **Security.** VMs are isolated "guests", so they work as sandboxes. Malware can be contained or studied there. Isolation is not perfect, because malicious code can sometimes escape to the host, so VMs should never be fully trusted.
- **Efficiency.** Many VMs can run on one machine and you can switch between them easily, like many passengers sharing one bus.

**Hypervisors** manage VMs and allocate shared resources. **KVM** (Kernel-based Virtual Machine) is an open-source hypervisor built into the Linux kernel.

Virtualization also covers virtual servers and virtual networks, not just VMs.

## 7. CLI vs. GUI

| | GUI (graphical) | CLI (command line) |
|---|---|---|
| Display | Icons, windows, graphics | Text only |
| Requests | One at a time | Many at once |
| Best for | Beginners, visual workflows | Repetitive or bulk tasks, automation |

**Why analysts like the CLI**

- **Efficiency.** Once you know it, bulk work (such as creating many files) is much faster, and commands can be scripted.
- **History file.** The Linux CLI records commands you ran. This helps you check that a playbook was followed correctly during an incident, and may let you trace an attacker's actions on a compromised system.

Security analysts should be comfortable with both.

## 8. Linux architecture

Linux is a **multi-user** system: several users can share the same resources at once.

| Component | Role |
|---|---|
| **User** | The person who starts and manages tasks. |
| **Applications** | Programs that perform specific tasks. Installed and managed with a package manager. |
| **Shell** | The command-line interpreter. It translates your text commands for the kernel and returns responses. |
| **Filesystem Hierarchy Standard (FHS)** | Defines how directories and files are organized, so the OS knows where data lives. |
| **Kernel** | The core that manages processes, memory, and hardware resources. It is unique to Linux. |
| **Hardware** | The physical components. |

**Example flow:** creating a file. You type the command in the **shell**. The file is created in the **FHS**. Its contents and location pass to the **kernel**. The kernel tells the **hardware** how and where to save it.

**Hardware categories**

- **Peripheral devices** are optional, attachable components such as a monitor, keyboard, mouse, or printer.
- **Internal hardware** is what the computer needs to run, all attached to the motherboard:
  - **CPU** is the main processor and executes program instructions.
  - **RAM** is short-term memory. It is cleared when programs close or the power goes off.
  - **Hard drive** is long-term storage that persists without power.

## 9. Linux distributions

Because the kernel is open source, anyone can build a new **distribution** on top of it.

| Distribution | Focus | Base |
|---|---|---|
| **Kali Linux** | Penetration testing and digital forensics tools preinstalled | Debian |
| **Ubuntu** | User-friendly and general purpose. Large community and widely used in cloud. | Debian |
| **Parrot** | Security, privacy, and development. Pen-testing and forensics tools preinstalled. Easy GUI. | Debian |
| **Red Hat Enterprise Linux (RHEL)** | Paid, enterprise-grade, with dedicated support | Red Hat |
| **AlmaLinux** | Community-driven drop-in replacement for CentOS 8 | Red Hat family |
| **CentOS** | Community enterprise server distribution. Final stable release (CentOS 8) was December 2021. | RHEL |

Key terms:

- **Penetration test:** a simulated attack used to find vulnerabilities.
- **Digital forensics:** collecting and analyzing data to work out what happened after an attack.

## 10. Package managers and APT

- A **package** is software that can be combined with others to form an application. It includes **dependencies**, the supporting files an app needs.
- A **package manager** installs, manages, and removes packages, and resolves dependency issues.
- Always prefer the latest version, because it carries the newest bug fixes and security patches.

**Which tool goes with which family**

| Family | Package manager | File type | Management tool |
|---|---|---|---|
| Debian-derived (Kali, Ubuntu, Parrot) | dpkg | `.deb` | **APT** |
| Red Hat-derived (RHEL, CentOS) | RPM | `.rpm` | **YUM** |

APT and YUM are command-line tools that make basic package tasks easier than using the low-level package managers directly.

## 11. Shells and Bash

The shell is the translator between you and the OS. When you enter a command, the shell interprets it, sends it to the kernel, and returns the result.

| Shell | Name |
|---|---|
| bash | Bourne-Again Shell |
| csh | C Shell |
| ksh | Korn Shell |
| tcsh | Enhanced C Shell |
| zsh | Z Shell |

All use common Linux commands but differ in features. For example, bash and ksh show a `$` prompt, while zsh uses `%`.

**Bash** is the default on most distributions, is user-friendly, and is the most popular shell in cybersecurity.

**Input and output streams**

| Stream | Meaning |
|---|---|
| Standard input (stdin) | What you send to the OS |
| Standard output (stdout) | Results the OS sends back |
| Standard error (stderr) | Error messages returned through the shell |

After a command, the shell returns either output or an error message.

## 12. Lab walkthroughs

### Lab 1: Installing apps with APT

Scenario: install, uninstall, and reinstall the network security tools **Suricata** and **tcpdump** in a Debian-based Bash shell.

```bash
apt                          # confirm APT is installed (shows usage info)
sudo apt install suricata    # install Suricata
suricata                     # verify it runs (prints version/usage)
sudo apt remove suricata     # uninstall
suricata                     # now errors: command not found
sudo apt install tcpdump     # install tcpdump
apt list --installed         # list installed apps; tcpdump shows, Suricata doesn't
sudo apt install suricata    # reinstall
apt list --installed         # both are listed
```

Takeaways:

- Installing and removing software needs elevated privileges, so use `sudo`.
- Installing may pull in extra dependencies and ask you to confirm.
- Running the app is a quick way to verify an install. Listing installed packages confirms the exact set and versions.
- The labs run in a VM, so mistakes can't damage a real system and you can revert to an earlier state.

### Lab 2: Input and output in the shell

```bash
echo hello          # prints: hello
echo "hello"        # same output; quotes group characters together
expr 32 - 8         # 24 (e.g., false-positive alerts: 32 total minus 8 actionable)
expr 3500 \* 12     # 42000 (expected yearly logins from a monthly average)
clear               # clears the screen
```

Notes on `expr`:

- Operators are `+`, `-`, `*`, `/`. In Bash, `*` usually needs escaping as `\*`.
- Terms and operators must be separated by spaces (`expr 25 + 15`, not `expr 25+15`).
- It does integer math only, and results are rounded down.

`echo hello` is the **input**, and `hello` is the **output**.

### Lab 3: SQL joins on the `organization` database

Scenario: during a security incident investigation, find which employees use which machines, which machines or users have no match, and every employee's login attempts. The lab opens in the MariaDB shell; to reconnect run `sudo mysql organization`.

```sql
-- 1. Employees matched to their machines (185 rows)
SELECT *
FROM machines
INNER JOIN employees ON machines.device_id = employees.device_id;

-- 2a. All machines, even unassigned ones (unassigned rows show NULL username)
SELECT *
FROM machines
LEFT JOIN employees ON machines.device_id = employees.device_id;

-- 2b. All employees, even those without a machine
SELECT *
FROM machines
RIGHT JOIN employees ON machines.device_id = employees.device_id;

-- 3. Login attempts made by employees (200 rows)
SELECT *
FROM employees
INNER JOIN log_in_attempts ON employees.username = log_in_attempts.username;
```

Takeaways:

- Join on the shared column (`device_id`, `username`), always qualified as `table.column`.
- The left join finds machines that belong to no user. The right join finds users with no machine.
- Inner joins keep only matched rows, so use them when you only care about records that exist on both sides.

---

*Source course: [Tools of the Trade: Linux and SQL](https://www.coursera.org/learn/linux-and-sql) on Coursera.*
