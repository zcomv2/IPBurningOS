# Debian Stable Router / Firewall / NetServices OS  
**Based on Debian 13 Trixie (Stable)**  

## 📌 Overview
This project aims to build a **Router / Firewall / Network Services Operating System** on top of **Debian Stable Trixie 13**, designed for stability, security, and extensibility.  
It provides a complete **network workstation** to manage routing, firewalling, VPN, proxy, IDS/IPS, monitoring, and administration tasks in a single reliable system.  

---

## 🔹 1. Core System
- **Base:** Debian 13 Trixie (Stable branch)  
- **Kernel:** `linux-image-amd64` with networking modules enabled (`NETFILTER`, `IP_SET`, `NFTABLES`)  
- **Toolchain:** `build-essential`, `linux-headers`, `dkms` for driver/module compilation  
- **Default users:**  
  - `root` (direct admin, no sudo)  
  - `netadmin` (restricted SSH access)  

---

## 🔹 2. Networking Services
- **DHCP Server:** `isc-dhcp-server` or `kea-dhcp4-server`  
- **DNS Resolver:** `unbound` (caching) + `dnsmasq` (forwarding, blacklists)  
- **Time Sync:** `chrony` or `ntpd`  
- **VPN Servers:**  
  - WireGuard (native kernel support ≥ 5.6)  
  - OpenVPN (for legacy compatibility)  

---

## 🔹 3. Firewall & Filtering
- **Main firewall:** `nftables` (successor to iptables)  
- **Proxy:** `squid` (transparent) with `squidGuard` or `ufdbGuard` URL filtering  
- **IDS/IPS:**  
  - `suricata` (DPI, inline IPS mode)  
  - `snort3` (rule-based detection)  
  - `clamav-daemon` (malware scanning for HTTP/SMTP flows)  

---

## 🔹 4. Security Enhancements
- **Port Knocking:** `knockd` for hidden SSH access  
- **Fail2ban:** block brute-force attempts dynamically  
- **AppArmor** (mandatory profiles)  
- **SELinux** (optional stricter security)  
- **Auditd:** syscall logging for forensic traceability  

---

## 🔹 5. Management Tools
- **WebUI:** `lighttpd` + `php-fpm`, modular design similar to OpenWRT/LuCI  
- **SSH:** `openssh-server` with key-only login, per-user restrictions  
- **Logs:** `rsyslog` + `logrotate`  
- **Monitoring:**  
  - `collectd` + `influxdb` + `grafana` for dashboards  
  - `iftop`, `nload`, `htop` for live CLI usage  

---

## 🔹 6. Network Interfaces
- **Firmware support:** `firmware-linux-nonfree` for Intel, Broadcom, Realtek NICs  
- **Management:** `ifupdown2` for modern interface management  
- **Features:** VLANs (`vlan`), bonding (`ifenslave`), bridging (`bridge-utils`)  
- **Routing protocols:** `bird2` or `quagga` for OSPF/BGP deployments  

---

## 🔹 7. Access Control
- **Captive Portal:** `nginx` + `nodogsplash` or `coova-chilli`  
- **Welcome page:** HTML5 portal with redirection logic  
- **QoS / Traffic Shaping:** `tc` with `htb`, `cake` or `fq_codel`  

---

## 🔹 8. Audit & Pentesting Tools (Admin only)
- `nmap`, `netdiscover`, `tcpdump`, `wireshark-cli`  
- `openssl`, `gnupg`  
- `iptraf-ng`, `ngrep`  

---

## 🔹 9. Hardening
- **Sysctl tuning:**  
  - `net.ipv4.ip_forward=1`  
  - `net.ipv4.conf.all.rp_filter=1`  
  - `net.ipv4.icmp_echo_ignore_broadcasts=1`  
- **Block unused ports** via `nftables`  
- **Filesystem security:** `nodev`, `nosuid`, `noexec` on `/tmp`, `/var/tmp`  

---
# IPBurningOS — Aracne-Net-Bug-27 🕸️

**Codename:** `Aracne-Net-Bug-27` 🕸️

## Overview
Aracne-Net-Bug-27 extends IPBurningOS with an AI-assisted network analysis CLI and an isolated Red-Team container environment for controlled offensive testing. Designed for audit, training and defensive improvement — only in lab/authorized contexts.

---

