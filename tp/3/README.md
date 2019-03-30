# TP3 - Utilisation de matériel Cisco

**Auteur :** DINH Jonathan & HERNANDEZ Theo & SEGHIR Souleimane<br />
**Date :** 11/03/2019<br />
**Desc :** Rendu du 3nd tp en CCNA

## I. Manipulation de switches et VLAN
### 1. Mise en place du lab

- Vérification

  - `client1` :
  
  ```
  [jdinh@client1 ~]$ ping client2 -c 2
  PING client2 (10.1.1.2) 56(84) bytes of data.
  64 bytes from client2 (10.1.1.2): icmp_seq=1 ttl=64 time=1.33 ms
  64 bytes from client2 (10.1.1.2): icmp_seq=2 ttl=64 time=2.97 ms
  
  --- client2 ping statistics ---
  2 packets transmitted, 2 received, 0% packet loss, time 1002ms
  rtt min/avg/max/mdev = 1.337/2.154/2.971/0.817 ms
  
  [jdinh@client1 ~]$ ping client3 -c 2
  PING client3 (10.1.1.3) 56(84) bytes of data.
  64 bytes from client3 (10.1.1.3): icmp_seq=1 ttl=64 time=4.31 ms
  64 bytes from client3 (10.1.1.3): icmp_seq=2 ttl=64 time=3.41 ms
  
  --- client3 ping statistics ---
  2 packets transmitted, 2 received, 0% packet loss, time 1004ms
  rtt min/avg/max/mdev = 3.416/3.865/4.315/0.453 ms
  ```
  
  - `client2` :
  
  ```
  [jdinh@client2 ~]$ ping client1 -c2
  PING client1 (10.1.1.1) 56(84) bytes of data.
  64 bytes from client1 (10.1.1.1): icmp_seq=1 ttl=64 time=1.31 ms
  64 bytes from client1 (10.1.1.1): icmp_seq=2 ttl=64 time=2.82 ms
  
  --- client1 ping statistics ---
  2 packets transmitted, 2 received, 0% packet loss, time 1003ms
  rtt min/avg/max/mdev = 1.315/2.067/2.820/0.753 ms

  [jdinh@client2 ~]$ ping client3 -c2
  PING client3 (10.1.1.3) 56(84) bytes of data.
  64 bytes from client3 (10.1.1.3): icmp_seq=1 ttl=64 time=4.24 ms
  64 bytes from client3 (10.1.1.3): icmp_seq=2 ttl=64 time=3.26 ms
  
  --- client3 ping statistics ---
  2 packets transmitted, 2 received, 0% packet loss, time 1002ms
  rtt min/avg/max/mdev = 3.267/3.757/4.248/0.494 ms
  ```
  
  - `client3` :
  
  ```
  [jdinh@client3 ~]$ ping client1 -c 2
  PING client1 (10.1.1.1) 56(84) bytes of data.
  64 bytes from client1 (10.1.1.1): icmp_seq=1 ttl=64 time=1.44 ms
  64 bytes from client1 (10.1.1.1): icmp_seq=2 ttl=64 time=3.45 ms
  
  --- client1 ping statistics ---
  2 packets transmitted, 2 received, 0% packet loss, time 1002ms
  rtt min/avg/max/mdev = 1.448/2.451/3.455/1.004 ms

  [jdinh@client3 ~]$ ping client2 -c 2
  PING client2 (10.1.1.2) 56(84) bytes of data.
  64 bytes from client2 (10.1.1.2): icmp_seq=1 ttl=64 time=1.46 ms
  64 bytes from client2 (10.1.1.2): icmp_seq=2 ttl=64 time=3.52 ms
  
  --- client2 ping statistics ---
  2 packets transmitted, 2 received, 0% packet loss, time 1002ms
  rtt min/avg/max/mdev = 1.465/2.493/3.522/1.029 ms
  ```
  
### 2. Configuration des VLANs

