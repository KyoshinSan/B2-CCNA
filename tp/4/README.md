# Menu 3 : Infra small/medium office
INSERER IMAGE DE PRESENTATION ICI.
## Sommaire
   #### I. Infrastructure GNS3
   #### II. Schéma, tableaux d'addressage et matériel
   #### III. Configuration des postes clients/admin/RH/serveurs et imprimantes
   #### IV. Configuration des switchs et VLANs
    
##      I. Infrastructure GNS3 
![Screenshot_1](https://github.com/KyoshinSan/B2-CCNA/blob/master/tp/4/infragns3.png)

##      II. Composition de l'infrastructure et tableau d'addressage 

#### Schéma architecture du réseau
![Screenshot_2](https://github.com/KyoshinSan/B2-CCNA/blob/master/tp/4/archi.png)

#### Tableau d'adressage

| Hosts | `192.168.10.0/24` | `192.168.20.0/24` |	`192.168.30.0/24` |	`192.168.80/24` | `192.168.90.0/24` | 
| ---------------- | ----------- | ----------- | ------------ | ------------ | ------------ | 
| `client1.lab1.tp4` | `192.168.10.1/24`| X           | X            | X            | X |
| `client6.lab1.tp4` | `192.168.10.6/24`| X           | X            | X            | X | 
| `client11.lab1.tp4`| `192.168.10.11/24`| X           | X            | X            | X |
| `client16.lab1.tp4`| `192.168.10.16/24`| X           | X            | X            | X |
| `admin.lab1.tp4` | X           | X           | `192.168.30.1/24` | X            | X |
| `rh.lab1.tp4` | X           | `192.168.20.1/24` | X            | X           | X |
| `server1.lab1.tp4` | X           | X           | X            | X            | `192.168.90.1/24` |
| `server2.lab1.tp4` | X           | X           | X            | X            | `192.168.90.2/24` |  
| `server3.lab1.tp4` | X           | X           | X            | X            | `192.168.90.3/24` |
| `server4.lab1.tp4` | X           | X           | X            | X            | `192.168.90.4/24` |
| `server5.lab1.tp4` | X           | X           | X            | X            | `192.168.90.5/24` |
| `printer1.lab1.tp4` | X           | X           | X            | `192.168.80.1/24` | X |
| `printer2.lab1.tp4` | X           | X           | X            | `192.168.80.2/24` | X |
| `printer3.lab1.tp4` | X           | X           | X            | `192.168.80.3/24` | X |
| `printer4.lab1.tp4` | X           | X           | X            | `192.168.80.4/24` | X |
| `printer5.lab1.tp4` | X           | X           | X            | `192.168.80.5/24` | X |


#### Tableau Matériel

| Matériel | Quantité | Prix unitaire | Prix Total | Référence |
| ---------------- | ----------- | ----------- | ----------- | ----------- |
| `Serveurs` | `5` | `2 000,00 €` | `10 000,00 €` | `PowerEdge T440` |
| `Postes Employés` | `24` | `900,00 €` | `21 600,00 €` | `DELL XPS Tour` |
| `Switchs` | `7` | `135,00 €` | `945,00 €` | `Cisco SG110-16` |
| `Routeur` | `1` | `200,00 €` | `200,00 €` | `Cisco RV320` |
| `Imprimantes` | `5` | `105,00 €` | `525,00 €` | `Brother DCP 1612 W` |
| `Câbles` | `5` | `150,00 €/100 mètres` | `750,00 €` | `Monobrin RJ45 catégorie 7` |

#### Prix total de l'infrastructure : 34 020 €

##      III. Configuration des postes clients/admin/RH/serveurs et imprimantes

Nous avons dans un premier temps créé une machine **client01**, à laquelle on a attribué son **hostname et adresse ip respectifs** (cf. tableau d'adressage).
- Hostname : 
``` 
$ sudo vi /etc/hosname
```
```
client1.tp4
```
- Adresse IP :
```
$ sudo vi /etc/sysonfig/network-scripts/ifcfg-enp0s3
```
```
   TYPE=Ethernet
   BOOTPROTO=static
   NAME=enp0s3
   DEVICE=enp0s3
   ONBOOT=yes
   IPADDR=192.168.10.1
   NETMASK=255.255.255.0
   ZONE=Public
```
Dans un second temps, nous avons configuré le fichier **hosts**, dans lequel nous avons renseigné le nom de chaque machine ainsi que leurs **adresses respectives**.
- Hosts :
```
$ sudo vi /etc/hosts
```
```
127.0.0.1   localhost localhost.localdomain localhost4 localhost4.localdomain4
::1         localhost localhost.localdomain localhost6 localhost6.localdomain6
192.168.10.1 client1 client1.tp4
192.168.10.6 client6 client6.tp4
192.168.10.11 client11 client11.tp4
192.168.10.16 client16 client16.tp4
192.168.20.1 rh1 rh1.tp4
192.168.30.1 admin admin.tp4
192.168.90.1 server1 server1.tp4
192.168.90.2 server2 server2.tp4
192.168.90.3 server3 server3.tp4
192.168.90.4 server4 server4.tp4
192.168.90.5 server5 server5.tp4
```
Enfin, nous avons édité le fichier **network** dans le répertoire **sysconfig** afin d'y ajouter la route par défaut qui pointe vers sa passerelle respective. C'est une étape nécéssaire à la mise en place du **router on-a-stick** à venir.
- Network :
```
$sudo vi /etc/sysconfig/network
```
```
GATEWAY=192.168.10.254
```
```
[jdinh@client1 ~]$ ip r s
default via 192.168.10.254 dev enp0s3 proto static metric 100
192.168.10.0/24 dev enp0s3 proto kernel scope link src 192.168.10.1 metric 100
```
Une fois ces étapes terminées, on clone la machine client01 afin de créer les autres postes, ainsi il n'y a plus qu'à adapter le **hostname** et **l'adresse ip** des autres machines pour les configurer.

Pour les imprimantes, nous avons créé des VPCS depuis GNS3. Il seulement fallu leur attribuer un nom(depuis GNS3) ainsi que leurs adresses ip respectives, puis sauvegarder la configuration :
- IP 
```
Print1> ip 192.168.80.1/24 192.168.80.254
```
```
Print1> show ip

NAME        : Print1[1]
IP/MASK     : 192.168.80.1/24
GATEWAY     : 192.168.80.254
DNS         :
MAC         : 00:50:79:66:68:00
LPORT       : 10041
RHOST:PORT  : 127.0.0.1:10042
MTU:        : 1500
```
- Sauvegarder la configuration 
```
Print1> save conftp
Saving startup configuration to conftp.vpc
.  done
```
On fait de même pour les autres imprimantes.

##      IV. Configuration des switchs et VLANs
![Screenshot_1](https://github.com/KyoshinSan/B2-CCNA/blob/master/tp/4/trunk.png)
Sur le switch6, nous avons tout d'abord créé les VLANs de notre réseau :
- *Switch6*
```
# conf t
```
```
(config)# vlan 10
(config-vlan)# name VLAN10
(config-vlan)# exit
(config)# vlan 20
(config-vlan)# name VLAN20
(config-vlan)# exit
(config)# vlan 30
(config-vlan)# name VLAN30
(config-vlan)# exit
(config)# vlan 80
(config-vlan)# name VLAN80
(config-vlan)# exit
(config)# vlan 90
(config-vlan)# name VLAN90
(config-vlan)# exit
```

- Puis configuré chaque interface entre deux switchs (mode trunk) :
```
(config)# interface Ethernet 0/1
(config-if)# switchport trunk encapsulation dot1q
(config-if)# switchport mode trunk
```
--> Le faire pour toutes les interfaces du *switch6*

- Afficher les VLANs actuels du *Switch6*
```
# show vlan
# show vlan br
```
```
Switch6#show vlan br

VLAN Name                             Status    Ports
---- -------------------------------- --------- -------------------------------
1    default                          active    Et0/1, Et0/2, Et0/3, Et1/0
                                                Et1/2, Et1/3, Et2/0, Et2/1
                                                Et2/2, Et2/3, Et3/0, Et3/1
                                                Et3/2, Et3/3
10   vlan10                           active
20   vlan20                           active
30   vlan30                           active
80   vlan80                           active
90   vlan90                           active
1002 fddi-default                     act/unsup
1003 token-ring-default               act/unsup
1004 fddinet-default                  act/unsup
1005 trnet-default                    act/unsup

```
##      V. Configuration du _Router-on-a-stick_

--> On passe à la configuration de `R1` en tant que *router-on-a-stick*

- `R1`

```
# on passe en mode config et on démarre l'interface que l'on va 'couper' en plusieurs interfaces
R1#conf t
R1(config)#int fastEthernet 1/0
R1(config-if)#no shut
R1(config-if)#exit

# on crée une nouvelle interface virtuelle pour le VLAN 10
R1(config)#int fastEthernet 1/0.10

# on modifie l'encapsulation en dot1Q et on la tague sur le VLAN 10
R1(config-subif)#encapsulation dot1Q 10
R1(config-subif)#ip address 192.168.10.254 255.255.255.0

# pareil pour les autres VLAN
R1(config)#int fastEthernet 1/0.20
R1(config-subif)#encapsulation dot1Q 20
R1(config-subif)#ip address 192.168.20.254 255.255.255.0

R1(config)#int fastEthernet 1/0.30
R1(config-subif)#encapsulation dot1Q 30
R1(config-subif)#ip address 192.168.30.254 255.255.255.0

R1(config)#int fastEthernet 1/0.80
R1(config-subif)#encapsulation dot1Q 80
R1(config-subif)#ip address 192.168.80.254 255.255.255.0

R1(config)#int fastEthernet 1/0.90
R1(config-subif)#encapsulation dot1Q 90
R1(config-subif)#ip address 192.168.90.254 255.255.255.0

# on vérifie nos changement
R1#sh ip int br
Interface                  IP-Address      OK? Method Status                Protocol
FastEthernet0/0            192.168.122.12  YES DHCP   up                    up
FastEthernet1/0            unassigned      YES NVRAM  up                    up
FastEthernet1/0.10         192.168.10.254  YES NVRAM  up                    up
FastEthernet1/0.20         192.168.20.254  YES NVRAM  up                    up
FastEthernet1/0.30         192.168.30.254  YES NVRAM  up                    up
FastEthernet1/0.80         192.168.80.254  YES NVRAM  up                    up
FastEthernet1/0.90         192.168.90.254  YES NVRAM  up                    up
FastEthernet2/0            unassigned      YES NVRAM  administratively down down
FastEthernet3/0            unassigned      YES NVRAM  administratively down down
NVI0                       unassigned      NO  unset  up                    up
```

--> Petite vérification que l'admin peut joindre tous le monde

- `admin`

```
[jdinh@admin ~]$ ping server1 -c 2
PING server1 (192.168.90.1) 56(84) bytes of data.
64 bytes from server1 (192.168.90.1): icmp_seq=1 ttl=63 time=18.7 ms
64 bytes from server1 (192.168.90.1): icmp_seq=2 ttl=63 time=13.3 ms

--- server1 ping statistics ---
2 packets transmitted, 2 received, 0% packet loss, time 1002ms
rtt min/avg/max/mdev = 13.347/16.044/18.742/2.700 ms

[jdinh@admin ~]$ ping client1 -c 2
PING client1 (192.168.10.1) 56(84) bytes of data.
64 bytes from client1 (192.168.10.1): icmp_seq=1 ttl=63 time=13.1 ms
64 bytes from client1 (192.168.10.1): icmp_seq=2 ttl=63 time=21.4 ms

--- client1 ping statistics ---
2 packets transmitted, 2 received, 0% packet loss, time 1001ms
rtt min/avg/max/mdev = 13.150/17.311/21.473/4.163 ms

[jdinh@admin ~]$ ping rh1 -c 2
PING rh1 (192.168.20.1) 56(84) bytes of data.
64 bytes from rh1 (192.168.20.1): icmp_seq=1 ttl=63 time=22.6 ms
64 bytes from rh1 (192.168.20.1): icmp_seq=2 ttl=63 time=33.2 ms

--- rh1 ping statistics ---
2 packets transmitted, 2 received, 0% packet loss, time 1000ms
rtt min/avg/max/mdev = 22.607/27.919/33.232/5.315 ms

[jdinh@admin ~]$ ping 192.168.80.5 -c 2
PING 192.168.80.5 (192.168.80.5) 56(84) bytes of data.
64 bytes from 192.168.80.5: icmp_seq=1 ttl=63 time=21.4 ms
64 bytes from 192.168.80.5: icmp_seq=2 ttl=63 time=18.8 ms

--- 192.168.80.5 ping statistics ---
2 packets transmitted, 2 received, 0% packet loss, time 1001ms
rtt min/avg/max/mdev = 18.801/20.117/21.433/1.316 ms
```

##      VI. Configuration du Virtual Router Redundancy Protocol (VRRP)

Dans cette partie, on va mettre en place la redondance des routeurs. Cela va changer un peu notre architecture actuelle. On va ajouter R2 dans notre schéma.

![Screenshot_2](https://raw.githubusercontent.com/KyoshinSan/B2-CCNA/master/tp/4/Screenshot_2.png)

Puis on change les ip des routers :

Configuration de `R1` :

```
R1#sh ip int br
Interface                  IP-Address      OK? Method Status                Protocol
FastEthernet0/0            192.168.122.12  YES DHCP   up                    up
FastEthernet1/0            unassigned      YES NVRAM  up                    up
FastEthernet1/0.10         192.168.10.252  YES manual up                    up
FastEthernet1/0.20         192.168.20.252  YES manual up                    up
FastEthernet1/0.30         192.168.30.252  YES manual up                    up
FastEthernet1/0.80         192.168.80.252  YES manual up                    up
FastEthernet1/0.90         192.168.90.252  YES manual up                    up
FastEthernet2/0            unassigned      YES NVRAM  administratively down down
FastEthernet3/0            unassigned      YES NVRAM  administratively down down
NVI0                       unassigned      NO  unset  up                    up
```

Configuration de `R2` :

```
R2#sh ip int br
Interface                  IP-Address      OK? Method Status                Protocol
FastEthernet0/0            192.168.122.13  YES DHCP   up                    up
FastEthernet1/0            unassigned      YES unset  up                    up
FastEthernet1/0.10         192.168.10.253  YES manual up                    up
FastEthernet1/0.20         192.168.20.253  YES manual up                    up
FastEthernet1/0.30         192.168.30.253  YES manual up                    up
FastEthernet1/0.80         192.168.80.253  YES manual up                    up
FastEthernet1/0.90         192.168.90.253  YES manual up                    up
FastEthernet2/0            unassigned      YES unset  administratively down down
FastEthernet3/0            unassigned      YES unset  administratively down down
NVI0                       unassigned      NO  unset  up                    up
```

Maintenant on va passer à la configuration de VRRP sur les routeurs.

- `R1` :

```
R1(config)# interface fastEthernet 1/0.10
R1(config-if)# vrrp 1 ip 192.168.10.254      # IP virtuelle
R1(config-if)# vrrp 1 priority 200           # Pour définir le master et les backup (le master est celui qui a la plus haute priorité
R1(config-if)# vrrp 1 preempt                # on active le mode préemption (pour redevenir master en cas de coupure)
```

- `R2` :

```
R2(config)# interface fastEthernet 1/0.10
R2(config-if)# vrrp 1 ip 192.168.10.254
R2(config-if)# vrrp 1 priority 100
```

On fait la même chose pour autre interface.

Pour vérifier la configuration :

```
R1#sh vrrp
FastEthernet1/0.10 - Group 1
  State is Master                         # on peut voir que R1 est le Master
  Virtual IP address is 192.168.10.254    # l'addresse virutelle est bien la gateway
  Virtual MAC address is 0000.5e00.0101
  Advertisement interval is 1.000 sec
  Preemption enabled
  Priority is 200
  Master Router is 192.168.10.252 (local), priority is 200
  Master Advertisement interval is 1.000 sec
  Master Down interval is 3.218 sec

R2#sh vrrp
FastEthernet1/0.10 - Group 1
  State is Backup                         # on peut voir que R2 est un backup
  Virtual IP address is 192.168.10.254 
  Virtual MAC address is 0000.5e00.0101
  Advertisement interval is 1.000 sec
  Preemption enabled
  Priority is 100
  Master Router is 192.168.10.252, priority is 200
  Master Advertisement interval is 1.000 sec
  Master Down interval is 3.609 sec (expires in 2.789 sec)

```

Passons au test :

```
[jdinh@client1 ~]$ traceroute server1
traceroute to server1 (192.168.90.1), 30 hops max, 60 byte packets
 1  192.168.10.252 (192.168.10.252)  13.860 ms  13.758 ms  13.663 ms
 2  server1 (192.168.90.1)  22.099 ms !X  22.015 ms !X  21.935 ms !X
```
On peut voir que ça passe par `R1` (192.168.10.252)

- On coupe `R1` puis on refait un `traceroute` :

```
[jdinh@client1 ~]$ traceroute server1
traceroute to server1 (192.168.90.1), 30 hops max, 60 byte packets
 1  192.168.10.253 (192.168.10.253)  13.072 ms  12.947 ms  12.859 ms
 2  server1 (192.168.90.1)  22.510 ms !X  22.435 ms !X  22.326 ms !X
```
On peut voir que ça passe par `R2` (192.168.10.253)

##      VII. Configuration du serveur web redondée (PCSD, Corosync, Pacemaker / HAProxy)

config HA Nginx :<br />
notes perso : <br />

firewall-cmd --permanent --add-service=high-availability<br />
Firewall au plus pres du routeur. Utilisé haproxy | Pacemaker&Corosync
OpenVpn avec Radius ?
VRRP, HSRP with subnet ?

