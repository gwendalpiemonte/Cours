# Algèbre relationnelle

## Introduction
L’algèbre relationnelle a été initialement proposée par Codd (1970)

C’est un langage formel qui fait intervenir 5 opérateurs

Chaque opérateur prend en entrée une (opérateur unaire) ou deux
(opérateur binaire) relation(s) et produit une relation en sortie

Il est donc possible de composer des opérateurs (en appliquer plusieurs à
la suite en utilisant la relation de sortie en entrée de l’opérateur suivant)

**Le but est d’exprimer des requêtes afin d’extraire de l’information d’un ensemble de relations**

Le SQL (Structured Query Language) est une implémentation
informatique de l’algèbre relationnelle

## Les opérateurs
En algèbre relationnelle il y a 5 opérateurs fondamentaux:
|Operateur|symbole|Type|
|-----|-----|-----|
|La sélection|$\sigma$|opérateur unaire |
|La projection|$\pi$|opérateur unaire|
|Le produit cartésien|$\times$|opérateur binaire|
|L’union |$\cup$|opérateur binaire|
|La différence|$\-$|opérateur binaire|

À partir de ces opérateurs **il est possible d’en définir d’autres par composition**:

Le plus connu et utilisé est **la jointure** ($\otimes$) qui est la composition d’un produit cartésien et d’une sélection

Les autres sont la **division** ($\/$) et **l’intersection** ($\cap$)

Certaines requêtes, selon les noms des attributs impliqués, peuvent nécessiter l'utilisation de l’opérateur de **renommage** ($\rho$)

## La sélection
La sélection s’écrit: $\sigma$ c(r)
- r est la relation de schéma R sur laquelle s’applique la sélection
- c est le critère de sélection qui est une expression booléenne
- Le résultat est une relation r' qui contiendra tous les tuples de r qui satisfont au critère c

Le critère c peut comparer un attribut A $\in$ R avec:
- Une constante a A $\theta$ a où $\theta$ $\in$ {=, $\not=$, >, <, $\ge$, $\le$}
- Un autre attribut de r A1 $\theta$ A2 où $\theta$ $\in$ {=, $\not=$, >, <, $\ge$, $\le$}

Exemple: Liste des réalisateurs nés après 1970   

**$\sigma$ annéeNaissance > 1970(Réalisateur)**

<img src="/BDR/images/exemple1.PNG" width="400"/>

## La projection
La projection s’écrit: $\pi$ s(r)
- r est la relation de schéma R, projetée sur S $\subseteq$ R
- Le résultat est une relation r' de schéma S qui ne conserve que les attributs de S
- Les éventuels doublons sont supprimés

Exemple: Liste du nom, du prénom et de l’année de naissance des réalisateurs   

**$\pi$ nom, prénom, annéeNaissance (Réalisateur)**

<img src="/BDR/images/exemple2.PNG" width="400"/>

### Doublons
Exemple: Liste des noms des réalisateurs   

**$\pi$ nom (Réalisateur)**

<img src="/BDR/images/exemple3.PNG" width="400"/>

Cette fois le résultat ne contient que **6 réalisateurs (au lieu de 7)**
- Il y a deux tuples qui ont ‘Scott’ comme nom dans Réalisateur
- Le résultat étant une relation, **il ne peut pas y avoir deux lignes identiques**
- Il faut donc **faire attention lorsque les attributs de la projection ne contiennent pas une clé de la relation initiale**
- Le but d’une projection est, la plupart du temps, de retourner le même nombre de lignes que la relation initiale

### Requêtes avec plusieurs opérateurs
Par combinaison des 2 requêtes précédentes, on peut obtenir le
nom, prénom et l’année de naissance des réalisateurs nés après 1970:   

**$\pi$ nom, prénom, annéeNaissance ($\sigma$ annéeNaissance > 1970 (Réalisateur))**

<img src="/BDR/images/exemple4.PNG" width="400"/>

