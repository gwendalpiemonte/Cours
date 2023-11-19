# Java TCP Programming Questions

**Qu'est-ce qu'un socket ?**
- Un socket est un point de communication bidirectionnel entre des processus, généralement sur des ordinateurs différents à travers un réseau.
- Il permet l'échange de données entre ces processus.

**Quelle est la différence entre un socket serveur et un socket client ?**
- Un socket serveur est utilisé pour écouter les demandes de connexion entrantes et accepter les connexions des clients.
- En revanche, un socket client est utilisé pour initier une connexion vers un socket serveur.

**Quel est l'objectif de la concurrence ?**
- La concurrence vise à gérer efficacement plusieurs tâches simultanément afin d'améliorer les performances et l'efficacité d'un système, en particulier dans les environnements où de nombreuses opérations doivent être traitées en parallèle.

**Citez trois façons de gérer plusieurs clients avec la concurrence :**
- Thread par client : Chaque client est géré dans un thread séparé, permettant ainsi de gérer simultanément plusieurs clients.   
- Utilisation de processus légers (lightweight processes) : Ces entités de traitement sont plus légères que les threads traditionnels et peuvent être utilisées pour gérer plusieurs clients.
- Modèle asynchrone : En utilisant des mécanismes tels que les I/O asynchrones ou les événements, un seul processus peut gérer plusieurs clients sans bloquer le flux d'exécution.
