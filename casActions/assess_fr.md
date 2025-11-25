# percentile.assess

> L'action `assess` du jeu d'actions `percentile` est un outil puissant pour évaluer et comparer les performances de modèles prédictifs. Elle est particulièrement utile dans les scénarios de classification binaire et de régression. Pour les modèles de classification, elle calcule des statistiques d'ajustement, génère des courbes ROC (Receiver Operating Characteristic) et des courbes de lift, qui sont essentielles pour comprendre la capacité du modèle à discriminer les classes. Pour les modèles de régression, elle fournit des métriques d'erreur pour évaluer la précision des prédictions. Cette action permet une analyse fine en supportant la pondération des observations, le traitement par groupe (`groupBy`) et l'évaluation sur des partitions de données spécifiques, ce qui en fait un outil flexible pour la validation de modèles dans SAS Viya.

## Syntaxe
sas
percentile.assess result=<results> status=<rc> /
  attributes={{name="variable-name", format="string", formattedLength=integer, label="string", nfd=integer, nfl=integer}, ...}
  binNum=64-bit-integer
  casOut={caslib="string", compress=TRUE|FALSE, indexVars={"variable-name-1", ...}, label="string", lifetime=64-bit-integer, maxMemSize=64-bit-integer, memoryFormat="DVR"|"INHERIT"|"STANDARD", name="table-name", promote=TRUE|FALSE, replace=TRUE|FALSE, replication=integer, tableRedistUpPolicy="DEFER"|"NOREDIST"|"REBALANCE", threadBlockSize=64-bit-integer, timeStamp="string", where={"string-1", ...}}
  cutStep=double
  epsilon=double
  event="string"
  fitStatOut={caslib="string", compress=TRUE|FALSE, indexVars={"variable-name-1", ...}, label="string", lifetime=64-bit-integer, maxMemSize=64-bit-integer, memoryFormat="DVR"|"INHERIT"|"STANDARD", name="table-name", promote=TRUE|FALSE, replace=TRUE|FALSE, replication=integer, tableRedistUpPolicy="DEFER"|"NOREDIST"|"REBALANCE", threadBlockSize=64-bit-integer, timeStamp="string", where={"string-1", ...}}
  freq="variable-name"
  groupByLimit=64-bit-integer
  includeCutoffOne=TRUE|FALSE
  includeFitStat=TRUE|FALSE
  includeLift=TRUE|FALSE
  includeRoc=TRUE|FALSE
  includeZeroDepth=TRUE|FALSE
  inputs={{name="variable-name", format="string", formattedLength=integer, label="string", nfd=integer, nfl=integer}, ...}
  maxIters=integer
  method="EXACT"|"ITERATIVE"
  nBins=integer
  noMissingTarget=TRUE|FALSE
  partition=TRUE|FALSE
  partKey={"string-1", ...}
  pEvent={"string-1", ...}
  pResponse="variable-name"
  pVar={"variable-name-1", ...}
  response="variable-name"
  responseFmt="string"
  rocOut={caslib="string", compress=TRUE|FALSE, indexVars={"variable-name-1", ...}, label="string", lifetime=64-bit-integer, maxMemSize=64-bit-integer, memoryFormat="DVR"|"INHERIT"|"STANDARD", name="table-name", promote=TRUE|FALSE, replace=TRUE|FALSE, replication=integer, tableRedistUpPolicy="DEFER"|"NOREDIST"|"REBALANCE", threadBlockSize=64-bit-integer, timeStamp="string", where={"string-1", ...}}
  table={caslib="string", computedOnDemand=TRUE|FALSE, computedVars={{name="variable-name", format="string", formattedLength=integer, label="string", nfd=integer, nfl=integer}, ...}, computedVarsProgram="string", dataSourceOptions={key-1=any-list-or-data-type-1, ...}, groupBy={{name="variable-name", format="string", formattedLength=integer, label="string", nfd=integer, nfl=integer}, ...}, groupByMode="NOSORT"|"REDISTRIBUTE", importOptions={fileType="ANY"|"AUDIO"|"AUTO"|"BASESAS"|"CSV"|"DELIMITED"|"DOCUMENT"|"DTA"|"ESP"|"EXCEL"|"FMT"|"HDAT"|"IMAGE"|"JMP"|"LASR"|"PARQUET"|"SOUND"|"SPSS"|"VIDEO"|"XLS", fileType-specific-parameters}, name="table-name", orderBy={{name="variable-name", format="string", formattedLength=integer, label="string", nfd=integer, nfl=integer}, ...}, singlePass=TRUE|FALSE, where="where-expression", whereTable={casLib="string", dataSourceOptions={adls_noreq-parameters | bigquery-parameters | cas_noreq-parameters | clouddex-parameters | db2-parameters | dnfs-parameters | esp-parameters | fedsvr-parameters | gcs_noreq-parameters | hadoop-parameters | hana-parameters | impala-parameters | informix-parameters | jdbc-parameters | mongodb-parameters | mysql-parameters | odbc-parameters | oracle-parameters | path-parameters | postgres-parameters | redshift-parameters | s3-parameters | sapiq-parameters | sforce-parameters | singlestore_standard-parameters | snowflake-parameters | spark-parameters | spde-parameters | sqlserver-parameters | ss_noreq-parameters | teradata-parameters | vertica-parameters | yellowbrick-parameters}, importOptions={fileType="ANY"|"AUDIO"|"AUTO"|"BASESAS"|"CSV"|"DELIMITED"|"DOCUMENT"|"DTA"|"ESP"|"EXCEL"|"FMT"|"HDAT"|"IMAGE"|"JMP"|"LASR"|"PARQUET"|"SOUND"|"SPSS"|"VIDEO"|"XLS", fileType-specific-parameters}, name="table-name", vars={{name="variable-name", format="string", formattedLength=integer, label="string", nfd=integer, nfl=integer}, ...}, where="where-expression"}}
  useRawPResponse=TRUE|FALSE
  userCutoff=double
  weight="variable-name";


