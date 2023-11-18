# Java TCP programming

## TCP
TCP est un protocole de transport. Il est utilisé pour transférer des données entre deux applications. 

TCP ne peut faire qu'UniCast : une seule application ne peut communiquer avec une autre application.
TCP est un protocole orienté connexion : une connexion doit être établie entre les deux applications avant que les données puissent être échangées dans une manière bidirectionnelle.
TCP est un protocole fiable : les données envoyées sont garanties d'être reçues par les autres applications.

Une bonne analogie consiste à considérer TCP comme un appel téléphonique. Vous devez d'abord établir une connexion avec l’autre personne avant de pouvoir lui parler. 
Une fois la connexion est établie, vous pouvez parler à l'autre personne et elle le fera entends tout ce que tu dis.

À l'aide des numéros de port, TCP permet à plusieurs applications de communiquer entre eux sur la même machine.

TCP est un protocole orienté flux : les données sont envoyées sous forme de flux d'octets.  L'application doit diviser les données en segments. Chaque segment est identifié par un numéro de séquence.
Les segments TCP sont encapsulés dans des paquets IP, appelés payloads.

Grâce aux numéros de séquence, TCP est capable de réassembler les segments dans le bon ordre. Si un segment est perdu, TCP le retransmettra.

## Le socket API
Le socket API est une API Java qui vous permet de créer des clients TCP/UDP et les serveurs. 
Il est décrit dans le package java.net du module java.base.

Il a été initialement développé en C dans le contexte du système d'exploitation Unix créé par l’Université de Berkeley.
Il a été porté sur Java et est maintenant disponible sur de nombreuses plateformes et langues.

Pour faire simple, un socket est comme un fichier que vous pouvez ouvrir, lire, écrire et fermer. 
Pour échanger des données, les sockets des deux côtés doivent être connectés.

Un socket est identifié par une adresse IP et un numéro de port.

Un socket peut agir en tant que client ou en tant que serveur :
-	Un socket acceptant des connexions est appelé un socket serveur (classe ServerSocket).
-	Un socket initiant une connexion est appelé socket client (classe Socket).

Le schéma suivant montre le workflow d'une application client/serveur 

Schema

## Traitement des données des flux
Les sockets utilisent des flux de données pour envoyer et recevoir des données, tout comme les fichiers.

Vous obtenez un flux d'entrée pour lire les données d'un socket et un flux de sortie pour écrire des données sur un socket.
```java
/ Get input stream
input = socket.getInputStream();
// Get output stream
output = socket.getOutputStream();
```

Vous pouvez ensuite décorer les flux d'entrée et de sortie avec d'autres flux pour traiter les données.
```java
// Get input stream as text
input = new InputStreamReader(socket.getInputStream(), StandardCharsets.UTF_8);
// Get output stream as text
output = new OutputStreamWriter(socket.getOutputStream(), StandardCharsets.UTF_8);
```

Utilisez des flux mis en mémoire tampon pour améliorer les performances.
```java
// Get input stream as binary with buffer
input = new BufferedReader(new InputStreamReader(socket.getInputStream());
// Get output stream as binary with buffer
output = new BufferedWriter(new OutputStreamWriter(socket.getOutputStream());
```

### Avertissement
N'oubliez pas de vider le flux de sortie après y avoir écrit des données.
Sinon, les données restantes dans le tampon ne seront pas envoyées au autre application !
```java
out.flush();
```
N’oubliez pas non plus toutes les bonnes pratiques vues dans le chapitre Java IOs (encodage, mise en mémoire tampon, etc.).
Il faut les appliquer ici aussi !


### Données de longueur variable
Selon le protocole d'application, les données envoyées peuvent avoir une variable longueur.

Il existe deux manières de gérer des données de longueur variable :
-	Utiliser un délimiteur
-	Utiliser une longueur fixe

Si les données ont un délimiteur, vous pouvez utiliser un lecteur tamponné pour lire les données
jusqu'à ce que le délimiteur soit trouvé.
```java
// End of transmission character
String EOT = "\u0004";
// Read data until the delimiter is found
String line;
while ((line = in.readLine()) != null && !line.equals(EOT)) {
  System.out.println(
  "[Server " + SERVER_ID + "] received data from client: " + line
  );
}
```

Si les données ont une longueur fixe, vous devez envoyer la longueur des données avant
envoyer les données elle-même.
```java
// Send the length of the data
out.write("DATA_LENGTH " + data.length() + "\n");
// Send the data
out.write(data);
// Read the length of the data
String[] parts = in.readLine().split(" ");
int dataLength = Integer.parseInt(parts[1]);
// Read the data
for (int i = 0; i < dataLength; i++) {
  System.out.print((char) in.read());
}
```

## Gestion d'un client à la fois
Un serveur qui gère un client à la fois est appelé monothread, ou serveur monothread.

