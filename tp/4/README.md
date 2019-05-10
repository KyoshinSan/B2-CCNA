# Menu 3 : Infra small/medium office

## Sommaire
   #### I. Infrastructure GNS3
   #### II. Schéma, tableaux d'addressage et matériel
   #### III. Configuration des postes clients, admin, RH et des serveurs
    
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

##      III. Configuration des postes clients, admin, RH et des serveurs

Nous avons dans un premier temps créé une machine **client01**, à laquelle on a attribué son **hostname et adresse ip respectifs** (cf. tableau d'adressage).
- Hostname : 
``` 
$ sudo vi /etc/hosname
```
```
client1.lab1.tp4
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
127.0.0.1 localhost localhosts.localdomain localhost4 localhost4.localdomain4
::1       localhost localhosts.localdomain localhost6 localhost6.localdomain6
client1.lab1.tp4 192.168.10.1
client6.lab1.tp4 192.168.10.6
client11.lab1.tp4 192.168.10.11
client16.lab1.tp4 192.168.10.16
admin.lab1.tp4 192.168.30.1
rh1.lab1.tp4 192.168.20.1
server.lab1.tp4 192.168.90.1
server2.lab1.tp4 192.168.90.2
server3.lab1.tp4 192.168.90.3
server4.lab1.tp4 192.168.90.4
server5.lab1.tp4 192.168.90.5
```
Enfin, nous avons édité le fichier **network** dans le répertoire **sysconfig** afin d'y ajouter la route par défaut qui pointe vers sa passerelle respective. C'est une étape nécéssaire à la mise en place du **router on-a-stick** à venir.
- Network :
```
$sudo vi /etc/sysconfig/network
```
```
GATEWAY=192.168.90.1
```
Une fois ces étapes terminées, on clone la machine client01 afin de créer les autres postes, ainsi il n'y a plus qu'à adapter le **hostname** et **l'adresse ip** des autres machines pour les configurer.
on a fait 1 poste client. hostname, hostnames, enp0s3, copie vm<br />
config HA Nginx :<br />
notes perso : <br />
vi /etc/sysconfig/network<br />
-GATEWAY=ip machine.<br />
=>ajouter sur les clients et servers les routes par défault qui point vers leur passerelles respectives<br />
firewall-cmd --permanent --add-service=high-availability<br />

