# accessControl.assumeRole

> L'action `assumeRole` permet à un utilisateur d'adopter temporairement un rôle administratif spécifique au sein de la session CAS. Cette action est cruciale pour les tâches de gestion et de maintenance qui nécessitent des privilèges élevés, sans pour autant accorder ces droits de manière permanente à l'utilisateur. En assumant un rôle, l'utilisateur hérite de toutes les permissions associées à ce rôle pour la durée de la session ou jusqu'à ce que le rôle soit abandonné.

## Syntaxe
sas
proc cas;
   accessControl.assumeRole /
      adminRole="ACTION" | "DATA" | "SUPERUSER";
quit;


## Paramètres

| Paramètre | Description |
| --- | --- |
| adminRole | Spécifie le rôle administratif à endosser. Ce paramètre est obligatoire. |

## Aucune création de données nécessaire

L'action `assumeRole` ne manipule pas directement les données. Elle est utilisée pour la gestion des permissions et des rôles au sein du serveur CAS. Par conséquent, aucune étape de création de table n'est requise pour utiliser cette action.

sas
/* Pas de code de création de données pour cet exemple */


## Exemples

### Assumer le rôle de Super-Utilisateur
Cet exemple montre comment un administrateur peut endosser le rôle de `SUPERUSER` pour obtenir des privilèges complets sur le serveur CAS.
sas
proc cas; accessControl.assumeRole / adminRole='SUPERUSER'; run;

L'utilisateur obtient les privilèges de Super-Utilisateur, lui donnant un accès illimité à toutes les actions et données, ainsi que la capacité de gérer les rôles et les chemins.

### Endosser le rôle d'Administrateur des Actions
Cet exemple illustre comment un utilisateur peut assumer le rôle `ACTION`. Ce rôle est idéal pour les tâches de maintenance ou de déploiement qui nécessitent un accès complet aux jeux d'actions (action sets) et aux actions, sans pour autant donner accès aux données sensibles.
sas
proc cas; accessControl.assumeRole / adminRole='ACTION'; run;

L'utilisateur dispose d'un accès sans restriction (exempt de permissions) aux jeux d'actions et aux actions, mais ses permissions sur les données restent inchangées.

### Endosser le rôle d'Administrateur des Données
Cet exemple montre comment un utilisateur peut assumer le rôle `DATA`. Ce rôle est conçu pour les administrateurs de données qui ont besoin de gérer les caslibs, les tables et les colonnes sans avoir besoin de privilèges étendus sur les actions du système.
sas
proc cas; accessControl.assumeRole / adminRole='DATA'; run;

L'utilisateur obtient un accès sans restriction (exempt de permissions) aux définitions des caslibs, des tables et des colonnes, y compris la possibilité d'ajouter de nouvelles caslibs.

### Endosser le rôle de Super-Utilisateur (SUPERUSER)
Cet exemple montre comment un utilisateur assume le rôle `SUPERUSER`, qui combine les privilèges des rôles `ACTION` et `DATA`. C'est le niveau de privilège le plus élevé, nécessaire pour une administration complète du serveur CAS, y compris la gestion des autres rôles et des chemins d'accès.
sas
proc cas; accessControl.assumeRole / adminRole='SUPERUSER'; run;

L'utilisateur bénéficie des privilèges des rôles ACTION et DATA, ainsi que de la capacité de gérer les rôles et les chemins, lui conférant un contrôle total sur l'environnement CAS.
