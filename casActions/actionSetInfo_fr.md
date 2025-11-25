# builtins.actionSetInfo

> Affiche les informations de construction (build) des jeux d'actions (action sets) qui sont actuellement chargés dans la session CAS. Cette action est fondamentale pour l'administration et la compréhension de l'environnement CAS, car elle permet de vérifier quelles fonctionnalités sont disponibles et leur version.

## Syntaxe
sas
builtins.actionSetInfo <result=results> <status=rc> / 
   all=TRUE | FALSE;


## Paramètres
| Paramètre | Description |
|---|---|
| all | Lorsque défini sur True, les résultats incluent tous les jeux d'actions disponibles sur le serveur CAS, en plus de ceux qui sont déjà chargés. L'exécution de cette action est plus lente lorsque ce paramètre est activé car elle nécessite une interrogation plus large du système. |

## Création de Données
sas
/* Pas de code de création de données requis pour cette action */


## Obtenir les informations sur les jeux d'actions chargés
sas
proc cas;
   builtins.actionSetInfo;
run;


## Obtenir les informations sur TOUS les jeux d'actions disponibles
sas
proc cas;
   builtins.actionSetInfo / all=TRUE;
run;

