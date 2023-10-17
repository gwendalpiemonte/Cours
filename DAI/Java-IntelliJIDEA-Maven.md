# Java, IntelliJ IDEA et Maven

## Java

[Java](https://www.java.com/) est un outil à usage général, basé sur les classes,
langage de programmation orienté objet. Il est destiné à permettre aux programmeurs de coder en
une fois, exécutable n'importe où (WORA), ce qui signifie que le code Java compilé peut s'exécuter sur toutes
les plateformes supportant Java, grâce à la machine virtuelle Java (JVM).

Java a été créé par James Gosling chez Sun Microsystems (qui fait maintenant partie d'Oracle
Corporation) et est sorti en 1995.

Java est un langage de programmation très populaire, notamment pour les applications Web client-serveur.

### Machine virtuelle Java

Java est un langage **compilé**, ce qui signifie que le code source est compilé en
bytecode, qui est ensuite exécuté par une **machine virtuelle Java (JVM)**.

Java est destiné à être **portable**, ce qui signifie que le code Java compilé peut s'exécuter sur
toutes les plateformes prenant en charge Java, sans avoir besoin de recompilation, grâce à
la JVM.

### Versions de la JVM

De nombreuses implémentations de la JVM existent, ciblant **différents matériels et
environnements logiciels et/ou optimisations spécifiques** pour une plateforme donnée et/ou pour chaque cas d’utilisation.

Afin d'installer Java sur votre ordinateur, vous pouvez trouver le **JDK (Java
Development Kit)** ou les packages **JRE (Java Runtime Environment)**.

Si vous souhaitez développer des applications Java, vous aurez besoin du JDK. Si vous voulez
exécuter des applications Java, vous aurez besoin du JRE.

### Versions Java et gestionnaires de versions

Java a différentes **versions**, chacune avec son **propre ensemble de fonctionnalités et
améliorations**. La dernière version de support à long terme (LTS) est **Java 17**.

