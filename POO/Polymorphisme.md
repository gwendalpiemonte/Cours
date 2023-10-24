# Polymorphisme

Plusieurs polymorphismes ([Strachey 67] [Cardelli 85] [Meyer 88] [Booch 91]...).
- Universel
  - Paramétrique opération applicable sur plusieurs types (généricité).
  - Inclusion opération applicable sur des types liés par une relation de sous-typage (rem: héritage Þ sous-typage).

- Ad hoc
  - Surcharge même identificateur pour des opérations différentes.
  - Conversion conversion automatique de type.   

Principe de **substitution** en POO (polymorphisme d’inclusion) :   

si C’ est une sous-classe de C, une instance de C’ peut être utilisée là où une instance de C est attendue (affectations, passages de paramètres);
```Java
Person p = new Sportsman(…); // affectation polymorphique
```
   
fortement lié au mécanisme de liaison dynamique.
```Java
System.out.println(p); // invoque Sportsman::toString()
```
   
Un entier peut toujours être utilisé là où un réel est attendu, mais non nécessairement l’inverse.   

Pour les types primitifs, les conversions élargissantes suivantes sont effectuées implicitement par le compilateur :   

<img src="/POO/images/Poly.PNG" width="700"/>

Les autres conversions restrictives de types primitifs (sauf pour des valeurs connues à la compilation qui sont dans le domaine du type) doivent être effectuées explicitement.
```Java
int i = 1; double d = i; // conversion élargissante
i = (int) (d * 2.3); // transtypage nécessaire, i == 2
```

Autant un objet d’une sous-classe peut être utilisé comme un objet d’une super-classe (**un Student est une Person**), autant l’inverse n’est pas vrai.   

Exemple :

<img src="/POO/images/Poly2.PNG" width="700"/>

## Polymorphisme : instanceof

Une instance d’une classe C est membre de toutes les super-classes de C.

<img src="/POO/images/Poly3.PNG" width="700"/>   

**Transtypage (cast)**
- Permet de forcer le type d’une référence, d’une variable ou d’un littéral.
- Extrêmement **dangereux** si on ne sait pas ce que l’on fait.
- Pour les références, il faut que l’**objet** référencé soit effectivement **membre** de la nouvelle classe, sinon une exception **ClassCastException** est levée.
- Syntaxe : **(Type) variable**.

Exemple :

<img src="/POO/images/Poly4.PNG" width="700"/>  

Pour éviter ces erreurs de transtypage à l’exécution, l’opérateur **instanceof** permet de déterminer si un objet est **membre** d’une classe.
- Syntaxe : **expression instanceof Classe => boolean**
- instanceof rend true pour les classes dont l’objet rendu par l’expression **expression** est membre.

Exemple :
```Java
Person p = new Sportsman(…);
if (p instanceof Sportsman) {
  Sportsman s = (Sportsman) p;
  s.sport();
  boolean b = p instanceof Person; // Toujours true
}
```

Ne pas abuser de l’opérateur **instanceof** (dénote un code pas très OO…).   

Le **pattern matching** (filtrage par motif) pour l’opérateur **instanceof** permet le transtypage à la volée dans une nouvelle référence et d’éviter des erreurs (p.ex. mauvais transtypage en **(Student) p)** :
- Syntaxe : **expression instanceof Classe [identifiant] => boolean**
- Exemple :
```Java
if (p instanceof Sportsman s)
s.sport();
```

Dans l’expression **exp instanceof C [id]** :
- **exp** est une expression rendant une référence à un objet ;
- **C** peut être un nom de classe, d’interface ou un tableau.

Si **C** est une classe, détermine si **exp** référence un objet de la classe **C** ou d’une de ses sous-classes.   

Si **C** est une interface, détermine si **exp** référence un objet dont la classe implémente l’interface **C**.   

Si **C** dénote un tableau, **T[]**, détermine si **exp** référence un tableau d’éléments de type **T** (où **T** peut être un **type primitif, une classe ou une interface**).   

