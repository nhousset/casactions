# bart.bartGauss

> L'action `bartGauss` de l'ensemble d'actions `bart` (Bayesian Additive Regression Trees) est utilisée pour ajuster des modèles d'arbres de régression additifs bayésiens. Elle est spécifiquement conçue pour les situations où la variable de réponse suit une distribution normale. Cette méthode est un puissant algorithme d'apprentissage automatique non paramétrique qui modélise la relation entre les prédicteurs et une réponse continue en construisant un ensemble d'arbres de régression. Le modèle final est la somme des prédictions de tous les arbres, ce qui le rend robuste et capable de capturer des interactions complexes et des relations non linéaires dans les données.

## Syntaxe
sas
bart.bartGauss {
  alpha=double,
  attributes={{name='variable-name', format='string', formattedLength=integer, label='string', nfd=integer, nfl=integer}, ...},
  class={{vars={'variable-name-1', ...}, descending=TRUE|FALSE, order="FORMATTED"|"FREQ"|"FREQFORMATTED"|"FREQINTERNAL"|"INTERNAL", ref="FIRST"|"LAST"|double|"string"}, ...},
  differences={{evtMargin='string', refMargin='string', label='string', name='string'}, ...},
  display={caseSensitive=TRUE|FALSE, exclude=TRUE|FALSE, excludeAll=TRUE|FALSE, keyIsPath=TRUE|FALSE, names={'string-1', ...}, pathType="LABEL"|"NAME", traceNames=TRUE|FALSE},
  distributeChains=integer,
  freq='variable-name',
  inputs={{name='variable-name', format='string', formattedLength=integer, label='string', nfd=integer, nfl=integer}, ...},
  leafSigmaK=double,
  margins={{name='string', at={{var='string', value="string"|double}, ...}, label='string'}, ...},
  maxTrainTime=double,
  minLeafSize=integer,
  missing="MACBIG"|"MACSMALL"|"NONE"|"SEPARATE",
  model={depVars={{name='variable-name'}, ...}, effects={{vars={'string-1', ...}}, ...}},
  nBI=integer,
  nBins=integer,
  nClassLevelsPrint=integer,
  nMC=integer,
  nMCDist=integer,
  nominals={{name='variable-name', format='string', formattedLength=integer, label='string', nfd=integer, nfl=integer}, ...},
  nThin=integer,
  nTree=integer,
  obsLeafMapInMem=TRUE|FALSE,
  orderSplit=integer,
  output={casOut={caslib='string', name='table-name', ...}, alpha=double, avgOnly=TRUE|FALSE, copyVars="ALL"|"ALL_MODEL"|"ALL_NUMERIC"|{'variable-name-1', ...}, lcl='string', pred='string', resid='string', role='string', ucl='string'},
  outputMargins={caslib='string', name='table-name', ...},
  outputTables={names={'string-1', ...} | {key-1={casouttable-1}, ...}, ...},
  partByFrac={test=double, seed=integer},
  partByVar={name='variable-name', test='string', train='string'},
  quantileBin=TRUE|FALSE,
  sampleSummary={casout={caslib='string', name='table-name', ...}, avgNode='string', propAccepted='string', sampSaved='string', variance='string'},
  seed=64-bit-integer,
  sigmaDF=double,
  sigmaLambda=double,
  sigmaQuantile=double,
  store={caslib='string', name='table-name', ...},
  table={name='table-name', caslib='string', ...},
  target='variable-name',
  trainInMem=TRUE|FALSE,
  treePrior={depthBase=double, depthPower=double, pPrune=double, pSplit=double},
  varAutoCorr={integer-1, ...},
  varEst=double
};


