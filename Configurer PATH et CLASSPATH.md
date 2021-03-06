---
layout: post
title: Configurer PATH et CLASSPATH
---

La configuration de ces variables d'environnement est nécessaire afin de pouvoir utiliser Java à travers un terminal. Si les systèmes Linux et MacOS sont souvent configurés d'avance, les systèmes Windows nécessitent quasiment toujours de ce réglage.

## Explication des variables

Les deux variables d'environnement `PATH` et `CLASSPATH` contiennent une liste de noms de répertoires (séparés par des `;` ou des `:`, selon le système) que le système utilise pour rechercher les exécutables de système et les bytecodes Java.

La variable d'environnement `PATH` instruit les terminaux (cmd sous Windows, bash sous Linux et MacOS, etc.) et le système d'exploitation sur les endroits où trouver des programmes exécutables. Lorsque l'on tape un nom de commande (par exemple, `javac`) dans un terminal, le système cherche dans chacun des dossiers de `PATH` un fichier exécutable nommé `javac` ou `javac.exe`. Lorsque le fichier exécutable n'est trouvé dans aucun des fichier, le système renvoie une erreur.

La variable d'environnement `CLASSPATH` est utilisée exclusivement par Java, en particulier par les commandes `java` et `javac`. Elle instruit le compilateur et l'interprète sur les endroits où trouver les fichiers `.class` des bibliothèques Java. Cette liste doit nécessairement contenir les endroits où sont installés les paquets par défaut de Java, mais elle peut aussi contenir des répertoires privés de l'utilisateur où sont compilés des paquets développés par ce dernier.


## Configuration sous Windows


### Configuration de `PATH`

Repérez le dossier contenant les exécutables `javac.exe` et `java.exe`. En général, il s'agit du dossier `C:\Programmes\jdkX.X.X\bin` ou `C:\Program Files\jdkX.X.X\bin`. 

Ajoutez un `;` à la fin de la variable d'environnement `PATH`, suivi par le nom de ce dossier. Ces instructions <http://java.com/fr/download/help/path.xml> vous expliquez comment éditer des variables d'environnement sous Windows.

Si vous avez fait une fausse manip et que vous voulez remettre `PATH` à sa valeur d'origine, le `PATH` par défaut de Windows 7 est 

~~~
%SystemRoot%\system32;%SystemRoot%;%SystemRoot%\System32\Wbem;%SYSTEMROOT%\System32\WindowsPowerShell\v1.0\
~~~

d'après [cette source](http://superuser.com/questions/124239/what-is-the-default-path-environment-variable-setting-on-fresh-install-of-window).


### Configuration de `CLASSPATH`

La configuration de `CLASSPATH` n'est en général pas nécessaire. Si vous deviez avoir besoin de la configurer, suivez ces instructions <http://java.com/fr/download/help/path.xml>.


## Configuration sous Linux

### Configuration du `PATH`

Normalement les exécutables `java` et `javac` s'installent dans `/usr/bin` ou `/usr/local/bin`, et ces dossiers sont déjà présents dans le `PATH`. Aucune configuration n'est donc nécessaire.

Si votre installation de Java se trouve dans un dossier exotique, par exemple `/home/java/bin`, ajoutez les lignes suivantes à la fin du fichier `.profile`.

~~~
PATH="$PATH:/home/java/bin`
~~~

Pour rendre effectives les modifications, tapez

~~~
. .profile
~~~

dans le terminal.

### Configuration du `CLASSPATH`

La configuration du `CLASSPATH` n'est en général pas nécessaire. Si vous devez ajouter des répertoires au classpath, éditer le fichier `.profile` comme pour `PATH`.


## Configuration sous MaxOS

Les mêmes instructions que pour Linux devraient marcher.
