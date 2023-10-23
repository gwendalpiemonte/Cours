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
-  La valeur de chaque attribut **doit appartenir à son domaine**
- Lors de la création d’une table, cela correspond à attribuer un type à chaque attribut:
```SQL
CREATE TABLE Réalisateur (
id serial,
nom varchar(80), -- ou typeNoms (voir ci-dessous)
prénom varchar(80), -- ou typeNoms (voir ci-dessous)
dateNaissance date
);
```

- On peut créer des domaines (selon le SGBD):
```SQL
CREATE DOMAIN name [ AS ] data_type;
);
```

- Par exemple:
```SQL
CREATE DOMAIN typeNoms varchar(80);
);
```

- Il est possible d’ajouter des contraintes en utilisant la clause **CHECK**

- **CHECK** est suivi d’une expression booléenne qui exprime la contrainte à vérifier

- Exemple: Implémentation de la contrainte qui assure qu’il n’est pas possible d’insérer de réalisateur pas encore né
```SQL
CREATE TABLE Réalisateur (
id serial,
nom varchar(80),
prénom varchar(80),
dateNaissance date CHECK (dateNaissance <= CURRENT_DATE)
);
```

- Il est possible d’ajouter **CONSTRAINT name** afin de nommer la contrainte

## Contraintes de clé <a name="11"></a>
- Rappels:
  - Une relation peut avoir **plusieurs clés (clés candidates)**
  - La clé retenue devient la **clé primaire**
  - Il n’existe pas 2 tuples ayant la même valeur pour tous les attributs d’une clé
    
- Exemple avec définition de la clé primaire:
```SQL
CREATE TABLE Réalisateur (
id serial PRIMARY KEY,
nom varchar(80),
prénom varchar(80),
dateNaissance date
);
```

- La définition de la clé primaire peut aussi être faite après l’attribut:
```SQL
CREATE TABLE Réalisateur (
id serial,
nom varchar(80),
prénom varchar(80),
dateNaissance date,
PRIMARY KEY(id)
);
```

- Cette forme permet de nommer la contrainte:
```SQL
CREATE TABLE Réalisateur (
id serial,
nom varchar(80),
prénom varchar(80),
dateNaissance date,
CONSTRAINT PK_Réalisateur PRIMARY KEY(id)
);
```

- Il est **recommandé de nommer les contraintes** car cela facilite leur accès et les rend parlantes

## Contraintes d’intégrité des entités <a name="12"></a>
- **Aucune valeur de clé primaire ne peut être NULL**
- Si une clé primaire est composée de **plusieurs attributs**, ils doivent **tous être non NULL**
- Dès qu’une clé primaire est définie dans le SGBD tous ses attributs sont automatiquement non NULL, il n’y a donc **pas de vérification à ajouter explicitement**

## Contraintes de valeurs non NULL <a name="13"></a>
- Permet de définir un attribut comme étant obligatoire

- Par défaut, les attributs d’une table peuvent valoir NULL

- Exemple complété pour que chaque réalisateur ait obligatoirement un nom, un prénom et une date de naissance:
```SQL
CREATE TABLE Réalisateur (
id serial,
nom varchar(80) NOT NULL,
prénom varchar(80) NOT NULL,
dateNaissance date NOT NULL,
CONSTRAINT PK_Réalisateur PRIMARY KEY(id)
);
```

## Contraintes de valeurs uniques <a name="14"></a>
- Permet de définir qu’un attribut ne peut pas avoir 2 fois la même valeur pour tous les tuples de la relation/table

- Un cas classique d’utilisation est quand une clé primaire artificielle est ajoutée alors qu’une clé naturelle existe
  - Par exemple si une clé artificielle est ajoutée à Film, alors que titre est une clé naturelle
```SQL
CREATE TABLE Film (
id serial,
titre varchar(250) NOT NULL,
année smallint NOT NULL,
CONSTRAINT PK_Film PRIMARY KEY(id),
CONSTRAINT UC_Film_titre UNIQUE(titre)
);
```

- La contrainte UNIQUE permet de maintenir le fait que 2 films ne peuvent pas avoir le même titre

## Références: clé primaire - clé étrangère <a name="15"></a>
<img src="/BDR/images/Tableau.PNG" width="700"/>

