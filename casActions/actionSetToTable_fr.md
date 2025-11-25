# builtins.actionSetToTable

> Crée une table en mémoire à partir d'un jeu d'actions défini par l'utilisateur. Cette action est utile pour inspecter ou archiver la définition d'un jeu d'actions personnalisé sous forme de table CAS.

## Syntaxe
sas
builtins.actionSetToTable /
   actionSet="nom_du_jeu_d_actions",
   casOut={caslib="caslib", name="nom_table", replace=true, ...};


## Paramètres

| Paramètre | Description |
| --- | --- |
| actionSet | Spécifie le nom du jeu d'actions défini par l'utilisateur à convertir. Le jeu d'actions est transformé de sa structure de définition en une table en mémoire. |
| casOut | Spécifie les paramètres de la table de sortie en mémoire. Vous devez au minimum spécifier le nom de la table. |

## Définition d'un jeu d'actions personnalisé
Avant de pouvoir convertir un jeu d'actions en table, nous devons en définir un. Ce code utilise l'action `defineActionSet` pour créer un jeu d'actions simple nommé `myActionSet`. Il contient une action `myEcho` qui affiche une valeur.
sas
proc cas;
  builtins.defineActionSet /
     actionSet='myActionSet',
     definition={
        myEcho={
           description='Echo a value.',
           parameters={
              value={ type='string', description='Value to echo.' }
           },
           code='print(params.value);'
        }
     };
run; quit;


## Exemples

### Conversion simple d'un jeu d'actions en table
Cet exemple de base convertit le jeu d'actions `myActionSet` précédemment défini en une table CAS nommée `myActionSetTable` dans la caslib `CASUSER`.
sas
proc cas; builtins.actionSetToTable / actionSet='myActionSet', casOut={caslib='CASUSER', name='myActionSetTable', replace=true}; run; quit;

Une nouvelle table CAS nommée `myActionSetTable` est créée dans la caslib `CASUSER`. Cette table contient la structure et les métadonnées du jeu d'actions `myActionSet`, incluant les détails de l'action `myEcho` et de ses paramètres.

### Conversion et promotion d'un jeu d'actions en table globale
Cet exemple convertit le jeu d'actions `myActionSet` en une table CAS, puis la promeut. Une table promue est accessible globalement par toutes les sessions CAS, ce qui est utile pour partager des définitions de jeux d'actions.
sas
proc cas; builtins.actionSetToTable / actionSet='myActionSet', casOut={caslib='CASUSER', name='myActionSetTable_Global', replace=true, promote=true}; run; quit;

Une table CAS globale nommée `myActionSetTable_Global` est créée. Elle est visible et utilisable par toutes les sessions CAS actives sur le serveur. La table contient la définition détaillée du jeu d'actions `myActionSet`.
