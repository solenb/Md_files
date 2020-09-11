---
title: TD4-M3206 IPERF
---
Le lien du CodiMD est disponible à cette adresse : 
https://md.iutbeziers.org/s/H1tO4PDAS#

# M3206-TD4_Iperf
## Tests de la bande passante sans perturbation
### Test de Iperf sans perturbation en TCP
```bash=
test@214-2:~/Téléchargements/TD4$ iperf -c 10.12.0.1
------------------------------------------------------------
Client connecting to 10.12.0.1, TCP port 5001
TCP window size: 85.0 KByte (default)
------------------------------------------------------------
[  3] local 10.12.0.2 port 44984 connected with 10.12.0.1 port 5001
[ ID] Interval       Transfer     Bandwidth
[  3]  0.0-10.0 sec  72.7 GBytes  62.5 Gbits/sec
```
### Test de Iperf sans perturbation en UDP
```bash=
test@214-2:~/Téléchargements/TD4$ iperf -c 10.12.0.1 -u
------------------------------------------------------------
Client connecting to 10.12.0.1, UDP port 5001
Sending 1470 byte datagrams, IPG target: 11215.21 us (kalman adjust)
UDP buffer size:  208 KByte (default)
------------------------------------------------------------
[  3] local 10.12.0.2 port 43524 connected with 10.12.0.1 port 5001
[ ID] Interval       Transfer     Bandwidth
[  3]  0.0-10.0 sec  1.25 MBytes  1.05 Mbits/sec
[  3] Sent 892 datagrams
[  3] Server Report:
[  3]  0.0-10.0 sec  1.25 MBytes  1.05 Mbits/sec   0.002 ms    0/  892 (0%)
```
## Graphiques de la bande passante en fonction des différentes perturbations en TCP
![](https://i.imgur.com/1NQQTdW.png)

![](https://i.imgur.com/JZPvKA1.png)

![](https://i.imgur.com/7yfiTd3.png)

## Graphiques de la bande passante en fonction des différentes perturbations en UDP

![](https://imgur.com/n5Jzig6.png)

![](https://i.imgur.com/6VWbM0d.png)

![](https://i.imgur.com/QFESTQE.png)





