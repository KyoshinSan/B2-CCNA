# TP1 - Remise dans le bain

**Auteur :** DINH Jonathan & HERNANDEZ Theo <br />
**Date :** 14/02/2019 <br />
**Desc :** Rendu du 1er tp en CCNA

## I. Exploration du réseau d'une machine CentOS
### 1. Mise en place
#### Configuration de VirtualBox

- Combien y-a-il d'adresses disponible dans un `/24` ?

	- Il y en a 254. Le calcul : 2^(32-24)-2.
	
- Combien y-a-il d'adresses disponible dans un `/30` ?

	- Il y en a 2. Même calcul.
	
- Quelle est l'utilité d'un `/30` ?

	- C'est un réseau restreint qui permet d'attribuer que 2 adresses IP. 
	
#### Allumage et configuration de la VM

 - [x] [définition d'IP statique sur les deux cartes host-only](https://github.com/It4lik/B2-Reseau-2018/blob/master/cours/procedures.md#d%C3%A9finir-une-ip-statique)
 
	- On modifie `IPADDR` et `NETMASK` dans les 2 fichiers suivants :
	- `sudo vi /etc/sysconfig/network-scripts/ifcfg-enp0s8`
	- `sudo vi /etc/sysconfig/network-scripts/ifcfg-enp0s9`
	- `ifdown`, `ifup` les 2 interfaces
	- Puis `$ ip a | grep "inet "` pour vérifie qu'on a les bonnes ip
	
 - [x] Connexion en SSH
 
	 - `ssh theo@10.1.1.2`
	 
 - [x] Définition d'un nom de domaine
 
	 - `[theo@client1 ~]$`
	 
 - [x] [compléter le fichier hosts de la VM](https://github.com/It4lik/B2-Reseau-2018/blob/master/cours/procedures.md#editer-le-fichier-hosts)
 
	- `10.1.1.1    host host.tp1.b2`
	- `10.1.1.2    client1 client1.tp1.b2`
	- `10.1.2.1    host host.tp1.b2`
	- `10.1.2.2    client1 client1.tp1.b2`
	
 - [x] s'assurer que les 3 cartes réseaux fonctionnent :
 
- Expliquer le retour que vous obtenez (code retour HTTP `301`)

	- '_301 Moved Permanently_' C'est utilisé pour une Redirection d'URL permanente.
	
### 2. Basics
#### Routes

 - [x]  Supprimer la route par default
 
	 - `sudo ip route del default`
	 
- afficher les routes que connaît votre machine, expliquer chacune des lignes 

	- `10.0.2.0/24 dev enp0s3 proto kernel scope link src 10.0.2.15 metric 100`<br />
	Pour communiquer dans cette adresse réseau `10.0.2.0/24`, la machine utilisera cette adresse ip dans ce réseau qui est `10.0.2.15/24` de même pour les deux autres :
	- `10.1.1.0/24 dev enp0s8 proto kernel scope link src 10.1.1.2 metric 101`
	- `10.1.2.0/30 dev enp0s9 proto kernel scope link src 10.1.2.2 metric 102`
	
-  supprimer une route

	-  `sudo ip route del 10.1.2.0/30`
	- `ping 10.1.2.1`<br />
	`connect: Network is unreachable`
	
- remettre la route 

	- `sudo ip route add 10.1.2.0/30 dev enp0s9`
	- `ping 10.1.2.1`<br />
	`PING 10.1.2.1 (10.1.2.1) 56(84) bytes of data.`<br />
	`64 bytes from 10.1.2.1: icmp_seq=1 ttl=128 time=0.499 ms`<br />
	`64 bytes from 10.1.2.1: icmp_seq=2 ttl=128 time=0.695 ms`<br />
	`64 bytes from 10.1.2.1: icmp_seq=3 ttl=128 time=0.707 ms`
	
#### Tables ARP

- afficher les voisins que connaît votre machine = la table ARP, expliquer chacune des lignes

	- `10.1.2.1 dev enp0s9 lladdr 0a:00:27:00:00:4d REACHABLE`<br />
    	`10.1.1.1 dev enp0s8 lladdr 0a:00:27:00:00:49 REACHABLE` <br />
    	Ces lignes indique qu'elles voisins est enregistré dans la table ARP.
	
- vider la table ARP

	- `ip neigh show`<br />
	`10.1.1.1 dev enp0s8 lladdr 0a:00:27:00:00:09 REACHABLE`

- effectuer une requête simple vers l'hôte, expliquer la nouvel ligne

	- On a vider la table ARP donc on a perdu une ligne, il ne rester que la ligne 2 car on était connecter en ssh, après la commande `ping 10.1.2.1` le voisin s'est enregistré dans la table une 2ème fois.
	
#### Capture Réseau

	- 1er capture avec la requête ARP :
	![Screenshot_1](Screensho_1.png)

## Communication simple entre deux machines
