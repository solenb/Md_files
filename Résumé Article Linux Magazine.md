---
title: Résumé Article Linux Magazine
---
> [name=Solen BELLOUATI ] [time=mer, 28 mars, 2020]

Il est préférable de consulter le document à l'adresse suivante : https://md.iutbeziers.org/s/S1MDeiyLL#
# Résumé Article Linux Magazine

Nom de l'auteur  :  Romain Pelisse
Date de publication : Décembre 2019
Source : Linux Magazine*
Titre : Git, les gestionnaire de versions pour tous les développeurs
Thèmes : Outils Git et examples, Correspondance entre outils et développeurs


L'auteur de l'article nous décrit un scénario, d'un passionné travaillant pour le compte d'une association.
Celui-ci a mis en place en place un serveur web, il est malheureusement le seul personnel qualifié. Mais d'autres personnes ont besoin d'accéder au site pour y implémenter des annonces.
Dans un premier, il met en place un système assez rudimentaire, et risqué. Car il n'y a qu'une seul et unique version du site web en fonction, ce qui multiplie les risques d'indisponibilité et de pertes de donnèes sensibles.
L'article est donc là pour l'aider à mettre en place une solution viable, afin d'avoir un historique permanent des version de son site web, grace à Git.
En résumé Git est un outils de gestion de version, ce qui veut dire qu'il permet de mettre en lieu sûr le code (le travaille) qu'un développeur aura produit.
Les quatre mots de vocabulaires essentiels pour comprendre l'univers de cet outils sont : 
- **Historique de révision** : Permettant de laisser toute trace de modification dans le projet en cours
- **Commit** : Ce sont toutes les opérations de modifications répertoriées, elles sont accompagnées de commentaires décrivant la modification
- **Espace de travail** : C'est l'arborescence contenant les fichier de l'utilisateur
- **Branches** : Ce sont les différentes voies que le projet a empreinté, elles permettent le travaillent en parallèle, mais aussi la sauvegarde des différentes versions

L'article nous décrit ensiute la marche à suivre afin de connaitre les bases du fonctionnement de l'outils Git.
Premièrement, on installe l'outils sur sont ordinateur via des lignes de commande (il se trouve sur le système d'exploitation Linux) : ***sudo dnf install git***

Ensuite, on se place dans un dossier où l'on va initialiser le dêpot Git : 
- ***cd /web*** -> on se place dans le dossier *web*
- ***git init*** -> on initialise Git

Le dêpot est maintenant initialisé, ce qui signifie que l'on va pouvoir commencer à travailler.

L'article nous décrit par la suite, de nombreuses commandes : 
 -  ***git add*** -> permettant d'activer le suivi sur les fichier contenus dans le dossier courant (en l'occurence /web)
 -  ***git status*** -> permettant d'afficher toutes les informations importantes sur les fichiers suivis et les commit
 -  ***git log*** -> permettant d'afficher tout l'historique des opérations faites en local sur les fichiers (toutes les commandes effectuées)
 -  ***git diff*** -> permettant de voir les modifications apportées entre la version actuelle et les versions antérieurs
 
 La dernière commande est essentielles, elle permet de revenir à une version précédente du code : 
 ***git checkout 'numéro_du_commit' -b 'numéro_dernière_version'***
 
 On peut aussi modifier les étiquettes placé par défaut sur les numéros de révisions, qui sont très long et impossibles à retenir : 
 ***git tag numéro_de_version v1***
