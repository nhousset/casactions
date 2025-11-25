# neuralNet.annScore

> L'action `annScore` est utilisée pour évaluer (scorer) une table de données en utilisant un modèle de réseau de neurones artificiels préalablement entraîné. Elle applique le modèle stocké dans une table de modèle sur une nouvelle table de données pour générer des prédictions. Cette action est une étape cruciale dans le cycle de vie du machine learning, permettant de déployer un modèle pour faire des inférences sur de nouvelles données.

## Syntaxe
sas
neuralNet.annScore {
  assess=VRAI | FAUX,
  assessOneRow=VRAI | FAUX,
  casOut={caslib='string', compress=VRAI | FAUX, indexVars={'variable-name-1' <, 'variable-name-2', ...>}, label='string', lifetime=entier-64-bits, maxMemSize=entier-64-bits, memoryFormat='DVR' | 'INHERIT' | 'STANDARD', name='table-name', promote=VRAI | FAUX, replace=VRAI | FAUX, replication=entier, tableRedistUpPolicy='DEFER' | 'NOREDIST' | 'REBALANCE', threadBlockSize=entier-64-bits, timeStamp='string', where={'string-1' <, 'string-2', ...>}},
  copyVars={'variable-name-1' <, 'variable-name-2', ...>},
  impute=VRAI | FAUX,
  includeMissing=VRAI | FAUX,
  listNode='ALL' | 'HIDDEN' | 'INPUT' | 'OUTPUT',
  modelId='string',
  modelTable={caslib='string', computedOnDemand=VRAI | FAUX, computedVars={{format='string', formattedLength=entier, label='string', name='variable-name', nfd=entier, nfl=entier}, {...}}, computedVarsProgram='string', dataSourceOptions={key-1=any-list-or-data-type-1 <, key-2=any-list-or-data-type-2, ...>}, importOptions={fileType='ANY' | 'AUDIO' | 'AUTO' | 'BASESAS' | 'CSV' | 'DELIMITED' | 'DOCUMENT' | 'DTA' | 'ESP' | 'EXCEL' | 'FMT' | 'HDAT' | 'IMAGE' | 'JMP' | 'LASR' | 'PARQUET' | 'SOUND' | 'SPSS' | 'VIDEO' | 'XLS', fileType-specific-parameters}, name='table-name', singlePass=VRAI | FAUX, vars={{format='string', formattedLength=entier, label='string', name='variable-name', nfd=entier, nfl=entier}, {...}}, where='where-expression', whereTable={casLib='string', dataSourceOptions={adls_noreq-parameters | bigquery-parameters | cas_noreq-parameters | clouddex-parameters | db2-parameters | dnfs-parameters | esp-parameters | fedsvr-parameters | gcs_noreq-parameters | hadoop-parameters | hana-parameters | impala-parameters | informix-parameters | jdbc-parameters | mongodb-parameters | mysql-parameters | odbc-parameters | oracle-parameters | path-parameters | postgres-parameters | redshift-parameters | s3-parameters | sapiq-parameters | sforce-parameters | singlestore_standard-parameters | snowflake-parameters | spark-parameters | spde-parameters | sqlserver-parameters | ss_noreq-parameters | teradata-parameters | vertica-parameters | yellowbrick-parameters}, importOptions={fileType='ANY' | 'AUDIO' | 'AUTO' | 'BASESAS' | 'CSV' | 'DELIMITED' | 'DOCUMENT' | 'DTA' | 'ESP' | 'EXCEL' | 'FMT' | 'HDAT' | 'IMAGE' | 'JMP' | 'LASR' | 'PARQUET' | 'SOUND' | 'SPSS' | 'VIDEO' | 'XLS', fileType-specific-parameters}, name='table-name', vars={{format='string', formattedLength=entier, label='string', name='variable-name', nfd=entier, nfl=entier}, {...}}, where='where-expression'}},
  table={caslib='string', computedOnDemand=VRAI | FAUX, computedVars={{format='string', formattedLength=entier, label='string', name='variable-name', nfd=entier, nfl=entier}, {...}}, computedVarsProgram='string', dataSourceOptions={key-1=any-list-or-data-type-1 <, key-2=any-list-or-data-type-2, ...>}, importOptions={fileType='ANY' | 'AUDIO' | 'AUTO' | 'BASESAS' | 'CSV' | 'DELIMITED' | 'DOCUMENT' | 'DTA' | 'ESP' | 'EXCEL' | 'FMT' | 'HDAT' | 'IMAGE' | 'JMP' | 'LASR' | 'PARQUET' | 'SOUND' | 'SPSS' | 'VIDEO' | 'XLS', fileType-specific-parameters}, name='table-name', singlePass=VRAI | FAUX, vars={{format='string', formattedLength=entier, label='string', name='variable-name', nfd=entier, nfl=entier}, {...}}, where='where-expression', whereTable={casLib='string', dataSourceOptions={adls_noreq-parameters | bigquery-parameters | cas_noreq-parameters | clouddex-parameters | db2-parameters | dnfs-parameters | esp-parameters | fedsvr-parameters | gcs_noreq-parameters | hadoop-parameters | hana-parameters | impala-parameters | informix-parameters | jdbc-parameters | mongodb-parameters | mysql-parameters | odbc-parameters | oracle-parameters | path-parameters | postgres-parameters | redshift-parameters | s3-parameters | sapiq-parameters | sforce-parameters | singlestore_standard-parameters | snowflake-parameters | spark-parameters | spde-parameters | sqlserver-parameters | ss_noreq-parameters | teradata-parameters | vertica-parameters | yellowbrick-parameters}, importOptions={fileType='ANY' | 'AUDIO' | 'AUTO' | 'BASESAS' | 'CSV' | 'DELIMITED' | 'DOCUMENT' | 'DTA' | 'ESP' | 'EXCEL' | 'FMT' | 'HDAT' | 'IMAGE' | 'JMP' | 'LASR' | 'PARQUET' | 'SOUND' | 'SPSS' | 'VIDEO' | 'XLS', fileType-specific-parameters}, name='table-name', vars={{format='string', formattedLength=entier, label='string', name='variable-name', nfd=entier, nfl=entier}, {...}}, where='where-expression'}},
  target='variable-name'
};


