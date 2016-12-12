title: NTFS
category: OS X
time: 1476133839
---
Enable NTFS Read-Write on OS X using UUID.

Plug-in the drive, and execute:

```
diskutil info /Volumes/DRIVENAME | grep UUID
```

Where `DRIVENAME` is your mounting drive directory name.

Then write to `/etc/fstab`

```
sudo echo "UUID=ENTER_UUID_HERE none ntfs rw,auto,nobrowse" >> /etc/fstab
```

Unmount the disk and re-plugin it, the target disk will not show on desktop. So open it from terminal:

```
open /Volume
```

And write files to the target disk.
