# GRUB Boots the Wrong Linux Partition (Two Installs, One Shared ESP)

**System:** Acer Nitro AN515-45, UEFI, MX Linux KDE 25.3 (Debian 13 base)
**Layout:** main system on `nvme0n1p6`, test system on `nvme0n1p9`, both sharing the ESP on `nvme0n1p1`

---

## 1. Symptom

After installing a second MX Linux on a test partition (p9), the machine boots into p9 by default. The main system (p6) is only reachable by pressing F12 and picking "MX Linux" from the firmware boot menu.

## 2. Root cause

Both installs share one EFI System Partition (ESP). The ESP does not hold the GRUB menu itself. It holds small **stub** files (`grub.cfg`, 3 lines) that say "find the partition with this UUID and load its menu."

The second install (p9) wrote **its own root UUID** into those stubs, so every boot path that reads them hands control to p9.

The firmware was booting through the generic fallback entry (`Linpus lite` → `\EFI\Boot\grubx64.efi`), which appears to read `EFI/debian/grub.cfg`. That stub still pointed to p9 even after `grub-install` fixed `EFI/MX/grub.cfg`.

> Note: "Linpus lite" is just a leftover label (from Acer's old netbook distro) that the firmware uses for the fallback loader. It is not a real install.

## 3. Key identifiers used in this case

| Item | Value |
|---|---|
| p6 (main) root UUID | `cb67a219-7807-486c-aef5-41610b676cdf` |
| p9 (test) root UUID | `df503a65-377d-4bf8-a273-dfffee6d4b8c` |
| Stub 1 | `/boot/efi/EFI/MX/grub.cfg` |
| Stub 2 | `/boot/efi/EFI/debian/grub.cfg` |

Find your own UUIDs with `lsblk -f`.

## 4. Diagnosis

Run these from the system you want to be the "boss" (p6).

```bash
findmnt /                          # confirm you are on the intended root
ls /boot/efi/EFI/                  # list EFI folders (MX, debian, ubuntu, ...)
efibootmgr                         # list boot entries and BootOrder
sudo cat /boot/efi/EFI/MX/grub.cfg
sudo cat /boot/efi/EFI/debian/grub.cfg
lsblk -f                           # map UUIDs to partitions
```

**What to look for:** the `search.fs_uuid` line in each stub. If it shows the wrong partition's UUID, that stub is the problem.

Example of a stub:

```
search.fs_uuid df503a65-377d-4bf8-a273-dfffee6d4b8c root
set prefix=($root)'/boot/grub'
configfile $prefix/grub.cfg
```

## 5. Fix

### Step 1: Back up the stubs

```bash
sudo cp -a /boot/efi/EFI/debian ~/efi-debian-backup
sudo cp -a /boot/efi/EFI/MX ~/efi-MX-backup
```

### Step 2: Make sure os-prober is enabled

So the other install and Windows still show up in the menu.

```bash
grep OS_PROBER /etc/default/grub
```

It should read `GRUB_DISABLE_OS_PROBER=false`. Change it if not.

### Step 3: Reinstall GRUB and rebuild the menu (from p6)

```bash
sudo grub-install
sudo update-grub
```

- `grub-install` writes the bootloader and its stub (and may add an NVRAM entry).
- `update-grub` generates `/boot/grub/grub.cfg`, the menu itself.
- Stop and investigate if either prints an error. Do not reboot.

Expected `update-grub` output includes your kernel, "Found Windows Boot Manager", and "Found MX ... on /dev/nvme0n1p9".

### Step 4: Check every stub, not just one

```bash
sudo cat /boot/efi/EFI/MX/grub.cfg /boot/efi/EFI/debian/grub.cfg
```

In this case `grub-install` fixed `EFI/MX/grub.cfg` but **left `EFI/debian/grub.cfg` pointing to p9**.

### Step 5: Repoint any stub that still shows the wrong UUID

```bash
sudo sed -i 's/df503a65-377d-4bf8-a273-dfffee6d4b8c/cb67a219-7807-486c-aef5-41610b676cdf/' /boot/efi/EFI/debian/grub.cfg
sudo cat /boot/efi/EFI/debian/grub.cfg
```

The `search.fs_uuid` line should now show p6's UUID.

### Step 6: Pre-reboot gate

Reboot only if all of these are true:

- [ ] Every stub shows p6's UUID.
- [ ] `sudo grep -m3 "root=UUID" /boot/grub/grub.cfg` shows p6's UUID on the kernel lines.
- [ ] `efibootmgr` still lists MX Linux and Windows Boot Manager.

### Step 7: Reboot without pressing F12

Expected result: p6's GRUB menu appears, with p9, Windows, and "EFI firmware configuration" as extra entries. Confirm with `findmnt /`.

## 6. Result in this case

After the `EFI/debian/grub.cfg` stub was repointed to p6, the default boot went to the intended system.

## 7. Notes on the duplicate boot entries (`mx` and `MX Linux`)

After `grub-install`, `efibootmgr` showed two near-identical entries:

```
Boot0002* mx        ... \EFI\mx\shimx64.efi
Boot0003* MX Linux  ... \EFI\MX\shimx64.efi
```

- They differ only in letter case. Linux's FAT driver ignores case, so `grub-install` wrote into the existing `MX` folder, and no lowercase `mx` folder exists.
- The UEFI spec says firmware should also ignore case, but some firmware is sloppy about it. The lowercase entry **may** fail on some machines. This was **not confirmed** as a cause here.
- It is harmless to leave both. To tidy up (optional, and test one change at a time):

```bash
sudo efibootmgr -b 0002 -B
sudo efibootmgr -o 0003,0004,0001,0000,2001,2002,2003
efibootmgr
```

Use your own entry numbers. Keep the fallback entry (`Linpus lite`) and Windows in the list as safety nets. A later `grub-install` may recreate the `mx` entry.

## 8. Rollback and recovery

| Situation | What to do |
|---|---|
| Want to undo the stub edits | `sudo cp -a ~/efi-MX-backup/. /boot/efi/EFI/MX/` and the same for `debian` |
| Dropped to a `grub>` prompt | Type `ls` to list partitions, then `configfile (hdX,gptN)/boot/grub/grub.cfg` using the p6 partition |
| Cannot reach GRUB at all | Use F12 to pick another entry (Windows or the other Linux), or boot a live USB, mount the ESP, and restore the backups |

## 9. Prevention

- Run `grub-install` and `update-grub` **only from the main system (p6)**. Running them from the test system, or letting its GRUB packages update, can rewrite the stubs back to the test UUID.
- Treat the test partition as a pure test system, and delete it when done. After deleting it, run `sudo update-grub` from p6 to remove its menu entry.
- Before installing a second OS on a shared ESP, note which partition is which (`lsblk -f`) and **never tick "format"** on the main root, home, or data partitions.
- Check free space on the ESP (`df -h /boot/efi`). Here the filesystem was only ~96 MB with about 23 MB free, even though the partition is 600 MB. Old `ubuntu` and `kubuntu` folders can be removed later to free space, but only after several clean boots.
- Make a backup of the ESP before bootloader work: `sudo cp -a /boot/efi ~/esp-backup`.

## 10. Quick reference

```bash
# 1. Identify
lsblk -f && efibootmgr && findmnt /

# 2. Inspect stubs
sudo cat /boot/efi/EFI/MX/grub.cfg /boot/efi/EFI/debian/grub.cfg

# 3. Backup
sudo cp -a /boot/efi/EFI/debian ~/efi-debian-backup
sudo cp -a /boot/efi/EFI/MX ~/efi-MX-backup

# 4. Reinstall loader and rebuild menu (from the main system)
sudo grub-install && sudo update-grub

# 5. Re-check stubs, then fix any that still show the wrong UUID
sudo sed -i 's/<WRONG_UUID>/<CORRECT_UUID>/' /boot/efi/EFI/debian/grub.cfg
```
