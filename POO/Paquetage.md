# Paquetage

Un paquetage (package) est une bibliothèque de classes.   

Déclaration d’un paquetage (déclarer en début de fichier) :
```Java
package nomPackage;
```
  => Toutes les classes définies dans un fichier appartiennent **au même paquetage**.
- Si aucun paquetage n’est déclaré dans un fichier, les classes correspondantes appartiennent au **paquetage par défaut** (i.e., il contient toutes les classes hors paquetage accessibles).

Les fichiers compilés (**.class**) d’un paquetage doivent être placés dans un répertoire de même nom.
Les paquetages peuvent être organisés de manière hiérarchique :
- **package nom1.nom2.…nomN;**  
  => hiérarchie de répertoires pour les fichiers .class: **nom1/nom2/…/nomN**.

### Utilisation des paquetages
Par défaut, le compilateur recherche la définition des classes utilisées dans le paquetage courant (ou par défaut). Pour les autres, il est nécessaire de préciser dans quel paquetage elles se trouvent.   

Utilisation d’une classe, notation pointée : 
```Java
nomPaquetage.NomClasse
people.Person p = new people.Person("John");
```
   
Importation d’une seule classe : 
```Java
import nomPaquetage.NomClasse;
```

**NomClasse** est ensuite utilisable directement:
```Java
import people.Person;
// …
Person p = new Person("John");
```
   
Importation de toutes les classes d’un paquetage : 
```Java
import nomPaquetage.*;
```

Attention : n’importe pas les sous-paquetages d’un paquetage.
```Java
import people.*;
```

<img src="/POO/images/Paquetage.PNG" width="700"/>

## Accès au paquetages
**CLASSPATH**
- Variable d’environnement permettant de localiser les paquetages à la compilation et à l’exécution.
- Si non initialisée (par défaut), contient le répertoire courant.

Le répertoire racine d’un paquetage utilisateur doit être accessible depuis les répertoires définis dans le **CLASSPATH**.
- Par défaut, si un fichier utilise un paquetage **p** et est compilé ou exécuté depuis un répertoire **r**, **p** doit être un sous-répertoire de **r** (i.e., …/r/p).
- Pour compiler (depuis **r**) une classe C du paquetage **p: javac p/C.java**.
- Pour exécuter (depuis **r**) la méthode main de la classe **C: java p.C**.

Il est toujours possible d’accéder aux classes prédéfinies Java (**java.*, javax.*…**).   

Les classes prédéfinies de **java.lang.*** sont implicitement importées.

<img src="/POO/images/AccesPaquetage.PNG" width="700"/>






