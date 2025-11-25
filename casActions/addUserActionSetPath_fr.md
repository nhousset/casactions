# builtins.addUserActionSetPath

> Ajoute une caslib au chemin de recherche des jeux d'actions définis par l'utilisateur. Cela permet au serveur CAS de trouver et de charger des jeux d'actions personnalisés à partir d'un emplacement spécifié, étendant ainsi les fonctionnalités disponibles pour les utilisateurs.

## Syntaxe
sas
proc cas;
   builtins.addUserActionSetPath /
      caslib="nom_de_la_caslib";
quit;


## Paramètres

| Paramètre | Description |
|---|---|
| caslib | Spécifie le nom de la caslib à ajouter au chemin de recherche des jeux d'actions définis par l'utilisateur. |

## Prérequis : Création d'une caslib pour les jeux d'actions
sas
caslib myUserActions path="/chemin/vers/vos/actions/personnalisees/";


## Ajout d'une caslib au chemin de recherche
sas
proc cas; builtins.addUserActionSetPath caslib="myUserActions"; quit;


## Utilisation complète : Définir, Ajouter et Vérifier le chemin des actions
sas
proc cas; 
   /* Étape 1: Définir une caslib pointant vers les actions personnalisées */
   caslib myUserActions path="/chemin/vers/vos/actions/personnalisees/";

   /* Étape 2: Ajouter la caslib au chemin de recherche */
   builtins.addUserActionSetPath caslib="myUserActions";

   /* Étape 3: Vérifier la liste des chemins de recherche des actions utilisateur */
   builtins.userActionSetPathInfo;
quit;

