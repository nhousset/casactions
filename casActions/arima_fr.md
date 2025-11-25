# uniTimeSeries.arima

> L'action `arima` du jeu d'actions `uniTimeSeries` est utilisée pour l'analyse et la prévision de séries temporelles univariées à l'aide de modèles ARIMA (AutoRegressive Integrated Moving Average). Elle permet d'identifier, d'estimer et de prévoir des modèles pour des données temporelles, en prenant en compte les tendances, la saisonnalité et les composantes aléatoires. Cette action est fondamentale en économétrie et en prévision pour modéliser des phénomènes qui évoluent dans le temps.

## Syntaxe
sas
uniTimeSeries.arima result=<results> status=<rc> / 
   alignId="BEGIN" | "END" | "MIDDLE",
   auxData={{...}},
   boundaryAlign="BOTH" | "END" | "NONE" | "START",
   casOut={...},
   display={...},
   interval="string",
   nlFormat=TRUE | FALSE,
   nThreads=integer,
   outEst={...},
   outFor={...},
   outputTables={...},
   outStat={...},
   seasonality=integer,
   series={{...}, {...}},
   sumOut={...},
   table={...},
   tEnd=double | date | datetime,
   timeId={...},
   trimId="BOTH" | "LEFT" | "NONE" | "RIGHT",
   tStart=double | date | datetime;


## Paramètres

| Paramètre | Description |
| --- | --- |
| alignId | Spécifie l'alignement de l'identifiant temporel. |
| auxData | Spécifie les tables de données de séries temporelles auxiliaires. |
| boundaryAlign | Spécifie l'alignement des horodatages de début et de fin. |
| casOut | Nomme la table de données de sortie pour contenir les prévisions des variables. |
| display | Spécifie la liste des tables d'affichage que vous souhaitez que l'action crée. |
| interval | Spécifie l'intervalle de temps (ou la fréquence) de la série. |
| nlFormat | Si VRAI, choisit le meilleur format international pour la variable d'horodatage en fonction de l'intervalle de temps d'accumulation. |
| nThreads | Spécifie le nombre de threads à utiliser par nœud de travail dans une session CAS. |
| outEst | Nomme la table de données de sortie pour contenir les estimations des paramètres du modèle et les statistiques de test associées. |
| outFor | Nomme la table de données de sortie pour contenir les composantes de la série temporelle de prévision (réel, prédit, limites de confiance inférieure et supérieure, erreur de prédiction, erreur standard de prédiction). |
| outputTables | Spécifie la liste des tables d'affichage que vous souhaitez sortir en tant que tables CAS. |
| outStat | Nomme la table de données de sortie pour contenir les statistiques d'ajustement. |
| seasonality | Spécifie le nombre de périodes de temps par cycle saisonnier. |
| series | Spécifie le nom de la série à modéliser et les options de modélisation. |
| sumOut | Nomme la table de données de sortie pour contenir les statistiques récapitulatives et la sommation des prévisions. |
| table | Spécifie la table de données d'entrée. |
| tEnd | Spécifie la fin de la fenêtre temporelle. |
| timeId | Spécifie la variable d'horodatage. |
| trimId | Spécifie comment découper la série temporelle en fonction des valeurs manquantes. |
| tStart | Spécifie le début de la fenêtre temporelle. |

## Création de Données Temporelles
Ce code SAS crée une table CAS nommée `series_data` contenant une série temporelle simple. La série a une variable `date` qui représente le temps et une variable `valeur` qui contient les observations. Ces données sont utilisées pour illustrer comment appliquer un modèle ARIMA.
sas
data casuser.series_data;\n   do i = 1 to 100;\n      date = intnx('month', '01jan2020'd, i-1);\n      valeur = 50 + 2*i + 10*sin(i/6) + rannor(12345);\n      output;\n   end;\n   format date monyy.;\nrun;


## Exemples

## Modélisation ARIMA de base
Cet exemple exécute un modèle ARIMA simple sur la série `valeur`. Il utilise une différenciation d'ordre 1 et un modèle autorégressif d'ordre 1 (p=1) et à moyenne mobile d'ordre 1 (q=1).
sas
proc cas;\n   uniTimeSeries.arima table={name='series_data'},\n      timeId={name='date'},\n      interval='month',\n      series={{name='valeur', model={{estimate={p=1, q=1, diff=1}}}}};\nrun;


## Modèle ARIMA Saisonnier avec Prévisions
Cet exemple ajuste un modèle ARIMA saisonnier plus complexe. Il inclut une différenciation saisonnière (sDiff=12), des termes autorégressifs et à moyenne mobile saisonniers (sP=1, sQ=1), et demande 24 périodes de prévision. Les résultats des prévisions sont stockés dans la table `arima_forecasts` et les estimations des paramètres dans `arima_estimates`.
sas
proc cas;\n   uniTimeSeries.arima table={name='series_data'},\n      timeId={name='date'},\n      interval='month',\n      seasonality=12,\n      series={{name='valeur', model={{estimate={p=1, q=1, diff=1, sP=1, sQ=1, sDiff=12, method='ML'}, forecast={lead=24, alpha=0.95}}}},\n      outFor={name='arima_forecasts', replace=true},\n      outEst={name='arima_estimates', replace=true};\nrun;

