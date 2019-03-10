# TP2 - Routage statique et services d'infra

**Auteur :** DINH Jonathan & HERNANDEZ Theo & SEGHIR Souleimane <br />
**Date :** 25/02/2019 <br />
**Desc :** Rendu du 2nd tp en CCNA

## I. Mise en place du lab
### 1. Création des VMs et adressage ip
#### Checklist IP VMs

On s'assure ques les interfaces fonctionnent :

- `client1`

```
[jdinh@client1 ~]$ ping -c 2 router1
PING router1 (10.2.1.254) 56(84) bytes of data.
64 bytes from router1 (10.2.1.254): icmp_seq=1 ttl=64 time=0.560 ms
64 bytes from router1 (10.2.1.254): icmp_seq=2 ttl=64 time=0.359 ms

--- router1 ping statistics ---
2 packets transmitted, 2 received, 0% packet loss, time 1001ms
rtt min/avg/max/mdev = 0.359/0.459/0.560/0.102 ms
```

- `router1`

```
[jdinh@router1 ~]$ ping -c 2 client1
PING client1 (10.2.1.10) 56(84) bytes of data.
64 bytes from client1 (10.2.1.10): icmp_seq=1 ttl=64 time=0.275 ms
64 bytes from client1 (10.2.1.10): icmp_seq=2 ttl=64 time=0.725 ms

--- dhcp-server ping statistics ---
2 packets transmitted, 2 received, 0% packet loss, time 999ms
rtt min/avg/max/mdev = 0.275/0.500/0.725/0.225 ms

[jdinh@router1 ~]$ ping -c 2 router2
PING router2 (10.2.12.3) 56(84) bytes of data.
64 bytes from router2 (10.2.12.3): icmp_seq=1 ttl=64 time=0.578 ms
64 bytes from router2 (10.2.12.3): icmp_seq=2 ttl=64 time=0.520 ms

--- router2 ping statistics ---
2 packets transmitted, 2 received, 0% packet loss, time 1000ms
rtt min/avg/max/mdev = 0.520/0.549/0.578/0.029 ms
```

- `router2`

```
[jdinh@router2 ~]$ ping -c 2 router1
PING router1 (10.2.12.2) 56(84) bytes of data.
64 bytes from router1 (10.2.12.2): icmp_seq=1 ttl=64 time=0.281 ms
64 bytes from router1 (10.2.12.2): icmp_seq=2 ttl=64 time=0.251 ms

--- router1 ping statistics ---
2 packets transmitted, 2 received, 0% packet loss, time 1000ms
rtt min/avg/max/mdev = 0.251/0.266/0.281/0.015 ms

[jdinh@router2 ~]$ ping -c 2 server1
PING server1 (10.2.2.10) 56(84) bytes of data.
64 bytes from server1 (10.2.2.10): icmp_seq=1 ttl=64 time=0.782 ms
64 bytes from server1 (10.2.2.10): icmp_seq=2 ttl=64 time=0.565 ms

--- server1 ping statistics ---
2 packets transmitted, 2 received, 0% packet loss, time 1000ms
rtt min/avg/max/mdev = 0.565/0.673/0.782/0.111 ms
```

- `server1`

```
[jdinh@server1 ~]$ ping -c 2 router2
PING router2 (10.2.2.254) 56(84) bytes of data.
64 bytes from router2 (10.2.2.254): icmp_seq=1 ttl=64 time=0.394 ms
64 bytes from router2 (10.2.2.254): icmp_seq=2 ttl=64 time=0.647 ms

--- router2 ping statistics ---
2 packets transmitted, 2 received, 0% packet loss, time 999ms
rtt min/avg/max/mdev = 0.394/0.520/0.647/0.128 ms
```

### 2. Routage statique

- `router1`

```
[jdinh@router1 ~]$ sudo ip route add 10.2.2.0/24 via 10.2.12.3 dev enp0s9
[jdinh@router1 ~]$ ip r s
10.2.2.0/24 via 10.2.12.3 dev enp0s9
```

- `router2`

```
[jdinh@router2 ~]$ sudo ip route add 10.2.1.0/24 via 10.2.12.2 dev enp0s8
[jdinh@router2 ~]$ ip r s
10.2.1.0/24 via 10.2.12.2 dev enp0s8
```

- `client1`

```
[jdinh@client1 ~]$ sudo ip route add 10.2.2.0/24 via 10.2.1.254 dev enp0s3
[jdinh@client1 ~]$ ip r s
10.2.2.0/24 via 10.2.1.254 dev enp0s3
```

- `server1`

```
[jdinh@server1 ~]$ sudo ip route add 10.2.1.0/24 via 10.2.2.254 dev enp0s3
[jdinh@server1 ~]$ ip r s
10.2.1.0/24 via 10.2.2.254 dev enp0s3
```

