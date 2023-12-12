1. **Différence entre un conteneur et une image :**   
   Un conteneur est une instance en cours d'exécution d'une image Docker. Une image est un modèle de système de fichiers en lecture seule qui contient tout ce dont un conteneur a besoin pour s'exécuter : le code, les dépendances, les variables d'environnement, etc.

2. **Différence entre un fichier Dockerfile et un fichier Docker Compose :**   
   Un fichier Dockerfile est un fichier texte utilisé pour créer une image Docker en décrivant les étapes nécessaires pour assembler cette image. Un fichier Docker Compose est un fichier YAML utilisé pour définir et gérer des applications multi-conteneurs. Il permet de spécifier les services, les réseaux et les volumes utilisés par ces conteneurs.

3. **Différence entre les instructions RUN et CMD :**   
   L'instruction `RUN` est utilisée dans un Dockerfile pour exécuter des commandes lors de la création d'une image. Elle est utilisée pour installer des dépendances, configurer l'environnement, etc. L'instruction `CMD` est utilisée pour spécifier la commande par défaut à exécuter lorsqu'un conteneur basé sur cette image est lancé. Elle peut être écrasée lors du démarrage du conteneur.

4. **Différence entre les instructions ENTRYPOINT et CMD :**   
   L'instruction `ENTRYPOINT` définit la commande de base à exécuter lorsqu'un conteneur démarre. Elle peut être complétée par des arguments fournis lors du lancement du conteneur. L'instruction `CMD`, lorsqu'elle est utilisée avec `ENTRYPOINT`, fournit des arguments par défaut à la commande `ENTRYPOINT`.

5. **But de l'instruction HEALTHCHECK :**   
   L'instruction `HEALTHCHECK` permet de définir une commande pour vérifier l'état de santé d'un conteneur. Elle peut être utilisée pour indiquer à Docker de surveiller l'état d'une application à l'intérieur du conteneur et de prendre des mesures en cas de problème.

6. **But de l'instruction EXPOSE : Est-elle requise ?**   
   L'instruction `EXPOSE` indique les ports sur lesquels un conteneur écoute les connexions. Elle documente les ports utilisés par l'application, mais elle n'est pas obligatoire pour que les conteneurs communiquent entre eux ou avec l'hôte. Elle est surtout utile pour la documentation.

7. **Comment un volume peut-il être utilisé pour persister les données ?**   
   Un volume Docker est un moyen de stocker des données générées et utilisées par des conteneurs. En montant un volume dans un conteneur (à l'aide de l'option `-v` ou via Docker Compose), les données stockées dans ce volume persistiront même si le conteneur est supprimé. Cela permet de partager des données entre les conteneurs ou de sauvegarder des données importantes.
