title: IP Rules
category: Networking
---
#### IP Rules for special routed server

Add a table name to `/etc/iproute2/rt_tables`

```
10 orig
```

Add IP Rule, assuming main IP is `192.168.1.100`, gateway is `192.168.1.1`

```
ip rule add from 192.168.1.100 lookup orig
ip route add table orig default via 192.168.1.1
```
