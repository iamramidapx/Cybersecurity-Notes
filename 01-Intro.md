# Module 01 — Networks & The Internet

> **My learning journey:** BSc Microbiology → Forensic Pathology Internship → Cybersecurity
> Google Cybersecurity Certificate | Targeting MSc Cybersecurity 

---

## What is a Network?

A network is any collection of connected entities, such as smartphones, laptops, and sensors.

The **Internet** is a global network of smaller interconnected networks:
- Private networks: smaller, self-contained groups (local, internal) 
- Public networks: inked together via public infrastructure 
---

## Device Identification

Each device on a network must be uniquely identifiable: This is achieved through the use of specific labels or addresses.
- A name, which can be changed
- A unique physical identifier, which cannot be changed


### MAC Address (Media Access Control address)
- Functions: a built-in serial number unique to each device
- Permanent, 12-character hexadecimal identifier (0–9 and a–f) but only on the physical hardware.
- Burned in at manufacturing (e.g. `a4:c3:f0:85:ac:2d`)
- First half = manufacturer | Second half = unique device ID
- ⚠️ Can be **spoofed** (faked) to impersonate another device

### IP Address (Internet Protocol address)
- Changeable numerical label composed of four segments, known as octets.(four octets, 0–255 each)
- Temporarily identifies a device
- An uniquely identifies a device within a specific network 
- Cannot be duplicated or used by multiple devices at the same time on that network.
- Two types:


| Type | Range Example | Use |
|---|---|---|
| Private IP addresses | ClassA,B,C | used within local/internal networks(home or office) |
| Public IP addresses | Assigned by ISP | Communicates over Internet|

- Routers use **NAT** (Network Address Translation) to let many private devices share one public IP.

---

## Ping
- One of the most essential tools in networking, whether a connection is active and how reliable it is.
- Uses **ICMP Echo Request/Reply** packets
- Measures round-trip latency

```
ping 192.168.1.1
```

---

## Network Topologies (Local Area Networks-LAN Layouts)

### 1. Ring Topology
Devices connected in a closed loop — data travels in a single direction.

**Benefits:**
- Less cabling required 
- Lower reliance on specialized networking hardware
- More resistant to traffic congestion than bus topology
- Easy fault isolation — single direction makes problems easier to trace

**Drawbacks:**
- Inefficient — data may pass through many devices before reaching destination
- Single point of failure — one broken cable or device brings down the whole network

---

### 2. Bus Topology
All devices share a single central cable, known as a backbone

**Benefits:**
- Cost-effective and simple to set up — good for small or temporary networks
- Minimal cabling needed
- No specialized hardware required

**Drawbacks:**
- Performance drops when multiple devices communicate simultaneously
- Difficult to troubleshoot — all traffic shares one route
- No redundancy — If the backbone cable failure fails, the entire network goes down.

---

### 3. Star Topology
Each device is individually connected to a central networking component, such as a switch.

**Benefits:**
- Highly scalable — adding new devices is simple and minimally disruptive
- Reliable and widely used — fewer general points of failure

**Drawbacks:**
- Higher cost — more cabling and specialized equipment needed
- More maintenance required as network grows
- Central switch is a single point of failure — If it fails, all communication stops.

---

## Networking Devices

### Switch
- Connects multiple devices within a network—such as computers, printers
- Each device gets its own port configurations (4, 8, 16, 24, 32, or even 64 ports)
- Tracks which device is on which port
- Forwards data **only to intended destination** (more efficient than hubs), reducing unnecessary network traffic

### Router
- Connects **different networks** together
- Directs data between networks via **routing**
- Find a suitable path for data across interconnected networks

---

## Subnetting

Divides a large network into smaller, more manageable pieces

Three key components:
- **Network Address** — identifies the network itself (e.g. 192.168.1.0)
- **Host Address** — identifies specific devices(hosts) on that network (e.g. 192.168.1.100)
- **Default Gateway** — sends data to other networks (e.g. 192.168.1.254)

### CIDR Notation Quick Reference

| CIDR | Subnet Mask | Usable Hosts | Common Use |
|---|---|---|---|
| /8 | 255.0.0.0 | 16,777,214 | Very large networks |
| /16 | 255.255.0.0 | 65,534 | Medium networks |
| /24 | 255.255.255.0 | 254 | Small networks LANs |
| /30 | 255.255.255.252 | 2 | Point-to-point links |

**Real example:** A café separating staff devices from public Wi-Fi = subnetting in action.

### Why Subnetting Matters
- Reduces broadcast traffic
- Increases security and control
- Allows efficient use of IP space
- Enables network segmentation

---

## ARP — Address Resolution Protocol

Links a device's **IP address** to its **MAC address** so they can communicate properly on a network.


Process:
1. **ARP Request** — broadcast: *"Who has this IP address?"*
2. **ARP Reply** — owning device responds with its MAC address

Each device keeps an **ARP cache** (a small table) that stores mappings of IP-to-MAC addresses for known devices.

--- 

IP addresses can be assigned to devices in two ways:
1. Manually configure - by entering them directly into the device's settings (static IP address)
2. Automatically - use a DHCP server (dynamic IP address)

## DHCP — Dynamic Host Configuration Protocol
Automatically assigns dynamic IP addresses to devices on a network using a four-step process called **DORA**:

1. **Discover** — device broadcasts looking for DHCP server e.g. A device connects to a network and doesn’t already have an IP address.
2. **Offer** — server proposes an available IP with a DHCP Offer
3. **Request** — device asks to use that IP with a DHCP Request
4. **Acknowledgment (ACK)** — server confirms, device starts using it


---

## My Notes

- Subnetting = like compartmentalizing a lab (prevent security leaks)
- ARP spoofing = like DNA mimicry in bacteria — pretending to be something trusted
- DHCP = automated resource allocation, like how immune cells get assigned roles

---

*Last updated: 2026 | Next module: Foundations of Cybersecurity
