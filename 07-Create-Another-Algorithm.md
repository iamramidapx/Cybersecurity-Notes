# Summary: Create Another Algorithm (Python)

## Main Idea

Security analysts control who can access restricted content by keeping an **allow list** of IP addresses. This lab uses Python to automate that upkeep: read a text file of allowed IPs (`allow_list.txt`), remove the addresses that should no longer have access, and save the updated list back to the file.

**Example:** If `192.168.97.225` was allowed yesterday but the employee left the company, the algorithm removes that address from the file so it can no longer reach the restricted content.

## How the Algorithm Works (with Examples)

The sample data below is illustrative. The real file contents come from the lab.

### 1. Open and read the file
`with` opens the file and closes it automatically. `"r"` means read mode. `.read()` returns the whole file as **one string**.

```python
with open("allow_list.txt", "r") as file:
    ip_addresses = file.read()

# e.g. "192.168.97.225\n192.168.1.10\n192.168.58.57"
```

### 2. Convert the string to a list
`.split()` with no argument splits on whitespace (spaces and new lines), so each IP becomes its own item.

```python
ip_addresses = ip_addresses.split()

# e.g. ["192.168.97.225", "192.168.1.10", "192.168.58.57"]
```

**Why:** you can't easily check individual addresses inside one long string, but you can loop through a list.

### 3. Loop and check each address
The `for` loop visits each address, and `if ... in remove_list` checks whether it should be removed.

```python
remove_list = ["192.168.97.225", "192.168.58.57"]

for element in ip_addresses:
    if element in remove_list:
        ip_addresses.remove(element)

# e.g. ["192.168.1.10"]
```

### 4. Convert the list back to a string
Files are written as text, so `" ".join()` merges the list into one string, with a space between items.

```python
ip_addresses = " ".join(ip_addresses)

# e.g. "192.168.1.10"
```

### 5. Overwrite the file
`"w"` means write mode. It **deletes the old contents** and replaces them with the new string.

```python
with open("allow_list.txt", "w") as file:
    file.write(ip_addresses)
```

### 6. Verify the result
Reopen the file in read mode and print it to confirm the update worked.

```python
with open("allow_list.txt", "r") as file:
    text = file.read()

print(text)   # e.g. 192.168.1.10
```

## Packaging It as a Function

All the steps are combined into one reusable function:

```python
def update_file(import_file, remove_list):
    with open(import_file, "r") as file:
        ip_addresses = file.read()

    ip_addresses = ip_addresses.split()

    for element in ip_addresses:
        if element in remove_list:
            ip_addresses.remove(element)

    ip_addresses = " ".join(ip_addresses)

    with open(import_file, "w") as file:
        file.write(ip_addresses)

# Call it with a file name and a list of IPs to remove
update_file("allow_list.txt", ["192.168.25.60", "192.168.140.81", "192.168.203.198"])
```

**Benefits of a function:**
- **Reusable:** call it on any file with any list, e.g. `update_file("vpn_users.txt", ["10.0.0.5"])`.
- **Organized:** one call replaces many lines of code.
- **Less error-prone:** you don't have to copy and paste the steps each time.

## Key Takeaways

| Concept | Tool used | Purpose |
|---|---|---|
| Read a file | `open()`, `.read()` | Get the file's contents |
| String to list | `.split()` | Handle each IP individually |
| Repeat and check | `for` loop, `if ... in` | Find addresses to remove |
| Delete an item | `.remove()` | Drop it from the list |
| List to string | `" ".join()` | Prepare data for writing |
| Save changes | `open(..., "w")`, `.write()` | Update the original file |
| Reuse the logic | `def update_file(...)` | Automate the task |

## Worth Noting: A Hidden Bug

Removing items from a list while looping over that same list can make the loop **skip elements**.

```python
ips = ["1.1.1.1", "2.2.2.2", "3.3.3.3"]
remove = ["1.1.1.1", "2.2.2.2"]

for ip in ips:
    if ip in remove:
        ips.remove(ip)

print(ips)   # ['2.2.2.2', '3.3.3.3']  <- 2.2.2.2 should be gone!
```

After `1.1.1.1` is removed, the list shifts left and the loop jumps past `2.2.2.2`. A safer approach builds a new list instead:

