# Summary: Create Another Algorithm (Python Lab)

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
