# image.augmentImages

> L'action `augmentImages` est une fonctionnalité puissante du traitement d'images dans SAS Viya. Elle permet de créer des versions augmentées d'un jeu d'images en générant des 'patches' (imagettes) et en leur appliquant diverses mutations. Cette technique est fondamentale en apprentissage profond (deep learning) pour enrichir artificiellement les jeux de données, ce qui aide à améliorer la robustesse et la performance des modèles de vision par ordinateur en les exposant à une plus grande variété de scénarios (changements de luminosité, d'angle, de couleur, etc.).

## Syntaxe
sas
image.augmentImages {
  addColumns={augmentImagesAddColumnsParms},
  augmentations={{augmentationOptions-1} <, {augmentationOptions-2}, ...>},
  casOut={casouttable},
  copyVars={"variable-name-1" <, "variable-name-2", ...>},
  decode=TRUE | FALSE,
  image="variable-name",
  seed=64-bit-integer,
  table={castable},
  writeRandomly=TRUE | FALSE
};


## Paramètres

| Paramètre | Description |
| --- | --- |
| `addColumns` | Spécifie des colonnes supplémentaires à ajouter à la table de sortie, comme les attributs d'augmentation. |
| `augmentations` | Définit la liste des options de recadrage et de mutation à appliquer aux images. |
| `casOut` | Spécifie la table de sortie pour stocker les images augmentées. |
| `copyVars` | Liste des variables à copier de la table d'entrée vers la table de sortie. |
| `decode` | Si TRUE, décode les images avant de les écrire dans la table de sortie. |
| `image` | Indique la colonne contenant les données binaires des images. Par défaut, '_image_'. |
| `seed` | Spécifie une graine pour la génération de nombres aléatoires, assurant la reproductibilité des augmentations aléatoires. |
| `table` | Spécifie la table d'entrée contenant les images à augmenter. |
| `writeRandomly` | Si TRUE, écrit les images résultantes dans la table de sortie de manière aléatoire. |

## Préparation des données initiales
Avant d'utiliser `augmentImages`, il est nécessaire de charger les images dans une table CAS. Ce code montre comment charger des images depuis un chemin accessible par le serveur CAS dans une table nommée `myImages`.
sas
proc casutil;
  load path="/chemin/vers/vos/images/" casout="myImages" importoptions=(fileType="IMAGE");
quit;


## Exemples

### Exemple 1 : Retournement horizontal simple
Cet exemple applique une seule augmentation : un retournement horizontal sur l'ensemble de chaque image. C'est une des techniques d'augmentation les plus courantes.
sas
proc cas;
  image.augmentImages / 
    table={name='myImages'}
    casOut={name='augmented_hflip', replace=true}
    augmentations={{useWholeImage=true, mutations={horizontalFlip=true}}};
quit;


### Exemple 2 : Création de patches et retournement vertical
Ici, nous extrayons des patches de 224x224 pixels de chaque image et appliquons un retournement vertical à chacun de ces patches.
sas
proc cas;
  image.augmentImages / 
    table={name='myImages'}
    casOut={name='augmented_patches_vflip', replace=true}
    augmentations={{width=224, height=224, sweepImage=true, mutations={verticalFlip=true}}};
quit;


### Exemple 3 : Augmentation complète pour l'entraînement d'un modèle
Cet exemple complexe génère des données d'entraînement enrichies. Pour chaque image, il crée des patches de 256x256, les redimensionne en 224x224, puis applique une série de mutations aléatoires : retournement horizontal, rotation aléatoire jusqu'à 30 degrés vers la gauche ou la droite, et un léger assombrissement. Une graine (seed) est utilisée pour garantir que les résultats aléatoires peuvent être reproduits.
sas
proc cas;
  image.augmentImages /
    table={name='myImages'}
    casOut={name='augmented_training_data', replace=true}
    seed=1234
    augmentations={{
      sweepImage=true, width=256, height=256, outputWidth=224, outputHeight=224,
      mutations={
        horizontalFlip=true,
        rotateLeft={type='RANGE', value={0, 30}},
        rotateRight={type='RANGE', value={0, 30}},
        darken={type='RANGE', value={0.8, 1.0}}
      }
    }};
quit;


### Exemple 4 : Utilisation de plusieurs configurations d'augmentation
Cette technique avancée applique deux schémas d'augmentation distincts en une seule passe. La première crée des patches de 128x128 avec retournement horizontal. La seconde utilise l'image entière et applique une gigue de couleur (color jittering). Cela permet de générer un jeu de données encore plus varié.
sas
proc cas;
  image.augmentImages /
    table={name='myImages'}
    casOut={name='augmented_multi_config', replace=true}
    augmentations={
      {width=128, height=128, sweepImage=true, mutations={horizontalFlip=true}},
      {useWholeImage=true, mutations={colorJittering=true}}
    };
quit;