```python
ips = [ip for ip in ips if ip not in remove]
print(ips)   # ['3.3.3.3']
```
# Update a File Through a Python Algorithm

## Overview

This document is a portfolio activity from a security-focused course. The scenario: you are a security professional at a health care company, and you must regularly update a file listing the IP addresses allowed to access a restricted subnetwork containing patient records. A separate **remove list** identifies IP addresses that must be taken off this **allow list** (`allow_list.txt`). The task is to write a Python algorithm that automates this update.

The document contains three parts: the activity instructions, a completed exemplar, and a blank template to fill in.

## The Algorithm

| Step | What it does | Key Python elements |
|------|--------------|---------------------|
| 1. Open the file | Assigns `"allow_list.txt"` to `import_file` and opens it for reading | `with` statement, `open(import_file, "r")`, `as file` |
| 2. Read the contents | Converts the file's contents into a string stored in `ip_addresses` | `.read()` |
| 3. Convert to a list | Splits the string into individual IP addresses so they can be removed one by one | `.split()` (splits on whitespace by default) |
| 4. Iterate through the remove list | Loops over each IP address in `remove_list` using `element` as the loop variable | `for element in remove_list:` |
| 5. Remove matching IPs | Checks whether `element` is in `ip_addresses`, and if so removes it | `if` conditional, `.remove()` |
| 6. Update the file | Joins the list back into a string separated by newlines, then overwrites the file | `"\n".join(...)`, `open(import_file, "w")`, `.write()` |

### Key Details

- **`with` and `open()`**: The `with` statement automatically closes the file after use. `open()` takes the file name and a mode (`"r"` to read, `"w"` to write).
- **`.read()` and `.write()`**: `.read()` turns file contents into a string. `.write()` writes a string to the file and replaces any existing content when the file is opened with `"w"`.
- **`.split()`**: Turns the whitespace-separated string of IPs into a list, which is necessary for removing individual elements.
- **`for` loop**: Repeats the same code for every IP address in the remove list.
- **`.remove()`**: Deletes an element from the list. It works cleanly here because `ip_addresses` contains **no duplicates**, and the preceding `if` check prevents an error when an element isn't in the list.
- **`.join()`**: Converts the revised list back into a string, using `"\n"` to place each IP address on its own line.

## What the Activity Requires

The finished portfolio document should include:

- Screenshots or typed versions of the Python code for each step
- Explanations of the syntax, functions, and keywords used
- A **project description** at the start (3–5 sentences)
- A **summary** at the end (4–6 sentences)
- Specific details on `with`/`open()`, `.read()`/`.write()`, `.split()`, the `for` loop, and `.remove()`

The instructions also recommend saving a copy of the finished work to use in a professional portfolio, followed by a self-assessment (Step 10).

## Purpose

The exercise demonstrates how Python can automate a real access-control task: keeping an allow list current so that IP addresses no longer authorized cannot reach restricted patient data. It also serves as a portfolio piece showing file handling, string and list manipulation, loops, and conditionals to potential employers.

---  

# Python Debugging

## 1. Types of Errors

| Type | What it is | Error message? | Example |
|---|---|---|---|
| **Syntax error** | Invalid use of Python's grammar | Yes (`SyntaxError`, or `IndentationError`, a subclass) | Missing colon, quote, or closing bracket |
| **Logic error** | Code is valid and runs, but gives unintended results | No | Using `>=` instead of `<` in a condition |
| **Exception** | Syntactically correct code that cannot execute | Yes | Undefined variable, bad index |

**Common exceptions**
- `NameError`: a variable or function was never assigned or defined.
- `IndexError`: an index doesn't exist in the sequence (e.g., `usernames[3]` in a 3-item list).
- `TypeError`: wrong data type used (e.g., adding a string to an integer).
- `FileNotFoundError`: opening a file that doesn't exist at the given location.

**Useful notes**
- Python reports errors one at a time, starting with the first it hits. Fix it, rerun, and repeat.
- Syntax error messages usually point you to the fix. Logic errors and exceptions need extra strategies.

---

## 2. Debugging Strategies

