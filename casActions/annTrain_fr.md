# neuralNet.annTrain

> L'action `annTrain` du jeu d'actions `neuralNet` est un outil puissant pour entraîner des réseaux de neurones artificiels (ANN) dans SAS Viya. Elle permet de construire et d'optimiser des modèles prédictifs pour des tâches de classification et de régression. Cette action supporte diverses architectures, y compris les perceptrons multicouches (MLP) et les modèles linéaires généralisés (GLIM), offrant une flexibilité pour modéliser des relations complexes dans les données. Elle intègre des fonctionnalités avancées telles que la régularisation, le dropout, et plusieurs algorithmes d'optimisation (SGD, L-BFGS, ADAM) pour améliorer la performance et éviter le surajustement. L'action peut également générer du code de scoring SAS pour déployer facilement le modèle entraîné.

## Syntaxe
sas
neuralNet.annTrain {
  acts={"EXP", "IDENTITY", "LOGISTIC", "RECTIFIER", "SIN", "SOFTPLUS", "TANH"},
  applyRowOrder=TRUE | FALSE,
  arch="DIRECT" | "GLIM" | "MLP",
  attributes={{...}, ...},
  bias=double,
  casOut={...},
  code={...},
  combs={"ADD", "LINEAR", "RADIAL"},
  delta=double,
  dropOut=double,
  dropOutInput=double,
  errorFunc="ENTROPY" | "GAMMA" | "NORMAL" | "POISSON",
  freq="variable-name",
  fullWeights=TRUE | FALSE,
  hiddens={64-bit-integer-1, ...},
  includeBias=TRUE | FALSE,
  inputs={{...}, ...},
  inversePriors=TRUE | FALSE,
  listNode="ALL" | "HIDDEN" | "INPUT" | "OUTPUT",
  missing="MAX" | "MEAN" | "MIN" | "NONE",
  modelId="string",
  modelTable={...},
  nAnns=64-bit-integer,
  nloOpts={...},
  nominals={{...}, ...},
  nTries=64-bit-integer,
  randDist="CAUCHY" | "MSRA" | "NORMAL" | "UNIFORM" | "XAVIER",
  resume=TRUE | FALSE,
  samplingRate=double,
  saveState={...},
  scaleInit=64-bit-integer,
  seed=double,
  std="MIDRANGE" | "NONE" | "STD",
  step=double,
  t=double,
  table={...},
  target="variable-name",
  targetAct="EXP" | "IDENTITY" | "LOGISTIC" | "SIN" | "SOFTMAX" | "TANH",
  targetComb="ADD" | "LINEAR" | "RADIAL",
  targetMissing="MAX" | "MEAN" | "MIN" | "NONE",
  targetStd="MIDRANGE" | "NONE" | "STD",
  validTable={...},
  weight="variable-name"
};


| Paramètre | Description |
| --- | --- |
| acts | Spécifie la fonction d'activation pour les neurones de chaque couche cachée. |
| applyRowOrder | Spécifie si l'action doit utiliser un ordre de lignes pré-spécifié pour la reproductibilité. |
| arch | Spécifie l'architecture du réseau à entraîner (par exemple, MLP, GLIM). |
| attributes | Spécifie les attributs temporaires, comme un format, à appliquer aux variables d'entrée. |
| bias | Spécifie une valeur de biais fixe pour tous les neurones cachés et de sortie. Dans ce cas, les paramètres de biais sont fixes et non optimisés. |
| casOut | Spécifie la table de sortie pour stocker les poids du modèle entraîné. |
| code | Demande à l'action de produire du code de scoring SAS pour le déploiement du modèle. |
| combs | Spécifie la fonction de combinaison pour les neurones de chaque couche cachée. |
| delta | Spécifie le paramètre de recuit lors d'une optimisation globale par recuit simulé (SA). |
| dropOut | Spécifie le ratio de dropout (abandon) pour les couches cachées afin de prévenir le surajustement. |
| dropOutInput | Spécifie le ratio de dropout (abandon) pour les couches d'entrée. |
| errorFunc | Spécifie la fonction d'erreur (ou de perte) à utiliser pour entraîner le réseau, adaptée au type de variable cible (par exemple, ENTROPY pour la classification). |
| freq | Spécifie une variable numérique qui contient la fréquence d'occurrence de chaque observation. |
| fullWeights | Génère le modèle de poids complet pour l'optimiseur LBFGS. |
| hiddens | Spécifie le nombre de neurones cachés pour chaque couche cachée dans le modèle. |
| includeBias | Indique si les paramètres de biais doivent être inclus pour les unités cachées et de sortie. |
| inputs | Spécifie les variables d'entrée (prédicteurs) à utiliser dans l'analyse. |
| inversePriors | Calcule le poids appliqué à l'erreur de prédiction de chaque variable cible nominale en fonction de la fréquence des classes. |
| listNode | Spécifie les nœuds (entrée, cachés, sortie) à inclure dans la table de sortie générée par le code de scoring. |
| missing | Spécifie comment imputer les valeurs manquantes pour les variables d'entrée. |
| modelId | Spécifie un nom de variable d'ID de modèle qui est inclus dans le code de scoring DATA step généré. |
| modelTable | Spécifie la table qui contient un modèle de réseau de neurones pré-entraîné à utiliser comme point de départ. |
| nAnns | Spécifie le nombre de réseaux à sélectionner parmi le nombre d'essais spécifié. |
| nloOpts | Spécifie les options pour l'optimisation non linéaire, y compris l'algorithme (par exemple, SGD, LBFGS, ADAM) et ses paramètres. |
| nominals | Spécifie les variables d'entrée et cibles nominales (catégorielles) à utiliser dans l'analyse. |
| nTries | Spécifie le nombre d'essais lors de l'entraînement de réseaux avec des poids initiaux aléatoires. |
| randDist | Spécifie la distribution pour la génération aléatoire des poids de connexion initiaux du réseau. |
| resume | Reprend une optimisation d'entraînement en utilisant les poids obtenus d'un entraînement précédent. |
| samplingRate | Spécifie la fraction des données à utiliser pour construire un réseau de neurones. |
| saveState | Spécifie la table dans laquelle sauvegarder l'état du modèle pour une prédiction ou un entraînement futurs. |
| scaleInit | Spécifie comment mettre à l'échelle les poids initiaux. |
| seed | Spécifie la graine aléatoire pour la génération de nombres aléatoires afin d'initialiser les poids du réseau, assurant la reproductibilité. |
| std | Spécifie la méthode de standardisation à utiliser sur les variables d'intervalle. |
| step | Spécifie une taille de pas pour les perturbations sur les poids du réseau lors des optimisations globales. |
| t | Spécifie le paramètre de température artificielle pour les optimisations globales de type Monte Carlo ou recuit simulé. |
| table | Spécifie la table de données d'entraînement contenant les variables d'entrée et la variable cible. |
| target | Spécifie la variable cible (ou de réponse) pour l'entraînement. |
| targetAct | Spécifie la fonction d'activation pour les neurones de la couche de sortie. |
| targetComb | Spécifie la fonction de combinaison pour les neurones sur les nœuds de sortie cibles. |
| targetMissing | Spécifie comment imputer les valeurs manquantes pour la variable cible. |
| targetStd | Spécifie la standardisation à utiliser sur les variables cibles d'intervalle. |
| validTable | Spécifie la table avec les données de validation pour l'arrêt précoce et l'évaluation du modèle. |
| weight | Spécifie une variable pour pondérer les erreurs de prédiction pour chaque observation pendant l'entraînement. |

