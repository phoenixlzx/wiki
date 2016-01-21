title: System Security Configuration
category: Security
---

### File permission

Reset all file permissions on CentOS 6 (and RPM distros)

```
for package in $(rpm -qa); do rpm --setperms $package; done
for package in $(rpm -qa); do rpm --setugids $package; done
```

Reference:

1. <https://geekpeek.net/reset-file-permissions/>
