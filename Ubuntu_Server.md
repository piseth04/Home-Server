# 🖥️ Home Server Setup (Old Laptop)

A personal project to repurpose an old laptop as a home Ubuntu server with static IP and remote SSH access.

> **Note:** Ubuntu Server was installed **offline (no internet connection)**. Network was connected afterwards via a **WiFi card** (not ethernet).

---

## ✅ Progress Tracker

- [x] Install Ubuntu Server on old laptop (offline)
- [x] Configure laptop to stay on with lid closed
- [x] Set static IP using Netplan (WiFi)
- [x] Enable SSH access

---

## 📋 What Was Done

### 1. Install Ubuntu Server
- Flashed Ubuntu Server ISO onto a USB drive and installed it on the old laptop.
- Installation was done **without an internet connection** — all base setup was completed offline.
- Network was configured afterwards using a **WiFi card** (not the ethernet port).

### 2. Lid Close Behavior
- Configured the server to ignore lid close events so it keeps running when the laptop is shut.
- Edited `/etc/systemd/logind.conf`:
  ```
  HandleLidSwitch=ignore
  HandleLidSwitchExternalPower=ignore
  HandleLidSwitchDocked=ignore
  ```
- Restarted the service:
  ```bash
  sudo systemctl restart systemd-logind
  ```

### 3. Static IP with Netplan (WiFi)
- Configured a static IP over WiFi so the server is always reachable at the same address on the local network.
- Edited `/etc/netplan/00-installer-config.yaml`:
  ```yaml
  network:
    version: 2
    wifis:
      wlo1:
        dhcp4: false
        addresses:
          - 192.168.x.25/24
        routes:
          - to: default
            via: 192.168.x.1
        nameservers:
          addresses: [8.8.8.8, 1.1.1.1]
        access-points:
          "YourWifi":
            password: "YourWifiPassword"
  ```
- Applied config:
  ```bash
  sudo netplan apply
  ```

> **Note:** Replace `wlo1` with your actual WiFi interface name (check with `ip a`).  
> Replace `192.168.x.25`, `192.168.x.1`, `YourWifi`, and `YourWifiPassword` with your real values.

### 4. Enable SSH
- Installed and enabled OpenSSH so the server can be accessed remotely from another machine on the network.
  ```bash
  sudo apt update
  sudo apt install openssh-server
  sudo systemctl enable ssh
  sudo systemctl start ssh
  ```
- Connect from another machine:
  ```bash
  ssh username@192.168.x.25
  ```

---

## 🛠️ Tech Stack

| Tool | Purpose |
|------|---------|
| Ubuntu Server | OS |
| Netplan | Network / WiFi configuration |
| OpenSSH | Remote access |
| systemd | Service & lid management |

---

## 🔜 Next Steps

- TBD