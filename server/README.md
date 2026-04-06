# Server Setup — Debian 13

## Hardware
Dell Inspiron 560s
- CPU: Intel Core 2 Duo
- RAM: 4GB
- OS Drive: 320GB WD HDD
- Data Drive: 1.8TB Toshiba HDD
- Hostname: inspiron

## OS
Debian 13 (Trixie) — minimal install, no GUI

## Network
- Hostname: inspiron
- Local IP: 192.168.1.116 (static, assigned via router DHCP reservation)

## Installation Notes

### Why Minimal Install
No desktop environment installed. Every package on this server
is there deliberately. Less software = smaller attack surface
= more secure server.

### Partition Layout (320GB OS drive)
| Partition | Size | Mount | Purpose |
|---|---|---|---|
| sda1 | 21.8GB | / | Root filesystem |
| sda5 | 8GB | /var | Logs, package data, service data |
| sda6 | 4GB | swap | Virtual memory |
| sda7 | 2.8GB | /tmp | Temporary files |
| sda8 | 261.5GB | /home | User files |

Separated /var deliberately — runaway logs cannot fill the
root partition and crash the server.

## Post-Install Configuration

### sudo Setup
Debian minimal doesn't configure sudo automatically.
Manually installed and added user to sudo group:
```bash
su -
apt install sudo -y
usermod -aG sudo priyanshudeep
```

### Essential Tools Installed
```bash
sudo apt install -y curl wget git htop tree ufw unattended-upgrades net-tools
```

### Automatic Security Updates
```bash
sudo dpkg-reconfigure unattended-upgrades
```

### UFW Firewall
```bash
sudo ufw default deny incoming
sudo ufw default allow outgoing
sudo ufw allow ssh
sudo ufw enable
```
Only port 22 (SSH) is open. Every other port is denied by default.

### SSH Hardening
- Password authentication disabled
- Root login disabled
- Key-based authentication only
- Dedicated SSH key created for server (separate from GitHub key)
- SSH config on laptop enables `ssh inspiron` shorthand

Config changes in /etc/ssh/sshd_config:
PasswordAuthentication no
PermitRootLogin no

## Storage

### 2TB HDD
- Device: /dev/sdb
- Label: 2TB_HDD
- Filesystem: ext4
- Mount point: /srv/data
- UUID: 2b7df7ab-7b59-4b21-a547-9b18d978fbbf
- Mounted via /etc/fstab with nofail flag

### fstab Entry
UUID=2b7df7ab-7b59-4b21-a547-9b18d978fbbf  /srv/data  ext4  defaults,nofail  0  2

### nofail Flag
Critical for servers — if the data drive fails or is disconnected,
the server still boots normally. Without this, a missing drive
prevents boot entirely.

## What I Learned
- Debian minimal doesn't configure sudo automatically — this is
  intentional, forcing conscious decisions about privilege escalation
- UFW must allow SSH before being enabled — enabling first would
  lock you out of your own server instantly
- Always test fstab with `sudo mount -a` before rebooting —
  a broken fstab entry can prevent the system from booting
- Use UUIDs in fstab, not device names — device names like /dev/sdb
  can change between boots, UUIDs never do
- Separate /var partition on servers prevents runaway logs from
  filling root and crashing the system
- SSH keys should be per-purpose — GitHub key and server key
  are separate so compromising one doesn't affect the other

## Services

### Samba File Sharing
- Share name: data
- Path: /srv/data
- Valid users: priyanshudeep
- UFW: Samba allowed

Mounted on laptop at /mnt/2TB_HDD via fstab (cifs).
Visible in Dolphin sidebar under Network.

### Jellyfin Media Server (Docker)
- Container: jellyfin/jellyfin:latest
- Port: 8096
- Config: ~/docker/jellyfin/config
- Cache: ~/docker/jellyfin/cache
- Media: /srv/data/media (mounted read-only)
- Access: http://192.168.1.116:8096
- Compose file: ~/docker/jellyfin/docker-compose.yml

#### Post-Setup Configuration
- OpenSubtitles plugin installed and configured
- Transcoding: software only, 2 threads (Core 2 Duo)
- Subtitle mode: Smart
- Hardware acceleration: None (Core 2 Duo not supported)
- Library scan: triggered manually when new media added
- Subtitle download: runs on server startup automatically

#### Media Structure
/srv/data/media/
├── movies/
└── tvshows/

#### Usage Notes
Server is not running 24/7 — powered on via smart plug when needed.
Scheduled tasks configured for on-startup triggers instead of
fixed times due to this usage pattern.

### Duck DNS Dynamic DNS
- Domain: goku-home.duckdns.org (see Bitwarden for full details)
- Update script: ~/scripts/duckdns.sh
- Schedule: every 5 minutes via cron
- Log file: ~/duckdns.log

Keeps domain pointing to current public IP automatically.
Essential for remote access when ISP changes public IP.
- [ ] WireGuard VPN
- [ ] Automated rsync backups
- [ ] fail2ban intrusion prevention

## WireGuard — Not Implemented

WireGuard VPN was configured correctly on both server and laptop
but failed due to Jio Fiber using CGNAT (Carrier Grade NAT).

CGNAT means no real public IP is assigned to the router —
inbound connections never reach the server regardless of port
forwarding configuration.

Solution: Switching to Oracle Cloud free VPS as a WireGuard
relay — VPS has a real public IP, home server connects outbound
to it bypassing CGNAT entirely.
