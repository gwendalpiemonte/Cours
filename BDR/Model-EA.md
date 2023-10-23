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
<img src="/BDR/images/CardinaliteMin.PNG" width="700"/>

- La **cardinalité minimale de Film indique** qu’un film:
  - Ne peut pas exister sans son réalisateur
  - Doit apparaître dans au moins un couple (réalisateur, film)
    
- La **cardinalité minimale de Réalisateur indique** qu’un réalisateur:
  - Peut n’avoir réalisé aucun film
  - N’apparaît pas forcément dans un couple (réalisateur, film)

## Cardinalités maximales: conséquences <a name="15"></a>
<img src="/BDR/images/CardinaliteMax.PNG" width="700"/>

- La **cardinalité maximale de Film indique** qu’un film:
  - Ne peut pas avoir plus d’un réalisateur
  - Apparaît dans au plus un couple (réalisateur, film)

- La **cardinalité maximale de Réalisateur indique** qu’un réalisateur:
  - Peut avoir réalisé plusieurs films
  - Peut apparaitre dans plusieurs couples (réalisateur, film)

## Cardinalités: conventions UML <a name="16"></a>
- 1 un et un seul (équivalent à 1..1)
- 0..1 zéro ou un
- N exactement N (N connu)
- M..N entre M et N (M et N connus)
- 0..* entre zéro et N (N indéfini)
- (*) équivalent à 0..*
- 1..* de un à N (N indéfini)

## Associations: noms courants <a name="17"></a>
- Association un-un (1:1)
- Association un à plusieurs (1:n)
- Association plusieurs à plusieurs (n:m)

<img src="/BDR/images/AssociationPlusieurs.PNG" width="700"/>

## Associations multiples <a name="18"></a>
- On peut avoir **plus d’une association entre 2 types d’entité**
- Par exemple pour modéliser des films ainsi que leurs acteurs et leurréalisateur:

<img src="/BDR/images/AssociationMultiple.PNG" width="700"/>

- Regroupement de Réalisateur et Acteur dans Artiste
- acteur et réalisateur sont les **rôles**

## Attributs d’association <a name="19"></a>
- S’il faut, dans l’exemple suivant, ajouter l’information du cachet (salaire) que touche un acteur par film:

<img src="/BDR/images/AttributAssociation.PNG" width="700"/>

- Un attribut **cachet doit être ajouté**, mais où ?

- Si l’attribut cachet est ajouté dans Acteur, il n’est **pas possible de changer le cachet d’un acteur d’un film à l’autre**:

<img src="/BDR/images/AttributAssociation2.PNG" width="700"/>

- Si l’attribut cachet est ajouté dans Film, cela signifie que **tous les acteurs d’un même film ont le même cachet**:

<img src="/BDR/images/AttributAssociation3.PNG" width="700"/>

- Il faut donc **mettre l’attribut sur l’association**:

<img src="/BDR/images/AttributAssociation4.PNG" width="700"/>
  
- Ainsi le cachet dépend de **l’acteur et du film**

## Clé d’une association <a name="20"></a>
- Comme les types d’entité, les **associations plusieurs à plusieurs (n:m)** donneront une **table** dans la base de données
  - Elles ont donc aussi **besoin d’une clé primaire**
  - Cette clé est **composée des clés primaires** de tous les types d’entités auxquelles cette association est liée
  - L’exemple suivant permet de stocker quel client séjourne dans quel hôtel (avec la date d’arrivée et la durée du séjour):

<img src="/BDR/images/ClefAssociation.PNG" width="700"/>

  - Dans cet exemple, la clé de "séjourne" sera (idClient, nomHôtel)

- Avoir (idClient, nomHôtel) comme clé primaire **ne permet pas de saisir plus d’un séjour** pour un client donné dans un hôtel donné
- Dans ce cas, il faut **ajouter dateArrivée à la clé primaire** de « séjourne » qui devient (idClient, nomHôtel, dateArrivée)

<img src="/BDR/images/ClefAssociation2.PNG" width="700"/>
 
## Type d’entité faible <a name="21"></a>
- Parfois un type d’entité ne peut pas exister seul, il est **lié à un autre type d’entité**
  - Dans ce cas on parle de **type d’entité faible, lié à un type d’entité fort**
    
-  L’exemple suivant modélise des cinémas et leurs salles:

<img src="/BDR/images/EntiteFaible.PNG" width="700"/>
  
- Un des problèmes est qu’il a fallu créer une clé artificielle dans Salle alors que son numéro + l’id du cinéma serait plus logique

- À la place de chercher à avoir un **identifiant absolu** à Salle, une meilleure solution consiste à recourir à une **identification relative** (à Cinéma)
  - C’est le concept d’entité faible
 
<img src="/BDR/images/EntiteFaible2.PNG" width="700"/>
    
