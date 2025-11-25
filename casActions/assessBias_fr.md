# fairAITools.assessBias

> L'action `assessBias` du jeu d'actions `fairAITools` est un outil essentiel pour évaluer l'équité des modèles prédictifs. Elle calcule un ensemble complet de métriques de biais, permettant aux data scientists d'identifier et de quantifier les disparités de performance du modèle entre différents sous-groupes démographiques. Cette évaluation est cruciale pour garantir que les modèles ne perpétuent pas ou n'amplifient pas les biais existants, favorisant ainsi le développement d'une IA responsable et éthique.

## Syntaxe
sas
fairAITools.assessBias result=<results> status=<rc> /
    table={name='<table-name>' <caslib='caslib'> <where='where-expression'>},
    response={name='<variable-name>'},
    sensitiveVariable={name='<variable-name>'},\n    predictedVariables={{name='<variable-name>'} <, {...}>},
    <referenceLevel='string'>,
    <event='string'>,
    <cutoff=double>;


## Paramètres
| Paramètre | Description |
| --- | --- |
| `table` | Spécifie la table de données d'entrée à analyser. |
| `response` | Spécifie la variable de réponse (cible) réelle. |
| `sensitiveVariable` | Spécifie la variable sensible (par exemple, le genre, l'ethnie) pour laquelle le biais doit être évalué. |
| `predictedVariables` | Spécifie la liste des variables contenant les prédictions du modèle. |
| `referenceLevel` | Spécifie le niveau de référence pour la variable sensible, qui servira de base de comparaison. |
| `event` | Spécifie la valeur de l'événement d'intérêt dans la variable de réponse. |
| `cutoff` | Spécifie le seuil de classification pour convertir les probabilités prédites en décisions binaires. |
| `modelTable` | Spécifie la table contenant le modèle à évaluer (ASTORE ou code DATA step). |
| `scoredTable` | Spécifie la table de sortie pour stocker les résultats du scoring. |
| `code` | Spécifie le code DATA step ou DS2 pour le scoring. |
| `frequency` | Spécifie la variable de fréquence. |
| `modelTables` | Spécifie les tables de modèle lorsque plusieurs sont nécessaires. |
| `modelTableType` | Spécifie le type de table de modèle (ASTORE, DATASTEP, NONE). |
| `nBins` | Spécifie le nombre de bins pour les calculs de lift. |
| `responseLevels` | Spécifie la liste des niveaux de la variable de réponse. |
| `rocStep` | Spécifie la taille du pas pour les calculs de la courbe ROC. |
| `selectionDepth` | Spécifie la profondeur de sélection pour les calculs de lift. |
| `weight` | Spécifie la variable de pondération. |

## Création de Données
Ce bloc de code crée une table CAS nommée `HMEQ_PRED` qui servira d'entrée pour l'évaluation de biais. Elle contient des informations sur des prêts immobiliers, incluant la variable cible `BAD`, une variable sensible `RACE`, et les probabilités prédites par un modèle (`P_BAD1`).
sas
data casuser.HMEQ_PRED;
  set sampsio.hmeq;
  if _n_=1 then call streaminit(123);
  P_BAD1 = rand('UNIFORM');
  P_BAD0 = 1 - P_BAD1;
run;


## Évaluation de biais de base
Cet exemple montre comment effectuer une évaluation de biais simple en utilisant l'action `assessBias`. Il spécifie la table d'entrée, la variable de réponse, la variable sensible et la variable prédite pour calculer les métriques de biais par défaut.
sas
proc cas;
  fairAITools.assessBias
    table={name='HMEQ_PRED'},
    response={name='BAD'},
    sensitiveVariable={name='RACE'},
    predictedVariables={{name='P_BAD1'}};
run;


## Évaluation de biais détaillée avec niveau de référence et seuil personnalisé
Cet exemple plus avancé démontre comment affiner l'évaluation de biais. Il définit 'White' comme niveau de référence pour la variable sensible `RACE`, spécifie '1' comme l'événement d'intérêt pour la variable `BAD`, et utilise un seuil de classification (`cutoff`) de 0.4. Cela permet une analyse plus ciblée et contextuelle des biais du modèle.
sas
proc cas;
  fairAITools.assessBias
    table={name='HMEQ_PRED'},
    response={name='BAD'},
    sensitiveVariable={name='RACE'},
    predictedVariables={{name='P_BAD1'}},
    referenceLevel='White',
    event='1',
    cutoff=0.4;
run;

