title: DNSMasq
category: Server Configuration
time: 1476133839
---
#### Block DNS Amplification Attack

Add to IPTables rules:
```
# Create a chain to store block rules in
iptables -N BADDNS
# Match all "IN ANY" DNS Queries, and run them past the BADDNS chain.
iptables -A INPUT -p udp --dport 53 -m string --hex-string "|00 00 ff 00 01|" --to 255 --algo bm -m comment --comment "IN ANY?" -j BADDNS
iptables -A FORWARD -p udp --dport 53 -m string --hex-string "|00 00 ff 00 01|" --to 255 --algo bm -m comment --comment "IN ANY?" -j BADDNS
# Block domains that are being used for DNS Amplification...
iptables -A BADDNS -m string --hex-string "|04 72 69 70 65 03 6e 65 74 00|" --algo bm -j DROP --to 255 -m comment --comment "ripe.net"
iptables -A BADDNS -m string --hex-string "|03 69 73 63 03 6f 72 67 00|" --algo bm -j DROP --to 255 -m comment --comment "isc.org"
iptables -A BADDNS -m string --hex-string "|04 73 65 6d 61 02 63 7a 00|" --algo bm -j DROP --to 255 -m comment --comment "sema.cz"
iptables -A BADDNS -m string --hex-string "|09 68 69 7a 62 75 6c 6c 61 68 02 6d 65 00|" --algo bm -j DROP --to 255 -m comment --comment "hizbullah.me"
# Rate limit the rest.
iptables -A BADDNS -m recent --set --name DNSQF --rsource
iptables -A BADDNS -m recent --update --seconds 10 --hitcount 1 --name DNSQF --rsource -j DROP
```

#### DNS Amplification IPTABLES block lists

https://github.com/smurfmonitor/dns-iptables-rules

Reference:

1. [Dataforce](http://blog.dataforce.org.uk/2013/08/limiting-the-effectiveness-of-dns-amplification/)