Remarque :
- A la compilation, le type dénoté par **C** doit être **compatible** avec **exp**.
- Exemple: si le type de la référence rendue par **exp** est une classe **R et C** dénote une classe, **C** doit être une sur-classe ou une sous-classe de **R**.

### Exemples

```Java
class Color {}
class Red extends Color {}
class Blue extends Color {}
// …
Color c = new Blue();
Red r = new Red();
System.out.println("Object c: " + (c instanceof Object)); true
System.out.println("Color c: " + (c instanceof Color));   true
System.out.println("Blue c: " + (c instanceof Blue));     true
System.out.println("Red c: " + (c instanceof Red));       false
System.out.println("Color r: " + (r instanceof Color));   true
System.out.println("Blue r: " + (r instanceof Blue));     erreur
System.out.println("Red r: " + (r instanceof Red));       true

Integer i[] = new Integer[5];
System.out.println("I[] i: " + (i instanceof Integer[])); true
System.out.println("O[] i: " + (i instanceof Object[]));  true
System.out.println("O i: " + (i instanceof Object));      true
System.out.println("S[] i: " + (i instanceof String[]));  erreur

Object o = i;
System.out.println("S[] o: " + (o instanceof String[]));  false
System.out.println("I[] o: " + (o instanceof Integer[])); true
System.out.println("O[] o: " + (o instanceof Object[]));  true
```

## Polymorphisme : Class

La classe prédéfinie **Class** représente tous les types de l’application.
- A chaque classe/interface/type primitif correspond un **unique** objet **Class**.
- Un attribut **class** est associé à chaque nom de type et référence l’objet **Class** correspondant. Syntaxe: **UnType.class**
- La méthode **getClass()** définie sur la classe **Object** rend un objet de type Class représentant la classe où est instancié l’objet manipulé.

<img src="/POO/images/Poly5.PNG" width="700"/>

Pour tester si un objet est **instancié** dans une classe donnée (et non pas seulement **membre**) il est nécessaire d’utiliser la classe **Class**.  

Exemple :
```Java
Person p = new Sportsman(…);
if (p.getClass() == Sportsman.class)
{
  Sportsman s = (Sportsman) p;
  s.sport();
  boolean b = p.getClass() == Person.class; // false…
}
```

Par ailleurs, la classe Class définit des méthodes de **réflexion**, comme :
- **String getName() et String getSimpleName()** rendant respectivement le nom complet et court de la classe (p. ex. **java.lang.String / String**),
- **Class getSuperClass()**, rendant l’éventuelle superclasse de la classe,
- **Object newInstance()**, permettant de créer un objet de la classe, etc.

## Polymorphisme : isInstance

La méthode **boolean isInstance(Object o)** définie sur la classe **Class** permet d’invoquer dynamiquement l’opérateur **instanceof**.   

### Exemples

<img src="/POO/images/Poly6.PNG" width="700"/>

<img src="/POO/images/Poly7.PNG" width="700"/>


Exemple :
```Java
class Person { /* … */ }
class Student extends Person { /* … */ }
class Sportsman extends Person { /* … */ }

Class[] types = { Student.class, Sportsman.class,
                  Person.class, Object.class };
//^
//Importance de l’ordre :
//classes plus spécifiques
//en premier pour avoir le
//vrai type (en ajoutant un
//break dans le for)

Person p = new Student(…);

for (int i = 0; i < types.length; ++i)
  if (types[i].isInstance(p))
    System.out.println("p : " + types[i].getSimpleName());
```
Résultat :
```Java
p : Student
p : Person
p : Object
```

## Pattern matching pour switch
Des patterns peuvent apparaître dans les labels des expressions **switch**.
- Si le label est une classe/interface/tableau, l’opérateur **instanceof** est appliqué (attention à l’ordre des labels dans les hiérarchies).

Exemple :
```Java
class Cat   { String meow() { return "meeeeow"; } }
class Human { int age()     { /* … */ }           }

Object o = /* … */
String sound =
  switch (o) {
    case null -> {
      System.out.println("No animal...");
      yield "";
    }
    case Cat c -> c.meow();
    case Human h when h.age() > 2 -> "Hello !";
    default -> "Argle-bargle";
};
```