- Vérification :
  
  - `client1` :
  
  ```
  [jdinh@client1 ~]$ traceroute client2
  traceroute to client2 (10.1.1.2), 30 hops max, 60 byte packets
   1  client1 (10.1.1.1)  3005.799 ms !H  3005.630 ms !H  3005.606 ms !H
  
  [jdinh@client1 ~]$ traceroute client3
  traceroute to client3 (10.1.1.3), 30 hops max, 60 byte packets
  1  client3 (10.1.1.3)  3.846 ms !X  3.916 ms !X  3.912 ms !X
  ```
  
  - `client2` :
  
  ```
  [jdinh@client2 ~]$ traceroute client1
  traceroute to client1 (10.1.1.1), 30 hops max, 60 byte packets
   1  * * *
   2  * * *
   3  * * *
   4  * * *
   5  * * *
   6  * * *
   7  * * *
   8  * * *
   9  * * *
  10  * * *
  11  * * client2 (10.1.1.2)  3010.349 ms !H

  [jdinh@client2 ~]$ traceroute client3
  traceroute to client3 (10.1.1.3), 30 hops max, 60 byte packets
   1  * * *
   2  * * *
   3  * * *
   4  * * *
   5  * * *
   6  * * *
   7  * * *
   8  * * *
   9  * * *
  10  * * *
  11  * * client2 (10.1.1.2)  3005.006 ms !H
  ```
  
  - `client3` :
  
  ```
  [jdinh@client3 ~]$ traceroute client1
  traceroute to client1 (10.1.1.1), 30 hops max, 60 byte packets
   1  client1 (10.1.1.1)  4.767 ms !X  4.726 ms !X  4.766 ms !X

  [jdinh@client3 ~]$ traceroute client2
  traceroute to client2 (10.1.1.2), 30 hops max, 60 byte packets
   1  * * *
   2  * * *
   3  * * *
   4  * * *
   5  * * *
   6  * * *
   7  * * *
   8  * * *
   9  * * *
  10  * * *
  11  * * client3 (10.1.1.3)  3005.788 ms !H
  ```
  
## II. Manipulation simple de routeurs
### 1. Mise en place du lab

- `client2`:

```
[jdinh@client2 ~]$ ping 10.2.1.254 -c 2
PING 10.2.1.254 (10.2.1.254) 56(84) bytes of data.
64 bytes from 10.2.1.254: icmp_seq=1 ttl=255 time=13.1 ms
64 bytes from 10.2.1.254: icmp_seq=2 ttl=255 time=6.18 ms

--- 10.2.1.254 ping statistics ---
2 packets transmitted, 2 received, 0% packet loss, time 1002ms
rtt min/avg/max/mdev = 6.180/9.651/13.123/3.472 ms
```

- `server1`:

```
[jdinh@server1 ~]$ ping 10.2.2.254 -c 2
PING 10.2.2.254 (10.2.2.254) 56(84) bytes of data.
64 bytes from 10.2.2.254: icmp_seq=1 ttl=255 time=11.3 ms
64 bytes from 10.2.2.254: icmp_seq=2 ttl=255 time=7.68 ms

--- 10.2.2.254 ping statistics ---
2 packets transmitted, 2 received, 0% packet loss, time 1002ms
rtt min/avg/max/mdev = 7.684/9.536/11.389/1.855 ms
```

- `router1`:

```
R1#ping 10.2.1.11

Type escape sequence to abort.
Sending 5, 100-byte ICMP Echos to 10.2.1.11, timeout is 2 seconds:
!!!!!
Success rate is 100 percent (5/5), round-trip min/avg/max = 8/10/16 ms
```

- `router2`:

```
R2#ping 10.2.2.10

Type escape sequence to abort.
Sending 5, 100-byte ICMP Echos to 10.2.2.10, timeout is 2 seconds:
!!!!!
Success rate is 100 percent (5/5), round-trip min/avg/max = 1/9/16 ms
```

### 2. Configuration du routage statique

- `client2` :

```
[jdinh@client2 ~]$ ping 10.2.2.10 -c 2
PING 10.2.2.10 (10.2.2.10) 56(84) bytes of data.
64 bytes from 10.2.2.10: icmp_seq=1 ttl=62 time=24.8 ms
64 bytes from 10.2.2.10: icmp_seq=2 ttl=62 time=29.2 ms

--- 10.2.2.10 ping statistics ---
2 packets transmitted, 2 received, 0% packet loss, time 1003ms
rtt min/avg/max/mdev = 24.884/27.067/29.250/2.183 ms
```

- `server1` :

