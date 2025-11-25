# dataSciencePilot.analyzeMissingPatterns

> L'action `analyzeMissingPatterns` effectue une analyse des modèles de valeurs manquantes dans un jeu de données. Elle est utile pour comprendre la nature et la structure des données manquantes, ce qui est une étape cruciale dans la préparation des données pour le machine learning. Cette action peut identifier les combinaisons de variables qui ont souvent des valeurs manquantes ensemble, et fournir des statistiques sur la fréquence de ces modèles.

## Syntaxe
sas
dataSciencePilot.analyzeMissingPatterns {
  casOut={CASOUTTABLE},
  distinctCountLimit=integer,
  ecdfTolerance=double,
  freq="variable-name",
  inputs={{CASINVARDESC-1} <, {CASINVARDESC-2}, ...>},
  misraGries=TRUE | FALSE,
  nominals={"variable-name-1" <, "variable-name-2", ...>},
  table={CASTABLE},
  target="variable-name"
};


## Paramètres

| Paramètre | Description |
| --- | --- |
| casOut | Spécifie la table CAS pour stocker les résultats de l'analyse. |
| distinctCountLimit | Spécifie la limite de comptage distinct. Si la limite est dépassée et que le paramètre `misraGries` est à True, l'algorithme de sketch de fréquence Misra-Gries est utilisé pour estimer la distribution de fréquence. Sinon, l'opération de comptage distinct est abandonnée. |
| ecdfTolerance | Spécifie la valeur de tolérance pour la fonction de distribution cumulative empirique. Cette valeur est utilisée par l'algorithme de sketch de quantile. |
| freq | Spécifie la variable de fréquence. |
| inputs | Spécifie les variables à utiliser pour l'analyse. Vous pouvez spécifier un sous-ensemble des variables de la table d'entrée. |
| misraGries | Lorsque défini sur True, utilise l'algorithme Misra-Gries pour l'estimation de la distribution de fréquence si la limite de comptage distinct est dépassée. |
| nominals | Spécifie les variables nominales. |
| table | Spécifie la table d'entrée contenant les données à analyser. |
| target | Spécifie la variable cible. |

## Création d'un jeu de données avec des valeurs manquantes

Ce code crée une table CAS nommée `hmeq_missing` avec des valeurs manquantes intentionnelles pour illustrer l'analyse des modèles de valeurs manquantes. La table est basée sur la table `sampsio.hmeq`.
sas
data casuser.hmeq_missing;
  set sampsio.hmeq;
  if _n_ in (1, 10, 20) then call missing(of _all_);
  if 5 <= _n_ <= 15 then call missing(yoj, derog);
  if 30 <= _n_ <= 40 then call missing(value, reason);
run;


## Exemples

## Analyse de base des modèles de valeurs manquantes
Cet exemple exécute une analyse de base des modèles de valeurs manquantes sur la table `hmeq_missing` et stocke les résultats dans une table nommée `missing_patterns_analysis`.
sas
proc cas;
  dataSciencePilot.analyzeMissingPatterns
    table={name='hmeq_missing'},
    casOut={name='missing_patterns_analysis', replace=true};
run;
quit;


## Analyse détaillée avec spécification des variables et de la cible
Cet exemple effectue une analyse des modèles de valeurs manquantes en se concentrant sur un sous-ensemble de variables d'entrée (`inputs`) et en spécifiant une variable cible (`target`). Cela permet d'analyser comment les valeurs manquantes dans les prédicteurs se rapportent à la variable de résultat.
sas
proc cas;
  dataSciencePilot.analyzeMissingPatterns
    table={name='hmeq_missing'},
    target='bad',
    inputs={'loan', 'mortdue', 'value', 'yoj', 'derog', 'delinq', 'clage', 'ninq', 'clno', 'debtinc'},
    nominals={'reason', 'job'},
    casOut={name='detailed_missing_analysis', replace=true, caslib='casuser'};
run;
quit;


## Utilisation des limites pour les grands jeux de données
Pour les très grands jeux de données avec une haute cardinalité, il est efficace d'utiliser les paramètres `distinctCountLimit` et `misraGries`. Cet exemple montre comment limiter le comptage distinct et utiliser l'algorithme Misra-Gries pour une estimation efficace.
sas
proc cas;
  dataSciencePilot.analyzeMissingPatterns
    table={name='hmeq_missing'},
    distinctCountLimit=5000,
    misraGries=true,
    casOut={name='large_data_missing_analysis', replace=true};
run;
quit;

