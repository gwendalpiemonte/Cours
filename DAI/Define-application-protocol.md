# Définir une application protocole

## Objectifs

Ce chapitre vous aidera à comprendre comment comprendre et définir un
protocole d'application.

Un protocole d'application est un document utilisé pour définir comment
les applications échangent des informations entre elles (généralement entre un
client et un serveur). Elle est définie par un ensemble de règles que chaque partie doit
suivre pour communiquer.  

Dans ce chapitre, vous apprendrez où trouver des informations sur l'application
protocoles, comment est défini un protocole d'application et comment définir votre
propre protocole d'application. Dans les prochains chapitres, vous apprendrez à interagir
avec un protocole d'application bien connu.

## Qu'est-ce qu'une application protocole?

Un protocole d'application est un document qui définit comment deux applications
peut communiquer.

Ces documents sont généralement appelés RFC (Request For Comments) et sont
disponible sur le site de l'IETF, une organisation qui définit des normes pour le
Internet (entre autres).

Le nom RFC vient du fait que ces documents sont généralement les
résultat d’une discussion entre plusieurs personnes. La RFC est un document qui
est ouvert aux commentaires et suggestions. Il est généralement mis à jour plusieurs fois
avant d’être considéré comme un standard.

Un protocole d'application s'appuie sur un protocole de transport (TCP ou UDP) et un
protocole réseau (IP). Il vient s'ajouter à ces protocoles et définit comment
les applications peuvent communiquer.

Plusieurs révisions du même protocole peuvent exister. Par exemple, le HTTP
le protocole a plusieurs révisions (HTTP/1.0, HTTP/1.1, HTTP/2, HTTP/3). Chaque
la révision est définie par une RFC différente et a des fonctionnalités différentes.

## Comment est structuré un protocole d'application ?

Un protocole d'application est généralement défini par un ensemble de règles que chaque partie
doit suivre pour communiquer.

Ces règles sont généralement définies dans une RFC sous forme de messages. La RFC définit le
messages pouvant être échangés entre le client et le serveur, le
format de ces messages et l'ordre dans lequel ils peuvent être échangés.

Par exemple, le protocole SMTP définit les messages suivants (parmi autres):

- `HELO` : utilisé pour initier une connexion avec le serveur
- `EHLO` : permet d'initier une connexion avec le serveur (version étendue de SALUT)
- `MAIL` : permet de préciser l'expéditeur du message
- `RCPT` : permet de préciser le destinataire du message
- `DATA` : utilisé pour envoyer le contenu du message
- `RSET` : utilisé pour réinitialiser la connexion

Chaque message a un format spécifique. Par exemple, le message `MAIL` a le format suivant :

```sh
MAIL FROM:<sender>
```

Le message `MAIL` est utilisé pour spécifier l'expéditeur du message. L'expéditeur
est spécifié après le mot-clé `MAIL FROM:`. Vous en apprendrez davantage sur le
Protocole SMTP dans un prochain chapitre pour illustrer cet exemple.

Une RFC définit également l'ordre dans lequel les messages peuvent ou doivent être
échangé.

Cela se fait à l'aide d'un diagramme de séquence, en fonction de la nature/
complexité du protocole.

Un diagramme de séquence est un diagramme qui définit les différents messages qui
peuvent être échangés entre le client et le serveur et l'ordre dans lequel
ils peuvent être échangés.

Une RFC définit également les cas extrêmes et les cas d'erreur, en utilisant les mêmes diagrammes. Il
est important de définir ces cas pour éviter toute ambiguïté et définir comment
le protocole doit se comporter dans ces cas.

## Comment définir une application protocole?

Définir un protocole d’application n’est pas une tâche facile. Cela demande beaucoup de
réflexion et beaucoup de tests.

Il est également important de garder à l’esprit qu’un protocole n’est jamais parfait. Ça peut
toujours être amélioré. Il est important de garder l'esprit ouvert et d'être prêt
pour modifier le protocole si nécessaire.

Plus vous réfléchissez et concevez vos protocoles d'application, moins vous
je devrai les changer à l’avenir et découvrir des problèmes.

### Section 1 - Aperçu

Cette section définit l'objectif du protocole. Quel est le but du
protocole? Quel est le problème qu’il tente de résoudre ?

Le protocole DAI est destiné à transférer des fichiers sur le réseau.

Le protocole DAI est un protocole client-serveur.

Le client se connecte à un serveur et demande un fichier. Le serveur envoie

le fichier ou un message d'erreur si le fichier n'existe pas.

### Section 2 - Protocole de transport

Le protocole DAI utilise le protocole TCP. Le serveur fonctionne sur le port 55555.

Le client doit connaître l'adresse IP du serveur auquel se connecter. Il
établit la connexion avec le serveur.

