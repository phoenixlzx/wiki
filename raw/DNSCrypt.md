title: DNSCrypt
category: Security
---

Install from PPA:

```
sudo apt-get install software-properties-common
sudo add-apt-repository ppa:xuzhen666/dnscrypt
sudo apt-get update && sudo apt-get install dnscrypt-proxy
```

Append HK server by d0wn to `/usr/share/dnscrypt-proxy/dnscrypt-resolvers.csv`

```
d0wn-hk-ns1,d0wn server in Hong Kong,Server provided by Martin 'd0wn' Albus,Hong Kong,,https://dns.d0wn.biz,1,no,yes,yes,119.81.242.146:54,2.dnscrypt-cert.hk.d0wn.biz,9E3E:203F:7446:A15E:4451:9095:A5D2:A65E:2B48:9736:5AD2:4760:6627:8F33:0327:5382,
```

Update `/etc/default/dnscrypt-proxy`

```
DNSCRYPT_PROXY_LOCAL_ADDRESS=127.0.0.1:53  # Change to another port if you have local dns service

DNSCRYPT_PROXY_RESOLVER_NAME=d0wn-hk-ns1

DNSCRYPT_PROXY_OPTIONS="--ephemeral-keys"
```


