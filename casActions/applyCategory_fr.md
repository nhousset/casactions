# textRuleScore.applyCategory

> L'action `applyCategory` permet de catégoriser du texte en utilisant un modèle de catégorie préalablement compilé (fichier MCO). Elle applique les règles linguistiques définies dans le modèle pour assigner une ou plusieurs catégories aux documents textuels fournis en entrée.

## Syntaxe
sas
textRuleScore.applyCategory {
  casOut={...},
  docId="string",
  docType="TEXT"|"XML",
  groupedMatchOut={...},
  matchDelimiter="string",
  matchOut={...},
  model={...},
  modelOut={...},
  scoringAlgorithm="FREQUENCY"|"WEIGHTED",
  table={...},
  text="string"
};


## Paramètres

| Paramètre | Description |
| --- | --- |
| casOut | Spécifie la table de sortie CAS pour les résultats de la catégorisation. |
| docId | Spécifie le nom de la variable d'identifiant unique dans la table d'entrée pour référencer chaque document. |
| docType | Spécifie le type de document. 'TEXT' pour du texte brut, 'XML' pour des documents au format XML. |
| groupedMatchOut | Spécifie la table de sortie pour les correspondances groupées par catégorie pour chaque document. |
| matchDelimiter | Spécifie le délimiteur à utiliser pour les instances de texte correspondant dans la table de sortie 'groupedMatchOut'. |
| matchOut | Spécifie la table de sortie pour les termes correspondants des catégories. |
| model | Spécifie la table CAS d'entrée qui contient le modèle de catégorisation (fichier MCO). |
| modelOut | Spécifie la table de sortie pour le modèle de catégories. |
| scoringAlgorithm | Spécifie l'algorithme de scoring à utiliser. 'FREQUENCY' (par défaut) ou 'WEIGHTED'. |
| table | Spécifie la table de données d'entrée contenant le texte à catégoriser. |
| text | Spécifie le nom de la variable de texte dans la table d'entrée sur laquelle le modèle est appliqué. |

## Création des données de démonstration
Ce bloc de code crée une table CAS nommée 'reviews' contenant des commentaires de clients. Cette table sera utilisée pour appliquer le modèle de catégorisation.
sas
data casuser.reviews;
   infile datalines delimiter='|';
   length text $500. id $10.;
   input text$ id$;
   datalines;
I love my new SAS software! It's amazing.|1
This is the best software I have ever used.|2
I am not a fan of this product. It is difficult to use.|3
SAS is a great company with great products.|4
;
run;


## Exemples

### Application simple d'un modèle de catégorie
Cet exemple montre comment appliquer un modèle de catégorie (stocké dans 'category_model_table') à la colonne 'text' de la table 'reviews' et stocker les résultats dans la table 'reviews_categorized'.
sas
proc cas;
  textRuleScore.applyCategory / 
    table={name='reviews'},
    model={name='category_model_table'},
    docId='id',
    text='text',
    casOut={name='reviews_categorized', replace=true};
run;


### Application détaillée d'un modèle de catégorie avec sorties multiples
Cet exemple complet applique un modèle de catégorie, génère une table de sortie principale ('reviews_categorized'), une table des correspondances détaillées ('reviews_matches') et une table des correspondances groupées ('reviews_grouped_matches'). L'algorithme de scoring 'WEIGHTED' est utilisé.
sas
proc cas;
  textRuleScore.applyCategory / 
    table={name='reviews', caslib='casuser'},
    model={name='category_model_table', caslib='casuser'},
    docId='id',
    text='text',
    scoringAlgorithm='WEIGHTED',
    casOut={name='reviews_categorized', caslib='casuser', replace=true},
    matchOut={name='reviews_matches', caslib='casuser', replace=true},
    groupedMatchOut={name='reviews_grouped_matches', caslib='casuser', replace=true},
    matchDelimiter='|';
run;

