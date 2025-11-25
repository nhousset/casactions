# textRuleScore.applyConcept

> L'action `applyConcept` effectue une extraction de concepts en utilisant un modèle d'extraction de concepts (fichier LI). Elle permet d'identifier et d'extraire des informations structurées à partir de données textuelles non structurées en se basant sur des règles linguistiques prédéfinies.

## Syntaxe
sas
textRuleScore.applyConcept {
  casOut={...},
  docId="string",
  dropConcepts={"string-1", ...},
  factOut={...},
  language="string",
  litiChunkSize="string",
  matchType="ALL"|"BEST"|"LONGEST",
  model={...},
  parseTableIn={...},
  parseTableOut={...},
  ruleMatchOut={...},
  table={...},
  text="string"
};


## Paramètres

| Paramètre | Description |
| --- | --- |
| **casOut** | Spécifie la table CAS de sortie qui contient les informations sur les correspondances de concepts. |
| **docId** | Spécifie le nom de la variable de la table CAS qui contient les identifiants de document. |
| **dropConcepts** | Spécifie une liste de concepts principaux à exclure des tables CAS de sortie. |
| **factOut** | Spécifie la table CAS de sortie qui contient les informations sur les correspondances de faits. |
| **language** | Spécifie la langue utilisée dans la table d'entrée. La valeur par défaut est "ENGLISH". |
| **litiChunkSize** | Spécifie la taille des blocs de données (chunks) utilisés lors du traitement d'un document. La valeur par défaut est "32K". "ALL" traite le document entier en une seule fois. |
| **matchType** | Spécifie le type de correspondance à utiliser. "ALL" (par défaut) renvoie toutes les correspondances, "BEST" la meilleure, et "LONGEST" la plus longue. |
| **model** | Spécifie une table CAS d'entrée qui contient le modèle LITI défini par l'utilisateur. Si non spécifié, le modèle de base est utilisé. |
| **parseTableIn** | Spécifie le nom de la table CAS contenant des documents pré-analysés, créée avec le paramètre `parseTableOut` lors d'un appel précédent. |
| **parseTableOut** | Spécifie une table CAS de sortie pour contenir les documents pré-analysés, afin d'améliorer les performances lors d'utilisations futures avec `parseTableIn`. |
| **ruleMatchOut** | Spécifie la table CAS de sortie qui contient les informations sur les correspondances de règles. Peut être utilisée comme entrée pour l'action `ruleGen`. |
| **table** | Spécifie une table CAS d'entrée qui contient les documents d'entrée à analyser. |
| **text** | Spécifie le nom de la variable de la table CAS qui contient le texte à traiter. |

## Création des Données d'Exemple
Ce code crée une table CAS nommée 'reviews' contenant des commentaires de clients. Cette table sera utilisée pour illustrer l'extraction de concepts.
sas
data casuser.reviews;
   infile datalines delimiter='|';
   length text $500 id $10;
   input id$ text$;
   datalines;
id1|Le service client était excellent, très réactif.
id2|Je suis déçu par la qualité du produit. Il s'est cassé après une semaine.
id3|Le produit est bon, mais le support technique est lent.
;
run;


## Exemples

### Extraction de Concepts de Base
Cet exemple simple applique un modèle de concepts (préalablement chargé dans 'mycas.concept_model') sur la colonne 'text' de la table 'reviews' et stocke les correspondances dans 'concept_matches'.
sas
proc cas;
   textRuleScore.applyConcept / 
      table={caslib="casuser", name="reviews"},
      docId="id",
      text="text",
      model={caslib="casuser", name="concept_model"},
      casOut={caslib="casuser", name="concept_matches", replace=true};
run;


### Extraction de Concepts avec Sorties Multiples et Filtrage
Cet exemple détaillé applique un modèle de concepts, spécifie la langue française, et génère trois tables de sortie : `concept_matches` pour les concepts, `fact_matches` pour les faits, et `rule_matches` pour les détails des règles. Il utilise également `matchType='LONGEST'` pour ne garder que la correspondance la plus longue et `dropConcepts` pour exclure le concept 'product' des résultats.
sas
proc cas;
   textRuleScore.applyConcept / 
      table={caslib="casuser", name="reviews"},
      docId="id",
      text="text",
      model={caslib="casuser", name="concept_model"},
      language="french",
      matchType="LONGEST",
      dropConcepts={"product"},
      casOut={caslib="casuser", name="concept_matches", replace=true},
      factOut={caslib="casuser", name="fact_matches", replace=true},
      ruleMatchOut={caslib="casuser", name="rule_matches", replace=true};
run;

