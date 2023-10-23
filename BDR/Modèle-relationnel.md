# Modèle relationnel

# Table des matières:
1. [Introduction](#1)
2. [Concepts de base](#2)
3. [Notion de relation](#3)
4. [Arité](#4)
5. [Mathématique/BDD](#5)
6. [Définition](#6)


## Introduction <a name="1"></a>
- Le **modèle relationnel (MR)** est le modèle logique de données (MLD) utilisé par la majorité des SGBDR (SGBD relationnels)
- Il trouve ses origines dans les travaux de Codd (1970)
- Il se base sur la notion mathématique de **relation** et fait appel à la **théorie des ensembles**
- C’est l’étape suivant le modèle EA, des règles permettent de transformer un modèle EA en MR
- Il est associé à l’algèbre relationnelle pour manipuler les données
  - SQL est une "implémentation" informatique de l’algèbre relationnelle

## Concepts de base <a name="2"></a>
- Chaque **relation** correspond à une **table** dans une base de données
- Une relation a des **attributs**, chacun représente (le nom d’) une **colonne** de la table
- Chaque attribut a un **domaine** duquel seront issues toutes ses valeurs
- Une relation a un ensemble de **tuples**, qui correspondent aux **lignes** de la table
- **L’état (ou la population) d’une relation R**, noté r(R), est l’ensemble des tuples de cette relation à un temps t

## Notion de relation <a name="3"></a>
- En base de données R(X, Y):
  - R est le nom de la relation
  - X est un attribut qui prend sa valeur dans E
  - Y est un attribut qui prend sa valeur dans F

- {Nom, Prénom} est le schéma de la relation R plus généralement, un schéma S d’une relation R est un ensemble fini d’attributs {A1, A2, …, An } tel que Ai prend ses valeurs dans le domaine Di
- Une relation R de schéma S (nommée R(S)) est un sous-ensemble du produit cartésien D1 x D2 x … x Dn
- Donc R = {(a1, a2, …, an)} est appelé un tuple (ou n-uplet)
- Représentation en extension d’un état de R(Nom, Prénom): {(Sorogoyen, Rodrigo), (Scott, Ridley)}
- Représentation tabulaire (utilisée en base de données):

<img src="/BDR/images/NotionRelation.PNG" width="700"/>

## Arité <a name="4"></a>
- L’arité est le **nombre d’attributs** d’une relation
- Par exemple:
R(Nom, Prénom)
  - A une arité de 2
  - C’est une relation binaire

## Mathématique/BDD <a name="5"></a>
- La définition **mathématique** d’une **relation** comme un ensemble de tuples a notamment les propriétés suivantes:
  1. L’ordre des tuples est sans importance, idem pour les attributs
  2. Il ne peut pas y avoir de doublons de tuples
  3. Dans chaque tuple, la valeur de tous les attributs est connue

- En **base de données**:
  1. Idem qu’en mathématique
  2. Idem qu’en mathématique mais uniquement si une clé primaire a été définie
  3. Dépend des données, vrai que s’il n’y a pas de NULL Exemple ne respectant pas cette propriété mathématique:

<img src="/BDR/images/NotionRelationTab.PNG" width="700"/>

## Définition <a name="6"></a>
- Toute **clé** d’une relation R (de schéma S) est définie comme un sous-ensemble K de S tel qu’il n’existe pas 2 tuples ayant la même valeur pour tous les attributs de K
- Une **base de données** est un ensemble fini de relations
- Un schéma d’une base de données (ou **schéma relationnel**) est l’ensemble des schémas des relations de cette base
