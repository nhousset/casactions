# s3.addRegion

> L'action `addRegion` dans le jeu d'actions S3 est utilisée pour ajouter ou remplacer des régions personnalisées pour l'environnement S3 (Simple Storage Service) d'Amazon Web Services (AWS). Cela permet à SAS Viya de se connecter à des points de terminaison S3 qui ne sont pas des régions AWS standard, comme des déploiements S3 sur site (on-premises) ou d'autres fournisseurs de stockage compatibles S3. En définissant une région personnalisée, vous spécifiez l'hôte, le port et les paramètres SSL nécessaires pour que CAS puisse interagir avec ce stockage externe.

## Syntaxe
sas
s3.addRegion {
  host="chaîne",
  name="chaîne",
  nossl=TRUE | FALSE,
  port=entier_64_bits,
  region="chaîne",
  sslallowed=TRUE | FALSE,
  sslport=entier_64_bits,
  sslrequired=TRUE | FALSE
};


## Paramètres

| Paramètre | Description |
| --- | --- |
| `host` | Spécifie le serveur auquel CAS se connecte sur AWS. |
| `name` | Spécifie un nom unique pour la région que vous ajoutez ou remplacez. |
| `nossl` | Spécifie que SSL ou TLS est désactivé pendant le transfert de données. |
| `port` | Spécifie le port HTTP pour se connecter sans utiliser SSL. Si aucune valeur de port n'est spécifiée, la valeur par défaut est utilisée. |
| `region` | Spécifie le code de région de la région que vous ajoutez ou remplacez. |
| `sslallowed` | Spécifie que SSL est autorisé lors de la communication avec l'environnement S3. Cette valeur est ignorée si le paramètre SSLREQUIRED est spécifié. |
| `sslport` | Spécifie le port HTTP pour se connecter à S3 avec SSL. Si aucune valeur de port n'est spécifiée, la valeur par défaut est utilisée. |
| `sslrequired` | Spécifie que toutes les communications avec l'environnement S3 doivent être effectuées en utilisant SSL. |

## Création de Données
sas
/* Pas de code de création de données nécessaire pour cet exemple */


## Exemples

### Ajouter une région S3 personnalisée simple
Cet exemple montre comment ajouter une nouvelle région S3 personnalisée nommée 'mon-datacenter' qui pointe vers un serveur de stockage compatible S3 sur site.
sas
proc cas;
  s3.addRegion name='mon-datacenter' host='s3.mondatacenter.local';
run;


### Ajouter une région S3 personnalisée avec des options de port et SSL
Cet exemple ajoute une région S3 personnalisée pour un fournisseur de stockage cloud tiers. Il spécifie un nom, un hôte, un code de région, et configure les ports pour les connexions non-SSL et SSL, tout en exigeant que SSL soit utilisé.
sas
proc cas;
  s3.addRegion 
    name='autre-fournisseur-cloud' 
    region='custom-west-1' 
    host='storage.autrecloud.com' 
    port=8080 
    sslport=8443 
    sslrequired=true;
run;

