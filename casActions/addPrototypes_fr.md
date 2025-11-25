# fcmpact.addPrototypes

> L'action `addPrototypes` du jeu d'actions `fcmpact` permet d'ajouter des définitions de prototypes de fonctions externes (souvent écrites en C ou C++) à une session CAS. Ces prototypes sont ensuite stockés dans une table CAS, les rendant accessibles pour être utilisés dans des programmes FCMP ou des étapes DATA exécutées sur le serveur CAS. Cela est particulièrement utile pour intégrer du code externe performant au sein de l'environnement SAS Viya.

## Syntaxe
sas
fcmpact.addPrototypes <result=results> <status=rc> /
    bridgeCatchSignals=TRUE | FALSE,
    bridgeFile="string",
    encode=TRUE | FALSE,
    funcTable={casouttable},
    library="string",
    package="string",
    routineCode={"string-1" <, "string-2", ...>},
    saveTable=TRUE | FALSE,
    stdcall=TRUE | FALSE;


## Paramètres

| Paramètre | Description |
| --- | --- |
| `bridgeCatchSignals` | Spécifie si le fichier de pont (bridge file) doit contenir du code pour installer et gérer les signaux système. Utile pour le débogage de code externe. |
| `bridgeFile` | Spécifie le chemin d'accès au module source du fichier de pont PROTO. Ce fichier fait le lien entre CAS et le code externe. |
| `encode` | Spécifie si les définitions de prototypes doivent être encodées (chiffrées) dans la table de fonctions de sortie pour protéger la propriété intellectuelle. Les alias sont `encrypt` ou `hide`. |
| `funcTable` | Paramètre obligatoire. Spécifie la table CAS de sortie où les définitions PROTO seront écrites. |
| `library` | Spécifie une bibliothèque FCMP existante à charger. Permet de combiner des prototypes avec des fonctions déjà définies. |
| `package` | Spécifie le nom du package FCMP utilisé pour stocker les définitions PROTO. Aide à organiser les fonctions. |
| `routineCode` | Paramètre obligatoire. Spécifie le code de définition PROTO à sauvegarder dans la table. C'est ici que vous déclarez la signature de la fonction externe. L'alias est `code`. |
| `saveTable` | Spécifie si la table de fonctions FCMP doit être sauvegardée de manière permanente dans une caslib. |
| `stdcall` | Spécifie que les fonctions doivent être appelées en utilisant la convention d'appel `__stdcall`. Principalement utilisé sur les systèmes d'exploitation Windows. |

## Préparation de l'environnement
Avant d'utiliser `addPrototypes`, il est courant d'avoir un fichier source C contenant la fonction que l'on souhaite appeler, et de le compiler en une bibliothèque partagée (.so sur Linux, .dll sur Windows). Pour cet exemple, nous allons simuler la déclaration d'une fonction C nommée `my_c_function` qui prend un double et retourne un double.
sas
/* Note: Ce bloc est conceptuel. Vous devriez avoir un fichier .c compilé sur votre serveur CAS. */
/* Fichier my_c_code.c */
#include <stdio.h>

double my_c_function(double input) {
    return input * 2.0;
}

/* Compiler avec: gcc -shared -fPIC -o my_c_library.so my_c_code.c */


## Exemples

### Exemple simple : Ajout d'un prototype de fonction C
Cet exemple montre comment déclarer un prototype pour une fonction C externe `my_c_function` qui se trouve dans une bibliothèque partagée `my_c_library.so`. Le prototype est ensuite stocké dans une table CAS nommée `myprotos`.
sas
proc cas;
    fcmpact.addPrototypes
        routineCode={"proto my_c_function(double) returns double;"}
        library="my_c_library"
        bridgeFile="/cas/caslibs/public/my_c_library.so"
        funcTable={name="myprotos", caslib="casuser", replace=true};
quit;

> L'action s'exécute avec succès et crée une table CAS nommée `myprotos` dans la caslib `casuser`. Cette table contient la définition du prototype pour la fonction `my_c_function`, la liant à la bibliothèque partagée spécifiée.

### Exemple détaillé : Ajout de plusieurs prototypes avec package et encodage
Cet exemple plus complexe ajoute deux prototypes de fonctions (`c_sum` et `c_product`) à un package nommé `myExternalCalcs`. Les définitions sont encodées pour la sécurité et la table de sortie est sauvegardée de manière permanente.
sas
proc cas;
    fcmpact.addPrototypes
        routineCode={
            "proto c_sum(double, double) returns double;",
            "proto c_product(double, double) returns double;"
        }
        library="my_math_lib"
        bridgeFile="/cas/caslibs/public/my_math_library.so"
        package="myExternalCalcs"
        encode=true
        funcTable={name="myExternalMath", caslib="public", replace=true}
        saveTable=true;
quit;

> Une table CAS nommée `myExternalMath` est créée et sauvegardée dans la caslib `public`. Elle contient les définitions encodées des prototypes pour `c_sum` et `c_product` sous le package `myExternalCalcs`. La table est promue pour être accessible globalement dans la session CAS.
