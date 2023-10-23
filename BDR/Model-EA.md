# Modèle EA

# Table des matières:
1. [Généralités](#1)
2. [Exemple d’une bonne structure](#2)
3. [Avantages du modèle EA](#3)
4. [Désavantages du modèle EA](#4)
5. [Les concepts du modèle EA](#5)
6. [Type d’Entité](#6)
7. [Attribut](#7)
8. [Identifiant (ou clé)](#8)
9. [Clé naturelle vs Clé artificielle](#9)
10. [Choix de la clé primaire](#10)
11. [Type d’entité: Représentation graphique](#11)
12. [Degré d’une association](#12)
13. [Cardinalités](#13)
14. [Cardinalités minimales: conséquences](#14)
15. [Cardinalités maximales: conséquences](#15)
16. [Cardinalités: conventions UML](#16)
17. [Associations: noms courants](#17)
18. [Associations multiples](#18)
19. [Attributs d’association](#19)
20. [Clé d’une association](#20)
21. [Type d’entité faible](#21)
22. [Types d’associations (en UML)](#22)
23. [Associations redondantes](#23)
24. [Association réflexive](#24)
25. [Associations n-aires](#25)
26. [Contraintes d’intégrité (CI) sémantiques](#26)
27. [Héritage / spécialisation](#27)
28. [Héritage multiple](#28)
29. [Héritage / spécialisation: Types](#29)

## Généralités <a name="1"></a>
- Le **modèle** EA, parfois appelé modèle ER (de l’anglais entity-relationship) a été proposé par l’américain Chen en 1976.
- Ce modèle permet de formaliser un problème, souvent issu d’un cahier des charges, et est **indépendant de tout choix d’implémentation** (typiquement du choix du SGDB).
- On a donc un **modèle conceptuel** de données (MCD) et non un modèle logique de données (MLD).
- Peut être représenté selon plusieurs normes (**UML**, MERISE,…).

## Exemple d’une bonne structure <a name="2"></a>
- Afin d’éviter les anomalies vues précédemment, il faut:
  
1. **Représenter séparément** les films et les réalisateurs afin que, par exemple, la suppression d’un film ne risque pas de supprimer son réalisateur

<img src="/BDR/images/Separation.PNG" width="700"/>

2. **Identifier univoquement** chaque élément (film, réalisateur) afin d’éviter les doublons

<img src="/BDR/images/Identifier.PNG" width="700"/>
   
3. Lier les films et les réalisateurs **sans redondance** (un réalisateur ne doit être **stocké qu’une seule fois**, même s’il a réalisé plusieurs films)

<img src="/BDR/images/Lier.PNG" width="700"/>

## Avantages du modèle EA <a name="3"></a>

- Permet assez simplement de **modéliser** des problèmes comme l’exemple précédent mais aussi de beaucoup plus complexes
- Offre une **représentation graphique** basée sur des conventionsprécises mais relativement simples à comprendre pour tous lesintervenants d’un projet (aussi les clients)
- Supporté par beaucoup de logiciels (StarUML, …)
- Certains permettent de générer automatiquement le modèle relationnel voir le code SQL (par exemple PowerDesigner)


## Désavantages du modèle EA <a name="4"></a>

- **Non-déterministe** car il n’y a pas de règles absolues pour déterminer ce qui doit être entité, association ou attribut
- **Ne permet pas toujours de représenter toutes les contraintes sémantiques**, notamment les intégrités (référentielles et sur les données)
- Par exemple qu’une date de naissance ne doit pas être dans le futur
- Il existe plusieurs **conventions** de représentation graphique (notamment UML et MERISE), qui ont parfois des **différences fondamentales** (positionnement des cardinalités)


## Les concepts du modèle EA <a name="5"></a>
- Type d’entité
- Attribut
- Identifiant (ou clé)
- Association
- Cardinalité (ou multiplicité)
- Rôle

## Type d’Entité <a name="6"></a>

Chaque type d’entité a :

<img src="/BDR/images/typeentite.PNG" width="700"/>


Un type d’entité est aussi souvent appelé simplement "entité".

## Attribut <a name="7"></a>

- Un **attribut** est une propriété caractéristique d’un type d’entité
  - Par exemple le titre d’un film
- Correspond à une **variable/attribut** en programmation
  - Est désigné par un **identificateur** (nom)
  - Est d’un certain **type** (domaine énumérable tel que les entiers, lesdates,…)
- En règle générale lors de la phase de conception on ne fixe pas encore les types:
  - Ils vont parfois dépendre du SGBD et peuvent avoir plus de contraintes (par ex min/max) que le type de base
  - L’exception est souvent le type énuméré (enum) car on veut (et peut) la plupart du temps en connaître toutes les valeurs dès la phase de conception
- **Monovalué** si l’attribut ne peut prendre qu’une seule valeur, par exemple une date de naissance
  - S’il est **multivalué**, l’attribut peut prendre plusieurs valeurs (par exemple pour gérer des personnes pouvant avoir plusieurs prénoms)
- **Simple (ou atomique)** si l’attribut ne peut pas être décomposé en plusieurs autres attributs
  - Sinon il est **composite**, par exemple une adresse (que l’on peut décomposer en ville + NPA + rue +…)
- **Obligatoire** s’il doit forcément avoir une valeur pour chaque tuple
  - Sinon il est **facultatif** (souvent dit nullable car peut prendre la valeur **NULL**)
- **Stocké**
  - La valeur est simplement stockée dans la base de données
- **Dérivé/calculé**
  - Par exemple l’âge d’une personne peut être calculé à partir de sa date de naissance (qui est un attribut stocké) et de la date courante
  - Autre exemple: si la base de données stocke des entreprises et leurs listes d’employés, le nombre d’employés par entreprise peut être déduit
- Sauf en cas d’obligation, par exemple pour des questions de performance, **on ne stocke jamais d’attributs qui peuvent être dérivés/calculés**
  - C’est une forme de redondance et est **très compliqué et couteux** à maintenir à jour (cohérent)

## Identifiant (ou clé) <a name="8"></a>
- **Un identifiant est tout sous-ensemble minimal** des attributs d’un type d’entité permettant d’**identifier univoquement chaque tuple**
- Un type d’entité peut avoir plusieurs clés possibles, dans ce cas onparle de **clés candidates**
- Celle retenue sera la **clé primaire** (il ne peut y en avoir qu’une)
- Une bonne clé primaire doit:
  - Être absolument **univoque**
  - Ne, si possible, **pas voir sa valeur être modifiée**
  - Avoir une **taille de stockage la plus petite possible**

## Clé naturelle vs Clé artificielle <a name="9"></a>
- **Clé naturelle**:
  - Ensemble, **minimal et unique**, d’attributs présents naturellement dans le type d’entité (par exemple le no AVS d’une personne)
  - **Avantage**: pas besoin d’ajouter d’attribut
  - **Inconvénient**: est liée à la logique métier donc pourrait être modifiée voir supprimée
    - Peut aussi être composée de beaucoup d’attributs de types peu performants lors des jointures (par exemple plusieurs chaînes de caractères)

- **Clé artificielle**:
  - **Attribut que l’on rajoute** et qui n’est pas lié sémantiquement aux attributs naturels
  - **Avantage**: n’est pas lié à la logique métier, donc on a le choix du type et ses valeurs ne seront pas modifiées
  - **Inconvénient**: ne fournit aucune information métier

## Choix de la clé primaire <a name="10"></a>
- L’identification des clés et le choix de la clé primaire est crucial mais pas toujours facile:
  - titre de film est-il bien unique ?
  - Idem pour las attributs naturels de Réalisateur, composeraient-ils une bonne clé primaire ?
    
<img src="/BDR/images/CelfPrimaire.PNG" width="700"/>
 
## Type d’entité: Représentation graphique <a name="generalite"></a>

<img src="/BDR/images/TypeentiteRepGraph.PNG" width="700"/>

## Association <a name="11"></a>
- Le concept d’association est lié à celui de **relation dans la théorie des ensembles**

<img src="/BDR/images/Association.PNG" width="700"/>

- On a les n-uplets: {(R1, F1), (R1, F2), (R2, F3)}

- En reprenant l’exemple du début du chapitre, on a :
  - Un réalisateur peut réaliser plusieurs films
  - Un film est réalisé par un seul réalisateur

## Degré d’une association <a name="12"></a>
- Le degré d’une association se dit aussi **arité**
- Il est déterminé par le nombre de types d’entité (TE) impliqués dans l’association
  - S’il y en a 2, comme entre Film et Réalisateur, l’association est **binaire**
  - S’il y en a 3, l’association est **ternaire**
  - S’il y a n TE, l’association est **n-aire**
    
<img src="/BDR/images/DegreAssiciation.PNG" width="700"/>

- **Un nom est donné à chaque association**, ici "a réalisé"

## Cardinalités <a name="13"></a>

Cardinalités
- La cardinalité (ou multiplicité) est une paire [min, max] de valeurs telle que:
  - min (cardinalité minimale) est le nombre minimal de fois qu’un TE peut intervenir dans l’association
    - Vaut en général 0 ou 1
  - max (cardinalité maximale) est le nombre maximal de fois qu’un TE peut intervenir dans l’association
    - Vaut en général 1 ou * (un nombre quelconque de fois)

<img src="/BDR/images/Cardinalite.PNG" width="700"/>
    - Si min = max, on peut ne mettre la valeur qu’une seule fois

## Cardinalités minimales: conséquences <a name="14"></a>

## Cardinalités maximales: conséquences <a name="15"></a>

## Cardinalités: conventions UML <a name="16"></a>

## Associations: noms courants <a name="17"></a>

## Associations multiples <a name="18"></a>

## Attributs d’association <a name="19"></a>

## Clé d’une association <a name="20"></a>
 
## Type d’entité faible <a name="21"></a>

## Types d’associations (en UML) <a name="22"></a>

## Associations redondantes <a name="23"></a>

## Association réflexive <a name="24"></a>

## Associations n-aires <a name="25"></a>

## Contraintes d’intégrité (CI) sémantiques <a name="26"></a>

## Héritage / spécialisation <a name="27"></a>

## Héritage multiple <a name="28"></a>

## Héritage / spécialisation: Types <a name="29"></a>

<img src="/BDR/images/HeritageSpecialisationType.PNG" width="700"/>

- Il **faut toujours indiquer le type** de chaque héritage sur le schéma, ici il signifie qu’on:
  - Ne veut pas de personne qui ne soit ni gymnaste ni juge (**complete**)
  - Permet à une personne d’être à la fois juge et gymnaste en même temps (**overlapping**)

