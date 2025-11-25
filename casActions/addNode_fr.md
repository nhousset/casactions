# builtins.addNode

> Ajoute une ou plusieurs machines (nœuds) à un serveur CAS existant. Cette action est essentielle pour l'élasticité et la scalabilité d'un environnement SAS Viya, permettant d'augmenter la puissance de calcul à la volée en ajoutant des workers ou d'améliorer la haute disponibilité en ajoutant un contrôleur de secours.

## Syntaxe
sas
proc cas;
   builtins.addNode /
      node={'nom-hôte-1', 'nom-hôte-2', ...},
      role='WORKER' | 'CONTROLLER';
quit;


## Paramètres

| Paramètre | Description |
| --- | --- |
| node | Spécifie une liste de noms d'hôtes des machines à ajouter au serveur. Chaque nom d'hôte doit être une chaîne de caractères valide et résolvable sur le réseau. |
| role | Définit le rôle de la ou des machines ajoutées. 'WORKER' ajoute des nœuds de travail pour distribuer les calculs. 'CONTROLLER' ajoute un contrôleur de secours pour la haute disponibilité (un maximum de deux contrôleurs est supporté). La valeur par défaut est 'WORKER'. |

## Prérequis pour l'ajout de nœuds
sas
/* Aucun code de création de données n'est nécessaire pour cette action. */
/* Assurez-vous que les machines 'casworker2' et 'cascontroller2' sont prêtes à rejoindre le cluster. */


## Ajouter un nouveau nœud de travail (Worker)
sas
proc cas; builtins.addNode / node={'casworker2'}, role='WORKER'; run; quit;


## Ajouter plusieurs workers simultanément
sas
proc cas; builtins.addNode / node={'casworker3', 'casworker4'}, role='WORKER'; run; quit;


## Ajouter un contrôleur de secours pour la haute disponibilité
sas
proc cas; builtins.addNode / node={'cascontroller2'}, role='CONTROLLER'; run; quit;

