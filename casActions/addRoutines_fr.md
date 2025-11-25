# fcmpact.addRoutines

> L'action `addRoutines` de l'ensemble d'actions FCMP est un outil puissant pour définir et compiler des fonctions et subroutines personnalisées (routines FCMP) directement sur le serveur CAS. Elle prend le code source écrit en syntaxe PROC FCMP, le compile et stocke les routines binaires résultantes dans une table CAS en mémoire. Cette table peut ensuite être sauvegardée physiquement. Une fois les routines ajoutées, elles peuvent être utilisées dans d'autres actions CAS, notamment dans les programmes DATA Step exécutés côté serveur, permettant d'étendre les capacités de traitement de données de SAS Viya avec une logique personnalisée et réutilisable.

## Syntaxe
sas
fcmpact.addRoutines /
   routineCode={"string-1" <, "string-2", ...>}
   funcTable={table-options}
   [appendTable=TRUE | FALSE]
   [library="string"]
   [package="string"]
   [saveTable=TRUE | FALSE];


## Paramètres

| Paramètre | Description |
| --- | --- |
| `appendTable` | Lorsqu'il est défini sur TRUE, ajoute les nouvelles routines à la table `funcTable` si elle existe déjà. Si FALSE (par défaut), la table est remplacée si elle existe (sauf si `replace=FALSE` est spécifié dans les options de `funcTable`). |
| `funcTable` | Spécifie la table CAS de sortie où les routines FCMP compilées seront stockées. Ce paramètre est obligatoire et contient des sous-paramètres comme `name` et `caslib`. |
| `library` | Optionnel. Spécifie le nom de la bibliothèque FCMP dans laquelle les routines seront stockées. C'est un niveau d'organisation logique au sein d'un package. Si non spécifié, une bibliothèque par défaut est utilisée. |
| `package` | Spécifie le nom du package FCMP. Un package est un conteneur pour un ensemble de fonctions et de subroutines. Ce nom sera utilisé pour référencer les routines. Si non spécifié, un nom par défaut est généré. |
| `routineCode` | Une liste de chaînes de caractères, où chaque chaîne contient une déclaration de fonction ou de subroutine FCMP complète. C'est le code source qui sera compilé. Ce paramètre est obligatoire. Alias : `code`. |
| `saveTable` | Si défini sur TRUE, la table en mémoire spécifiée dans `funcTable` est sauvegardée comme une table physique (par exemple, un fichier SASHDAT) dans la caslib correspondante. Cela la rend persistante entre les sessions CAS. Par défaut (FALSE), la table n'existe qu'en mémoire pour la durée de la session. |

## Connexion à une session CAS
Avant d'utiliser les actions CAS, il est nécessaire de démarrer une session CAS et de définir une caslib pour stocker les données et les résultats.
sas
cas mySession sessopts=(caslib='CASUSER');


## Exemples

### Ajouter une fonction d'addition simple
Cet exemple compile une fonction simple nommée `MYADD` qui additionne deux nombres. La fonction est placée dans le package `myPackage` et sauvegardée dans la table CAS en mémoire `myfuncs` dans la caslib `CASUSER`. L'option `replace=true` assure que la table est écrasée si elle existe déjà.
sas
proc cas;
   fcmpact.addRoutines /
      routineCode = {
         'function myadd(a, b); return(a+b); endsub;'
      },
      package = 'myPackage',
      funcTable = {name='myfuncs', caslib='CASUSER', replace=true};
quit;

L'action s'exécute avec succès. Le journal SAS affiche une note confirmant que le package `myPackage` contenant la fonction `MYADD` a été créé et stocké dans la table `CASUSER.MYFUNCS`.

### Compiler plusieurs routines et rendre la table persistante
Cet exemple compile deux routines : une fonction `MYMULT` pour la multiplication et une subroutine `MYDIV` pour la division avec reste. Les deux sont regroupées dans `myMathPackage`. L'option `saveTable=true` promeut la table en mémoire `myadvfuncs` en une table physique (fichier SASHDAT) dans la caslib `CASUSER`, la rendant ainsi disponible pour de futures sessions CAS sans avoir à la recréer.
sas
proc cas;
   fcmpact.addRoutines /
      routineCode = {
         'function mymult(a, b); return(a*b); endsub;',
         'subroutine mydiv(a, b, c, d); outargs c, d; c=a/b; d=mod(a,b); endsub;'
      },
      package = 'myMathPackage',
      funcTable = {name='myadvfuncs', caslib='CASUSER', replace=true},
      saveTable = true;
quit;

L'action s'exécute avec succès. Le journal indique que le package `myMathPackage` a été créé. De plus, la table `myadvfuncs` est sauvegardée comme une table physique dans le répertoire de la caslib `CASUSER`, ce qui est confirmé par une note dans le journal.
