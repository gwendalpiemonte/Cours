# Docker et DockerCompose

# Table des matières:
1. [Objectifs](#1)
2. [Installation du logiciel : traditionnel vs conteneurisation](#2)
3. [OCI, images, conteneurs et registres](#3)
4. [Docker](#4)
5. [Docker Compose](#5)
6. [Docker Desktop](#6)
7. [Docker Hub et GitHub Container Registry](#7)
8. [Tips and tricks](#8)

## Objectifs <a name="1"></a>
Dans ce chapitre, vous apprendrez à quel point l'installation de logiciels peut être fastidieuse
et comment la conteneurisation peut vous y aider.

Vous apprendrez à installer et exécuter des logiciels sur votre ordinateur avec le
l'aide de Docker et Docker Compose pour éviter d'avoir à installer le
logiciel directement sur votre ordinateur.

C'est un chapitre assez long et complet. Il vous faudra du temps pour
complète le. N'hésitez pas à faire des pauses. Le contenu de ce chapitre sera
être utile pour la suite du cours.


## Installation du logiciel : traditionnel vs conteneurisation <a name="2"></a>
Lorsque vous souhaitez installer un logiciel sur votre ordinateur, la méthode traditionnelle est
pour télécharger un programme d'installation, exécutez-le et suivez les instructions. L'installateur va
installez le logiciel sur votre ordinateur et vous pourrez l'utiliser.

Le programme d'installation a écrit certains fichiers sur votre ordinateur et les a modifiés
certains paramètres. Il peut être assez difficile de savoir ce que l'installateur a fait
sur ton ordinateur. Il peut également être difficile de désinstaller le logiciel, car
vous devez savoir ce que l'installateur a fait. Même si l'installateur dispose d'un
programme de désinstallation, il n'aurait pas pu tout supprimer.

Le problème survient lorsque vous souhaitez installer une autre version du
logiciel ou sur un autre ordinateur : si vous installez la nouvelle version, elle
écraser l'ancienne version. Si vous souhaitez conserver l'ancienne version, vous devez
installez la nouvelle version dans un autre répertoire. Si tu veux garder les deux
versions, vous devez installer la nouvelle version dans un autre répertoire, et vous
vous devez modifier les paramètres de l'application pour utiliser la nouvelle version. Il
devient tout un gâchis.

Si vous souhaitez installer le logiciel sur un autre ordinateur, vous devez
modifier manuellement les paramètres ; il peut être difficile de savoir quoi changer.

La conteneurisation résout ces problèmes. Avec la conteneurisation, vous pouvez
installer le logiciel dans un conteneur. Le conteneur est un environnement virtuel
qui contient le logiciel et toutes ses dépendances. Le conteneur est
isolé du reste de l'ordinateur. Le conteneur peut être exécuté sur n'importe quel
ordinateur sur lequel le logiciel de conteneurisation est installé.

Chaque conteneur est indépendant des autres. Vous pouvez exécuter plusieurs
conteneurs sur le même ordinateur. Vous pouvez exécuter plusieurs conteneurs d'un
logiciels utilisant des versions différentes, sachant qu'elles n'interféreront pas avec
l'un l'autre.

Les conteneurs sont légers. Ils sont plus rapides à démarrer que les machines virtuelles.
Ils sont également plus rapides à créer et à détruire que les machines virtuelles.

Le logiciel de conteneurisation est appelé moteur de conteneur. Le plus
Le moteur de conteneurs le plus populaire est Docker. Docker est une implémentation de
Spécification de l’Open Container Initiative (OCI).


## OCI, images, conteneurs et registres <a name="3"></a>
La spécification OCI définit une norme pour les images de conteneurs et les conteneurs
durées d'exécution. La spécification OCI est implémentée par Docker, mais aussi par d'autres
moteurs de conteneurs.

La spécification OCI définit les termes suivants (entre autres) :

- Image : un modèle en lecture seule avec des instructions pour créer un conteneur
- Conteneur : une instance exécutable d'une image
- Registre : un service qui stocke des images

Une image de conteneur est un package qui contient tout ce dont vous avez besoin pour exécuter un
application. Il contient l'application et toutes ses dépendances. Ça aussi
contient des métadonnées sur l'image, telles que l'auteur, la version, le
descriptif, etc

Une image de conteneur est immuable. Il ne peut pas être modifié. Si tu veux
modifier une image de conteneur, vous devez créer une nouvelle image.

Une image conteneur est composée de calques. Chaque couche est un ensemble de fichiers. Le
les couches sont empilées les unes sur les autres. Les couches sont en lecture seule. Lorsque vous
modifier un fichier, le fichier est copié sur la couche supérieure. Le fichier original n'est pas
modifié.

Une image de conteneur est stockée dans un registre de conteneurs. Un registre de conteneurs est un
service qui stocke les images de conteneurs. Le registre de conteneurs le plus populaire est
DockerHub. Il s'agit d'un registre public. Vous pouvez également créer votre propre compte privé
enregistrement.

Une image de conteneur peut être téléchargée à partir d'un registre de conteneurs. Ça peut aussi
être téléchargé dans un registre de conteneurs.

Une image de conteneur peut être utilisée pour créer un conteneur. Un conteneur est un
instance exécutable d’une image. Un conteneur est créé à partir d'une image. C'est
possible de créer plusieurs conteneurs à partir de la même image.

Un conteneur est isolé du reste de l'ordinateur. Il est isolé de
d'autres conteneurs.


## Docker <a name="4"></a>
> Docker est un ensemble de produits de plateforme en tant que service (PaaS) qui utilisent OS-
> virtualisation de niveau pour fournir des logiciels dans des packages appelés conteneurs.

Docker est composé de deux parties :

- Le démon Docker : un service en arrière-plan qui gère les conteneurs
- Le Docker CLI : une interface en ligne de commande pour interagir avec le Docker démon

Sous Linux, le démon Docker s'exécute de manière native. La CLI Docker communique
avec le démon Docker via un socket.

Sous macOS et Windows, le démon Docker s'exécute dans une machine virtuelle. Le
Docker CLI communique avec le démon Docker via un socket.

La Docker CLI est utilisée pour gérer les conteneurs. Il est utilisé pour créer, démarrer, arrêter,
redémarrer, supprimer, etc. les conteneurs. Il est également utilisé pour gérer les images. C'est utilisé
pour télécharger, télécharger, créer, etc. des images.

### Spécification du fichier Docker (DockerFile)

La spécification Dockerfile définit une norme pour la création d'images Docker.
La spécification Dockerfile est implémentée par Docker, mais aussi par d'autres
moteurs de conteneurs.

La spécification Dockerfile définit les termes suivants (entre autres) :

- Dockerfile : un fichier texte contenant des instructions pour créer un Dockerimage
- Contexte de construction : un répertoire qui contient les fichiers nécessaires à la construction d'un Image Docker

La spécification Dockerfile définit un ensemble d'instructions. Chaque instruction
correspond à une commande exécutable dans un shell. Les instructions sont
exécuté dans l'ordre. Chaque instruction crée un nouveau calque dans l'image.

La spécification Dockerfile définit les instructions suivantes (parmi autres):
- `FROM` : spécifie l'image de base
- `ARG` : spécifie un argument à passer à la commande build
- `RUN` : exécute une commande dans le conteneur
- `COPIER` : copie les fichiers du contexte de construction vers le conteneur
- `CMD` : spécifie la commande à exécuter au démarrage du conteneur
- `ENTRYPOINT` : précise le point d'entrée du conteneur
- `ENV` : spécifie une variable d'environnement
- `EXPOSE` : spécifie le port à exposer
- `WORKDIR` : spécifie le répertoire de travail
- `VOLUME` : spécifie un volume

Un Dockerfile est ensuite utilisé pour créer une image Docker. Le Dockerfile est passé
à la commande `docker build`. La commande `docker build` construit l'image
à partir du fichier Docker. La commande `docker build` prend le Dockerfile et le
construire un contexte comme arguments.

Une fois l'image créée, elle peut être exécutée avec la commande `docker run`. Le
La commande `docker run` prend le nom de l'image comme argument.

La plupart des images Docker sont basées sur Linux mais d'autres sont également disponibles
(Windows par exemple). Il est possible d'exécuter des conteneurs Linux sur Linux,
macOS et Windows (avec l'aide de la machine virtuelle Linux).

Plus d'informations sur la spécification Dockerfile peuvent être trouvées dans le
documentation officielle : [ici](https://docs.docker.com/engine/reference/builder/)

### Considérations de sécurité

Un conteneur est isolé du reste de l'ordinateur. Il est isolé de
d'autres conteneurs. Il n'est pas isolé du démon Docker. Le Docker
le démon a accès au conteneur.

Un conteneur n'est pas une machine virtuelle. Ce n'est pas un bac à sable. Ce n'est pas une sécurité
frontière. Il ne s'agit pas d'une frontière de sécurité entre le conteneur et le
Démon Docker.

Le démon Docker s'exécute avec les privilèges root. Il faut être prudent quand
conteneurs en cours d’exécution. Une faille de sécurité dans un conteneur peut entraîner une
compromis de l'hôte. Essayez toujours d'exécuter des conteneurs avec un utilisateur non root.

Il n'est pas toujours possible d'exécuter un conteneur avec un utilisateur non root. Quelques
les conteneurs nécessitent les privilèges root pour s'exécuter. Certains conteneurs nécessitent un accès
au démon Docker. Ceci est généralement explicitement indiqué dans la documentation
du conteneur.

### Ignorer les fichiers

Lors de la création d'une image, Docker enverra le contexte de construction au Docker
démon.

Le contexte de build est le répertoire qui contient le Dockerfile. Ignorer
fichiers qui ne sont pas nécessaires pour créer l'image, vous pouvez créer un `.dockerignore`
fichier dans le contexte de construction. Le fichier `.dockerignore` est similaire au fichier `.gitignore`.

Cela peut être utile pour ignorer des fichiers tels que le répertoire `target` d'un Maven
projet ou des clés privées afin qu’elles ne soient pas envoyées au démon Docker.

### Cheatsheet

```sh
# Build and tag an image
docker build -t <image-name> <build-context>

# Start a container using its image name
docker run <image-name>

# Start a container in background
docker run -d <image-name>

# Display all running containers
docker ps

# Stop a container
docker stop <container-id>

# Access a running container
docker exec -it <container-id> /bin/sh

# Start a container and override the entry point
docker run --entrypoint /bin/sh <image-name>

# Start a container and override the command
docker run <image-name> <command>

# Delete all stopped containers
docker container prune

# Delete all images
docker image prune
```

## Docker Compose <a name="5"></a>

> Docker Compose est un outil pour définir et exécuter plusieurs conteneurs
> Applications Docker.

Docker Compose est un outil utilisé pour exécuter plusieurs conteneurs. C'est utilisé
pour exécuter plusieurs conteneurs liés les uns aux autres. Il est utilisé pour courir
plusieurs conteneurs faisant partie de la même application (un backend et son
base de données par exemple).

### Spécification Docker Compose

La spécification Docker Compose définit une norme pour définir et
exécuter des applications Docker multi-conteneurs. La composition Docker
la spécification est implémentée par Docker, mais aussi par d'autres outils.

La spécification Docker Compose définit les termes suivants (parmi autres):

- Service : un conteneur qui fait partie d'une application Docker multi-conteneurs
- Volume : un répertoire partagé entre le conteneur et l'hôte
- Réseau : un réseau partagé entre les conteneurs

Docker Compose permet de définir une application Docker multi-conteneurs dans un
Fichier Docker Compose. Il est plus facile à utiliser que les simples commandes Docker et
peut être versionné avec l’application.

Le format du fichier Docker Compose est YAML. Le fichier Docker Compose est
nommé `docker-compose.yml` par convention.

Plus d'informations sur la spécification Docker Compose peuvent être trouvées dans
la documentation officielle : [ici](https://docs.docker.com/compose/compose-déposer/)

### Docker Compose v1 et Docker Compose v2

Veuillez noter qu'il existe deux versions de Docker Compose : Docker Composez v1 et Docker Compose v2.

Docker Compose v1 est la version originale de Docker Compose. Il a été construit
avec Python et est désormais obsolète. Il est toujours disponible mais ce n'est pas le cas
recommandé de l'utiliser. La commande pour utiliser Docker Compose v1 était
`docker-compose`.

Docker Compose v2 est la nouvelle version de Docker Compose. Il est construit avec Go
et c'est la version recommandée à utiliser. La nouvelle commande pour utiliser Docker
Compose v2 est `docker compose`.

### Cheatsheet

```sh
# Start all services defined in the docker-compose.yml file
docker compose up

# Start all services defined in the docker-compose.yml file in background
docker compose up -d

# Display all running services
docker compose ps

# Stop all services defined in the docker-compose.yml file
docker compose down

# Check the logs of a service
docker compose logs <service-name>

# Check the logs of all services defined in the docker-compose.yml file docker compose logs
 Docker Compose

# Follow the logs of a service
docker compose logs -f <service-name>
```

## Docker Desktop <a name="6"></a>

Docker Desktop est le moyen le plus simple d'installer Docker sur votre ordinateur. C'est
disponible pour macOS et Windows. En tant qu'étudiant, vous pouvez l'utiliser gratuitement.

Docker Desktop gère une machine virtuelle Linux. La machine virtuelle Linux
exécute le démon Docker.

Comme Docker Desktop utilise une machine virtuelle, certaines configurations (principalement
réseau) peut être un peu différent de celui que vous pouvez trouver sur un Linux
machine (poste de travail ou serveur).

## Docker Hub et GitHub Container Registry <a name="7"></a>

Docker Hub est la plus grande bibliothèque et communauté au monde pour les conteneurs
images.

Docker Hub est un registre de conteneurs public. Il s'agit du registre par défaut pour Docker.

D'autres registres de conteneurs sont disponibles. Certains sont publics, d'autres sont privés.

Dans ce cours, nous utiliserons le GitHub Container Registry pour stocker nos
images.

## Tips and tricks <a name="8"></a>

Ces trucs et astuces ne sont pas obligatoires à connaître. Ils sont là pour vous aider
et pour votre culture générale.

### Healthchecks

Les Healthchecks sont utilisés pour vérifier si un conteneur est sain. Ils ont l'habitude de
vérifiez si le conteneur est prêt à accepter les demandes.

Les Healthchecks sont définis dans le Dockerfile. Ils sont définis avec les
Instructions HEALTHCHECK.

L'instruction HEALTHCHECK prend les arguments suivants :

- `--interval` : l'intervalle entre deux contrôles de santé
- `--timeout` : le délai d'expiration d'un contrôle de santé
- `--start-period` : le temps d'attente avant de démarrer les contrôles de santé
- `--retries` : le nombre de tentatives avant de considérer le conteneur malsains
- `CMD` : la commande à exécuter pour vérifier la santé du conteneur

Par exemple, l'instruction suivante définit un contrôle de santé qui s'exécute chaque
30 secondes et cela expire après 10 secondes :

```sh
HEALTHCHECK --interval=30s --timeout=10s \
CMD curl -f http://localhost/ || exit 1
```

Si aucun contrôle de santé n'est défini, Docker utilisera le contrôle de santé par défaut. Le
Le contrôle de santé par défaut consiste à vérifier si le conteneur est en cours d'exécution.

Si aucun contrôle de santé n'est défini, le conteneur sera considéré comme sain car
dès qu'il est en marche. Ce n'est pas toujours ce que vous souhaitez. Tu pourrais vouloir
attendez que le conteneur soit prêt à accepter les demandes.

Vous pouvez définir un `healthcheck` directement dans le fichier Docker Compose avec le
option de contrôle de santé. Il vérifiera ensuite la santé du conteneur au démarrage.

Par exemple, l'option suivante définit un contrôle de santé qui s'exécute tous les 30
secondes et cela expire après 10 secondes :

```sh
healthcheck:
  test: ["CMD", "curl", "-f", "http://localhost/"]
  interval: 30s
  timeout: 10s
```

### Libérez de l'espace

Les images Docker, les conteneurs et les volumes peuvent prendre beaucoup de place sur votre
ordinateur.

Vous pouvez utiliser les commandes suivantes pour libérer de l'espace :

```sh
# Delete all stopped containers, all networks not used by at least one container, all
  anonymous volumes not used by at least one container, all images without at
  least one container associated to them and all build cache

docker system prune --all --volumes
```

### Constructions en plusieurs étapes

Lorsque vous travaillez avec Docker, vous commencez généralement avec une image de base et vous le faites ce qui suit:

1. Installez les dépendances de votre application.
2. Copiez la source de votre application dans l'image.
3. Créez l'application.
4. Exécuter.

Ce processus crée une grande image. Il contient l'image de base, le
les dépendances, le code source et les outils de build, même s'ils ne le sont pas
plus nécessaire.

Vous pouvez utiliser des constructions en plusieurs étapes pour réduire la taille de l'image finale. Le le processus serait le suivant :

1. Commencez à partir d’une image de base nommée `builder`.
2. Installez les dépendances pour créer votre application.
3. Copiez la source de votre application dans l'image.
4. Créez l'application.
5. Commencez à partir d’une image de base nommée `runner`.
6. Installez les dépendances pour exécuter votre application (si nécessaire).
7. Copiez les artefacts de construction de l'image du `builder` vers l'image du `runner`.
8. Exécutez l'application.

L'image finale ne contiendra que les dépendances pour exécuter votre application
et les artefacts de construction. Il ne contiendra pas les outils de build, le code source ou
les dépendances pour construire votre application, réduisant considérablement la taille
de l'image.

### Constructions multi-architectures

Par défaut, Docker utilisera l'architecture de votre ordinateur. Si vous êtes
en utilisant un ordinateur avec une architecture `amd64`, Docker utilisera l'`amd64`
version de l'image.

Vous pouvez utiliser des versions multi-architectures pour créer des images pour plusieurs
architectures, telles que `amd64`, `arm64` et `armv7`. Vous pourrez ensuite publier le
images sur un registre. Lorsque vous extrairez l'image, Docker le fera
utiliser automatiquement la version qui correspond à l'architecture de votre
ordinateur, rendant votre application compatible avec plusieurs architectures.


