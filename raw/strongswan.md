title: Strongswan
category: Server Configuration
time: 1485925404602
---

## Compile Strongswan > 5.5.2

```
./configure  --enable-eap-identity --enable-eap-md5 --enable-eap-mschapv2 --enable-eap-tls --enable-eap-ttls --enable-eap-peap  --enable-eap-tnc --enable-eap-dynamic --enable-eap-radius --enable-xauth-eap  --enable-xauth-pam  --enable-dhcp  --enable-openssl  --enable-addrblock --enable-unity  --enable-certexpire --enable-radattr --enable-swanctl --enable-openssl --disable-gmp
```

## Apply for SINGLE DOMAIN Certificate

* `caCert.pem` for Root CA and Intermediate
* `serverCert.pem` for Certificate
* `serverKey.pem` for Private Key

```
mkdir -p /usr/local/etc/ipsec.d/{certs,cacerts,private}
cp caCert.pem /usr/local/etc/ipsec.d/cacerts/
cp serverCert.pem /usr/local/etc/ipsec.d/certs/
cp serverKey.pem /usr/local/etc/ipsec.d/private/
```

## Authentication

`/usr/local/etc/ipsec.secrets`

```
: RSA serverKey.pem
username : EAP "password"
username : XAUTH "password"
```

## Push DNS

`/usr/local/etc/strongswan.conf`

```
# strongswan.conf - strongSwan configuration file
#
# Refer to the strongswan.conf(5) manpage for details
#
# Configuration changes should be made in the included files

charon {
  load_modular = yes
  duplicheck.enable = no
  compress = yes
  dns1 = 8.8.8.8
  dns2 = 8.8.4.4
  plugins {
    include strongswan.d/charon/*.conf
  }
}
include strongswan.d/*.conf
```

## IPSec

`/usr/local/etc/ipsec.conf`

```
config setup
  uniqueids = never
conn ikev2ios
  keyexchange = ikev2
  ike=aes256-sha256-modp2048,3des-sha1-modp2048,aes256-sha1-modp2048,aes256-sha1-modp1024,aes128-sha1-modp1024!
  esp = aes256-sha256,3des-sha1,aes256-sha1!
  dpdaction=clear
  dpddelay=300s
  rekey=no
  left = %any
  leftid = your.domain.example.com
  leftsendcert = always
  leftcert = serverCert.pem
  leftsubnet = 0.0.0.0/0
  right = %any
  rightauth = eap-mschapv2
  rightsourceip = 10.99.1.0/24
  rightsendcert=never
  eap_identity = %any
  auto = add
  fragmentation = yes
conn ikev1android
  keyexchange = ikev1
  left = %any
  leftid = your.domain.example.com
  leftsendcert = always
  leftcert = serverCert.pem
  leftsubnet = 0.0.0.0/0
  right = %any
  rightauth = xauth
  rightsourceip = 10.99.1.0/24
  auto = add
```

## NAT Rules & IP forward

```
sysctl -w net.ipv4.ip_forward=1
iptables -t nat -I POSTROUTING -s 10.99.1.0/24 -j POSTROUTING
```

## Start & debug

```
ipsec start
ipsec stop
ipsec start --nofork # debug
ipsec restart
ipsec reload
```

## Connecting

#### For iOS

* VPN -> IKEv2
* Server -> Domain
* ID -> Domain

---

Reference:

* [zFox49's Blog](https://49.gs/post/2017-02-01)

