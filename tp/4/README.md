# Menu 3 : Infra small/medium office

## Sommaire
   #### I. Infrastructure GNS3
   #### II. Schéma, tableaux d'addressage et matériel
   #### III. Configuration des postes clients, admin et RH
    
##      I. Infrastructure GNS3 
![Screenshot_1](https://github.com/KyoshinSan/B2-CCNA/blob/master/tp/4/infragns3.png)

##      II. Composition de l'infrastructure et tableau d'addressage 

#### Schéma architecture du réseau
![Screenshot_2](https://github.com/KyoshinSan/B2-CCNA/blob/master/tp/4/archi.png)


notes perso : 
vi /etc/sysconfig/network
-GATEWAY=ip machine.
=>ajouter sur les clients et servers les routes par défault qui point vers leur passerelles respectives

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

##      III. Configuration des postes clients, admin et RH
on a fait 1 poste client. hostname, hostnames, enp0s3, copie vm
config HA Nginx :

firewall-cmd --permanent --add-service=high-availability

