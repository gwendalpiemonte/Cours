# Visibilité
La déclaration d’une classe ou d’une propriété peut être préfixée par un **modificateur de visibilité**.   

Permet de réaliser le concept d’encapsulation.   

**Visibilité des propriétés (attributs et méthodes):**
- **private** : visible seulement dans la classe.
- **aucun** : visibilité « package » (paquetage).
- **protected** : visibilité « package » et pour les sous-classes (même si dans un paquetage différent).
- **public** : visible partout.
- La plupart du temps, préférer une visibilité **private pour les attributs** (encapsulation).

Il est possible de déclarer un constructeur **private**, mais cela est pertinent :
- si un autre constructeur public l’invoque (par **this(…)**);
- ou s’il faut **interdire la création d’instances** de cette classe (p. ex. classes ne déclarant que des propriétés statiques, comme **java.lang.Math**), le constructeur par défaut étant **public**.

**Visibilité des classes:**
- **aucun**  : visibilité « package ». Avantage, classe pas exposée.
- **public** : visible partout, déclarée dans un fichier de même nom.
- **private/protected** : possible seulement pour les classes internes.
  - **private**   : visible pour la classe et la classe englobante.
  - **protected** : visibilité « package » et pour les sous-classes.   
  
<img src="/POO/images/Visibilite.PNG" width="700"/>

## Visibilité et héritage
Dans une sous-classe il est possible d’augmenter la visibilité d’une méthode redéfinie (mais pas de la restreindre, cela violerait les spécifications de
l’interface héritée) .
- private
- aucun
- protected
- public

Exemple
```Java
class Invisible
{
  protected void m() { … }
}

class Visible extends Invisible
{
  public void m() { … }
}
//^
//Mais pas private: d’après
//l’interface d’Invisible, une
//méthode m() doit être accessible
//dans Visible pour le module.
```
