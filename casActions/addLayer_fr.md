# deepLearn.addLayer

> L'action `addLayer` est une composante fondamentale de la construction de modèles de deep learning dans SAS Viya. Elle permet d'ajouter séquentiellement une nouvelle couche (layer) à une architecture de réseau neuronal existante. Chaque couche ajoute une transformation spécifique aux données qui la traversent, permettant au modèle d'apprendre des représentations de plus en plus complexes. Cette action est utilisée de manière itérative pour construire le modèle couche par couche, de la couche d'entrée (Input) à la couche de sortie (Output), en passant par diverses couches cachées (convolution, pooling, récurrentes, etc.).

## Syntaxe
sas
deepLearn.addLayer {
  layer={...},
  modelTable={...},
  name='string',
  nThreads=integer,
  replace=boolean,
  sharingWeights='string',
  srcLayers={'string-1', 'string-2', ...}
};


| Paramètre | Description |
| --- | --- |
| **layer** | Spécifie le type de couche et ses paramètres associés. Le type de couche que vous spécifiez détermine les autres paramètres applicables. |
| **modelTable** | Spécifie la table en mémoire qui contient le modèle à modifier. |
| **name** | Spécifie un nom unique pour la couche. Ce nom est crucial car il sert de référence pour les couches suivantes via le paramètre `srcLayers`. |
| **nThreads** | Spécifie le nombre de threads à utiliser pour l'exécution de l'action. |
| **replace** | Lorsqu'il est défini sur True, remplace une couche existante portant le même nom par cette nouvelle couche. |
| **sharingWeights** | Nomme la couche dont les poids seront partagés avec la couche actuelle. Utile pour des architectures comme les réseaux siamois. |
| **srcLayers** | Spécifie les noms des couches sources pour cette couche, définissant ainsi les connexions dans le graphe du modèle. |

## Création des Données d'Exemple
sas
/* L'action addLayer ne nécessite pas de données d'entrée directes, elle opère sur une table de modèle. */


## Ajout d'une Couche de Convolution Simple
sas
proc cas;
deepLearn.addLayer / 
    modelTable={name='mon_modele'},
    name='conv1',
    layer={type='CONVOLUTION', nFilters=32, width=3, height=3, stride=1},
    srcLayers={'entree'};
quit;


## Construction d'une Architecture LeNet-5 Séquentielle
sas
proc cas;
/* Étape 1: Construire le modèle initial avec une couche d'entrée */
deepLearn.buildModel / model={name='lenet5', replace=true}, type='CNN';
deepLearn.addLayer / modelTable={name='lenet5'}, name='data', layer={type='input', nchannels=1, width=28, height=28};

/* Étape 2: Ajouter la première couche de convolution */
deepLearn.addLayer / modelTable={name='lenet5'}, name='conv1', layer={type='convolution', nFilters=20, width=5, height=5, act='relu'}, srcLayers={'data'};

/* Étape 3: Ajouter la première couche de pooling */
deepLearn.addLayer / modelTable={name='lenet5'}, name='pool1', layer={type='pooling', width=2, height=2, stride=2, pool='max'}, srcLayers={'conv1'};

/* Étape 4: Ajouter la seconde couche de convolution */
deepLearn.addLayer / modelTable={name='lenet5'}, name='conv2', layer={type='convolution', nFilters=50, width=5, height=5, act='relu'}, srcLayers={'pool1'};

/* Étape 5: Ajouter la seconde couche de pooling */
deepLearn.addLayer / modelTable={name='lenet5'}, name='pool2', layer={type='pooling', width=2, height=2, stride=2, pool='max'}, srcLayers={'conv2'};

/* Étape 6: Ajouter une couche entièrement connectée */
deepLearn.addLayer / modelTable={name='lenet5'}, name='fc1', layer={type='fullconnect', n=500, act='relu'}, srcLayers={'pool2'};

/* Étape 7: Ajouter la couche de sortie */
deepLearn.addLayer / modelTable={name='lenet5'}, name='output', layer={type='output', n=10, act='softmax'}, srcLayers={'fc1'};
quit;

