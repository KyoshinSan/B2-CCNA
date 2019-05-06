# Menu 3 : Infra small/medium office

Déroulement

- Choix du sujet
- INSERER ARCHITECTURE ICI
- INSERER PLAN D'ADDRESSAGE ICI


notes perso : 
vi /etc/sysconfig/network
-GATEWAY=ip machine.
=>ajouter sur les clients et servers les routes par défault qui point vers leur passerelles respectives

| Hosts | `192.168.10.0/24` | `192.168.20.0/24` |	`192.168.30.0/24` |	`192.168.80/24` | `192.168.80.0/24` | 
| ---------------- | ----------- | ----------- | ------------ | ------------ | ------------ | 
| `client1.lab1.tp4` | `192.168.10.1/24`| X           | X            | X            | X |
| `client6.lab1.tp4` | `192.168.10.6/24`| X           | X            | X            | X | 
| `client11.lab1.tp4`| `192.168.10.11/24`| X           | X            | X            | X |
| `client16.lab1.tp4`| `192.168.10.16/24`| X           | X            | X            | X |
| `admin.lab1.tp4` | X           | X           | `192.168.30.1/24` | X            | X |
| `rh.lab1.tp4` | X           | `192.168.20.1/24` | X            | X           | X |
| `server1.lab1.tp4` | X           | X           | X            | `192.168.90.1/24` | X |
| `server2.lab1.tp4` | X           | X           | X            | `192.168.90.2/24` | X |  
| `server3.lab1.tp4` | X           | X           | X            | `192.168.90.3/24` | X |
| `server4.lab1.tp4` | X           | X           | X            | `192.168.90.4/24` | X |
| `server5.lab1.tp4` | X           | X           | X            | `192.168.90.5/24` | X |
