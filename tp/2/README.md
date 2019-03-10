# TP2 - Routage statique et services d'infra

**Auteur :** DINH Jonathan & HERNANDEZ Theo & SEGHIR Souleimane<br />
**Date :** 25/02/2019<br />
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
*****
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
*****
### 3. Visualisation du routage avec Wireshark
  
- Capture 1 (net12) :
![Screenshot_3](https://github.com/KyoshinSan/B2-CCNA/blob/master/tp/2/Screenshot_3.png?raw=true)

- Capture 2 (net2) :
![Screenshot_4](https://github.com/KyoshinSan/B2-CCNA/blob/master/tp/2/Screenshot_4.png?raw=true)

La seul différence c'est les adresses mac.

## II. NAT et services d'infra
### 1. Mise en place du NAT

Le NAT a bien été mis en place, on peut faire un <b>`curl google.com`</b> depuis la machine :

- <b>`router1`</b>

```
[soussou@router1 ~]$ curl google.com
<HTML><HEAD><meta http-equiv="content-type" content="text/html;charset=utf-8">
<TITLE>301 Moved</TITLE></HEAD><BODY>
<H1>301 Moved</H1>
The document has moved
<A HREF="http://www.google.com/">here</A>.
</BODY></HTML>
```

- <b>`router2`</b>

```
[soussou@router2 ~]$ ip r s
default via 10.2.12.2 dev enp0s8

[soussou@router2 ~]$ curl google.com
<HTML><HEAD><meta http-equiv="content-type" content="text/html;charset=utf-8">
<TITLE>301 Moved</TITLE></HEAD><BODY>
<H1>301 Moved</H1>
The document has moved
<A HREF="http://www.google.com/">here</A>.
</BODY></HTML>
```

- <b>`client1`</b>

```
[soussou@client1 ~]$ ip r s
default via 10.2.1.254 dev enp0s3

[soussou@client1 ~]$ curl google.com
<HTML><HEAD><meta http-equiv="content-type" content="text/html;charset=utf-8">
<TITLE>301 Moved</TITLE></HEAD><BODY>
<H1>301 Moved</H1>
The document has moved
<A HREF="http://www.google.com/">here</A>.
</BODY></HTML>
```
*****
### 2. DHCP Server

- Après avoir recyclé la machine <b>`client1.net1.b2`</b> en <b>`dhcp-server.net1.b2`</b> :

```
[soussou@dhcp-server ~]$ sudo systemctl start dhcpd
[soussou@dhcp-server ~]$ sudo systemctl status dhcpd
● dhcpd.service - DHCPv4 Server Daemon
   Loaded: loaded (/usr/lib/systemd/system/dhcpd.service; disabled; vendor preset: disabled)
   Active: active (running) since dim. 2019-03-10 00:14:18 CET; 1s ago
     Docs: man:dhcpd(8)
           man:dhcpd.conf(5)
 Main PID: 3928 (dhcpd)
   Status: "Dispatching packets..."
   CGroup: /system.slice/dhcpd.service
           └─3928 /usr/sbin/dhcpd -f -cf /etc/dhcp/dhcpd.conf -user dhcpd -group dhcpd --no-pid
mars 10 00:14:18 dhcp-server.net1.b2 dhcpd[3928]: Not searching LDAP since ldap-server, ldap-port and ldap-base-dn were not s...g file
mars 10 00:14:18 dhcp-server.net1.b2 dhcpd[3928]: Internet Systems Consortium DHCP Server 4.2.5
mars 10 00:14:18 dhcp-server.net1.b2 dhcpd[3928]: Copyright 2004-2013 Internet Systems Consortium.
mars 10 00:14:18 dhcp-server.net1.b2 dhcpd[3928]: All rights reserved.
mars 10 00:14:18 dhcp-server.net1.b2 dhcpd[3928]: For info, please visit https://www.isc.org/software/dhcp/
mars 10 00:14:18 dhcp-server.net1.b2 dhcpd[3928]: Wrote 1 leases to leases file.
mars 10 00:14:18 dhcp-server.net1.b2 dhcpd[3928]: Listening on LPF/enp0s3/08:00:27:e3:87:b0/10.2.1.0/24
mars 10 00:14:18 dhcp-server.net1.b2 dhcpd[3928]: Sending on   LPF/enp0s3/08:00:27:e3:87:b0/10.2.1.0/24
mars 10 00:14:18 dhcp-server.net1.b2 dhcpd[3928]: Sending on   Socket/fallback/fallback-net
mars 10 00:14:18 dhcp-server.net1.b2 systemd[1]: Started DHCPv4 Server Daemon.
Hint: Some lines were ellipsized, use -l to show in full.
```

=> On a désormais un serveur DHCP qui va permettre aux <b>clients</b> de récupérer une ip et l'adresse de la passerelle directement dans net1.

- On configure une nouvelle machine nommée <b>`client2`</b> puis on s'assure que ça a bien fonctionné :

```
[soussou@client2 ~]$ ip a
1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN group default qlen 1000
    link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
    inet 127.0.0.1/8 scope host lo
       valid_lft forever preferred_lft forever
    inet6 ::1/128 scope host
       valid_lft forever preferred_lft forever
2: enp0s3: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc pfifo_fast state UP group default qlen 1000
    link/ether 08:00:27:cb:85:33 brd ff:ff:ff:ff:ff:ff
    inet 10.2.1.50/24 brd 10.2.1.255 scope global noprefixroute dynamic enp0s3
       valid_lft 572sec preferred_lft 572sec
    inet6 fe80::a00:27ff:fecb:8533/64 scope link
       valid_lft forever preferred_lft forever
       
[soussou@client2 ~]$ ip r s
default via 10.2.1.254 dev enp0s3 proto dhcp metric 100
```
*****
### 3. NTP Server

- Une fois <b>`chrony`</b> mis en place :

	- `router1`
	
	```
	[jdinh@router1 ~]$ chronyc sources
	210 Number of sources = 4
	MS Name/IP address         Stratum Poll Reach LastRx Last sample
	===============================================================================
	^? ns3.stoneartprod.xyz          2   6     1     7   +226us[ +226us] +/-   52ms
	^? eva.aplu.fr                   2   6     1     6  +1768us[+1768us] +/-   69ms
	^? ks312903.kimsufi.com          3   6     1     6   -871us[ -871us] +/-   65ms
	^? v.bsod.fr                     2   6     1     8    -32ms[  -32ms] +/-   90ms
	
	[jdinh@router1 ~]$ chronyc tracking
	Reference ID    : 7F7F0101 ()
	Stratum         : 10
	Ref time (UTC)  : Sun Mar 10 15:57:53 2019
	System time     : 0.000000000 seconds fast of NTP time
	Last offset     : +0.000000000 seconds
	RMS offset      : 0.000000000 seconds
	Frequency       : 1.837 ppm fast
	Residual freq   : +0.000 ppm
	Skew            : 0.000 ppm
	Root delay      : 0.000000000 seconds
	Root dispersion : 0.000000000 seconds
	Update interval : 0.0 seconds
	Leap status     : Normal
	```

	- `router2`
	
	```
	[jdinh@router2 ~]$ chronyc sources
	210 Number of sources = 0
	MS Name/IP address         Stratum Poll Reach LastRx Last sample
	===============================================================================

	[jdinh@router2 ~]$ chronyc tracking
	Reference ID    : 7F7F0101 ()
	Stratum         : 10
	Ref time (UTC)  : Sun Mar 10 16:01:29 2019
	System time     : 0.000000000 seconds fast of NTP time
	Last offset     : +0.000000000 seconds
	RMS offset      : 0.000000000 seconds
	Frequency       : 0.000 ppm slow
	Residual freq   : +0.000 ppm
	Skew            : 0.000 ppm
	Root delay      : 0.000000000 seconds
	Root dispersion : 0.000000000 seconds
	Update interval : 0.0 seconds
	Leap status     : Normal
	```

	- `server1`
	
	```
	[jdinh@server1 ~]$ chronyc sources
	210 Number of sources = 0
	MS Name/IP address         Stratum Poll Reach LastRx Last sample
	===============================================================================

	[jdinh@server1 ~]$ chronyc tracking
	Reference ID    : 7F7F0101 ()
	Stratum         : 10
	Ref time (UTC)  : Sun Mar 10 16:01:46 2019
	System time     : 0.000000000 seconds fast of NTP time
	Last offset     : +0.000000000 seconds
	RMS offset      : 0.000000000 seconds
	Frequency       : 0.000 ppm slow
	Residual freq   : +0.000 ppm
	Skew            : 0.000 ppm
	Root delay      : 0.000000000 seconds
	Root dispersion : 0.000000000 seconds
	Update interval : 0.0 seconds
	Leap status     : Normal
	```

*****
### 4. Web Server

Nos clients peuvent **`curl`** le **`server1`** :

- sur `dhcp-server`:

```
[jdinh@dhcp-server ~]$ curl server1
<!DOCTYPE html PUBLIC "-//W3C//DTD XHTML 1.1//EN" "http://www.w3.org/TR/xhtml11/DTD/xhtml11.dtd">

<html xmlns="http://www.w3.org/1999/xhtml" xml:lang="en">
    <head>
        <title>Test Page for the Nginx HTTP Server on Fedora</title>
        <meta http-equiv="Content-Type" content="text/html; charset=UTF-8" />
        <style type="text/css">
            /*<![CDATA[*/
            body {
                background-color: #fff;
                color: #000;
                font-size: 0.9em;
                font-family: sans-serif,helvetica;
                margin: 0;
                padding: 0;
            }
            :link {
                color: #c00;
            }
            :visited {
                color: #c00;
            }
            a:hover {
                color: #f50;
            }
            h1 {
                text-align: center;
                margin: 0;
                padding: 0.6em 2em 0.4em;
                background-color: #294172;
                color: #fff;
                font-weight: normal;
                font-size: 1.75em;
                border-bottom: 2px solid #000;
            }
            h1 strong {
                font-weight: bold;
                font-size: 1.5em;
            }
            h2 {
                text-align: center;
                background-color: #3C6EB4;
                font-size: 1.1em;
                font-weight: bold;
                color: #fff;
                margin: 0;
                padding: 0.5em;
                border-bottom: 2px solid #294172;
            }
            hr {
                display: none;
            }
            .content {
                padding: 1em 5em;
            }
            .alert {
                border: 2px solid #000;
            }

            img {
                border: 2px solid #fff;
                padding: 2px;
                margin: 2px;
            }
            a:hover img {
                border: 2px solid #294172;
            }
            .logos {
                margin: 1em;
                text-align: center;
            }
            /*]]>*/
        </style>
    </head>

    <body>
        <h1>Welcome to <strong>nginx</strong> on Fedora!</h1>

        <div class="content">
            <p>This page is used to test the proper operation of the
            <strong>nginx</strong> HTTP server after it has been
            installed. If you can read this page, it means that the
            web server installed at this site is working
            properly.</p>

            <div class="alert">
                <h2>Website Administrator</h2>
                <div class="content">
                    <p>This is the default <tt>index.html</tt> page that
                    is distributed with <strong>nginx</strong> on
                    Fedora.  It is located in
                    <tt>/usr/share/nginx/html</tt>.</p>

                    <p>You should now put your content in a location of
                    your choice and edit the <tt>root</tt> configuration
                    directive in the <strong>nginx</strong>
                    configuration file
                    <tt>/etc/nginx/nginx.conf</tt>.</p>

                </div>
            </div>

            <div class="logos">
                <a href="http://nginx.net/"><img
                    src="nginx-logo.png"
                    alt="[ Powered by nginx ]"
                    width="121" height="32" /></a>

                <a href="http://fedoraproject.org/"><img
                    src="poweredby.png"
                    alt="[ Powered by Fedora ]"
                    width="88" height="31" /></a>
            </div>
        </div>
    </body>
</html>
```

- sur `client2`:

```
[jdinh@client2 ~]$ curl server1
<!DOCTYPE html PUBLIC "-//W3C//DTD XHTML 1.1//EN" "http://www.w3.org/TR/xhtml11/DTD/xhtml11.dtd">

<html xmlns="http://www.w3.org/1999/xhtml" xml:lang="en">
    <head>
        <title>Test Page for the Nginx HTTP Server on Fedora</title>
        <meta http-equiv="Content-Type" content="text/html; charset=UTF-8" />
        <style type="text/css">
            /*<![CDATA[*/
            body {
                background-color: #fff;
                color: #000;
                font-size: 0.9em;
                font-family: sans-serif,helvetica;
                margin: 0;
                padding: 0;
            }
            :link {
                color: #c00;
            }
            :visited {
                color: #c00;
            }
            a:hover {
                color: #f50;
            }
            h1 {
                text-align: center;
                margin: 0;
                padding: 0.6em 2em 0.4em;
                background-color: #294172;
                color: #fff;
                font-weight: normal;
                font-size: 1.75em;
                border-bottom: 2px solid #000;
            }
            h1 strong {
                font-weight: bold;
                font-size: 1.5em;
            }
            h2 {
                text-align: center;
                background-color: #3C6EB4;
                font-size: 1.1em;
                font-weight: bold;
                color: #fff;
                margin: 0;
                padding: 0.5em;
                border-bottom: 2px solid #294172;
            }
            hr {
                display: none;
            }
            .content {
                padding: 1em 5em;
            }
            .alert {
                border: 2px solid #000;
            }

            img {
                border: 2px solid #fff;
                padding: 2px;
                margin: 2px;
            }
            a:hover img {
                border: 2px solid #294172;
            }
            .logos {
                margin: 1em;
                text-align: center;
            }
            /*]]>*/
        </style>
    </head>

    <body>
        <h1>Welcome to <strong>nginx</strong> on Fedora!</h1>

        <div class="content">
            <p>This page is used to test the proper operation of the
            <strong>nginx</strong> HTTP server after it has been
            installed. If you can read this page, it means that the
            web server installed at this site is working
            properly.</p>

            <div class="alert">
                <h2>Website Administrator</h2>
                <div class="content">
                    <p>This is the default <tt>index.html</tt> page that
                    is distributed with <strong>nginx</strong> on
                    Fedora.  It is located in
                    <tt>/usr/share/nginx/html</tt>.</p>

                    <p>You should now put your content in a location of
                    your choice and edit the <tt>root</tt> configuration
                    directive in the <strong>nginx</strong>
                    configuration file
                    <tt>/etc/nginx/nginx.conf</tt>.</p>

                </div>
            </div>

            <div class="logos">
                <a href="http://nginx.net/"><img
                    src="nginx-logo.png"
                    alt="[ Powered by nginx ]"
                    width="121" height="32" /></a>

                <a href="http://fedoraproject.org/"><img
                    src="poweredby.png"
                    alt="[ Powered by Fedora ]"
                    width="88" height="31" /></a>
            </div>
        </div>
    </body>
</html>
```
