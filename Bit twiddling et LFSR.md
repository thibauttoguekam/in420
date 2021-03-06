---
layout: post
title: Bit twiddling et LFSR
scripts: ['js/show-after.js']
---

Le Linear Feedback Shift Register (LFSR) est un outil pour la production d'une suite pseudo-aléatoire, de conception très simple et  adaptée à la réalisation matérielle. Ce TD compare les deux techniques principales pour réaliser un LFSR: le LFSR de Fibonacci et celui de Galois.

Le but de ce TD est aussi de se familiariser avec les opérateurs bit-à-bit de Java, que nous rappelons ici.

|-----------|----------|-------
| Opération | Résultat | Effet
|-----------|----------|-------
| `3 >> 1`  |  `1`     | décalage vers la droite, équivalent à `3 / 2`.
|-----------|----------|-------
| `3 << 1`  |  `6`     | décalage vers la gauche, équivalent à `3 * 2`.
|-----------|----------|-------
| `3 & 2`   |  `2`     | *et* bit-par-bit, le résultat aura des 1
|           |          | seulement aux positions où les deux opérands
|           |          | ont un 1.
|-----------|----------|-------
| `3 | 2`   |  `3`     | *ou* bit-par-bit, le résultat aura des 1
|           |          | aux positions où l'un des deux opérands a un
|           |          | 1.
|-----------|----------|-------
| `3 ^ 2`   |  `1`     | *xor* bit-par-bit, le résultat aura des 1
|           |          | aux positions où *exactement* un des deux
|           |          | opérands a un 1.
|-----------|----------|-------
| `~2`      |  `1`     |  négation bit-par-bit, les 0 et les 1 de
|           |          |  l'opérand sont inversés
|-----------|----------|-------



## LFSR de Fibonacci

On rappelle la structure d'un LFSR (de Fibonacci).

