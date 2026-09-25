# TryHackMe Cybersecurity Fundamentals — Cloud Computing & Operating Systems

> Notes summarized from a hands-on TryHackMe learning path covering cloud computing basics and operating system fundamentals.

## 1. Cloud Computing Basics

### Key Terminology

| Term | Definition |
|---|---|
| **Public Cloud** | Cloud services accessed over the internet, shared by many people and companies |
| **Private Cloud** | A cloud built for a single company, offering more control and security |
| **Hybrid Cloud** | A mix of public and private clouds that work together and share data |
| **IaaS** | Infrastructure as a Service — rent basic compute parts (servers, storage) from the cloud |
| **PaaS** | Platform as a Service — a ready-to-use environment to build and run apps without managing servers |
| **SaaS** | Software as a Service — software used online without installing anything (e.g., Gmail, Zoom) |
| **EC2** | Amazon's virtual computers/servers that can be quickly created, used, and resized on demand |

### Benefits of Cloud Computing
- Scalability
- On-demand self-service
- Pay only for what you use
- Security
- High availability
- Global access

### Deploying Cloud Environments (AWS Example)
- **EC2 (Virtual Computer/Server):** A virtual machine in the cloud with its own CPU and RAM, capable of running applications. Adding an EC2 instance adds a computer to your environment.
- **Instance Type** (e.g., `t2`, `t3`, `m5`): Describes the power of the virtual machine.
  - Bigger instances → more power, higher cost
  - Smaller instances → less power, lower cost
- **Region selection:** Cloud resources are deployed to a chosen geographic region (e.g., `us-east-1`, N. Virginia).
- Cybersecurity training environments often use the **IaaS** model, deploying multiple EC2 instances to get full OS-level access — needed to install tools, configure systems, and safely simulate attacks/defenses.

## 2. Operating Systems Overview

### Scenario / Learning Objectives
Starting point: inheriting an old, unfamiliar computer and needing to investigate what's actually running on it.

Objectives:
- Understand what an operating system is and the role it plays
- Explain the core duties of an operating system
- Identify common OS types and their typical use cases
- Practice interacting with an OS to gather system information

### OS Categories & Examples

**Mobile**
- **Android** — most widely used mobile OS; runs on phones, tablets, smart devices (versions: Android 14–16, manufacturer variants)
- **iOS** — Apple's mobile OS for iPhones/iPads (versions: iOS 17, 18, 26)

**Embedded & IoT Devices**
- **Embedded Linux** — specialized OS for dedicated-function devices (e.g., OpenWrt, Ubuntu Core, Yocto Project)
- **Real-Time OS (RTOS)** — guarantees response times for critical applications like aircraft controls (e.g., FreeRTOS, VxWorks, QNX)

**Virtual & Cloud**
- **Cloud/VM** — full OS images for data centers hosting websites, apps, and streaming services (e.g., Ubuntu LTS, Amazon Linux, Rocky Linux)
- **Container-optimized** — lightweight OS alternatives that package just the app and its dependencies (e.g., Alpine Linux, Bottlerocket AWS, Flatcar Linux)

**Desktop-class families** (referenced via logos): Windows, Linux, macOS, Android

### Why So Many Operating Systems?
Different devices and environments demand different capabilities:
- **Laptops** need to be user-friendly and support multitasking
- **Servers** require stability, security, and continuous uptime
- **Mobile devices** need power efficiency and tight hardware integration for battery life
- **Embedded systems** need lightweight, purpose-built OSes

The organizations building these OSes also prioritize different goals — ease of use, performance, security, openness, or customization — which is why no single OS fits every situation; instead, an ecosystem of operating systems has evolved.

### Hands-On Practice
The exercise walks through investigating a real machine (a Ubuntu Linux system) to determine:
- The Linux distribution, version, and release
- Further system details via built-in tools (e.g., "About This Computer," Home directory exploration)

## Suggested Next Steps
- Continue into deeper OS internals: how OS components manage hardware, processes, memory, and security
- Apply cloud terminology (EC2, IaaS/PaaS/SaaS) when setting up isolated lab environments for security testing
