title: iptables
category: Security
time: 1489077283723
---

## Save rules and apply at startup

```
sudo sh -c "iptables-save > /etc/iptables.rules"
```

* To keep information from byte and packet counters:

```
sudo sh -c "iptables-save -c > /etc/iptables.rules"
```

Add to `/etc/network/interfaces` under corresponding interface:

```
pre-up iptables-restore < /etc/iptables.rules
```


