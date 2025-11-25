# session.actionstatus

> Obtient le statut d'une action pour une session spécifiée.

## Syntaxe
sas
session.actionstatus <result=results> <status=rc> / uuid="string";


## Paramètres

| Paramètre | Description |
|---|---|
| uuid | Spécifie l'identifiant unique universel (UUID) de la session pour laquelle obtenir le statut de l'action. |

## Prérequis
sas
/* Aucune création de données n'est nécessaire pour utiliser cette action */


## Obtenir le statut d'une action pour une session spécifique
sas
session.actionstatus uuid='votre_uuid_de_session';


## Exemple complet : Lister les sessions et vérifier le statut d'une action
sas
/* Étape 1: Lister les sessions pour obtenir un UUID */
session.listSessions result=r;
print r;

/* Étape 2: Utiliser un UUID de la liste précédente pour vérifier le statut de l'action */
/* Remplacez 'uuid_de_la_session_cible' par un UUID réel obtenu à l'étape 1 */
session.actionstatus uuid='uuid_de_la_session_cible' result=status_result;
print status_result;

