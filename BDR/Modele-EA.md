# Le modèle Entité-Association

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
