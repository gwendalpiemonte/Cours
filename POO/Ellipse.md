# Ellipse

Définition d’une méthode acceptant un nombre de paramètres variables.
Syntaxe :
```Java
TypeRetour methode( [ paramètres ] Type … args).
```
D’autres paramètres peuvent être définis, mais la déclaration d’un nombre de paramètres variables doit être la dernière de la liste.
Les paramètres effectifs sont récupérés dans un tableau de **Type** (le type précédant l’ellipse … définit le type des arguments, contrairement au C).   
Remarque : la méthode **main** peut aussi s’écrire,
```Java
public static void main(String … args) (ou String[] args)
```
Exemple :
```Java
class A {
  void m(String … args) {
    for (int i = 0; i < args.length; i++)
      System.out.println(i + ": " + args[i]);
    }
}
new A().m("foo", "bar"); // objet anonyme sur lequel est invoqué m()
```

Arguments de main():

La méthode **main**, point d’entrée d’une application, reçoit en paramètre un tableau de **String** contenant les arguments de la ligne de commande.
Exemple :
```Java
public class Test
{
  public static void main(String[] args) {
    for (int i = 0; i < args.length; ++i)
      System.out.println(i + ": " + args[i]);
    }
}
> java Test un deux trois
0: un
1: deux
2: trois
```
