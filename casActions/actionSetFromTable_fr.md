# builtins.actionSetFromTable

> Restaure un jeu d'actions défini par l'utilisateur à partir d'une table CAS préalablement sauvegardée. Cette action est l'inverse de `actionSetToTable` et permet de recharger dynamiquement des fonctionnalités personnalisées dans une session CAS, facilitant ainsi la réutilisation et le partage de code.

## Syntaxe
sas
BUILTINS.ACTIONSETFROMTABLE<
  result=results
  status=rc
> /
  NAME="string",
  TABLE={<castable>};


## Paramètres
| Paramètre | Description |
| --- | --- |
| `name` | Spécifie le nom à attribuer au jeu d'actions défini par l'utilisateur qui sera restauré. |
| `table` | Spécifie la table en mémoire à partir de laquelle le jeu d'actions défini par l'utilisateur sera construit. Cette table doit avoir été créée précédemment par l'action `actionSetToTable`. |

## Création et Sauvegarde d'un Jeu d'Actions Personnalisé
Avant de pouvoir restaurer un jeu d'actions, nous devons d'abord en définir un, puis le sauvegarder dans une table CAS à l'aide de l'action `actionSetToTable`. Cet exemple crée un jeu d'actions simple nommé `myActionSet` avec une action `myEcho` qui renvoie simplement les paramètres fournis.
sas
proc cas;
  builtins.defineActionSet
    name="myActionSet",
    actions={
      {
        name="myEcho",
        parms={ {name="...", type="any"} },
        script="print(params)"
      }
    };

  builtins.actionSetToTable
    name="myActionSet",
    table={name="myActionSetTable", caslib="CASUSER", replace=true};
quit;


## Exemples

### Restauration Simple d'un Jeu d'Actions
Cet exemple montre comment restaurer le jeu d'actions `myActionSet` à partir de la table `myActionSetTable` sauvegardée précédemment. Nous le restaurons sous un nouveau nom, `restoredActionSet`, pour éviter les conflits si l'original existe toujours.
sas
proc cas; builtins.actionSetFromTable name='restoredActionSet' table={caslib='CASUSER', name='myActionSetTable'}; run; restoredActionSet.myEcho p1='Hello' p2='World'; run; quit;

L'action `myEcho` du jeu d'actions restauré s'exécute et affiche les paramètres `p1` et `p2` dans le journal SAS, confirmant que la restauration a réussi.

### Restauration et Vérification du Jeu d'Actions
Cet exemple restaure le jeu d'actions et utilise ensuite l'action `actionSetInfo` pour vérifier que le nouveau jeu d'actions (`restoredSetVerify`) est bien chargé sur le serveur CAS. Cela permet de confirmer programmatiquement le succès de l'opération.
sas
proc cas; builtins.actionSetFromTable name='restoredSetVerify' table={caslib='CASUSER', name='myActionSetTable'}; run; builtins.actionSetInfo actionSet='restoredSetVerify'; run; quit;

Le journal affichera les informations détaillées sur le jeu d'actions `restoredSetVerify`, y compris la liste des actions qu'il contient (dans ce cas, `myEcho`). Cela prouve que le jeu d'actions a été correctement restauré à partir de la table CAS.

### Gestion des Conflits de Noms
Si vous essayez de restaurer un jeu d'actions avec un nom qui existe déjà, l'opération échouera. Cet exemple illustre ce cas en restaurant une première fois, puis en tentant de le restaurer à nouveau avec le même nom, ce qui génère une erreur.
sas
proc cas; builtins.actionSetFromTable name='conflictTest' table={caslib='CASUSER', name='myActionSetTable'}; run; builtins.actionSetFromTable name='conflictTest' table={caslib='CASUSER', name='myActionSetTable'}; run; quit;

La première action `actionSetFromTable` réussit. La seconde échoue et une erreur est consignée dans le journal SAS, indiquant que le jeu d'actions `conflictTest` est déjà chargé. Pour remplacer un jeu d'actions, il faut d'abord le supprimer avec `dropActionSet`.
