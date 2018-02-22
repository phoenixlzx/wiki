title: SSH
category: Server Configuration
time: 1489077036566
---

## SSH PortForwarding

### Enable IP Forwarding

```
sudo sysctl net.ipv4.ip_forward=1
```

### Forward traffic on port 2222 to IP 10.0.0.2 on port 22

```
sudo iptables -t nat -A PREROUTING -p tcp --dport 2222 -j DNAT --to-destination 10.0.0.2:22
```

### Masquerade

```
sudo iptables -t nat -A POSTROUTING -j MASQUERADE
```

### OpenSSH ProxyCommand

```
Host hosta
 	User userfoo
 	Hostname 123.123.123.123

Host hostb
	User userbar
 	Hostname 192.168.1.1
 	Port 2222
 	ProxyCommand ssh -q -W %h:%p hosta
```

Reference:

- [1] https://heipei.github.io/2015/02/26/SSH-Agent-Forwarding-considered-harmful/

