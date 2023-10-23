# Modèle EA

## Généralités
- Le **modèle** EA, parfois appelé modèle ER (de l’anglais entity-relationship) a été proposé par l’américain Chen en 1976.
- Ce modèle permet de formaliser un problème, souvent issu d’un cahier des charges, et est **indépendant de tout choix d’implémentation** (typiquement du choix du SGDB).
- On a donc un **modèle conceptuel** de données (MCD) et non un modèle logique de données (MLD).
- Peut être représenté selon plusieurs normes (**UML**, MERISE,…).

## Exemple d’une bonne structure
- Afin d’éviter les anomalies vues précédemment, il faut:
1. **Représenter séparément** les films et les réalisateurs afin que, par exemple, la suppression d’un film ne risque pas de supprimer son réalisateur
2. **Identifier univoquement** chaque élément (film, réalisateur) afin d’éviter les doublons
3. Lier les films et les réalisateurs **sans redondance** (un réalisateur ne doit être **stocké qu’une seule fois**, même s’il a réalisé plusieurs films)

![]()

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

![image](\typeentite.PNG)

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



