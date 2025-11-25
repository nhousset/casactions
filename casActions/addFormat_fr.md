# sessionProp.addFormat

> Ajoute un format à une bibliothèque de formats. Les formats permettent de contrôler la manière dont les valeurs des variables sont affichées. Cette action est fondamentale pour personnaliser les sorties de données, améliorer la lisibilité des rapports et préparer les données pour l'analyse.

## Syntaxe
sas
sessionProp.addFormat <result=results> <status=rc> / 
   dataType={"string-1" <, "string-2", ...>},
   defaultL=64-bit-integer,
   fill={"string-1" <, "string-2", ...>},
   fmtLibName="string",
   fmtName="string",
   fmtType="string",
   fuzz=double,
   locale="string",
   maxL=64-bit-integer,
   minL=64-bit-integer,
   mult={double-1 <, double-2>, ...},
   multiLabel=TRUE | FALSE,
   noedit={"string-1" <, "string-2", ...>},
   notSorted=TRUE | FALSE,
   prefix={"string-1" <, "string-2", ...>},
   ranges={"string-1" <, "string-2", ...>},
   replace=TRUE | FALSE;


## Paramètres

| Paramètre | Description |
| --- | --- |
| `dataType` | Indique si la valeur est de type DATE, TIME ou DATETIME. |
| `defaultL` | Spécifie la longueur par défaut du format. |
| `fill` | Indique le caractère de remplissage pour un format de type PICTURE. |
| `fmtLibName` | Spécifie le nom de la bibliothèque de formats où le nouveau format sera ajouté. |
| `fmtName` | Spécifie le nom du format à créer. Un nom de format ne peut pas se terminer par un chiffre. Pour un format de caractère, le nom doit commencer par un '$'. |
| `fmtType` | Indique le type de format : PICTURE, INVALUE, ou VALUE. |
| `fuzz` | Spécifie un facteur de tolérance (fuzz factor) pour faire correspondre les valeurs numériques à une plage. Utile pour les données non exactes. |
| `locale` | Spécifie la locale à utiliser dans le préfixe de locale du nom de format, pour créer des formats spécifiques à une langue. |
| `maxL` | Spécifie une longueur maximale pour le format, en octets. |
| `minL` | Spécifie une longueur minimale pour le format, en octets. |
| `mult` | Indique le multiplicateur pour un format de type PICTURE, au lieu de le calculer à partir des points décimaux. |
| `multiLabel` | Si défini sur True, plusieurs étiquettes peuvent être spécifiées pour la même valeur interne. |
| `noedit` | Indique une étiquette non-PICTURE pour un format de type PICTURE. |
| `notSorted` | Si défini sur True, les valeurs ou les plages sont stockées dans l'ordre où elles sont définies, plutôt que d'être triées. |
| `prefix` | Indique les caractères de préfixe pour un format de type PICTURE. |
| `ranges` | Spécifie une liste de paires valeur=étiquette ou plage=étiquette. Les plages sont définies comme min-max=étiquette. |
| `replace` | Si défini sur True, un format existant portant le même nom sera remplacé par ce nouveau format. |

## Création d'une table de données pour les exemples
Avant de créer des formats, nous avons besoin de données. Ce bloc de code SAS crée une table en mémoire dans CAS nommée 'produits' contenant des identifiants de produits et leurs quantités. Cette table servira de base pour appliquer les formats que nous allons créer.
sas
data casuser.produits; 
  input ID_Produit $ Quantite; 
  datalines; 
  A100 10 
  A200 25 
  B100 55 
  B200 80 
  C100 120 
  ; 
run;


## Exemples

### Création d'un format numérique simple
Cet exemple crée une bibliothèque de formats nommée 'maBiblioFmt' si elle n'existe pas, puis y ajoute un format numérique simple appelé 'NiveauStock'. Ce format catégorise les quantités en trois niveaux : 'Bas', 'Moyen' et 'Haut'.
sas
proc cas; 
  sessionprop.addFmtLib fmtLibName="maBiblioFmt"; 
  sessionprop.addFormat fmtLibName="maBiblioFmt" fmtName="NiveauStock" 
    ranges={"1-50='Bas'", "51-100='Moyen'", "101-high='Haut'"}; 
run;

L'action crée le format 'NiveauStock' dans la bibliothèque 'maBiblioFmt' de la session CAS active. Aucune sortie n'est affichée, mais le format est prêt à être utilisé pour formater des colonnes numériques.

### Création d'un format de caractères avec remplacement et locale
Cet exemple crée un format de caractères '$CatProduit' dans la bibliothèque 'maBiblioFmt'. L'option `replace=true` assure que si un format du même nom existe, il sera écrasé. L'option `locale='fr_FR'` spécifie que ce format est destiné à la locale française, ce qui est utile pour la gestion de formats multilingues. Le format regroupe les ID de produits en catégories.
sas
proc cas; 
  sessionprop.addFormat 
    fmtLibName="maBiblioFmt" 
    fmtName="$CatProduit" 
    replace=true 
    locale='fr_FR' 
    ranges={"'A100'-'A999'='Catégorie A'", "'B100'-'B999'='Catégorie B'", "other='Autre'"}; 
run;

Le format de caractères '$CatProduit' est créé ou mis à jour dans la bibliothèque 'maBiblioFmt' avec une spécificité pour la locale 'fr_FR'. Ce format peut maintenant être appliqué à des colonnes de type caractère comme 'ID_Produit' pour afficher des étiquettes plus descriptives.
