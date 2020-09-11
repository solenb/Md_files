---
title: M3108 TD1 SNMP
---
# M3108 - TD1 SNMP client

## I- Installation du client SNMP
1. Le client a été correctement configuré et lancé dans un conteneur Docker.
2. Les équipements accessibles de la salle sous SNMP via nmap sur le réseau 10.6.0 sont 10.6.0.1 : 
```bash=1 
root@801f99e73f7b:/# nmap -sP 10.6.0.1
Starting Nmap 7.70 ( https://nmap.org ) at 2019-12-06 08:31 UTC
Nmap scan report for 10.6.0.1
Host is up (0.00020s latency).
Nmap done: 1 IP address (1 host up) scanned in 0.26 seconds
```
## II- Interrogation de serveur Windows via SNMP
### 1) Interrogation de base d’un des serveurs 10.6.0 via snmp.
1. L'ensemble des informations du serveur sont accessibles via la commande ***snmpwalk*** suivante : 
2. La commande permettant de retrouver le système d'exploitation de la machine : 
```bash=1
root@snmpserver:/# snmpget -v2c -c public 10.6.0.1 sysUpTime.0
DISMAN-EVENT-MIB::sysUpTimeInstance = Timeticks: (1994672753) 230 days, 20:45:27.53
```
3. La commande permettant d'afficher l'uptime est : 
```bash=1
root@snmpserver:/# snmpget -v2c -c public 10.6.0.1 sysUpTime.0
DISMAN-EVENT-MIB::sysUpTimeInstance = Timeticks: (1994672753) 230 days, 20:45:27.53
```
4. L'arbre système de la MIB est le suivant : 
```bash=1
root@801f99e73f7b:/# snmptranslate -On -Tp SNMPv2-MIB::system
+--system(1)
   |
   +-- -R-- String    sysDescr(1)
   |        Textual Convention: DisplayString
   |        Size: 0..255
   +-- -R-- ObjID     sysObjectID(2)
   +-- -R-- TimeTicks sysUpTime(3)
   |  |
   |  +--sysUpTimeInstance(0)
   |
   +-- -RW- String    sysContact(4)
   |        Textual Convention: DisplayString
   |        Size: 0..255
   +-- -RW- String    sysName(5)
   |        Textual Convention: DisplayString
   |        Size: 0..255
   +-- -RW- String    sysLocation(6)
   |        Textual Convention: DisplayString
   |        Size: 0..255
   +-- -R-- INTEGER   sysServices(7)
   |        Range: 0..127
   +-- -R-- TimeTicks sysORLastChange(8)
   |        Textual Convention: TimeStamp
   |
   +--sysORTable(9)
      |
      +--sysOREntry(1)
         |  Index: sysORIndex
         |
         +-- ---- INTEGER   sysORIndex(1)
         |        Range: 1..2147483647
         +-- -R-- ObjID     sysORID(2)
         +-- -R-- String    sysORDescr(3)
         |        Textual Convention: DisplayString
         |        Size: 0..255
         +-- -R-- TimeTicks sysORUpTime(4)
                  Textual Convention: TimeStamp
```
5. La liste des connections TCP et UDP du serveur SNMP sont les suivantes : 
```bash=1
root@snmpserver:/# snmpnetstat -v2c -c public 10.6.0.1
Active Internet (udp) Connections
Proto Local Address               Remote Address                PID
udp4  *.*                         *.*                             0
```
6. La commande ***snmpgetnext*** permet de récupérer l'OID à la suite de l'OID spécifié dans la commande.
Dans l'arbre l'OID placé après celui du UpTime est contact, donc on récupère le contact avec ***snmpgetnext***: 
```bash=1 
r=root@snmpserver:/# snmpgetnext -v2c -c public 10.6.0.1 sysUpTime.0
SNMPv2-MIB::sysContact.0 = STRING: M. Duban
```
## III- nterrogation de serveurs Linux via SNMP

Le containeur serveur SNMP est accessible : 
```bash=1
root@snmpserver:/# snmpget -v2c -c publicbeziers 127.0.0.1 sysDescr.0
SNMPv2-MIB::sysDescr.0 = STRING: Linux snmpserver 4.19.0-6-amd64 #1 SMP Debian 4.19.67-2+deb10u1 (2019-09-20) x86_64
```
Taille totale de la mémoire : 
```bash=1
root@snmpserver:/# snmpget -v2c -c publicbeziers 127.0.0.1 memTotalReal.0
UCD-SNMP-MIB::memTotalReal.0 = INTEGER: 16332904 kB
```
UpTime : 
```bash=1
root@snmpserver:/# snmpget -v2c -c publicbeziers 127.0.0.1 sysUpTime.0
DISMAN-EVENT-MIB::sysUpTimeInstance = Timeticks: (608935) 1:41:29.35
```
Process de la machine : 
```bash=1
root@snmpserver:/# snmpwalk -v2c -c publicbeziers 127.0.0.1 HOST-RESOURCES-MIB::hrSWRun
[...]
HOST-RESOURCES-MIB::hrSWRunType.259 = INTEGER: application(4)
HOST-RESOURCES-MIB::hrSWRunStatus.1 = INTEGER: runnable(2)
HOST-RESOURCES-MIB::hrSWRunStatus.6 = INTEGER: running(1)
HOST-RESOURCES-MIB::hrSWRunStatus.191 = INTEGER: runnable(2)
HOST-RESOURCES-MIB::hrSWRunStatus.259 = INTEGER: runnable(2)
```

