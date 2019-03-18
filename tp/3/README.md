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
  
