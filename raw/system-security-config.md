title: System Security Configuration
category: Security
---

### File permission

Reset all file permissions on CentOS 6 (and RPM distros)

```
for package in $(rpm -qa); do rpm --setperms $package; done
for package in $(rpm -qa); do rpm --setugids $package; done
```

### Disable PING request

For iptables rules:

```
iptables -A INPUT -p icmp --icmp-type echo-request -j DROP
iptables -A OUTPUT -p icmp --icmp-type echo-reply -j DROP
```

Reference:

1. <https://geekpeek.net/reset-file-permissions/>
2. <http://crybit.com/iptables-rules-for-icmp/>
