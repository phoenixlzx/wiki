title: System Security Configuration
category: Security
---

### File permission

Reset all file permissions on CentOS 6 (and RPM distros)

```
for package in $(rpm -qa); do rpm --setperms $package; done
for package in $(rpm -qa); do rpm --setugids $package; done
```

### Disable PING and traceroute/MTR request

For iptables rules:

```
iptables -A OUTPUT -p icmp  --icmp-type 0 -j DROP
iptables -A OUTPUT -p icmp  --icmp-type 8 -j DROP
iptables -A OUTPUT -p icmp  --icmp-type 11 -j DROP
iptables -A OUTPUT -p icmp  --icmp-type 30 -j DROP
```

Reference:

1. <https://geekpeek.net/reset-file-permissions/>
2. <http://crybit.com/iptables-rules-for-icmp/>
3. <http://serverfault.com/questions/701168>
