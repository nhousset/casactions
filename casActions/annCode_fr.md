# neuralNet.annCode

> L'action `annCode` génère un code SAS DATA step à partir d'un modèle de réseau de neurones artificiels entraîné. Ce code peut ensuite être utilisé pour scorer de nouvelles données, c'est-à-dire pour appliquer le modèle et obtenir des prédictions. C'est une étape cruciale pour déployer un modèle en production ou pour l'intégrer dans d'autres processus SAS.

## Syntaxe
sas
neuralNet.annCode {
  code={...},
  listNode="ALL" | "HIDDEN" | "INPUT" | "OUTPUT",
  modelId="string",
  modelTable={...}
};


## Paramètres
| Paramètre | Description |
| --- | --- |
| `code` | Spécifie les options pour la génération du code de scoring SAS DATA step. Ce paramètre est essentiel pour obtenir le code exécutable. |
| `listNode` | Définit quels types de nœuds (entrée, cachés, sortie, ou tous) seront inclus dans la table de sortie scorée. Utile pour l'auto-encodage afin de réduire la dimensionnalité. |
| `modelId` | Assigne un nom de variable pour l'ID du modèle dans le code DATA step généré. Par défaut, le nom est préfixé par 'ANN_' suivi du nom de la variable cible. |
| `modelTable` | Spécifie la table CAS en mémoire qui contient le modèle de réseau de neurones à partir duquel le code de scoring sera généré. Ce paramètre est obligatoire. |

## Création de la table d'exemple `hmeq`
Ce code télécharge un fichier CSV contenant des données sur des prêts immobiliers (Home Equity) et le charge dans une table CAS nommée `HMEQ`. Cette table sera utilisée pour entraîner un modèle de réseau de neurones.
sas
proc casutil;
  load file '%sas_sashome%/dmcas_packages/samples/hmeq.csv' 
    casout={name='hmeq', caslib='casuser', replace=true};
quit;


## Génération de code de scoring simple
Cet exemple entraîne d'abord un modèle de réseau de neurones simple sur la table `hmeq` avec la procédure `annTrain`. Ensuite, il utilise l'action `annCode` pour générer le code de scoring DATA step correspondant et le stocker dans une table CAS nommée `score_code`.
sas
proc cas;
  neuralNet.annTrain / 
    table={name='hmeq'},
    inputs={'LOAN', 'MORTDUE', 'VALUE', 'YOJ', 'DEROG', 'DELINQ', 'CLAGE', 'NINQ', 'CLNO', 'DEBTINC'},
    target='BAD',
    modelTable={name='nn_model', caslib='casuser', replace=true};
  neuralNet.annCode / 
    modelTable={name='nn_model', caslib='casuser'},
    code={casOut={name='score_code', caslib='casuser', replace=true}};
quit;


## Génération de code avec personnalisation de l'ID du modèle
Cet exemple, après avoir entraîné un modèle, génère le code de scoring en spécifiant un `modelId` personnalisé. Cela permet de mieux identifier l'origine des prédictions dans le code généré, ce qui est utile pour la traçabilité et la gestion de plusieurs modèles.
sas
proc cas;
  neuralNet.annTrain / 
    table={name='hmeq'},
    inputs={'LOAN', 'MORTDUE', 'VALUE', 'YOJ'},
    target='BAD',
    modelTable={name='nn_model_detailed', caslib='casuser', replace=true};
  neuralNet.annCode / 
    modelTable={name='nn_model_detailed', caslib='casuser'},
    modelId='MyFirstNNModel',
    code={casOut={name='score_code_detailed', caslib='casuser', replace=true}};
quit;

