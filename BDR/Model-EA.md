# Modèle EA

## Généralités
- Le **modèle** EA, parfois appelé modèle ER (de l’anglais entity-relationship) a été proposé par l’américain Chen en 1976.
- Ce modèle permet de formaliser un problème, souvent issu d’un cahier des charges, et est **indépendant de tout choix d’implémentation** (typiquement du choix du SGDB).
- On a donc un **modèle conceptuel** de données (MCD) et non un modèle logique de données (MLD).
- Peut être représenté selon plusieurs normes (**UML**, MERISE,…).

## Exemple d’une bonne structure
- Afin d’éviter les anomalies vues précédemment, il faut:
  
1. **Représenter séparément** les films et les réalisateurs afin que, par exemple, la suppression d’un film ne risque pas de supprimer son réalisateur

<img src="/BDR/images/Separation.PNG" width="700"/>

2. **Identifier univoquement** chaque élément (film, réalisateur) afin d’éviter les doublons

<img src="/BDR/images/Identifier.PNG" width="700"/>
   
3. Lier les films et les réalisateurs **sans redondance** (un réalisateur ne doit être **stocké qu’une seule fois**, même s’il a réalisé plusieurs films)

<img src="/BDR/images/Lier.PNG" width="700"/>

## Avantages du modèle EA

- Permet assez simplement de **modéliser** des problèmes comme l’exemple précédent mais aussi de beaucoup plus complexes
- Offre une **représentation graphique** basée sur des conventionsprécises mais relativement simples à comprendre pour tous lesintervenants d’un projet (aussi les clients)
- Supporté par beaucoup de logiciels (StarUML, …)
- Certains permettent de générer automatiquement le modèle relationnel voir le code SQL (par exemple PowerDesigner)


## Désavantages du modèle EA

- **Non-déterministe** car il n’y a pas de règles absolues pour déterminer ce qui doit être entité, association ou attribut
- **Ne permet pas toujours de représenter toutes les contraintes sémantiques**, notamment les intégrités (référentielles et sur les données)
- Par exemple qu’une date de naissance ne doit pas être dans le futur
- Il existe plusieurs **conventions** de représentation graphique (notamment UML et MERISE), qui ont parfois des **différences fondamentales** (positionnement des cardinalités)


## Les concepts du modèle EA
- Type d’entité
- Attribut
- Identifiant (ou clé)
- Association
- Cardinalité (ou multiplicité)
- Rôle

## Type d’Entité

Chaque type d’entité a :

<img src="/BDR/images/typeentite.PNG" width="700"/>


Un type d’entité est aussi souvent appelé simplement "entité".

## Attribut

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

## Identifiant (ou clé)
- **Un identifiant est tout sous-ensemble minimal** des attributs d’un type d’entité permettant d’**identifier univoquement chaque tuple**
- Un type d’entité peut avoir plusieurs clés possibles, dans ce cas onparle de **clés candidates**
- Celle retenue sera la **clé primaire** (il ne peut y en avoir qu’une)
- Une bonne clé primaire doit:
  - Être absolument **univoque**
  - Ne, si possible, **pas voir sa valeur être modifiée**
  - Avoir une **taille de stockage la plus petite possible**

## Clé naturelle vs Clé artificielle
- **Clé naturelle**:
  - Ensemble, **minimal et unique**, d’attributs présents naturellement dans le type d’entité (par exemple le no AVS d’une personne)
  - **Avantage**: pas besoin d’ajouter d’attribut
  - **Inconvénient**: est liée à la logique métier donc pourrait être modifiée voir supprimée
    - Peut aussi être composée de beaucoup d’attributs de types peu performants lors des jointures (par exemple plusieurs chaînes de caractères)

- **Clé artificielle**:
  - **Attribut que l’on rajoute** et qui n’est pas lié sémantiquement aux attributs naturels
  - **Avantage**: n’est pas lié à la logique métier, donc on a le choix du type et ses valeurs ne seront pas modifiées
  - **Inconvénient**: ne fournit aucune information métier

## Choix de la clé primaire
- L’identification des clés et le choix de la clé primaire est crucial mais pas toujours facile:
  - titre de film est-il bien unique ?
  - Idem pour las attributs naturels de Réalisateur, composeraient-ils une bonne clé primaire ?
    
<img src="/BDR/images/ClefPrimaire.PNG" width="700"/>

## Type d’entité: Représentation graphique

<img src="/BDR/images/TypeentiteRepGraph.PNG" width="700"/>

## Association
- Le concept d’association est lié à celui de **relation dans la théorie des ensembles**

  <img src="/BDR/images/Association.PNG" width="700"/>

- On a les n-uplets: {(R1, F1), (R1, F2), (R2, F3)}

- En reprenant l’exemple du début du chapitre, on a :
  - Un réalisateur peut réaliser plusieurs films
  - Un film est réalisé par un seul réalisateur

## Degré d’une association
- Le degré d’une association se dit aussi **arité**
- Il est déterminé par le nombre de types d’entité (TE) impliqués dans l’association
  - S’il y en a 2, comme entre Film et Réalisateur, l’association est **binaire**
  - S’il y en a 3, l’association est **ternaire**
  - S’il y a n TE, l’association est **n-aire**
    
<img src="/BDR/images/DegreAssociation.PNG" width="700"/>

- **Un nom est donné à chaque association**, ici "a réalisé"

## Cardinalités

Cardinalités
- La cardinalité (ou multiplicité) est une paire [min, max] de valeurs telle que:
  - min (cardinalité minimale) est le nombre minimal de fois qu’un TE peut intervenir dans l’association
    - Vaut en général 0 ou 1
  - max (cardinalité maximale) est le nombre maximal de fois qu’un TE peut intervenir dans l’association
    - Vaut en général 1 ou * (un nombre quelconque de fois)

<img src="/BDR/images/Cardinalite.PNG" width="700"/>
  
    - Si min = max, on peut ne mettre la valeur qu’une seule fois