![](http://upload.wikimedia.org/wikipedia/commons/1/16/LFSR-F16.gif)

Des registres binaires (16 dans la figure) sont connectés en série de
la gauche vers la droite. Leur contenu forme l'*état* du LFSR.

Parmi ces registres, on en choisit certains
appelés *taps*, qui servent à effectuer la *rétroaction* (le
*feedback*): dans la figure ce sont les registres 11, 13, 14 et 16.

À chaque cycle, le LFSR produit un nouveau bit de la séquence pseudo-aléatoire de la façon suivante. Tous les
bits de l'état sont décalés vers la droite d'une position ; tandis que le bit le plus à la droite sort de l'état
pour s'ajouter à la séquence pseudo-aléatoire. Le bit le plus à gauche est remplacé par le **XOR** du contenu des *taps*.

Nous allons implanter un LFSR avec un état constitué de 16
bits. Les 16 bits seront stockés dans un seul `char` (qui en Java est
un type de 2 octets). Créez le fichier suivant.


~~~
public class LFSR {
    char etat;
    char taps;
    
    public LFSR(char etat, char taps) {
    }
    
    int parite(int masque) {
        return 0;
    }

    static String bin(int registres) {
        return String.format("%16s",
                             Integer.toBinaryString(registres)).replace(" ",
                                                                        "0");
    }

    public String fibonacci(int combien) {
        return null;
    }

    public String galois(int combien) {
        return null;
    }

    public static void main(String args[]) {
    }
}
~~~

1. Écrivez la méthode **`parite`** qui compte si l'expression binaire d'un
`char` contient un nombre pair ou impair de 1. Vous devez pouvoir vous
en sortir en trois lignes avec une boucle `for` et les opérateurs sur
les bits rappelés plus haut.

2. Écrivez la méthode **`fibonacci`**. Elle doit renvoyer une suite de **`combien`** bits pseudo-aléatoires sous forme de `String`. Utilisez la fonction `parite`
pour calculer le bit qui entre sur la gauche de l'état.

Testez votre programme (ajouter des affichages vous aidera à voir s'il
marche bien). La méthode `bin` vous aide à debugger votre programme:
par exemple, vous pouvez appeler

~~~
System.out.println(bin(etat));
~~~

Pour afficher le contenu de la variable `etat` exprimé en binaire.


3. Écrivez le constructeur, qui se limite à copier ses arguments dans
les attributs de l'objet.

4. Écrivez le `main`. Il doit prendre trois arguments sur la ligne de
    commande, dont deux obligatoires:

    - Un entier en base hexadécimale, qui code les *taps*. Voici
      comment transformer une chaîne de caractères hexadécimales (de
      longeur au plus 4) en `char`:
          
      ~~~
      char taps = (char)Integer.parseInt(args[0], 16);
      ~~~

    - Un entier donnant la longueur de la séquence pseudo-aléatoire que l'on souhaite
      générer (pour transformer une chaîne en `int`,
      [`Integer.parseInt`](http://docs.oracle.com/javase/6/docs/api/java/lang/Integer.html#parseInt%28java.lang.String%29)).


    - Une *graine* pour initialiser l'état du générateur (utilisez à
      nouveau
      [`Integer.parseInt`](http://docs.oracle.com/javase/6/docs/api/java/lang/Integer.html#parseInt%28java.lang.String%29));
      à défaut, la *graine* vaudra 1.

    
    Après avoir lu les arguments, le `main` crée un objet de type
    LFSR, appelle sa méthode `fibonacci` et en affiche la
    sortie.


## LFSR de Galois

Les LFSR de Galois sont une autre façon de générer une suite pseudo-aléatoire. On peut démontrer qu'ils sont équivalents aux LFSR de Fibonacci. La figure ci-dessous en illustre le fonctionnement.

![](http://upload.wikimedia.org/wikipedia/commons/3/3f/LFSR-G16.gif)

À chaque itération le bit sortant est celui tout à droite. Ce même bit
est réinjecté dans les *taps* en faisant un **XOR** avec le bit
qui précède le *tap*.

5. Implantez la méthode `galois`. Cette fois-ci, vous n'avez pas
besoin de faire appel à `parite`.

6. Modifiez le `main` pour qu'il prenne un argument permettant de choisir entre le mode Fibonacci et le mode Galois.


<div class="show-after" data-show-after="2014-03-12">

## Solution

Voici une solution complète. Essayez de compléter le TD avant de la regarder.

~~~
public class LFSR {           // cliquez ici pour voir la solution
    char etat;
    char taps;
        
    public LFSR(char etat, char taps) {
        this.etat = etat;
        this.taps = taps;
    }
        
    static int parite(int masque) {
        int p = 0;
        for ( ; masque > 0 ; masque >>>= 1)
            p ^= masque & 1;
        return p;
    }

    static String bin(int registres) {
        return String.format("%16s",
                             Integer.toBinaryString(registres)).replace(" ",
                                                                        "0");
    }

    public String fibonacci(int combien) {
        String alea = "";
        for (int i = 0 ; i < combien ; i++) {
            System.out.println("Debug info: etat = " + bin(this.etat));
            int bit = this.parite(this.etat & this.taps);
            alea += this.etat & 1;
            this.etat >>= 1;
            this.etat |= bit << 15;
        }
        return alea;
    }

    public String galois(int combien) {
        String alea = "";
        for (int i = 0 ; i < combien ; i++) {
            System.out.println("Debug info: etat = " + bin(this.etat));
            int bit = this.etat & 1;
            alea += bit;
            this.etat >>= 1;
            if (bit == 1)
                this.etat ^= this.taps;
        }
        return alea;
    }

    public static void main(String args[]) {
        if (args.length < 2) {
            System.out.println("Pas assez d'arguments");
            System.exit(0);
        }
        
        char taps = (char)Integer.parseInt(args[0], 16);
        int combien = Integer.parseInt(args[1]);
        
        char graine = 1;
        if (args.length >= 3) {
            graine = (char)Integer.parseInt(args[2], 16);
        }

        LFSR lfsr = new LFSR(graine, taps);
        
        System.out.println("Debug info: taps = " + bin(taps));
        System.out.println();

        if (args.length < 4 || !args[3].equals("galois"))
            System.out.println("\n" + lfsr.fibonacci(combien));
        else
            System.out.println("\n" + lfsr.galois(combien));
    }
}
~~~
{: .collapsible .collapsed}

</div>
