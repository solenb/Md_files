---
title: M3104-TP1 Annuaires
---
# M3104 TP1 - Annuaires unifiés 
### Installation d’OpenLDAP à partir des packages, découverte dela configuration d’OpenLdap

1. L'installation d'OpenLDAP a été effectuée. 
2. 1. Grace à la commande ***apt download slapd*** on récupère tout l'arbre de l'annuaire dans sa machine.
     2. La commande ***slaptest*** vérifie si  les fichiers de config sont correctes 
```bash=1
root@debian:~# slaptest
config file testing succeeded
```
2. 3. Le miroir de la configuration d'Openldap se trouve dans le dossier ***/etc/ldap/slapd.d***
```bash=1
root@debian:/etc/ldap/slapd.d# tree -F
.
├── cn=config/
│   ├── cn=module{0}.ldif
│   ├── cn=schema/
│   │   ├── cn={0}core.ldif
│   │   ├── cn={1}cosine.ldif
│   │   ├── cn={2}nis.ldif
│   │   └── cn={3}inetorgperson.ldif
│   ├── cn=schema.ldif
│   ├── olcBackend={0}mdb.ldif
│   ├── olcDatabase={0}config.ldif
│   ├── olcDatabase={-1}frontend.ldif
│   └── olcDatabase={1}mdb.ldif
└── cn=config.ldif

```
2. 4. ???
    5.  La liste de la configuration grace au ldapsearch est la suivante:
```bash=1
root@debian:/etc/ldap/schema# ldapsearch -LLL -Y EXTERNAL -H ldapi:/// -b cn=config dn
SASL/EXTERNAL authentication started
SASL username: gidNumber=0+uidNumber=0,cn=peercred,cn=external,cn=auth
SASL SSF: 0
dn: cn=config

dn: cn=module{0},cn=config

dn: cn=schema,cn=config

dn: cn={0}core,cn=schema,cn=config

dn: cn={1}cosine,cn=schema,cn=config

dn: cn={2}nis,cn=schema,cn=config

dn: cn={3}inetorgperson,cn=schema,cn=config

dn: olcBackend={0}mdb,cn=config

dn: olcDatabase={-1}frontend,cn=config

dn: olcDatabase={0}config,cn=config

dn: olcDatabase={1}mdb,cn=config
```
L'option ***-Y EXTERNAL*** permet d'accéder en root à l'annuaire local.
L'option ***ldapi*** permet de sonder la pronfondeur voulue.


2. 6. 
