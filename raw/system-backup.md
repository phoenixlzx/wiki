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

