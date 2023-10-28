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
## Associations binaires `1:1` <a name="5"></a>
## Associations binaires `1:N` <a name="6"></a>
## Associations binaires `N:M` <a name="7"></a>
## Attributs multivalués <a name="8"></a>
## Associations n-aires <a name="9"></a>
## Associations réflexives <a name="10"></a>