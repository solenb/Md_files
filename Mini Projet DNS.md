---
title : Mini Projet DNS
---
> #  Mini projet DNS

## DNS

> Fichier de configuration du DNS de bind9 :


```bash=1
;
; BIND data file
;
$TTL	604800
@	IN	SOA	SOLEN-BELLOUATI.local. root.SOLEN-BELLOUATI.local. (
			3		;Serial
			604800		; Refresh   
			86400		; Retry
			2419200		; Expire
			604800 )	; Negative Cache TTL
;
@	IN	NS	dns.SOLEN-BELLOUATI.local.
dns     IN	A	10.205.6.1
server  IN      A       10.205.6.1
smtp    IN      A       10.205.6.1
pop     IN      A       10.205.6.1
imap    IN      A       10.205.6.1

```
> Fichier de configuration du reverse DNS bind9 :
```bash=1
;
; BIND reverse data file
;
$TTL	604800
@	IN	SOA	0.17.172.in-addr.arpa 0.17.172.in-addr.arpa (
			 2       	; Serial
			 604800		; Refresh
			  86400		; Retry
			2419200		; Expire
			 604800 )	; Negative Cache TTL
;
@	IN	NS	dns.SOLEN-BELLOUATI.local.
2	IN	PTR	dns.SOLEN-BELLOUATI.local.


```
> Fichier de configuration du DNS local de bind9 :
```bash=1
// Do any local configuration here

// Consider adding the 1918 zones here, if they are not used in your
// organization
//include "/etc/bind/zones.rfc1918";
	zone "SOLEN-BELLOUATI.local" {
		type master;
		file "/etc/bind/SOLEN-BELLOUATI.local";
	};

	zone  "0.17.172.in-addr.arpa" {
		type master;
		file "/etc/bind/db.172.17.0.2";
	};

```
> Fichier de configuration des options de bind9:
```bash=1
root@758da7855583:/etc/bind# cat named.conf.options 
options {
	directory "/var/cache/bind";

	// If there is a firewall between you and nameservers you want
	// to talk to, you may need to fix the firewall to allow multiple
	// ports to talk.  See http://www.kb.cert.org/vuls/id/800113

	// If your ISP provided one or more IP addresses for stable 
	// nameservers, you probably want to use them as forwarders.  
	// Uncomment the following block, and insert the addresses replacing 
	// the all-0's placeholder.

	forwarders {
	 	10.205.2.64;
	};

	//========================================================================
	// If BIND logs error messages about the root key being expired,
	// you will need to update your keys.  See https://www.isc.org/bind-keys
	//========================================================================
	dnssec-validation auto;

	listen-on-v6 { any; };
	allow-query-cache { any; };
	allow-query { any; };
};

```
> On test que la configuration de bind fonctionne en rentrant la commande *dig* :
```html=1
solen.bellouati@205-6:~$ dig +tcp @10.205.6.1 -p 1053 dns.SOLEN-BELLOUATI.local

; <<>> DiG 9.11.3-1ubuntu1.9-Ubuntu <<>> +tcp @10.205.6.1 -p 1053 dns.SOLEN-BELLOUATI.local
; (1 server found)
;; global options: +cmd
;; Got answer:
;; WARNING: .local is reserved for Multicast DNS
;; You are currently testing what happens when an mDNS query is leaked to DNS
;; ->>HEADER<<- opcode: QUERY, status: NOERROR, id: 14912
;; flags: qr aa rd ra; QUERY: 1, ANSWER: 1, AUTHORITY: 1, ADDITIONAL: 1

;; OPT PSEUDOSECTION:
; EDNS: version: 0, flags:; udp: 4096
;; QUESTION SECTION:
;dns.SOLEN-BELLOUATI.local.	IN	A

;; ANSWER SECTION:
dns.SOLEN-BELLOUATI.local. 604800 IN	A	10.205.6.1

;; AUTHORITY SECTION:
SOLEN-BELLOUATI.local.	604800	IN	NS	dns.SOLEN-BELLOUATI.local.

;; Query time: 0 msec
;; SERVER: 10.205.6.1#1053(10.205.6.1)
;; WHEN: Tue Oct 15 17:13:21 CEST 2019
;; MSG SIZE  rcvd: 84

```

## Mail










git : https://github.com/solenb/MiniProjetDNS

