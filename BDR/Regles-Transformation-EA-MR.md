# Règles de transformation modèle EA => MR

# Table des matières:
1. [Introduction](#1)
2. [Types d’entité normaux (forts)](#2)
3. [Types d’entité faibles](#3)
4. [Héritages](#4)
5. [Associations binaires `1:1`](#5)
6. [Associations binaires `1:N`](#6)
7. [Associations binaires `N:M`](#7)
8. [Attributs multivalués](#8)
9. [Associations n-aires](#9)
10. [Associations réflexives](#10)


## Introduction <a name="1"></a>
Il y a 9 règles de transformation, elles servent à convertir les:

1. Types d’entité normaux (forts)
2. Types d’entité faibles
3. Héritages
4. Associations binaires `1:1`
    - Obligatoire que d’un côté `(cardinalité 0..1 d’un des deux cotés)`
    - Obligatoire des deux côtés `(cardinalité de 1 des deux côtes)`
    - Facultatif des deux côtés `(cardinalité de 0..1 des deux côtés)`
    - Cas spéciaux (avec un type d’entité spécial)
5. Associations binaires `1:N`
6. Associations binaires `N:M`
7. Attributs multivalués
8. Associations n-aires
9. Associations réflexives
    - Association N:M
    - Association 1:N


## Types d’entités normaux (forts) <a name="2"></a>
Pour chaque type d’entité normal (fort) E:
- **Créer une relation R avec tous les attributs** simples, et les composantes simples des attributs composites, de E
- **La clé primaire de E devient celle de R**

|  Cas simple  | Attribut composite  | Clé de plus d'un attribut  |
|--------------|---------------------|----------------------------|
| <img src="/BDR/images/CasSimple.PNG" width="120"/>  | <img src="/BDR/images/AttributComposite.PNG" width="120"/>| <img src="/BDR/images/PlusUnAttribut.PNG" width="120"/>  |
| R1([k](), a, b)  | R2([k](), c1, c2)| R3([k1, k2](), d)  |


## Types d’entité faibles <a name="3"></a>
Pour chaque type d’entité faible:
- Créer **une relation R contenant tous les attributs de E**
- La **clé est celle du type d’entité fort avec le(s) attribut(s) discriminant(s)**

<img src="/BDR/images/EntiteFaibleTransfo.PNG" width="400"/>

- Type d’entité fort: R1([k1](), a)
- Type d’entité faible: R2([k1, k2](), b)
- Contrainte de clé **étrangère**: R2.k1 référence R1.k1

## Héritages <a name="4"></a>
**Le type d’entité parent** se transforme comme un type d’entité normal (fort)   

**Les types d’entité enfant** deviennent des relations avec leurs attributs et comme **clé primaire celle de leur parent** (qui est donc aussi une **clé étrangère**)

|    | 
|--------------|
| <img src="/BDR/images/Heritage.PNG" width="400"/>  | 
|R1([k](), a)   
R2([k](), b, c)   
R2.k référence R1.k   
R3([k](), b)   
R3.k référence R1.k  |    


Pour rappel, il existe 4 types d’héritage possibles:
- {complete, overlapping}
- {complete, disjoint}
- {incomplete, overlapping}
- {incomplete, disjoint}
   
L’exemple précédent est de type {incomplete, overlapping}, c’est le seul cas qui **n’a pas besoin de contraintes supplémentaires** une fois transformé en relations

Pour les autres cas il faudra utiliser des triggers et/ou des CHECK

## Associations binaires `1:1` <a name="5"></a>

### 0..1 d’un côté
Il faut **identifier** le type d’entité qui a obligatoirement une référence
sur l’autre (**cardinalité minimale de 1**) et:
- En faire une relation qui a comme **clé étrangère** la clé primaire de l’autre type d’entité
- Tous les attributs de l’association sont mis dans cette relation

<img src="/BDR/images/AssoBinaire11.PNG" width="500"/> 

À noter que dans les cas précédents (relations enfant dans un héritage) les clés étrangères étaient aussi UNIQUE et NOT NULL, mais implicitement du fait que c’était aussi des clé primaires

### 1 des 2 côtés
Idem que le cas précédent sauf qu’ici **on doit choisir un des deux types d’entité**

| En choisissant E1   | 
|--------------|
| <img src="/BDR/images/AssoBinaire112.PNG" width="400"/> | 
| R2([k2](), b)
  R1([k1](), k2, a, c)
  R1.k2 référence R2.k2, R1.k2 UNIQUE et NOT NULL | 

### 0..1 des 2 côtés
Identique au cas précédent, il faut **choisir un type d’entité**

| En choisissant E2  | 
|--------------|
| <img src="/BDR/images/AssoBinaire01.PNG" width="400"/> | 
| R1([k1](), b)
  R2([k2](), k1, a, c)
  R2.k1 référence R1.k1, R2.k1 UNIQUE| 

À noter que cette fois la clé étrangère n’est **pas NOT NULL (car la cardinalité minimale est de 0)**

### cas spéciaux
Un type d’entité est considéré comme "spécial" s’il **n’a pas de clé explicite** (enfant d’héritage par exemple)   

Les règles vues précédemment, dans lesquelles il faut choisir un type d’entité, sont valides si les 2 types d’entité sont normaux ou si les 2 sont spéciaux
- Dans le cas des associations 0..1 – 1 il n’y a rien à changer (car aucun choix n’est à effectuer)
- Si un des deux types d’entité est "spécial" alors c’est lui qu’il faut prendre pour y faire migrer les attributs

Dans cet exemple c’est donc **automatiquement E2 qui est choisi** pour la transformation de l’association binaire

| automatiquement E2  | 
|--------------|
| <img src="/BDR/images/AssoSP.PNG" width="350"/> | 
| R1([k1](), a)
  R3([k3](), c)
  R2([k1](), k3, b, d)
  R2.k1 référence R1.k1
  R2.k3 référence R3.k3, R2.k3 UNIQUE et NOT NULL| 

## Associations binaires `1:N` <a name="6"></a>

### Sans attribut
De base, on applique **la même règle que pour les associations 0..1 – 1**

| <img src="/BDR/images/Asso1n1.PNG" width="350"/>  |
|---------------------------------------------------|
| R2([k2](), b) 
  R1([k1](), k2, a)
  R1.k2 référence R2.k2  |

| <img src="/BDR/images/Asso1n2.PNG" width="350"/>|
|-------------------------------------------------|
| R2([k2](), b) 
  R1([k1](), k2, a) 
  R1.k2 référence R2.k2 
  la cardinalité de E1 vaut 1 (schéma de gauche) => R1.k2 NOT NULL|

La différence avec la transformation d’une association 0..1 – 1 est que cette fois la clé étrangère n’est pas UNIQUE car la cardinalité max du type référencé (E2) vaut n (au lieu de 1)

### Avec attribut
**Si l’association a au moins un attribut**, le cas avec une cardinalité de 1 se transforme de manière identique

| <img src="/BDR/images/Asso1nA.PNG" width="350"/>  |
|---------------------------------------------------|
| R2([k2](), b)
  R1([k1](), k2, a, c)
  R1.k2 référence R2.k2, R1.k2 NOT NULL  |
  
|Dans ce cas (cardinalité 0..1)|
| <img src="/BDR/images/Asso1nA2.PNG" width="350"/>|
|-------------------------------------------------|
| R2([k2](), b)
  R1([k1](), a)
  R3([k1](), k2, c)
  R3.k1 référence R1.k1
  R3.k2 référence R2.k2
  R3.k2 NOT NULL|

## Associations binaires `N:M` <a name="7"></a>
## Attributs multivalués <a name="8"></a>
## Associations n-aires <a name="9"></a>
## Associations réflexives <a name="10"></a>
