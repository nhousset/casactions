# session.addNodeStatus

> L'action `addNodeStatus` est utilisée dans un environnement SAS Viya pour surveiller l'état des machines (nœuds) qui sont en cours d'ajout à un serveur CAS (Cloud Analytic Services). C'est une action administrative essentielle pour les opérations d'élasticité du cluster, permettant de suivre la progression et de diagnostiquer les problèmes lors de l'ajout de nouvelles ressources de calcul.

## Syntaxe
sas
proc cas;
   session.addNodeStatus result=<nom_resultat> status=<nom_statut>;
run;


## Création de Données
Cette action ne requiert pas la création de tables de données en amont. Elle interroge directement l'état interne du serveur CAS pour fournir des informations sur les opérations d'ajout de nœuds en cours.
sas
/* Aucun code de préparation de données n'est requis pour cette action. */


## Exemple de base pour vérifier l'état d'ajout des nœuds
Cet exemple montre comment appeler l'action `addNodeStatus` pour obtenir un rapport sur les machines en cours d'intégration au cluster CAS. Le résultat est stocké dans une variable CASL nommée `r`.
sas
proc cas; 
   session.addNodeStatus result=r;
   print r;
run;

> Le résultat est une table qui liste chaque machine en cours d'ajout. Les colonnes typiques incluent le nom du nœud, son état (par exemple, 'pending', 'installing', 'active', 'failed'), et des messages d'information. Si aucune machine n'est en cours d'ajout, la table de résultats sera vide.

## Exemple détaillé avec affichage conditionnel du statut des nœuds
Cet exemple exécute l'action `addNodeStatus` et utilise une structure conditionnelle pour n'afficher la table de statut que si l'action s'est déroulée sans erreur. Cela permet de gérer les cas où l'action elle-même pourrait échouer et de fournir un retour plus clair à l'utilisateur.
sas
proc cas;
   session.addNodeStatus result=r status=st;
   if (st.statusCode == 0) then do;
      print "Vérification du statut d'ajout des nœuds réussie.";
      if (r.caslib is not null) then
         print r['addNodeStatus'];
      else
         print "Aucune opération d'ajout de nœud en cours.";
   end;
   else do;
      print "Erreur lors de l'appel de l'action addNodeStatus:";
      print st;
   end;
run;

> Si l'action réussit, un message de succès s'affiche, suivi de la table des statuts si des nœuds sont en cours d'ajout. Si aucune machine n'est ajoutée, un message l'indique. En cas d'échec de l'appel à l'action, les informations sur l'erreur sont affichées.
