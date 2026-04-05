# Laptop Setup — Fedora KDE

## Hardware
HP Pavilion Gaming Laptop
- CPU: AMD Ryzen 5 5600H
- GPU: NVIDIA RTX 3050 Mobile (4GB) + AMD Radeon integrated
- RAM: 16GB
- Storage: 512GB NVMe SSD + 1TB HDD

## OS
Fedora 43 KDE Plasma Spin

## Installation Notes

### NVIDIA Black Screen Fix
On first boot after installation, the system shows a black screen
due to the nouveau driver conflicting with the Optimus hybrid
graphics setup. Fix:

1. At GRUB menu press E to edit boot entry
2. Add `nomodeset` to the end of the linux line
3. Boot with Ctrl+X
4. Install proper NVIDIA drivers immediately

### NVIDIA Driver Installation
```bash
# Enable RPM Fusion
sudo dnf install https://mirrors.rpmfusion.org/free/fedora/rpmfusion-free-release-$(rpm -E %fedora).noarch.rpm https://mirrors.rpmfusion.org/nonfree/fedora/rpmfusion-nonfree-release-$(rpm -E %fedora).noarch.rpm -y

# Install driver and CUDA utilities
sudo dnf install akmod-nvidia xorg-x11-drv-nvidia-cuda -y

# Wait 5 minutes for kernel module to compile, then reboot
# Do NOT add nomodeset on next boot — proper driver takes over
```

### Post-Install Steps
- Media codecs installed via RPM Fusion
- Firmware updated via fwupdmgr
- Hostname set to `pavilion`
- Automatic security updates enabled via dnf5-automatic
- Google Chrome installed via official RPM
- GPG key imported manually due to DNF5 lock timing issue

## Storage

### NVMe SSD (512GB)
- OS drive — Fedora installation
- Managed automatically by Fedora installer

### 1TB HDD
- Device: /dev/sda
- Label: 1TB_HDD
- Filesystem: ext4
- Mount point: /mnt/data
- UUID: 367b71e8-613e-41c5-8314-9a9db1111c30
- Mounted via /etc/fstab with nofail flag

### fstab Entry
UUID=367b71e8-613e-41c5-8314-9a9db1111c30  /mnt/data  ext4  defaults,nofail,x-gvfs-show  0  2

## Network
- Hostname: pavilion
- Local IP: 192.168.1.119 (static, assigned via router DHCP reservation)

## SSH
- Key type: ed25519
- Public key location: ~/.ssh/id_ed25519.pub
- Added to: GitHub
- SSH config: ~/.ssh/config

## What I Learned
- Hybrid graphics (Optimus) hands display control between AMD and
  NVIDIA — the nouveau driver fails this handoff on first boot
- `nomodeset` forces the kernel to skip GPU mode switching,
  using basic framebuffer instead
- `akmod-nvidia` installs the kernel driver; nvidia-smi comes
  separately in the cuda package
- DNF5 replaced DNF4 in Fedora 43 — timer unit names changed,
  always verify with `systemctl list-unit-files` instead of guessing
- UUIDs are used in fstab instead of device names like /dev/sda
  because device names can change between boots, UUIDs never do
- `nofail` in fstab ensures the system boots even if the drive
  is missing or fails
