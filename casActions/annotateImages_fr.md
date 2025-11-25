# image.annotateImages

> Annote les images d'une table avec des métadonnées contenues dans cette même table. Cette action est fondamentale pour superposer des informations visuelles, telles que des boîtes englobantes, des lignes, des points ou des masques de segmentation, directement sur les images. C'est une étape cruciale dans les pipelines de vision par ordinateur pour la visualisation des résultats de détection d'objets, de segmentation sémantique ou d'autres analyses d'images.

## Syntaxe
sas
image.annotateImages /
   annotations={{annotation={annotationType="LINES"|"POINTS"|"PROTOBUF"|"SEGMENTATION", annotationType-specific-parameters}}, ...}
   casOut={caslib="string", compress=TRUE|FALSE, indexVars={"variable-name-1" <, "variable-name-2", ...>}, label="string", lifetime=64-bit-integer, maxMemSize=64-bit-integer, memoryFormat="DVR"|"INHERIT"|"STANDARD", name="table-name", promote=TRUE|FALSE, replace=TRUE|FALSE, replication=integer, tableRedistUpPolicy="DEFER"|"NOREDIST"|"REBALANCE", threadBlockSize=64-bit-integer, timeStamp="string", where={"string-1" <, "string-2", ...>}}
   copyVars={"variable-name-1" <, "variable-name-2", ...>}
   decode={convert=TRUE|FALSE, encodeType="string", value=TRUE|FALSE}
   images={dimension="variable-name", id="variable-name", image="variable-name", imageFormat="variable-name", label="variable-name", path="variable-name", resolution="variable-name", size="variable-name", table={...}, type="variable-name"};


| Paramètre | Description |
| --- | --- |
| `annotations` | Spécifie les annotations à réaliser par l'action. |
| `casOut` | Spécifie la table de sortie pour contenir les images annotées. |
| `copyVars` | Spécifie les variables à copier de la table d'entrée vers la table de sortie. |
| `decode` | Spécifie les paramètres liés au décodage des images avant l'annotation. |
| `images` | Spécifie la table d'images d'entrée à annoter. |
| `annotationType` | Définit le type d'annotation à appliquer : 'LINES' (lignes), 'POINTS' (points), 'PROTOBUF' (boîtes englobantes, polygones), ou 'SEGMENTATION' (masques). |
| `representation` | Définit comment les données d'annotation sont structurées dans la table d'entrée, généralement en spécifiant la colonne qui les contient. |
| `colorMap` | Pour 'SEGMENTATION', spécifie la palette de couleurs (par exemple, 'JET', 'HOT', 'COOL') à utiliser pour visualiser les masques. |
| `transparency` | Pour 'SEGMENTATION', définit le pourcentage de transparence (0-100) de la superposition du masque sur l'image originale. |
| `thickness` | Pour 'LINES', spécifie l'épaisseur de la ligne en pixels. |
| `radius` | Pour 'POINTS', spécifie le rayon du point en pixels. |
| `r, g, b` | Spécifient les valeurs des canaux Rouge, Vert et Bleu (0-255) pour définir la couleur des annotations comme les lignes ou les points. |

## Création d'une table d'images de test
Ce bloc de code SAS crée une table CAS nommée `IMAGES_TO_ANNOTATE`. Il charge des images à partir d'un chemin spécifié (`/path/to/your/images/`) dans la caslib `CASUSER`. Cette table contiendra les images qui seront ensuite utilisées pour l'annotation. Assurez-vous que le chemin d'accès aux images est correct et que la caslib est active.
sas
proc casutil; load path="/path/to/your/images/" casout="IMAGES_TO_ANNOTATE" caslib="CASUSER"; quit;


## Exemple 1: Annotation de points sur des images
Cet exemple simple montre comment annoter des images avec des points. Il suppose qu'une table `IMAGES_WITH_POINTS` existe et contient une colonne `_points_` avec les coordonnées des points à dessiner. L'action `annotateImages` lit cette table, dessine les points en rouge sur chaque image, et sauvegarde le résultat dans une nouvelle table `IMAGES_ANNOTATED_POINTS`.
sas
proc cas; image.annotateImages / table={name="IMAGES_WITH_POINTS"} casout={name="IMAGES_ANNOTATED_POINTS", replace=true} annotations={{annotation={annotationType="POINTS", representation={representationType="SINGLE_COLUMN", columnName="_points_"}, r=255, radius=5}}}; run; quit;

La table `IMAGES_ANNOTATED_POINTS` est créée dans la caslib active. Elle contient les images originales avec des points rouges superposés aux coordonnées spécifiées dans la colonne `_points_`.

## Exemple 2: Superposition de masques de segmentation avec une palette de couleurs
Cet exemple détaillé illustre comment superposer des masques de segmentation sur des images. Il utilise deux tables : une contenant les images originales (`MY_IMAGES`) et une autre contenant les masques de segmentation (`MY_MASKS`). L'action `annotateImages` fusionne ces deux sources, applique une palette de couleurs 'JET' aux masques avec une transparence de 50%, et sauvegarde les images annotées dans la table `IMAGES_ANNOTATED_SEGMENTATION`.
sas
proc cas; image.annotateImages / table={name="MY_IMAGES"} casout={name="IMAGES_ANNOTATED_SEGMENTATION", replace=true} annotations={{annotation={annotationType="SEGMENTATION", image="mask_image_column", table={name="MY_MASKS", caslib="CASUSER"}, colorMap="JET", transparency=50}}}; run; quit;

Une nouvelle table CAS nommée `IMAGES_ANNOTATED_SEGMENTATION` est créée. Chaque image de cette table est une superposition de l'image originale et de son masque de segmentation correspondant, coloré selon la palette 'JET' et avec une transparence de 50%.

## Exemple 3: Dessin de lignes bleues sur des images
Cet exemple montre comment dessiner des lignes sur des images. Il prend en entrée la table `IMAGES_WITH_LINES` qui doit contenir une colonne `_lines_` définissant les coordonnées des points de départ et d'arrivée de chaque ligne. L'action dessine des lignes bleues d'une épaisseur de 3 pixels et enregistre les images modifiées dans la table `IMAGES_ANNOTATED_LINES`.
sas
proc cas; image.annotateImages / table={name="IMAGES_WITH_LINES"} casout={name="IMAGES_ANNOTATED_LINES", replace=true} annotations={{annotation={annotationType="LINES", representation={representationType="SINGLE_COLUMN", columnName="_lines_"}, b=255, thickness=3}}}; run; quit;

La table de sortie `IMAGES_ANNOTATED_LINES` est générée, contenant les images avec des lignes bleues superposées.
