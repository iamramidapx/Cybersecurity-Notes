# macOS File Types — Cybersecurity & Forensics Notes

Quick reference for common macOS file types, what they're used for, and why they matter in digital forensics / incident response.

## Summary Table

| File Type | Purpose | Windows/Linux Equivalent |
|---|---|---|
| `.app` | Application bundle | `.exe` |
| `.dmg` | Disk image installer | `.iso` |
| `.kext` | Kernel extension | Windows driver |
| `.dylib` | Shared/dynamic library | `.dll` (Windows) / `.so` (Linux) |
| `.xar` | Archive installer format | `.pkg` (predecessor) |
| `.plist` | Configuration/property list | Windows Registry |

## Details

### `.app` — Application Bundle
Not a single file — it's a folder disguised as an app (view with **Right click → Show Package Contents**).

```
AppName.app
└── Contents
     ├── MacOS        (actual executable)
     ├── Resources    (icons, images)
     ├── Frameworks   (libraries)
     └── Info.plist   (configuration)
```

**Forensics note:** Malware can hide inside `.app` bundles — check `Contents/MacOS/` for suspicious executables.

### `.dmg` — Disk Image
A virtual disk container, mounted by macOS (e.g. `/Volumes/Google Chrome/`). Typically used to distribute an installer (app + shortcut to `/Applications`).

**Forensics note:** Malware often spreads via malicious DMG installers — inspect mounted images.

### `.kext` — Kernel Extension
Adds functionality to the macOS kernel (drivers, VPN software, filesystem drivers, security tools). Since macOS Big Sur, third-party kernel extensions require user approval via Recovery Mode / a security policy change.

**Forensics note:** Kernel extensions can access memory, control hardware, and bypass protections — rootkits sometimes use `.kext` files.

### `.dylib` — Dynamic Library
Shared library format, allowing programs to reuse common code instead of duplicating it (e.g. `libSystem.dylib`, `libcrypto.dylib`).

**Forensics note:** Attackers may perform **Dynamic Library Hijacking** — replacing a legitimate dylib so a program loads malicious code instead.

### `.xar` — Archive Format
"eXtensible ARchive" format, used to package installers and extensions (e.g. Safari extensions, macOS installer packages). Superseded the older `.pkg` format.

**Forensics note:** Investigators extract `.xar` archives to inspect installation scripts or payloads.

### `.plist` — Property List
The macOS equivalent of the Windows Registry. Stores application settings, system configuration, user preferences, and metadata. Comes in two formats:
- **XML** (human-readable)
- **Binary** (requires tools like `plutil` or Xcode)

**Forensics note:** One of the most important artifact types in macOS forensics. Key locations investigators check:
- `~/Library/Preferences/`
- `~/Library/LaunchAgents/`
- `~/Library/LaunchDaemons/`

These often reveal installed applications, user activity, and **malware persistence mechanisms**.

### Most Commonly Abused by Malware
1. `.plist`
2. `.kext`
3. `.dylib`

---

## Appendix: APFS Volume UUID Lookup (TryHackMe-style lab note)

When a lab asks for **"the UUID of the volume containing user data"**, it wants the *Volume UUID* of the Data volume — not the container UUID, and not the numeric volume index (e.g. `4`) used by mounting tools like `apfs-fuse -v 4`.

**Steps:**
1. Run `apfsutil <image>` to list volumes and their UUIDs.
2. Identify the entry for the **Data** volume (e.g. `Macintosh HD - Data`).
3. Submit the UUID listed next to that volume, in standard UUID format:
   `xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx`
4. Confirm you mounted the right volume by checking `ls mac/root/Users` — it should show user home directories (e.g. `Shared`, a username).
