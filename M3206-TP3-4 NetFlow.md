---
title: M3206-TP3/4 NetFlow
---
# M3206-TP3/4 NetFlow
> [name= Solen BELLOUATI][name= Pablo HOUSSE]
> Vous pouvez accéder au compte rendu de ce TP en Markdown à l'adresse suivante : https://md.iutbeziers.org/s/HkJiU-UlU#

### 2- Réalisation d’une maquette Netflow sur la salle

#### 2.1- Configuration de la sonde fprobe

La configuration du fichier ***/etc/default/fprobe*** est la suivante :
```bash=
#fprobe default configuration file

INTERFACE="eth0"
FLOW_COLLECTOR="localhost:1564 10.202.0.64:1565 10.202.0.172:1569 10.202.0.94:1563 10.202.0.137:1561 10.202.0.73:1572 10.202.10.1:1570 10.202.0.161:1573 10.202.0.96:1564 10.202.14.1:1568 10.202.0.189:1574 10.202.0.144:1557 10.202.0.168:1558"

#fprobe can't distinguish IP packet from other (e.g. ARP)
OTHER_ARGS="-fip"
```

#### 2.2- Configuration de nfsen
Il faut ensuite lancer le docker-compose NFSEN et configurer le dossier ***/data/nfsen/etc/nfsen.conf*** :
```bash=
%sources = (
    'ipfix-global'  => { 'port' => '4739', 'col' => '#0000ff', 'type' => 'netflow' },
    'sflow-global'  => { 'port' => '6343', 'col' => '#0000ff', 'type' => 'sflow' },
    'nat' => { 'port' => '1555', 'col' =>  '#B71818', 'type' => 'netflow' },
    'bb'  => { 'port' => '1559', 'col' => '#50B718', 'type' => 'netflow' },
    'Juclem' => {'port' => '1565','col'=> '#FFF933', 'type'=>'netflow'},
    'theo' => {'port' => '1569','col' => '#3339FF', 'type' => 'netflow'},
    'hugo' => {'port' => '1563', 'col' => '#FF3333', 'type' => 'netflow'},
    'sam' => {'port' => '1561', 'col' => '#FF33C1', 'type' => 'netflow'},
    'grou' => {'port' => '1572', 'col' => '#000000', 'type' => 'netflow'},
    'thomas' => {'port' => '1570', 'col' => '#00FCF0', 'type' => 'netflow'},
    'nathan' => {'port' => '1573', 'col' => '#00FCA4', 'type' => 'netflow'},
    'solenPab' => {'port' => '1564', 'col' => '#FC0000', 'type' => 'netflow'},
    'anon' => {'port' => '1568', 'col' => '#BB00FC', 'type' => 'netflow'},
    'zhouQ' => {'port' => '1574', 'col' => '#FCCA00', 'type' => 'netflow'},
);
```
On peut vérifier que notre containeur récpetion bien le traffic via Netflow en lançant la commande ***tcpdump -n -i any*** dans le containeur : 
```bash=
tcpdump: verbose output suppressed, use -v or -vv for full protocol decode
listening on any, link-type LINUX_SLL (Linux cooked), capture size 262144 bytes
14:23:45.005399 IP 172.17.0.1.36710 > 172.17.0.2.1564: UDP, length 72
14:23:45.985466 IP 10.202.0.64.55278 > 172.17.0.2.1565: UDP, length 72
14:23:47.005963 IP 10.202.0.137.47168 > 172.17.0.2.1564: UDP, length 72
14:23:48.998225 IP 10.202.0.168.54468 > 172.17.0.2.1564: UDP, length 72
14:23:49.904520 IP 10.202.0.161.45341 > 172.17.0.2.1573: UDP, length 216
14:23:50.001615 IP 172.17.0.1.36710 > 172.17.0.2.1564: UDP, length 120
14:23:54.098508 ARP, Request who-has 172.17.0.2 tell 172.17.0.1, length 28
14:23:54.098526 ARP, Reply 172.17.0.2 is-at 02:42:ac:11:00:02, length 28
```



