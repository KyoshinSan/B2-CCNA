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

Le NAT a bien été mis en place, on peut faire un <b>curl google.com</b> depuis :
- La machine <b>router2</b><br /><br /> 
`[soussou@router2 ~]$ curl google.com`<br />
`<HTML><HEAD><meta http-equiv="content-type" content="text/html;charset=utf-8">`<br />
`<TITLE>301 Moved</TITLE></HEAD><BODY>\'`<br />
`<H1>301 Moved</H1>\`<br />
`The document has moved`<br />
`<A HREF="http://www.google.com/">here</A>.`<br />
`</BODY></HTML>`<br />
`[soussou@router2 ~]$`<br /><br />

- La machine <b>client1</b><br /><br />
`[soussou@client1 ~]$ curl google.com`<br />
`<HTML><HEAD><meta http-equiv="content-type" content="text/html;charset=utf-8">`<br />
`<TITLE>301 Moved</TITLE></HEAD><BODY>`<br />
`<H1>301 Moved</H1>`<br />
`The document has moved`<br />
`<A HREF="http://www.google.com/">here</A>.`<br />
`</BODY></HTML>`<br />
`[soussou@client1 ~]$`<br /><br />

## III. NTP Server

- Une fois <b>chrony</b> mis en place :<br /><br />
`[soussou@router1 ~]$ chronyc sources`<br />
`210 Number of sources = 4`<br />
`MS Name/IP address         Stratum Poll Reach LastRx Last sample`<br />
`===============================================================================`<br />
`^? ntp-2.arkena.net              2   6     1     6  -943.7s[-943.7s] +/-   27ms`<br />
`^? 30-251-47-212.rev.cloud.>     2   6     1     7  -943.7s[-943.7s] +/-   60ms`<br />
`^? y.ns.gin.ntt.net              2   6     1     6  -943.7s[-943.7s] +/-  106ms`<br />
`^? ks3370497.kimsufi.com         2   6     1     6  -943.6s[-943.6s] +/-   57ms`<br />
`[soussou@router1 ~]$ chronyc tracking`<br />
`Reference ID    : 7F7F0101 ()`<br />
`Stratum         : 10`<br />
`Ref time (UTC)  : Mon Mar 04 14:55:33 2019`<br />
`System time     : 0.000000000 seconds fast of NTP time`<br />
`Last offset     : +0.000000000 seconds`<br />
`RMS offset      : 0.000000000 seconds`<br />
`Frequency       : 0.381 ppm fast`<br />
`Residual freq   : +0.000 ppm`<br />
`Skew            : 0.000 ppm`<br />
`Root delay      : 0.000000000 seconds`<br />
`Root dispersion : 0.000000000 seconds`<br />
`Update interval : 0.0 seconds`<br />
`Leap status     : Normal`<br />
`[soussou@router1 ~]$`<br />