- La clé primaire de Salle est **composée** des attributs id (de Cinéma) et numéro
- En UML cela est modélisé par une **association qualifiée**
- numéro est devenu une clé partielle (**discriminateur en UML**), placée du côté de l’entité forte
  
- Un type d’entité faible (ici Salle) a de **fortes contraintes**:
  - Lors de l’insertion d’une salle il faut **déjà avoir inséré** son cinéma
  - Si la clé primaire du cinéma est modifiée, il faut **répercuter cette modification** sur toutes ses salles
  - Si on supprime un cinéma il faut **aussi supprimer** toutes ses salle
    
- Ce ne sont pas forcément des inconvénients, c’est même souvent un des buts d’une telle modélisation
  - Les SGBD offrent des mécanismes pour gérer certains cas comme la mise à jour en cascade


## Types d’associations (en UML) <a name="22"></a>
- **Association**
  - Ce qui sera principalement utilisé en modélisation EA
- **Agrégation**
  - Modélise une relation de type conteneur-contenu mais sans destruction du contenu si le conteneur est détruit
  - N’est pas structurellement différente d’une association simple
  - En modélisation EA cela peut donc être représenté soit par une agrégation, soit par une association
- **Composition**
 - Est identique à l’agréation mais avec suppression automatique des contenus
  - Cela correspond à un lien fort-faible, dans l’exemple précédent supprimer un cinéma implique d’en supprimer ses salles

## Associations redondantes <a name="23"></a>
- Exemple: à partir d’un cahier des charges qui demande de modéliser:
  - Des personnes et la ville dans laquelle elles habitent
  - Le pays dans lequel chaque ville se situe
  - Le pays dans lequel chaque personne habit
    
- Le schéma EA suivant modélise ces points, mais est-il idéal ?

<img src="/BDR/images/AssociationRedondante.PNG" width="700"/>

- Le pays dans lequel une personne habite peut s’obtenir:
  - Par l’association "lieu habitation"
  - Par les associations "habite dans" puis "située dans
    
- Il y a donc **redondance** car dans ces 2 cas la **sémantique** est la même (lieu d’habitation) et les cardinalités aussi (on obtient à chaque fois un et un seul pays)
- L’association "lieu habitation" **est à supprimer** car elle n’offre aucune information supplémentaire

## Association réflexive <a name="24"></a>
- **Association entre un type d’entité et lui-même**, est aussi parfois appelée association **récursive**
- Par exemple, pour représenter le lien de parenté biologique:

<img src="/BDR/images/AssociationReflexive.PNG" width="700"/>

- Dans ce schéma il faut nommer les rôles de l’association (parent, enfant) afin de lever toute ambiguïté
  - Le type d’entité Personne est lié à 2 parents et à zéro à N (indéfini) enfants

## Associations n-aires <a name="25"></a>
- Se dit des associations lorsque n > 2
- Généralisation des associations binaires
- Étant plus complexes à comprendre et mettre en place, elles sont plus rarement utilisées
- Même s’il n’y a pas de limites au degré d’une association, **il est rare d’avoir un degré supérieur à 3**

- Pour modéliser l’horaire d’un cours, en y associant la salle et la période horaire, le schéma suivant utilise une association **ternaire**:

<img src="/BDR/images/AssociationNAire.PNG" width="700"/>

- En notation UML, une association n-aire est représentée par un losange
- Les cardinalités n’ont pas besoin d’être notées car une association n-aire est **implicitement de type plusieurs à plusieurs (n:m)**

- Les associations n-aires ne sont **pas toujours évidentes à comprendre**
- Étant de type plusieurs à plusieurs, **cette association donnera une table** dans la base de données avec **la clé primaire** (nomUnité, heureDébutPériode, nomSalle)
- Est-ce que cela correspond à ce qui est voulu/attendu ?

- Cette clé va permettre d’insérer, par exemple:

<img src="/BDR/images/TabAssiciationNAire.PNG" width="700"/>

- Ce n’est **pas ce qui est voulu** (2 cours en même temps dans la même salle)
- Ce problème pourra être résolu plus tard, dans le modèle logique ou physique (en utilisant une clause UNIQUE, un trigger,…)
- Un autre problème est que des associations de degré élevé génèrent des **clés volumineuses** (peu performantes)

- Les associations n-aires posent souvent (mais pas toujours) problème au niveau du modèle E
  
- Elles peuvent **toujours être remplacées** par un type d’entité selon les étapes suivantes:
  
  1. **Transformer l’association en type d’entité** et lui attribuer une clé artificielle. Cela évite donc aussi le problème de la clé primaire trop volumineuse
  2. **Créer une association de type un à plusieurs depuis ce type d’entité vers chacun des types d’entités** qui étaient reliés par l’association. Le côté plusieurs est à mettre du côté du type d’entité qui remplace l’association

