# Autoboxing-Unboxing
### Autoboxing
- Conversion automatique d’une valeur de type primitif en une instance de la classe wrapper (enrobeur) correspondante.
- int => Integer
```Java
Integer obj = 1; // au lieu de Integer obj = Integer.valueOf(1);
```

- Optimisation: pour certaines valeurs d’un type primitif (en tout cas de -128 à 127 pour les **int**), les objets enrobeurs correspondants ne sont créées qu’une seule fois (leur valeur n’étant pas modifiable).


### Unboxing
- Conversion automatique d’une instance d’une classe représentant un type primitif en la valeur correspondante.
- Integer => int
```Java
int i = obj; // au lieu de int i = obj.intValue();
```

### Exemples
Affichage des classes des objets enrobeurs :
```Java
Object array[] = { 1, 2.4, 4.0 / 2 }; // Autoboxing
for (int i = 0; i < array.length; ++i)
  System.out.println(array[i].getClass()); // toString()
```
Résultat :
```Java
class java.lang.Integer
class java.lang.Double
class java.lang.Double
```

Comparaison d’identités d’objets :
```Java
Integer integer = 1;   // Autoboxing
int value = integer;   // Unboxing
Object object = value; // Autoboxing

System.out.println(integer == 1);                  // true
System.out.println(integer == object);             // true mais false si integer = 1000
System.out.println(integer == new Integer(1));     // false
System.out.println(integer == Integer.valueOf(1)); // true mais false si integer = 1000
```

   
Simplification d’écriture, mais **une valeur de type primitif n’est pas un objet !**   

Attention aux performances : **l’autoboxing n’est pas gratuit**.   
=> préférer l’utilisations de types primitifs lorsque cela est possible.
   
Exemple :
```Java
private static long sum() {
  Long sum = 0L;
  for (long i = 0; i <= Integer.MAX_VALUE; ++i)
    sum += i; // unboxing, somme, creation d’un nouveau Long
  return sum;
}
```
code environ **5 fois plus lent** que la version avec un long au lieu du Long.
