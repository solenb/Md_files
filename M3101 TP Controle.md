---
title: M3101 TP Controle
---
# M3101 TP Controle
Solen BELLOUATI
Le compte rendu est disponible en Markdown à l'adresse suivante : 
https://md.iutbeziers.org/s/SyO2-8AnH#

## I. Wifi
1. La configuration IP et Wifi du Rpi 205-10 est la suivante : 
```bash=1
pi@pi205-10:~ $ ip a
1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN group default qlen 1000
    link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
    inet 127.0.0.1/8 scope host lo
       valid_lft forever preferred_lft forever
    inet6 ::1/128 scope host 
       valid_lft forever preferred_lft forever
2: eth0: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc pfifo_fast state UP group default qlen 1000
    link/ether b8:27:eb:64:16:15 brd ff:ff:ff:ff:ff:ff
    inet 10.205.5.2/16 brd 10.205.255.255 scope global eth0
       valid_lft forever preferred_lft forever
    inet6 fe80::ba27:ebff:fe64:1615/64 scope link 
       valid_lft forever preferred_lft forever
3: wlan0: <NO-CARRIER,BROADCAST,MULTICAST,UP> mtu 1500 qdisc pfifo_fast state DOWN group default qlen 1000
    link/ether b8:27:eb:31:43:40 brd ff:ff:ff:ff:ff:ff
```
```bash=1
pi@pi205-10:~ $ iw wlan0 info
Interface wlan0
	ifindex 3
	wdev 0x1
	addr b8:27:eb:31:43:40
	type managed
	wiphy 0
	channel 1 (2412 MHz), width: 20 MHz, center1: 2412 MHz
	txpower 31.00 dBm
```
2. Le fichier scan contient bien toute la liste des réseaux wifi disponibles : 
```bash=1
root@pi205-10:/home/pi/Desktop# cat scan-BELLOUATI.txt 
SSID: IUTBZ SSID: eduroam SSID: PSK-CTRL205-1 SSID: IUTBEZIERS SSID: PSK-CTRL205-2 * SSID List SSID: eduroam SSID: PSK-CTRL205-3 * SSID List SSID:  SSID: IUTBEZIERS SSID: IUTBEZIERS SSID: IUTBEZIERS SSID:  SSID: eduroam SSID: eduroam SSID: FreeWifi_secure SSID: FreeWifi SSID: freebox_DXZNQP
```
3. On peut se connecter au réseau PSK-CTRL205-3 grâce à la commande wpa_supplicant et au fichier de configuration suivant : 
```bash=1
root@pi205-10:/home/pi/Desktop# cat connect-PSK-CTRL205-3.txt 
network={
	ssid="PSK-CTRL205-3"
	auth_alg=OPEN
	proto=RSN
	key_mgmt=WPA-PSK
	pairwise=CCMP
	group=CCMP
	psk="taratata"
}
```
Il faut ensuite lancer la commande suivante : 
```bash=1
wpa_supplicant -i wlan0 -c connect-PSK-CTRL205-3.txt 
```
On fait enfin une requête DHCP au serveur : 
```bash=1
pi@pi205-10:~ $ sudo dhclient -v wlan0
Internet Systems Consortium DHCP Client 4.4.1
Copyright 2004-2018 Internet Systems Consortium.
All rights reserved.
For info, please visit https://www.isc.org/software/dhcp/

Listening on LPF/wlan0/b8:27:eb:31:43:40
Sending on   LPF/wlan0/b8:27:eb:31:43:40
Sending on   Socket/fallback
DHCPDISCOVER on wlan0 to 255.255.255.255 port 67 interval 6
DHCPOFFER of 192.168.53.158 from 192.168.53.65
DHCPREQUEST for 192.168.53.158 on wlan0 to 255.255.255.255 port 67
DHCPACK of 192.168.53.158 from 192.168.53.65
bound to 192.168.53.158 -- renewal in 41981 seconds.
```
On remarque que l'on reçoit un DHCP offer de l'adresse 192.168.53.65, et on teste sa réponse au ping : 
```bash=1
pi@pi205-10:~ $ ping 192.168.53.65
PING 192.168.53.65 (192.168.53.65) 56(84) bytes of data.
64 bytes from 192.168.53.65: icmp_seq=1 ttl=64 time=6.45 ms
64 bytes from 192.168.53.65: icmp_seq=2 ttl=64 time=4.55 ms
64 bytes from 192.168.53.65: icmp_seq=3 ttl=64 time=4.59 ms
^C
--- 192.168.53.65 ping statistics ---
3 packets transmitted, 3 received, 0% packet loss, time 5ms
rtt min/avg/max/mdev = 4.554/5.196/6.451/0.890 ms
```
## II. Capture de trames 802.11
Afin de capturer des trames 802.11, j'ai équipé mon raspberry pi d'une clé wifi prenant en charge le mode monitor : 
```bash=1
root@pi205-10:/home/pi/Desktop# iw wlan1 info
Interface wlan1
	ifindex 5
	wdev 0x200000001
	addr 30:b5:c2:18:e3:ab
	type monitor
	wiphy 2
	channel 1 (2412 MHz), width: 20 MHz (no HT), center1: 2412 MHz
	txpower 20.00 dBm
```
Grâce à l'utilitaire Tshark j'ai capturé tout le traffic et écrit dans le fichier capture.pcap : 
```bash=1
root@pi205-10:/home/pi/Desktop# tshark -ni wlan1 -w capture.pcap
```
J'ai ensuite envoyé tous mes fichier (dont la capture) sur mon ordinateur afin de l'ouvrir sur wireshark : 
```bash=1
root@pi205-10:/home/pi/Desktop# scp /home/pi/Desktop/* solen.bellouati@10.205.10.1:/home/s/solen.bellouati/Bureau
The authenticity of host '10.205.10.1 (10.205.10.1)' can't be established.
ECDSA key fingerprint is SHA256:Lg8POVHPC2/jAZRgNwOS/rgMs1bXUw5V5ZJxN8/Cv+Q.
Are you sure you want to continue connecting (yes/no)? yes
Warning: Permanently added '10.205.10.1' (ECDSA) to the list of known hosts.
solen.bellouati@10.205.10.1's password: 
capture.pcap                                                                                                                                                                                                100%   67KB   4.8MB/s   00:00    
connect-PSK-CTRL205-3.txt                                                                                                                                                                                   100%  121    92.4KB/s   00:00    
scan-BELLOUATI.txt                                                                                                                                                                                          100%  294   219.3KB/s   00:00    
```
Il faut ensuite ouvrir le fichier dans wireshark et rentrer le filtre ***wlan.addr==5e:cf:7f:1b:74:2f*** : 

