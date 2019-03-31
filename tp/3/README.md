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
### 1.Mise en place du lab

- Topologie
> le router2 a aussi pour role DHCP pour les clients 4 et 5

![Image de la topologie](https://github.com/KyoshinSan/B2-CCNA/blob/master/tp/3/pic/topologie-lab-4.jpg)

-  Tableau d'adressage

| Hosts | `10.4.1.0/30` | `10.4.1.4/30` |	`10.4.1.8/30` |	`10.4.10.0/24` | `10.4.20.0/24` | `10.4.30.0/24` |	`10.4.40.0/24` |
| ---------------- | ----------- | ----------- | ------------ | ------------ | ------------ | ------------ | --------------- | 
| `client1.lab4.tp3` | X           | X           | X            | `10.4.10.1/24` | X            | X            | X |
| `client2.lab4.tp3` | X           | X           | X            | `10.4.10.2/24` | X            | X            | X |
| `client3.lab4.tp3` | X           | X           | X            | X            | `10.4.20.1/24`           | X            | X |
| `client4.lab4.tp3` | X           | X           | X            | X            | X            | X | `10.4.40.101/24` |
| `client5.lab4.tp3` | X           | X           | X            | X            | X           | X | `10.4.40.102/24` |
| `server1.lab4.tp3` | X           | X           | X            | X           | X  | `10.4.30.1/24` | X            |
| `server2.lab4.tp3` | X           | X           | X            | X           | X | `10.4.30.2/24` | X            |
| `router1.lab4.tp3` | `10.4.1.1/30` | X           | `10.4.1.10/30` | X            | X            | X | X |   
| `router2.lab4.tp3` | `10.4.1.2/30` | `10.4.1.5/30` | X            | X            | X    | X       | `10.4.40.254/24` |
| `router3.lab4.tp3` | X | `10.4.1.6/30` | `10.4.1.9/30` | `10.4.10.254/24` | `10.4.20.254/24` | `10.4.30.254/24` | X |



-  Vérification

- [x] tous les serveurs peuvent se ping entre eux :

    - `server1`
  ```
  [jdinh@server1 ~]$ ping server2 -c 2
  PING server2 (10.4.30.2) 56(84) bytes of data.
  64 bytes from server2 (10.4.30.2): icmp_seq=1 ttl=64 time=1.68 ms
  64 bytes from server2 (10.4.30.2): icmp_seq=2 ttl=64 time=6.60 ms
  
  --- server2 ping statistics ---
  2 packets transmitted, 2 received, 0% packet loss, time 1002ms
  rtt min/avg/max/mdev = 1.680/4.142/6.604/2.462 ms
  ```
    - `server2`
  ```
  [jdinh@server2 ~]$ ping server1 -c 2
  PING server1 (10.4.30.1) 56(84) bytes of data.
  64 bytes from server1 (10.4.30.1): icmp_seq=1 ttl=64 time=1.81 ms
  64 bytes from server1 (10.4.30.1): icmp_seq=2 ttl=64 time=3.59 ms
  
  --- server1 ping statistics ---
  2 packets transmitted, 2 received, 0% packet loss, time 1001ms
  rtt min/avg/max/mdev = 1.812/2.702/3.593/0.892 ms
  ```
  
- [x] les clients 1 et 2 peuvent se ping entre eux :

    - `client1`
  ```
  [jdinh@client1 ~]$ ping client2 -c 2
  PING client2 (10.4.10.2) 56(84) bytes of data.
  64 bytes from client2 (10.4.10.2): icmp_seq=1 ttl=64 time=6.24 ms
  64 bytes from client2 (10.4.10.2): icmp_seq=2 ttl=64 time=4.70 ms
  
  --- client2 ping statistics ---
  2 packets transmitted, 2 received, 0% packet loss, time 1002ms
  rtt min/avg/max/mdev = 4.703/5.473/6.244/0.774 ms
  ```
    - `client2`
  ```
  [jdinh@client2 ~]$ ping client1 -c 2
  PING client1 (10.4.10.1) 56(84) bytes of data.
  64 bytes from client1 (10.4.10.1): icmp_seq=1 ttl=64 time=1.53 ms
  64 bytes from client1 (10.4.10.1): icmp_seq=2 ttl=64 time=3.35 ms
  
  --- client1 ping statistics ---
  2 packets transmitted, 2 received, 0% packet loss, time 1001ms
  rtt min/avg/max/mdev = 1.531/2.444/3.357/0.913 ms
  ```

- [x] les routeurs peuvent discuter entre eux (de point à point):

    - `router1`
  ```
  R1#ping 10.4.1.2
  
  Type escape sequence to abort.
  Sending 5, 100-byte ICMP Echos to 10.4.1.2, timeout is 2 seconds:
  .!!!!
  Success rate is 80 percent (4/5), round-trip min/avg/max = 16/25/32 ms
  R1#ping 10.4.1.9
  
  Type escape sequence to abort.
  Sending 5, 100-byte ICMP Echos to 10.4.1.9, timeout is 2 seconds:
  .!!!!
  Success rate is 80 percent (4/5), round-trip min/avg/max = 16/22/24 ms
  ```
  
    - `router2`
  ```
  R2#ping 10.4.1.1
  
  Type escape sequence to abort.
  Sending 5, 100-byte ICMP Echos to 10.4.1.1, timeout is 2 seconds:
  !!!!!
  Success rate is 100 percent (5/5), round-trip min/avg/max = 12/18/32 ms
  R2#ping 10.4.1.6
  
  Type escape sequence to abort.
  Sending 5, 100-byte ICMP Echos to 10.4.1.6, timeout is 2 seconds:
  .!!!!
  Success rate is 80 percent (4/5), round-trip min/avg/max = 12/17/20 ms
  ```
  
    - `router3`
  ```
  R3#ping 10.4.1.5
  
  Type escape sequence to abort.
  Sending 5, 100-byte ICMP Echos to 10.4.1.5, timeout is 2 seconds:
  !!!!!
  Success rate is 100 percent (5/5), round-trip min/avg/max = 8/25/60 ms
  R3#ping 10.4.1.10
  
  Type escape sequence to abort.
  Sending 5, 100-byte ICMP Echos to 10.4.1.10, timeout is 2 seconds:
  !!!!!
  Success rate is 100 percent (5/5), round-trip min/avg/max = 4/17/44 ms
  ```

### 2. Configuration de OSPF

On s’occupe ensuite de la configuration de OSPF sur les routers 1, 2 et 3.

- [x] Passer en mode configuration d’interface
- [x] Activer OSPF
- [x] Définir un router-id
- [x] Partager une ou plusieurs routes
- [x] Vérifier l’état d’OSPF

- Configuration de OSPF sur le `router1` :

```
R1#conf t
R1(config)#router ospf 1
R1(config-router)#router-id 1.1.1.1
R1(config-router)#network 10.4.1.0 0.0.0.3 area 0
R1(config-router)#network 10.4.1.8 0.0.0.3 area 0
R1#sh ip pro
Routing Protocol is "ospf 1"
  Outgoing update filter list for all interfaces is not set
  Incoming update filter list for all interfaces is not set
  Router ID 1.1.1.1
  Number of areas in this router is 1. 1 normal 0 stub 0 nssa
  Maximum path: 4
  Routing for Networks:
    10.4.1.0 0.0.0.3 area 0
    10.4.1.8 0.0.0.3 area 0
 Reference bandwidth unit is 100 mbps
  Routing Information Sources:
    Gateway         Distance      Last Update
  Distance: (default is 110)


```
Suivre la même procédure sur les autres routeurs.

- `router2` :

```
R2#conf t
R2(config)#router ospf 1
R2(config-router)#router-id 2.2.2.2
R2(config-router)#network 10.4.1.0 0.0.0.3 area 0
R2(config-router)#network 10.4.1.4 0.0.0.3 area 0
R2(config-router)#network 10.4.40.0 0.0.0.255 area 2
R2#sh ip pro
Routing Protocol is "ospf 1"
  Outgoing update filter list for all interfaces is not set
  Incoming update filter list for all interfaces is not set
  Router ID 2.2.2.2
  Number of areas in this router is 2. 2 normal 0 stub 0 nssa
  Maximum path: 4
  Routing for Networks:
    10.4.1.0 0.0.0.3 area 0
    10.4.1.4 0.0.0.3 area 0
    10.4.40.0 0.0.0.255 area 2
 Reference bandwidth unit is 100 mbps
  Routing Information Sources:
    Gateway         Distance      Last Update
    1.1.1.1              110      00:09:48
  Distance: (default is 110)
```

- `router3` :

```
R3#conf t
R3(config)#router ospf 1
R3(config-router)#router-id 3.3.3.3
R3(config-router)#network 10.4.1.4 0.0.0.3 area 0
R3(config-router)#network 10.4.1.8 0.0.0.3 area 0
R3(config-router)#network 10.4.10.0 0.0.0.255 area 1
R3(config-router)#network 10.4.20.0 0.0.0.255 area 1
R3(config-router)#network 10.4.30.0 0.0.0.255 area 1
R3#sh ip pro
Routing Protocol is "ospf 1"
  Outgoing update filter list for all interfaces is not set
  Incoming update filter list for all interfaces is not set
  Router ID 3.3.3.3
  Number of areas in this router is 2. 2 normal 0 stub 0 nssa
  Maximum path: 4
  Routing for Networks:
    10.4.1.4 0.0.0.3 area 0
    10.4.1.8 0.0.0.3 area 0
    10.4.10.0 0.0.0.255 area 1
    10.4.20.0 0.0.0.255 area 1
    10.4.30.0 0.0.0.255 area 1
 Reference bandwidth unit is 100 mbps
  Routing Information Sources:
    Gateway         Distance      Last Update
    1.1.1.1              110      00:09:11
  Distance: (default is 110)
```

- Vérification :

- [x] tous les routeurs peuvent se joindre :

    - `router1`
```
R1#ping 10.4.1.5

Type escape sequence to abort.
Sending 5, 100-byte ICMP Echos to 10.4.1.5, timeout is 2 seconds:
!!!!!
Success rate is 100 percent (5/5), round-trip min/avg/max = 16/22/28 ms
R1#ping 10.4.1.6

Type escape sequence to abort.
Sending 5, 100-byte ICMP Echos to 10.4.1.6, timeout is 2 seconds:
!!!!!
Success rate is 100 percent (5/5), round-trip min/avg/max = 12/20/28 ms
```

- [ ] tous les clients peuvent joindre les serveurs (valider a la fin du tp)

**********

### 3. Configuration d'un routeur Cisco avec le rôle de serveur DHCP

On va ajoute un rôle au `router2`, celui de DHCP. Il s'occupera de distribuer les ip pour les clients 4 et 5 de façon dynamique.

- Sur `router2` :

- [x] Passer en mode configuration d’interface
- [x] Activer le DHCP
- [x] Définir l'adresse réseau
- [x] Définir un serveur dns
- [x] Définir la gateway par défault
- [x] Exclure une plage d'ip

```
R2#conf t
R2(config)#ip dhcp pool CLIENT_LAN40
R2(dhcp-config)#network 10.4.40.0 255.255.255.0
R2(dhcp-config)#dns-server 8.8.8.8
R2(dhcp-config)#default-router 10.4.40.254
R2(config)#ip dhcp excluded-address 10.4.40.1 10.4.40.100
```

- Sur `client4` :

- [x] enp0s3 doit être en DHCP
- [x] on redémarre enp0s3

```
[jdinh@client4 ~]$ ip a | grep " inet "
    inet 127.0.0.1/8 scope host lo
    inet 10.4.40.101/24 brd 10.4.40.255 scope global noprefixroute dynamic enp0s3
```


- Même chose sur `client5` :

- [x] enp0s3 doit être en DHCP
- [x] on redémarre enp0s3

```
[jdinh@client5 ~]$ ip a | grep " inet "
    inet 127.0.0.1/8 scope host lo
    inet 10.4.40.102/24 brd 10.4.40.255 scope global noprefixroute dynamic enp0s3
```

- Vérification :

- [x] Les clients peuvent joindre la gateway mais aussi se ping entre eux :

- `client4`
```
[jdinh@client4 ~]$ ping 10.4.40.254 -c 2
PING 10.4.40.254 (10.4.40.254) 56(84) bytes of data.
64 bytes from 10.4.40.254: icmp_seq=1 ttl=255 time=3.60 ms
64 bytes from 10.4.40.254: icmp_seq=2 ttl=255 time=6.76 ms

--- 10.4.40.254 ping statistics ---
2 packets transmitted, 2 received, 0% packet loss, time 1001ms
rtt min/avg/max/mdev = 3.609/5.188/6.768/1.581 ms

[jdinh@client4 ~]$ ping 10.4.40.102 -c 2
PING 10.4.40.102 (10.4.40.102) 56(84) bytes of data.
64 bytes from 10.4.40.102: icmp_seq=1 ttl=64 time=3.92 ms
64 bytes from 10.4.40.102: icmp_seq=2 ttl=64 time=1.34 ms

--- 10.4.40.102 ping statistics ---
2 packets transmitted, 2 received, 0% packet loss, time 1001ms
rtt min/avg/max/mdev = 1.340/2.631/3.923/1.292 ms
```
   
- `client5`
```
[jdinh@client5 ~]$ ping 10.4.40.254 -c 2
PING 10.4.40.254 (10.4.40.254) 56(84) bytes of data.
64 bytes from 10.4.40.254: icmp_seq=1 ttl=255 time=20.4 ms
64 bytes from 10.4.40.254: icmp_seq=2 ttl=255 time=10.1 ms

--- 10.4.40.254 ping statistics ---
2 packets transmitted, 2 received, 0% packet loss, time 1001ms
rtt min/avg/max/mdev = 10.173/15.327/20.482/5.155 ms

[jdinh@client5 ~]$ ping 10.4.40.101 -c 2
PING 10.4.40.101 (10.4.40.101) 56(84) bytes of data.
64 bytes from 10.4.40.101: icmp_seq=1 ttl=64 time=1.68 ms
64 bytes from 10.4.40.101: icmp_seq=2 ttl=64 time=1.28 ms

--- 10.4.40.101 ping statistics ---
2 packets transmitted, 2 received, 0% packet loss, time 1001ms
rtt min/avg/max/mdev = 1.280/1.482/1.684/0.202 ms
```

**********

### 4. Configuration des VLANs

On s’attaque maintenant sur la configuration de VLANs sur les switch 1, 2, 3 et 4.

Ici entre `SW1`, `SW2` et `SW3` les ports sont en mode `trunk`.

--> Mise en place du TRUNK entre les routeurs 1 et 2 :
```
SW1(config)#interface ethernet 0/1
SW1(config-if)#switchport trunk encapsulation dot1q
SW1(config-if)#switchport mode trunk
```
```
SW2(config)#interface ethernet 0/0
SW2(config-if)#switchport trunk encapsulation dot1q
SW2(config-if)#switchport mode trunk
```


--> Mise en place du TRUNK entre les routeurs 2 et 3 :
```
SW2(config)#interface ethernet 0/3
SW2(config-if)#switchport trunk encapsulation dot1q
SW2(config-if)#switchport mode trunk
```
```
SW3(config)#interface ethernet 0/0
SW3(config-if)#switchport trunk encapsulation dot1q
SW3(config-if)#switchport mode trunk
```

- `switch1`

Création du VLAN 10.

```
SW1# conf t
SW1(config)# vlan 10
SW1(config-vlan)# name sw-vlan10
SW1(config-vlan)# exit
```

On assigne une interface pour donner accès au VLAN.

  --> `Client1`
```
SW1(config)# interface Ethernet 0/2
SW1(config-if)# switchport mode access
SW1(config-if)# switchport access vlan 10
```

Création du VLAN 30.

```
SW1# conf t
SW1(config)# vlan 30
SW1(config-vlan)# name sw-vlan30
SW1(config-vlan)# exit
```

On assigne une interface pour donner accès au VLAN.

  --> `Server2`
```
SW1(config)# interface Ethernet 0/3
SW1(config-if)# switchport mode access
SW1(config-if)# switchport access vlan 30
```

- `switch2`

Création du VLAN 10.

```
SW2# conf t
SW2(config)# vlan 10
SW2(config-vlan)# name sw-vlan10
SW2(config-vlan)# exit
```

On assigne une interface pour donner accès au VLAN.

  --> `Client 2`
```
SW2(config)# interface Ethernet 0/1
SW2(config-if)# switchport mode access
SW2(config-if)# switchport access vlan 10
```
Création du VLAN 20.

```
SW2# conf t
SW2(config)# vlan 20
SW2(config-vlan)# name sw-vlan20
SW2(config-vlan)# exit
```

  --> `Client 3`
```
SW2(config)# interface Ethernet 0/2
SW2(config-if)# switchport mode access
SW2(config-if)# switchport access vlan 20
```

- `switch3`

Création du VLAN 30.

```
SW3# conf t
SW3(config)# vlan 30
SW3(config-vlan)# name sw-vlan30
SW3(config-vlan)# exit
```

On assigne une interface pour donner accès au VLAN.

  --> `Server1`
```
SW3(config)# interface Ethernet 0/1
SW3(config-if)# switchport mode access
SW3(config-if)# switchport access vlan 30
```

- `switch4`

Création du VLAN 40.

```
SW4# conf t
SW4(config)# vlan 40
SW4(config-vlan)# name sw-vlan40
SW4(config-vlan)# exit
```

On assigne une interface pour donner accès au VLAN.

  --> `Client 4`
```
SW4(config)# interface Ethernet 1/0
SW4(config-if)# switchport mode access
SW4(config-if)# switchport access vlan 40
```
  --> `Client 5`
```
SW4(config)# interface Ethernet 2/0
SW4(config-if)# switchport mode access
SW4(config-if)# switchport access vlan 40
```

  --> `Router 2`
```
SW4(config)# interface Ethernet 0/0
SW4(config-if)# switchport mode access
SW4(config-if)# switchport access vlan 40
```

-> Vérification

- [x] client 1 et 2 peuvent se joindre
- [x] server 1 et 2 peuvent se joindre
- [x] client 3 peut joindre personne

- `client1` et `client2` :

```
[jdinh@client1 ~]$ ping client2 -c 2
PING client2 (10.4.10.2) 56(84) bytes of data.
64 bytes from client2 (10.4.10.2): icmp_seq=1 ttl=64 time=1.55 ms
64 bytes from client2 (10.4.10.2): icmp_seq=2 ttl=64 time=1.64 ms

--- client2 ping statistics ---
2 packets transmitted, 2 received, 0% packet loss, time 1001ms
rtt min/avg/max/mdev = 1.557/1.603/1.649/0.046 ms

[jdinh@client2 ~]$ ping client1 -c 2
PING client1 (10.4.10.1) 56(84) bytes of data.
64 bytes from client1 (10.4.10.1): icmp_seq=1 ttl=64 time=2.77 ms
64 bytes from client1 (10.4.10.1): icmp_seq=2 ttl=64 time=1.42 ms

--- client1 ping statistics ---
2 packets transmitted, 2 received, 0% packet loss, time 1002ms
rtt min/avg/max/mdev = 1.426/2.101/2.777/0.677 ms
```

- `server1` et `server2`

```
[jdinh@server1 ~]$ ping server2 -c 2
PING server2 (10.4.30.2) 56(84) bytes of data.
64 bytes from server2 (10.4.30.2): icmp_seq=1 ttl=64 time=2.64 ms
64 bytes from server2 (10.4.30.2): icmp_seq=2 ttl=64 time=4.68 ms

--- server2 ping statistics ---
2 packets transmitted, 2 received, 0% packet loss, time 1003ms
rtt min/avg/max/mdev = 2.643/3.664/4.686/1.023 ms

[jdinh@server2 ~]$ ping server1 -c 2
PING server1 (10.4.30.1) 56(84) bytes of data.
64 bytes from server1 (10.4.30.1): icmp_seq=1 ttl=64 time=1.84 ms
64 bytes from server1 (10.4.30.1): icmp_seq=2 ttl=64 time=3.28 ms

--- server1 ping statistics ---
2 packets transmitted, 2 received, 0% packet loss, time 1001ms
rtt min/avg/max/mdev = 1.845/2.567/3.289/0.722 ms
```

- `client3`

```
[jdinh@client3 ~]$ ping client1 -c 2
PING client1 (10.4.10.1) 56(84) bytes of data.
From client3 (10.4.20.1) icmp_seq=1 Destination Host Unreachable
From client3 (10.4.20.1) icmp_seq=2 Destination Host Unreachable

--- client1 ping statistics ---
2 packets transmitted, 0 received, +2 errors, 100% packet loss, time 1001ms
pipe 2
[jdinh@client3 ~]$ ping client2 -c 2
PING client2 (10.4.10.2) 56(84) bytes of data.
From client3 (10.4.20.1) icmp_seq=1 Destination Host Unreachable
From client3 (10.4.20.1) icmp_seq=2 Destination Host Unreachable

--- client2 ping statistics ---
2 packets transmitted, 0 received, +2 errors, 100% packet loss, time 1000ms
pipe 2
```

**********

### 5. Mise en place de la NAT

On va permettre aux clients du réseau (client et serveurs) d'accéder à Internet grâce à la fonctionnalité NAT.

- [x] récupérer une ip pour faire du NAT
- [x] définir les interfaces internes et externes
- [x] activer le NAT et créer une access-list
- [x] diffuser la route par défault par OSPF

- `Router1`

--> Récupérer une ip pour faire du NAT

```
R1#conf t
R1(config)#int fastEthernet 0/0
R1(config-if)#ip address dhcp
R1(config-if)#no shut

# ensuite on vérifie si l'on peut aller sur internet
R1#ping 8.8.8.8

Type escape sequence to abort.
Sending 5, 100-byte ICMP Echos to 8.8.8.8, timeout is 2 seconds:
!!!!!
Success rate is 100 percent (5/5), round-trip min/avg/max = 60/62/68 ms
```

--> Passage en mode configuration puis définition des interfaces internes pour le NAT :
```
R1# conf t
R1(config)# interface fastEthernet 0/0
R1(config-if)# ip nat outside
R1(config-if)# exit
```

--> définition des interfaces internes pour le NAT (celles des routers 2 et 3) :
```
Router1(config)# interface fastEthernet 1/0
Router1(config-if)# ip nat inside
Router1(config-if)# exit

Router1(config)# interface fastEthernet 2/0
Router1(config-if)# ip nat inside
Router1(config-if)# exit
```

--> On active le NAT et on créer un access-list très permissive qui autorise tout le trafic :

```
R1(config)#ip nat inside source list 1 interface fastEthernet 0/0 overload
R1(config)#access-list 1 permit any
```

--> On diffuse la route par défault par OSPF:

```
R1#conf t
R1(config)#router ospf 1
R1(config-router)#default-information originate
```

==> Ces deux routeurs ont désormais aussi accès à internet.

- `router2`

```
R2#ping 8.8.8.8

Type escape sequence to abort.
Sending 5, 100-byte ICMP Echos to 8.8.8.8, timeout is 2 seconds:
!!!!!
Success rate is 100 percent (5/5), round-trip min/avg/max = 64/84/100 ms
```

- `router3`

```
R3#ping 8.8.8.8

Type escape sequence to abort.
Sending 5, 100-byte ICMP Echos to 8.8.8.8, timeout is 2 seconds:
!!!!!
Success rate is 100 percent (5/5), round-trip min/avg/max = 80/84/88 ms
```

**********

### 6. Routage *inter-vlan*, Mise en place du *router-on-a-stick*

Dans cette partie nous allons essayer de faire joindre les clients et les serveurs entre-eux.

--> Prérequis dans notre topologie :

- [x] le mode trunk entre tous les switchs et entre `R3` et `SW1`

- `SW1`

```
SW1#sh int trunk

Port        Mode             Encapsulation  Status        Native vlan
Et0/0       on               802.1q         trunking      1
Et0/2       on               802.1q         trunking      1

Port        Vlans allowed on trunk
Et0/0       1-4094
Et0/2       1-4094

Port        Vlans allowed and active in management domain
Et0/0       1,10,20,30
Et0/2       1,10,20,30

Port        Vlans in spanning tree forwarding state and not pruned
Et0/0       1,10,20,30
Et0/2       1,10,20,30

```

- `SW2`

```
SW2#sh int trunk

Port        Mode             Encapsulation  Status        Native vlan
Et0/0       on               802.1q         trunking      1
Et0/1       on               802.1q         trunking      1

Port        Vlans allowed on trunk
Et0/0       1-4094
Et0/1       1-4094

Port        Vlans allowed and active in management domain
Et0/0       1,10,20,30
Et0/1       1,10,20,30

Port        Vlans in spanning tree forwarding state and not pruned
Et0/0       1,10,20,30
Et0/1       1,10,20,30

```

- `SW3`

```
SW3#sh int trunk

Port        Mode             Encapsulation  Status        Native vlan
Et0/0       on               802.1q         trunking      1

Port        Vlans allowed on trunk
Et0/0       1-4094

Port        Vlans allowed and active in management domain
Et0/0       1,30

Port        Vlans in spanning tree forwarding state and not pruned
Et0/0       1,30

```

--> On passe à la configuration de `R3` en tant que *router-on-a-stick*

- `R3`

```
# on passe en mode config et on démarre l'interface que l'on va 'couper' en plusieurs interfaces
R3#conf t
R3(config)#int fastEthernet 0/0
R3(config-if)#no shut
R3(config-if)#exit

# on crée une nouvelle interface virtuelle pour le VLAN 10
R3(config)#int fastEthernet 0/0.10

# on modifie l'encapsulation en dot1Q et on la tagues sur le VLAN 10
R3(config-subif)#encapsulation dot1Q 10
R3(config-subif)#ip address 10.4.10.254 255.255.255.0

# pareil pour les autres VLAN
R3(config)#int fastEthernet 0/0.20
R3(config-subif)#encapsulation dot1Q 20
R3(config-subif)#ip address 10.4.20.254 255.255.255.0

R3(config)#int fastEthernet 0/0.30
R3(config-subif)#encapsulation dot1Q 30
R3(config-subif)#ip address 10.4.30.254 255.255.255.0

# on vérifie nos changement
R3#sh ip int br
Interface                  IP-Address      OK? Method Status                Protocol
FastEthernet0/0            unassigned      YES NVRAM  up                    up
FastEthernet0/0.10         10.4.10.254     YES manual up                    up
FastEthernet0/0.20         10.4.20.254     YES manual up                    up
FastEthernet0/0.30         10.4.30.254     YES manual up                    up
FastEthernet1/0            10.4.1.9        YES NVRAM  up                    up
FastEthernet2/0            unassigned      YES NVRAM  administratively down down
FastEthernet3/0            10.4.1.6        YES NVRAM  up                    up
```

--> Petite vérification que les clients et les servers peuvent joindre leur gateway

- `client1`

```
[jdinh@client1 ~]$ ping 10.4.10.254 -c 2
PING 10.4.10.254 (10.4.10.254) 56(84) bytes of data.
64 bytes from 10.4.10.254: icmp_seq=1 ttl=255 time=6.20 ms
64 bytes from 10.4.10.254: icmp_seq=2 ttl=255 time=3.85 ms

--- 10.4.10.254 ping statistics ---
2 packets transmitted, 2 received, 0% packet loss, time 1001ms
rtt min/avg/max/mdev = 3.855/5.028/6.201/1.173 ms
```

- `client3`

```
[jdinh@client3 ~]$ ping 10.4.20.254 -c 2
PING 10.4.20.254 (10.4.20.254) 56(84) bytes of data.
64 bytes from 10.4.20.254: icmp_seq=1 ttl=255 time=10.9 ms
64 bytes from 10.4.20.254: icmp_seq=2 ttl=255 time=3.91 ms

--- 10.4.20.254 ping statistics ---
2 packets transmitted, 2 received, 0% packet loss, time 1000ms
rtt min/avg/max/mdev = 3.918/7.449/10.981/3.532 ms
```

- `server1`


```
[jdinh@server1 ~]$ ping 10.4.30.254 -c 2
PING 10.4.30.254 (10.4.30.254) 56(84) bytes of data.
64 bytes from 10.4.30.254: icmp_seq=1 ttl=255 time=6.19 ms
64 bytes from 10.4.30.254: icmp_seq=2 ttl=255 time=10.3 ms

--- 10.4.30.254 ping statistics ---
2 packets transmitted, 2 received, 0% packet loss, time 1002ms
rtt min/avg/max/mdev = 6.195/8.284/10.373/2.089 ms
```

--> il faut ajouter sur les `clients` et `servers` les routes par défault qui point vers leur passerelles respectives

- `client1`

```
[jdinh@client1 ~]$ sudo ip route add default via 10.4.10.254 dev enp0s3
[jdinh@client1 ~]$ ip r s
default via 10.4.10.254 dev enp0s3
10.4.10.0/24 dev enp0s3 proto kernel scope link src 10.4.10.1 metric 100
```

- `client3`

```
[jdinh@client3 ~]$ sudo ip route add default via 10.4.20.254 dev enp0s3
[jdinh@client3 ~]$ ip r s
default via 10.4.20.254 dev enp0s3
10.4.20.0/24 dev enp0s3 proto kernel scope link src 10.4.10.1 metric 100
```

- `server1`

```
[jdinh@server1 ~]$ sudo ip route add default via 10.4.30.254 dev enp0s3
[jdinh@server1 ~]$ ip r s
default via 10.4.30.254 dev enp0s3
10.4.30.0/24 dev enp0s3 proto kernel scope link src 10.4.30.1 metric 100
```

--> Vérification :

Tous le monde doit pouvoir se ping.

Prenons par exemple `client4`

```
# il peut joindre le reseau 10.4.10.0/24
[jdinh@client4 ~]$ ping client1 -c 2
PING client1 (10.4.10.1) 56(84) bytes of data.
64 bytes from client1 (10.4.10.1): icmp_seq=1 ttl=62 time=34.5 ms
64 bytes from client1 (10.4.10.1): icmp_seq=2 ttl=62 time=27.7 ms

--- client1 ping statistics ---
2 packets transmitted, 2 received, 0% packet loss, time 1002ms
rtt min/avg/max/mdev = 27.776/31.183/34.590/3.407 ms

# il peut joindre le reseau 10.4.30.0/24
[jdinh@client4 ~]$ ping server1 -c 2
PING server1 (10.4.30.1) 56(84) bytes of data.
64 bytes from server1 (10.4.30.1): icmp_seq=1 ttl=62 time=30.9 ms
64 bytes from server1 (10.4.30.1): icmp_seq=2 ttl=62 time=39.1 ms

--- server1 ping statistics ---
2 packets transmitted, 2 received, 0% packet loss, time 1001ms
rtt min/avg/max/mdev = 30.903/35.031/39.159/4.128 ms

# il peut joindre le reseau 10.4.20.0/24
[jdinh@client4 ~]$ ping 10.4.20.1 -c 2
PING 10.4.20.1 (10.4.20.1) 56(84) bytes of data.
64 bytes from 10.4.20.1: icmp_seq=1 ttl=62 time=41.8 ms
64 bytes from 10.4.20.1: icmp_seq=2 ttl=62 time=40.1 ms

--- 10.4.20.1 ping statistics ---
2 packets transmitted, 2 received, 0% packet loss, time 1002ms
rtt min/avg/max/mdev = 40.140/41.013/41.887/0.896 ms
```

### 7. Service d'infra

On va installer un serveur nginx. Les clients pourrons le `curl`.

On va utiliser le serveur web NGINX pour faire ça simplement :

- activation des dépôts EPEL (plus d'infos sur le net, ou posez moi la question)
  - `sudo yum install -y epel-release`
- installation du serveur web NGINX
  - `sudo yum install -y nginx`
- ouverture du port firewall pour l'HTTP
  - `sudo firewall-cmd --add-port=80/tcp --permanent`
- lancer le serveur web
  - `sudo systemctl start nginx`
- pour vérifier qu'il est lancé
  - `sudo systemctl status nginx`
  - `sudo ss -altnp4`

On devrait pouvoir accéder au serveur web depuis nos clients avec un curl.

- `client1`

```
[jdinh@client1 ~]$ curl server1
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


- `client3`

```
[jdinh@client3 ~]$ curl server1
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


- `client4`

```
[jdinh@client4 ~]$ curl server1
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
