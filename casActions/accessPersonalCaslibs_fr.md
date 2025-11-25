# accessControl.accessPersonalCaslibs

> Fournit un accès administratif à toutes les caslibs personnelles (CASUSER et CASUSERHDFS). Cette action est cruciale pour les administrateurs qui ont besoin de gérer ou de dépanner les espaces de travail des utilisateurs sans avoir à se connecter en tant que chaque utilisateur individuellement. Elle simplifie la maintenance et la supervision des données personnelles sur le serveur CAS.

## Syntaxe
sas
accessControl.accessPersonalCaslibs <result=results> <status=rc>;


## Accorder l'accès aux caslibs personnelles
sas
proc cas;
   accessControl.accessPersonalCaslibs;
run;


## Accorder l'accès et vérifier le statut de l'opération
sas
proc cas;
   accessControl.accessPersonalCaslibs result=resultat_action;
   print resultat_action;
run;