| Paramètre | Description |
| --- | --- |
| attributes | Spécifie les attributs temporaires, tels qu'un format, à appliquer aux variables d'entrée. |
| binNum | Nombre de bins dans un processus en trois passes. |
| casOut | Spécifie les paramètres de la table de sortie pour les statistiques d'évaluation. |
| cutStep | Spécifie la taille du pas à utiliser pour les calculs ROC. |
| epsilon | Spécifie la tolérance utilisée pour déterminer la convergence de l'algorithme itératif pour le calcul des percentiles. |
| event | Spécifie la valeur formatée de la variable de réponse qui représente l'événement. Si non spécifié pour une variable numérique, une évaluation de régression est effectuée. |
| fitStatOut | Spécifie la table de sortie pour les statistiques d'ajustement. Pour les variables de réponse nominales, les événements et variables de probabilité doivent être spécifiés. |
| freq | Spécifie la variable de fréquence d'une observation. |
| groupByLimit | Spécifie le nombre maximum de niveaux dans un ensemble de regroupement pour éviter de créer des ensembles de résultats trop volumineux. |
| includeCutoffOne | Si TRUE, inclut une ligne pour cutoff=1 dans les statistiques ROC pour simplifier le traçage de la courbe. Ignoré si la variable cible est un intervalle. |
| includeFitStat | Si FALSE, les statistiques d'ajustement ne sont pas générées. |
| includeLift | Si FALSE, les calculs de lift ne sont pas générés. |
| includeRoc | Si FALSE, les calculs ROC ne sont pas générés. |
| includeZeroDepth | Si TRUE, inclut une ligne pour depth=0 dans les statistiques de lift pour simplifier le traçage de la courbe de lift. |
| inputs | Spécifie les variables d'entrée à utiliser dans l'analyse. |
| maxIters | Spécifie le nombre maximum d'itérations de l'algorithme. |
| method | Spécifie l'algorithme pour l'analyse des percentiles (EXACT ou ITERATIVE). |
| nBins | Spécifie le nombre de bins à utiliser pour les calculs de lift. |
| noMissingTarget | Exclut les observations où la variable cible a une valeur manquante. |
| partition | Si TRUE et que la table est partitionnée, les résultats sont calculés efficacement pour chaque partition. |
| partKey | Spécifie une clé de partition pour calculer les résultats sur une seule partition spécifique. |
| pEvent | Spécifie les événements correspondant à chaque variable de probabilité, à l'exclusion de l'événement de la variable de réponse. |
| pResponse | Spécifie la variable de réponse prédite pour l'évaluation du modèle. |
| pVar | Spécifie les variables de probabilité d'événement. Doit être utilisé avec le paramètre pEvent. |
| response | Spécifie la variable de réponse pour l'évaluation du modèle. (Requis) |
| responseFmt | Spécifie un format temporaire pour la variable de réponse afin de produire l'événement spécifié. |
| rocOut | Spécifie les paramètres de la table de sortie pour les calculs ROC. |
| table | Spécifie les paramètres de la table d'entrée. (Requis) |
| useRawPResponse | Si TRUE, les valeurs brutes de la variable de réponse prédite sont utilisées pour filtrer les observations. |
| userCutoff | Seuil de classification spécifié par l'utilisateur pour la matrice de confusion. |
| weight | Spécifie la variable de poids d'une observation. |

