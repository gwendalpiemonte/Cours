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

- HELO : utilisé pour initier une connexion avec le serveur
- EHLO : permet d'initier une connexion avec le serveur (version étendue de SALUT)
- MAIL : permet de préciser l'expéditeur du message
- RCPT : permet de préciser le destinataire du message
- DATA : utilisé pour envoyer le contenu du message
- RSET : utilisé pour réinitialiser la connexion

Chaque message a un format spécifique. Par exemple, le message MAIL a le format suivant :

```sh
MAIL FROM:<sender>
```

Le message MAIL est utilisé pour spécifier l'expéditeur du message. L'expéditeur
est spécifié après le mot-clé MAIL FROM:. Vous en apprendrez davantage sur le
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

> Le protocole DAI est destiné à transférer des fichiers sur le réseau.
> Le protocole DAI est un protocole client-serveur.
> Le client se connecte à un serveur et demande un fichier. Le serveur envoie
> le fichier ou un message d'erreur si le fichier n'existe pas.

### Section 2 - Protocole de transport

> Le protocole DAI utilise le protocole TCP. Le serveur fonctionne sur le port 55555.
> Le client doit connaître l'adresse IP du serveur auquel se connecter. Il
> établit la connexion avec le serveur.
> Le serveur ferme la connexion lorsque le transfert est effectué ou si un
> une erreur se produit (par exemple, le fichier n'a pas été trouvé).

### Section 3 - Messages

Cette section définit les messages pouvant être échangés entre les
client et le serveur.

> Le client peut envoyer les messages suivants :
> 
> - GET <file> : utilisé pour demander un fichier au serveur
>       <file> : le nom du fichier à demander - Le nom du fichier est un chemin absolu du fichier (/data/file.txt)
> - QUIT       : permet de fermer la connexion avec le serveur
> 
> Le serveur peut envoyer les messages suivants :
> 
> - OK           : utilisé pour informer le client que la connexion a réussi et le serveur est prêt à recevoir des commandes
> - FILE <file>  : permet d'envoyer le contenu du fichier demandé - la connexion est fermée après ce message
> - ERROR <code> : utilisé pour avertir le client qu'une erreur s'est produite - la connexion est fermée après ce message
>            400 : la demande a été mal formée
>            404 : le fichier n'a pas été trouvé
> 
> Tous les messages sont codés en UTF-8 et se terminent par un caractère de nouvelle ligne (\n).
> 
> Si le fichier existe, le serveur envoie le contenu du fichier sous forme de données binaires.

### Section 4 - Exemples

Cette section définit des exemples de messages pouvant être échangés entre
le client et le serveur et l'ordre d'échange. Il est important de définir
ces exemples pour illustrer le protocole et aider le lecteur à
comprendre le protocole à l'aide de diagrammes de séquences ou d'états par exemple.