## Création de Données de Classification
Ce bloc de code SAS crée un jeu de données synthétique nommé `class_data`. Il est conçu pour des tâches de classification binaire. Il contient cinq variables d'entrée numériques (`input1`-`input5`) et une variable cible binaire (`target`) qui est créée en fonction d'une combinaison linéaire des entrées. Ces données sont ensuite chargées dans une table CAS nommée `CLASS_DATA` pour être utilisées par l'action `annTrain`.
sas
data class_data;
  call streaminit(12345);
  do i = 1 to 1000;
    input1 = rand('UNIFORM');
    input2 = rand('UNIFORM') * 2;
    input3 = rand('NORMAL', 0, 1);
    input4 = rand('NORMAL', 5, 1.5);
    input5 = rand('POISSON', 3);
    if (input1 + 0.5*input2 - 0.2*input4 > 0.5) then target = 1;
    else target = 0;
    output;
  end;
run;

proc casutil;
  load data=class_data outcaslib=casuser casout='CLASS_DATA' replace;
quit;


## Entraînement d'un Perceptron Multicouche (MLP) de Base
Cet exemple montre comment entraîner un réseau de neurones de type Perceptron Multicouche (MLP) simple. Le réseau a deux couches cachées, la première avec 10 neurones et la seconde avec 5. Il utilise les variables `input1` à `input5` pour prédire la variable `target` à partir de la table `CLASS_DATA`. L'optimisation est effectuée à l'aide de la descente de gradient stochastique (SGD).
sas
proc cas;
  action neuralNet.annTrain / 
    table={name='CLASS_DATA'},
    inputs={{name='input1'}, {name='input2'}, {name='input3'}, {name='input4'}, {name='input5'}},
    target='target',
    hiddens={10, 5},
    arch='MLP',
    nloOpts={algorithm='SGD', optmlOpt={maxIters=100}, sgdOpt={learningRate=0.01}}
  ;
quit;


## Entraînement d'un Réseau avec Validation et Régularisation L1/L2
Cet exemple plus complexe divise les données en ensembles d'entraînement (70%) et de validation (30%). Il entraîne un réseau MLP avec une seule couche cachée de 20 neurones. Il utilise l'optimiseur L-BFGS et applique une régularisation L1 (0.01) et L2 (0.005) pour éviter le surajustement. La table `validTable` est utilisée pour l'arrêt précoce (`stagnation=5`), qui arrête l'entraînement si l'erreur de validation n'améliore pas pendant 5 itérations consécutives. Le modèle final et son état sont sauvegardés dans les tables `my_model_weights` et `my_model_state` respectivement.
sas
proc cas;
  partition.partition table={name='CLASS_DATA'} partition={fraction={train=0.7, valid=0.3}, seed=54321} casout={name='PARTITIONED_DATA', replace=true};

  action neuralNet.annTrain / 
    table={name='PARTITIONED_DATA', where='_PartInd_=1'},
    validTable={name='PARTITIONED_DATA', where='_PartInd_=2'},
    inputs={{name='input1'}, {name='input2'}, {name='input3'}, {name='input4'}, {name='input5'}},
    target='target',
    hiddens={20},
    arch='MLP',
    std='STD',
    targetStd='NONE',
    nloOpts={
      algorithm='LBFGS',
      optmlOpt={regL1=0.01, regL2=0.005, maxIters=200},
      validate={stagnation=5}
    },
    casOut={name='my_model_weights', replace=true},
    saveState={name='my_model_state', replace=true}
  ;
quit;

