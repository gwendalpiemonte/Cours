# Requêtes SELECT

## Structure d'une requête
```sql
SELECT expression
FROM table_name
[WHERE condition];
```
- `expression` est souvent une liste de noms d’attributs dont la requête doit indiquer les valeurs 
- `table_name` est le nom de la table sur laquelle s’applique la requête
- `condition` est l’expression conditionnelle (booléenne) que doivent respecter les tuples qui sortiront de la requête

On peut avoir un résultat ensembliste en rajoutant `DISTINCT`.   
La présence d’une clause `WHERE` n’est pas obligatoire.   
Lorsqu'il faut qu’une requête retourne tous les attributs, il est possible d’utiliser un astérisque (*).

## [NOT] LIKE
Exemple: Liste des films contenant ‘Alien’ dans leur titre
```sql
SELECT *
FROM Film
WHERE titre LIKE '%Alien%';
```
`%` signifie un nombre quelconque de caractères (y compris aucun)   
`%` est le symbole le plus utilisé, mais il en existe d’autres:   
   
Par exemple, pour avoir tous les films dont la 2e lettre est ‘s’
```sql
SELECT *
FROM Film
WHERE titre LIKE '_s%';
```
_ signifie exactement 1 caractère     
   
Le résultat de la requête précédente montre que `LIKE` est "case sensitive" car le film ‘JSA’ ne sort pas dans les résultats
   
La recherche peut se faire à base d’expressions régulières en utilisant notamment `SIMILAR TO` (qui est "case sensitive")
   
Exemple: Liste des films commençant par une lettre comprise entre a et c
```sql
SELECT *
FROM Film
WHERE lower(titre) SIMILAR TO '[a-c]%';
```

## BETWEEN
Une condition sur un intervalle de valeurs s’écrit: datatype `BETWEEN` datatype `AND` datatype → boolean

Exemple: Liste des films qui durent entre 120 et 150 minutes
```sql
SELECT *
FROM Film
WHERE durée BETWEEN 120 AND 150;
```
Cette requête est équivalente à:
```sql
SELECT *
FROM Film
WHERE durée >= 120 AND durée <= 150;
```

## ORDER BY
Pour trier les résultats d’une requête, selon un ou plusieurs critères, il faut utiliser la clause `ORDER BY`:   
`ORDER BY` sort_expression1 [ASC | DESC] `ASC` pour un tri par ordre croissant (valeur par défaut)   
   
Exemple: Liste des films par ordre décroissant de leur durée, puis par ordre croissant de leur année
```sql
SELECT *
FROM Film
ORDER BY durée DESC, année; -- ou année ASC
```

## Jointures

## Alias
Il peut aussi y avoir des ambiguïtés sur les résultats.   
   
Exemple: Liste de noms des cinémas qui … et des réalisateurs
```sql
SELECT
Cinéma.nom, Réalisateur.nom
FROM
Cinéma INNER JOIN ... INNER JOIN Réalisateur...;
```
Cette requête est valide mais le résultat n’est pas parlant.   
Dans ce genre de cas il faut renommer le nom des attributs du résultat.   
   
Exemple précédent **avec renommage des attributs** du résultat:
```sql
SELECT
Cinéma.nom [AS] cinéma,
Réalisateur.nom [AS] réalisateur
FROM
Cinéma INNER JOIN ... INNER JOIN Réalisateur...;
```
Bien que le mot-clé AS soit facultatif, il est recommandé de le mettre pour une question de lisibilité.

Parfois un résultat n’a pas de nom implicite, la colonne de résultat sera nommée ?column?   
   
Exemple: Titre des films et indication s’ils durent plus de 2h
```sql
SELECT
titre, durée > 120
FROM
Film;
```
Dans ce genre de cas, il faut aussi utiliser un alias  

Un alias peut être réutilisé dans la requête, notamment dans la clause `ORDER BY`.   
   
Requête précédente avec alias et tri:
```sql
SELECT
titre, durée > 120 AS "film de plus de 2h"
FROM
Film
ORDER BY
"film de plus de 2h" DESC;
```

## IS [NOT] NULL

## UNION

## UNION ALL

## [NOT] IN

## Requêtes imbriquées

## ANY, SOME, ALL

## [NOT] EXISTS

## Requêtes corrélées

## Fonctions d’agrégation

## GROUP BY

## HAVING

## CASE

## COALESCE

## CTE
