# DDL-DML

# Table des matières:

DDL:
1. [Création: base de données](#1)
2. [Création: schéma](#2)
3. [Création: table](#3)
4. [Types de données: numériques](#4)
5. [Types de données: caractères](#5)
6. [Types de données: temporels](#6)
7. [Types de données: Séquences](#7)
8. [Types de données: Création d’un ENUM](#8)
9. [Contraintes: introduction](#9)
10. [Contraintes de domaine](#10)
11. [Contraintes de clé](#11)
12. [Contraintes d’intégrité des entités](#12)
13. [Contraintes de valeurs non NULL](#13)
14. [Contraintes de valeurs uniques](#14)
15. [Références: clé primaire ](#15)
16. [Contraintes d’intégrité référentielle](#16)


DML:
1. [INSERT](#17)
2. [DELETE](#18)
3. [UPDATE](#19)
4. [RETURNING](#20)
5. [Intégrité référentielle: UPDATE/DELETE](#11)
6. [Intégrité référentielle: actions possibles](#22)
7. [Intégrité référentielle: exemple](#23)
8. [Modification de la structure d’une BDD](#24)


# SQL
<img src="/BDR/images/SQLLanguage.PNG" width="700"/>

# DDL

## Création: base de données <a name="1"></a>
Pour créer une base de données:
```SQL
CREATE DATABASE name;
```

Pour la supprimer:
```SQL
DROP DATABASE [ IF EXISTS ] name;
```

IF EXISTS permet d’éviter une erreur si la base de données
appellée name n’existe pas
- À noter que les instructions présentées dans ce chapitre le sont avec le minimum d’options habituellement utilisées, les versions complètes pour PostgreSQL sont trouvables [ici](https://www.postgresql.org/docs/15/index.html)

## Création: schéma <a name="2"></a>
- Les schémas permettent de structurer les grandes bases de données et/ou de gérer les droits plus finement
- Un schéma **public** est créé par défaut par PostgreSQL

Pour créer un schéma:
```SQL
CREATE SCHEMA schema_name;
```
Pour supprimer un schéma:
```SQL
DROP SCHEMA [ IF EXISTS ] name [ CASCADE | RESTRICT ]
```

L’option **CASCADE** permet de forcer la suppression de tout ce qui est contenu dans le schéma (tables, vues,…) **La valeur par défaut** est **RESTRICT**, elle n’autorise que la suppression de schémas vides

## Création: table <a name="3"></a>
Pour créer une table (et ses attributs):
```SQL
CREATE TABLE [ IF NOT EXISTS ] table_name
( [{ column_name data_type }] );
```

Exemple (incomplet pour le moment):
```SQL
CREATE TABLE Réalisateur (
id serial,
nom varchar(80),
prénom varchar(80),
dateNaissance date
);
```

## Types de données: numériques <a name="4"></a>
- Types entiers (signés):
  - **smallint (2 octets), integer (4 octets), bigint (8 octets)**
    
- Types réels point fixe:
  - **numeric** (precision, scale)
    - precision (> 0): nombre total de chiffres significatifs
    - scale (>= 0): nombre de chiffres après la virgule
    - Exemple: numeric(5, 2) va de -999.99 à 999.99
      
- Types réels point-flottant:
  - **real et double precision**
  - Types numériques inexacts de précision variable
    
- Types auto-incrémentés:
  - serial est un "raccourci" PostgreSQL pour avoir un type entier avec une séquence (**smallserial pour smallint**,…) afin qu’il s’auto-incrémente

## Types de données: caractères <a name="5"></a>
- **varchar(n) ou varying(n)**
  - Chaîne de caractères de **longueur variable d’au plus n caractères**

- **char(n) ou character(n)**
  - Chaîne de caractères de **longueur n fixe**
  - Si moins de n caractères sont donnés, les autres seront automatiquement remplis avec des espaces
  - Si plus de n caractères sont donnés, une erreur sera levée (sauf si les caractères excédentaires sont des espaces…)

- **text**
  - Chaîne de caractères de **longueur variable quelconque**
  - Pas standard en SQL, mais supporté par la plupart des SGBD
 
## Types de données: temporels <a name="6"></a>
- **date**
  - Pour représenter une date **sans les heures**
  - Le format recommandé est ISO 8601 (yyyy-mm-dd)
  - Par exemple le 8 janvier 1999: 1999-01-08
    
- **time**
  - Temps **sans la date**
  - Par exemple: 04:05:06

- **Timestamp**
  - Permet d’avoir **la date et l’heure**
  - Par exemple: 1999-01-08 04:05:06

**time** et **timestamp** peuvent avoir l’option **with time zone** qui permet d’ajouter un fuseau horaire (par ex. 04:05:06 PST)
    - Si rien n’est précisé, c’est équivalent à mettre: without time zone

- **interval** [ fields ]
  - Pour représenter **un intervalle de temps**
  - fields définit l’unité, par exemple **YEAR, HOUR,…**
  - Rarement utilisé comme type d’attribut mais souvent pour des calculs dans les requêtes, par exemple: uneDate + interval '2' months

- **boolean**
  - Valeurs standardisées SQL: **TRUE ou FALSE**
  - N’est pas supporté nativement par tous les SGBD (par exemple MySQL utilise en réalité un TINYINT(1))

- Pour avoir la liste exhaustive des types supportés par PostgreSQL: [ici](https://www.postgresql.org/docs/15/datatype.html)

## Types de données: Séquences <a name="7"></a>
Pour créer une séquence:
```SQL
CREATE SEQUENCE name [ AS data_type ]
[ INCREMENT increment ] [ MINVALUE minvalue ];
```
  - data_type doit être un type entier (bigint par défaut)
  - increment vaut 1 par défaut
  - minvalue depend de data_type, vaut 1 par défau
  
Exemple de création et d’utilisation d’une séquence:
```SQL
CREATE SEQUENCE réalisateurId AS integer;
CREATE TABLE Réalisateur (
id integer DEFAULT nextval('réalisateurId'),
...
```
  - DEFAULT permet de définir la valeur par défaut d’un attribut
  - nextval() permet d’obtenir la prochaine valeur de la séquence

- Cet exemple est équivalent à mettre serial comme type à id

## Types de données: Création d’un ENUM <a name="8"></a>
Pour créer un type énuméré:
```SQL
CREATE TYPE name AS ENUM ( [ 'label' [, ... ] ] )
```

Exemple:
```SQL
CREATE TYPE typeLecon AS ENUM ('cours', 'labo');
```
- Les valeurs d’un type énuméré sont "case sensitive"
- Il n’est pas possible de comparer des valeurs de types énumérés différents
- Les valeurs sont ordonnées, il est donc possible d’utiliser les opérateurs de comparaison (par exemple > ou <) entre 2 valeurs
- Les types énumérés sont prévus pour être "constants", on ne peut pas supprimer, modifier ou changer l’ordre des valeurs (mais on peut en ajouter)

## Contraintes: introduction <a name="9"></a>
- Une contrainte est une **condition qui doit être vérifiée pour tout tuple d’une relation/table**
  
- Les principaux types de contraintes sont ceux de:
  - Domaine
  - Clé
  - Intégrité des entités
  - Valeurs non NULL
  - Valeurs uniques
  - Intégrité référentielle

## Contraintes de domaine <a name="10"></a>

## Contraintes de clé <a name="11"></a>

## Contraintes d’intégrité des entités <a name="12"></a>

## Contraintes de valeurs non NULL <a name="13"></a>

## Contraintes de valeurs uniques <a name="14"></a>

## Références: clé primaire - clé étrangère <a name="15"></a>

## Contraintes d’intégrité référentielle <a name="16"></a>


# DML

## INSERT <a name="17"></a>

## DELETE <a name="18"></a>

## UPDATE <a name="19"></a>

## RETURNING <a name="20"></a>

## Intégrité référentielle: UPDATE/DELETE <a name="21"></a>

## Intégrité référentielle: actions possibles <a name="22"></a>

## Intégrité référentielle: exemple <a name="23"></a>

## Modification de la structure d’une BDD <a name="24"></a> 
- La commande **ALTER** permet de modifier les objets d’une base de données, par exemple pour une table on peut:
  - Ajouter ou supprimer des attributs
  - Modifier ou ajouter des valeurs par défaut aux attributs
  - Modifier ou ajouter des contraintes
  - […](https://www.postgresql.org/docs/15/sql-altertable.html)

Exemples:
```SQL
ALTER TABLE Film ADD COLUMN sortieCiné boolean NOT NULL;
ALTER TABLE Film ALTER COLUMN sortieCiné SET DEFAULT TRUE;
```

Les 2 instructions précédentes peuvent être faites en une seule:
```SQL
ALTER TABLE Film ADD COLUMN sortieCiné boolean NOT NULL
DEFAULT TRUE;
```