## 1) AI CLI for Network Analysis
**Purpose**  
A lightweight CLI that leverages a local or internal model (e.g. Ollama local or a secure endpoint) to analyze network data and produce actionable intelligence.

**Features**
- Accepts logs, PCAPs, syslog streams and structured events.
- Detects anomalies (scans, spikes, repeated failures) and outputs:
  - Human summary
  - Structured JSON report (events, severity, recommended actions)
- CLI examples:  
  - `analyze-net --input dhcp.log --format json` → `report.json`  
  - `analyze-net --pcap capture.pcap --summary`
- Runs offline with local models (privacy-first) or against internal API.

**Integration**
- Packaged as a Docker service (model + CLI wrapper).
- Minimal internal HTTP API for other stack components.
- Output written to `/var/log/ipburningos/analysis/` and an index for auditing.

---

## 2) Red-Team Containers (lab only)
**Included examples (for isolated labs)**  
- `red/nmap` — discovery & mapping.  
- `red/enum` — enumeration and fuzzing (web, SMB, RDP).  
- `red/exploit` — controlled exploit frameworks for test targets.  
- `red/post` — post-exploitation toolset for artifact collection.  
- `red/report` — aggregator that normalizes results to CSV/JSON/Markdown.

**Security notes**
- Only deploy in isolated lab networks with explicit written permission.  
- Use VLAN segmentation, strict egress rules and logging.  
- Preserve evidence; all actions must be auditable.

---

## 3) Hardware (no prices)
Base platform recommended — expandable to GPU and extra NICs.

- **Tower: Fujitsu Esprimo P757 (MT)**  
  MT tower with PCIe slots, M.2 NVMe support and 4x DIMM DDR4 (up to 64 GB). Good base for a lab server.

- **GPU: MSI GeForce RTX 3050 8GB**  
  Low–mid power GPU for CUDA acceleration and light model inference.

- **NICs: PCIe add-on (Intel recommended)**  
  Extra physical interfaces (e.g. Intel i350 family) for 3+ RJ45 ports and stable Linux drivers.

- **Storage: NVMe M.2 or SATA SSD**  
  NVMe preferred for system and model storage; plan for 1–2 TB.

- **Memory: DDR4 UDIMM up to 64 GB**  
  Start 16–32 GB; increase to 64 GB for heavier containerization / VMs.

- **PSU**  
  Use a PSU with PCIe 8-pin connector(s) if installing a discrete GPU (recommend 550–650 W for future upgrades).

---

## 4) Best practices
- Run **only** in lab/authorized environments.  
- Keep Red-Team traffic isolated (separate VLANs, firewall egress rules).  
- Log and archive all test activity; store immutable copies for forensics.  
- Default to offline/local AI models to avoid exfiltration of sensitive logs.  
- Harden the host (minimal services, up-to-date packages, strict SSH policies).

---

## 5) Contribute / Deploy
- Add a `docker-compose.yml` with services: `analyzer`, `red-*`, `storage`.  
- Provide `examples/` with:
  - `collect-logs.sh` (ingest samples),
  - `playbooks/response/*.yml` (example remediation playbooks),
  - `reports/template.md` (standardized output).
- Include an `ETHICS.md` that documents permitted use, consent, and legal limits.

---

## Legal & Ethics
Tooling for offensive testing is included **only** for authorized audits, training and research in isolated environments. Misuse may be illegal. Operators are responsible for compliance with local law and for obtaining explicit permission from owners of tested systems.

---

**Tag:** `Aracne-Net-Bug-27` 🕸️ — AI + Red-Team lab for responsible security testing.


## 🔹 10. Deployment
- **Custom ISO build:** `live-build`  
  ```bash
  lb config --distribution trixie --archive-areas "main contrib non-free-firmware" --interactive shell

  ## 🕸️ Special Thanks

A heartfelt thanks to the open-source teams behind **Zeroshell NetServices** —  
my very first Linux network distribution. Although it no longer exists,  
its spirit continues to inspire.  

And to **IPFire**, the foundation and inspiration behind **🔥 IPBurningOS** —  
your dedication to open-source network security made this project possible.  

**Many thanks to all the teams who make this possible.**  

**by Zcom**

<p align="center">
  <img src="https://psy-k.org/images/IPBurnigOS.png" alt="IPBurningOS Logo" width="400"/>
</p>
<p align="center">
  <img src="https://www.korman.es/Aracne-ANB-27.png" alt="IPBurningOS Logo" width="400"/>
</p>

