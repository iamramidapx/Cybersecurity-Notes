# Linux Basics: Worked Examples

Examples based on the notes in `Linux.docx` (TryHackMe-style tasks).

---

## 1. Check disk space: `df -h`

```bash
df -h
```

- `df` = disk free
- `-h` = human-readable sizes (GB / MB)

Example output:

```text
Filesystem      Size  Used Avail Use% Mounted on
/dev/root        70G   12G   58G  17% /
tmpfs           2.0G     0  2.0G   0% /dev/shm
```

How to read it:

| Column     | Meaning                    |
| ---------- | -------------------------- |
| Filesystem | Disk name                  |
| Size       | Total size                 |
| Used       | Space already used         |
| Avail      | Space remaining            |
| Use%       | Percentage used            |
| Mounted on | Where it is mounted        |

Key points:

- Focus on the `/dev/root` row: it is the main disk.
- "How much free disk space?" means read the **Avail** column (58G in the example).
- `tmpfs` is temporary storage in RAM (cache, temp files), not a real disk, so ignore it here.

---

## 2. File system structure

```text
/                  <- root (the "world")
└── home           <- the "country"
    └── ubuntu     <- your "house"
        └── .logs
            └── archive
                └── day1_report.txt
```

---

## 3. Paths must be exact

Full path:

```text
/home/ubuntu/.logs/archive/day1_report.txt
```

Common mistakes:

```bash
# Wrong: incomplete path
cat /home/ubuntu/day1_report.txt
# cat: /home/ubuntu/day1_report.txt: No such file or directory

# Correct: full path
cat /home/ubuntu/.logs/archive/day1_report.txt
```

Hidden folders start with `.` (like `.logs`). Show them with:

```bash
ls -a
```

---

## 4. Basic commands

```bash
pwd                      # where am I?
ls                       # list files
ls -a                    # list files including hidden ones
cd /home/ubuntu          # go into a folder
cat file.txt             # read a file
find / -name day1_report.txt 2>/dev/null   # search for a file
```

---

## 5. Putting it together: the "No such file" problem

```bash
# Step 1: find the file
find / -name day1_report.txt 2>/dev/null
# -> /home/ubuntu/.logs/archive/day1_report.txt

# Step 2: read it using the exact path returned
cat /home/ubuntu/.logs/archive/day1_report.txt
```

---

## 6. Quick mindset cheat sheet

| Task keyword          | Command  |
| --------------------- | -------- |
| disk / storage / space | `df -h` |
| find a file           | `find`   |
| read a file           | `cat`    |
| list / see hidden     | `ls -a`  |
| where am I            | `pwd`    |

Remember:

1. The path must be exact.
2. A file is not a folder.
3. Use the right command (`cd` / `cat` / `ls`).
