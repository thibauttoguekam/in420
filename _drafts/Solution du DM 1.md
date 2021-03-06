---
layout: post
title: Solution du DM1
---

De nombreuses bonnes solutions ont été proposées. Voici celle proposée par Mickael De Oliveira.

~~~
import java.io.*;

/*
  Cette classe code un échiquier de diffusion. Les données sont
  calculée à partir d'une représentation de l'échiquier sous forme
  de chaîne de caractères. Voici un exemple d'échiquier formaté
  pour cette classe:

    se rit anudfjbkghclmopw.z'vyxq
*/
class Echiquier {
    int ligne2;
    int ligne3;
    char[][] echiquier = new char[3][10];
    
    public Echiquier(String echiq)
    {
        this.ligne2 = echiq.indexOf(' ');
        this.ligne3 = echiq.indexOf(' ', this.ligne2+1);
                
        int y=0;
                
        for(int i=0 ; i<this.echiquier.length ; i++)
            {
                for(int j=0 ; j<this.echiquier[i].length ; j++)
                    {
                        this.echiquier[i][j] = echiq.charAt(y);
                        y++;
                    }
            }
    }
}


class ChiffreVIC {
    public String chiffre(String clair,Echiquier E,String clef)
    {
        String code = "";
        String chiffre = "";
        int c = 0 , x =0 , texteCode = 0 , cle=0;
                
        for(int y=0 ; y<clair.length() ; y++)
            {
                for(int i=0 ; i< E.echiquier.length ; i++)
                    {
                        for(int j=0 ; j<E.echiquier[i].length ; j++)
                            {
                                if(clair.charAt(y) == E.echiquier[i][j])
                                    {
                                        switch(i)
                                            {
                                            case 0: code += j;
                                                break;
                                            case 1:        code += E.ligne2*10 + j;
                                                break;
                                            case 2: code += E.ligne3*10 + j;
                                                break;
                                            }
                                    }
                            }
                    }
            }
                
        for(int y=0 ; y<code.length() ; y++)
            {
                texteCode = Integer.parseInt(code.charAt(y)+"");
                cle = Integer.parseInt(clef.charAt(c)+"");
                        
                x = (texteCode+cle)%10;
                chiffre += x;
                        
                if (c<clef.length()-1)
                    c += 1;
                else
                    c = 0;
                        
            }
                
        return chiffre;
    }
        
    public String dechiffre(String code,Echiquier E,String clef)
    {
        String dechiffre = "";
        String decode = "";
        int c = 0 , x = 0, texte = 0, cle = 0, ligne = 0, colonne = 0;
                
        for(int i=0 ; i<code.length() ; i++)
            {
                texte = Integer.parseInt(code.charAt(i)+"");
                cle = Integer.parseInt(clef.charAt(c)+"");
                        
                if(texte - cle < 0)
                    {
                        x = texte - cle + 10;
                        decode += x;
                    }
                else
                    {
                        x = texte - cle;
                        decode += x;
                    }
                        
                        
                if (c<clef.length()-1)
                    c += 1;
                else
                    c = 0;
            }
                
        for(int y=0 ; y<decode.length() ; y++)
            {
                ligne = Integer.parseInt(decode.charAt(y)+"");
                        
                if(ligne == E.ligne2)
                    {
                        colonne = Integer.parseInt(decode.charAt(y+1)+"");
                        dechiffre += E.echiquier[1][colonne];
                        y += 1;
                    }
                else if(ligne == E.ligne3)
                    {
                        colonne = Integer.parseInt(decode.charAt(y+1)+"");
                        dechiffre += E.echiquier[2][colonne];
                        y += 1;
                    }
                else
                    {
                        dechiffre += E.echiquier[0][ligne];
                    }
            }
                
        return dechiffre;
    }
}


public class VIC {
    /**
     * @param args
     * On utilise 4 arguments différents :
     * - args[0] correspond au nom du fichier texte que l'on souhaite chiffré/déchiffré,
     * - args[1] correspond au nom du fichier texte contenant notre echiquier,
     * - args[2] correspond au nom du fichier texte contenant notre clef,
     * - args[3] correspond à la fonction que l'on souhaite utiliser:
     *     -chiffre : permet de chifffrer notre texte,
     *     -dechifffre : permet de dechiffrer notre texte.
     */
    public static void main(String[] args) throws IOException {
        if (args.length < 4) {
            System.out.println("Pas assez d'arguments");
            System.exit(0);
        }
                
        BufferedReader in = new BufferedReader(new FileReader(args[0]));  
        String texte = "", ligne;
                
        while ((ligne = in.readLine()) != null) 
            {
                texte += ligne;
            }
        System.out.println(texte+"\n");
               
                
        BufferedReader in2 = new BufferedReader(new FileReader(args[1])); 
        String echiq = "";
                
        while ((ligne = in2.readLine()) != null) 
            {
                echiq += ligne;
            }   
        System.out.println(echiq+"\n");
                
                
        BufferedReader in3 = new BufferedReader(new FileReader(args[2]));
        String clef = "";
                
        while ((ligne = in3.readLine()) != null) 
            {
                clef += ligne;
            } 
        System.out.println(clef+"\n");
                
                
        ChiffreVIC V = new ChiffreVIC();
        Echiquier E = new Echiquier(echiq);
        String VIC = "";
                
        System.out.println(E.ligne2+"\n");
        System.out.println(E.ligne3+"\n");
                
        switch(args[3])
            {
            case "chiffre": VIC = V.chiffre(texte,E,clef);
                break;
            case "dechiffre": VIC = V.dechiffre(texte,E,clef);
                break;
            }
                
        System.out.println(VIC+"\n");
    }
}
~~~

Quelques commentaires s'imposent. La boucle qui implante le chiffre de Vigenère utilise un compteur `c` pour parcourir la clef; ce compteur est remis à zéro à chaque fois que l'on dépasse la longueur de la clef par le `if` à la fin de la boucle.

~~~
int c = 0;

for(int y=0 ; y<code.length() ; y++) {
    texteCode = Integer.parseInt(code.charAt(y) + "");
    cle = Integer.parseInt(clef.charAt(c) + "");
                        
    x = (texteCode + cle) % 10;
    chiffre += x;
                        
    if (c<clef.length()-1)
        c += 1;
    else
        c = 0;
}
~~~

Je trouve que ceci aurait pu être écrit de façon plus élégante en se servant de l'opérateur `%` et en se passant du compteur `c`, comme suit:

~~~
for(int y=0 ; y<code.length() ; y++) {
    texteCode = Integer.parseInt(code.charAt(y) + "");
    cle = Integer.parseInt(clef.charAt(y % clef.length()) + "");
                        
    x = (texteCode + cle) % 10;
    chiffre += x;
}
~~~

Un défaut que j'ai pu observer dans absolument tous les rendus, est la duplication de la partie de code qui implante Vigenère dans le chiffrement et le déchiffrement. Il est évident que la seule différence entre les deux consiste à additionner ou soustraire la clef du texte clair ou chiffré. Ceci peut être résumé dans une unique fonction

~~~
private String vigenere(String code, String clef, Boolean chiffre) {
    int modificateur = chiffre ? 1 : -1

    for(int y=0 ; y<code.length() ; y++) {
        texteCode = Integer.parseInt(code.charAt(y) + "");
        cle = Integer.parseInt(clef.charAt(y % clef.length()) + "");

        // Remarquez l'ajout d'un 10, pour éviter d'avoir une valeur
        // négative lorsque modificateur vaut -1.
        x = (10 + texteCode + modificateur * cle) % 10;
        chiffre += x;
    }
}
~~~
