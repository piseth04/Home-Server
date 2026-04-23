# 🖥️ Home Server Setup (Old Laptop)

A personal project to repurpose an old laptop as a home Ubuntu server with static IP and remote SSH access.

---

## ✅ Progress Tracker

- [x] Install Ubuntu Server on old laptop
- [x] Configure laptop to stay on with lid closed
- [x] Set static IP using Netplan
- [x] Enable SSH access

---

## 📋 What Was Done

### 1. Install Ubuntu Server
- Flashed Ubuntu Server ISO onto a USB drive and installed it on the old laptop.

### 2. Lid Close Behavior
- Configured the server to ignore lid close events so it keeps running when the laptop is shut.
- Edited `/etc/systemd/logind.conf`:
  ```
  HandleLidSwitch=ignore
  HandleLidSwitchExternalPower=ignore
  ```
- Restarted the service:
  ```bash
  sudo systemctl restart systemd-logind
  ```

### 3. Static IP with Netplan
- Configured a static IP address so the server is always reachable at the same address on the local network.
- Edited `/etc/netplan/00-installer-config.yaml`:
  ```yaml
  network:
    version: 2
    ethernets:
      eth0:
        dhcp4: no
        addresses: [192.168.x.x/24]
        gateway4: 192.168.x.x
        nameservers:
          addresses: [8.8.8.8, 8.8.4.4]
  ```
- Applied config:
  ```bash
  sudo netplan apply
  ```

### 4. Enable SSH
- Installed and enabled OpenSSH so the server can be accessed remotely.
  ```bash
  sudo apt update
  sudo apt install openssh-server
  sudo systemctl enable ssh
  sudo systemctl start ssh
  ```
- Connect from another machine:
  ```bash
  ssh username@192.168.x.x
  ```

---

## 🛠️ Tech Stack

| Tool | Purpose |
|------|---------|
| Ubuntu Server | OS |
| Netplan | Network configuration |
| OpenSSH | Remote access |
| systemd | Service & lid management |

---

## 🔜 Next Steps

- [ ] (Add your next goals here — e.g. install Docker, set up Nginx, host a service...)

---

## 📝 Notes
- Replace `192.168.x.x` with your actual static IP and gateway.
- Replace `eth0` with your actual network interface (check with `ip a`).