![](https://i.imgur.com/YUal1iY.png)

## IV. Bluetooth
1. La configuration de mon équipement bluetooth est la suivante : 
```bash=1
root@pi205-10:/home/pi# hciconfig -a
hci0:	Type: Primary  Bus: UART
	BD Address: B8:27:EB:52:EB:C5  ACL MTU: 1021:8  SCO MTU: 64:1
	UP RUNNING PSCAN 
	RX bytes:794 acl:0 sco:0 events:53 errors:0
	TX bytes:3006 acl:0 sco:0 commands:53 errors:0
	Features: 0xbf 0xfe 0xcf 0xfe 0xdb 0xff 0x7b 0x87
	Packet type: DM1 DM3 DM5 DH1 DH3 DH5 HV1 HV2 HV3 
	Link policy: RSWITCH SNIFF 
	Link mode: SLAVE ACCEPT 
	Name: 'pi205-10'
	Class: 0x4c0000
	Service Classes: Rendering, Capturing, Telephony
	Device Class: Miscellaneous, 
	HCI Version: 4.1 (0x7)  Revision: 0x168
	LMP Version: 4.1 (0x7)  Subversion: 0x2209
	Manufacturer: Broadcom Corporation (15)
```
2. La liste des réseaux disponibles est la suivante : 
```bash=1
root@pi205-10:/home/pi# hcitool scan
Scanning ...
	B8:27:EB:73:45:D8	pi205-profs
	B8:27:EB:FB:FB:62	pi205-2
```

3. La marche à suivre est al suivante : 
```bash=1
root@pi205-10:/home/pi# bluetoothctl

[bluetooth]# scan on
[bluetooth]# agent on
Agent is already registered
[bluetooth]# default-agent
Default agent request successful
[bluetooth]# devices 
Device DC:F7:56:CD:18:BB JB TE YEET
Device B8:27:EB:73:45:D8 pi205-profs
[bluetooth]# pair B8:27:EB:73:45:D8
[bluetooth]# paired-devices 
Device DC:F7:56:CD:18:BB JB TE YEET
Device B8:27:EB:73:45:D8 pi205-profs
Device B8:27:EB:32:77:F6 pi205-11
```
On va vérifier le canal de l'agent OBEX
```bash=1
root@pi205-10:/home/pi# sdptool browse B8:27:EB:73:45:D8
Service Name: File Transfer
Service RecHandle: 0x1000c
Service Class ID List:
  "OBEX File Transfer" (0x1106)
Protocol Descriptor List:
  "L2CAP" (0x0100)
  "RFCOMM" (0x0003)
    Channel: 10
  "OBEX" (0x0008)
Profile Descriptor List:
  "OBEX File Transfer" (0x1106)
    Version: 0x0102
```
Et enfin on récupère le fichier grâce à la commande ***obexget*** : 
```bash=1
root@pi205-10:/home/pi# obexget --channel 10 -b B8:27:EB:73:45:D8 /tp6c3
Connecting..\done
Receiving "/tp6c3"... Sending ""...|done
-done
Disconnecting..\done
root@pi205-10:/home/pi# ls
Desktop  Documents  Downloads  M3101.conf  MagPi  Music  OBEX  Pictures  Public  Templates  Videos  hostapd.conf  tmp  tp6c3
root@pi205-10:/home/pi# cd tp6c3 
bash: cd: tp6c3: Not a directory
root@pi205-10:/home/pi# cat tp6c3 
Msg secret: CTRL TP 205-3
```
