# Laptop Setup — Fedora KDE

## Hardware
HP Pavilion Gaming Laptop
- CPU: AMD Ryzen 5 5600H
- GPU: NVIDIA RTX 3050 Mobile (4GB) + AMD Radeon integrated
- RAM: 16GB
- Storage: 512GB SSD + 1TB HDD

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

## What I Learned
- Hybrid graphics (Optimus) hands display control between AMD and
  NVIDIA — the nouveau driver fails this handoff on first boot
- `nomodeset` forces the kernel to skip GPU mode switching,
  using basic framebuffer instead
- `akmod-nvidia` installs the kernel driver; nvidia-smi comes
  separately in the cuda package
- DNF5 replaced DNF4 in Fedora 43 — timer unit names changed,
  always verify with `systemctl list-unit-files` instead of guessing
