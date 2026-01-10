# Trim Tool 

**WIP - Alpha release**

Trim Tool is a whiptail-based TUI for inspecting and managing TRIM behaviour on Linux systems using SSD or NVMe storage. It is designed to inspect and configure TRIM with conservative defaults and explicit safety gates to avoid accidental data loss.

---

## What it does

Trim Tool detects and reports:

- Your Linux OS and whether systemd is present
- All block devices, clearly identifying:
  - SSD / NVMe
  - Spinning rust (HDD)
  - ZFS member disks
- Whether devices support discard / TRIM
- Mounted filesystems that are safe TRIM candidates (ext4, xfs, btrfs, f2fs)
- Whether mounts use continuous discard or rely on scheduled TRIM
- Scheduled TRIM status via `systemd fstrim.timer`
- Drive health via SMART / NVMe data (read-only, if `smartctl` is installed)

---

## What it will NOT do

For data protection, Trim Tool is intentionally conservative:

- ZFS safety gate
  - If ZFS is detected (mounted datasets or `zfs_member` disks), the tool:
    - Will not enable, disable, or configure TRIM scheduling
    - Will only provide information and health checks
- No destructive defaults
  - All confirmation dialogs explicitly default to no
- No silent changes
  - If the tool cannot safely help (no systemd, ZFS detected, missing tools), it explains why instead of guessing

---

## What it can safely do

- Show clear recommendations based on your setup
- Run TRIM dry-runs to estimate reclaimable space
- Run manual TRIM on explicitly selected non-ZFS mounts
- Enable / disable / schedule TRIM only on non-ZFS systems
- Export a plain-text recommendations report

---

## Usage

```bash
chmod +x trim-tool
./trim-tool
# or, for full capability:
sudo ./trim-tool
```



LICENSE: GPLv3 or later
