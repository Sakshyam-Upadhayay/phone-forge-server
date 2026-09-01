<div align="center">

# 📱➡️🖥️ PhoneForge

### Turn Recycled Phones Into Servers

[![Status](https://img.shields.io/badge/Status-Trial%20Phase-orange?style=for-the-badge)]()
[![Budget](https://img.shields.io/badge/Budget-NPR%203%2C00%2C000-blue?style=for-the-badge)]()
[![Phones](https://img.shields.io/badge/Target-50%2B%20Phones-green?style=for-the-badge)]()
[![License](https://img.shields.io/badge/License-MIT-yellow?style=for-the-badge)]()

**A distributed server cluster built from recycled Android phones and salvaged laptop motherboards.**  
*Sustainable computing • Data sovereignty • E-waste reduction*

[Full Report](./PhoneForge_Feasibility_Report.md) · [Architecture](#architecture) · [Getting Started](#getting-started) · [Budget](#budget) · [Roadmap](#roadmap)

</div>

---

## 🧐 What is PhoneForge?

PhoneForge repurposes **discarded Android phones** as lightweight web server nodes, orchestrated by a central master server (Gaming PC) and supported by **salvaged laptop motherboard servers** for heavy compute tasks like databases and caching.

Phones that can't be repurposed are sent to a partner organization for **precious metal (gold) extraction** — ensuring zero waste.

```
┌──────────────────────────────┐
│     RECYCLED PHONE INTAKE    │
└──────────┬───────────┬───────┘
           │           │
     ┌─────▼─────┐ ┌───▼───────┐
     │  USABLE   │ │ NON-USABLE│
     │  Root +   │ │ Dead/     │
     │  Flash    │ │ Broken    │
     └─────┬─────┘ └───┬───────┘
           │           │
     ┌─────▼─────┐ ┌───▼───────┐
     │  SERVER   │ │   GOLD    │
     │  NODE     │ │ EXTRACTION│
     └───────────┘ └───────────┘
```

## ✨ Key Features

| Feature | Description |
|---------|-------------|
| 🔄 **Distributed Cluster** | 50+ phone nodes serving web traffic via Nginx reverse proxy |
| 📱 **Custom Slave App** | Android app with heartbeat, health monitoring, auto-restart & remote management |
| 💻 **Laptop MB Servers** | 5 salvaged motherboards running MySQL, Redis, MinIO, CI/CD & monitoring |
| ⚡ **Custom Power** | Industrial USB hubs / custom PSU powering all nodes (batteries removed for safety) |
| 🔌 **USB/ADB Control** | Master communicates with all phones via ADB over USB — no WiFi dependency |
| 📊 **Monitoring Dashboard** | Grafana + Prometheus for real-time node health, CPU, RAM, temp & request metrics |
| 🚀 **Rolling Deploys** | Push code updates to all phone nodes from Git with zero-downtime rollouts |
| 🥇 **Gold Recovery** | Non-usable phones yield ~0.03g gold each via partner e-waste processing |

## 🏗️ Architecture

```
                    INTERNET (100+ Mbps Fiber)
                            │
                    ┌───────▼────────┐
                    │  Router/Switch  │
                    └───┬────────┬───┘
                        │        │
              ┌─────────▼──┐  ┌──▼─────────────┐
              │  GAMING PC  │  │ LAPTOP SERVERS  │
              │  (Master)   │  │ (5x Heavy Compute)│
              │             │  │                 │
              │ • Nginx     │  │ #1 MySQL/PgSQL  │
              │ • Node.js   │  │ #2 Redis Cache  │
              │ • ADB Host  │  │ #3 MinIO/NFS    │
              │ • Dashboard │  │ #4 Gitea + CI   │
              └──────┬──────┘  │ #5 Prometheus   │
                     │         └─────────────────┘
          ┌──────────▼──────────┐
          │   USB HUBS (3x20)   │
          └─┬──┬──┬──┬──┬──┬──┬┘
            │  │  │  │  │  │  │
           📱 📱 📱 📱 📱 📱 📱  ← 50+ Phone Nodes
           (LineageOS + Termux + Nginx + PHP/Laravel)
```

### Request Flow

```
User → Internet → Router → Nginx (Master) → Load Balancer
                                                  │
                                    ┌─────────────┼─────────────┐
                                    │             │             │
                                 Phone-01      Phone-02     Phone-50
                                    │             │             │
                                    └──────── Database (Laptop MB #1)
                                              Cache (Laptop MB #2)
                                                  │
                                              Response → User
```

## 🛠️ Tech Stack

| Layer | Technology |
|-------|-----------|
| **Phone OS** | LineageOS (custom ROM, debloated) |
| **Phone Environment** | Termux + Termux:Boot |
| **Phone Web Server** | Nginx + PHP 8.1 + Laravel |
| **Slave App** | Android (Kotlin) — foreground service |
| **Master OS** | Ubuntu Server 22.04 |
| **Orchestrator** | Node.js + ADB tools |
| **Reverse Proxy** | Nginx (least-connection load balancing) |
| **Database** | MySQL 8 / PostgreSQL 15 |
| **Cache** | Redis 7 |
| **Object Storage** | MinIO (S3-compatible) |
| **CI/CD** | Gitea + Drone CI |
| **Monitoring** | Prometheus + Grafana |
| **Client App** | Flutter (iOS + Android) |

## 📱 Slave Node App

Every rooted phone runs the **PhoneForge Slave App** — a Kotlin Android application that manages the phone's lifecycle as a server node.

### Features
- 💓 **Heartbeat** — Reports status to master every 10 seconds
- 📊 **Health Monitor** — CPU %, RAM, disk, temperature
- 🔄 **Auto-restart** — Detects & restarts crashed Nginx/PHP services
- 🚀 **Remote Deploy** — Master pushes new code via `adb push`
- 🔌 **Auto-provision** — Plug in a new phone → auto-detected → joins cluster
- 🚫 **Quarantine** — 3+ failed health checks → auto-removed from load balancer
- 📝 **Log Streaming** — Sends Nginx/PHP logs to master
- ⬆️ **OTA Updates** — Receive app updates from master

### API Endpoints (per phone)

```
GET  /status     → { node_id, cpu, ram_free_mb, temp_c, uptime_hrs, ... }
POST /deploy     → Pull latest code, restart services
POST /restart    → Restart nginx | php | all
POST /reboot     → Reboot the phone
GET  /logs       → Tail service logs
GET  /metrics    → Prometheus-compatible metrics
```

## 🚀 Getting Started

### Prerequisites

- A Gaming PC (or any desktop) running **Ubuntu** as the master node
- 5+ recycled Android phones (Xiaomi Redmi recommended)
- USB data cables + powered USB hub
- Basic Linux command-line knowledge

### Phase 1: First Phone Node (Day 1–3)

```bash
# 1. Install ADB on master PC
sudo apt install android-tools-adb

# 2. Connect a rooted phone via USB
adb devices -l

# 3. Install Termux on the phone (from F-Droid, NOT Play Store)
adb install termux.apk

# 4. Set up web server inside Termux
adb -s <serial> shell
# Inside Termux:
pkg update && pkg upgrade -y
pkg install -y php nginx openssh git
nginx   # Start web server

# 5. Forward phone's web server to master
adb -s <serial> forward tcp:9001 tcp:8080

# 6. Test it!
curl http://localhost:9001
```

### Phase 2: Scale to Cluster

```bash
# Repeat for each phone, incrementing the port:
adb -s <serial_2> forward tcp:9002 tcp:8080
adb -s <serial_3> forward tcp:9003 tcp:8080
# ...

# Configure Nginx on master as reverse proxy:
# upstream phone_workers {
#     least_conn;
#     server 127.0.0.1:9001;
#     server 127.0.0.1:9002;
#     server 127.0.0.1:9003;
# }
```

## 💰 Budget

**Trial Budget: NPR 3,00,000 (~USD 2,250)**

| Category | Amount (NPR) | % |
|----------|-------------|---|
| 📱 55 Recycled Phones + Cables | 91,750 | 30.6% |
| 💻 5 Laptop Motherboards + SSDs | 51,500 | 17.2% |
| ⚡ Power (USB hubs, UPS, wiring) | 30,500 | 10.2% |
| 🌐 Networking (router, switch, AP) | 23,500 | 7.8% |
| 🌐 Internet (100 Mbps fiber, 1 yr) | 16,000 | 5.3% |
| 🏗️ Rack + Cooling + Tools | 17,500 | 5.8% |
| 💻 Domain + Software (open source) | 1,500 | 0.5% |
| 📦 **Contingency (22.6%)** | **67,750** | **22.6%** |

**Monthly Operating Cost: ~NPR 9,925 (~USD 75)**

## 📅 Roadmap

```
Week 1–2   ████░░░░░░░░░░░░░░  Foundation (PC setup, networking, first 10 phones)
Week 3–4   ████████░░░░░░░░░░  PoC (5 phones serving HTTP, first Laravel app)
Week 5–7   ████████████░░░░░░  Scale (25 phones, laptop MB servers, monitoring)
Week 8–10  ████████████████░░  Full Trial (50 phones, slave app v1, orchestrator v1)
Week 11–14 ██████████████████  Commercial Prep (Flutter client app, beta clients)
```

| Milestone | Target |
|-----------|--------|
| 🎯 First HTTP from recycled phone | Week 4 |
| 🎯 25 phones serving a website | Week 7 |
| 🎯 50 phones + 5 servers operational | Week 10 |
| 🎯 Beta clients onboarded | Week 14 |

## 📊 Expected Performance

| Metric | Target |
|--------|--------|
| Active phone nodes | 40–45 (out of 55 purchased) |
| Cluster compute power | ≈ 1–2 conventional servers |
| Hosting capacity | 5–15 small Laravel sites |
| Requests per second | 50–200 RPS |
| Power consumption | ~650W continuous |
| Monthly electricity | NPR 5,300 (~USD 40) |

## ⚠️ Important Safety Notes

> **🔋 ALWAYS REMOVE BATTERIES** — Running 50+ old phones 24/7 with degraded lithium batteries is a fire hazard. Wire direct 4.2V power from the PSU instead.

> **🧪 NEVER DIY GOLD EXTRACTION** — The process uses toxic chemicals (hydrochloric acid, nitric acid). Always use a licensed e-waste processing partner.

## 🌿 Environmental Impact

- ♻️ **50 phones rescued** from landfills per trial cycle
- 🌍 **~80% reduction** in embodied carbon vs. new server hardware
- 🥇 **~0.03g gold recovered** per non-usable phone (via partner)
- ⚡ **650W** total power — less than a single gaming PC under load

## 📂 Project Structure

```
phoneforge/
├── README.md                          # This file
├── PhoneForge_Feasibility_Report.md   # Full technical report
├── slave-app/                         # Android Kotlin slave app
│   ├── app/src/main/
│   └── build.gradle
├── master-orchestrator/               # Node.js orchestrator
│   ├── src/
│   │   ├── adb-manager.js
│   │   ├── load-balancer.js
│   │   └── dashboard.js
│   └── package.json
├── client-app/                        # Flutter client app
│   └── lib/
├── configs/
│   ├── nginx/                         # Nginx configs
│   ├── termux/                        # Termux setup scripts
│   └── monitoring/                    # Prometheus + Grafana
├── scripts/
│   ├── setup-phone.sh                 # Automated phone provisioning
│   ├── setup-master.sh                # Master PC setup
│   └── deploy.sh                      # Code deployment to cluster
└── docs/
    ├── phone-preparation.md
    ├── laptop-mb-build.md
    └── troubleshooting.md
```

## 🤝 Contributing

This is currently a trial-phase project based in **Kathmandu, Nepal**. If you're interested in:

- 🔧 Contributing code (slave app, orchestrator, client app)
- 📱 Donating old phones for the cluster
- 🧪 Partnering on e-waste processing / gold extraction
- 📝 Improving documentation

Feel free to open an issue or reach out!

## 📄 License

This project is licensed under the MIT License — see the [LICENSE](LICENSE) file for details.

## 📚 References & Inspiration

- [UCSD Phone Cluster Computing](https://cseweb.ucsd.edu/) — Academic research proving phone clusters match conventional servers
- [Google Sustainable Computing Research](https://research.google/) — Reducing embodied carbon through device reuse
- [LineageOS](https://lineageos.org/) — Open-source Android distribution
- [Termux](https://termux.dev/) — Linux environment for Android
- [Magisk](https://github.com/topjohnwu/Magisk) — Universal root solution

---

<div align="center">

**Built with ♻️ in Kathmandu, Nepal**

*Every phone rescued from a landfill and turned into a server node is a small act of defiance against planned obsolescence.*

</div>
