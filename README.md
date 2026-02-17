# mkinitcpio-autohooks

`mkinitcpio-autohooks` is a small helper that automatically manages the `HOOKS=(...)` line in a mkinitcpio config file based on *actual* storage features detected on the machine.

It is designed for setups where `mdadm` can be installed as a dependency, but you only want the `mdadm_udev` hook when **mdraid is really present**, and the `encrypt` (or `sd-encrypt`) hook only when **LUKS is really present**.

## What it does

On each run it:

- Reads the first `HOOKS=(...)` line from the target config file (default: `/etc/mkinitcpio.conf.d/hooks-udev.conf`)
- Detects:
  - **mdraid** presence
    - `blkid -t TYPE=linux_raid_member` (preferred)
    - `/sys/block/md*` (fallback)
    - `lsblk` raid/md types (fallback)
  - **LUKS** presence
    - `blkid -t TYPE=crypto_LUKS` (preferred)
    - `lsblk` FSTYPE fallback
- Removes managed hooks first: `mdadm_udev`, `encrypt`, `sd-encrypt`
- Re-inserts:
  - `mdadm_udev` **only if mdraid is present**
  - `encrypt` **only if LUKS is present**
  - If the existing HOOKS contain `systemd`, it uses `sd-encrypt` instead of `encrypt`
- Inserts the managed hooks **after `block`** (or before `filesystems` if `block` is missing)
- Writes a timestamped backup:
  - `/etc/mkinitcpio.conf.d/hooks-udev.conf.bak.YYYYMMDD-HHMMSS`
- Runs `mkinitcpio -P` **only if the HOOKS line changed**

## Files in this package

- `/usr/bin/mkinitcpio-autohooks` — the script
- `/usr/share/libalpm/hooks/90-mkinitcpio-autohooks.hook` — pacman hook to run it automatically
- `/usr/share/doc/mkinitcpio-autohooks/README.md` — this file
- `/usr/share/licenses/mkinitcpio-autohooks/LICENSE` — MIT license

## Configuration

By default the pacman hook calls:

```bash
/usr/bin/mkinitcpio-autohooks /etc/mkinitcpio.conf.d/hooks-udev.conf
```

If your system uses a different mkinitcpio config file, edit the hook and change the path.

## Manual usage

Run (as root):

```bash
sudo mkinitcpio-autohooks /etc/mkinitcpio.conf.d/hooks-udev.conf
```

You should see output like:

- `OLD: HOOKS=(...)`
- `NEW: HOOKS=(...)`
- `running mkinitcpio -P` (only when changed)

## Quick verification

Check detections:

```bash
blkid -t TYPE=linux_raid_member -o device || true
blkid -t TYPE=crypto_LUKS -o device || true
ls -1 /sys/block | grep -E '^md[0-9]+' || echo "md devices: none"
```

Check the final HOOKS line:

```bash
grep -E '^[[:space:]]*HOOKS=\(' -n /etc/mkinitcpio.conf.d/hooks-udev.conf
```

Rebuild with verbose output (optional):

```bash
sudo mkinitcpio -P -v
```

## Notes

- Keeping `mdadm` installed does **not** automatically imply mdraid is used; this tool only enables `mdadm_udev` when mdraid metadata/devices are detected.
- If you use `systemd` in mkinitcpio HOOKS, you generally want `sd-encrypt` for LUKS.
- This tool intentionally limits itself to managing only `mdadm_udev` and `encrypt`/`sd-encrypt` and leaves all other hooks untouched.

## Build the package

From the directory containing `PKGBUILD`:

```bash
makepkg -si
```

## License

GNU (see `LICENSE`).
