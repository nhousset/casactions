# accessControl.accessPersonalCaslibs

> Fournit un accès administratif à toutes les caslibs personnelles (CASUSER et CASUSERHDFS). Cette action est essentielle pour les administrateurs qui ont besoin de gérer, auditer ou dépanner les espaces de travail personnels des utilisateurs sans avoir à se connecter en tant que chaque utilisateur individuellement. Une fois exécutée, la session de l'administrateur peut voir et interagir avec le contenu de n'importe quelle caslib personnelle.

**Cette action nécessite des privilèges d'administrateur (Superuser).**
Code pour élever les privilèges :
sas
proc cas; accessControl.assumeRole / adminRole="superuser"; run;


## Syntaxe
sas
proc cas;
   accessControl.accessPersonalCaslibs / result=results status=rc;
run;


## Paramètres

| Paramètre | Description |
|---|---|
| result | Spécifie une variable CASL pour stocker les résultats de l'action. Ce paramètre est optionnel. |
| status | Spécifie une variable CASL pour stocker le code de statut de l'exécution de l'action, utile pour la gestion des erreurs. Ce paramètre est optionnel. |

## Exemples

### Obtenir l'accès administratif aux caslibs personnelles
Cet exemple montre comment un administrateur peut d'abord assumer le rôle de 'Superuser', puis exécuter l'action `accessPersonalCaslibs` pour obtenir un accès à toutes les caslibs personnelles sur le serveur CAS.
sas
proc cas; 
   accessControl.assumeRole / adminRole='superuser'; run; 
   accessControl.accessPersonalCaslibs; run; 
quit;


### Lister les tables d'un utilisateur spécifique après avoir obtenu l'accès
Après avoir obtenu les privilèges administratifs sur les caslibs personnelles, cet exemple illustre une utilisation concrète : lister les tables présentes dans la caslib personnelle (CASUSER) d'un utilisateur spécifique nommé 'user1'.
```
proc cas; 
   /* Étape 1: Assumer le rôle d'administrateur pour avoir le droit d'exécuter les actions suivantes. */ 
   accessControl.assumeRole / adminRole='superuser'; run; 

   /* Étape 2: Obtenir l'accès à toutes les caslibs personnelles. */ 
   accessControl.accessPersonalCaslibs; run; 

   /* Étape 3: Utiliser l'accès obtenu pour lister les tables dans la caslib personnelle de 'user1'. */ 
   table.tableInfo / caslib='CASUSER(user1)'; run; 
quit;