| Paramètre | Description |
| --- | --- |
| alpha | Spécifie le niveau de significativité pour construire les limites de crédibilité pour les marges prédictives. |
| attributes | Modifie les attributs des variables utilisées dans l'action. |
| class | Désigne les variables de classification à utiliser comme variables explicatives. |
| differences | Spécifie les différences de marges prédictives à calculer. |
| display | Spécifie une liste de tables de résultats à envoyer au client pour affichage. |
| distributeChains | Spécifie un mode distribué qui divise l'échantillonnage MCMC dans un environnement de grille. |
| freq | Désigne la variable numérique contenant la fréquence d'occurrence pour chaque observation. |
| inputs | Spécifie les variables d'entrée à utiliser dans l'analyse. |
| leafSigmaK | Spécifie la valeur utilisée pour déterminer la variance a priori pour le paramètre de feuille. |
| margins | Spécifie une marge prédictive à calculer. |
| maxTrainTime | Spécifie une limite supérieure (en secondes) pour la durée de l'échantillonnage MCMC. |
| minLeafSize | Spécifie le nombre minimum d'observations que chaque enfant d'une division doit contenir. |
| missing | Spécifie comment gérer les valeurs manquantes dans les variables prédictives ('MACBIG', 'MACSMALL', 'NONE', 'SEPARATE'). |
| model | Désigne la variable dépendante et les effets explicatifs. |
| nBI | Spécifie le nombre d'itérations de burn-in (rodage) à effectuer avant de commencer à sauvegarder les échantillons. |
| nBins | Spécifie le nombre de classes (bins) à utiliser pour discrétiser les variables d'entrée continues. |
| nClassLevelsPrint | Limite l'affichage des niveaux de variables de classe. |
| nMC | Spécifie le nombre d'itérations MCMC, hors itérations de burn-in. |
| nMCDist | Spécifie le nombre d'itérations MCMC pour chaque chaîne lorsque plusieurs chaînes sont utilisées. |
| nominals | Spécifie les variables d'entrée nominales à utiliser dans l'analyse. |
| nThin | Spécifie le taux d'amincissement (thinning) de la simulation. |
| nTree | Spécifie le nombre d'arbres dans un échantillon de l'ensemble de somme d'arbres. |
| obsLeafMapInMem | Si VRAI, stocke en mémoire une table de correspondance de chaque observation aux nœuds terminaux. |
| orderSplit | Spécifie la cardinalité minimale pour laquelle une entrée catégorielle utilise des règles de division selon l'ordre des niveaux. |
| output | Crée une table contenant des statistiques par observation, calculées après l'ajustement du modèle. |
| outputMargins | Spécifie la table de sortie pour les marges prédictives. |
| outputTables | Liste les noms des tables de résultats à sauvegarder en tant que tables CAS. |
| partByFrac | Spécifie la fraction des données à utiliser pour les tests. |
| partByVar | Désigne la variable et ses valeurs utilisées pour partitionner les données en ensembles d'entraînement et de test. |
| quantileBin | Si VRAI, les limites des classes sont définies aux quantiles des entrées numériques. |
| sampleSummary | Crée une table contenant un résumé des échantillons de l'ensemble de somme d'arbres. |
| seed | Spécifie une graine pour démarrer le générateur de nombres pseudo-aléatoires. |
| sigmaDF | Spécifie les degrés de liberté de la loi a priori Scaled Inverse Chi-Square pour le paramètre de variance. |
| sigmaLambda | Spécifie le paramètre d'échelle de la loi a priori Scaled Inverse Chi-Square pour la variance. |
| sigmaQuantile | Spécifie le niveau de quantile à utiliser pour déterminer le paramètre d'échelle de la loi a priori pour la variance. |
| store | Stocke le modèle dans un objet table binaire que vous pouvez utiliser pour le scoring. |
| table | Spécifie la table de données d'entrée. |
| target | Spécifie la variable cible. |
| trainInMem | Si VRAI, stocke les données en mémoire lors de l'entraînement du modèle. |
| treePrior | Spécifie la loi a priori de régularisation pour l'ensemble de somme d'arbres. |
| varAutoCorr | Spécifie les retards d'autocorrélation pour le paramètre de variance. |
| varEst | Spécifie la valeur initiale pour la variance. |

