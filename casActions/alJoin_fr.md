# activeLearn.alJoin

> L'action `alJoin` du set d'actions `activeLearn` est un outil essentiel pour fusionner des données brutes avec leurs annotations correspondantes. Ce processus est une étape fondamentale dans de nombreux flux de travail d'apprentissage automatique, en particulier pour l'apprentissage supervisé où les étiquettes (annotations) sont nécessaires pour entraîner un modèle. L'action permet de combiner une table de données principale avec une table d'annotations en se basant sur un identifiant commun, offrant plusieurs types de jointures (interne, externe, etc.) pour s'adapter à divers scénarios de préparation de données.

## Syntaxe
sas
activeLearn.alJoin /
    annotatedTable={<table-source>}
    casOut={<table-de-sortie>}
    id="variable_id"
    [joinType="APPEND" | "FULL" | "INNER" | "LEFT" | "RIGHT"]
    [logLevel=entier]
    table={<table-source>};


## Paramètres
| Paramètre | Description |
| --- | --- |
| annotatedTable | Spécifie la table CAS en mémoire qui contient les données d'annotation à joindre. C'est la table de droite dans la jointure. |
| casOut | Spécifie la table de sortie pour stocker les résultats de la jointure. |
| id | Spécifie la colonne d'identification utilisée comme clé pour joindre la table de données et la table d'annotation. |
| joinType | Spécifie le type de jointure à effectuer. Les options sont APPEND, INNER (interne), LEFT (gauche), RIGHT (droite), et FULL (complète). La valeur par défaut est 'LEFT'. |
| logLevel | Spécifie le niveau de détail des messages de progression. 0 (par défaut) pour aucun message, 1 pour les messages de début/fin, 2 pour l'historique des itérations. |
| table | Spécifie la table CAS en mémoire qui contient les données principales à joindre. C'est la table de gauche dans la jointure. |

## Création de Données
Ce bloc de code crée deux tables dans la caslib `mycas`. La première, `data_to_annotate`, contient les données brutes avec un identifiant. La seconde, `annotations`, contient les étiquettes correspondantes pour certains des identifiants.
sas
data mycas.data_to_annotate;
  input id feature1 feature2;
  datalines;
1 10 20
2 12 22
3 15 25
4 18 28
;
run;

data mycas.annotations;
  input id $ label $;
  datalines;
1 A
2 B
3 A
;
run;


## Exemples

### Exemple de jointure à gauche (LEFT JOIN)
Cet exemple effectue une jointure à gauche par défaut (`joinType='LEFT'`) entre la table de données et la table d'annotations. Toutes les lignes de la table de gauche (`data_to_annotate`) sont conservées.
sas
proc cas;
  action activeLearn.alJoin /
    table={name='data_to_annotate'}
    annotatedTable={name='annotations'}
    id='id'
    casOut={name='joined_data_left', replace=true};
run;
quit;

> La table `joined_data_left` est créée. Elle contient toutes les lignes de `data_to_annotate` avec les étiquettes correspondantes de `annotations`. La ligne avec id=4 aura une valeur manquante pour la colonne 'label' car elle n'existe pas dans la table d'annotations, ce qui est le comportement attendu d'une jointure à gauche.

### Exemple de jointure interne (INNER JOIN)
Cet exemple utilise une jointure interne (`joinType='INNER'`) pour ne conserver que les observations qui ont une correspondance dans les deux tables. C'est utile pour créer un jeu de données d'entraînement ne contenant que des exemples étiquetés.
sas
proc cas;
  action activeLearn.alJoin /
    table={name='data_to_annotate'}
    annotatedTable={name='annotations'}
    id='id'
    joinType='INNER'
    casOut={name='joined_data_inner', replace=true};
run;
quit;

> La table `joined_data_inner` est créée. Elle ne contient que les lignes pour lesquelles un identifiant commun existe dans les deux tables (`data_to_annotate` et `annotations`). Par conséquent, la ligne avec id=4 est exclue du résultat final, car elle n'a pas d'annotation correspondante.

### Exemple de jointure complète (FULL JOIN)
Cet exemple montre comment effectuer une jointure complète (`joinType='FULL'`) pour conserver toutes les lignes des deux tables, qu'elles aient ou non une correspondance. Cela peut être utile pour un audit complet des données et des annotations.
sas
/* Supposons qu'une annotation existe pour un id non présent dans la table de données */
data mycas.annotations_extra;
  set mycas.annotations;
  output;
  id='5'; label='C'; output;
run;

proc cas;
  action activeLearn.alJoin /
    table={name='data_to_annotate'}
    annotatedTable={name='annotations_extra'}
    id='id'
    joinType='FULL'
    casOut={name='joined_data_full', replace=true};
run;
quit;

> La table `joined_data_full` est créée. Elle contient toutes les lignes des deux tables. La ligne avec id=4 aura une valeur manquante pour 'label', et une nouvelle ligne avec id=5 sera ajoutée avec des valeurs manquantes pour 'feature1' et 'feature2'.
