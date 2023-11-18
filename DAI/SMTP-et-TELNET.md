# SMTP et TELNET

## Un petit rappel sur le réseau
Le protocole Internet (IP)
Chaque ordinateur connecté à Internet possède une adresse IP (Internet Protocol).
Cette adresse IP est utilisée pour identifier l'ordinateur sur Internet.
Il a une adresse unique. Les adresses IPv4 étant limitées, il existe des NAT (Network Address Translation) qui permettent de partager une seule adresse IP entre plusieurs ordinateurs.
C'est pourquoi IPv6 a été créé.

### Le système de noms de domaine (DNS)
Le Domain Name System (DNS) est un système qui permet de cartographier un nom de domaine à une adresse IP.
Par exemple, le nom de domaine heig-vd.ch est mappé à l'adresse IP 193.134.223.20.
Vous pouvez le vérifier en exécutant la commande suivante avec nslookup :
```sh
nslookup heig-vd.ch
```

Le résultat devrait être similaire à ce qui suit :
```sh
Server: 8.8.8.8
Address: 8.8.8.8#53
Non-authoritative answer:
Name: heig-vd.ch
Address: 193.134.223.20
```

Notez la ligne Adresse. Il s'agit de l'adresse IP mappant l'enregistrement DNS.
Le serveur DNS actuel utilisé pour résoudre la requête DNS est 8.8.8.8.
Lorsque vous envoyez un e-mail, votre client de messagerie utilisera le DNS pour trouver l'adresse IP du serveur de messagerie.
Ensuite, il utilisera l'adresse IP pour envoyer l’e-mail au serveur de messagerie.

### Enregistrements DNS courants
Le DNS contient ce qu'on appelle des enregistrements DNS. 
Ces enregistrements sont utilisés pour cartographier un nom de domaine à une adresse IP. 
Il existe de nombreux types d'enregistrements DNS. 

Les plus courants sont :
- NS : Cet enregistrement spécifie les serveurs de noms pour un nom de domaine donné.
- CNAME : cet enregistrement spécifie un alias pour un nom de domaine donné.
- A : Cet enregistrement spécifie l'adresse IP d'un nom de domaine donné (IPv4).
- AAAA : Cet enregistrement spécifie l'adresse IP d'un nom de domaine donné (IPv6).

## Protocoles de messagerie électronique : SMTP, POP3 et IMAP
Il existe trois protocoles principaux utilisés pour la messagerie électronique :
-	SMTP (Simple Mail Transfer Protocol)
-	POP3 (Post Office Protocol)
-	IMAP (Internet Message Access Protocol)
-	
Vous pouvez utiliser n'importe quel client de messagerie (appelé Mail User Agent (MUA) dans les RFC) comme Thunderbird, Gmail ou Outlook pour utiliser ces protocoles.
Ils vont envoyer et recevoir des e-mails depuis un serveur de messagerie (appelé Mail Transfer Agent (MTA) dans les RFC).

### SMTP
SMTP est le protocole utilisé pour envoyer des e-mails. 
Il s'agit d'un simple protocole basé sur du texte qui utilise le port TCP 25, 465 ou 587. 
Le port 25 est utilisé pour les connexions, tandis que les ports 465 et 587 sont utilisés pour les connexions cryptées. 
Le port 587 est le port le plus récent et recommandé pour les connexions cryptées.

Il est utilisé par les clients de messagerie pour envoyer des e-mails à un serveur de messagerie.

### POP3
POP3 est un protocole utilisé pour récupérer les e-mails. 
Il s'agit d'un simple protocole basé sur du texte qui utilise le port TCP 110 ou 995. 
Le port 110 est utilisé pour les communications non chiffrées, tandis que le port 995 est utilisé pour les connexions cryptées.

Il est utilisé par les clients de messagerie pour récupérer les e-mails d'un serveur de messagerie.

### IMAP
IMAP est un protocole utilisé pour récupérer et synchroniser les e-mails.
C'est un protocole plus complexe qui utilise le port TCP 143 ou 993. 
Le port 143 est utilisé pour les connexions non cryptées, tandis que le port 993 est utilisé pour les connexions cryptées.

Il est utilisé par les clients de messagerie pour synchroniser les e-mails depuis et vers un serveur mail entre le serveur de messagerie et le client de messagerie. 
Cela signifie que si vous lisez un e-mail sur votre client de messagerie, il sera marqué comme lu sur le serveur mail, alors que POP3 ne le permet pas.

