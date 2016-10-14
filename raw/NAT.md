title: NAT
category: Networking
time: 1476133839
---

## DNAT

Set up DNAT to forward with client source IP. (You may need a private tunnel as most IDCs have route filter)

* Gateway
  * eth0 public `1.2.3.4`
  * tun0 private `192.168.1.1`
* Backend
  * eth0 public `5.6.7.8`
  * tun0 private `192.168.1.2`

On gateway:

```
iptables -t nat -I PREROUTING -p tcp -i eth0 --dport 8080 -j DNAT --to-destination 192.168.1.2:8080
```

On backend:

Add to `/etc/iproute2/rt_tables`

```
100 priv
```

then set default route when traffic coming from NAT

```
ip rule add from 192.168.1.2 lookup priv
ip route add default dev tun0 table priv
```

## DNAT & SNAT

Use this if you do not need to forward client source IP.

On gateway:

```
iptables -t nat -I PREROUTING -p tcp -i eth0 --dport 8080 -j DNAT --to-destination 192.168.1.2:8080
iptables -t nat -I POSTROUTING -p tcp -o tun0 -j SNAT --to-source 192.168.1.1 
```


