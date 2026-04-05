# Homelab

A self-hosted homelab built from scratch while learning DevOps.
Every decision, config, and script here is something I understand
— not blindly copied from a tutorial.

## Hardware

| Machine | Specs | Role |
|---|---|---|
| HP Pavilion Gaming | Ryzen 5 5600H, RTX 3050, 16GB RAM, 512GB NVMe SSD + 1TB HDD | Daily driver — Fedora KDE |
| Dell Inspiron 560s | Core 2 Duo, 4GB RAM, 320GB HDD + 1.8TB HDD | Home server — Debian 13 |

## Network
| Device | Hostname | Local IP |
|---|---|---|
| HP Pavilion Gaming | pavilion | 192.168.1.119 |
| Dell Inspiron 560s | inspiron | 192.168.1.116 |

## What's Running
- **Samba** — Network file sharing
- **Jellyfin** — Self-hosted media server
- **WireGuard** — VPN for remote access
- **rsync + cron** — Automated laptop backups to server

## Repository Structure

homelab/
├── laptop/         # Fedora setup, configs, dotfiles
├── server/         # Debian setup, service configs
│   └── scripts/    # Automation and backup scripts
├── wireguard/      # VPN setup documentation
└── docs/           # Concepts, decisions, learning notes

## Progress

### Laptop
- [x] Fedora 43 KDE installed
- [x] NVIDIA drivers configured (RTX 3050 + Optimus)
- [x] System configured — codecs, Chrome, auto-updates
- [x] 1TB HDD mounted at /mnt/data
- [x] Static IP assigned — 192.168.1.119

### Server
- [x] Debian 13 minimal installed — no GUI
- [x] sudo configured
- [x] UFW firewall — only SSH allowed
- [x] SSH hardened — key-based auth, no passwords, no root login
- [x] 1.8TB HDD mounted at /srv/data
- [x] Static IP assigned — 192.168.1.116

### Infrastructure
- [x] GitHub SSH authentication configured
- [x] Separate SSH keys — GitHub and server
- [x] SSH config — `ssh inspiron` shorthand
- [ ] Samba file sharing
- [ ] Jellyfin media server
- [ ] Duck DNS dynamic DNS
- [ ] WireGuard VPN
- [ ] Automated rsync backups
- [ ] fail2ban intrusion prevention
