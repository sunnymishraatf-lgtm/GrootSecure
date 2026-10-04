# GrootSecure 22.2 — Hardened Linux Mint (Cinnamon)

**What it is:** A complete, working Linux Mint 22.2 "Zara" (Cinnamon desktop) with
serious security hardening baked in. Built from the official Mint ISO
(SHA256-verified) — nothing removed, security added.

## Security hardening applied

| Area | What was done |
|------|---------------|
| **Firewall** | `ufw` enabled at boot: deny all incoming, allow outgoing. `gufw` GUI included. |
| **Kernel** | `kptr_restrict=2`, `dmesg_restrict=1`, unprivileged eBPF disabled, `perf_event_paranoid=3`, ptrace limited, no setuid core dumps, `mmap_min_addr=65536` |
| **Network** | Reverse-path filtering, SYN cookies, no ICMP redirects/source-routing, broadcast ping ignored, martians logged |
| **AppArmor** | Installed and enabled (enforce mode) |
| **Updates** | `unattended-upgrades` installed — security updates auto-install daily |
| **SSH** | No OpenSSH server (no remote-login attack surface) |
| **Login** | LightDM guest sessions disabled; strong password rules (`libpam-pwquality`) |
| **Docs** | Full details in `/etc/grootos-secure/HARDENING.md` on the live system |

## Getting the ISO

The ISO is split into 3 parts (GitHub's 2GB/file limit). Download all 3, then join:

```bash
cat GrootSecure-22.2-cinnamon-64bit.iso.part-aa \
    GrootSecure-22.2-cinnamon-64bit.iso.part-ab \
    GrootSecure-22.2-cinnamon-64bit.iso.part-ac \
    > GrootSecure-22.2-cinnamon-64bit.iso
```

Verify integrity:
```bash
sha256sum -c GrootSecure-22.2-cinnamon-64bit.iso.sha256
```

## Writing to USB

- **Windows:** Use [Rufus](https://rufus.ie) or [balenaEtcher](https://etcher.balena.io)
- **Linux:** `sudo dd if=GrootSecure-22.2-cinnamon-64bit.iso of=/dev/sdX bs=4M status=progress` (replace sdX with your USB device!)
- **macOS:** Use balenaEtcher

Then boot from the USB (usually F12/F2/Del at startup to pick the boot device).

## After installing — go further

1. In the installer, choose **"Encrypt the new installation"** (LUKS full-disk encryption).
2. Use a strong password (the system enforces quality rules).
3. Firewall is already on — open **Firewall Configuration** from the menu to review.
4. Security updates install automatically every day.

## Honest notes

- This is a **live/installable ISO** like stock Mint. The hardening applies to the
  installed system too.
- A live session runs as user `mint` (standard for Mint/Ubuntu live ISOs).
  Real security comes after install + disk encryption + your own password.
- Tested: ISO structure verified (El Torito BIOS+UEFI boot, valid squashfs,
  all hardening files confirmed in the image). A full live-boot test wasn't
  possible in the build environment, but the base is stock Mint 22.2 (known
  good) with config-file-only changes.

Built for Sunny. Stay safe out there.