- ping `client1` et `server1` :

	- ping `client1`
	
	```
	[jdinh@client1 ~]$ ping -c 2 server1
	PING server1 (10.2.2.10) 56(84) bytes of data.
	64 bytes from server1 (10.2.2.10): icmp_seq=1 ttl=62 time=1.24 ms
	64 bytes from server1 (10.2.2.10): icmp_seq=2 ttl=62 time=2.02 ms
	
	--- server1 ping statistics ---
	2 packets transmitted, 2 received, 0% packet loss, time 1002ms
	rtt min/avg/max/mdev = 1.245/1.634/2.024/0.391 ms
	```

	- ping `server1`
	
	```
	[jdinh@server1 ~]$ ping -c2 client1
	PING client1 (10.2.1.10) 56(84) bytes of data.
	64 bytes from client1 (10.2.1.10): icmp_seq=1 ttl=62 time=1.10 ms
	64 bytes from client1 (10.2.1.10): icmp_seq=2 ttl=62 time=2.01 ms
	
	--- dhcp-server ping statistics ---
	2 packets transmitted, 2 received, 0% packet loss, time 1001ms
	rtt min/avg/max/mdev = 1.103/1.561/2.019/0.458 ms
	```
 
- ping `router1` et `server1` :

	- `router1`
	
	```
	[jdinh@router1 ~]$ ping -c 2 server1
	PING server1 (10.2.2.10) 56(84) bytes of data.
	^C
	--- server1 ping statistics ---
	2 packets transmitted, 0 received, 100% packet loss, time 999ms
	```
	c'est normal car `server1` ne possèdent aucun route vers `net12`
  
### 3. Visualisation du routage avec Wireshark
  
