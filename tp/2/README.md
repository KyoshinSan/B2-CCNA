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

## II. NAT et services d'infra

Le NAT a bien été mis en place, on peut :
- Faire un <b>curl google.com</b> depuis la machine <b>router2</b> 
`[soussou@router2 ~]$ curl google.com`<br />
`<HTML><HEAD><meta http-equiv="content-type" content="text/html;charset=utf-8">`<br />
`<TITLE>301 Moved</TITLE></HEAD><BODY>\' <br />
`<H1>301 Moved</H1>\` <br />
`The document has moved` <br />
`<A HREF="http://www.google.com/">here</A>.` <br />
`</BODY></HTML>` <br />
`[soussou@router2 ~]$`
