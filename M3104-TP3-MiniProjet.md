---
title: M3104-TP3-MiniProjet
---
# M3104 - MiniProjet
Afin de mettre en place un annuaire Ldap pour les étudiants RT2, il faut d'abord mettre en place le serveur Ldap :
```bash=1
apt-get install slapd ldap-utils

dpkg-reconfigure slapd
```
Il faut configurer slapd avec dc=iutbeziers,dc=fr.
Grâce au script de création en BASH des entrées étudiants, on créer le fichier au format LDIF à partir de **entreldif.sh** le fichier qui est retourné est **liste_RT2.ldif**.
Afin de créer l'organisation du DIT il faut ajouter le fichier **arbre_RT2.ldif** à l'annuaire grâce à la commande suivante : 
```bash=1 
slapadd -l arbre_RT2.ldif 
```
IL faut ensuite ajouter les entrées étudiants grâce à la commande suiavnte : 
```bash=1
slapadd -l liste_RT2.ldif 
```


