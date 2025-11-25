# accessControl.accessPersonalCaslibs

> Fournit un accès administratif à toutes les caslibs personnelles (CASUSER et CASUSERHDFS). Cette action est cruciale pour les administrateurs qui ont besoin de gérer ou de dépanner les espaces de travail des utilisateurs sans avoir à se connecter en tant que chaque utilisateur individuellement. Elle simplifie la maintenance et la supervision des données personnelles sur le serveur CAS.

## Syntaxe
sas
accessControl.accessPersonalCaslibs <result=results> <status=rc>;


## Paramètres
| Paramètre | Description |
|---|---|
| *Aucun* | *Cette action n'a pas de paramètres.* |

## Aucune création de données
Cette action ne crée pas de table de données. Elle est utilisée pour des tâches administratives de contrôle d'accès.

## Accorder l'accès aux caslibs personnelles
Cet exemple montre comment un administrateur peut obtenir les permissions nécessaires pour accéder à toutes les caslibs personnelles sur le serveur CAS.
sas
proc cas;
   accessControl.accessPersonalCaslibs;
run;


## Accorder l'accès et vérifier le statut de l'opération
Cet exemple exécute l'action `accessPersonalCaslibs` et stocke le statut de l'opération dans une variable CASL nommée `resultat_action`. L'affichage de cette variable permet de confirmer que l'action a réussi avant de procéder à des opérations sur les caslibs personnelles.
sas
proc cas;
   accessControl.accessPersonalCaslibs result=resultat_action;
   print resultat_action;
run;

