# Java UDP programming

## Objectifs

Vous avez découvert et expérimenté le TCP dans le chapitre précédent. Vous avez appris que le TCP est un protocole orienté connexion, ce qui signifie qu'une connexion doit être établie avant d'envoyer des données.

Dans ce chapitre, vous découvrirez et expérimenterez avec l'UDP. L'UDP est un protocole sans connexion. Cela signifie qu'une connexion n'a pas besoin d'être établie avant l'envoi de données.

L'UDP est principalement utilisé lorsque la fiabilité n'est pas nécessaire. Il est utilisé pour le streaming, les jeux, etc.

L'UDP est sensiblement différent du TCP et il est important de comprendre les différences entre les deux protocoles.

## UDP

L'UDP est un protocole de couche de transport, tout comme le TCP. Il est utilisé pour envoyer des données sur le réseau. Cependant, il existe de nombreuses différences entre le TCP et l'UDP.

L'UDP est un protocole sans connexion, ce qui signifie qu'il ne nécessite pas d'établir une connexion avant d'envoyer des données.

L'UDP ne fournit aucun mécanisme de fiabilité. Il ne garantit pas que les données seront reçues par le destinataire, ni que les données seront reçues dans le même ordre qu'elles ont été envoyées.

Une bonne analogie est de penser à l'UDP comme au service postal avec des cartes postales. Vous écrivez plusieurs cartes postales et les envoyez à quelqu'un. Vous ne savez pas si les cartes postales seront reçues ni si elles arriveront dans le même ordre qu'elles ont été envoyées. Vous ne savez pas non plus si les cartes postales seront reçues du tout si le service postal les perd.

Tout comme avec les cartes postales, l'UDP est utilisé lorsque la fiabilité n'est pas nécessaire.

## Différences entre TCP et UDP

Le tableau suivant résume les différences entre le TCP et l'UDP.

| TCP                                 | UDP                                        |
| ----------------------------------- | ------------------------------------------ |
| Orienté connexion                   | Sans connexion                              |
| Fiable                              | Non fiable                                 |
| Protocole de flux                   | Protocole de datagramme                    |
| Unicast                             | Unicast, broadcast et multicast            |
| Requête-réponse                     | Envoyer et oublier, requête-réponse (manuel) |
| -                                   | Protocoles de découverte de service        |
| Utilisé pour FTP, HTTP, SMTP, SSH, etc. | Utilisé pour DNS, streaming, jeux, etc.   |


## Datagrammes UDP

Contrairement au TCP, l'UDP n'est pas un protocole de flux. C'est un protocole de datagrammes. Cela signifie que l'UDP envoie des données sous forme de paquets discrets appelés datagrammes.

Les datagrammes sont similaires aux cartes postales dans l'analogie précédente. Ils sont envoyés indépendamment les uns des autres. Ils ne sont pas liés entre eux. Ils contiennent une adresse de destination, une charge utile et l'adresse de l'expéditeur. Si nécessaire, vous pouvez utiliser l'adresse de l'expéditeur pour lui répondre.

Les datagrammes UDP sont composés d'un en-tête et d'une charge utile. L'en-tête contient des informations sur le datagramme, telles que le port source et de destination. La charge utile contient les données à envoyer.

La taille de la charge utile est limitée à 65 507 octets. Cela est dû au fait que la longueur de la charge utile est encodée sur 16 bits dans l'en-tête.

La charge utile d'un datagramme UDP peut être une notification, une requête, une demande, une réponse, etc. C'est à l'application de définir le format de la charge utile.

Si la charge utile est trop grande, le datagramme sera fragmenté. Cela signifie que la charge utile sera divisée en plusieurs datagrammes. Le destinataire devra réassembler les datagrammes pour obtenir la charge utile d'origine.

## Fiabilité

Comme l'UDP ne fournit aucun mécanisme de fiabilité, c'est à l'application de l'implémenter. Par exemple, l'application peut mettre en place un mécanisme pour accuser réception d'un datagramme et le retransmettre s'il n'a pas été reçu.

Ce qui est offert par TCP doit être implémenté par l'application avec UDP.

Dans certains cas, la fiabilité n'est pas nécessaire. Certaines applications tolèrent la perte de données.

Par exemple, le streaming est un cas d'utilisation parfait pour l'UDP. Si un datagramme est perdu, cela n'a pas beaucoup d'importance : le récepteur recevra le datagramme suivant et le flux continuera.

