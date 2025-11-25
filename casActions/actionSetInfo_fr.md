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
### Aucune création de données nécessaire
Cette action ne nécessite pas de table en entrée car elle opère sur les métadonnées du serveur CAS.
sas
/* Pas de code de création de données requis pour cette action */


## Exemples

### Obtenir les informations sur les jeux d'actions chargés
Cet exemple montre comment obtenir la liste et les informations de version pour tous les jeux d'actions qui sont actuellement chargés dans la session CAS active.

sas
proc cas;
   builtins.actionSetInfo;
run;

**Résultat Attendu:** Une table de résultats listant chaque jeu d'actions chargé, avec des détails tels que son nom, son étiquette, sa version et la date de construction.

### Obtenir les informations sur TOUS les jeux d'actions disponibles
En utilisant le paramètre 'all=TRUE', cet exemple récupère les informations pour absolutely tous les jeux d'actions installés sur le serveur CAS, qu'ils soient chargés ou non. C'est utile pour découvrir l'ensemble des fonctionnalités disponibles sur le serveur.

sas
proc cas;
   builtins.actionSetInfo / all=TRUE;
run;

**Résultat Attendu:** Une table de résultats complète qui liste tous les jeux d'actions disponibles sur le serveur. La table inclut une colonne indiquant si chaque jeu d'actions est actuellement chargé ou non, en plus des informations de version et de construction.