- **Debuggers (in an IDE):** Use *breakpoints* to run code up to a chosen line, and inspect variable values as they change. Especially helpful for logic errors.
- **AI coding assistants** (e.g., Gemini Code Assist): Can analyze code, find errors, and suggest fixes. Always review and validate their output, since it may be inaccurate, suboptimal, or insecure.
- **Print statements:** Insert temporary prints (with descriptive text or line numbers) to trace execution flow.
  - *Example:* prints showed `.append()` ran for every user, even those already in `approved_users`. The fix is to put `.append(user)` inside an `else` block.

---

## 3. Lab Solutions (Activity: Debug Python Code)

| Task | Error type | Problem | Fix |
|---|---|---|---|
| 1 | Syntax | `for i in range(10)` has no colon | Add `:` |
| 2 | Syntax | Missing closing quote and comma after `"zdutchma"` | Close the string and separate elements with commas |
| 3 | Syntax | `print("update needed".upper()` is missing `)` | Add the closing parenthesis |
| 4 | Syntax ×2 + exception | `username_list` misspelled (`NameError`); `=` used instead of `==`; `print` not indented | Use `usernames_list`, `==`, and indent the body of the `if` |
| 5 | Exception (`IndexError`) | `usernames_list[5]` on a 5-item list | Use index `[4]` (indexing starts at 0) |
| 6 | Syntax + exception | `with open(...)` missing colon; `split.ip_addresses()` is wrong | Add `:` and use `ip_addresses.split()` |
| 7 | Logic | Indexes in `patch_schedule` were mismatched (OS 1 → `[2]`, OS 2 → `[0]`) | OS 1 → `[0]`, OS 2 → `[1]`, OS 3 → `[2]` |

**Task 4 corrected code**
```python
for name in usernames_list:
    if name == username:
        print("The user is an approved user")
```

**Task 6 corrected code**
```python
with open(import_file, "r") as file:
    ip_addresses = file.read()

ip_addresses = ip_addresses.split()
```

### Lab takeaways
- Read the error message: it names the error type and the line.
- Lists use 0-based indexing.
- `==` compares; `=` assigns.
- Colons, quotes, parentheses, and indentation matter in Python.

---

## 4. Beyond Parsing: Data Structures for Security Analysis

- **Sets:** Automatically remove duplicates. Useful for finding *unique* IP addresses or attackers.
- **Dictionaries:** Map a key to a value. Useful for frequency analysis, such as failed logins per user.

```python
stats = {}

with open("security.log", "r") as file:
    for line in file:
        level = line.split(":")[0]
        if level in stats:
            stats[level] += 1
        else:
            stats[level] = 1

# e.g. {"INFO": 500, "ERROR": 12, "WARNING": 45}
```

---

## 5. Big Picture: Python for Security Automation

1. **Why automate:** Increases speed, consistency, and scalability; lowers Mean Time to Respond (MTTR); reduces alert fatigue.
2. **Core Python tools:** Conditionals (`if`/`else`) make decisions; loops (`for`/`while`) process many items.
3. **File parsing:** Turns unstructured text into organized data.
   - Open files safely with `with open(file, "r") as f:` (auto-closes the file).
   - Use `.read()` to get the contents as a string.
   - Use `.split()` to break strings into lists, then index (e.g., `[0]`) to pull out values.
4. **Advanced handling:** Sets for uniqueness, dictionaries for counting.

---

## 6. Quiz Answers at a Glance

**Error-types quiz**
- Three error types: syntax errors, logic errors, exceptions.
- Python's "else if" keyword is `elif` (not `elsif`).
- Code that runs but produces the wrong result is a **logic error**.
- An unknown or out-of-range index is an **exception** (`IndexError`).

**Final quiz**
1. Identifying and fixing errors in code is **debugging**.
2. The `device_id = "p35rv47` error is a **missing quotation mark**.
3. Fix: **indent** the line assigning `"Charley"` to `first_name`.
4. `username_list[10]` on a 5-item list is an **exception** (`IndexError`).
5. Print statements help **identify which sections of code are working properly**.
6. Open a file for reading: `with open("logs.txt", "r") as file:`
7. String to list: `device_ids = logins.split()`
8. **Parsing** is converting data into a more readable, structured format.
9. `read_text = text.read()` reads the file in `text` and stores its contents as a **string** in `read_text`.
10. To automate login review, use **an `if` statement** (check for failed logins), **a `for` loop** (iterate the list), and **`split()`** (turn login info into a list).