## Enregistrements DNS liés au courrier électronique
Lorsque vous envoyez un e-mail, votre client de messagerie utilisera le DNS pour trouver l'adresse IP du serveur de messagerie. 
Ensuite, il utilisera l'adresse IP pour envoyer un e-mail au serveur de messagerie.

Pour trouver l'adresse IP du serveur de messagerie, votre client de messagerie recherchera l’enregistrements DNS suivants :
-	MX : Cet enregistrement précise le serveur de messagerie qui recevra les e-mails pour un nom de domaine donné.
  Par exemple, l'enregistrement MX de heig-vd.ch est heigvd-ch01b.mail.protection.outlook.com..
-	A : Cet enregistrement spécifie l'adresse IPv4 d'un nom de domaine donné.
  Par exemple, l'enregistrement A de heig-vd.ch est 193.134.223.20.
-	TXT : l'enregistrement SPF, comme indiqué par CloudFlare
  (https:// www.cloudflare.com/en-gb/learning/dns/dns-records/dns-spf-record),
 	"[] sont un type d'enregistrement DNS TXT couramment utilisé pour le courrier électronique d’authentification.
 	Les enregistrements SPF incluent une liste d'adresses IP et de domaines autorisé à envoyer des e-mails à partir de ce domaine.
 	"Par exemple, l'un des enregistrement TXT contient ces informations pour heig-vd.ch.
 	
Vous pouvez vérifier ces enregistrements en exécutant la commande suivante avec dig :
```sh
dig heig-vd.ch any
```

L'option any permet d'afficher tous les enregistrements DNS pour le nom de domaine donné.
Par défaut, dig affichera uniquement les enregistrements A.
Le résultat devrait être similaire à ce qui suit :
```sh
; <<>> DiG 9.10.6 <<>> heig-vd.ch any
;; global options: +cmd
;; Got answer:
;; ->>HEADER<<- opcode: QUERY, status: NOERROR, id: 57402
;; flags: qr rd ra; QUERY: 1, ANSWER: 17, AUTHORITY: 0, ADDITIONAL: 1
;; OPT PSEUDOSECTION:
; EDNS: version: 0, flags:; udp: 512
;; QUESTION SECTION:
;heig-vd.ch. IN ANY
;; ANSWER SECTION:
heig-vd.ch. 300 IN A 193.134.223.20
heig-vd.ch. 3600 IN NS ns-1308.awsdns-35.org.
heig-vd.ch. 3600 IN NS ns-2025.awsdns-61.co.uk.
heig-vd.ch. 3600 IN NS ns-459.awsdns-57.com.
heig-vd.ch. 3600 IN NS ns-811.awsdns-37.net.
heig-vd.ch. 3600 IN NS ns01.heig-vd.ch.
heig-vd.ch. 3600 IN NS ns02.heig-vd.ch.
heig-vd.ch. 3600 IN SOA ns01.heig-vd.ch. noc.heig-vd.ch. 2022060836 3600
600 1209600 3600
heig-vd.ch. 300 IN MX 0 heigvd-ch01b.mail.protection.outlook.com.
heig-vd.ch. 21600 IN TXT "MS=ms50694826"
heig-vd.ch. 21600 IN TXT "_globalsign-domain-
verification=KwkbjUch-6S14SrWPzp272TN8uENyWwrdQZsoQTI_J"
heig-vd.ch. 21600 IN TXT "adobe-idp-site-
verification=38a35c2c3cc6e86da35b8375404692f367713de605009c52e448eb39368c1203"
heig-vd.ch. 21600 IN TXT "atlassian-domain-verification=gKTmd7bpHiB/
2YT6iD6xu8e4EcLbgvoWjCKrKp1lGX2LeLNPWgQti2nsPGn6i3BS"
heig-vd.ch. 21600 IN TXT "atlassian-sending-domain-
verification=dad47472-4668-4676-b0ec-1421cff8a150"
heig-vd.ch. 21600 IN TXT "msyx415nv8c4jm0ytjr5kyklfcs0df21"
heig-vd.ch. 21600 IN TXT "swisssign-
check=uC1XddBVk6xjFFVza2UjLeEyODs5l1ELm8tk1KY2Nb"
heig-vd.ch. 21600 IN TXT "v=spf1 ip4:193.134.218.124 ip4:145.232.233.54
ip4:27.126.146.0/24 ip4:103.28.42.0/24 ip4:146.88.28.0/24 ip4:163.47.180.0/22 ip4:203.55.21.0/24
ip4:204.75.142.0/24 " "ip4:185.144.39.35/32 ip4:185.144.39.39/32 include:spf.hefr.ch
include:aspmx.pardot.com include:_spf.fullfabric.com include:alumnforce.org
include:spf.protection.outlook.com -all"
;; Query time: 79 msec
;; SERVER: 8.8.8.8#53(8.8.8.8)
;; WHEN: Tue Oct 17 10:50:46 CEST 2023
;; MSG SIZE rcvd: 1209
```

Le résultat est plus détaillé qu'avec nslookup, mais il contient la même information.

Notez la ligne SECTION RÉPONSE. Le A enregistre l’adresse IP mappant l’enregistrement DNS. 
L'enregistrement MX est le serveur de messagerie qui recevra les e-mails pour la période donnée. 
L'un des TXT contient les enregistrements SPF avec v=spf1 [...].

## Les problèmes de sécurité et blacklist
Les protocoles de messagerie sont assez anciens et n'ont pas été conçus pour assurer la sécurité. 
Si nous regardons le protocole SMTP, nous pouvons voir que :
-	SMTP ne nécessite pas d'authentification
-	SMTP ne nécessite pas de cryptage
-	SMTP n'exige pas que l'adresse de l'expéditeur soit valide
-	SMTP n'exige pas que l'adresse de l'expéditeur soit la même que celle de l'e-mail adresse utilisée pour s'authentifier
-	Et bien d'autres problèmes

Pour cette raison, SMTP est souvent utilisé par les spammeurs pour envoyer des courriers indésirables.
Pour éviter cela, il existe de nombreuses listes noires contenant des adresses IP des spammeurs connues. 
Si un serveur de messagerie est sur une liste noire, il ne pourra pas envoyer e-mails à certains serveurs de messagerie.
La maintenance des serveurs de messagerie est une tâche complexe qui nécessite beaucoup de connaissances sur les protocoles et les problèmes de sécurité. 
C'est pourquoi de nombreuses entreprises utilisez des services de messagerie tiers tels que Google ou Microsoft 365.
Dans ce cours, nous utiliserons ce qu'on appelle un serveur fictif. 
Un serveur fictif imite les fonctionnalités d'un serveur réel à des fins de test. 
Dans ce cas, nous utiliserons un simple serveur SMTP avec une interface Web pour envoyer et vérifier les e-mails. 
Ce serveur SMTP s'appelle MailHog et peut être exécuté avec Docker :
[](https://github.com/mailhog/MailHog)

### Avertissement
Compte tenu de ces failles de sécurité, sachez que l'usurpation d'une adresse e-mail n'est vraiment pas si difficile.
Toutefois, la HEIG-VD dispose d'une politique stricte concernant l’utilisation de ses adresses email. 
Si vous êtes surpris en train d'usurper une adresse e-mail, vous pourriez avoir des ennuis. 
Utilisez le serveur MailHog SMTP pour vos tests afin d'éviter tout problème.

## Focus sur le protocole SMTP
Le protocole SMTP est décrit dans la RFC 5321. Il utilise le protocole TCP sur le port 25, 465 ou 587. 
C'est un protocole textuel avec les commandes suivantes (entre autres) :
-	HELO ou EHLO : Utilisé pour identifier l'expéditeur
-	MAIL FROM : utilisé pour spécifier l'adresse e-mail de l'expéditeur
-	RCPT TO : Utilisé pour spécifier l'adresse e-mail du destinataire
-	DATA : Utilisé pour spécifier le contenu de l'e-mail
-	QUIT : Utilisé pour fermer la connexion

Pour envoyer un email, vous devrez utiliser les commandes suivantes :
```shout
EHLO <sender>
MAIL FROM: <sender email address>
RCPT TO: <recipient email address>
DATA
<email content>
.
QUIT
```

Le schéma suivant montre la séquence de commandes pour envoyer un e-mail :

<img src="/DAI/images/SMTP.PNG" width="600"/>


## Telnet
Telnet est un protocole client/serveur qui permet de se connecter à un serveur et lui envoyer des commandes.

Telnet peut être utilisé pour se connecter à de nombreux services tels que HTTP, SMTP, POP3, IMAP, etc. 
Dans ce cours, nous utiliserons Telnet pour nous connecter à un serveur SMTP et envoyez un e-mail.
Le protocole Telnet est décrit dans la RFC 854.

Comme SMTP, Telnet est très ancien et n'est pas considéré comme sécurisé. 
Cependant, c'est encore utilisé pour tester des services tels que SMTP et/ou pour configurer des périphériques réseau tels que des routeurs que vous devrez peut-être configurer au cours de votre carrière.
C'est pourquoi nous l'utiliserons dans ce cours pour des tests locaux.