## Contraintes d’intégrité (CI) sémantiques <a name="26"></a>
- Parfois les concepts de base du modèle EA ne permettent pas de représenter toutes les contraintes voulues
- Par exemple dire qu’une date de naissance ne peut pas être dans le futur
- Dans ces cas **il faut ajouter, sous formes de commentaires, ces contraintes au schéma**
  - Elles sont indispensables car **sans elles le schéma n’est pas complet**
  
- Elles seront implémentées plus tard, durant la mise en place de la base de données, notamment en fonction des possibilités offertes par le SGBD (triggers, …)

<img src="/BDR/images/CI.PNG" width="700"/>

- Dans ce schéma on peut par exemple définir la CI: "Il ne peut pas y avoir 2 unités (cours) dans la même salle pendant la même période"
  - Et d’autres cas issus de contraintes métier, comme par exemple une plage d’heures de début possibles (pour Période)

## Héritage / spécialisation <a name="27"></a>
- Exemple: il faut stocker des gymnastes et des juges pour des compétitions de gymnastique
  
- Si les 2 n’ont qu’un nom et un prénom, cela peut être modélisé par **un seul type d’entité**:
  
  <img src="/BDR/images/HeritageSP.PNG" width="700"/>
  
- type stocke une valeur gymnaste/juge (typiquement dans un enum)
  - Cet attribut sert de **discriminant**

- S’il faut ajouter les informations suivantes:
  - La date de naissance des gymnastes
  - La date d’obtention du certificat de juge pour les juges
- Une possibilité est d’ajouter 2 attributs à Personne:

<img src="/BDR/images/HeritageSP2.PNG" width="700"/>
  
- Cela génère plusieurs **problèmes**:
  - On aura beaucoup de **valeurs inutiles** (NULL en fonction de type)
  - Il faut **maintenir la cohérence des attributs** (type et dateNaissance/dateCertificat)

- Une autre possibilité est de faire **deux types d’entité**:

<img src="/BDR/images/HeritageSP3.PNG" width="700"/>
  
- Cela **corrige les problèmes de cohérence** précédents
- Mais on **perd la factorisation** des attributs communs
  - Cela est particulièrement problématique si une personne peut être gymnaste et juge

- Afin de retrouver la factorisation, il faut remettre le type d’entité Personne et avoir une modélisation à **3 types d’entité**

<img src="/BDR/images/HeritageSP4.PNG" width="700"/>

- Cette modélisation est **plus évolutive** mais elle:
  - **Ajoute une clé artificielle** à Gymnaste et à Juge
  - Ne permet plus de retrouver l’information complète pour un gymnaste/juge dans un seul type d’entité (il faudra faire une jointure dans les requêtes)

- Les **associations "est un" de type (1:1) traduisent un lien d’héritage**
- La modélisation précédente donne l’héritage suivant:

<img src="/BDR/images/HeritageSP5.PNG" width="700"/>

- Cette modélisation est **équivalente à la précédente** mais est plus claire et **évite les clés artificielles inutiles**
  - La clé primaire sera aussi "héritée" (par exemple Gymnaste aura comme clé primaire celle de Personne)
- Le processus de factorisation par héritage (dans un type d’entité parent) est aussi appelé **généralisation**

## Héritage multiple <a name="28"></a>
- Dans de très rares cas un type d’entité peut avoir plusieurs parents, l’héritage est dit **multiple** (autrement il est **simple**)
- Exemple précédent avec héritage multiple:

<img src="/BDR/images/HeritageMultiple.PNG" width="700"/>

## Héritage / spécialisation: Types <a name="29"></a>
- Il y a 2 types de contraintes, **la totale et la disjointe**
- La contrainte **totale** peut avoir la valeur:
  - **total/complete**: Tout tuple du type d’entité parent doit être dans au moins un des types d’entité enfant
  - **partiel/incomplete**: Aucune contrainte entre type d’entité parent et enfants

- La contrainte **disjointe** peut avoir la valeur:
  - **chevauchant/overlapping**: Un tuple peut être dans plusieurs types d’entité enfant 
  - **disjoint/disjoint**: Un tuple peut être au maximum dans un type d’entité enfant
  
- Les 4 possibilités sont donc:
  - **{complete, overlapping}**
  - **{complete, disjoint}**
  - **{incomplete, overlapping}**
  - **{incomplete, disjoint}**

<img src="/BDR/images/HeritageSpecialisationType.PNG" width="700"/>

- Il **faut toujours indiquer le type** de chaque héritage sur le schéma, ici il signifie qu’on:
  - Ne veut pas de personne qui ne soit ni gymnaste ni juge (**complete**)
  - Permet à une personne d’être à la fois juge et gymnaste en même temps (**overlapping**)