Comme les projets peuvent utiliser différentes versions de Java, il est courant d'utiliser une version avec un **
gestionnaire** tel que [SDKMAN!](https://sdkman.io/) ou [asdf](https://asdf-vm.com/).

Les gestionnaires de versions vous permettent d'**installer et de basculer entre différentes versions de
Java**.

Lorsque vous travaillez sur un projet, vous devez **utiliser la même version de Java que celle
des autres développeurs** pour garantir que le projet se compile et s'exécute correctement.

Vous pouvez développer des applications Java à l'aide d'un éditeur de texte et de la ligne de commande, mais
il est plus pratique d'utiliser un **environnement de développement intégré (IDE)**.

### Compilation et exécution de programmes Java

Une (simple) application Java peut être compilée à l'aide de la commande `javac` :

```bash
javac BonjourMonde.java
```

Le bytecode résultant peut être exécuté à l'aide de la commande `java` :

```bash
java BonjourMonde
```

Sortie:

```texte
Bonjour les étudiants du DAI !
```

Une application Java peut être regroupée dans un fichier **JAR (Java ARchive)**, qui est
un **fichier ZIP** contenant le bytecode compilé et d'autres ressources.

Un fichier JAR peut être exécuté à l'aide de la commande `java` :

```bash
java -Xmx1024M -Xms1024M -jar minecraft_server.1.20.1.jar nogui
```

Les options `-Xmx1024M` et `-Xms1024M` définissent le **maximum** et l'**initial**
pool d'allocation de mémoire pour une machine virtuelle Java (JVM), respectivement.

Ces options peuvent modifier les performances de la JVM, en fonction de l'application.

Comme de nombreuses applications Java dépendent de bibliothèques externes, il est courant d'utiliser un
**gestionnaire de dépendances** tel que **[Maven](https://maven.apache.org/)** ou **[Gradle](https://gradle.org/)**.

### Résumé

- Java est un langage de programmation généraliste, basé sur des classes et orienté objet.
- Java est compilé en bytecode, qui est ensuite exécuté par une machine virtuelle Java (JVM).
- Java se veut portable, grâce à la JVM.
- Java existe en différentes versions, chacune avec son propre ensemble de fonctionnalités et d'améliorations.
- Les gestionnaires de versions vous permettent d'installer et de basculer entre différentes versions
  de Java.

## IntelliJ IDEA

[IntelliJ IDEA](https://www.jetbrains.com/idea/) est un développement intégré
environnement (IDE) écrit en Java pour le développement de logiciels informatiques.
Développé par JetBrains et disponible en tant que communauté sous licence Apache 2
édition, et dans une édition commerciale propriétaire.

IntelliJ IDEA est un IDE très populaire pour le développement Java, mais il prend également en charge
de nombreux autres langages de programmation.

### Fichiers de configuration et Git

Lors de la création d'un nouveau projet, IntelliJ IDEA créera un répertoire `.idea`
contenant les fichiers de configuration du projet.

Certains de ces fichiers doivent être **ignorés** par Git, car ils contiennent des **
configurations locals** spécifique à votre ordinateur.

Les autres fichiers doivent être **commités** dans Git, car ils contiennent une **configuration projet** partagée entre tous les développeurs.

Cela vous permet de **partager la configuration du projet** avec d'autres développeurs, donc
ils peuvent ouvrir le projet dans leur instance d'IntelliJ IDEA et avoir la
même configuration que assure que le projet se compile et s'exécute
correctement.

### Résumé

- IntelliJ IDEA est un environnement de développement intégré (IDE) écrit en Java pour le développement de logiciels informatiques.
- IntelliJ IDEA est disponible en deux éditions : la Community Edition (gratuite etopen-source) et l'Ultimate Edition (propriétaire).
- Lors de la création d'un nouveau projet, IntelliJ IDEA créera un répertoire `.idea`contenant les fichiers de configuration du projet.
- Certains de ces fichiers doivent être ignorés par Git, car ils contiennent des configuration spécifique à votre ordinateur.

## Maven

[Apache Maven](https://maven.apache.org/) est un logiciel de gestion de projet et
un outil de compréhension. Basé sur le concept de modèle objet de projet (POM),
Maven peut gérer la construction, le reporting et la documentation d'un projet à partir d'une information.

Maven est un **gestionnaire de dépendances** pour les projets Java. Il est utilisé pour **gérer les
bibliothèques externes** (également appelées **dépendances**) utilisées par votre application.
Maven est un **outil de ligne de commande**. Il peut être utilisé à l'aide de la commande `mvn`.

Maven est également un **outil d'automatisation de build**. Il est utilisé pour **compiler** votre
application, **exécutez** vos tests unitaires, **packager** votre application, etc.

### Structure du projet Maven

Maven définit une **structure de répertoires standard** pour les projets Java, afin que tous
les développeurs puissent trouver le code source, les tests unitaires, etc. au même endroit. Il
**standardise le processus de build** de votre application, afin que tous les développeurs
puissent construire le projet de la même manière.

Lors de la création d'un nouveau projet dans IntelliJ IDEA, vous pouvez choisir entre différents **modèles de projet**.

Dans ce cours, vous utiliserez le modèle de projet **Maven**.

IntelliJ IDEA créera automatiquement une **structure de projet Maven** pour vous, avec les fichiers suivants :

- `pom.xml` : le fichier **Project Object Model (POM)**, qui est le cœur d'un Projet Maven.
- `src/main/java` : le **code source** de votre application.
- `src/test/java` : les **tests unitaires** de votre application.

### Fichier `pom.xml`

Le fichier `pom.xml` contient la **configuration** de votre projet Maven.

Il contient également la **configuration de build** de votre application, qui définit
comment votre application est compilée, testée, packagée, etc.

Il contient aussi les **dépendances** de votre application, qui sont les **bibliothèques externes** utilisées par votre application.

Le fichier `pom.xml` est **partagé** entre tous les développeurs, afin qu'ils puissent
**compilez** et **exécutez** l'application de la même manière.

Le fichier standard `pom.xml` contient les sections suivantes (entre autres) :

- `groupId` : le nom de l'organisation qui a créé le projet. Il définit le **namespace** du projet.
- `artifactId` : le nom du projet.
- `version` : la version du projet.
- `packaging` : le type de packaging du projet.
- `name` et `description` : le nom et la description du projet.
- `dépendances` : les dépendances du projet.

`artifactId`, `version` et `packaging` définissent le **nom du fichier JAR**.

### Cycle de vie Maven

Maven définit un **processus de construction standard** pour les projets Java, appelé **Mavenlifecycle**.

Le cycle de vie Maven est composé de **phases**. Chaque phase est composée d'**objectifs du plugin**.

Par exemple, la phase `compile` est composée de l'objectif de plugin `compiler:compile`
et la phase `package` est composée de l'objectif de plugin `jar:jar` et
de l'objectif de plugin `plugin:addPluginArtifactMetadata`, qui générera un fichier JAR.

Plus de détails sur le cycle de vie de Maven peuvent être trouvés dans le site officiel
Documentation: <https://maven.apache.org/guides/introduction/introduction-to-the-lifecycle.html>.

### Repository Maven

Le [Repository Maven](https://mvnrepository.com/) est un **Repository public** de
**Bibliothèques Java**. Il contient de nombreuses bibliothèques que vous pouvez utiliser dans votre
projets.

Vous pouvez rechercher une bibliothèque et copier la **déclaration de dépendance** dans votre
Fichier `pom.xml`.

Par exemple, la déclaration de dépendance suivante ajoute le
**[Reconnexion](https://mvnrepository.com/artifact/ch.qos.logback/logback-classic/1.4.11)**
bibliothèque à votre projet :

```xml
<!-- https://mvnrepository.com/artifact/ch.qos.logback/logback-classic -->
<dépendance>
    <groupId>ch.qos.logback</groupId>
    <artifactId>logback-classique</artifactId>
    <version>1.4.11</version>
    <scope>tester</scope>
</dépendance>
```

### "Installation" Maven et wrapper Maven

Maven est distribué sous forme d'**archive unique**. Cela signifie que vous devez
extrayez-le et ajoutez-le à votre « PATH » afin qu'il puisse être utilisé n'importe où. Ce n'est pas
très pratique.

La plupart des distributions Linux fournissent un **package** pour Maven, afin que vous puissiez
installez-le en utilisant le gestionnaire de paquets de votre distribution. Ce sera alors
disponible dans votre `PATH` et vous pourrez utiliser la commande `mvn` n'importe où.

La même chose s'applique à macOS avec l'aide de [Homebrew](https://brew.sh/).

Sous Windows, vous devez ajouter Maven à votre « PATH » manuellement. Ce n'est pas très
pratique non plus.

C'est pourquoi certains projets Maven utilisent un **wrapper Maven**. Le wrapper Maven est un
**script** (un script shell sous Linux et macOS et un script Batch sous Windows)
qui téléchargera et exécutera Maven pour vous.

Le wrapper Maven définit la **version de Maven** à utiliser, afin que tous
les développeurs utilisent la même version de Maven.

Le wrapper Maven et son fichier de configuration sont **validés** dans Git mais le
Le fichier exécutable Maven est **ignoré** par Git.

Un nouveau développeur peut alors **exécuter** le wrapper Maven pour **télécharger** et
**exécuter** Maven.

### Résumé

- Maven est un outil de gestion et de compréhension de projets logiciels.
- Maven est un gestionnaire de dépendances pour les projets Java.
- Maven est un outil d'automatisation de build pour les projets Java.
- Maven définit une structure de répertoires standard pour les projets Java.
- Maven définit un processus de build standard pour les projets Java.
- Le fichier `pom.xml` contient la configuration de votre projet Maven.

### Aide-mémoire

```sh
# Download the dependencies
mvn dependency:resolve

# Delete the compiled classes
mvn clean

# Compile the source code
mvn compile

# Package the application
mvn package
```

Plusieurs phases peuvent être exécutées en une seule commande :

```sh
# Delete the compiled classes, compile the source code and package the application
mvn clean compile package
```