## Paramètres

| Paramètre | Description |
| --- | --- |
| `assess` | Lorsque défini sur Vrai, les probabilités prédites pour les niveaux d'événement sont ajoutées à la table de résultats. Ces probabilités peuvent être utilisées avec l'action `assess`. |
| `assessOneRow` | Lorsque défini sur Vrai, les probabilités prédites pour les niveaux d'événement sont ajoutées à la table de résultats. Toutes les probabilités d'événement sont incluses en tant que colonnes séparées et sont nommées avec le préfixe _NN_P_. Ces probabilités peuvent être utilisées avec l'action `assess`. |
| `casOut` | Spécifie la table de sortie pour stocker les résultats du scoring. |
| `copyVars` | Spécifie les variables à transférer de la table d'entrée vers la table de sortie. |
| `impute` | Lorsque défini sur Vrai, les observations avec une valeur non manquante pour la variable cible sont utilisées comme valeurs prédites. Seules les observations avec des valeurs manquantes pour la variable cible sont scorées. |
| `includeMissing` | Par défaut, les observations avec des valeurs manquantes sont incluses. Lorsque défini sur Faux, toute observation avec des valeurs manquantes pour les variables utilisées dans le modèle n'est pas incluse. |
| `listNode` | Spécifie les nœuds à inclure dans la table de sortie scorée. Utile pour l'auto-encodage afin de réduire la dimensionnalité des entrées. |
| `modelId` | Spécifie le nom de la variable d'ID du modèle à utiliser lors de la génération de la table scorée. Par défaut, le nom de la variable est _NN_PredName_ pour les classifications et _NN_Pred_ pour les régressions. |
| `modelTable` | Spécifie la table qui contient le modèle de réseau de neurones artificiels à utiliser pour le scoring. |
| `table` | Spécifie la table d'entrée à scorer. |
| `target` | Spécifie la variable cible lors du scoring d'un jeu de données. Non requis si le nom de la variable cible dans le modèle est le même que dans la table à scorer. |