Si quelques datagrammes sont perdus, le récepteur pourrait le remarquer avec quelques perturbations (sur des vidéos avec des artefacts, comme un flux Twitch si votre connexion n'est pas bonne, par exemple), mais cela n'affectera pas le flux.

Si trop de datagrammes sont perdus, le récepteur ne pourra pas réassembler la charge utile et le flux s'arrêtera.

Comme mentionné précédemment, c'est à l'application d'implémenter un mécanisme de fiabilité si nécessaire (avec un identifiant de message et un accusé de réception, par exemple, tout comme TCP).

On peut illustrer ceci avec l'exemple suivant :

Vous avez développé un protocole d'application très simple où les clients peuvent envoyer des commandes `INCREMENTER` et `DECREMENTER` pour incrémenter/décrémenter un compteur sur le serveur. Le compteur est partagé entre tous les clients.

Si les clients envoient 10 commandes `INCREMENTER`, le compteur devrait être incrémenté de 10.

Dans un monde parfait, le serveur recevrait 10 commandes `INCREMENTER` et le compteur serait incrémenté de 10.

Cependant, nous savons qu'un des datagrammes pourrait être perdu. Si le serveur reçoit 9 commandes `INCREMENTER`, le compteur sera incrémenté de 9 au lieu de 10.

Les deux parties (le client et le serveur) pourraient implémenter un mécanisme de fiabilité pour résoudre ce problème.

Le serveur pourrait mettre en place un mécanisme de fiabilité pour accuser réception d'un datagramme. Si un client ne reçoit pas d'accusé de réception _dans une période spécifique_, il devrait retransmettre le datagramme.

Cependant, même l'accusé de réception pourrait être perdu. Le client pourrait retransmettre le datagramme plusieurs fois et le serveur pourrait le recevoir plusieurs fois.

Le serveur pourrait implémenter un mécanisme pour détecter les datagrammes en double et les ignorer. Il pourrait également implémenter un mécanisme pour détecter les datagrammes désordonnés et les réordonner.

La gestion de la fiabilité est assez difficile. Dans le contexte de ce cours, la fiabilité n'est pas requise. Nous nous concentrerons sur le protocole UDP lui-même et non sur le(s) mécanisme(s) de fiabilité.

Si vous êtes intéressé, vous pouvez jeter un œil au [protocole de Demande de Répétition Automatique (ARQ)](https://fr.wikipedia.org/wiki/Automatic_Repeat_reQuest). C'est un mécanisme utilisé pour détecter et retransmettre les datagrammes perdus.

## UDP dans l'API Socket

Comme vu dans le chapitre sur la [programmation TCP en Java](https://github.com/heig-vd-dai-course/heig-vd-dai-course/tree/main/13-java-tcp-programming), l'API Socket est une API Java qui vous permet de créer des clients et des serveurs TCP/UDP. Elle est décrite dans le [package `java.net`](https://docs.oracle.com/en/java/javase/17/docs/api/java.base/java/net/package-summary.html) du [module `java.base`](https://docs.oracle.com/en/java/javase/17/docs/api/java.base/module-summary.html).

Dans le monde UDP, la classe `Socket` est remplacée par la classe `DatagramSocket`.

La classe `DatagramSocket` est utilisée pour créer des clients et des serveurs UDP. Elle est utilisée pour envoyer et recevoir des datagrammes UDP.

Un datagramme est créé avec la classe `DatagramPacket`. Elle est utilisée pour créer un datagramme avec une charge utile et une adresse de destination.

Un socket de multidiffusion (multicast) est créé avec la classe `MulticastSocket`. Il est utilisé pour créer un datagramme de multidiffusion avec une charge utile et une adresse de multidiffusion, permettant à plusieurs hôtes de recevoir le datagramme.

UDP peut être utilisé pour créer une architecture client-serveur. Cependant, ce n'est pas obligatoire. Il est possible de créer une architecture pair-à-pair avec UDP.

## Unicast, broadcast et multicast

Contrairement au TCP, l'UDP prend en charge trois types de communication : unicast, broadcast et multicast (le TCP prend en charge uniquement unicast).

### Unicast

Le unicast est le type de communication le plus courant. C'est une communication de un à un. Cela signifie qu'un datagramme est envoyé d'un hôte à un autre, tout comme le TCP.

Pensez-y comme une conversation privée entre deux personnes.

Pour envoyer un datagramme de unicast, l'expéditeur doit connaître l'adresse IP et le port du destinataire. C'est principalement similaire au TCP, sans toutes les fonctionnalités fournies par le TCP mais avec toutes les performances de l'UDP.

### Broadcast

Le broadcast est une communication de un à tous. Cela signifie qu'un datagramme est envoyé d'un hôte à tous les hôtes sur le réseau.

Pensez-y comme une annonce publique.

Pour envoyer un datagramme de broadcast, l'expéditeur doit connaître l'adresse de diffusion. L'adresse de diffusion est une adresse spéciale qui représente tous les hôtes sur le réseau et/ou tous les hôtes d'un sous-réseau spécifique.

L'adresse de diffusion est définie par le masque de sous-réseau. Le masque de sous-réseau est un nombre de 32 bits. Il est représenté sous forme de quatre nombres séparés par un point (par exemple `255.255.255.0`). Parfois, le masque de sous-réseau est représenté par un seul nombre (par exemple `/24` pour `255.255.255.0` car 24 bits sont définis à `1`).

Un bon exemple est donné dans le tableau suivant (source : <https://en.wikipedia.org/wiki/Broadcast_address>) :

| Décomposition de l'adresse IP réseau pour `172.16.0.0/12`                                                                                                                                                                                                                     | Forme binaire                             | Notation décimale par points |
| ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------ | ---------------------------- |
| 1. Adresse IP réseau                                                                                                                                                                                                                                                        | 10101100.0001**0000.00000000.00000000** | 172.16.0.0                   |
| 2. Masque de sous-réseau, ou simplement "Masque" pour faire court (Le `/12` dans l'adresse IP signifie que seuls les 12 bits les plus à gauche sont des `1`, comme montré ici. Cela réserve les 12 bits les plus à gauche pour l'adresse du réseau (préfixe) et les `32 - 12 = 20` bits à droite pour l'adresse de l'hôte (suffixe).) | 11111111.1111**0000.00000000.00000000** | 255.240.0.0                  |
| 3. Complément de bits (NON logique) du masque de sous-réseau                                                                                                                                                                                                                  | 00000000.0000**1111.11111111.11111111** | 0.15.255.255                 |
| 4. Adresse de diffusion (OU logique de _Adresse IP réseau_ et du _Complément de bits du Masque_. Cela fait de l'adresse de diffusion l'_adresse IP (et l'adresse de l'hôte, car la partie adresse de l'hôte est entièrement composée de `1`s) la plus grande possible pour une adresse réseau donnée._)               | 10101100.0001**1111.11111111.11111111** | 172.31.255.255               |

Si vous souhaitez envoyer une diffusion à tous les appareils sur tous les sous-réseaux du réseau, vous pouvez utiliser l'adresse de diffusion `255.255.255.255`.

> **Attention**  
> Vous devez être conscient qu'il peut y avoir des restrictions quant à l'utilisation de la diffusion. Par exemple, la diffusion est limitée au réseau local mais peut tout de même être bloquée par un pare-feu et/ou un routeur.

### Multidiffusion

La multidiffusion est une communication de type un-vers-plusieurs. Cela signifie qu'un datagramme est envoyé d'un hôte à plusieurs hôtes.

On peut le comparer à une conversation de groupe.

Pour envoyer un datagramme en multidiffusion, l'émetteur utilise une adresse de multidiffusion. L'adresse de multidiffusion est une adresse spéciale qui représente un groupe d'hôtes sur le réseau. On peut la comparer à une chaîne radio ou à un salon Discord : tout le monde sur la chaîne recevra les messages envoyés dans ce salon particulier.

Les adresses de multidiffusion sont des adresses IP spécifiques dans la plage allant de `224.0.0.0` à `239.255.255.255` pour IPv4 et `f00::/8` pour IPv6.

Tout comme pour les ports, certaines adresses de multidiffusion sont réservées à des fins spécifiques. Une liste complète est disponible sur le [site de l'IANA](https://www.iana.org/assignments/multicast-addresses/multicast-addresses.xhtml) et décrite plus en détail dans le [RFC 5771](https://datatracker.ietf.org/doc/html/rfc5771).

Pour les réseaux locaux, la plage de multidiffusion est issue du **Bloc à Portée Administrative** du RFC. Plus de détails sont disponibles dans le [RFC 2365](https://datatracker.ietf.org/doc/html/rfc2365).

Toutes les adresses de multidiffusion dans la plage `239.0.0.0` à `239.255.255.255` peuvent être utilisées pour vos propres applications.

Tout comme pour la diffusion, l'émetteur doit connaître l'adresse de multidiffusion pour envoyer un datagramme à un groupe de multidiffusion. Tout comme pour la diffusion également, il peut y avoir des restrictions quant à l'utilisation de la multidiffusion.

La multidiffusion est pratiquement **impossible** à utiliser sur Internet public. Elle est seulement garantie de fonctionner sur un réseau local. Si vous avez besoin d'utiliser la multidiffusion entre plusieurs réseaux, vous devez utiliser un tunnel tel qu'un réseau privé virtuel (VPN) pour contourner cette restriction.

La multidiffusion est présentée dans ce cours car c'est un concept important dans les protocoles de découverte de services. Cependant, vous devez savoir qu'il est pratiquement impossible d'utiliser la multidiffusion sur Internet public, ce qui limite grandement son utilisation.

De plus, la multidiffusion est un sujet complexe. Elle n'est pas abordée en profondeur dans ce cours. Pour une meilleure compréhension des utilisations possibles de la multidiffusion sur Internet, vous pouvez consulter les ressources suivantes :

- [Multidiffusion IP](https://fr.wikipedia.org/wiki/Multicast_IP)
- [Protocole de gestion de groupe Internet](https://fr.wikipedia.org/wiki/Internet_Group_Management_Protocol)
- [Télévision par protocole Internet](https://fr.wikipedia.org/wiki/T%C3%A9l%C3%A9vision_par_protocole_Internet)

## Schémas de messagerie

Comme UDP ne fournit pas de mécanisme de connexion, il appartient à l'application de définir le schéma de messagerie (comment envoyer et recevoir des données).

Il existe deux schémas de messagerie courants : le mode envoyer-et-oublier (fire-and-forget) et la requête-réponse.

Le mode envoyer-et-oublier est le schéma de messagerie le plus simple. C'est une communication à sens unique. Cela signifie qu'un datagramme est envoyé d'un hôte à un autre sans attendre de réponse.

Le mode envoyer-et-oublier est utilisé lorsque l'émetteur n'a pas besoin de savoir si le datagramme a été reçu ou non.

Le schéma de requête-réponse (parfois appelé requête-réponse) est une communication bidirectionnelle. Cela signifie qu'un datagramme est envoyé d'un hôte à un autre, et une réponse est attendue.

Lors de la création d'un datagramme, il est possible de spécifier un port. Bien que cela ne soit pas obligatoire, ce port peut être utilisé par le récepteur pour savoir à qui répondre.

Si aucun port n'est spécifié, le système d'exploitation attribuera simplement un port aléatoire au datagramme sortant.

Le destinataire du datagramme peut alors extraire l'adresse IP et le port de l'expéditeur et les utiliser pour répondre à l'expéditeur en utilisant unicast.

Le schéma de requête-réponse peut être utilisé lorsque l'émetteur a besoin de savoir si le datagramme a été reçu ou non.

Les deux côtés de la communication peuvent envoyer une requête et recevoir une réponse.

## Protocoles de découverte de services

Avec unicast, l'émetteur doit connaître le destinataire ; l'émetteur doit connaître l'adresse IP du destinataire.

Avec la diffusion (broadcast) et la multidiffusion (multicast), l'émetteur n'a pas besoin de connaître les destinataires ; l'émetteur n'a pas besoin de connaître l'adresse IP des destinataires. L'émetteur sait que les nœuds à proximité (ou ceux qui ont exprimé un intérêt pour la diffusion) recevront le datagramme.

En utilisant cette propriété, il est possible de créer des protocoles de découverte de services.

Les protocoles de découverte de services sont utilisés pour découvrir des services sur le réseau. Ils sont utilisés pour trouver des services sans connaître leur adresse IP.

Il existe deux types de protocoles de découverte de services : passifs et actifs.

Les protocoles de découverte de services passifs sont basés sur la diffusion ou la multidiffusion. Ils sont utilisés pour annoncer la présence d'un service sur le réseau.

Les protocoles de découverte de services actifs sont également basés sur la diffusion ou la multidiffusion mais passent ensuite à unicast. Ils sont utilisés pour interroger le réseau afin de trouver un service.

Il existe de nombreux schémas de protocoles de découverte de services. Les plus courants sont les suivants :

- Publicité - Un schéma de protocole de découverte passif : un serveur (appelé fournisseur de services) annonce sa présence sur le réseau. Le fournisseur de services envoie un datagramme de diffusion ou de multidiffusion pour annoncer sa présence. Le datagramme contient des informations sur le service (nom, adresse IP, port, etc.). Le datagramme est envoyé périodiquement pour annoncer que le service est toujours disponible.

  Les clients (appelés consommateurs de services) écoutent les datagrammes de diffusion ou de multidiffusion pour découvrir les services sur le réseau.

  Si un consommateur de services est intéressé par l'annonce du fournisseur de services, il peut manifester son intérêt.

  ![Protocoles de découverte de services - Schéma de publicité](images/service-discovery-protocols-advertisement.png)

- Requête - Un schéma de protocole de découverte actif : un client (appelé consommateur de services) interroge le réseau pour trouver un service. Le client envoie un datagramme unicast sur le réseau pour demander des informations sur un service.

  Si un service qui fournit le service demandé (appelé fournisseur de services) est disponible, il répond avec un datagramme unicast contenant les informations demandées pour se connecter au service, tout comme avec le schéma de messagerie requête-réponse.

  ![Protocoles de découverte de services - Schéma de requête](images/service-discovery-protocols-query.png)

Ces schémas peuvent encore être utilisés avec d'autres protocoles tels que TCP.

