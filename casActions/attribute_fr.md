# table.attribute

> L'action `attribute` du jeu d'actions `table` est un outil puissant pour gérer les métadonnées étendues associées aux tables en mémoire dans CAS. Elle permet d'ajouter, de mettre à jour, de supprimer, d'exporter ou de convertir des attributs personnalisés pour une table entière ou pour des colonnes spécifiques. Ces attributs, stockés sous forme de paires clé-valeur, sont essentiels pour enrichir les données avec du contexte supplémentaire, comme des informations de version, des descriptions de variables, des règles de validation ou des métadonnées métier, sans altérer la structure des données elles-mêmes.

## Syntaxe
sas
table.attribute <result=results> <status=rc> / 
   attributes={{casattribute-1} <, {casattribute-2}, ...>},
   caslib="string",
   name="string",
   set="string",
   table="string",
   task="ADD" | "CONVERT" | "DROP" | "EXPORT" | "UPDATE",
   xml="string",
   xmlPath="string";


| Paramètre | Description |
|---|---|
| attributes | Spécifie les attributs étendus à gérer. Vous devez spécifier le paramètre `set` si vous utilisez ce paramètre. |
| caslib | Spécifie la caslib cible pour la table d'attributs étendus. |
| name | Spécifie le nom de la table en mémoire à laquelle les attributs sont associés. |
| set | Spécifie le nom de l'ensemble d'attributs étendus. |
| table | Spécifie le nom d'une table d'attributs étendus existante à utiliser avec une tâche ADD, UPDATE ou CONVERT. |
| task | Spécifie la tâche à effectuer (ADD, CONVERT, DROP, EXPORT, UPDATE). La valeur par défaut est ADD. |
| xml | Spécifie les attributs étendus sous forme de document XML. |
| xmlPath | Spécifie le chemin d'accès à un fichier contenant les attributs étendus sous forme de document XML. |

## Création de la table de données
Ce code crée une table nommée `CARS_ATTRIBUTES` dans la caslib `casuser`. Cette table sera utilisée dans les exemples pour démontrer la gestion des attributs étendus.
sas
data casuser.cars_attributes;
  set sashelp.cars;
run;


## Ajouter un attribut simple à une table
Cet exemple montre comment ajouter un attribut de table simple, 'Version', avec la valeur '1.0', à la table `CARS_ATTRIBUTES`.
sas
proc cas;
   table.attribute / 
      name='CARS_ATTRIBUTES',
      caslib='casuser',
      task='ADD',
      attributes={{key='Version', value='1.0'}};
run;
quit;


## Gestion complète des attributs (Ajout, Mise à jour, Suppression)
Cet exemple détaillé illustre un cycle de vie complet de la gestion des attributs. Il commence par ajouter un attribut de table ('Source') et un attribut de colonne ('Description' pour la colonne 'MPG_City'). Ensuite, il met à jour la valeur de l'attribut de table, puis supprime l'attribut de colonne.
sas
proc cas;
   /* Étape 1: Ajouter des attributs de table et de colonne */
   table.attribute / 
      name='CARS_ATTRIBUTES',
      caslib='casuser',
      task='ADD',
      attributes=[
         {key='Source', value='Internal Data Feed'},
         {key='Description', value='Miles Per Gallon (City)', column='MPG_City'}
      ];

   /* Étape 2: Mettre à jour un attribut de table */
   table.attribute / 
      name='CARS_ATTRIBUTES',
      caslib='casuser',
      task='UPDATE',
      attributes={{key='Source', value='Validated Internal Data Feed'}};

   /* Étape 3: Supprimer un attribut de colonne */
   table.attribute / 
      name='CARS_ATTRIBUTES',
      caslib='casuser',
      task='DROP',
      attributes={{key='Description', column='MPG_City'}};
run;
quit;


## Exporter les attributs d'une table
Après avoir ajouté des attributs, cette action permet de les exporter et de les visualiser. L'exemple ajoute deux attributs puis utilise la tâche 'EXPORT' pour afficher les attributs existants pour la table `CARS_ATTRIBUTES`.
sas
proc cas;
   table.attribute / 
      name='CARS_ATTRIBUTES',
      caslib='casuser',
      task='ADD',
      attributes=[
         {key='Owner', value='Data Science Team'},
         {key='Confidentiality', value='Internal'}
      ];

   table.attribute / 
      name='CARS_ATTRIBUTES',
      caslib='casuser',
      task='EXPORT';
run;
quit;

