---
title: PPP Multi_Client M3103
---

# PPP Multi_Client M3103
>[name=Solen BELLOUATI] 
>


### Schéma des échanges entre la machine et le serveur PPPOE
Voici le schéma des échanges entre une machine et un serveur PPPOE, de la recherche du serveur par la machine jusqu'à la cloture de la session.

![](https://i.imgur.com/N5od5oq.png)

### Echanges entre la machine et le serveur PPPOE

On test la connexion entre le client et le serveur, via un switch.

```wireshark=
No.     Time           Source                Destination           Protocol Length Info
    370 313.056333922  Dell_e0:7c:34         Broadcast             PPPoED   60     Active Discovery Initiation (PADI)

Frame 370: 60 bytes on wire (480 bits), 60 bytes captured (480 bits) on interface 0
Ethernet II, Src: Dell_e0:7c:34 (98:90:96:e0:7c:34), Dst: Broadcast (ff:ff:ff:ff:ff:ff)
PPP-over-Ethernet Discovery

    371 313.056429194  Luxshare_02:b9:be     Dell_e0:7c:34         PPPoED   57     Active Discovery Offer (PADO) AC-Name='214-2'

Frame 371: 57 bytes on wire (456 bits), 57 bytes captured (456 bits) on interface 0
Ethernet II, Src: Luxshare_02:b9:be (3c:18:a0:02:b9:be), Dst: Dell_e0:7c:34 (98:90:96:e0:7c:34)
PPP-over-Ethernet Discovery

    372 313.057264823  Dell_e0:7c:34         Luxshare_02:b9:be     PPPoED   60     Active Discovery Request (PADR)

Frame 372: 60 bytes on wire (480 bits), 60 bytes captured (480 bits) on interface 0
Ethernet II, Src: Dell_e0:7c:34 (98:90:96:e0:7c:34), Dst: Luxshare_02:b9:be (3c:18:a0:02:b9:be)
PPP-over-Ethernet Discovery

    373 313.057838512  Luxshare_02:b9:be     Dell_e0:7c:34         PPPoED   24     Active Discovery Session-confirmation (PADS)

Frame 373: 24 bytes on wire (192 bits), 24 bytes captured (192 bits) on interface 0
Ethernet II, Src: Luxshare_02:b9:be (3c:18:a0:02:b9:be), Dst: Dell_e0:7c:34 (98:90:96:e0:7c:34)
PPP-over-Ethernet Discovery
```
La connexion fonctionne correctement, car on peut remarquer le PADI-PADO,ce qui nous permet de continuer.
On peut maintenant mettre en place le PPP multi_client.


### Initialisation de la connexion multiclient via PPPOE

Afin de permettre à plusieurs clients de se connecter au même serveur, on va tout d'abord éditer le fichier de configuration /etc/ppp/pap-secrets, pour permettre leur authentification à tous.

```bash=
#	*	password
theo	*	test	*
pablo 	*	test	*
yunhan	*	test	*
```

Afin de mettre en route le serveur, il faut lancer la commande suivante : 
```bash=
root@214-2:/etc/ppp# pppoe-server -I enx3c18a002b9be -C SERVER -R 10.214.2.101 -L 10.214.2.100
```

### Options de la commande PPPOE-SERVER
> -C Permet de spécifier l'AC_NAME
> -I Permet de spécifier l'interface sur laquel le serveur communique avec les clients
> -L Permet de spécifier l'adresse IP du serveur
> -R Permet de spécifier la 1er adresse des clients

### Activation de routage et du proxy ARP
Sur le serveur, on active les fonctions de routage et de proxy ARP, permattant à chaque client d'avoir un accès Internet.

```bash=
root@214-2:/etc/ppp# echo 1 > /proc/sys/net/ipv4/ip_forward

root@214-2:/etc/ppp# echo 1 > /proc/sys/net/ipv4/conf/enx3c18a002b9be/proxy_arp
```
### Vérification de l'accès à internet
On test ensuite que chaque machine étant relié au serveur PPP ait accès à Internet. ET on vérifie que l'interface PPP est bien présente.
#### Oridnateur de Théo
```bash=
test@232-22:/$ ping 8.8.8.8
PING 8.8.8.8 (8.8.8.8) 56(84) bytes of data.
64 bytes from 8.8.8.8: icmp_seq=1 ttl=53 time=659 ms
64 bytes from 8.8.8.8: icmp_seq=2 ttl=53 time=7.85 ms
64 bytes from 8.8.8.8: icmp_seq=3 ttl=53 time=7.31 ms
^C
--- 8.8.8.8 ping statistics ---
3 packets transmitted, 3 received, 0% packet loss, time 4ms
rtt min/avg/max/mdev = 7.313/224.837/659.353/307.249 ms

test@232-22:/$ ip a s ppp0
42: ppp0: <POINTOPOINT,MULTICAST,NOARP,UP,LOWER_UP> mtu 1492 qdisc pfifo_fast state UNKNOWN group default qlen 3
    link/ppp 
    inet 10.214.2.104 peer 10.214.2.100/32 scope global ppp0
       valid_lft forever preferred_lft forever
```
#### Ordinateur de Pablo
```bash=
test@214-3:/etc/ppp/peers$ ping -c 4 8.8.8.8
PING 8.8.8.8 (8.8.8.8) 56(84) bytes of data.
64 bytes from 8.8.8.8: icmp_seq=1 ttl=53 time=8.13 ms
64 bytes from 8.8.8.8: icmp_seq=2 ttl=53 time=7.33 ms
64 bytes from 8.8.8.8: icmp_seq=3 ttl=53 time=7.17 ms
64 bytes from 8.8.8.8: icmp_seq=4 ttl=53 time=7.64 ms

--- 8.8.8.8 ping statistics ---
4 packets transmitted, 4 received, 0% packet loss, time 7ms
rtt min/avg/max/mdev = 7.173/7.567/8.128/0.374 ms
test@214-3:/etc/ppp/peers$ ip a s ppp0
36: ppp0: <POINTOPOINT,MULTICAST,NOARP,UP,LOWER_UP> mtu 1492 qdisc pfifo_fast state UNKNOWN group default qlen 3
    link/ppp 
    inet 10.214.2.102 peer 10.214.2.100/32 scope global ppp0
       valid_lft forever preferred_lft forever
```
#### Ordinateur de Yunhan
```bash=
test@214-4:/etc$ ping 8.8.8.8 -c 4
PING 8.8.8.8 (8.8.8.8) 56(84) bytes of data.
64 bytes from 8.8.8.8: icmp_seq=1 ttl=53 time=7.44 ms
64 bytes from 8.8.8.8: icmp_seq=2 ttl=53 time=6.97 ms
64 bytes from 8.8.8.8: icmp_seq=3 ttl=53 time=7.44 ms
64 bytes from 8.8.8.8: icmp_seq=4 ttl=53 time=8.10 ms

--- 8.8.8.8 ping statistics ---
4 packets transmitted, 4 received, 0% packet loss, time 8ms
rtt min/avg/max/mdev = 6.974/7.487/8.102/0.406 ms
test@214-4:/etc$ ip a s ppp0
28: ppp0: mtu 1492 qdisc pfifo_fast state UNKNOWN group default qlen 3
link/ppp
inet 10.214.2.110 peer 10.214.2.100/32 scope global ppp0
valid_lft forever preferred_lft forever
```

### Connexion PPPOE comprenant une machine Windows
Pour que la connexion PPPoE puisse être établi par les clients Windows, il faut préciser un DNS dans les paramètres du serveur.
On ajoute donc dans ***/etc/ppp/pppoe-server-options*** : 
```bash=1
ms-dns 8.8.8.8
ms-dns 8.8.4.4
```

## Conclusion

En conclusion, on remarque qu'après avoir travaillé sur le protocole PPP celui est indispensable pour les fournir d'accès. Car il permet de mettre en place assez facilement une connexion entre son réseau et celui de l'abboné.
Ce protocole reste flexible, est peut-être agrémenté de pleins de focntionnalitées, comme l'authentification ou la gestion de différents systèmes d'exploitation.