- Dans cet exemple, chaque film **référence** un réalisateur
- id est **la clé primaire** de Réalisateur
- idRéalisateur est une **clé étrangère** dans Film
  - Elle référence la **clé primaire** de Réalisateur (id)
  - Une clé étrangère peut référencer n’importe quel ensemble d’attributs UNIQUE, mais en général chaque clé étrangère référence une clé primaire

## Contraintes d’intégrité référentielle <a name="16"></a>
<img src="/BDR/images/Tableau.PNG" width="700"/>

- Les attributs de la clé étrangère doivent avoir **les mêmes domaines (types)** que les attributs qu’ils référencent
- La valeur de la clé étrangère d’un tuple peut valoir:
  - Une valeur (de la clé primaire) référencée
  - NULL dans certains cas (selon la cardinalité minimale)

- Exemple:
```SQL
CREATE TABLE Film (
titre varchar(250),
année smallint NOT NULL,
idRéalisateur integer NOT NULL,
CONSTRAINT PK_Film PRIMARY KEY(titre),
CONSTRAINT FK_Film_idRéalisateur FOREIGN KEY
(idRéalisateur) REFERENCES Réalisateur(id)
);
```

- idRéalisateur a le même domaine que id de Réalisateur, qui est **serial (donc integer)**
- idRéalisateur est défini comme NOT NULL car il faut que tout film ait un réalisateur (cardinalité minimale de 1)
  - Point détaillé dans le chapitre sur les transformations modèle EA -> MR

# DML

## INSERT <a name="17"></a>
- De base, **INSERT** permet d’ajouter un tuple à une table
- Exemple: ajouter un tuple dans Réalisateur
```SQL
INSERT INTO Réalisateur(nom, prénom, dateNaissance)
VALUES ('Sciamma', 'Céline', '1978-11-12');
```

- Cette forme de l’instruction INSERT **nomme les attributs** qui vont être insérés, ce qui permet:
  - De mettre les attributs dans **n’importe quel ordre**
  - De pouvoir **omettre les attributs** dont la valeur n’a pas besoin d’être spécifiée:
    - Ici id car c’est un type auto-incrémenté (sa séquence détermine la prochaine valeur à utiliser)
    - C’est le cas de tous les attributs qui ont une **valeur par défaut**



- Exemple d’insertion **sans préciser le nom des attributs**:
```SQL
INSERT INTO Film
VALUES ('Portrait de la jeune fille en feu', 2019, 8);
```

- L’ordre des valeurs des attributs doit être le même que celui défini lors de la création de la table
- Pour **omettre des valeurs** par défaut (ici id), il faut l’indiquer explicitement en utilisant **DEFAULT**:
```SQL
INSERT INTO Réalisateur VALUES
(DEFAULT, 'Sciamma', 'Céline', '1978-11-12');
```

- Si tous les attributs ont une valeur par défaut, il est possible d’insérer un tuple sans donner la moindre valeur:
```SQL
INSERT INTO ... DEFAULT VALUES;
```

- L’instruction INSERT nécessite, de la part du SGBD, de **vérifier toutes les contraintes vues précédemment**, soit:

\
Contraintes de **domaine**
```SQL
INSERT INTO Film VALUES ('Blade Runner', TRUE, 1);
```
Erreur, TRUE n’est pas dans le domaine de l'attribut année (smallint)

\
Contraintes de **clé** 
```SQL
INSERT INTO Film VALUES ('Alien', 1979, 1);
```
Erreur, il y a déjà un tuple avec 'Alien' comme clé

\
Contraintes d’intégrité des **entités**
```SQL
INSERT INTO Film VALUES (NULL, 1979, 1);
```
Erreur, la clé primaire ne peut pas valoir **NULL**

\
Contraintes de valeurs **non NULL**
```SQL
INSERT INTO Film VALUES ('Blade Runner', NULL, 1)
```
Erreur, l’attribut année est défini NOT NULL

\
Contraintes de **valeurs uniques**
  En reprenant la définition de Film d'avant:
```SQL
INSERT INTO Film VALUES (DEFAULT, 'Alien', 1979);
```
Erreur, il y a déjà un tuple avec 'Alien' comme valeur pour titre qui est défini UNIQUE

\
Contraintes d’**intégrité référentielle**
```SQL
INSERT INTO Film VALUES ('Blade Runner', 1982, 11);
```
Erreur, la valeur (11) de la clé référencée par idRéalisateur n’est pas présente dans la table Réalisateur

\
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