Un serveur monothread est assez simple à mettre en œuvre :
1.	Il crée un socket pour écouter les connexions entrantes.
2.	Lorsqu'une connexion est acceptée, elle crée un socket pour communiquer avec le client.
3.	Il lit ensuite les données envoyées par le client et envoie une réponse.

Le principal inconvénient d'un serveur monothread est qu'il ne peut gérer qu’un client à la fois. 
Si un autre client tente de se connecter, il devra attendre jusqu'à ce que le premier client soit déconnecté.

Une analogie consiste à considérer un serveur monothread comme un restaurant avec seulement une table. 
Si un client est déjà assis à table, un autre client devra attendre le départ du premier client.

Un serveur monothread n’est donc pas adapté à la production. 
C'est adapté à des fins de test et d’apprentissage. Afin de gérer plusieurs clients, un serveur doit gérer plusieurs sockets. 
Il existe plusieurs façons de gérer plusieurs sockets en même temps, ils sont appelés « en concurrence ».


## Gérer plusieurs clients avec la concurrence
La concurrence est la capacité d'une application à gérer plusieurs clients en même temps.

Il existe plusieurs façons de gérer plusieurs clients avec la concurrence (parmi d’autres):
-	Multi-processing
-	Multi-threading
-	Programmation asynchrone

Java dispose d'un package pour la concurrence appelé java.util.concurrent.

### Multi-processing
Le multi-processing est la capacité d'une application à gérer plusieurs processus en même temps.

Un processus est un programme en exécution. 
Il est identifié par un ID de processus.

Un processus possède son propre espace mémoire. 
Il ne peut pas accéder à l'espace mémoire d’un autre processus.

Le processus principal est le processus créé au démarrage de l'application.

Il crée d'autres processus pour gérer plusieurs clients.

Un processus est un objet lourd. 
C'est assez coûteux à créer et détruire car il s’agit d’une copie du processus principal.

Les processus peuvent communiquer entre eux grâce à l’inter-process communication (IPC) mais c’est assez complexe à mettre en œuvre.

Une analogie consiste à considérer un processus comme une chaîne de restaurants comportant de multiples restaurant. 
Chaque restaurant n'a qu'une seule table et peut gérer un client. 
Si un client est déjà assis à une table d'un restaurant donné, un autre client peut s'asseoir à une table dans un autre restaurant.

### Multi-threading 
Le multi-threading est la capacité d'une application à gérer plusieurs threads à la fois.

Un thread est une séquence d'instructions qui peuvent être exécutées indépendamment du thread principal.

Le thread principal est le thread créé au démarrage de l’application.

Il crée d'autres threads pour gérer plusieurs clients.

Chaque thread possède sa propre pile et son propre compteur de programme.

Un thread est donc assez similaire à un processus, sauf qu'il partage le même espace mémoire que les autres threads. 
Il est donc beaucoup moins cher de créer et détruire qu'un processus (mais toujours plus coûteux qu'un simple objet).

Les threads peuvent communiquer entre eux en utilisant la mémoire partagée.

Les threads sont plus légers que les processus mais leur nombre est limité par le système d’exploitation.

Il existe deux manières de gérer les threads :
-	Threads illimités
-	Pool de threads qui limite le nombre de threads

Lorsque l’on parle de l’approche des threads illimités, une analogie consiste à penser à un restaurant sans table. 
Lorsqu'un nouveau client arrive, le gérant du restaurant ajoute une nouvelle table pour le client. 
Chaque table peut gérer un client.

En utilisant cette approche, plus les clients arrivent, plus les tables sont ajoutées.
Cette approche n'est pas adaptée à la production car l'espace et les ressources sont limité.

Lorsque l’on discute de l’approche du pool de threads, une analogie consiste à penser à un restaurant avec un nombre de tables limité. Lorsqu'un nouveau client arrive, le gérant du restaurant vérifie si une table est disponible. Si une table est disponible, le client peut s'asseoir à table. Si aucune table n'est disponible, le client devra attendre qu'une table soit disponible.
En utilisant cette approche, le nombre de tables est limité. 

Cette approche est adaptée à la production car l’espace et les ressources sont gérés et limités.

### Programmation asynchrone
La programmation asynchrone est la capacité d'une application à gérer plusieurs tâches en même temps, sans bloquer le thread principal.

Grâce à la programmation asynchrone, le thread principal peut effectuer d'autres tâches en attendant qu'une tâche soit terminée.

La programmation asynchrone est basée sur le concept de callbacks. 
Un callback est une fonction appelée lorsqu'une tâche est terminée.

Une analogie consiste à considérer la programmation asynchrone comme un food truck sans aucune table. 
Une fois qu'un client veut quelque chose à manger, la personne du food truck donne un ticket au client. 
Le client attend alors que la nourriture soit prête mais peut faire autre chose en attendant.
Une fois les plats prêts, la personne qui gère le food truck appelle le client. 
Le client vient ensuite au food truck pour récupérer la nourriture.

La programmation asynchrone est assez complexe à mettre en œuvre.

Node.js est un bon exemple de programmation asynchrone.