Les requêtes n’utilisant qu’un opérateur sont rares

## L’union
L’union s’écrit: r $\cup$ s
- Le résultat est une relation r' qui contiendra tous les tuples qui sont dans r et tous ceux qui sont dans s
- Les éventuels **doublons sont supprimés**
- r et s doivent être de **même schéma** (leurs attributs doivent être identiques en nombre, de même nom et de même type)

Exemple: Liste des réalisateurs nés après 1970 ou avant 1950   

**$\sigma$ annéeNaissance > 1970 (Réalisateur) $\cup$ $\sigma$ annéeNaissance < 1950 (Réalisateur)**   

<img src="/BDR/images/exemple5.PNG" width="400"/>

## La différence
La différence s’écrit: r $\-$ s
- Le résultat est une relation r' qui contiendra tous les tuples de r qui n’existent pas dans s
- Les éventuels **doublons sont supprimés**
- r et s doivent être de **même schéma**

La différence sert souvent à résoudre les requêtes voulant des résultats **ne vérifiant pas** une propriété

Exemple: Liste des réalisateurs en excluant ceux nés en 1943   

**Réalisateur $\-$ $\sigma$ annéeNaissance = 1943 (Réalisateur)**   

<img src="/BDR/images/exemple6.PNG" width="400"/>

## L’intersection
L’intersection s’écrit: r $\cap$ s
- Le résultat est une relation r' qui contiendra tous les tuples communs à r et s
- Les éventuels doublons sont supprimés
- r et s doivent être de même schéma

Exemple: les films qui datent d’après 2001 et durent moins de 2h   

**$\sigma$ année > 2001 (Film) $\cap$ durée < 120 (Film)**   

<img src="/BDR/images/exemple7.PNG" width="400"/>

L’intersection a l’équivalence suivante:
**r $\-$ (r $\-$ s)**
**$\sigma$ année > 2001 (Film) – ($\sigma$ année > 2001 (Film) $\-$ durée < 120 (Film))**

## Opérateur de renommage
L’opérateur de renommage s’écrit: **$\rho$ A $\to$ B (r) ou $\rho$ B/A (r)**
- Le résultat est une relation r' qui contiendra tous les tuples de r mais en **renommant l’attribut A en B**
- Il existe une notation abrégée qui permet de ne spécifier que les nouveaux noms des attributs: $\rho$ (B, C,..) (r) Cette notation impose de renommer **tous les attributs** de r

Cet opérateur est souvent nécessaire pour les opérateurs binaires nécessitant **2 relations de même schéma**

Exemple: Liste des cinémas (id) n’ayant aucune séance

**$\pi$ id(Cinéma) $\-$ $\rho$ idCinéma $\to$ id ($\pi$ idCinéma (Séance))**

<img src="/BDR/images/exemple8.PNG" width="50"/>

Il est aussi possible de renommer la relation:
Exemple de renommage de la relation r en s: **$\rho$ s (r)**

## Le produit cartésien
Le produit cartésien s’écrit: **r $\times$ s**
- Le résultat est une relation r' où **chaque tuple de r est associé à chaque tuple de s**

Exemple: 

<img src="/BDR/images/exemple9.PNG" width="500"/>

Le nombre de lignes du résultat vaut | r | * | s |
(où | r | est le nombre de lignes de r)

Le produit cartésien est, en lui-même, **rarement utile** car il combine "aveuglément" les tuples
- Dans l’exemple précédent chaque salle est associée à chaque cinéma, ce qui ne représente pas d’information pertinente
- Il y a donc beaucoup de **tuples inutiles**, les seuls pertinents sont ceux liant chaque salle à son cinéma

- Le produit cartésien est utile lorsqu’il est associé à l’opérateur de sélection afin de faire une **jointure**

## La jointure
Le jointure s’obtient par **composition d’un produit cartésien et d’une sélection**

Exemple: Les cinémas et **leurs** salles (et non pas les cinémas et toutes les salles comme dans l’exemple du seul produit cartésien)   