Enfin en se connectant à l'interface web on obtient les graphiques suivants : 
![](https://i.imgur.com/BGqt9wI.png)
![](https://i.imgur.com/rE0sfSw.png)


#### 3- Utilisation des profils
On a créé les profils dédiés aux requêtes HTTP et DNS, ainsi que pour chacun un profil TOTAL_TRAFFIC permettant de comparer les deux : 

![](https://i.imgur.com/PmffZvQ.png)

![](https://i.imgur.com/RiCxLW8.png)
> ***Traffic HTTP :*** [color=black]
>![](https://i.imgur.com/PWr4qSk.png)
>![](https://i.imgur.com/M5Kc9CO.png)

> ***Traffic DNS :*** [color=black]
>![](https://i.imgur.com/lKXPpGh.png)

#### 5- Entrainement à l’analyse de flux sur la VM de l’agence euro-péenne de la sécurité (enisa)

On commence par télécharger l'ova sur store.iutbeziers.fr, puis on la lance.
On démarre ensuite NFSEN avec:
```sudo /data/nfsen/start.sh```
On peux maintenant accéder à 127.0.0.1/nfsen.nfsen.php avec un navigateur:
![](https://i.imgur.com/rIVLlvk.png)

On liste ensuite le contenu du répertoire /data/nfsen/profiles-data/live/upstream1
```bash=1
enisa@enisa-vm:/data/nfsen/profiles-data/live/upstream1$ ls -lh nfcapd.*
-rwxr-x--- 1 www-data www-data 343K Jan 16  2015 nfcapd.200702232000
-rwxr-x--- 1 www-data www-data 283K Jan 16  2015 nfcapd.200702232005
-rwxr-x--- 1 www-data www-data 354K Jan 16  2015 nfcapd.200702232010
-rwxr-x--- 1 www-data www-data 312K Jan 16  2015 nfcapd.200702232015
-rwxr-x--- 1 www-data www-data 344K Jan 16  2015 nfcapd.200702232020
-rwxr-x--- 1 www-data www-data 341K Jan 16  2015 nfcapd.200702232025
-rwxr-x--- 1 www-data www-data 277K Jan 16  2015 nfcapd.200702232030
-rwxr-x--- 1 www-data www-data 296K Jan 16  2015 nfcapd.200702232035
-rwxr-x--- 1 www-data www-data 347K Jan 16  2015 nfcapd.200702232040
-rwxr-x--- 1 www-data www-data 290K Jan 16  2015 nfcapd.200702232045
-rwxr-x--- 1 www-data www-data 288K Jan 16  2015 nfcapd.200702232050
-rwxr-x--- 1 www-data www-data 358K Jan 16  2015 nfcapd.200702232055
-rwxr-x--- 1 www-data www-data 375K Jan 16  2015 nfcapd.200702232100
-rwxr-x--- 1 www-data www-data 363K Jan 16  2015 nfcapd.200702232105
-rwxr-x--- 1 www-data www-data 321K Jan 16  2015 nfcapd.200702232110
-rwxr-x--- 1 www-data www-data 352K Jan 16  2015 nfcapd.200702232115
-rwxr-x--- 1 www-data www-data 243K Jan 16  2015 nfcapd.200702232120
-rwxr-x--- 1 www-data www-data 220K Jan 16  2015 nfcapd.200702232125
-rwxr-x--- 1 www-data www-data 266K Jan 16  2015 nfcapd.200702232130
-rwxr-x--- 1 www-data www-data 282K Jan 16  2015 nfcapd.200702232135
-rwxr-x--- 1 www-data www-data 249K Jan 16  2015 nfcapd.200702232140
-rwxr-x--- 1 www-data www-data 234K Jan 16  2015 nfcapd.200702232145
-rwxr-x--- 1 www-data www-data 279K Jan 16  2015 nfcapd.200702232150
-rwxr-x--- 1 www-data www-data 202K Jan 16  2015 nfcapd.200702232155
-rwxr-x--- 1 www-data www-data 262K Jan 16  2015 nfcapd.200702232200
-rwxr-x--- 1 www-data www-data 235K Jan 16  2015 nfcapd.200702232205
-rwxr-x--- 1 www-data www-data 176K Jan 16  2015 nfcapd.200702232210
-rwxr-x--- 1 www-data www-data 238K Jan 16  2015 nfcapd.200702232215
-rwxr-x--- 1 www-data www-data 225K Jan 16  2015 nfcapd.200702232220
-rwxr-x--- 1 www-data www-data 226K Jan 16  2015 nfcapd.200702232225
-rwxr-x--- 1 www-data www-data 194K Jan 16  2015 nfcapd.200702232230
-rwxr-x--- 1 www-data www-data 218K Jan 16  2015 nfcapd.200702232235
-rwxr-x--- 1 www-data www-data 230K Jan 16  2015 nfcapd.200702232240
-rwxr-x--- 1 www-data www-data 265K Jan 16  2015 nfcapd.200702232245
-rwxr-x--- 1 www-data www-data 150K Jan 16  2015 nfcapd.200702232250
-rwxr-x--- 1 www-data www-data 172K Jan 16  2015 nfcapd.200702232255
-rwxr-x--- 1 www-data www-data 216K Jan 16  2015 nfcapd.200702232300
-rwxr-x--- 1 www-data www-data 204K Jan 16  2015 nfcapd.200702232305
-rwxr-x--- 1 www-data www-data 186K Jan 16  2015 nfcapd.200702232310
-rwxr-x--- 1 www-data www-data 175K Jan 16  2015 nfcapd.200702232315
-rwxr-x--- 1 www-data www-data 161K Jan 16  2015 nfcapd.200702232320
-rwxr-x--- 1 www-data www-data 153K Jan 16  2015 nfcapd.200702232325
-rwxr-x--- 1 www-data www-data 161K Jan 16  2015 nfcapd.200702232330
-rwxr-x--- 1 www-data www-data 155K Jan 16  2015 nfcapd.200702232335
-rwxr-x--- 1 www-data www-data 195K Jan 16  2015 nfcapd.200702232340
-rwxr-x--- 1 www-data www-data 151K Jan 16  2015 nfcapd.200702232345
-rwxr-x--- 1 www-data www-data 157K Jan 16  2015 nfcapd.200702232350
-rwxr-x--- 1 www-data www-data 135K Jan 16  2015 nfcapd.200702232355
-rwxr-x--- 1 www-data www-data 107K Jan 16  2015 nfcapd.200702240000
-rwxr-x--- 1 www-data www-data 142K Jan 16  2015 nfcapd.200702240005
-rwxr-x--- 1 www-data www-data  99K Jan 16  2015 nfcapd.200702240010
-rwxr-x--- 1 www-data www-data 171K Jan 16  2015 nfcapd.200702240015
-rwxr-x--- 1 www-data www-data 139K Jan 16  2015 nfcapd.200702240020
-rwxr-x--- 1 www-data www-data 120K Jan 16  2015 nfcapd.200702240025
-rwxr-x--- 1 www-data www-data 152K Jan 16  2015 nfcapd.200702240030
-rwxr-x--- 1 www-data www-data 157K Jan 16  2015 nfcapd.200702240035
-rwxr-x--- 1 www-data www-data 106K Jan 16  2015 nfcapd.200702240040
-rwxr-x--- 1 www-data www-data 112K Jan 16  2015 nfcapd.200702240045
-rwxr-x--- 1 www-data www-data 132K Jan 16  2015 nfcapd.200702240050
-rwxr-x--- 1 www-data www-data 168K Jan 16  2015 nfcapd.200702240055
-rwxr-x--- 1 www-data www-data 127K Jan 16  2015 nfcapd.200702240100
-rwxr-x--- 1 www-data www-data 152K Jan 16  2015 nfcapd.200702240105
-rwxr-x--- 1 www-data www-data  79K Jan 16  2015 nfcapd.200702240110
-rwxr-x--- 1 www-data www-data 124K Jan 16  2015 nfcapd.200702240115
-rwxr-x--- 1 www-data www-data 113K Jan 16  2015 nfcapd.200702240120
-rwxr-x--- 1 www-data www-data  88K Jan 16  2015 nfcapd.200702240125
-rwxr-x--- 1 www-data www-data 137K Jan 16  2015 nfcapd.200702240130
-rwxr-x--- 1 www-data www-data 104K Jan 16  2015 nfcapd.200702240135
-rwxr-x--- 1 www-data www-data 100K Jan 16  2015 nfcapd.200702240140
-rwxr-x--- 1 www-data www-data  94K Jan 16  2015 nfcapd.200702240145
-rwxr-x--- 1 www-data www-data 100K Jan 16  2015 nfcapd.200702240150
-rwxr-x--- 1 www-data www-data  73K Jan 16  2015 nfcapd.200702240155
-rwxr-x--- 1 www-data www-data  94K Jan 16  2015 nfcapd.200702240200
-rwxr-x--- 1 www-data www-data 123K Jan 16  2015 nfcapd.200702240205
-rwxr-x--- 1 www-data www-data  90K Jan 16  2015 nfcapd.200702240210
-rwxr-x--- 1 www-data www-data 130K Jan 16  2015 nfcapd.200702240215
-rwxr-x--- 1 www-data www-data  84K Jan 16  2015 nfcapd.200702240220
-rwxr-x--- 1 www-data www-data 108K Jan 16  2015 nfcapd.200702240225
-rwxr-x--- 1 www-data www-data  87K Jan 16  2015 nfcapd.200702240230
-rwxr-x--- 1 www-data www-data  95K Jan 16  2015 nfcapd.200702240235
-rwxr-x--- 1 www-data www-data 105K Jan 16  2015 nfcapd.200702240240
-rwxr-x--- 1 www-data www-data 147K Jan 16  2015 nfcapd.200702240245
-rwxr-x--- 1 www-data www-data 130K Jan 16  2015 nfcapd.200702240250
-rwxr-x--- 1 www-data www-data 117K Jan 16  2015 nfcapd.200702240255
-rwxr-x--- 1 www-data www-data 110K Jan 16  2015 nfcapd.200702240300
-rwxr-x--- 1 www-data www-data 104K Jan 16  2015 nfcapd.200702240305
-rwxr-x--- 1 www-data www-data 113K Jan 16  2015 nfcapd.200702240310
-rwxr-x--- 1 www-data www-data 132K Jan 16  2015 nfcapd.200702240315
-rwxr-x--- 1 www-data www-data 123K Jan 16  2015 nfcapd.200702240320
-rwxr-x--- 1 www-data www-data  67K Jan 16  2015 nfcapd.200702240325
-rwxr-x--- 1 www-data www-data  88K Jan 16  2015 nfcapd.200702240330
-rwxr-x--- 1 www-data www-data 115K Jan 16  2015 nfcapd.200702240335
-rwxr-x--- 1 www-data www-data 136K Jan 16  2015 nfcapd.200702240340
-rwxr-x--- 1 www-data www-data 124K Jan 16  2015 nfcapd.200702240345
-rwxr-x--- 1 www-data www-data 102K Jan 16  2015 nfcapd.200702240350
-rwxr-x--- 1 www-data www-data  88K Jan 16  2015 nfcapd.200702240355
-rwxr-x--- 1 www-data www-data 3.9M Jan 16  2015 nfcapd.200702240400
-rwxr-x--- 1 www-data www-data  11M Jan 16  2015 nfcapd.200702240405
-rwxr-x--- 1 www-data www-data  13M Jan 16  2015 nfcapd.200702240410
-rwxr-x--- 1 www-data www-data  17M Jan 16  2015 nfcapd.200702240415
-rwxr-x--- 1 www-data www-data  11M Jan 16  2015 nfcapd.200702240420
-rwxr-x--- 1 www-data www-data  29M Jan 16  2015 nfcapd.200702240425
-rwxr-x--- 1 www-data www-data  27M Jan 16  2015 nfcapd.200702240430
-rwxr-x--- 1 www-data www-data  16M Jan 16  2015 nfcapd.200702240435
-rwxr-x--- 1 www-data www-data  18M Jan 16  2015 nfcapd.200702240440
-rwxr-x--- 1 www-data www-data  14M Jan 16  2015 nfcapd.200702240445
-rwxr-x--- 1 www-data www-data  17M Jan 16  2015 nfcapd.200702240450
-rwxr-x--- 1 www-data www-data  19M Jan 16  2015 nfcapd.200702240455
-rwxr-x--- 1 www-data www-data  15M Jan 16  2015 nfcapd.200702240500
-rwxr-x--- 1 www-data www-data  15M Jan 16  2015 nfcapd.200702240505
-rwxr-x--- 1 www-data www-data  15M Jan 16  2015 nfcapd.200702240510
-rwxr-x--- 1 www-data www-data  15M Jan 16  2015 nfcapd.200702240515
-rwxr-x--- 1 www-data www-data  18M Jan 16  2015 nfcapd.200702240520
-rwxr-x--- 1 www-data www-data  16M Jan 16  2015 nfcapd.200702240525
-rwxr-x--- 1 www-data www-data  23M Jan 16  2015 nfcapd.200702240530
-rwxr-x--- 1 www-data www-data  15M Jan 16  2015 nfcapd.200702240535
-rwxr-x--- 1 www-data www-data  16M Jan 16  2015 nfcapd.200702240540
-rwxr-x--- 1 www-data www-data  15M Jan 16  2015 nfcapd.200702240545
-rwxr-x--- 1 www-data www-data  18M Jan 16  2015 nfcapd.200702240550
-rwxr-x--- 1 www-data www-data  16M Jan 16  2015 nfcapd.200702240555
-rwxr-x--- 1 www-data www-data  18M Jan 16  2015 nfcapd.200702240600
-rwxr-x--- 1 www-data www-data  17M Jan 16  2015 nfcapd.200702240605
-rwxr-x--- 1 www-data www-data  15M Jan 16  2015 nfcapd.200702240610
-rwxr-x--- 1 www-data www-data  22M Jan 16  2015 nfcapd.200702240615
-rwxr-x--- 1 www-data www-data  15M Jan 16  2015 nfcapd.200702240620
-rwxr-x--- 1 www-data www-data  16M Jan 16  2015 nfcapd.200702240625
-rwxr-x--- 1 www-data www-data  15M Jan 16  2015 nfcapd.200702240630
-rwxr-x--- 1 www-data www-data  15M Jan 16  2015 nfcapd.200702240635
-rwxr-x--- 1 www-data www-data  14M Jan 16  2015 nfcapd.200702240640
-rwxr-x--- 1 www-data www-data  14M Jan 16  2015 nfcapd.200702240645
-rwxr-x--- 1 www-data www-data  15M Jan 16  2015 nfcapd.200702240650
-rwxr-x--- 1 www-data www-data  15M Jan 16  2015 nfcapd.200702240655
-rwxr-x--- 1 www-data www-data  15M Jan 16  2015 nfcapd.200702240700
-rwxr-x--- 1 www-data www-data  16M Jan 16  2015 nfcapd.200702240705
-rwxr-x--- 1 www-data www-data  15M Jan 16  2015 nfcapd.200702240710
-rwxr-x--- 1 www-data www-data  14M Jan 16  2015 nfcapd.200702240715
-rwxr-x--- 1 www-data www-data  13M Jan 16  2015 nfcapd.200702240720
-rwxr-x--- 1 www-data www-data  12M Jan 16  2015 nfcapd.200702240725
-rwxr-x--- 1 www-data www-data  13M Jan 16  2015 nfcapd.200702240730
-rwxr-x--- 1 www-data www-data  14M Jan 16  2015 nfcapd.200702240735
-rwxr-x--- 1 www-data www-data  15M Jan 16  2015 nfcapd.200702240740
-rwxr-x--- 1 www-data www-data  13M Jan 16  2015 nfcapd.200702240745
-rwxr-x--- 1 www-data www-data  16M Jan 16  2015 nfcapd.200702240750
-rwxr-x--- 1 www-data www-data  13M Jan 16  2015 nfcapd.200702240755
-rwxr-x--- 1 www-data www-data  15M Jan 16  2015 nfcapd.200702240800
-rwxr-x--- 1 www-data www-data  12M Jan 16  2015 nfcapd.200702240805
-rwxr-x--- 1 www-data www-data  14M Jan 16  2015 nfcapd.200702240810
-rwxr-x--- 1 www-data www-data  14M Jan 16  2015 nfcapd.200702240815
-rwxr-x--- 1 www-data www-data  14M Jan 16  2015 nfcapd.200702240820
-rwxr-x--- 1 www-data www-data  15M Jan 16  2015 nfcapd.200702240825
-rwxr-x--- 1 www-data www-data  15M Jan 16  2015 nfcapd.200702240830
-rwxr-x--- 1 www-data www-data  12M Jan 16  2015 nfcapd.200702240835
-rwxr-x--- 1 www-data www-data  13M Jan 16  2015 nfcapd.200702240840
-rwxr-x--- 1 www-data www-data  15M Jan 16  2015 nfcapd.200702240845
-rwxr-x--- 1 www-data www-data  15M Jan 16  2015 nfcapd.200702240850
-rwxr-x--- 1 www-data www-data  22M Jan 16  2015 nfcapd.200702240855
-rwxr-x--- 1 www-data www-data  11M Jan 16  2015 nfcapd.200702240900
-rwxr-x--- 1 www-data www-data 7.5M Jan 16  2015 nfcapd.200702240905
-rwxr-x--- 1 www-data www-data 8.9M Jan 16  2015 nfcapd.200702240910
-rwxr-x--- 1 www-data www-data 8.1M Jan 16  2015 nfcapd.200702240915
-rwxr-x--- 1 www-data www-data 9.1M Jan 16  2015 nfcapd.200702240920
-rwxr-x--- 1 www-data www-data  12M Jan 16  2015 nfcapd.200702240925
-rwxr-x--- 1 www-data www-data  12M Jan 16  2015 nfcapd.200702240930
-rwxr-x--- 1 www-data www-data  12M Jan 16  2015 nfcapd.200702240935
-rwxr-x--- 1 www-data www-data  13M Jan 16  2015 nfcapd.200702240940
-rwxr-x--- 1 www-data www-data  12M Jan 16  2015 nfcapd.200702240945
-rwxr-x--- 1 www-data www-data  12M Jan 16  2015 nfcapd.200702240950
-rwxr-x--- 1 www-data www-data  13M Jan 16  2015 nfcapd.200702240955
-rwxr-x--- 1 www-data www-data  13M Jan 16  2015 nfcapd.200702241000
-rwxr-x--- 1 www-data www-data  13M Jan 16  2015 nfcapd.200702241005
-rwxr-x--- 1 www-data www-data  12M Jan 16  2015 nfcapd.200702241010
-rwxr-x--- 1 www-data www-data  13M Jan 16  2015 nfcapd.200702241015
-rwxr-x--- 1 www-data www-data  13M Jan 16  2015 nfcapd.200702241020
-rwxr-x--- 1 www-data www-data  14M Jan 16  2015 nfcapd.200702241025
-rwxr-x--- 1 www-data www-data  13M Jan 16  2015 nfcapd.200702241030
-rwxr-x--- 1 www-data www-data  14M Jan 16  2015 nfcapd.200702241035
-rwxr-x--- 1 www-data www-data  19M Jan 16  2015 nfcapd.200702241040
-rwxr-x--- 1 www-data www-data  21M Jan 16  2015 nfcapd.200702241045
-rwxr-x--- 1 www-data www-data 5.5M Jan 16  2015 nfcapd.200702241050
-rwxr-x--- 1 www-data www-data 6.0M Jan 16  2015 nfcapd.200702241055
-rwxr-x--- 1 www-data www-data 6.2M Jan 16  2015 nfcapd.200702241100
-rwxr-x--- 1 www-data www-data 5.9M Jan 16  2015 nfcapd.200702241105
-rwxr-x--- 1 www-data www-data 5.8M Jan 16  2015 nfcapd.200702241110
-rwxr-x--- 1 www-data www-data 5.8M Jan 16  2015 nfcapd.200702241115
-rwxr-x--- 1 www-data www-data 5.8M Jan 16  2015 nfcapd.200702241120
-rwxr-x--- 1 www-data www-data 5.9M Jan 16  2015 nfcapd.200702241125
-rwxr-x--- 1 www-data www-data 5.5M Jan 16  2015 nfcapd.200702241130
-rwxr-x--- 1 www-data www-data 5.9M Jan 16  2015 nfcapd.200702241135
-rwxr-x--- 1 www-data www-data 5.8M Jan 16  2015 nfcapd.200702241140
-rwxr-x--- 1 www-data www-data 6.0M Jan 16  2015 nfcapd.200702241145
-rwxr-x--- 1 www-data www-data 6.2M Jan 16  2015 nfcapd.200702241150
-rwxr-x--- 1 www-data www-data 6.3M Jan 16  2015 nfcapd.200702241155
-rwxr-x--- 1 www-data www-data 6.4M Jan 16  2015 nfcapd.200702241200
-rwxr-x--- 1 www-data www-data 6.2M Jan 16  2015 nfcapd.200702241205
-rwxr-x--- 1 www-data www-data 6.3M Jan 16  2015 nfcapd.200702241210
-rwxr-x--- 1 www-data www-data 6.5M Jan 16  2015 nfcapd.200702241215
-rwxr-x--- 1 www-data www-data 6.1M Jan 16  2015 nfcapd.200702241220
-rwxr-x--- 1 www-data www-data 6.2M Jan 16  2015 nfcapd.200702241225
-rwxr-x--- 1 www-data www-data 6.1M Jan 16  2015 nfcapd.200702241230
-rwxr-x--- 1 www-data www-data 6.5M Jan 16  2015 nfcapd.200702241235
-rwxr-x--- 1 www-data www-data 6.2M Jan 16  2015 nfcapd.200702241240
-rwxr-x--- 1 www-data www-data 5.9M Jan 16  2015 nfcapd.200702241245
-rwxr-x--- 1 www-data www-data 5.8M Jan 16  2015 nfcapd.200702241250
-rwxr-x--- 1 www-data www-data 5.9M Jan 16  2015 nfcapd.200702241255
-rwxr-x--- 1 www-data www-data 6.1M Jan 16  2015 nfcapd.200702241300
-rwxr-x--- 1 www-data www-data 6.1M Jan 16  2015 nfcapd.200702241305
-rwxr-x--- 1 www-data www-data 5.9M Jan 16  2015 nfcapd.200702241310
-rwxr-x--- 1 www-data www-data 6.1M Jan 16  2015 nfcapd.200702241315
-rwxr-x--- 1 www-data www-data 6.3M Jan 16  2015 nfcapd.200702241320
-rwxr-x--- 1 www-data www-data 6.1M Jan 16  2015 nfcapd.200702241325
-rwxr-x--- 1 www-data www-data 6.2M Jan 16  2015 nfcapd.200702241330
-rwxr-x--- 1 www-data www-data 6.0M Jan 16  2015 nfcapd.200702241335
-rwxr-x--- 1 www-data www-data 5.8M Jan 16  2015 nfcapd.200702241340
-rwxr-x--- 1 www-data www-data 5.6M Jan 16  2015 nfcapd.200702241345
-rwxr-x--- 1 www-data www-data 5.8M Jan 16  2015 nfcapd.200702241350
-rwxr-x--- 1 www-data www-data 5.5M Jan 16  2015 nfcapd.200702241355
-rwxr-x--- 1 www-data www-data 5.5M Jan 16  2015 nfcapd.200702241400
-rwxr-x--- 1 www-data www-data 5.5M Jan 16  2015 nfcapd.200702241405
-rwxr-x--- 1 www-data www-data 5.6M Jan 16  2015 nfcapd.200702241410
-rwxr-x--- 1 www-data www-data 5.7M Jan 16  2015 nfcapd.200702241415
-rwxr-x--- 1 www-data www-data 5.9M Jan 16  2015 nfcapd.200702241420
-rwxr-x--- 1 www-data www-data 6.1M Jan 16  2015 nfcapd.200702241425
-rwxr-x--- 1 www-data www-data 5.8M Jan 16  2015 nfcapd.200702241430
-rwxr-x--- 1 www-data www-data 2.5M Jan 16  2015 nfcapd.200702241435
-rwxr-x--- 1 www-data www-data 2.4M Jan 16  2015 nfcapd.200702241440
-rwxr-x--- 1 www-data www-data 2.4M Jan 16  2015 nfcapd.200702241445
-rwxr-x--- 1 www-data www-data 2.4M Jan 16  2015 nfcapd.200702241450
-rwxr-x--- 1 www-data www-data 2.3M Jan 16  2015 nfcapd.200702241455
-rwxr-x--- 1 www-data www-data 2.4M Jan 16  2015 nfcapd.200702241500
-rwxr-x--- 1 www-data www-data 2.4M Jan 16  2015 nfcapd.200702241505
-rwxr-x--- 1 www-data www-data 2.3M Jan 16  2015 nfcapd.200702241510
-rwxr-x--- 1 www-data www-data 2.3M Jan 16  2015 nfcapd.200702241515
-rwxr-x--- 1 www-data www-data 2.2M Jan 16  2015 nfcapd.200702241520
-rwxr-x--- 1 www-data www-data 2.3M Jan 16  2015 nfcapd.200702241525
-rwxr-x--- 1 www-data www-data 2.3M Jan 16  2015 nfcapd.200702241530
-rwxr-x--- 1 www-data www-data 2.4M Jan 16  2015 nfcapd.200702241535
-rwxr-x--- 1 www-data www-data 2.4M Jan 16  2015 nfcapd.200702241540
-rwxr-x--- 1 www-data www-data 2.4M Jan 16  2015 nfcapd.200702241545
-rwxr-x--- 1 www-data www-data 1.1M Jan 16  2015 nfcapd.200702241550
-rwxr-x--- 1 www-data www-data 283K Jan 16  2015 nfcapd.200702241555
-rwxr-x--- 1 www-data www-data 1.2M Jan 16  2015 nfcapd.200702241600
-rwxr-x--- 1 www-data www-data 2.8M Jan 16  2015 nfcapd.200702241605
-rwxr-x--- 1 www-data www-data 250K Jan 16  2015 nfcapd.200702241610
-rwxr-x--- 1 www-data www-data 245K Jan 16  2015 nfcapd.200702241615
-rwxr-x--- 1 www-data www-data 222K Jan 16  2015 nfcapd.200702241620
-rwxr-x--- 1 www-data www-data 188K Jan 16  2015 nfcapd.200702241625
-rwxr-x--- 1 www-data www-data 268K Jan 16  2015 nfcapd.200702241630
-rwxr-x--- 1 www-data www-data 205K Jan 16  2015 nfcapd.200702241635
-rwxr-x--- 1 www-data www-data 240K Jan 16  2015 nfcapd.200702241640
-rwxr-x--- 1 www-data www-data 244K Jan 16  2015 nfcapd.200702241645
-rwxr-x--- 1 www-data www-data 216K Jan 16  2015 nfcapd.200702241650
-rwxr-x--- 1 www-data www-data 259K Jan 16  2015 nfcapd.200702241655
-rwxr-x--- 1 www-data www-data 221K Jan 16  2015 nfcapd.200702241700
-rwxr-x--- 1 www-data www-data 208K Jan 16  2015 nfcapd.200702241705
-rwxr-x--- 1 www-data www-data 226K Jan 16  2015 nfcapd.200702241710
-rwxr-x--- 1 www-data www-data 203K Jan 16  2015 nfcapd.200702241715
-rwxr-x--- 1 www-data www-data 237K Jan 16  2015 nfcapd.200702241720
-rwxr-x--- 1 www-data www-data 276K Jan 16  2015 nfcapd.200702241725
-rwxr-x--- 1 www-data www-data 235K Jan 16  2015 nfcapd.200702241730
-rwxr-x--- 1 www-data www-data 276K Jan 16  2015 nfcapd.200702241735
-rwxr-x--- 1 www-data www-data 261K Jan 16  2015 nfcapd.200702241740
-rwxr-x--- 1 www-data www-data 261K Jan 16  2015 nfcapd.200702241745
-rwxr-x--- 1 www-data www-data 268K Jan 16  2015 nfcapd.200702241750
-rwxr-x--- 1 www-data www-data 269K Jan 16  2015 nfcapd.200702241755
-rwxr-x--- 1 www-data www-data 295K Jan 16  2015 nfcapd.200702241800
-rwxr-x--- 1 www-data www-data 278K Jan 16  2015 nfcapd.200702241805
-rwxr-x--- 1 www-data www-data 285K Jan 16  2015 nfcapd.200702241810
-rwxr-x--- 1 www-data www-data 262K Jan 16  2015 nfcapd.200702241815
-rwxr-x--- 1 www-data www-data 295K Jan 16  2015 nfcapd.200702241820
-rwxr-x--- 1 www-data www-data 265K Jan 16  2015 nfcapd.200702241825
-rwxr-x--- 1 www-data www-data 282K Jan 16  2015 nfcapd.200702241830
-rwxr-x--- 1 www-data www-data 243K Jan 16  2015 nfcapd.200702241835
-rwxr-x--- 1 www-data www-data 254K Jan 16  2015 nfcapd.200702241840
-rwxr-x--- 1 www-data www-data 286K Jan 16  2015 nfcapd.200702241845
-rwxr-x--- 1 www-data www-data 319K Jan 16  2015 nfcapd.200702241850
-rwxr-x--- 1 www-data www-data 307K Jan 16  2015 nfcapd.200702241855
-rwxr-x--- 1 www-data www-data 319K Jan 16  2015 nfcapd.200702241900
-rwxr-x--- 1 www-data www-data 329K Jan 16  2015 nfcapd.200702241905
-rwxr-x--- 1 www-data www-data 264K Jan 16  2015 nfcapd.200702241910
-rwxr-x--- 1 www-data www-data 302K Jan 16  2015 nfcapd.200702241915
-rwxr-x--- 1 www-data www-data 299K Jan 16  2015 nfcapd.200702241920
-rwxr-x--- 1 www-data www-data 256K Jan 16  2015 nfcapd.200702241925
-rwxr-x--- 1 www-data www-data 251K Jan 16  2015 nfcapd.200702241930
-rwxr-x--- 1 www-data www-data 286K Jan 16  2015 nfcapd.200702241935
-rwxr-x--- 1 www-data www-data 279K Jan 16  2015 nfcapd.200702241940
-rwxr-x--- 1 www-data www-data 291K Jan 16  2015 nfcapd.200702241945
-rwxr-x--- 1 www-data www-data 277K Jan 16  2015 nfcapd.200702241950
-rwxr-x--- 1 www-data www-data 264K Jan 16  2015 nfcapd.200702241955
-rwxr-x--- 1 www-data www-data 241K Jan 16  2015 nfcapd.200702242000
-rwxr-x--- 1 www-data www-data 283K Jan 16  2015 nfcapd.200702242005
-rwxr-x--- 1 www-data www-data 292K Jan 16  2015 nfcapd.200702242010
-rwxr-x--- 1 www-data www-data 271K Jan 16  2015 nfcapd.200702242015
-rwxr-x--- 1 www-data www-data 252K Jan 16  2015 nfcapd.200702242020
-rwxr-x--- 1 www-data www-data 221K Jan 16  2015 nfcapd.200702242025
-rwxr-x--- 1 www-data www-data 254K Jan 16  2015 nfcapd.200702242030
-rwxr-x--- 1 www-data www-data 292K Jan 16  2015 nfcapd.200702242035
-rwxr-x--- 1 www-data www-data 254K Jan 16  2015 nfcapd.200702242040
-rwxr-x--- 1 www-data www-data 254K Jan 16  2015 nfcapd.200702242045
-rwxr-x--- 1 www-data www-data 258K Jan 16  2015 nfcapd.200702242050
-rwxr-x--- 1 www-data www-data 285K Jan 16  2015 nfcapd.200702242055
-rwxr-x--- 1 www-data www-data 333K Jan 16  2015 nfcapd.200702242100
-rwxr-x--- 1 www-data www-data 327K Jan 16  2015 nfcapd.200702242105
-rwxr-x--- 1 www-data www-data 288K Jan 16  2015 nfcapd.200702242110
-rwxr-x--- 1 www-data www-data 236K Jan 16  2015 nfcapd.200702242115
-rwxr-x--- 1 www-data www-data 232K Jan 16  2015 nfcapd.200702242120
-rwxr-x--- 1 www-data www-data 190K Jan 16  2015 nfcapd.200702242125
-rwxr-x--- 1 www-data www-data 221K Jan 16  2015 nfcapd.200702242130
-rwxr-x--- 1 www-data www-data 195K Jan 16  2015 nfcapd.200702242135
-rwxr-x--- 1 www-data www-data 187K Jan 16  2015 nfcapd.200702242140
-rwxr-x--- 1 www-data www-data 205K Jan 16  2015 nfcapd.200702242145
-rwxr-x--- 1 www-data www-data 302K Jan 16  2015 nfcapd.200702242150
-rwxr-x--- 1 www-data www-data 251K Jan 16  2015 nfcapd.200702242155
-rwxr-x--- 1 www-data www-data 253K Jan 16  2015 nfcapd.200702242200
-rwxr-x--- 1 www-data www-data 221K Jan 16  2015 nfcapd.200702242205
-rwxr-x--- 1 www-data www-data 229K Jan 16  2015 nfcapd.200702242210
-rwxr-x--- 1 www-data www-data 206K Jan 16  2015 nfcapd.200702242215
-rwxr-x--- 1 www-data www-data 208K Jan 16  2015 nfcapd.200702242220
-rwxr-x--- 1 www-data www-data 167K Jan 16  2015 nfcapd.200702242225
-rwxr-x--- 1 www-data www-data 185K Jan 16  2015 nfcapd.200702242230
-rwxr-x--- 1 www-data www-data 219K Jan 16  2015 nfcapd.200702242235
-rwxr-x--- 1 www-data www-data 229K Jan 16  2015 nfcapd.200702242240
-rwxr-x--- 1 www-data www-data 192K Jan 16  2015 nfcapd.200702242245
-rwxr-x--- 1 www-data www-data 206K Jan 16  2015 nfcapd.200702242250
-rwxr-x--- 1 www-data www-data 186K Jan 16  2015 nfcapd.200702242255
-rwxr-x--- 1 www-data www-data 215K Jan 16  2015 nfcapd.200702242300
-rwxr-x--- 1 www-data www-data 189K Jan 16  2015 nfcapd.200702242305
-rwxr-x--- 1 www-data www-data 197K Jan 16  2015 nfcapd.200702242310
-rwxr-x--- 1 www-data www-data 155K Jan 16  2015 nfcapd.200702242315
-rwxr-x--- 1 www-data www-data 141K Jan 16  2015 nfcapd.200702242320
-rwxr-x--- 1 www-data www-data 153K Jan 16  2015 nfcapd.200702242325
-rwxr-x--- 1 www-data www-data 181K Jan 16  2015 nfcapd.200702242330
-rwxr-x--- 1 www-data www-data 139K Jan 16  2015 nfcapd.200702242335
-rwxr-x--- 1 www-data www-data 171K Jan 16  2015 nfcapd.200702242340
-rwxr-x--- 1 www-data www-data 149K Jan 16  2015 nfcapd.200702242345
-rwxr-x--- 1 www-data www-data 180K Jan 16  2015 nfcapd.200702242350
-rwxr-x--- 1 www-data www-data 140K Jan 16  2015 nfcapd.200702242355
-rwxr-x--- 1 www-data www-data 192K Jan 16  2015 nfcapd.200702250000
-rwxr-x--- 1 www-data www-data 124K Jan 16  2015 nfcapd.200702250005
-rwxr-x--- 1 www-data www-data 107K Jan 16  2015 nfcapd.200702250010
-rwxr-x--- 1 www-data www-data 140K Jan 16  2015 nfcapd.200702250015
-rwxr-x--- 1 www-data www-data 133K Jan 16  2015 nfcapd.200702250020
-rwxr-x--- 1 www-data www-data  98K Jan 16  2015 nfcapd.200702250025
-rwxr-x--- 1 www-data www-data 119K Jan 16  2015 nfcapd.200702250030
-rwxr-x--- 1 www-data www-data 124K Jan 16  2015 nfcapd.200702250035
-rwxr-x--- 1 www-data www-data 130K Jan 16  2015 nfcapd.200702250040
-rwxr-x--- 1 www-data www-data 117K Jan 16  2015 nfcapd.200702250045
-rwxr-x--- 1 www-data www-data 104K Jan 16  2015 nfcapd.200702250050
-rwxr-x--- 1 www-data www-data 142K Jan 16  2015 nfcapd.200702250055
-rwxr-x--- 1 www-data www-data  95K Jan 16  2015 nfcapd.200702250100
-rwxr-x--- 1 www-data www-data  92K Jan 16  2015 nfcapd.200702250105
-rwxr-x--- 1 www-data www-data  98K Jan 16  2015 nfcapd.200702250110
-rwxr-x--- 1 www-data www-data 153K Jan 16  2015 nfcapd.200702250115
-rwxr-x--- 1 www-data www-data 105K Jan 16  2015 nfcapd.200702250120
-rwxr-x--- 1 www-data www-data 155K Jan 16  2015 nfcapd.200702250125
-rwxr-x--- 1 www-data www-data 110K Jan 16  2015 nfcapd.200702250130
-rwxr-x--- 1 www-data www-data  96K Jan 16  2015 nfcapd.200702250135
-rwxr-x--- 1 www-data www-data  96K Jan 16  2015 nfcapd.200702250140
-rwxr-x--- 1 www-data www-data 107K Jan 16  2015 nfcapd.200702250145
-rwxr-x--- 1 www-data www-data  81K Jan 16  2015 nfcapd.200702250150
-rwxr-x--- 1 www-data www-data 103K Jan 16  2015 nfcapd.200702250155
-rwxr-x--- 1 www-data www-data  86K Jan 16  2015 nfcapd.200702250200
-rwxr-x--- 1 www-data www-data 100K Jan 16  2015 nfcapd.200702250205
-rwxr-x--- 1 www-data www-data  96K Jan 16  2015 nfcapd.200702250210
-rwxr-x--- 1 www-data www-data  82K Jan 16  2015 nfcapd.200702250215
-rwxr-x--- 1 www-data www-data  72K Jan 16  2015 nfcapd.200702250220
-rwxr-x--- 1 www-data www-data 108K Jan 16  2015 nfcapd.200702250225
-rwxr-x--- 1 www-data www-data  79K Jan 16  2015 nfcapd.200702250230
-rwxr-x--- 1 www-data www-data  60K Jan 16  2015 nfcapd.200702250235
-rwxr-x--- 1 www-data www-data 112K Jan 16  2015 nfcapd.200702250240
-rwxr-x--- 1 www-data www-data 111K Jan 16  2015 nfcapd.200702250245
-rwxr-x--- 1 www-data www-data  95K Jan 16  2015 nfcapd.200702250250
-rwxr-x--- 1 www-data www-data  77K Jan 16  2015 nfcapd.200702250255
-rwxr-x--- 1 www-data www-data  47K Jan 16  2015 nfcapd.200702250300
-rwxr-x--- 1 www-data www-data  92K Jan 16  2015 nfcapd.200702250305
-rwxr-x--- 1 www-data www-data  97K Jan 16  2015 nfcapd.200702250310
-rwxr-x--- 1 www-data www-data  99K Jan 16  2015 nfcapd.200702250315
-rwxr-x--- 1 www-data www-data 100K Jan 16  2015 nfcapd.200702250320
-rwxr-x--- 1 www-data www-data  84K Jan 16  2015 nfcapd.200702250325
-rwxr-x--- 1 www-data www-data  88K Jan 16  2015 nfcapd.200702250330
-rwxr-x--- 1 www-data www-data  54K Jan 16  2015 nfcapd.200702250335
-rwxr-x--- 1 www-data www-data  64K Jan 16  2015 nfcapd.200702250340
-rwxr-x--- 1 www-data www-data  91K Jan 16  2015 nfcapd.200702250345
-rwxr-x--- 1 www-data www-data  98K Jan 16  2015 nfcapd.200702250350
-rwxr-x--- 1 www-data www-data  91K Jan 16  2015 nfcapd.200702250355
-rwxr-x--- 1 www-data www-data  64K Jan 16  2015 nfcapd.200702250400
-rwxr-x--- 1 www-data www-data 128K Jan 16  2015 nfcapd.200702250405
enisa@enisa-vm:/data/nfsen/profiles-data/live/upstream1$ 
```

