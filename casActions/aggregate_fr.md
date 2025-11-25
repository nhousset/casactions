# aggregation.aggregate

> L'action `aggregate` du jeu d'actions `aggregation` est un outil puissant dans SAS Viya pour effectuer des agrégations sur des variables sélectionnées. Elle permet de résumer de grands volumes de données en calculant diverses statistiques (somme, moyenne, nombre, etc.) sur des groupes de données. Cette action est particulièrement utile pour la préparation de données en vue de l'analyse, la création de rapports de synthèse ou la transformation de données transactionnelles en séries temporelles. Elle peut regrouper les données sur la base de variables catégorielles, d'intervalles de temps ou de fenêtres glissantes, offrant une grande flexibilité pour l'analyse exploratoire et la modélisation.

## Syntaxe
sas
aggregation.aggregate /
    table={...} 
    <varSpecs={{...}, {...}}>
    <casOut={...}> 
    <groupByLimit=64-bit-integer>
    <interval="string">
    <id="variable-name">;


## Paramètres

| Paramètre | Description |
| --- | --- |
| table | Spécifie la table CAS d'entrée à utiliser pour l'agrégation. |
| varSpecs | Spécifie les variables à agréger et les paramètres de l'agrégateur (par exemple, SUM, MEAN, N). |
| casOut | Spécifie la table CAS de sortie pour stocker les résultats de l'agrégation. |
| groupBy | Spécifie une ou plusieurs variables pour regrouper les données avant l'agrégation. |
| id | Spécifie une variable numérique qui identifie l'horodatage associé à chaque observation, utilisée pour les agrégations temporelles. |
| interval | Spécifie la période de temps pour l'accumulation des observations (par exemple, 'MONTH', 'DAY', 'HOUR'). |
| align | Spécifie l'alignement de la valeur représentative par rapport à un intervalle ou un bac (BEGINNING, MIDDLE, ENDING). |
| copyVars | Spécifie les variables à copier de la table d'entrée vers la table de sortie. |
| weight | Spécifie une variable numérique dont les valeurs sont utilisées comme poids pour les variables d'analyse. |
| freq | Spécifie une variable numérique dont les valeurs sont utilisées comme fréquence des valeurs de la variable d'analyse. |

## Création de la table d'exemple 'ventes_produits'
Ce code SAS crée une table CAS nommée 'ventes_produits'. Elle contient des informations sur les ventes (produit, quantité, date_vente) et servira de base pour les exemples d'agrégation.
sas
data casuser.ventes_produits; 
 informat date_vente date9.; 
 format date_vente date9.; 
 input produit $ quantite date_vente; 
 datalines; 
 A 10 01JAN2023 
 B 15 01JAN2023 
 A 20 15JAN2023 
 C 5 15JAN2023 
 B 10 01FEB2023 
 A 5 15FEB2023 
 C 25 01MAR2023 
 ; 
 run;


## Exemples

### Agrégation simple par produit
Cet exemple montre comment calculer la quantité totale vendue (SUM) et le nombre de transactions (N) pour chaque produit.
sas
proc cas; 
 aggregation.aggregate / 
  table={name='ventes_produits', groupBy={'produit'}}, 
  varSpecs={{name='quantite', agg='SUM'}, {name='quantite', agg='N'}}, 
  casOut={name='ventes_agg_simple', replace=true}; 
 run; 
 quit;


### Agrégation temporelle mensuelle avec statistiques complètes
Cet exemple agrège les données de ventes par mois en utilisant la variable 'date_vente'. Il calcule un ensemble complet de statistiques descriptives (SUMMARY) pour la variable 'quantite' pour chaque intervalle mensuel.
sas
proc cas; 
 aggregation.aggregate / 
  table={name='ventes_produits', groupBy={'date_vente'}}, 
  id='date_vente', 
  interval='MONTH', 
  varSpecs={{name='quantite', agg='SUMMARY'}}, 
  casOut={name='ventes_mensuelles_details', replace=true}; 
 run; 
 quit;


### Agrégation avec spécifications de variables multiples et sortie personnalisée
Cet exemple agrège les données par 'produit'. Il calcule la quantité maximale (MAX) et minimale (MIN) pour la variable 'quantite'. Les colonnes de résultats sont renommées en 'Quantite_Max' et 'Quantite_Min' pour plus de clarté.
sas
proc cas; 
 aggregation.aggregate / 
  table={name='ventes_produits', groupBy={'produit'}}, 
  varSpecs={ 
   {name='quantite', agg='MAX', columnNames={'Quantite_Max'}}, 
   {name='quantite', agg='MIN', columnNames={'Quantite_Min'}} 
  }, 
  casOut={name='ventes_produit_minmax', replace=true}; 
 run; 
 quit;

