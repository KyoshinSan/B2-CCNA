# TP2 - Routage statique et services d'infra

**Auteur :** DINH Jonathan & HERNANDEZ Theo & SEGHIR Souleimane <br />
**Date :** 25/02/2019 <br />
**Desc :** Rendu du 2nd tp en CCNA

## I. Mise en place du lab
### 1. Création des VMs et adressage ip
#### Checklist IP VMs

Les VMs sont bien configuré :

![Screenshot_1](https://github.com/KyoshinSan/B2-CCNA/blob/master/tp/2/Screenshot_1.png?raw=true)

### 2. Routage statique

- ping `client1` et `server1` :<br />
![Screenshot_2](https://github.com/KyoshinSan/B2-CCNA/blob/master/tp/2/Screenshot_2.png?raw=true)
 
- ping `router1` et `server1` :
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

• <b>router1</b><br/><br/>
`[soussou@router1 ~]$ curl google.com`<br/>
`<HTML><HEAD><meta http-equiv="content-type" content="text/html;charset=utf-8">`<br/>
`<TITLE>301 Moved</TITLE></HEAD><BODY>`<br/>
`<H1>301 Moved</H1>`<br/>
`The document has moved`<br/>
`<A HREF="http://www.google.com/">here</A>.`<br/>
`</BODY></HTML>`<br/>
`[soussou@router1 ~]$`<br/>

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



