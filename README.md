# homelab
Personal homelab — Fedora laptop + Debian server, Jellyfin, WireGuard VPN, automated backups

# Homelab

A self-hosted homelab built from scratch while learning DevOps.
Every decision, config, and script here is something I understand
— not blindly copied from a tutorial.

## Hardware

| Machine | Specs | Role |
|---|---|---|
| HP Pavilion Gaming | Ryzen 5 5600H, RTX 3050, 16GB RAM, 512GB SSD + 1TB HDD | Daily driver — Fedora KDE |
| Dell Inspiron 560s | Core 2 Duo, 4GB RAM, 321GB HDD + 2TB HDD | Home server — Debian 12 |

## What's Running

- **Jellyfin** — Self-hosted media server
- **Samba** — Network file sharing
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

- [x] Fedora KDE installed with NVIDIA drivers
- [x] GitHub SSH authentication configured
- [ ] Debian 12 server setup
- [ ] Storage and mount configuration
- [ ] Jellyfin media server
- [ ] Samba file sharing
- [ ] WireGuard VPN
- [ ] Automated rsync backups
- [ ] UFW firewall + fail2ban
