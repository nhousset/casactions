# sessionProp.addFmtLib

> L'action `addFmtLib` est une action de la catégorie 'Session Properties' dans SAS Viya. Elle permet d'ajouter une bibliothèque de formats définie par l'utilisateur à la session CAS active. Cette bibliothèque peut être vide et créée à la volée, ou provenir d'une table de contrôle de formats (créée par PROC FORMAT) préalablement chargée dans CAS. Une fois ajoutée, les formats de cette bibliothèque peuvent être utilisés pour formater les valeurs des variables dans les tables CAS lors des analyses et des affichages de résultats.

## Syntaxe
sas
proc cas;
   sessionProp.addFmtLib /
      caslib="nom_caslib"
      fmtLibName="nom_bibliotheque_format"
      fmtSearch="APPEND" | "INSERT" | "NONE" | "REPLACE"
      name="nom_table_format"
      path="chemin_vers_fichier"
      promote=TRUE | FALSE
      replace=TRUE | FALSE;
run;


## Paramètres
| Paramètre | Description |
| --- | --- |
| caslib | Spécifie la caslib où réside la table de contrôle de la bibliothèque de formats. |
| fmtLibName | Spécifie le nom à donner à la bibliothèque de formats dans la session CAS. Ce nom ne doit pas dépasser 63 caractères. |
| fmtSearch | Définit comment la nouvelle bibliothèque de formats est ajoutée à la liste de recherche de formats de la session (APPEND, INSERT, NONE, REPLACE). La valeur par défaut est APPEND. |
| name | Spécifie le nom de la table de contrôle de formats (avec ou without l'extension .sashdat) au sein de la caslib spécifiée. |
| path | Spécifie le chemin d'accès complet à un fichier de bibliothèque de formats (par exemple, un fichier .sashdat) sur le nœud de contrôle du serveur CAS. |
| promote | Si défini sur TRUE, la bibliothèque de formats est promue à une portée globale, la rendant accessible à toutes les sessions CAS. Une autorisation d'administrateur peut être requise. |
| replace | Si défini sur TRUE, une bibliothèque de formats existante portant le même nom est remplacée par la nouvelle. |

## Création d'une table de contrôle de formats
sas
libname mycas cas; 
proc format cntlout=work.myformatcontrol; 
   value $gender 'M'='Masculin' 'F'='Féminin'; 
   value agefmt 1-25='Jeune' 26-50='Adulte' 51-100='Senior'; 
run; 
proc casutil; 
   load data=work.myformatcontrol casout={name='myformatcontrol', caslib='casuser', replace=true}; 
run;


## Exemples

## Ajouter une bibliothèque de formats vide
sas
proc cas; sessionprop.addFmtLib / fmtLibName='MaNouvelleFmtLib'; run;


## Ajouter une bibliothèque de formats à partir d'une table CAS
sas
proc cas; sessionprop.addFmtLib / caslib='casuser' name='myformatcontrol' fmtLibName='MesFormatsGlobaux' replace=true promote=true; run;