## Création de Données de Régression
Ce bloc de code SAS crée une table CAS nommée `my_regression_data`. Cette table contient 1000 observations avec une variable dépendante `y` et 10 variables prédictives indépendantes (`x1` à `x10`). Les données sont générées de manière aléatoire pour simuler un problème de régression typique, où `y` est une combinaison linéaire des `x` avec un bruit gaussien ajouté. Cette table sera utilisée dans les exemples pour entraîner et évaluer le modèle BART.
sas
data mycas.my_regression_data;\n  call streaminit(123);\n  do i = 1 to 1000;\n    y = 10 + 5*rand('NORMAL') + 2*rand('UNIFORM');\n    x1 = rand('UNIFORM') * 10;\n    x2 = rand('UNIFORM') * 5;\n    x3 = rand('NORMAL', 0, 2);\n    x4 = rand('NORMAL', 5, 3);\n    x5 = rand('POISSON', 3);\n    x6 = rand('INTEGER', 1, 10);\n    x7 = rand('BERNOULLI', 0.3);\n    x8 = rand('BERNOULLI', 0.6);\n    x9 = rand('NORMAL', 10, 5);\n    x10 = rand('UNIFORM') * 20;\n    output;\n  end;\nrun;


## Ajustement d'un Modèle BART Gaussien de Base
Cet exemple montre comment ajuster un modèle BART de base avec les paramètres par défaut. Nous spécifions la table d'entrée `my_regression_data`, la variable cible `y` et les variables prédictives `x1` à `x10`. C'est le moyen le plus simple d'exécuter l'action `bartGauss`.
sas
proc cas;\n  bart.bartGauss / \n    table={name='my_regression_data'},\n    model={depVars={{name='y'}}, effects={{vars={'x1', 'x2', 'x3', 'x4', 'x5', 'x6', 'x7', 'x8', 'x9', 'x10'}}};\nrun;

> Le résultat affichera plusieurs tables, y compris 'Model Information' qui décrit la configuration du modèle, 'Number of Observations' qui indique le nombre d'observations lues et utilisées, et 'Analysis of Deviance' qui fournit des statistiques sur l'ajustement du modèle. Aucune table de sortie n'est créée par défaut.

## Modèle BART Détaillé avec Partitionnement, Priors et Sorties
Cet exemple avancé montre comment utiliser plusieurs options de l'action `bartGauss`. Nous partitionnons les données en 80% pour l'entraînement et 20% pour le test. Nous personnalisons les priors de l'arbre (`treePrior`) et de la variance (`sigmaDF`, `sigmaQuantile`). Le modèle est stocké dans une table CAS nommée `bart_model_store` pour un scoring ultérieur. De plus, une table de sortie `bart_predictions` est générée, contenant les valeurs prédites (`Pred_y`) et les résidus (`Resid_y`) pour chaque observation.
sas
proc cas;\n  bart.bartGauss / \n    table={name='my_regression_data'},\n    model={depVars={{name='y'}}, effects={{vars={'x1', 'x2', 'x3', 'x4', 'x5', 'x6', 'x7', 'x8', 'x9', 'x10'}}}},\n    partByFrac={test=0.2, seed=456},\n    nTree=100,\n    nBI=500,\n    nMC=2000,\n    treePrior={depthPower=1.5, pSplit=0.6, pPrune=0.4},\n    sigmaDF=5,\n    sigmaQuantile=0.95,\n    output={casOut={name='bart_predictions', replace=true}, pred='Pred_y', resid='Resid_y'},\n    store={name='bart_model_store', replace=true};\nrun;

> En plus des tables de résultats standard, cette exécution créera deux nouvelles tables dans la caslib active. La première, `bart_predictions`, contiendra les prédictions et les résidus pour chaque observation de la table d'entrée. La seconde, `bart_model_store`, est un item binaire contenant l'état du modèle ajusté, prêt à être utilisé par l'action `bart.bartScore` pour noter de nouvelles données. Les journaux indiqueront également que les données ont été partitionnées pour l'entraînement et le test.
