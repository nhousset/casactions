# table.append

> Ajoute les lignes d'une table source à la fin d'une table cible en mémoire. Cette action est particulièrement utile pour consolider des données provenant de différentes sources ou pour agréger des lots de données dans une table principale sans avoir à recréer entièrement la table.

## Syntaxe
sas
table.append <result=results> <status=rc> / 
   source={
      caslib="string",
      dataSourceOptions={key-1=any-list-or-data-type-1 <, key-2=any-list-or-data-type-2, ...>},
      name="table-name",
      singlePass=TRUE | FALSE,
      where="where-expression"
   },
   target={
      caslib="string",
      name="table-name"
   };


## Paramètres

| Paramètre | Description |
| --- | --- |
| **source** | Spécifie la table contenant les lignes à ajouter. |
| **target** | Spécifie la table en mémoire à laquelle les lignes de la table source seront ajoutées. |
| **caslib** | Spécifie la caslib de la table. Si omis, la caslib active est utilisée. |
| **dataSourceOptions** | Spécifie les options de la source de données, utiles pour les types de fichiers qui ne sont pas au format natif SASHDAT. |
| **name** | Spécifie le nom de la table. |
| **singlePass** | Si TRUE, la table source n'est pas chargée comme une table transitoire sur le serveur. Cela peut améliorer les performances pour les sources de données de base de données, mais l'ordre des données peut ne pas être stable entre les exécutions. |
| **where** | Spécifie une expression pour filtrer les lignes de la table source avant de les ajouter à la table cible. |

## Création des tables pour l'exemple
Ce bloc de code crée deux tables en mémoire dans la caslib 'casuser'. La table 'my_table_target' est la table de destination initiale, et 'my_table_source' contient les nouvelles données à ajouter.
sas
data casuser.my_table_target(promote=true); 
  id=1; value='A'; 
  output; 
  id=2; value='B'; 
  output; 
run; 

data casuser.my_table_source(promote=true); 
  id=3; value='C'; 
  output; 
  id=4; value='D'; 
  output; 
run;


## Exemples

### Ajout simple de toutes les lignes
Cet exemple montre comment ajouter toutes les lignes de la table 'my_table_source' à la fin de la table 'my_table_target'.
sas
proc cas; 
  table.append / 
    source={name='my_table_source', caslib='casuser'}, 
    target={name='my_table_target', caslib='casuser'}; 
run; 
quit;

L'opération réussit. La table 'my_table_target' contient maintenant 4 lignes : les 2 lignes originales plus les 2 lignes de 'my_table_source'.

### Ajout conditionnel avec une clause 'where'
Cet exemple illustre comment ajouter un sous-ensemble de données. Seules les lignes de 'my_table_source' où la colonne 'id' est égale à 4 sont ajoutées à 'my_table_target'.
sas
proc cas; 
  table.append / 
    source={name='my_table_source', caslib='casuser', where='id=4'}, 
    target={name='my_table_target', caslib='casuser'}; 
run; 
quit;

L'opération réussit. La table 'my_table_target' contient maintenant 3 lignes : les 2 lignes originales plus la ligne de 'my_table_source' qui satisfait la condition 'id=4'.
