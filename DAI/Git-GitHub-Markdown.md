# Git
Git est un système de contrôle de version (VCS) distribué, libre et gratuit.
conçu pour gérer tous les projets, des plus petits aux plus grands, avec rapidité et efficacité. Créé par Linus Torvalds en 2005 pour gérer le code source du noyau Linux,
Git permet de suivre les modifications apportées à un ensemble de fichiers.
travail entre les programmeurs pendant le développement d'un logiciel.

## Architechture
Git est un système client-serveur, où le serveur est appelé **repository** et les
clients sont appelés **clones**.

Le repository est la source unique de vérité, et les clones sont les copies locales du repository.

Git est un VCS **distributed**, ce qui signifie que chaque clone est une copie complète du repository. Cela permet de travailler hors ligne.

## Commits, hashes et tags
Git utilise les **commits** pour suivre les modifications. Un commit est un instantané du dépôt à un moment donné.
Chaque commit possède un identifiant unique, appelé **hash**. Le hash est calculé à partir du contenu du commit,
Il est donc impossible de modifier un commit sans changer son hash.

Les commits peuvent être **tagged** pour créer une référence à un commit. Ceci est souvent utilisé
pour marquer un commit en tant que release.

Les commits peuvent être signées pour prouver qu'elles ont été effectuées par une personne spécifique.
personne. Ceci est fait pour des raisons de sécurité.

## Branches
Git utilise des **branches** pour suivre les différentes versions du repository. La branche par défaut
est souvent appelée **main** (l'ancien nom était master).

Chaque branche a un nom et un pointeur vers un commit. Le pointeur est appelé
**head**. La head de la branche principale est appelée **HEAD**.

Souvent, lors de l'implémentation d'une nouvelle fonctionnalité, une nouvelle branche est créée. Pour ce faire, on crée une nouvelle branche à partir de la branche principale.

Une fois que vous avez effectué tous les changements, les fichiers modifiés sont **staged** et un nouveau
commit est créé. Le commit est alors **pushed** vers le repository.

Les commits peuvent être comparées pour voir les **différences** entre les fichiers mis à disposition et le repository de travail.
Cela se fait en comparant les fichiers avec le dernier commit.

Les commits peuvent être **pulled** du repository pour être placées dans la branche courante.

## Merging branches
La collaboration sur un projet se fait en créant des branches, en apportant et en validant des modifications, en pushant et en mergant les branches dans le projet cible.

Il existe trois façons principales de fusionner les branches :

- **Merge** :  merge les modifications des deux branches dans un nouveau commit.
               C'est le comportement par défaut de Git.
- **Rebase** : ajouter la branche source à la branche cible afin qu'aucun nouveau commit ne soit créé.
               commits ne soit créé. Il s'agit d'une technique plus avancée.
- **Squash** : combiner plusieurs commits en un seul pour réduire le nombre de commits dans une branche.

## Conflicts
Le travail en collaboration peut donner lieu à des **conflicts**. Les conflits surviennent lorsque deux ou plus
personnes modifient le même fichier en même temps. Git est capable de
détecter les conflits et demande à l'utilisateur de les résoudre.

## Ignore files
Les fichiers peuvent être ignorés par Git. Cela se fait en créant un fichier .gitignore à la racine du repository.
Cela permet d'éviter de livrer des fichiers qui ne devraient pas être livrés, tels que les fichiers de configuration de l'IDE.
Si possible ne pas utiliser les générateurs de gitignore (tels que https://gitignore.io/), 
car ils ajoutent souvent trop de fichiers à la liste des fichiers ignorés. Il est préférable
d'ajouter des fichiers à la liste des ignorés au fur et à mesure que vous en avez besoin.



## Git Cheat sheet
```bash
# Clone a Git repository
git clone <url>

# Create a branch and switch to it
git checkout -b <branch-name>

# Switch to a branch
git checkout <branch-name>

# Add changes to the staging area
git add <file>

# View differences between the working directory and the staging area
git diff <file>

# Check Git status
git status

# Commit changes
git commit -m "Commit message"

# Push changes to a branch
git push origin <branch-name>

# Pull changes from a branch
git pull origin <branch-name>

# Merge a branch into another
git merge <branch-name>
```
   
# GitHub
GitHub est une plateforme et un service cloud pour les logiciels de
développement et contrôle de version à l'aide de Git, permettant aux développeurs de stocker et gérer leur code.

GitHub est un service d'hébergement de repository Git basé sur le Web. Il fournit tous les
fonctionnalités de Git, ainsi que quelques fonctionnalités supplémentaires.

Il s'agit du service d'hébergement de repository Git le plus populaire, avec plus de 372
millions de repository (janvier 2023), foyer de nombreux projets open source et un endroit où de nombreuses entreprises hébergent leurs repository privés.

## Social platform
GitHub est une plateforme **sociale**. Il permet de **follow** d'autres utilisateurs et de **star** des 
repository. Il permet également de **fork** des repository, ce qui signifie créer un
copie d'un repository dans votre propre compte.

## Users and organizations
Les repository peuvent appartenir à un utilisateur ou à une organisation.
Une organisation peut avoir plusieurs membres et les repository peuvent être partagés
entre les membres. C'est une bonne pratique de créer une organisation pour un
projet sur lequel plusieurs personnes travailleront.

## Issues, pull requests and forks
GitHub fournit également un wiki pour chaque repository, un **project board** pour gérer des
tâches, un outil de suivi d'**issue** pour signaler les bugs et un système de **pull request** à **review**
et **merge** les modifications.

Contribuer à un projet hébergé sur GitHub se fait en **forkant** le
repository, apporter des modifications et ouvrir une **pull request** pour merge le
repository dans le repository d'origine.

Le forking n'est nécessaire que si vous n'êtes pas membre du repository ou
de l'organisation.

Une fois qu'une branche est prête à être merge avec la branche cible, c'est une bonne
pratique d'ouvrir une pull request (ou une **merge request** sur GitLab). Une pull
request est une demande de merge d’une branche dans une autre. C'est une façon de discuter
les modifications avant de les fusionner.

L’examen d’une pull request se fait en **commentant** les modifications. Le
le réviseur peut demander que des modifications soient apportées avant de merge la branche ou peut
approuver les modifications.

Une fois la pull request approuvée, la branche peut être merge dans la
branche cible.

Par défaut, GitHub ne supprime pas la branche une fois qu'elle est fusionnée. Il est possible de
l'activer par défaut dans les paramètres de votre repository ou le supprimer
manuellement. Cela évite de cumuler des branches obsolètes dans votre repository.