## Création des Données et Entraînement d'un Modèle
Avant de pouvoir utiliser `annScore`, nous devons d'abord entraîner un modèle de réseau de neurones. Ce code charge les données `iris`, les partitionne, puis entraîne un modèle simple avec l'action `annTrain`. Le modèle entraîné est stocké dans la table `my_ann_model`.
sas
proc cas; 
  loadactionset 'dataStep';
  loadactionset 'sampling';
  loadactionset 'neuralNet';

  /* 1. Charger les données */
  load data=sashelp.iris out=iris;

  /* 2. Partitionner les données */
  action sampling.srs result=p_iris / 
    table={name='iris'},
    samppct=70,
    partind=TRUE,
    seed=1234;

  /* 3. Créer les tables d'entraînement et de validation */
  action dataStep.runCode / 
    code='data train_data; set p_iris.srs; if _partind_ = 1; run; data valid_data; set p_iris.srs; if _partind_ = 0; run;';

  /* 4. Entraîner le modèle de réseau de neurones */
  action neuralNet.annTrain / 
    table={name='train_data'},
    validTable={name='valid_data'},
    target='Species',
    inputs={'SepalLength', 'SepalWidth', 'PetalLength', 'PetalWidth'},
    hiddens={50, 20},
    modelTable={name='my_ann_model', replace=true};
quit;


## Exemple de Scoring de Base
Cet exemple utilise le modèle `my_ann_model` (créé précédemment) pour scorer la table de validation `valid_data`. Les prédictions sont stockées dans une nouvelle table CAS nommée `scored_results`.
sas
proc cas; 
  loadactionset 'neuralNet';
  action neuralNet.annScore / 
    table={name='valid_data'},
    modelTable={name='my_ann_model'},
    casOut={name='scored_results', replace=true};

  /* Afficher les 5 premières lignes des résultats */
  action table.fetch / 
    table={name='scored_results'},
    to=5;
quit;


## Scoring avec Copie de Variables et Évaluation
Cet exemple plus détaillé montre comment scorer les données tout en conservant les variables d'origine (`copyVars`) et en générant les probabilités de prédiction pour chaque classe (`assessOneRow`). Cela est utile pour une analyse plus approfondie et pour évaluer la performance du modèle.
sas
proc cas;
  loadactionset 'neuralNet';
  action neuralNet.annScore / 
    table={name='valid_data'},
    modelTable={name='my_ann_model'},
    copyVars={'Species', 'SepalLength', 'PetalLength'},
    assessOneRow=true,
    casOut={name='scored_results_detailed', replace=true};

  /* Afficher les 5 premières lignes des résultats détaillés */
  action table.fetch / 
    table={name='scored_results_detailed'},
    to=5;
quit;


## Scoring pour l'Auto-encodage (Réduction de Dimension)
L'auto-encodage est une technique où un réseau de neurones est utilisé pour apprendre une représentation compressée des données. En utilisant `listNode='HIDDEN'`, nous pouvons extraire les valeurs des neurones de la couche cachée. Ces valeurs peuvent ensuite être utilisées comme de nouvelles caractéristiques (features) de dimension réduite pour d'autres modèles.
sas
proc cas;
  loadactionset 'neuralNet';
  /* Note: Un modèle d'auto-encodeur aurait été entraîné au préalable */
  /* Pour cet exemple, nous utilisons le même modèle mais illustrons le concept */
  action neuralNet.annScore / 
    table={name='valid_data'},
    modelTable={name='my_ann_model'},
    listNode='HIDDEN',
    casOut={name='autoencoded_features', replace=true};

  /* Afficher les 5 premières lignes des caractéristiques encodées */
  action table.fetch / 
    table={name='autoencoded_features'},
    to=5;
quit;