## Création de Données pour l'Évaluation de Modèle
Ce bloc de code SAS crée une table CAS nommée `hmeq_scored`. Il charge d'abord la table `hmeq` depuis la librairie `Sampsio`, puis la charge dans CAS. Ensuite, un modèle de régression logistique est entraîné pour prédire la variable `BAD` en fonction d'autres variables. Enfin, le modèle est utilisé pour scorer la table `hmeq` et stocker les probabilités prédites dans `hmeq_scored`, qui servira de base pour l'évaluation avec l'action `assess`.
sas
data casuser.hmeq_scored; 
  set sampsio.hmeq;
run;

proc casutil;
  load data=casuser.hmeq_scored outcaslib='casuser' replace;
quit;

proc logistic data=casuser.hmeq_scored;
  class reason job;
  model bad(event='1') = reason job derog delinq clage ninq clno debtinc;
  output out=casuser.hmeq_scored p=p_bad1;
quit;


## Exemples

### Évaluation de base d'un modèle de classification
Cet exemple montre comment utiliser l'action `assess` pour évaluer un modèle de classification binaire. Il utilise la table `hmeq_scored` où `BAD` est la variable de réponse réelle et `P_BAD1` est la probabilité prédite de l'événement '1'. L'action générera des statistiques de lift et ROC par défaut.
sas
proc cas;
  percentile.assess table='hmeq_scored', response='BAD', event='1', pVar={'P_BAD1'};
run;


### Évaluation d'un modèle de régression
Ici, l'action `assess` est utilisée pour évaluer un modèle de régression. La variable `DEBTINC` est la réponse réelle et `P_BAD1` (utilisé ici comme une prédiction continue) est la réponse prédite. Comme le paramètre `event` n'est pas spécifié, l'action calcule des statistiques d'erreur de régression.
sas
proc cas;
  percentile.assess table='hmeq_scored', response='DEBTINC', pResponse='P_BAD1';
run;


### Évaluation complète avec tables de sortie ROC et Fit Statistics
Cet exemple réalise une évaluation complète d'un modèle de classification. Il spécifie la variable de réponse `BAD`, l'événement d'intérêt '1', et la probabilité prédite `P_BAD1`. De plus, il demande la création de deux tables de sortie : `hmeq_roc` pour les données de la courbe ROC et `hmeq_fit` pour les statistiques d'ajustement détaillées. Ces tables peuvent ensuite être utilisées pour des visualisations ou des analyses plus poussées.
sas
proc cas;
  percentile.assess 
    table={name='hmeq_scored'},
    response='BAD',
    event='1',
    pVar={'P_BAD1'},
    rocOut={name='hmeq_roc', replace=true},
    fitStatOut={name='hmeq_fit', replace=true};
run;


### Évaluation avec personnalisation des courbes ROC et de Lift
Cet exemple personnalise les calculs de ROC et de lift. `cutStep=0.05` augmente la granularité de la courbe ROC. `nBins=50` augmente le nombre de quantiles pour les calculs de la courbe de lift, offrant une vue plus détaillée. `includeCutoffOne=true` et `includeZeroDepth=true` ajoutent des points de départ aux tables ROC et de lift respectivement, ce qui facilite leur tracé.
sas
proc cas;
  percentile.assess 
    table='hmeq_scored', 
    response='BAD', 
    event='1', 
    pVar={'P_BAD1'}, 
    cutStep=0.05, 
    nBins=50,
    includeCutoffOne=true,
    includeZeroDepth=true,
    casOut={name='hmeq_lift', replace=true};
run;


### Évaluation de modèle par groupe avec la variable JOB
Cette analyse évalue la performance du modèle séparément pour chaque catégorie de la variable `JOB`. L'option `groupBy={'JOB'}` segmente les données, permettant de comparer comment le modèle se comporte pour différents segments de la population (par exemple, par profession). Cela est crucial pour détecter d'éventuels biais ou des variations de performance du modèle.
sas
proc cas;
  percentile.assess 
    table={name='hmeq_scored', groupBy={'JOB'}},
    response='BAD',
    event='1',
    pVar={'P_BAD1'},
    fitStatOut={name='hmeq_fit_by_job', replace=true};
run;

