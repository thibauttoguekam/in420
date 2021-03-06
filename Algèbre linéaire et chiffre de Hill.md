---
layout: post
title: Algèbre linéaire et chiffre de Hill
---

Le [chiffre de Hill](Chiffrement par transposition) est une méthode de
chiffrement linéaire où la clef est constitué d'une matrice inversible
$$n\times n$$ à coefficients dans $$\mathbb{Z}/26\mathbb{Z}$$ et le
chiffrement se fait en décomposant le texte clair en blocs de longueur
$$n$$ et en appliquant la matrice à chacun par un produit matrice
vecteur.

## Algèbre linéaire

Nous allons commencer par quelques rappels d'arithmétique et d'algèbre linéaire.

1. Évaluer les expressions suivantes :

   $$3\cdot 5 + 12 \mod 26,\qquad 4\cdot 13 \mod 26,\qquad 10\cdot 26 + 1 \mod 26.$$

2. Calculer les produits matrice-vecteur suivants dans
   $$\mathbb{Z}/26\mathbb{Z}$$ (c'est à dire, tous les résultats son
   réduits modulo 26) :

   $$\begin{pmatrix}3&2\\5&3\end{pmatrix}\begin{pmatrix}3\\2\end{pmatrix},\qquad
   \begin{pmatrix}3&2&1\\5&3&2\\1&2&3\end{pmatrix}\begin{pmatrix}4\\2\\4\end{pmatrix}.$$

3. Calculer le produit matrice-matrice suivant dans $$\mathbb{Z}/26\mathbb{Z}$$ :

   $$\begin{pmatrix}3&2\\5&3\end{pmatrix}\begin{pmatrix}3&1\\2&3\end{pmatrix}.$$

4. Calculer le déterminant des matrices suivantes dans $$\mathbb{Z}/26\mathbb{Z}$$ :

   $$\begin{pmatrix}8&-2\\5&13\end{pmatrix},\qquad
   \begin{pmatrix}0&3&1\\1&2&3\\0&5&4\end{pmatrix}.$$


## Des matrices en Java

Nous allons maintenant implanter en Java les objets que nous venons de manipuler.

Créez un fichier `Matrice.java` contenant

~~~
public class Matrice {
    private int[][] coefficients;
    
    public Matrice(int[][] coeff) {}
    
    public int lignes() { return 0; }
    public int colonnes() { return 0; }
    /* Affiche la matrice */
    public String toString() { return null; }
    /* Additionne deux matrices */
    public Matrice add(Matrice autre) { return null; }
    /* Multiplie deux matrices */
    public Matrice mul(Matrice autre) { return null; }
    
    /* Traduit une chaine de caracteres en tableau
       d'entiers bidimensionnel */
    public static int[][] parse(String mat) {
        String[] lignes = mat.split(":");
        int[][] tableau = new int[lignes.length][lignes[0].split(",").length];
        
        for (int i = 0; i < lignes.length; i++) {
            String[] cols = lignes[i].split(",");
            for (int j = 0; j < cols.length; j++)
                tableau[i][j] = Integer.parseInt(cols[j]);
        }
        
        return tableau;
    }
        
    public static void main (String args[]) {
        if (args.length < 3) {
            System.out.println("Pas assez d'arguments");
            System.exit(0);
        }

        Matrice mat1 = new Matrice(parse(args[0]));
        Matrice mat2 = new Matrice(parse(args[2]));
        String op = args[1];

        System.out.println(mat1);
        System.out.println(op + "\n");
        System.out.println(mat2);
        System.out.println("=\n");

        Matrice mat3;
        if (op.equals("+"))
            mat3 = mat1.add(mat2);
        else
            mat3 = mat1.mul(mat2);

        System.out.println(mat3);
    }
}
~~~

Le code déjà écrit s'attend à recevoir sur la ligne de commande deux
matrices d'entiers séparées par une opération, `+` pour l'addition, toute autre chose pour la multiplication (mais attention à utiliser `*` de la ligne de commande: le terminal peut l'interpréter de façon bizarre).

Les matrices sont codées de la façon suivante: les éléments de chaque
ligne sont séparés par des virgules (`,`), ensuite chaque ligne est
séparée de la suivante par un deux points (`:`). Internement les
matrices sont stockées dans des tableaux bidimensionnels d'entiers.

Le programme lit les matrices et exécute l'opération demandée. Voici
un exemple d'exécution pour le programme une fois que vous aurez complété les autres fonctions :

~~~
$ java Matrice 1,2:3,3 + 2,1:3,3
1 2 
3 3 

+

2 1 
3 3 

=

3 3
6 6
~~~

1. Complétez le constructeur, qui stocke dans le champ privé le
tableau des coefficients.

2. Complétez les méthodes `lignes` et `colonnes` qui renvoient le
nombre de lignes et de colonnes de la matrice.

3. Complétez la méthode `toString` qui rend la représentation de la
matrice sous forme de chaîne de caractères.

4. Complétez la méthode `add` qui additionne deux matrices (seulement
si elles ont les mêmes dimensions).

5. Complétez la méthode `mul` qui multiplie deux matrices (seulement
si leurs dimensions sont compatibles).

Testez votre programme à travers un terminal.


## Le système de Hill

Dans le même dossier, créez un fichier `Hill.java` contenant 

~~~
import java.io.*;

public class Hill {
    // Transforme un texte en un tableau d'entiers entre 0 et 25
    public static int[] code(String texte) { return null; }
    // Transforme un tableau d'entiers entre 0 et 25 en texte
    public static String decode(int[] code) { return null; }
    // Chiffre un tableau d'entiers par la méthode de Hill
    public static int[] chiffre(Matrice clef, int[] code) { return code; }
    
    public static void main(String[] args) throws IOException {
        if (args.length < 2) {
            System.out.println("Pas assez d'arguments");
            System.exit(0);
        }
        
        BufferedReader in = new BufferedReader(new FileReader(args[0]));
        String texte = "", ligne;
        while ((ligne = in.readLine()) != null) {
            texte += ligne;
        }
        
        Matrice clef = new Matrice(Matrice.parse(args[1]));
        
        System.out.println(decode(chiffre(clef, code(texte))));
    }
}
~~~

Ce programme s'attend à recevoir sur sa ligne de commande le nom d'un
fichier à lire et une clef de Hill, codée comme dessus. Par exemple

~~~
java Hill clair.txt 1,2:3,3
~~~

1. Écrivez la fonction `code` qui transforme une chaîne de caractères
en un tableau d'entiers compris entre 0 et 25.
2. Écrivez la fonction `decode` qui effectue l'opération
inverse. Testez votre code en lançant le programme (si vous n'avez pas
encore changé la méthode `chiffre`, le résultat devrait être
exactement le fichier d'entrée).
3. Écrivez la fonction `chiffre` qui prend un tableau d'entiers de
longueur arbitraire et applique le chiffre de Hill à chaque bloc de
longueur $$n$$. N'oubliez pas de réduire les résultats modulo 26.
Testez que votre code marche en utilisant des matrices
dont vous savez calculer l'inverse.
