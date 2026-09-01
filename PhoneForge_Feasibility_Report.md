# 📱➡️🖥️ Project PhoneForge: Recycled Phone & Laptop Server Cluster

## Technical Feasibility Report — Trial Phase

| Field | Detail |
|---|---|
| **Project Name** | PhoneForge |
| **Location** | Kathmandu Valley, Nepal |
| **Trial Budget** | NPR 3,00,000 (~USD 2,250) |
| **Target Scale (Trial)** | 50+ phones, 5+ laptop motherboards |
| **Primary Use Case** | Web hosting & App hosting (PHP/Laravel) |
| **Date** | August 31, 2026 |
| **Report Type** | Technical Feasibility Report |

---

## Table of Contents

1. [Executive Summary](#1-executive-summary)
2. [Project Vision & Concept](#2-project-vision--concept)
3. [System Architecture](#3-system-architecture)
4. [Hardware Requirements & Sourcing](#4-hardware-requirements--sourcing)
5. [Phone Node Preparation Pipeline](#5-phone-node-preparation-pipeline)
6. [Laptop Motherboard Server Build](#6-laptop-motherboard-server-build)
7. [Power Infrastructure](#7-power-infrastructure)
8. [Networking Infrastructure](#8-networking-infrastructure)
9. [Software Stack & OS Configuration](#9-software-stack--os-configuration)
10. [Slave Node App — Architecture & Features](#10-slave-node-app--architecture--features)
11. [Master Node (Gaming PC) Configuration](#11-master-node-gaming-pc-configuration)
12. [Client-Facing App (React Native/Flutter)](#12-client-facing-app-react-nativeflutter)
13. [Gold Extraction — Non-Usable Phones](#13-gold-extraction--non-usable-phones)
14. [Budget Breakdown (3 Lakh NPR)](#14-budget-breakdown-3-lakh-npr)
15. [Operational Cost Analysis](#15-operational-cost-analysis)
16. [Risk Assessment & Mitigation](#16-risk-assessment--mitigation)
17. [Implementation Roadmap](#17-implementation-roadmap)
18. [Scaling Strategy (100+ Phones)](#18-scaling-strategy-100-phones)
19. [Legal & Regulatory Considerations (Nepal)](#19-legal--regulatory-considerations-nepal)
20. [Conclusion & Recommendations](#20-conclusion--recommendations)
21. [Appendices](#21-appendices)

---

## 1. Executive Summary

**PhoneForge** is a proof-of-concept project to build a distributed server cluster from recycled Android phones and salvaged laptop motherboards, located in Kathmandu Valley, Nepal. The cluster will serve as a web and app hosting platform powered by a custom slave-node Android app communicating with a central Gaming PC master node over USB/ADB.

### Key Findings

| Metric | Assessment |
|---|---|
| **Technical Feasibility** | ✅ Proven viable — UCSD + Google research shows 25–50 phones ≈ 1 conventional server |
| **Budget Feasibility** | ✅ Achievable within 3 lakh NPR for trial of 50 phones + 5 laptop boards |
| **Commercial Viability** | ⚠️ Viable for lightweight hosting; not competitive with cloud for heavy workloads |
| **Environmental Impact** | ✅ High — extends device lifespan, reduces embodied carbon by ~80% |
| **Gold Extraction (non-usable)** | ✅ Delegated to partner organization; ~0.03g gold per phone |
| **Risk Level** | 🟡 Medium — battery safety, hardware heterogeneity, and skill ramp-up are key risks |

> [!IMPORTANT]
> This is a **trial phase report**. The goal is to validate the concept with 50+ phones before scaling to 100+ devices as a commercial service.

---

## 2. Project Vision & Concept

### 2.1 The Problem

Every year, millions of smartphones are discarded in Nepal and South Asia. These devices contain powerful ARM processors (Cortex-A53/A73), 1–2 GB RAM, WiFi/Bluetooth radios, and flash storage — all going to waste. Meanwhile, traditional server hosting in Nepal is expensive and often relies on international cloud providers.

### 2.2 The Solution

PhoneForge creates a **three-tier value extraction pipeline** from recycled phones:

```
┌──────────────────────────────────────────────────────┐
│              RECYCLED PHONE INTAKE                    │
│         (Sourced from repair shops)                   │
└──────────────┬───────────────────────┬───────────────┘
               │                       │
        ┌──────▼──────┐         ┌──────▼──────┐
        │  USABLE     │         │ NON-USABLE  │
        │  Can boot   │         │ Dead/broken │
        │  Can root   │         │ No consent  │
        └──────┬──────┘         └──────┬──────┘
               │                       │
        ┌──────▼──────┐         ┌──────▼──────┐
        │ SERVER NODE │         │    GOLD     │
        │ Install     │         │ EXTRACTION  │
        │ Slave App   │         │ (Partner    │
        │ Join cluster│         │  Org)       │
        └─────────────┘         └─────────────┘
```

### 2.3 Value Proposition

| Value | Description |
|---|---|
| **Cost** | 10–30x cheaper than equivalent cloud hosting for light workloads |
| **Environmental** | Prevents e-waste, extends device lifespan by 3–5 years |
| **Sovereignty** | Data stays in Nepal — no dependency on AWS/GCP/Azure |
| **Education** | Team learns distributed systems, networking, Linux, app development |
| **Revenue** | Can offer budget hosting to Nepali startups and developers |
| **Gold Revenue** | Non-usable phones generate supplementary income via precious metal recovery |

---

## 3. System Architecture

### 3.1 High-Level Architecture

```
                    ┌─────────────────────────────────┐
                    │        INTERNET (Fiber)          │
                    │     100+ Mbps Fiber Optic        │
                    └───────────────┬─────────────────┘
                                    │
                    ┌───────────────▼─────────────────┐
                    │     ENTERPRISE ROUTER/SWITCH      │
                    │   (Managed switch + Firewall)     │
                    └───────┬───────────────┬──────────┘
                            │               │
              ┌─────────────▼──┐     ┌──────▼─────────────┐
              │  GAMING PC     │     │  LAPTOP MB SERVERS  │
              │  (MASTER NODE) │     │  (5x Heavy Compute) │
              │                │     │                     │
              │  • Orchestrator│     │  • Database Server  │
              │  • Load Balancr│     │  • File Storage     │
              │  • Dashboard   │     │  • Backup/Redund.   │
              │  • ADB Host    │     │  • Build Server     │
              │  • Reverse Prxy│     │  • Cache Server     │
              └───────┬────────┘     └─────────────────────┘
                      │
          ┌───────────▼────────────┐
          │   POWERED USB HUBS    │
          │   (Industrial Grade)   │
          └───┬───┬───┬───┬───┬───┘
              │   │   │   │   │
           ┌──▼┐┌─▼┐┌─▼┐┌─▼┐┌─▼──┐
           │P1 ││P2││P3││P4││P50+ │  ← Recycled Phone
           │   ││  ││  ││  ││     │     Slave Nodes
           └───┘└──┘└──┘└──┘└─────┘
```

### 3.2 Component Roles

| Component | Role | Communication |
|---|---|---|
| **Gaming PC (Master)** | Central orchestrator, reverse proxy (Nginx), load balancer, ADB host, monitoring dashboard | Ethernet to router, USB to phone hubs |
| **Laptop MB Servers (5x)** | Heavy compute: MySQL/PostgreSQL database, Redis cache, file storage (NAS), build/deployment server, backup | Ethernet to router |
| **Phone Slave Nodes (50+)** | Lightweight PHP/Laravel workers serving HTTP requests via Termux/Linux | USB to Gaming PC via ADB |
| **Router/Switch** | Traffic routing, DHCP, firewall, port forwarding | Fiber uplink + Ethernet LAN |

### 3.3 Request Flow

```
User Request → Internet → Router → Nginx (Gaming PC)
                                        │
                                   Load Balancer
                                   ┌────┼────┐
                                   │    │    │
                                Phone1 Phone2 Phone3 ...
                                   │    │    │
                                   └────┼────┘
                                        │
                              Database (Laptop MB #1)
                              Cache    (Laptop MB #2)
                                        │
                                   Response → User
```

---

## 4. Hardware Requirements & Sourcing

### 4.1 Phone Specifications (Minimum Viable)

| Spec | Minimum | Recommended |
|---|---|---|
| **Android Version** | 5.0 (Lollipop) | 7.0+ (Nougat) |
| **RAM** | 1 GB | 2 GB |
| **Storage** | 8 GB | 16 GB+ |
| **CPU** | Quad-core ARM Cortex-A53 | Octa-core |
| **USB** | Micro-USB or USB-C (data-capable) | USB-C preferred |
| **Root Access** | Must be rootable | Unlocked bootloader preferred |
| **Battery** | Can be removed (recommended) | Direct power bypass |
| **Screen** | Not needed (can be broken) | — |
| **WiFi/BT** | Not needed for USB setup | Useful as backup |

### 4.2 Sourcing Strategy — Kathmandu

| Source | Expected Price/Phone | Pros | Cons |
|---|---|---|---|
| **Phone repair shops** (Primary) | NPR 1,000–2,000 | Steady supply, tested, known issues | May have hidden defects |
| **New Road / Putalisadak markets** | NPR 800–1,500 | Bulk availability | Need to negotiate |
| **Community collection drives** | NPR 0–500 | Cheapest, good PR | Unpredictable quality |
| **E-waste dealers** | NPR 500–1,000 | Very cheap | High failure rate |

### 4.3 Recommended Phone Models (Common in Nepal)

These models are widely available in Nepal's secondhand market and have good rooting/ROM support:

| Model | Typical Price (NPR) | RAM | Root Support | Notes |
|---|---|---|---|---|
| Samsung Galaxy J2/J5/J7 (2016-2018) | 1,000–1,500 | 1.5–2 GB | ✅ Excellent | Very common in Nepal |
| Xiaomi Redmi 4A/5A/6A | 1,000–2,000 | 2 GB | ✅ Excellent | Unlocked bootloader easy |
| Xiaomi Redmi Note 4/5 | 1,500–2,500 | 3–4 GB | ✅ Excellent | Best value for compute |
| Samsung Galaxy A10/A20 | 2,000–3,000 | 2–3 GB | ✅ Good | Newer Android version |
| Huawei Y6/Y7 (2018-2019) | 1,000–1,500 | 2 GB | ⚠️ Medium | Bootloader unlock harder |
| Oppo A3s / Realme C1 | 1,000–1,500 | 2 GB | ⚠️ Medium | MediaTek — limited ROM support |

> [!TIP]
> **Prioritize Xiaomi Redmi devices** — they have the best bootloader unlock process, LineageOS support, and developer community. Aim for at least 2 GB RAM models.

### 4.4 Laptop Motherboard Sourcing

| Source | Expected Price (NPR) | What to Look For |
|---|---|---|
| Repair shops (non-repairable laptops) | 2,000–5,000 | Working CPU + RAM + Ethernet |
| Second-hand markets (Mahaboudha, New Road) | 3,000–8,000 | i3/i5 generation 4+ |
| Dead laptop donations | Free–1,000 | Test motherboard first |

**Minimum specs for laptop motherboards:**

| Spec | Minimum |
|---|---|
| **CPU** | Intel Core i3 (4th gen+) or AMD equivalent |
| **RAM** | 4 GB (upgradeable to 8 GB preferred) |
| **Storage** | Any working SATA slot or M.2 |
| **Ethernet** | Built-in RJ-45 port (or USB-to-Ethernet adapter) |
| **Power** | Original charger or compatible 19V DC adapter |

---

## 5. Phone Node Preparation Pipeline

### 5.1 Triage & Testing Flow

```
Phone Received
     │
     ▼
┌──────────────┐    NO     ┌──────────────┐
│ Powers on?   ├──────────►│ GOLD         │
│              │           │ EXTRACTION   │
└──────┬───────┘           │ (Partner)    │
       │ YES               └──────────────┘
       ▼
┌──────────────┐    NO     ┌──────────────┐
│ Charges via  ├──────────►│ GOLD         │
│ USB? Data OK?│           │ EXTRACTION   │
└──────┬───────┘           └──────────────┘
       │ YES
       ▼
┌──────────────┐    NO     ┌──────────────┐
│ Can enable   ├──────────►│ GOLD         │
│ Dev Options? │           │ EXTRACTION   │
└──────┬───────┘           └──────────────┘
       │ YES
       ▼
┌──────────────┐    NO     ┌──────────────┐
│ Can unlock   ├──────────►│ Try Termux   │
│ bootloader?  │           │ (stock ROM)  │
└──────┬───────┘           └──────────────┘
       │ YES
       ▼
┌──────────────┐
│ ROOT + FLASH │
│ LineageOS    │
│ Install Slave│
│ App → JOIN   │
│ CLUSTER      │
└──────────────┘
```

### 5.2 Step-by-Step Preparation

#### Phase 1: Hardware Preparation
1. **Remove battery** (critical for long-term safety)
2. **Remove screen** (saves power, not needed for headless operation)
3. **Remove camera modules** (not needed)
4. **Wire direct power** — Solder 4.2V regulated power to battery terminals
5. **Label each phone** with unique ID (e.g., `PF-001`, `PF-002`)

> [!WARNING]
> **BATTERY REMOVAL IS CRITICAL.** Running 50+ phones 24/7 with aging lithium batteries is a serious fire hazard. Always remove batteries and wire direct power from the custom PSU.

#### Phase 2: Software Preparation
1. **Factory reset** the phone
2. **Enable Developer Options** (Settings → About → Tap Build Number 7x)
3. **Enable USB Debugging**
4. **Unlock Bootloader** (varies by manufacturer)
   - Xiaomi: Apply via mi.com, wait 7 days, use Mi Unlock Tool
   - Samsung: Use `adb oem unlock` or Odin
5. **Flash Custom Recovery** (TWRP)
6. **Flash LineageOS** (latest compatible version)
7. **Root with Magisk**
8. **Install Termux + Termux:Boot** (auto-start on power)
9. **Install Slave Node App** (custom APK — see Section 10)
10. **Configure static USB network** via ADB

#### Phase 3: Cluster Registration
1. Connect phone via USB to master node
2. ADB detects device → registers serial number
3. Slave app sends heartbeat → master confirms
4. Phone assigned a node ID and workload bucket

---

## 6. Laptop Motherboard Server Build

### 6.1 Build Process

#### Step 1: Extraction
1. Remove motherboard from laptop chassis
2. Retain: CPU heatsink/fan, RAM modules, WiFi card, I/O board
3. Remove: Screen, keyboard, trackpad, battery, optical drive

#### Step 2: Power Solution
```
Wall Outlet (220V AC)
        │
        ▼
┌──────────────┐
│ Original     │
│ Laptop PSU   │   → 19V DC → Motherboard power jack
│ (Charger)    │
└──────────────┘
```
- Use the **original laptop charger** (safest and simplest)
- Wire a physical power button to the motherboard's power header pins
- For auto-power-on: Configure BIOS → "Power On After AC Loss" = Enabled

#### Step 3: Cooling
- Mount original CPU heatsink/fan on a custom bracket
- Add a 80mm/120mm USB or DC case fan for airflow
- Consider an open-air frame (wood/acrylic shelf) for easy maintenance

#### Step 4: Storage
- Install a **120–256 GB SSD** via SATA or M.2 slot
- Budget: NPR 2,500–5,000 per SSD

#### Step 5: Mounting
```
┌─────────────────────────────────────────┐
│           WOODEN/METAL SHELF RACK       │
│                                         │
│  ┌─────────┐ ┌─────────┐ ┌─────────┐   │
│  │ MB #1   │ │ MB #2   │ │ MB #3   │   │
│  │ DB Srvr │ │ Cache   │ │ File/NAS│   │
│  └─────────┘ └─────────┘ └─────────┘   │
│  ┌─────────┐ ┌─────────┐               │
│  │ MB #4   │ │ MB #5   │               │
│  │ Build   │ │ Backup  │               │
│  └─────────┘ └─────────┘               │
│                                         │
│  Non-conductive standoffs on each shelf │
└─────────────────────────────────────────┘
```

### 6.2 Server Role Assignment

| Server | OS | Primary Role | Software |
|---|---|---|---|
| **MB #1** | Ubuntu Server 22.04 | Database Server | MySQL 8 / PostgreSQL 15 |
| **MB #2** | Ubuntu Server 22.04 | Cache & Session Store | Redis 7, Memcached |
| **MB #3** | Ubuntu Server 22.04 | File Storage (NAS) | Samba, NFS, MinIO (S3-compatible) |
| **MB #4** | Ubuntu Server 22.04 | Build & Deployment | Git, CI/CD (Gitea + Drone CI) |
| **MB #5** | Ubuntu Server 22.04 | Backup & Monitoring | rsync, Prometheus, Grafana |

---

## 7. Power Infrastructure

### 7.1 Custom PSU Design for Phone Cluster

Since you selected **Custom PSU with USB rails**, here's the recommended design:

#### Option A: Modified ATX PSU (Recommended for Trial)

```
┌──────────────────────────────────────────────┐
│            ATX POWER SUPPLY (500W+)          │
│                                              │
│  12V Rail ──► DC-DC Buck Converter (4.2V)    │
│              ──► Distribution Board          │
│              ──► 50x USB-A female ports      │
│              ──► Each port: 4.2V @ 1A        │
│                                              │
│  5V Rail  ──► USB hubs for data (ADB)        │
│                                              │
│  Standby  ──► Always-on for master node      │
└──────────────────────────────────────────────┘
```

**Bill of Materials:**

| Component | Qty | Unit Cost (NPR) | Total (NPR) |
|---|---|---|---|
| Used ATX PSU (500W+) | 2 | 2,000 | 4,000 |
| LM2596 DC-DC Buck Converters (4.2V) | 10 | 150 | 1,500 |
| USB-A Female breakout boards | 50 | 50 | 2,500 |
| 18AWG wiring, terminals, fuses | 1 lot | 2,000 | 2,000 |
| Custom distribution PCB / Perf board | 2 | 500 | 1,000 |
| **Subtotal** | | | **11,000** |

#### Option B: Industrial USB Charging Hub

| Component | Qty | Unit Cost (NPR) | Total (NPR) |
|---|---|---|---|
| 20-port industrial USB charger (5V/2A per port) | 3 | 5,000 | 15,000 |
| Total | | | **15,000** |

> [!TIP]
> **Option A is cheaper and more customizable** but requires electronics knowledge. Option B is plug-and-play but costs more. Since the team is beginner-level, consider **Option B** for the trial phase and upgrade to Option A at scale.

### 7.2 Power Consumption Estimates

| Device | Count | Watts/Unit | Total Watts |
|---|---|---|---|
| Phone nodes (headless, no screen) | 50 | 2–3W | 100–150W |
| Laptop MB servers | 5 | 30–45W | 150–225W |
| Gaming PC (Master) | 1 | 200–350W | 200–350W |
| Router + Switch | 2 | 10–15W | 20–30W |
| USB Hubs | 3 | 5–10W | 15–30W |
| Cooling fans | 5 | 3–5W | 15–25W |
| **TOTAL** | | | **500–810W** |

### 7.3 Monthly Electricity Cost (Nepal)

```
Average consumption: ~650W continuous
Hours per day: 24
Monthly kWh: 650W × 24h × 30 days = 468 kWh

NEA Rate (high slab, 15A meter): ~NPR 10–11/kWh
Monthly electricity: 468 × 10.5 = NPR 4,914
+ 5% VAT on excess: ~NPR 220
+ Service charge: ~NPR 150

Total Monthly Electricity: ~NPR 5,300 (~USD 40)
```

> [!NOTE]
> This is remarkably cheap compared to equivalent cloud hosting. A comparable VPS setup would cost NPR 15,000–30,000/month.

---

## 8. Networking Infrastructure

### 8.1 Recommended Setup

```
FIBER (100+ Mbps)
       │
┌──────▼──────────────────────────────────┐
│  ENTERPRISE ROUTER                       │
│  TP-Link ER605 / Mikrotik hEX           │
│  • Static IP from ISP                    │
│  • Port forwarding (80, 443)             │
│  • Firewall rules                        │
│  • DDNS if no static IP                  │
└──────┬──────────────────────────────────┘
       │
┌──────▼──────────────────────────────────┐
│  MANAGED GIGABIT SWITCH                  │
│  TP-Link TL-SG108E (8-port) or similar  │
│  • VLAN: Management, Servers, Phones     │
│  • QoS for prioritizing HTTP traffic     │
└──┬───┬───┬───┬──────────────────────────┘
   │   │   │   │
  GPC MB1 MB2 MB3 ...
```

### 8.2 Shopping List

| Item | Model | Est. Price (NPR) |
|---|---|---|
| Enterprise Router | TP-Link ER605 or Mikrotik hEX | 8,000–12,000 |
| Gigabit Switch (8-port) | TP-Link TL-SG108E | 4,000–6,000 |
| Cat6 Ethernet Cables (10x) | 1m–2m lengths | 1,500 |
| WiFi AP (backup) | TP-Link EAP225 | 6,000–8,000 |
| **Total Networking** | | **19,500–27,500** |

### 8.3 Phone Networking via USB

Since you chose **ADB over USB** as the communication protocol:

```
Gaming PC (Master)
    │
    ├── USB Hub #1 (20 ports) ── Phones PF-001 to PF-020
    ├── USB Hub #2 (20 ports) ── Phones PF-021 to PF-040
    └── USB Hub #3 (15 ports) ── Phones PF-041 to PF-055
```

**ADB Network Bridge:**
Each phone gets a TCP port forwarded from the Gaming PC:

```bash
# For each phone:
adb -s <serial> reverse tcp:8080 tcp:8080   # Forward requests to phone
adb -s <serial> forward tcp:<unique_port> tcp:80   # Phone's web server
```

The master Nginx uses `upstream` blocks to distribute traffic across all `localhost:<port>` entries.

---

## 9. Software Stack & OS Configuration

### 9.1 Phone Nodes — Dual Approach

You selected both **Termux/Linux** and **LineageOS**. Here's the recommended layered approach:

#### Layer 1: Base OS
- **Flash LineageOS** (removes bloatware, improves performance by 30–50%)
- Select the latest version compatible with the phone model
- Enable auto-start, disable screen timeout

#### Layer 2: Linux Environment (Termux)
- Install **Termux** from F-Droid (NOT Play Store — outdated)
- Install **Termux:Boot** for auto-start services
- Install **Termux:API** for hardware access

#### Layer 3: Web Server Stack (Inside Termux)

```bash
# Install packages
pkg update && pkg upgrade
pkg install php nginx mariadb nodejs openssh git

# Configure Nginx
# /data/data/com.termux/files/usr/etc/nginx/nginx.conf
# Listen on port 8080 (non-root port)

# Configure PHP-FPM
# Serves Laravel applications

# Auto-start on boot (Termux:Boot)
mkdir -p ~/.termux/boot
echo '#!/data/data/com.termux/files/usr/bin/bash
nginx
php-fpm
sshd' > ~/.termux/boot/start-services.sh
chmod +x ~/.termux/boot/start-services.sh
```

### 9.2 Software Per Component

| Component | Software Stack |
|---|---|
| **Phone Nodes** | LineageOS → Termux → Nginx + PHP 8.1 + Laravel Worker |
| **Gaming PC (Master)** | Ubuntu Server 22.04 → Nginx (reverse proxy) → Node.js (Orchestrator) → ADB tools |
| **Laptop MB #1** | Ubuntu Server 22.04 → MySQL 8.0 / PostgreSQL 15 |
| **Laptop MB #2** | Ubuntu Server 22.04 → Redis 7 → Memcached |
| **Laptop MB #3** | Ubuntu Server 22.04 → MinIO (object storage) → NFS |
| **Laptop MB #4** | Ubuntu Server 22.04 → Gitea → Drone CI → PHP Deployer |
| **Laptop MB #5** | Ubuntu Server 22.04 → Prometheus → Grafana → rsync |

### 9.3 Laravel Deployment Strategy

Since you're hosting **PHP/Laravel** apps:

```
                   ┌─────────────────────────┐
                   │   Gaming PC (Master)     │
                   │   Nginx Reverse Proxy    │
                   │                          │
                   │   upstream laravel {     │
                   │     server 127.0.0.1:9001│ ← Phone PF-001
                   │     server 127.0.0.1:9002│ ← Phone PF-002
                   │     server 127.0.0.1:9003│ ← Phone PF-003
                   │     ...                  │
                   │     server 127.0.0.1:9050│ ← Phone PF-050
                   │   }                      │
                   └────────────┬─────────────┘
                                │
                   ┌────────────▼─────────────┐
                   │   Shared Resources        │
                   │   DB: Laptop MB #1        │
                   │   Cache: Laptop MB #2     │
                   │   Storage: Laptop MB #3   │
                   └──────────────────────────┘
```

Each phone runs a **stateless Laravel worker** — the database, cache (Redis), sessions, and file storage are all centralized on the laptop motherboard servers.

---

## 10. Slave Node App — Architecture & Features

### 10.1 Overview

The **PhoneForge Slave App** is a custom Android application (APK) installed on every rooted phone node. It manages the phone's lifecycle as a server node and communicates with the master orchestrator via USB/ADB.

### 10.2 Architecture

```
┌─────────────────────────────────────────────────────┐
│                 SLAVE NODE APP (Android)             │
│                                                     │
│  ┌──────────────┐  ┌──────────────┐  ┌───────────┐ │
│  │ Heartbeat    │  │ Task         │  │ Health    │ │
│  │ Service      │  │ Executor     │  │ Monitor   │ │
│  │              │  │              │  │           │ │
│  │ Sends status │  │ Receives &   │  │ CPU, RAM  │ │
│  │ every 10s    │  │ runs tasks   │  │ Temp, Disk│ │
│  │ to master    │  │ from master  │  │ Battery   │ │
│  └──────┬───────┘  └──────┬───────┘  └─────┬─────┘ │
│         │                 │                │        │
│  ┌──────▼─────────────────▼────────────────▼─────┐  │
│  │          LOCAL COMMUNICATION LAYER             │  │
│  │     USB/ADB → TCP Port Forwarding              │  │
│  │     Protocol: HTTP REST (JSON payloads)        │  │
│  └───────────────────────────────────────────────┘  │
│                                                     │
│  ┌───────────────────────────────────────────────┐  │
│  │          SERVICE MANAGEMENT LAYER              │  │
│  │  • Start/Stop Nginx + PHP-FPM (Termux)        │  │
│  │  • Deploy new code (git pull)                  │  │
│  │  • Restart services on command                 │  │
│  │  • Auto-recovery on crash                      │  │
│  └───────────────────────────────────────────────┘  │
│                                                     │
│  ┌───────────────────────────────────────────────┐  │
│  │          LOGGING & ANALYTICS                   │  │
│  │  • Request count, error count                  │  │
│  │  • Uptime tracking                             │  │
│  │  • Resource utilization history                 │  │
│  │  • Send logs to master via ADB                 │  │
│  └───────────────────────────────────────────────┘  │
└─────────────────────────────────────────────────────┘
```

### 10.3 Feature List

| Feature | Description | Priority |
|---|---|---|
| **Heartbeat** | Sends status (alive/load/temp) to master every 10 seconds | 🔴 Critical |
| **Task Distribution** | Receives workload assignments from master | 🔴 Critical |
| **Health Monitoring** | Reports CPU %, RAM %, disk %, temperature | 🔴 Critical |
| **Auto-restart** | Restarts Nginx/PHP if crashes detected | 🔴 Critical |
| **Remote Management** | Master can restart services, reboot phone, redeploy code | 🟡 High |
| **Auto-provisioning** | New phone plugged in → auto-detected, configured, joins cluster | 🟡 High |
| **Dashboard Reporting** | Sends metrics to Grafana/Prometheus via master | 🟡 High |
| **Quarantine Logic** | If node fails 3+ health checks → auto-removed from load balancer | 🟡 High |
| **Log Streaming** | Streams Nginx/PHP error logs to master | 🟢 Medium |
| **OTA Updates** | Master pushes app updates to all slaves | 🟢 Medium |

### 10.4 Communication Protocol (USB/ADB)

```
MASTER (Gaming PC)                    SLAVE (Phone)
      │                                    │
      │──── adb -s <serial> shell ────────►│
      │     "curl localhost:8888/status"    │
      │◄─── JSON Response ────────────────│
      │     {                              │
      │       "node_id": "PF-001",         │
      │       "status": "healthy",         │
      │       "cpu": 45,                   │
      │       "ram_free_mb": 412,          │
      │       "temp_c": 38,               │
      │       "uptime_hrs": 142,           │
      │       "requests_served": 89420,    │
      │       "errors": 12                 │
      │     }                              │
      │                                    │
      │──── adb forward tcp:9001 tcp:8080 ─►│
      │     (Port forwarding for web traffic) │
      │                                    │
      │──── adb push deploy.tar.gz /tmp/ ──►│
      │     (Code deployment)              │
```

### 10.5 Tech Stack for Slave App

| Layer | Technology |
|---|---|
| **App Platform** | Android (Kotlin) — Foreground Service |
| **Local Web Server** | Built-in HTTP server (Ktor/NanoHTTPD) for status API |
| **IPC with Termux** | Shell commands via `Runtime.exec()` |
| **Communication** | ADB port forwarding + REST API |
| **Persistence** | SQLite for local metrics |
| **Boot Receiver** | `BOOT_COMPLETED` broadcast to auto-start |

---

## 11. Master Node (Gaming PC) Configuration

### 11.1 Master Orchestrator Software

The Gaming PC runs a **Node.js-based orchestrator** that manages all phone nodes:

```
┌──────────────────────────────────────────────┐
│            MASTER ORCHESTRATOR               │
│                                              │
│  ┌────────────────────────────────────────┐  │
│  │  ADB Device Manager                    │  │
│  │  • Polls `adb devices` every 5s        │  │
│  │  • Detects new/disconnected phones     │  │
│  │  • Maintains device registry           │  │
│  └────────────────────────────────────────┘  │
│                                              │
│  ┌────────────────────────────────────────┐  │
│  │  Load Balancer (Nginx upstream)        │  │
│  │  • Round-robin across healthy nodes    │  │
│  │  • Auto-removes unhealthy nodes        │  │
│  │  • Weighted by phone capability        │  │
│  └────────────────────────────────────────┘  │
│                                              │
│  ┌────────────────────────────────────────┐  │
│  │  Web Dashboard (Grafana + Custom UI)   │  │
│  │  • Real-time node status               │  │
│  │  • CPU/RAM/Temp per node               │  │
│  │  • Request throughput graphs            │  │
│  │  • Alert system                         │  │
│  └────────────────────────────────────────┘  │
│                                              │
│  ┌────────────────────────────────────────┐  │
│  │  Deployment Pipeline                   │  │
│  │  • Git webhook → build → push to nodes │  │
│  │  • Rolling deploy (5 nodes at a time)  │  │
│  │  • Rollback capability                 │  │
│  └────────────────────────────────────────┘  │
└──────────────────────────────────────────────┘
```

### 11.2 Nginx Configuration (Master)

```nginx
# /etc/nginx/conf.d/phoneforge.conf

upstream phone_workers {
    least_conn;  # Route to least-loaded phone

    # Auto-generated list (updated by orchestrator)
    server 127.0.0.1:9001;  # PF-001
    server 127.0.0.1:9002;  # PF-002
    server 127.0.0.1:9003;  # PF-003
    # ... up to PF-050
    server 127.0.0.1:9050;  # PF-050
}

server {
    listen 80;
    listen 443 ssl;
    server_name yourdomain.com;

    # SSL via Let's Encrypt
    ssl_certificate /etc/letsencrypt/live/yourdomain.com/fullchain.pem;
    ssl_certificate_key /etc/letsencrypt/live/yourdomain.com/privkey.pem;

    location / {
        proxy_pass http://phone_workers;
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_connect_timeout 5s;
        proxy_read_timeout 30s;
        proxy_next_upstream error timeout;
    }

    # Static assets served directly from master
    location ~* \.(css|js|jpg|png|gif|ico|woff2)$ {
        root /var/www/static;
        expires 30d;
    }
}
```

---

## 12. Client-Facing App (React Native/Flutter)

### 12.1 Overview

A mobile app for end-users / hosting clients to manage their hosted websites/apps on the PhoneForge platform.

### 12.2 Recommended Framework

| Framework | Pros | Cons | Recommendation |
|---|---|---|---|
| **Flutter** | Single codebase iOS+Android, fast UI, Google backed | Dart learning curve | ✅ **Recommended** |
| **React Native** | JS ecosystem, huge community, hot reload | Performance gaps, bridge overhead | Good alternative |

### 12.3 App Features

| Feature | Description |
|---|---|
| **Dashboard** | View hosted sites, uptime, bandwidth usage |
| **Deploy** | Push new code / upload files |
| **Logs** | View real-time access & error logs |
| **Domain Management** | Point custom domains to PhoneForge |
| **Billing** | Simple billing system for hosting plans |
| **Notifications** | Downtime alerts, deployment status |
| **Analytics** | Traffic stats, request counts, response times |

---

## 13. Gold Extraction — Non-Usable Phones

> [!NOTE]
> As per your specification, gold extraction will be **handled by a separate partner organization**. This section provides an overview for context and partner evaluation.

### 13.1 Precious Metal Yield per Phone

| Metal | Average per Phone | Value (at current spot prices) |
|---|---|---|
| **Gold (Au)** | 0.025–0.050 g | NPR 200–400 per phone |
| **Silver (Ag)** | 0.250 g | NPR 25–30 per phone |
| **Palladium (Pd)** | 0.009 g | NPR 30–40 per phone |
| **Copper (Cu)** | 15 g | NPR 3–5 per phone |
| **Total recoverable value** | — | **~NPR 260–475 per phone** |

### 13.2 Economics at Scale

| Scenario | Phones | Estimated Gold | Total Metal Value |
|---|---|---|---|
| Trial rejects | ~20 phones | 0.6–1.0 g gold | NPR 5,200–9,500 |
| Annual (100+ intake) | ~40 rejects | 1.2–2.0 g gold | NPR 10,400–19,000 |
| At scale (500+ intake) | ~150 rejects | 4.5–7.5 g gold | NPR 39,000–71,250 |

### 13.3 Partner Evaluation Criteria

When selecting a gold extraction partner, verify:

- [ ] Proper environmental licenses (Nepal Department of Environment)
- [ ] Safe chemical handling (aqua regia, cyanide alternatives)
- [ ] Worker safety protocols (PPE, ventilation)
- [ ] Transparent yield reporting and revenue sharing
- [ ] Certification or track record in e-waste processing
- [ ] Ability to handle other components (plastic, glass, batteries)

> [!CAUTION]
> DIY gold extraction involves **highly toxic chemicals** (hydrochloric acid, nitric acid, cyanide) and produces **hazardous waste**. This must be done by licensed professionals in proper facilities. **Never attempt this at home.**

---

## 14. Budget Breakdown (3 Lakh NPR)

### 14.1 Detailed Budget

| Category | Item | Qty | Unit (NPR) | Total (NPR) |
|---|---|---|---|---|
| **📱 Phone Nodes** | | | | |
| | Recycled phones (avg NPR 1,500) | 55 | 1,500 | 82,500 |
| | USB data cables (quality) | 55 | 150 | 8,250 |
| | Phone labels + trays | 1 lot | 1,000 | 1,000 |
| | **Subtotal** | | | **91,750** |
| **💻 Laptop MBs** | | | | |
| | Laptop motherboards | 5 | 4,000 | 20,000 |
| | SSDs (120–256 GB) | 5 | 3,500 | 17,500 |
| | RAM upgrades (4→8 GB) | 3 | 2,500 | 7,500 |
| | Power adapters (19V) | 5 | 800 | 4,000 |
| | Cooling fans + heatsinks | 5 | 500 | 2,500 |
| | **Subtotal** | | | **51,500** |
| **⚡ Power** | | | | |
| | Industrial USB charger hubs (20-port) | 3 | 5,000 | 15,000 |
| | Power strips + surge protectors | 3 | 1,500 | 4,500 |
| | UPS (600VA for master + routers) | 1 | 8,000 | 8,000 |
| | Wiring + electrical work | 1 lot | 3,000 | 3,000 |
| | **Subtotal** | | | **30,500** |
| **🌐 Networking** | | | | |
| | Enterprise router (Mikrotik/TP-Link) | 1 | 10,000 | 10,000 |
| | Gigabit managed switch | 1 | 5,000 | 5,000 |
| | Cat6 cables | 10 | 150 | 1,500 |
| | WiFi AP (backup) | 1 | 7,000 | 7,000 |
| | **Subtotal** | | | **23,500** |
| **🌐 Internet** | | | | |
| | Fiber setup (100 Mbps annual) | 1 yr | 10,000 | 10,000 |
| | Static IP add-on | 1 yr | 6,000 | 6,000 |
| | **Subtotal** | | | **16,000** |
| **🏗️ Infrastructure** | | | | |
| | Wooden rack / shelving unit | 1 | 5,000 | 5,000 |
| | Ventilation fan (room) | 1 | 3,000 | 3,000 |
| | Zip ties, velcro, mounting | 1 lot | 1,000 | 1,000 |
| | **Subtotal** | | | **9,000** |
| **🔧 Tools & Misc** | | | | |
| | Soldering station | 1 | 3,000 | 3,000 |
| | Multimeter | 1 | 1,500 | 1,500 |
| | Screwdriver set (phone repair) | 1 | 1,000 | 1,000 |
| | USB-to-Ethernet adapters (for MBs without) | 2 | 1,500 | 3,000 |
| | **Subtotal** | | | **8,500** |
| **💻 Software & Domain** | | | | |
| | Domain name registration | 1 | 1,500 | 1,500 |
| | SSL Certificate (Let's Encrypt) | — | Free | 0 |
| | All software (open source) | — | Free | 0 |
| | **Subtotal** | | | **1,500** |
| **📦 Contingency** | | | | |
| | Buffer for replacements & unforeseen | — | — | 67,750 |
| | **Subtotal** | | | **67,750** |

### 14.2 Budget Summary

| Category | Amount (NPR) | % of Budget |
|---|---|---|
| Phone Nodes | 91,750 | 30.6% |
| Laptop Motherboard Servers | 51,500 | 17.2% |
| Power Infrastructure | 30,500 | 10.2% |
| Networking | 23,500 | 7.8% |
| Internet (1 year) | 16,000 | 5.3% |
| Physical Infrastructure | 9,000 | 3.0% |
| Tools & Miscellaneous | 8,500 | 2.8% |
| Software & Domain | 1,500 | 0.5% |
| **Contingency (22.6%)** | **67,750** | **22.6%** |
| **TOTAL** | **3,00,000** | **100%** |

> [!TIP]
> The **22.6% contingency** buffer is healthy for a trial project. Phone failure rates during setup can be 20–30%, so expect to need replacement units.

---

## 15. Operational Cost Analysis

### 15.1 Monthly Operating Costs

| Expense | Monthly (NPR) | Annual (NPR) |
|---|---|---|
| Electricity (~650W continuous) | 5,300 | 63,600 |
| Internet (100 Mbps fiber, monthly) | 1,000 | 12,000 |
| Replacement phones (2-3/month) | 3,000 | 36,000 |
| Replacement cables/parts | 500 | 6,000 |
| Domain renewal | 125 | 1,500 |
| **TOTAL** | **9,925** | **1,19,100** |

### 15.2 Break-Even Analysis

| Hosting Plan | Price/Month (NPR) | Clients Needed for Break-Even |
|---|---|---|
| Basic (shared, 1 site) | 500 | 20 clients |
| Standard (2 sites, more resources) | 1,000 | 10 clients |
| Premium (dedicated phone node) | 2,500 | 4 clients |

**Break-even: ~10–20 clients at NPR 500–1,000/month**

### 15.3 Comparison with Cloud Alternatives

| Provider | Equivalent Plan | Monthly Cost (NPR) |
|---|---|---|
| **PhoneForge (This Project)** | 50 lightweight workers + 5 servers | **9,925** |
| DigitalOcean (5 droplets) | 5 × $12 Basic | 8,000 |
| AWS Lightsail (5 instances) | 5 × $10 | 6,700 |
| Local Nepal Hosting (Jeewan Computers etc.) | Shared hosting × 5 | 5,000–10,000 |
| VPS Nepal (AGM Web Hosting) | 5 × standard VPS | 7,500–15,000 |

> [!IMPORTANT]
> PhoneForge is **NOT cheaper** than basic cloud hosting for pure cost. Its value lies in:
> 1. **Data sovereignty** (data stays in Nepal)
> 2. **No recurring cloud bills** after hardware investment
> 3. **Learning & IP creation** (you build expertise and own the platform)
> 4. **Environmental impact** (e-waste reduction)
> 5. **Commercial potential** at scale (100+ nodes)

---

## 16. Risk Assessment & Mitigation

| # | Risk | Severity | Likelihood | Mitigation |
|---|---|---|---|---|
| 1 | **Battery fire / thermal runaway** | 🔴 Critical | Medium | Remove ALL batteries; use direct PSU power; install smoke detectors |
| 2 | **High phone failure rate** (>30%) | 🟡 High | High | Budget 22% contingency; buy 55 phones for 50 active nodes |
| 3 | **USB hub instability** (disconnections) | 🟡 High | High | Use industrial-grade powered hubs; implement auto-reconnect in slave app |
| 4 | **Heterogeneous hardware** (different models) | 🟡 High | Certain | Standardize on 2–3 phone models; use Termux (abstracts hardware) |
| 5 | **Power outage** | 🟡 High | Medium | UPS for master + router; phones auto-rejoin cluster on power restore |
| 6 | **Internet downtime** | 🟡 High | Medium | Consider secondary ISP or mobile hotspot backup |
| 7 | **ADB authorization issues** | 🟢 Medium | High | Pre-authorize master PC's RSA key; automate recovery script |
| 8 | **Performance bottleneck** (1–2 GB RAM) | 🟢 Medium | Medium | Run stateless workers only; offload DB/cache to laptop servers |
| 9 | **Team skill gap** (beginner level) | 🟢 Medium | Certain | Start with 5 phones, document everything, scale gradually |
| 10 | **Legal/regulatory issues** | 🟢 Medium | Low | Research Nepal ICT laws; consult with lawyer if offering commercial hosting |
| 11 | **Security vulnerabilities** | 🟡 High | Medium | Firewall, SSH keys only, keep software updated, disable ADB over WiFi |
| 12 | **Storage failure (SSD on laptop MBs)** | 🟡 High | Low | Regular backups (MB #5), RAID-like replication where possible |

---

## 17. Implementation Roadmap

### Phase 0: Foundation (Week 1–2)

| Task | Duration | Details |
|---|---|---|
| Procure first 10 phones | 3 days | Test sourcing from repair shops |
| Set up Gaming PC as master | 2 days | Install Ubuntu, Nginx, Node.js, ADB tools |
| Purchase networking gear | 1 day | Router, switch, cables |
| Set up fiber internet | 1–3 days | ISP installation + static IP |

### Phase 1: Proof of Concept (Week 3–4)

| Task | Duration | Details |
|---|---|---|
| Root & flash 5 phones | 3 days | Learn the process on first batch |
| Remove batteries, wire direct power | 2 days | Build first PSU prototype |
| Install Termux + Nginx + PHP on 5 phones | 2 days | Document the exact steps |
| Deploy a test Laravel app | 1 day | "Hello World" served from phone cluster |
| Set up ADB port forwarding | 1 day | Master Nginx → 5 phone backends |
| **Milestone: First HTTP request served from a recycled phone** | | 🎉 |

### Phase 2: Scale to 25 (Week 5–7)

| Task | Duration | Details |
|---|---|---|
| Procure remaining 45 phones | 5 days | Bulk buying from multiple shops |
| Build phone triage pipeline | 2 days | Test → Root → Flash → Deploy |
| Prepare 20 more phone nodes | 5 days | Batch processing |
| Build laptop MB servers (#1–#3) | 3 days | Database, cache, file storage |
| Set up monitoring (Prometheus + Grafana) | 2 days | Dashboard for all nodes |
| **Milestone: 25 phones serving a real website** | | 🎉 |

### Phase 3: Full Trial (Week 8–10)

| Task | Duration | Details |
|---|---|---|
| Prepare remaining 25 phones | 5 days | Continue batch processing |
| Build laptop MB servers (#4–#5) | 2 days | Build + backup servers |
| Develop Slave Node App v1.0 | 7 days | Heartbeat, health, auto-restart |
| Develop Master Orchestrator v1.0 | 7 days | ADB manager, load balancer, dashboard |
| Load testing & benchmarking | 3 days | Measure throughput, latency, stability |
| **Milestone: 50 phones + 5 laptop servers operational** | | 🎉 |

### Phase 4: Client App & Commercial Prep (Week 11–14)

| Task | Duration | Details |
|---|---|---|
| Develop client-facing Flutter app | 14 days | Dashboard, deploy, logs, billing |
| Onboard first 3 beta clients (free) | 7 days | Real-world testing |
| Document all processes | 5 days | Create operations manual |
| Security hardening | 3 days | Firewall, SSL, access control |
| Gold extraction handoff (reject phones) | 1 day | Send to partner organization |
| **Milestone: Trial phase complete, ready for commercial evaluation** | | 🎉 |

---

## 18. Scaling Strategy (100+ Phones)

### 18.1 Architecture at Scale

```
                         INTERNET
                            │
                    ┌───────▼────────┐
                    │  Load Balancer  │  (HAProxy / Cloudflare)
                    └───┬────────┬───┘
                        │        │
              ┌─────────▼┐  ┌───▼──────────┐
              │ Master #1 │  │  Master #2   │  (Redundant masters)
              │ (Gaming PC)│ │  (New server)│
              └─────┬─────┘  └──────┬───────┘
                    │               │
         ┌──────────▼───┐   ┌──────▼──────────┐
         │  USB Hub Rack │   │  USB Hub Rack   │
         │  #1 (50 phn)  │   │  #2 (50 phn)   │
         └───────────────┘   └─────────────────┘

         Total: 100+ phones across 2 master nodes
```

### 18.2 Key Scaling Investments

| Item | Investment (NPR) | Notes |
|---|---|---|
| Additional 50 phones | 75,000 | Same sourcing |
| Second master server | 30,000–50,000 | Can be another gaming PC or dedicated server |
| Additional USB hubs | 15,000 | 3 more 20-port hubs |
| Rack/shelving expansion | 10,000 | More shelf space |
| Additional laptop MBs | 20,000 | 2–3 more for redundancy |
| **Total Scale-Up** | **~1,50,000–1,70,000** | |

### 18.3 USB Limitation & Solutions

> [!WARNING]
> A single PC can reliably manage **~50–60 USB devices** via hubs. Beyond that, consider:

| Solution | Phones Supported | Notes |
|---|---|---|
| Multiple master PCs | 50 per master | Most reliable |
| ADB over WiFi (hybrid) | Unlimited | Less reliable, higher latency |
| USB PCIe expansion cards | +30 per card | Requires desktop PC with PCIe slots |

---

## 19. Legal & Regulatory Considerations (Nepal)

### 19.1 Relevant Laws

| Area | Regulation | Status |
|---|---|---|
| **E-Waste** | Nepal Environment Protection Act, 2019 | ⚠️ Verify compliance for collecting phones |
| **Hosting/ISP** | Nepal Telecommunications Authority (NTA) | ⚠️ May need license for commercial hosting |
| **Data Protection** | Nepal's Privacy Act, 2018 | ✅ Comply with data handling regulations |
| **Business Registration** | Company/Firm registration | ✅ Required for commercial operations |
| **Tax** | IRD registration, PAN/VAT | ✅ Required if generating revenue |

### 19.2 Recommendations

1. **Register as a technology company** or IT firm at the Office of the Company Registrar
2. **Consult with a lawyer** regarding NTA licensing requirements for hosting services
3. **Obtain PAN** for tax compliance before accepting commercial clients
4. **Document phone sourcing** — ensure all phones are legally obtained (not stolen property)
5. **Environmental compliance** — ensure proper handling of phone batteries and e-waste

---

## 20. Conclusion & Recommendations

### 20.1 Verdict: GO ✅ (with conditions)

The PhoneForge project is **technically feasible** and financially achievable within the 3 lakh NPR trial budget. Research from UCSD and Google has proven that 25–50 smartphone motherboards can match a conventional server for lightweight workloads like web hosting.

### 20.2 Key Recommendations

| # | Recommendation | Rationale |
|---|---|---|
| 1 | **Start with 5 phones, not 50** | Learn the process before scaling; beginner team needs time to develop skills |
| 2 | **Standardize on Xiaomi Redmi phones** | Best rooting support, cheapest, most available in Nepal |
| 3 | **ALWAYS remove batteries** | Non-negotiable safety requirement for 24/7 operation |
| 4 | **Use plug-and-play USB hubs first** | Avoid custom PSU until the team has more electronics experience |
| 5 | **Deploy a real Laravel app by Week 4** | Early validation prevents wasted investment |
| 6 | **Set up monitoring before scaling** | You can't manage 50 nodes without Grafana/Prometheus |
| 7 | **Build the slave app iteratively** | Start with heartbeat + health check, add features over time |
| 8 | **Budget for 30% phone failure rate** | Old phones will die — the contingency fund covers this |
| 9 | **Get legal advice before commercial launch** | NTA licensing may be required for hosting services in Nepal |
| 10 | **Consider hybrid approach at scale** | Use laptop MB servers for heavy tasks, phones for stateless workers |

### 20.3 Expected Trial Outcomes

| Outcome | Target |
|---|---|
| Working phone nodes | 40–45 out of 55 purchased |
| Total cluster compute | Roughly equivalent to 1–2 conventional servers |
| Web hosting capacity | 5–15 small Laravel websites |
| Requests per second (est.) | 50–200 RPS across cluster |
| Monthly operating cost | NPR 9,925 |
| Time to operational | 10–14 weeks |
| Gold extracted (reject phones) | 0.3–0.5 g (NPR 3,000–5,000 via partner) |

### 20.4 Final Thought

> PhoneForge is not just a server project — it's a statement about sustainability, resourcefulness, and technological sovereignty. Every phone you rescue from a landfill and turn into a server node is a small act of defiance against planned obsolescence. The trial budget of 3 lakh NPR is modest, but the knowledge and infrastructure you build will compound as you scale to 100+ nodes and beyond.

---

## 21. Appendices

### Appendix A: Essential Commands Reference

```bash
# === ADB COMMANDS ===
adb devices -l                          # List all connected devices
adb -s <serial> shell                   # Open shell on specific phone
adb -s <serial> forward tcp:9001 tcp:8080  # Port forward
adb -s <serial> push file.tar /sdcard/  # Push files to phone
adb -s <serial> reboot                  # Reboot phone

# === TERMUX SETUP (on each phone) ===
pkg update && pkg upgrade -y
pkg install -y php nginx openssh git curl wget
termux-setup-storage                     # Access shared storage
sshd                                     # Start SSH server

# === NGINX (Termux) ===
nginx                                    # Start nginx
nginx -s reload                          # Reload config
nginx -s stop                            # Stop nginx

# === PHP/LARAVEL (Termux) ===
cd ~/laravel-app
php artisan serve --host=0.0.0.0 --port=8080

# === MONITORING ===
adb -s <serial> shell "cat /sys/class/thermal/thermal_zone0/temp"  # CPU temp
adb -s <serial> shell "free -m"          # Memory usage
adb -s <serial> shell "df -h"            # Disk usage
adb -s <serial> shell "uptime"           # Uptime
```

### Appendix B: Recommended Purchases (Nepal-Specific)

| Item | Where to Buy | Area |
|---|---|---|
| Recycled phones | Mobile repair shops | New Road, Putalisadak, Tammel |
| USB hubs (industrial) | Daraz.com.np or import from AliExpress | Online |
| Networking gear | Computer Bazar, ITTI shops | New Road, Putalisadak |
| SSDs, RAM | Computer Bazar | New Road |
| Soldering equipment | Electronics shops | Bishal Bazar, Putalisadak |
| Fiber Internet | WorldLink, Vianet, Classic Tech | Kathmandu Valley |
| Used laptops (for motherboards) | Repair shops, OLX Nepal | Various |

### Appendix C: Useful Resources

| Resource | URL | Purpose |
|---|---|---|
| LineageOS Device Wiki | wiki.lineageos.org | Check ROM compatibility |
| TWRP Device List | twrp.me/Devices | Check recovery compatibility |
| Termux Wiki | wiki.termux.com | Termux setup guides |
| Magisk (Root) | github.com/topjohnwu/Magisk | Latest Magisk releases |
| UCSD Phone Cluster Research | Paper: "Phone Cluster Computing" | Academic foundation |
| K3s (Lightweight K8s) | k3s.io | Future: container orchestration |
| GADS (Device Farm) | github.com/shamanec/GADS | Device management |

### Appendix D: PhoneForge Slave App — API Specification

```
Base URL: http://localhost:8888 (on each phone)

GET /status
  → Returns: { node_id, status, cpu, ram_free_mb, temp_c, uptime_hrs, 
               requests_served, errors, disk_free_mb, services: [] }

POST /deploy
  Body: { repo_url, branch }
  → Pulls latest code, restarts services

POST /restart
  Body: { service: "nginx" | "php" | "all" }
  → Restarts specified service

POST /reboot
  → Reboots the phone

GET /logs?service=nginx&lines=100
  → Returns last N lines of service logs

GET /metrics
  → Prometheus-compatible metrics endpoint
```

### Appendix E: Network Topology Diagram

```
INTERNET
   │
   │ Fiber 100+ Mbps
   │
┌──▼──────────────────────────────────────────────────────┐
│                    ROUTER (TP-Link ER605)                │
│  WAN: Static IP from ISP                                │
│  LAN: 192.168.1.0/24                                    │
│  DHCP: .100–.200 (dynamic)                              │
│  Static: .1 (router), .2 (master), .10–.14 (laptops)   │
│  Port Forward: 80,443 → 192.168.1.2 (master)           │
└──┬──────────────────────────────────────────────────────┘
   │
┌──▼──────────────────────────────────────────────────────┐
│               MANAGED SWITCH (8-port Gigabit)            │
│                                                          │
│  Port 1: Router uplink                                   │
│  Port 2: Gaming PC (Master) — 192.168.1.2               │
│  Port 3: Laptop MB #1 (DB) — 192.168.1.10              │
│  Port 4: Laptop MB #2 (Cache) — 192.168.1.11           │
│  Port 5: Laptop MB #3 (Storage) — 192.168.1.12         │
│  Port 6: Laptop MB #4 (Build) — 192.168.1.13           │
│  Port 7: Laptop MB #5 (Backup) — 192.168.1.14          │
│  Port 8: WiFi AP (backup/management)                     │
└──────────────────────────────────────────────────────────┘

Phone nodes: Connected via USB to Gaming PC (Master)
             Accessible as localhost:9001–9055 on master
             NOT on the network directly (USB only)
```

---

*Report prepared for PhoneForge Trial Phase — Kathmandu Valley, Nepal*  
*Budget: NPR 3,00,000 | Target: 50+ phones + 5 laptop motherboard servers*  
*Date: August 31, 2026*
