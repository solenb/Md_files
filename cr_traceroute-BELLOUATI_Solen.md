---
title: cr_traceroute-BELLOUATI_Solen
---
# Compte Rendu Traceroute
## Fonctionnement par défaut Traceroute
**Solen BELLOUATI**
**RT2**
Vous pouvez consulter ce compte rendu en MarkDown à l'adresse suivante :
**https://md.iutbeziers.org/s/H1bVrEJnS#**

Traceroute est un programme utilitaire ayant pour but de retracer le chemin parcourut par un datagramme IP, partant de la machine hôte jusqu'à une machine cible.

Les paquets IP passent par différents routeur (bonds), avant d'arriver à la machine cible, c'est sur ce principe que s'appuie Traceroute.

En fonctionnement par défaut Traceroute envoie 3 paquet en UDP ayant un TimeToLive (TTL) égal à 1. Ce procèsus va permettre de récupérer l'adresse IP du associée au premier saut.
Le TTL est ensuite incrémenté pour atteindre par la suite tous les sauts séparant les deux machines.

En effet car d'après la RFC 792, quand un paquet arrive à un routeur pour être routé, son TLL est décrémenté de 1. Mais lorsque celui-ci est nul, le routeur peut renvoyer un message en ICMP spécifiant l'erreur TTL exceeded.
C'est à ce moment que l'expéditeur peut récupérer l'adresse du routeur ayant envoyé le message d'erreur car c'est l'adresse de l'expéditeur.
### Illustration du fonctionnement de Traceroute
![](https://i.imgur.com/vIoDwX3.png)

## Modularité de l'utilitaire Traceroute

L'utilitaire Traceroute fonctionne correctement par défaut, mais il peut rencontrer de nombreux problèmes.
POur y remédier cet utilitaire est livré avec de nombreuses options permettant de le modeler à sa manière pour qu'il réponde aux attentes de chacun.
### Voici tous les options utiles que j'ai utilisé pour créer mon script

| Option | Explication |
|:------:|:-----------:|
|**-p**| Permet de modifier le port de destination |
|**-U**| Permet d'utiliser la méthode **UDP** (non connecté)|
|**-I**| Permet d'utiliser la méthode **ICMP**|
|**-T**|Permet d'utiliser la méthode **TCP** (connecté)|
|**-n**|Permet de ne **pas** faire de **requête DNS** pour les retour de messages d'erreur|
|**-A**|Permet de récupérer l'**AS** du routeur ciblé|
|**-f**|Permet de le TTL minimum que le paquet va prendre|
|**-m**|Permet de le TTL maximum que le paquet va prendre|
|**-w**|Permet de régler le temps d'attente d'une réponse d'un saut|
|**-q**|Permet de régler le nombre de paquet envoyé par saut|

## Explication du TLL

Le TTL est un champ spécifié dans l'en-tête d'un datagramme IP, et détaillé dans la RFC 792. Sa taille est de 8 bits.
Il indique la durée maximale pendant laquelle le datagramme est autorisé à rester dans les tuyaux d'Internet.
Sa valeur recommandée est de 64, d'après la RFC 1700, mais elle est souvent définie par l'OS.

## Conclusion
L'avantage de l'algorithme de Traceroute, c'est que chaque routeur dispose déjà de la fonction d'envoie de messages TTL exceeded. Aucun code spécial n'est requis.
Les inconvénients sont le nombre de paquets généré, le fait que le chemin d'accès peut différer au cours de ce processus.
De plus, cet algorithme ne retrace pas le chemin de retour, qui peut différer du saut.



