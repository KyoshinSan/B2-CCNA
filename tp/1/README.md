# TP1 - Remise dans le bain
**Auteur : DINH / HERNANDEZ**
**Date : 14/02/2019**
**Desc :** Rendu du 1er tp en CCNA
## I. Exploration du réseau d'une machine CentOS
### 1. Mise en place
#### Allumage et configuration de la VM

 - [x] [définition d'IP statique sur les deux cartes host-only](https://github.com/It4lik/B2-Reseau-2018/blob/master/cours/procedures.md#d%C3%A9finir-une-ip-statique)
	 - `sudo vi /etc/sysconfig/network-scripts/ifcfg-enp0s8`
	 - `sudo vi /etc/sysconfig/network-scripts/ifcfg-enp0s9`
 - [x] Connexion en SSH
	 - `ssh theo@10.1.1.2`
 - [x] Définition d'un nom de domaine
	 - `[theo@client1 ~]$`
 - [x] [compléter le fichier hosts de la VM](https://github.com/It4lik/B2-Reseau-2018/blob/master/cours/procedures.md#editer-le-fichier-hosts)
	 - `10.1.1.1    host host.tp1.b2`
    `10.1.1.2    client1 client1.tp1.b2`
`10.1.2.1    host host.tp1.b2`
`10.1.2.2    client1 client1.tp1.b2`

- Combien y-a-il d'adresses disponible dans un `/24` ?
	- Il y en a 254.
- Combien y-a-il d'adresses disponible dans un `/30` ?
	- Il y en a 2.
- Quelle est l'utilité d'un `/30` ?
	- C'est un réseau restreint qui permet d'attribuer que 2 adresses IP. 
- Expliquer le retour que vous obtenez (code retour HTTP `301`)
	- C'est une redirection car cela ne se fait pas automatiquement.
### 2. Basics
#### Routes

 - [x]  Supprimer la route par default
	 - `sudo ip route del default`
- Expliquer chacune des lignes 
	- `10.0.2.0/24 dev enp0s3 proto kernel scope link src 10.0.2.15 metric 100`
	Pour communiquer dans cette adresse réseau `10.0.2.0/24`, la machine utilisera cette adresse ip dans ce réseau qui est `10.0.2.15/24` de même pour les deux autres :
	- `10.1.1.0/24 dev enp0s8 proto kernel scope link src 10.1.1.2 metric 101`
	- `10.1.2.0/30 dev enp0s9 proto kernel scope link src 10.1.2.2 metric 102`
- Expliquer chacune des lignes
	- `10.1.2.1 dev enp0s9 lladdr 0a:00:27:00:00:4d REACHABLE`
    `10.1.1.1 dev enp0s8 lladdr 0a:00:27:00:00:49 REACHABLE`
    Ces lignes indique qu'elles voisins est enregistré dans la table ARP.
- Expliquer la nouvel ligne
	- On a vider la table ARP donc on a perdu une ligne, il ne rester que la ligne 2 car on était connecter en ssh, après la commande ping 10.1.2.1 le voisin s'est enregistré dans la table une 2ème fois.
	#### Capture Réseau
	- `PS C:\Users\theo3> ssh theo@10.1.2.2`
	- `[theo@client1 ~]$ sudo ip neigh flush all`
	- `[theo@client1 ~]$ ip n s`

## Communication simple entre deux machines
