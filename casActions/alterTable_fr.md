# table.alterTable

> L'action `alterTable` dans SAS Viya est un outil puissant pour la gestion des tables en mémoire. Elle permet de modifier les métadonnées d'une table CAS sans avoir à la recréer. Vous pouvez l'utiliser pour renommer la table elle-même, ajouter ou modifier son libellé, réorganiser, renommer, formater ou supprimer des colonnes, et même changer le type de certaines colonnes. C'est une action essentielle pour nettoyer et préparer les données directement sur le serveur CAS, optimisant ainsi les performances en évitant des transferts de données inutiles.

## Syntaxe
sas
proc cas;
   table.alterTable /
      caslib="caslib-name"
      name="table-name"
      <rename="new-table-name">
      <label="table-label">
      <columns={ {name="col1", rename="new_col1", label="nouveau libellé", format="nouveau_format"}, 
                 {name="col2", drop=true} }>
      <drop={"col3", "col4"}>
      <keep={"col5", "col6"}>
      <columnOrder={"new_col1", "col5", "col6"}>;
run;


## Paramètres

| Paramètre | Description |
| --- | --- |
| caslib | Spécifie la caslib où se trouve la table à modifier. Si omis, la caslib active de la session est utilisée. |
| name | Nom de la table en mémoire que vous souhaitez modifier. |
| rename | Spécifie le nouveau nom pour la table. |
| label | Assigne ou modifie le libellé de la table. |
| columns | Liste d'objets, chacun spécifiant les modifications à apporter à une colonne (renommer, changer de libellé, de format, de type ou supprimer). |
| drop | Liste de noms de colonnes à supprimer de la table. |
| keep | Liste de noms de colonnes à conserver, toutes les autres étant supprimées. |
| columnOrder | Spécifie le nouvel ordre des colonnes dans la table. |
| lifetime | Définit une durée de vie (en secondes) pour la table. La table sera supprimée de la mémoire si elle n'est pas accédée pendant cette durée. |
| tableRedistUpPolicy | Définit la politique de redistribution des données de la table si le nombre de nœuds de travail du serveur CAS augmente. |

## Création de la table de démonstration 'CARS'
Ce bloc de code SAS crée une table nommée 'CARS' dans la caslib de l'utilisateur (CASUSER). Cette table contient des informations sur différents modèles de voitures, incluant la marque, le modèle, le type, l'année et le prix. Elle servira de base pour les exemples suivants qui illustrent comment modifier ses propriétés avec l'action `alterTable`.
sas
data casuser.cars;
   length Make $ 12 Model $ 20 Type $ 8;
   infile datalines delimiter=',';
   input Make Model Type Year MSRP;
   datalines;
Toyota,Camry,Sedan,2023,26000
Ford,F-150,Truck,2024,45000
Honda,Civic,Sedan,2023,24000
Chevrolet,Tahoe,SUV,2024,60000
Nissan,Leaf,Electric,2023,35000
;
run;


## Exemples

### Renommer une table
Cet exemple montre comment utiliser `alterTable` pour simplement changer le nom d'une table en mémoire de 'CARS' à 'AUTOMOBILES'.
sas
proc cas;
   table.alterTable /
      caslib="casuser",
      name="CARS",
      rename="AUTOMOBILES";
run;
quit;

**Résultat attendu :** L'action s'exécute avec succès. La table 'CARS' n'existe plus dans la caslib 'CASUSER' et a été remplacée par la table 'AUTOMOBILES' avec le même contenu. Le journal SAS affichera une note confirmant le succès de l'opération.

### Supprimer une colonne d'une table
Cet exemple illustre comment supprimer la colonne 'Type' de la table 'CARS'.
sas
proc cas;
   table.alterTable /
      caslib="casuser",
      name="CARS",
      drop={"Type"};
run;
quit;

**Résultat attendu :** La colonne 'Type' est retirée de la table 'CARS'. Une vérification de la structure de la table (par exemple avec `table.columnInfo`) montrerait que cette colonne n'est plus présente.

### Modification complète d'une table
Cet exemple détaillé montre comment enchaîner plusieurs modifications en un seul appel à `alterTable`. On renomme la table, on lui ajoute un libellé, on renomme la colonne 'MSRP' en 'Price' tout en lui appliquant un format monétaire et un nouveau libellé, et on supprime la colonne 'Year'.
sas
proc cas;
   table.alterTable /
      caslib="casuser",
      name="CARS",
      rename="VEHICLE_SALES",
      label="Données de vente de véhicules 2023-2024",
      drop={"Year"},
      columns={{
         name="MSRP",
         rename="Price",
         label="Prix de Vente au Détail Suggéré",
         format="DOLLAR12.2"
      }};
run;
quit;

**Résultat attendu :** La table 'CARS' est transformée en 'VEHICLE_SALES' avec un nouveau libellé. La colonne 'Year' est supprimée. La colonne 'MSRP' est maintenant nommée 'Price', avec un libellé descriptif et un format d'affichage en dollars. Le journal SAS indiquera le succès de l'opération et les métadonnées de la table refléteront toutes ces modifications.

### Réordonner et formater plusieurs colonnes
Cet exemple se concentre sur la modification des colonnes. Il change le libellé de 'Make', applique un format à 'Year', et surtout, définit un nouvel ordre pour les colonnes de la table 'CARS'.
sas
proc cas;
   table.alterTable /
      caslib="casuser",
      name="CARS",
      columns={{
         name="Make",
         label="Constructeur Automobile"
      },
      {
         name="Year",
         format="4."
      }},
      columnOrder={"Model", "Make", "Year", "Type", "MSRP"};
run;
quit;

**Résultat attendu :** Les colonnes de la table 'CARS' sont maintenant dans l'ordre : Model, Make, Year, Type, MSRP. La colonne 'Make' a un nouveau libellé et la colonne 'Year' s'affichera sans séparateur de milliers. Le succès de l'opération est confirmé dans le journal SAS.