```
[jdinh@server1 ~]$ ping 10.2.1.11 -c 2
PING 10.2.1.11 (10.2.1.11) 56(84) bytes of data.
64 bytes from 10.2.1.11: icmp_seq=1 ttl=62 time=23.6 ms
64 bytes from 10.2.1.11: icmp_seq=2 ttl=62 time=31.5 ms

--- 10.2.1.11 ping statistics ---
2 packets transmitted, 2 received, 0% packet loss, time 1002ms
rtt min/avg/max/mdev = 23.642/27.607/31.572/3.965 ms
```

- Proposition de topologie :

On rajoute un switch entre `client1` et `client2`

![Screenshot_1](https://raw.githubusercontent.com/KyoshinSan/B2-CCNA/master/tp/3/Screenshot_1.png)

## III. Mise en place d'OSPF
### 1. Mise en place du lab

- `client1` :

```
[jdinh@client1 ~]$ ping 10.3.101.254 -c 2
PING 10.3.101.254 (10.3.101.254) 56(84) bytes of data.
64 bytes from 10.3.101.254: icmp_seq=1 ttl=255 time=33.6 ms
64 bytes from 10.3.101.254: icmp_seq=2 ttl=255 time=13.5 ms

--- 10.3.101.254 ping statistics ---
2 packets transmitted, 2 received, 0% packet loss, time 1002ms
rtt min/avg/max/mdev = 13.521/23.606/33.691/10.085 ms
```

- `server1` :

```
[jdinh@server1 ~]$ ping 10.3.102.254 -c 2
PING 10.3.102.254 (10.3.102.254) 56(84) bytes of data.
64 bytes from 10.3.102.254: icmp_seq=1 ttl=255 time=40.9 ms
64 bytes from 10.3.102.254: icmp_seq=2 ttl=255 time=12.4 ms

--- 10.3.102.254 ping statistics ---
2 packets transmitted, 2 received, 0% packet loss, time 1002ms
rtt min/avg/max/mdev = 12.477/26.735/40.994/14.259 ms
```

- `router1` :

```
R1#ping 10.3.102.10

Type escape sequence to abort.
Sending 5, 100-byte ICMP Echos to 10.3.102.10, timeout is 2 seconds:
!!!!!
Success rate is 100 percent (5/5), round-trip min/avg/max = 8/11/16 ms

R1#ping 10.3.100.2

Type escape sequence to abort.
Sending 5, 100-byte ICMP Echos to 10.3.100.2, timeout is 2 seconds:
!!!!!
Success rate is 100 percent (5/5), round-trip min/avg/max = 12/20/32 ms

R1#ping 10.3.100.21

Type escape sequence to abort.
Sending 5, 100-byte ICMP Echos to 10.3.100.21, timeout is 2 seconds:
!!!!!
Success rate is 100 percent (5/5), round-trip min/avg/max = 12/20/32 ms
```

- `router2` :

```
R2#ping 10.3.100.1

Type escape sequence to abort.
Sending 5, 100-byte ICMP Echos to 10.3.100.1, timeout is 2 seconds:
!!!!!
Success rate is 100 percent (5/5), round-trip min/avg/max = 12/27/44 ms

R2#ping 10.3.100.6

Type escape sequence to abort.
Sending 5, 100-byte ICMP Echos to 10.3.100.6, timeout is 2 seconds:
!!!!!
Success rate is 100 percent (5/5), round-trip min/avg/max = 12/23/32 ms
```

- `router3` :

```
R3#ping 10.3.100.5

Type escape sequence to abort.
Sending 5, 100-byte ICMP Echos to 10.3.100.5, timeout is 2 seconds:
!!!!!
Success rate is 100 percent (5/5), round-trip min/avg/max = 8/17/28 ms

R3#ping 10.3.100.10

Type escape sequence to abort.
Sending 5, 100-byte ICMP Echos to 10.3.100.10, timeout is 2 seconds:
!!!!!
Success rate is 100 percent (5/5), round-trip min/avg/max = 16/23/36 ms
```

- `router4` :

```
R4#ping 10.3.100.9

Type escape sequence to abort.
Sending 5, 100-byte ICMP Echos to 10.3.100.9, timeout is 2 seconds:
!!!!!
Success rate is 100 percent (5/5), round-trip min/avg/max = 16/31/76 ms

R4#ping 10.3.101.10

Type escape sequence to abort.
Sending 5, 100-byte ICMP Echos to 10.3.101.10, timeout is 2 seconds:
!!!!!
Success rate is 100 percent (5/5), round-trip min/avg/max = 4/9/20 ms

R4#ping 10.3.100.14

Type escape sequence to abort.
Sending 5, 100-byte ICMP Echos to 10.3.100.14, timeout is 2 seconds:
!!!!!
Success rate is 100 percent (5/5), round-trip min/avg/max = 12/25/44 ms
```

- `router5` :

```
R5#ping 10.3.100.13

Type escape sequence to abort.
Sending 5, 100-byte ICMP Echos to 10.3.100.13, timeout is 2 seconds:
!!!!!
Success rate is 100 percent (5/5), round-trip min/avg/max = 12/32/72 ms

R5#ping 10.3.100.18

Type escape sequence to abort.
Sending 5, 100-byte ICMP Echos to 10.3.100.18, timeout is 2 seconds:
!!!!!
Success rate is 100 percent (5/5), round-trip min/avg/max = 8/15/36 ms
```

- `router6` :

```
R6#ping 10.3.100.17

Type escape sequence to abort.
Sending 5, 100-byte ICMP Echos to 10.3.100.17, timeout is 2 seconds:
!!!!!
Success rate is 100 percent (5/5), round-trip min/avg/max = 4/12/16 ms

R6#ping 10.3.100.22

Type escape sequence to abort.
Sending 5, 100-byte ICMP Echos to 10.3.100.22, timeout is 2 seconds:
!!!!!
Success rate is 100 percent (5/5), round-trip min/avg/max = 4/14/24 ms
```

### 2. Configuration de OSPF

- `router1`: 

```
R1#ping 10.3.100.2

Type escape sequence to abort.
Sending 5, 100-byte ICMP Echos to 10.3.100.2, timeout is 2 seconds:
!!!!!
Success rate is 100 percent (5/5), round-trip min/avg/max = 16/20/24 ms

R1#ping 10.3.100.6

Type escape sequence to abort.
Sending 5, 100-byte ICMP Echos to 10.3.100.6, timeout is 2 seconds:
!!!!!
Success rate is 100 percent (5/5), round-trip min/avg/max = 36/41/44 ms

R1#ping 10.3.100.10

Type escape sequence to abort.
Sending 5, 100-byte ICMP Echos to 10.3.100.10, timeout is 2 seconds:
!!!!!
Success rate is 100 percent (5/5), round-trip min/avg/max = 52/68/108 ms

R1#ping 10.3.100.14

Type escape sequence to abort.
Sending 5, 100-byte ICMP Echos to 10.3.100.14, timeout is 2 seconds:
!!!!!
Success rate is 100 percent (5/5), round-trip min/avg/max = 40/43/44 ms

R1#ping 10.3.100.18

Type escape sequence to abort.
Sending 5, 100-byte ICMP Echos to 10.3.100.18, timeout is 2 seconds:
!!!!!
Success rate is 100 percent (5/5), round-trip min/avg/max = 20/20/24 ms

```

- `client1` :

```
[jdinh@client1 ~]$ ping server1 -c 2
PING server1 (10.3.102.10) 56(84) bytes of data.
64 bytes from server1 (10.3.102.10): icmp_seq=1 ttl=60 time=77.1 ms
64 bytes from server1 (10.3.102.10): icmp_seq=2 ttl=60 time=85.2 ms

--- server1 ping statistics ---
2 packets transmitted, 2 received, 0% packet loss, time 1003ms
rtt min/avg/max/mdev = 77.154/81.201/85.249/4.057 ms
```

- `server1` :

```
[jdinh@server1 ~]$ ping client1 -c 2
PING client1 (10.3.101.10) 56(84) bytes of data.
64 bytes from client1 (10.3.101.10): icmp_seq=1 ttl=60 time=87.7 ms
64 bytes from client1 (10.3.101.10): icmp_seq=2 ttl=60 time=78.9 ms

--- client1 ping statistics ---
2 packets transmitted, 2 received, 0% packet loss, time 1002ms
rtt min/avg/max/mdev = 78.925/83.354/87.783/4.429 ms
```

## IV. Lab Final
### Mise en place du lab

> Topologie

![Image de la topologie](https://github.com/KyoshinSan/B2-CCNA/blob/master/tp/3/pic/topologie-lab-4.png)

> Tableau d'adressage

> Vérification
