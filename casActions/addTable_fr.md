# table.addTable

> L'action `addTable` du jeu d'actions `table` est un mécanisme fondamental dans SAS Viya pour transférer des données depuis un programme client (comme Python, R, ou Lua) vers le serveur CAS. Contrairement à `loadTable` qui charge des données déjà présentes sur le serveur, `addTable` est utilisée pour créer une nouvelle table en mémoire dans CAS avec des données qui résident sur la machine cliente. C'est l'équivalent d'un 'upload' de données vers le serveur pour une analyse immédiate. Notez que cette action est généralement invoquée via des méthodes spécifiques au client, comme `add_table()` en Python (SWAT), plutôt que d'être appelée directement avec la syntaxe `proc cas`.

## Syntaxe
sas
table.addTable <result=results> <status=rc> / 
   append=TRUE | FALSE,
   caslib="string",
   columnar=TRUE | FALSE,
   commitRecords=64-bit-integer,
   commitSeconds=64-bit-integer,
   compress=TRUE | FALSE,
   copies=integer,
   descending={integer-1 <, integer-2, ...>},
   label="string",
   maxMBytes=64-bit-integer,
   memoryFormat="DVR" | "INHERIT" | "STANDARD",
   orderBy={"variable-name-1" <, "variable-name-2", ...>},
   partition={"variable-name-1" <, "variable-name-2", ...>},
   promote=TRUE | FALSE,
   * recLen=integer,
   repeat=TRUE | FALSE,
   replace=TRUE | FALSE,
   table="table-name",
   vars={{addtablevariable-1} <, {addtablevariable-2}, ...>};


| Paramètre | Description |
| --- | --- |
| append | Spécifie d'ajouter des lignes de la table à une table existante. |
| caslib | Spécifie la caslib cible pour la table. |
| columnar | Spécifie de créer la table en mémoire au format colonnaire. |
| commitRecords | Spécifie le nombre de lignes que le serveur doit recevoir avant de valider (commit) les lignes dans la table. |
| commitSeconds | Spécifie le nombre de secondes pendant lesquelles le serveur reçoit des lignes avant de valider (commit) les lignes dans la table. |
| compress | Lorsque défini sur True, compresse la table cible. |
| copies | Spécifie le nombre de copies redondantes à créer pour les lignes. Des valeurs plus élevées offrent une plus grande tolérance aux pannes de nœuds mais utilisent plus de mémoire. |
| descending | Spécifie d'inverser l'ordre de tri pour les variables spécifiées afin que les résultats soient triés de la plus grande à la plus petite valeur. Vous devez spécifier les noms de variables dans le paramètre orderBy. |
| label | Spécifie le libellé à associer à la table. |
| maxMBytes | Spécifie la quantité maximale de mémoire physique, en mégaoctets, à allouer pour la table. Au-delà de ce seuil, le serveur utilise des fichiers temporaires. |
| memoryFormat | Spécifie le format de mémoire pour la table de sortie (STANDARD, DVR, ou INHERIT). |
| orderBy | Spécifie les noms de variables à utiliser pour ordonner les lignes au sein des partitions. |
| partition | Spécifie les noms de variables à utiliser comme clés de partitionnement. |
| promote | Lorsque défini sur True, la table est ajoutée avec une portée globale (global scope), la rendant accessible à d'autres sessions CAS. |
| recLen | Paramètre obligatoire qui spécifie la longueur, en octets, d'une ligne. Ce paramètre est généralement géré par le client. |
| repeat | Lorsque défini sur True, les lignes de la table sont dupliquées sur chaque machine d'un serveur distribué. |
| replace | Lorsque défini sur True, une table existante portant le même nom est écrasée. |
| table | Spécifie le nom de la table de sortie en mémoire dans CAS. |
| vars | Spécifie les attributs pour chaque variable, tels que le nom, le type, la longueur, le format et le libellé. |

## Création de Données Côté Client (Python avec Pandas)
Pour utiliser l'action `addTable`, les données doivent d'abord être préparées sur la machine cliente. L'exemple suivant utilise la bibliothèque Python `pandas` pour créer un DataFrame. Ce DataFrame sera ensuite téléversé vers le serveur CAS en utilisant la méthode `add_table()` du client SWAT, qui exécute l'action `addTable` en arrière-plan.
sas
import pandas as pd
import swat

# Établir la connexion au serveur CAS (remplacer avec vos informations)
s = swat.CAS('cas-controller.example.com', 5570, 'casuser', 'caspassword')

# Créer un DataFrame pandas localement
data = {
    'ID_Produit': [101, 102, 103, 104, 105],
    'NomProduit': ['Stylo', 'Cahier', 'Gomme', 'Règle', 'Crayon'],
    'PrixUnitaire': [1.2, 2.5, 0.5, 1.0, 0.8],
    'QuantiteStock': [200, 150, 300, 120, 250]
}
df_produits = pd.DataFrame(data)


## Exemple 1 : Téléversement simple d'un DataFrame
Ceci est l'utilisation la plus fondamentale. Le client SWAT pour Python fournit une méthode `add_table()` qui simplifie l'appel à l'action `addTable`. Elle prend le DataFrame, le sérialise, et l'envoie au serveur CAS pour créer une table en mémoire.
sas
# Le code suivant suppose que la connexion 's' et le DataFrame 'df_produits' de l'étape de création de données existent.
r = s.add_table(table='PRODUITS_SIMPLES', caslib='CASUSER', **df_produits.to_cas_params())
print(r)


## Exemple 2 : Remplacer une table existante et la promouvoir
Cet exemple utilise les paramètres `replace=True` pour écraser une table du même nom si elle existe déjà, et `promote=True` pour rendre la table accessible à toutes les autres sessions CAS (portée globale), à condition que la caslib 'Public' soit également globale.
sas
# Le code suivant suppose que la connexion 's' et le DataFrame 'df_produits' existent.
r_promu = s.add_table(table='PRODUITS_PROMUS', caslib='Public', replace=True, promote=True, **df_produits.to_cas_params())
print(r_promu)


## Exemple 3 : Partitionnement de la table lors du téléversement
L'action `addTable` permet de spécifier des options de table avancées, comme le partitionnement. Ici, nous créons une table plus grande et la partitionnons par la colonne 'Categorie' pour optimiser les requêtes qui filtrent sur cette colonne.
sas
# Création d'un DataFrame plus grand pour l'exemple de partitionnement
data_large = {
    'ID_Vente': range(1000),
    'Categorie': ['A']*500 + ['B']*500,
    'Montant': [x*1.5 for x in range(1000)]
}
df_ventes = pd.DataFrame(data_large)

# Téléversement avec partitionnement
r_part = s.add_table(table='VENTES_PARTITIONNEES', caslib='CASUSER', replace=True, partition=['Categorie'], **df_ventes.to_cas_params())
print(r_part)

