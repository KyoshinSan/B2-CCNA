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
