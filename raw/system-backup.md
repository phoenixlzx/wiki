title: System Backup 
category: System Administration
time: 1478673135219
---

## Rsync

```
rsync --archive --one-file-system --hard-links \
  --acls --xattrs --sparse \
  --human-readable --numeric-ids --delete --delete-excluded \
  --itemize-changes --verbose --progress \
  --exclude='*~' \
  --exclude-from=root.exclude \
  src dest
```

## Mount Linux Images

```
~> file archlinux.img
archlinux.img: x86 boot sector; partition 1: ID=0x83, active, starthead 32, startsector 2048, 41938944 sectors, code offset 0x63
```

```
~> fdisk -l archlinux.img 
You must set cylinders.
You can do this from the extra functions menu.

Disk archlinux.img: 0 MB, 0 bytes
255 heads, 63 sectors/track, 0 cylinders
Units = cylinders of 16065 * 512 = 8225280 bytes
Sector size (logical/physical): 512 bytes / 512 bytes
I/O size (minimum/optimal): 512 bytes / 512 bytes
Disk identifier: 0x91d8e293

        Device Boot      Start         End      Blocks   Id  System
archlinux.img1   *           1        2611    20969472   83  Linux
Partition 1 has different physical/logical endings:
     phys=(1023, 254, 63) logical=(2610, 180, 2)
```

```
~> mount -t ext4 -o loop,offset=$((2048*512)) archlinux.img /mnt/img/
```