Le serveur ferme la connexion lorsque le transfert est effectué ou si un
une erreur se produit (par exemple, le fichier n'a pas été trouvé).

### Section 3 - Messages

Cette section définit les messages pouvant être échangés entre les
client et le serveur.

Le client peut envoyer les messages suivants :
 
- `GET <file>` : utilisé pour demander un fichier au serveur   
      `<file>` : le nom du fichier à demander - Le nom du fichier est un chemin absolu du fichier (`/data/file.txt`)   
 - `QUIT`       : permet de fermer la connexion avec le serveur   

 Le serveur peut envoyer les messages suivants :

- `OK`           : utilisé pour informer le client que la connexion a réussi et le serveur est prêt à recevoir des commandes   
- `FILE <file>`  : permet d'envoyer le contenu du fichier demandé - la connexion est fermée après ce message   
- `ERROR <code>` : utilisé pour avertir le client qu'une erreur s'est produite - la connexion est fermée après ce message   
  `400` : la demande a été mal formée   
  `404` : le fichier n'a pas été trouvé   

Tous les messages sont codés en UTF-8 et se terminent par un caractère de nouvelle ligne (\n).

Si le fichier existe, le serveur envoie le contenu du fichier sous forme de données binaires.

### Section 4 - Exemples

Cette section définit des exemples de messages pouvant être échangés entre
le client et le serveur et l'ordre d'échange. Il est important de définir
ces exemples pour illustrer le protocole et aider le lecteur à
comprendre le protocole à l'aide de diagrammes de séquences ou d'états par exemple.

<img src="/DAI/images/protocole.PNG" width="400"/>

## Ports réservés

Dans les réseaux informatiques, un port est un point final de communication. Au
Au niveau logiciel, au sein d'un système d'exploitation, un port est une construction logique qui
identifie un processus spécifique ou un type de service réseau. Les ports sont identifiés
pour chaque combinaison de protocole et d'adresse par des nombres non signés de 16 bits,
communément appelé numéro de port.

En utilisant des nombres non signés de 16 bits, le nombre maximum de ports est de 65 536.
Cependant, tous les ports ne peuvent pas être utilisés par n’importe qui. Certains ports sont réservés à
protocoles spécifiques.

Les premiers ports compris entre 0 et 1 023 sont appelés ports connus. Ces ports sont
réservé à des protocoles spécifiques. L'utilisation de ces ports peut nécessiter des
privilèges sur les systèmes Unix.

Voici une liste d’exemples de ports courants bien connus :
- `20` et `21` : FTP
- `22` : SSH
- `23` : Telnet
- `25`, `465` et `587` : SMTP
- `53` : DNS
- `80` et `443` : HTTP/HTTPS
- `110` et `995` : POP3
- `123` : NTP
- `143` et `993` : IMAP

Les 1 024 à 49 151 ports suivants sont appelés ports enregistrés. Certains ports sont
officiellement enregistré par l'IANA (Internet Assigned Numbers Authority) et
certains ne le sont pas. Ils peuvent être utilisés par n’importe qui.

Voici une liste d'exemples de ports enregistrés courants :
- `3306` : MySQL
- `5000-5500` : League of Legends
- `5432` : PostgreSQL
- `6379` : Redis
- `8080` : port alternatif HTTP
- `25565` : Minecraft
- `27017` : MongoDB

Les derniers ports 49 152 à 65 535 sont appelés ports dynamiques. Ces ports ne peuvent pas
être enregistré et peut être utilisé par n’importe qui. Ils sont généralement utilisés à des fins privées
ou des services personnalisés ou à des fins temporaires.

Voici une liste d'exemples de ports dynamiques courants :

- `51820` : WireGuard
- `64738` : Mumble
  
Wikipédia propose une liste de numéros de ports TCP et UDP que vous pouvez utiliser pour rechercher
le numéro de port d’un protocole spécifique.

## Un petit mot sur Unix

> philosophie et POSIX
> La philosophie Unix, créée par Ken Thompson, est un ensemble de
> normes et approches philosophiques des logiciels minimalistes et modulaires
> développement. Il est basé sur l'expérience des principaux développeurs de
> le système d'exploitation Unix.

La philosophie Unix est un ensemble de règles qui définissent la manière dont les programmes Unix doivent
être conçu. Il est utilisé pour définir le système d'exploitation Unix et le
programmes utilisés sur ce système d'exploitation.

La philosophie Unix peut être définie, entre autres, par les règles suivantes :

- Écrivez des programmes qui font une chose et le font bien.
- Écrivez des programmes pour travailler ensemble.
- Écrivez des programmes pour gérer les flux de texte, car c'est une solution universelle interface.

Vous pouvez vous inspirer de la philosophie Unix pour définir la vôtre
protocole d'application et outils, tels que l'outil CLI que vous avez créé dans le
chapitre précédent.

> La norme POSIX (Portable Operating System Interface) est une famille
> des normes spécifiées par l'IEEE Computer Society pour maintenir
> compatibilité entre les systèmes d'exploitation. POSIX définit à la fois le
> interfaces de programmation d'applications (API) au niveau du système et de l'utilisateur,
> ainsi que des shells de ligne de commande et des interfaces utilitaires, pour les logiciels
> compatibilité (portabilité) avec les variantes d'Unix et d'autres systèmes d'exploitation
> systèmes.

Tous les programmes ne sont pas/peuvent être compatibles POSIX. Si vous essayez de respecter les
Standard POSIX, vous pourrez exécuter votre programme sur différents systèmes d'exploitation
systèmes sans aucun problème.
