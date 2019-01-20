title: iLO-SSH
category: Hardware
time: 1533289161965
---

Firstly, Enable SSH in iLO Web Administration page.

1. Login with SSH

```
ssh Administrator@hpe_server_ilo_ip
```

and invoke virtual serial port using `vsp` command

```
vsp
```

2. Then reboot the server, wait for system to initialize. If you see `<F9 = Setup>` - do not press it or the serial port will boot into GUI mode.

3. Press `ESC` + `9` for ROM-Based Setup Utility

4. You should see `rbsu>` prompt. To list Video options, type

```
SHOW CONFIG VIDEO OPTIONS
```

It should display

```
1|Optional Video Primary, Embedded Video Disabled <=
2|Optional Video Primary, Embedded Video Secondary
3|Embedded Video Primary, Optional Video Secondary
```

5. Modify video options

```
SET CONFIG VIDEO OPTIONS 3
```

To change to

```
1|Optional Video Primary, Embedded Video Disabled
2|Optional Video Primary, Embedded Video Secondary
3|Embedded Video Primary, Optional Video Secondary <=
```

6. Type `EXIT` to logout and reboot.

### Set iLO Network via Onboard Administrator

**SSH in OA**

```
connect server [server bay id]
```

Note this is actually connecting to bay iLO, via the IP showing in OA.

If you set same IP for multiple server iLO, OA can't recognize them correctly.

**SSH in iLO**

```
show /map1/enetport1/lanendpt1/ipendpt1
```

```
set /map1/enetport1/lanendpt1/ipendpt1 IPv4Address=10.10.10.100 SubnetMask=255.255.255.0
```

Note: This will reset iLO

```
set  /map1/gateway1 AccessInfo=10.10.10.1
```

Note: This will reset iLO

**Others**

```
set /map1/dhcpendpt1 EnabledState=no
```

```
set /map1/accounts1/emergency password=Passw0rd
```

```
create /map1/accounts1 username=Admin password=P@ssword
```

Reference: <https://lukas.dzunko.sk/index.php/Hardware:_HP_Microserver_-_How_to_fix_ILO4_after_adding_second_graphics_card>
