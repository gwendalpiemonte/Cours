# Entree Sortie standard

## Entrée standard
La classe **System** déclare également l’attribut de classe **in** (de type InputStream) représentant le flux d’entrée standard.   

La classe **InputStream** est peu utilisable en tant que telle (lecture d’octets).   

La classe **Scanner** est un analyseur textuel simple permettant d’extraire des valeurs de type primitif et des instances de **String** d’un flux donné (**InputStream**, fichier ou chaîne de caractères).   

Exemples :
```Java
Scanner sin = new Scanner(System.in);

System.out.print("Nom: ");
String nom = sin.next();
System.out.print("Adresse: ");
String adresse = sin.nextLine();

int sum = 0;
while (sin.hasNextInt())
  sum += sin.nextInt();
System.out.printf("Somme: %d\n", sum);
```

## Sortie standard
La classe **System** définit les attributs de classe **out et err**, de type **PrintStream**, représentant les flux de sortie et d’erreur standards.   

La classe **PrintStream** offre les méthodes d’affichage :
- Surcharges de **print() et println()** acceptant en paramètre des valeurs de type primitif, des instances de **String** et des objets.
- Une méthode **printf(String format, Object … args)**, largement inspirée de celle de C acceptant un nombre variable d’arguments.
- Si un **String** est requis et qu’un objet est fourni la conversion s’effectue automagiquement par l’invocation de la méthode toString() sur l’objet.

Exemple, affichage d’un tableau d’objets Person :
```Java
Person tableau[] = /* … */;

for (int i = 0; i < tableau.length; ++i)
System.out.printf("%2d : %s\n", i, tableau[i]);

//Pour tableau[i] => Appel de Person::toString()
//Pour le i => Autoboxing de i en objet Integer
```
