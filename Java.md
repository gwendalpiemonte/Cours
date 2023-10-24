# Java

## Langages compilés et interprétés
- La conception d’un langage de programmation implique des compromis.
- Les **langages compilés** sont traduits en code machine avant l’exécution par le  compilateur. 
   Ils favorisent la performance au détriment des fonctionnalités et de la productivité (ex : C, C++).
- Les **langages interprétés** sont traduits dynamiquement pendant l’exécution par l’interpréteur. 
  - Ils favorisent les fonctionnalités et la productivité au détriment de la performance (ex : JavaScript, Python).
  
<img src="/BDR/images/SQLLanguage.PNG" width="700"/>

- **Langages compilés (+/-)**
  - (Souvent) plus rapide que les langages interprétés.
  - Performances prédictibles.
  - Contrôle fin du hardware.
  - Mémoire difficile à gérer.
  - Programmes difficiles à sécuriser (unsafe).
  - Programmes difficiles à porter
  - Etc.
- **Langages interprétés (+/-)**
  - (Souvent) plus lents que langages compilés.
  - Contrôle limité du hardware.
  - Fonctionnalités avancées (eval).
  - Programmes faciles à porter.
  - Idéal pour le prototypage.
  - Risque d’injection de code.
  - Etc.
 

## Types primitifs
- **Entiers, signés seulement** : byte (8 bits), short (16 bits), int (32 bits), long (64 bits).
- **Réels** : float (32 bits), double (64 bits).
- **Booléen** : boolean (true, false).
- **Caractère** : char (16 bits, Unicode).
- **Attention : String** (chaîne de caractères) est une classe, pas un type primitif.
- **Type par défaut des littéraux** :
  - un littéral entier est du type **int** (p. ex. 3) ;
  - lors d’une affectation dans une variable de type **byte**, **short** ou **char** (et **long**), conversion implicite d’un litteral entier si sa valeur est compatible ;
  - un littéral flottant est du type **double** (p. ex. 3.14 ou 3.14E2).

## Classes enveloppes (wrapper)
- A chaque type primitif correspond sa contrepartie en classe : **Byte, Short, Integer, Long, Float, Double, Boolean et Character**. 
  - Intérêt : utilisable partout ou peut l’être un objet + méthodes spécifiques. Ces types sont fréquemment rencontrés dans des collections d’objets.
  - Une fois l’objet créé, son état n’est plus modifiable (pas de méthode set). 
- Attention : l’opérateur == compare **l’identité des objets référencés**, pas leurs valeurs.
- Java permet la conversion implicite d’une variable de type primitif en sa contrepartie objet (Autoboxing/Unboxing).

## Références
- En Java, tous les objets sont manipulés au moyen de références typées.

<img src="/BDR/images/SQLLanguage.PNG" width="700"/>





  
