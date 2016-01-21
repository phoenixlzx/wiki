title: ethtool
category: Server Configuration
---
#### Check status of eth0

```
lspci |grep Ethernet
dmesg |grep eth0
ethtool eth0
ethtool -k eth0
ethtool -S eth0
```

#### Force 1000Mbps and full duplex on eth0

```
ethtool -s eth0 speed 1000 duplex full autoneg off
```
(Under CentOS / RHEL)

To make the changes permanent, edit `/etc/sysconfig/network-scripts/ifcfg-eth0`, add
```
ETHTOOL_OPTS="speed 1000 duplex full autoneg off"
```

Reference:
1. [Adrenalin’s Experience](https://adrenalinexp.wordpress.com/2011/01/06/linux-centos-how-to-force-1000megabits-ifconfig-eth0/)