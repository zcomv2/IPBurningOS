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

## 🔹 10. Deployment
- **Custom ISO build:** `live-build`  
  ```bash
  lb config --distribution trixie --archive-areas "main contrib non-free-firmware"