- Capture 1 (net12) :
![Screenshot_3](https://github.com/KyoshinSan/B2-CCNA/blob/master/tp/2/Screenshot_3.png?raw=true)

- Capture 2 (net2) :
![Screenshot_4](https://github.com/KyoshinSan/B2-CCNA/blob/master/tp/2/Screenshot_4.png?raw=true)

La seul différence c'est les adresses mac.

## II. NAT et services d'infra
### 1. Mise en place du NAT

Le NAT a bien été mis en place, on peut faire un <b>curl google.com</b> depuis la machine :

• <b>`router1`</b>

```
[soussou@router1 ~]$ curl google.com
<HTML><HEAD><meta http-equiv="content-type" content="text/html;charset=utf-8">
<TITLE>301 Moved</TITLE></HEAD><BODY>
<H1>301 Moved</H1>
The document has moved
<A HREF="http://www.google.com/">here</A>.
</BODY></HTML>
```

• <b>router2</b><br /><br /> 
`[soussou@router2 ~]$ curl google.com`<br />
`<HTML><HEAD><meta http-equiv="content-type" content="text/html;charset=utf-8">`<br />
`<TITLE>301 Moved</TITLE></HEAD><BODY>\'`<br />
`<H1>301 Moved</H1>\`<br />
`The document has moved`<br />
`<A HREF="http://www.google.com/">here</A>.`<br />
`</BODY></HTML>`<br />
`[soussou@router2 ~]$`<br /><br />

• <b>client1</b><br /><br />
`[soussou@client1 ~]$ curl google.com`<br />
`<HTML><HEAD><meta http-equiv="content-type" content="text/html;charset=utf-8">`<br />
`<TITLE>301 Moved</TITLE></HEAD><BODY>`<br />
`<H1>301 Moved</H1>`<br />
`The document has moved`<br />
`<A HREF="http://www.google.com/">here</A>.`<br />
`</BODY></HTML>`<br />
`[soussou@client1 ~]$`<br /><br />

### 2. DHCP Server

- Après avoir recyclé la machine <b>client1.net1.b2</b> en <b>dhcp-server.net1.b2</b> :<br />
`soussou@dhcp-server ~]$ sudo systemctl start dhcpd`<br />
`[soussou@dhcp-server ~]$ sudo systemctl status dhcpd`<br />
`● dhcpd.service - DHCPv4 Server Daemon`<br />
`   Loaded: loaded (/usr/lib/systemd/system/dhcpd.service; disabled; vendor preset: disabled)`<br />
`   Active: active (running) since dim. 2019-03-10 00:14:18 CET; 1s ago`<br />
`     Docs: man:dhcpd(8)`<br />
`           man:dhcpd.conf(5)`<br />
` Main PID: 3928 (dhcpd)`<br />
`   Status: "Dispatching packets..."`<br />
`   CGroup: /system.slice/dhcpd.service`<br />
`           └─3928 /usr/sbin/dhcpd -f -cf /etc/dhcp/dhcpd.conf -user dhcpd -group dhcpd --no-pid`<br />
`mars 10 00:14:18 dhcp-server.net1.b2 dhcpd[3928]: Not searching LDAP since ldap-server, ldap-port and ldap-base-dn were not s...g file`<br />
`mars 10 00:14:18 dhcp-server.net1.b2 dhcpd[3928]: Internet Systems Consortium DHCP Server 4.2.5`<br />
`mars 10 00:14:18 dhcp-server.net1.b2 dhcpd[3928]: Copyright 2004-2013 Internet Systems Consortium.`<br />
`mars 10 00:14:18 dhcp-server.net1.b2 dhcpd[3928]: All rights reserved.`<br />
`mars 10 00:14:18 dhcp-server.net1.b2 dhcpd[3928]: For info, please visit https://www.isc.org/software/dhcp/`<br />
`mars 10 00:14:18 dhcp-server.net1.b2 dhcpd[3928]: Wrote 1 leases to leases file.`<br />
`mars 10 00:14:18 dhcp-server.net1.b2 dhcpd[3928]: Listening on LPF/enp0s3/08:00:27:e3:87:b0/10.2.1.0/24`<br />
`mars 10 00:14:18 dhcp-server.net1.b2 dhcpd[3928]: Sending on   LPF/enp0s3/08:00:27:e3:87:b0/10.2.1.0/24`<br />
`mars 10 00:14:18 dhcp-server.net1.b2 dhcpd[3928]: Sending on   Socket/fallback/fallback-net`
`mars 10 00:14:18 dhcp-server.net1.b2 systemd[1]: Started DHCPv4 Server Daemon.`<br />
`Hint: Some lines were ellipsized, use -l to show in full.`<br />
`[soussou@dhcp-server ~]$`<br />
-- > On a désormais un serveur DHCP qui va permettre aux <b>clients</b> de récupérer une ip et l'adresse de la passerelle directement dans net1.<br /><br />
- On configure une nouvelle machine nommée <b>client2</b> puis on s'assure que ça a bien fonctionné :<br />
`[soussou@client2 ~]$ ip a`<br />
`1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN group default qlen 1000`<br />
`    link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00`<br />
`    inet 127.0.0.1/8 scope host lo`<br />
`       valid_lft forever preferred_lft forever`<br />
`    inet6 ::1/128 scope host`<br />
`       valid_lft forever preferred_lft forever`<br />
`2: enp0s3: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc pfifo_fast state UP group default qlen 1000`<br />
`    link/ether 08:00:27:cb:85:33 brd ff:ff:ff:ff:ff:ff`<br />
`    inet 10.2.1.50/24 brd 10.2.1.255 scope global noprefixroute dynamic enp0s3`<br />
`       valid_lft 572sec preferred_lft 572sec`<br />
`    inet6 fe80::a00:27ff:fecb:8533/64 scope link`<br />
`       valid_lft forever preferred_lft forever`<br />
`[soussou@client2 ~]$`<br />


### 3. NTP Server

- Une fois <b>chrony</b> mis en place :<br /><br />
`[soussou@router1 ~]$ chronyc sources`<br />
`210 Number of sources = 4`<br />
`MS Name/IP address         Stratum Poll Reach LastRx Last sample`<br />
`===============================================================================`<br />
`^? ntp-2.arkena.net              2   6     1     6  -943.7s[-943.7s] +/-   27ms`<br />
`^? 30-251-47-212.rev.cloud.>     2   6     1     7  -943.7s[-943.7s] +/-   60ms`<br />
`^? y.ns.gin.ntt.net              2   6     1     6  -943.7s[-943.7s] +/-  106ms`<br />
`^? ks3370497.kimsufi.com         2   6     1     6  -943.6s[-943.6s] +/-   57ms`<br />
`[soussou@router1 ~]$ chronyc tracking`<br />
`Reference ID    : 7F7F0101 ()`<br />
`Stratum         : 10`<br />
`Ref time (UTC)  : Mon Mar 04 14:55:33 2019`<br />
`System time     : 0.000000000 seconds fast of NTP time`<br />
`Last offset     : +0.000000000 seconds`<br />
`RMS offset      : 0.000000000 seconds`<br />
`Frequency       : 0.381 ppm fast`<br />
`Residual freq   : +0.000 ppm`<br />
`Skew            : 0.000 ppm`<br />
`Root delay      : 0.000000000 seconds`<br />
`Root dispersion : 0.000000000 seconds`<br />
`Update interval : 0.0 seconds`<br />
`Leap status     : Normal`<br />
`[soussou@router1 ~]$`<br />



