title: Ubuntu APT
category: Server Configuration
time: 1489719582264
---

## Disable IPv6 for apt

```
echo 'Acquire::ForceIPv4 "true";' | tee /etc/apt/apt.conf.d/99force-ipv4
```

## Disable unattended upgrades

```
echo 'APT::Periodic::Unattended-Upgrade "0";' | tee /etc/apt/apt.conf.d/10periodic
```

