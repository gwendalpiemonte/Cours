# SSH et SCP

## SSH généralités
SSH est un protocole qui permet de se connecter à une machine distante. C'est un remplacement du protocole Telnet.

Le protocole SSH est décrit dans de nombreuses RFC :
- RFC4250
- RFC4251
- RFC4252
- RFC4253.
- RFC4254

Le protocole SSH utilise le protocole TCP sur le port 22. Il utilise le protocole public/privé clés pour authentifier le client. Il est également possible d'utiliser un mot de passe pour authentifier le client mais ce n'est pas recommandé car c'est moins sécurisé.

Le protocole SSH est utilisé pour se connecter à une machine distante. Il est également utilisé pour copier des fichiers depuis/vers une machine distante avec SCP.

SSH est également le nom du logiciel qui implémente le protocole SSH. C'est disponible sur la plupart des systèmes d’exploitation et est considéré comme un standard.

## Empreinte digitale de la clé SSH
L'empreinte digitale de la clé SSH est un hachage de la clé publique. 

Elle est utilisé pour identifier le Clé publique.

L'empreinte digitale s'affiche lorsque la clé est générée.

Elle est également affiché lorsque la clé est utilisée pour se connecter à une machine distante.

L'empreinte digitale permet de vérifier que la clé publique est la même que celle utilisé pour se connecter à la machine distante.

Cela peut aider à détecter les attaques Man In The Middle.

Les clés publiques sont stockées dans le fichier ~/.ssh/known_hosts. 

Le fichier contient le empreinte digitale de la clé publique et le nom d'hôte de la machine distante.

## Génération de clé SSH
La génération de clé SSH se fait avec la commande ssh-keygen.

La commande ssh-keygen génère une paire de clés composée d'une clé publique et une clé privée.

Par défaut, la paire de clés est générée dans le répertoire ~/.ssh.

La clé privée est stocké dans le fichier ~/.ssh/<key> et la clé publique est stockée dans le fichier ~/.ssh/<key>.pub.

La clé privée doit rester secrète. La clé publique peut être partagée avec n'importe qui.

Une phrase secrète peut être utilisée pour protéger la clé privée.

La phrase secrète est utilisée pour chiffrer la clé privée. 
La phrase secrète doit être saisie à chaque fois que une clé privée est utilisée.

## SCP généralités
SCP est un protocole qui vous permet de copier des fichiers depuis/vers une machine distante.

Il est un remplacement du protocole FTP.

Le protocole SCP s'appuie sur le protocole SSH pour authentifier le client.

SCP est une illustration de l'architecture client/serveur utilisant le protocole SSH.
