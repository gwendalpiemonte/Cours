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

## ORDER BY

## Jointures

## Alias

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