$\sigma$ Salle.idCinéma = Cinéma.id (Cinéma $\times$ Salle)

<img src="/BDR/images/exemple10.PNG" width="500"/>

Tous les tuples du résultat sont cette fois utiles car ils représentent une **information pertinente**

La jointure s’écrit: **r $\otimes$ c s**
- c est le critère de rapprochement qui est une opération de comparaison entre un attribut de r et un attribut de s Le seul opérateur réellement utilisé est =
- Dans la grande majorité de cas, la comparaison se fait entre la clé étrangère d’une des relations et la clé primaire de l’autre relation

La requête de l’exemple précédent s’écrit avec la jointure suivante:

**Cinéma $\otimes$ Salle.idCinéma = Cinéma.id Salle**

Quand le critère de rapprochement est une égalité, la jointure s’appelle une **équi-jointure** (sinon c’est une thêta-jointure)

**La jointure est utilisée quand les attributs nécessaires à une requêtes proviennent de plusieurs relations**

## La jointure naturelle
**Si aucun critère de rapprochement** n’est précisé, la jointure est une équi-jointure qui se fait sur **tous les attributs qui ont le même nom dans les deux relations**

En prenant les 2 relations suivantes:

Réalisateur(idRéalisateur, nom, prénom, annéeNaissance)
Film(titre, idRéalisateur, durée, année)

La requête **Film $\otimes$ Réalisateur** donne comme résultat :

<img src="/BDR/images/exemple11.PNG" width="500"/>

À noter qu’une seule occurrence de chaque attribut de même nom (ici idRéalisateur) est gardée dans le résultat

## La sélection généralisée
Une sélection peut se faire sur **plus d’un critère**

Exemple: les films qui datent d’après 2001 **et** durent moins de 2h
Cette requête peut s’écrire par composition de 2 sélections

**$\sigma$ année > 2001 ($\sigma$ durée < 120 (Film))** ou **$\sigma$ durée < 120 ($\sigma$ année > 2001 (Film))**

La requête précédente peut s’écrire plus simplement en utilisant un ‘et’ logique ($\Lambda$, aussi parfois noté AND)

**$\sigma$ année > 2001 $\Lambda$ durée < 120 (Film)**

<img src="/BDR/images/exemple12.PNG" width="400"/>

Il en va de même pour d’autres opérateurs

Exemple: les films qui datent d’après 2001 ou durent moins de 2h peut s’écrire par composition de 2 unions:

**$\sigma$ durée < 120 (Film) $\cup$ $\sigma$ année > 2001 (Film)** ou **$\sigma$ année > 2001 (Film) $\cup$ $\sigma$ durée < 120 (Film)**

La requête précédente peut s’écrire plus simplement en utilisant un ‘ou’ logique ($\vee$, aussi parfois noté OR):

**$\sigma$ année > 2001 $\vee$ durée < 120 (Film)**

<img src="/BDR/images/exemple13.PNG" width="400"/>

Exemple: Liste des films qui durent moins de 2h et ne datent pas de 2016

**$\sigma$ durée < 120 (Film) $\-$ $\sigma$ année = 2016 (Film)**

La requête précédente peut s’écrire plus simplement:

**$\sigma$ durée < 120 $\Lambda$ année $\not=$ 2016 (Film)**

<img src="/BDR/images/exemple14.PNG" width="400"/>

Pour résumer, **les équivalences** ($\leftrightarrow$) suivantes existent:

<img src="/BDR/images/selectiongeneralise.PNG" width="400"/>

Il faut noter que ces équivalences sont toujours vraies seulement si elles ne concernent qu’**une seule relation**

$\neg$ est l’opérateur de négation booléenne (NOT)

## Quantification universelle

## Requêtes sur plusieurs relations

## Complémentarité d’un ensemble

## La division

## Écriture de requêtes

## Possibilités des opérateurs algébriques vus
