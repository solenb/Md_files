---
title:QCM_Supervision_M3108
---
# QCM Supervision M3108
### Question 1- Que permet de faire la commande suivante ? 
- ***snmpget -v2c -c xxx x.x.x.x sysUpTime.0***
    - [ ] 1- Afficher l'OID lié au temps de fonctionnement du serveur SNMP
    - [ ] 2- Afficher l'OID lié au nombre de ports actifs du serveur SNMP
    - [ ] 3- Afficher l'OID lié au temps de démarrage du serveur SNMP
    - [ ] 4- Afficher le MIB du serveur SNMP

**La réponse correcte est la *1*, cette commande retourne l'OID lié au temps depuis lequel le serveur SNMP est en marche.**

### Question 2- Que permet de faire la commande suivante ?
- ***snmpnetstat -v2c -c xxxx x.x.x.x***
    - [ ] 1- Afficher le traffic réseau du serveur SNMP
    - [ ] 2- Afficher la liste des connexions TCP et UDP du serveur SNMP
    - [ ] 3- Afficher le nombre de connexions par seconde au serveur SNMP
    - [ ] 4- Afficher les liens disponibles sur le serveur SNMP

**La réponse correcte est la *2*, car netstat permet d'afficher la liste des connections TCP et UDP du serveur SNMP** 

### Question 3- Quel est la différence entre les commandes snmpget et snmpwalk ?
-
    - [ ] 1- Les deux commandes correspondent à des versions différentes de SNMP
    - [ ] 2- Snmpwalk permet de récupérer la valeur d’un OID feuille alors que snmpget permet de récupérer toutes les valeurs d’un OID « noeud »
    - [ ] 3- Snmpget permet de récupérer la valeur d’un OID feuille alors que snmpwalk permet de récupérer toutes les valeurs d’un OID « noeud »
    - [ ] 4- Il n'y a pas de différence, elles remplissent la même fonction

**La réponse correcte est la *3*, snmpget permet de récupérer la valeur d’un OID feuille alors que snmpwalk permet de récupérer toutes les valeurs d’un OID « noeud »**

### Question 4- 