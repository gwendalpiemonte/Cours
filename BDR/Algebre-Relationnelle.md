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




