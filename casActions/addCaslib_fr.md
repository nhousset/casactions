# table.addCaslib

> L'action `addCaslib` est fondamentale dans SAS Viya car elle permet de déclarer une nouvelle bibliothèque de données CAS (Caslib). Une Caslib est un pointeur vers une source de données, qu'il s'agisse d'un chemin sur un système de fichiers, d'une base de données distante, ou d'un emplacement dans le cloud. Cette action ne charge pas les données en mémoire mais établit la connexion et les métadonnées nécessaires pour que le serveur CAS puisse y accéder. C'est la première étape indispensable avant de pouvoir charger des tables en mémoire à partir de cette source de données.

## Syntaxe
sas
table.addCaslib /
    activeOnAdd=TRUE | FALSE,
    createDirectory=TRUE | FALSE,
    dataSource={srcType='source' <, srcType-specific-parameters>},
    description='string',
    hidden=TRUE | FALSE,
    name='string',
    path='string',
    permission='permission-level' | integer,
    session=TRUE | FALSE,
    subDirectories=TRUE | FALSE,
    tableRedistUpPolicy="DEFER" | "NOREDIST" | "REBALANCE",
    transient=TRUE | FALSE;


## Paramètres

| Paramètre | Description |
| --- | --- |
| **activeOnAdd** | Si défini sur TRUE, la nouvelle caslib devient la caslib active pour la session en cours. Cela signifie que les actions futures qui ne spécifient pas de caslib utiliseront celle-ci par défaut. |
| **createDirectory** | Si défini sur TRUE, le répertoire spécifié dans le paramètre `path` sera créé s'il n'existe pas. Utile pour les caslibs de type PATH. |
| **dataSource** | Spécifie le type de source de données et ses paramètres spécifiques. Le paramètre `srcType` est crucial et détermine les autres options requises (par exemple, pour une base de données, il faudra des informations sur le serveur, l'utilisateur, etc.). |
| **description** | Une chaîne de caractères fournissant une description textuelle de la caslib, utile pour la documentation et la gestion. |
| **hidden** | Si défini sur TRUE, la caslib ne sera pas visible dans les listes de caslibs par défaut, la rendant 'cachée'. |
| **name** | Le nom unique à assigner à la caslib. Ce nom sera utilisé pour référencer la bibliothèque dans les futures opérations CAS. |
| **path** | Le chemin physique ou l'identifiant de la source de données. Pour une caslib de type PATH, c'est un chemin sur le système de fichiers du serveur CAS. Pour une base de données, cela peut être le nom de la base. |
| **permission** | Définit les permissions d'accès au niveau du système de fichiers lorsque `createDirectory` est utilisé. Peut être une chaîne de caractères prédéfinie (comme 'PUBLICREAD') ou une valeur octale numérique. |
| **session** | Si défini sur TRUE, la caslib est de portée session, ce qui signifie qu'elle n'est visible et utilisable que dans la session CAS actuelle et sera détruite à la fin de la session. |
| **subDirectories** | Si défini sur TRUE, permet à la caslib d'accéder aux fichiers et tables dans les sous-répertoires du chemin spécifié. |
| **tableRedistUpPolicy** | Définit la politique de redistribution des données pour les tables de cette caslib lorsque le nombre de nœuds workers du serveur CAS augmente. |
| **transient** | Si défini sur TRUE, la caslib est transitoire, ce qui signifie qu'elle n'est pas persistée et disparaîtra à la fin de la session. |

## Création de données de test
Pour illustrer l'utilisation de `addCaslib` avec une source de données de type 'PATH', nous créons d'abord un fichier CSV dans un répertoire temporaire. La caslib pointera ensuite vers ce répertoire pour rendre le fichier accessible au serveur CAS.
sas
/* Étape 1 : Créer un répertoire temporaire et un fichier CSV. Le chemin '/tmp/mydata' est un exemple. */
filename mycsv temp;
data _null_;
   file mycsv dsd dlm=',' lrecl=200;
   put 'Marque,Modele,Annee,Prix';
   put 'Toyota,Camry,2021,25000';
   put 'Honda,Accord,2022,27000';
   put 'Ford,Mustang,2023,45000';
run;

/* Note : Le code ci-dessus est exécuté dans une session SAS 9 ou SAS Viya (Compute Server). */
/* L'étape suivante, addCaslib, est exécutée dans une session CAS. */


## Exemples

### Ajouter une Caslib de type PATH (système de fichiers)
Ceci est l'utilisation la plus courante. On déclare une caslib qui pointe vers un répertoire sur le système de fichiers du serveur CAS. Tous les fichiers de données (comme des CSV, SAS7BDAT, etc.) dans ce répertoire deviennent alors accessibles.
sas
proc cas; table.addCaslib / name='MyPathCaslib' path='/tmp/mydata' dataSource={srcType='PATH'} description='Ma caslib basée sur un chemin'; run;

Une nouvelle caslib nommée 'MyPathCaslib' est créée. Le serveur CAS peut maintenant lire les fichiers depuis le répertoire '/tmp/mydata'. L'action renvoie une note de succès dans le journal SAS.

### Ajouter une Caslib pour une base de données Oracle
Cet exemple montre comment déclarer une caslib pour se connecter à une base de données Oracle. Il inclut des paramètres spécifiques à la source de données comme le serveur, le nom d'utilisateur, le mot de passe et le schéma. La caslib est définie comme non-session, ce qui la rend potentiellement disponible pour d'autres utilisateurs (selon les permissions).
sas
proc cas; table.addCaslib / name='MyOracleCaslib' dataSource={srcType='ORACLE', path='ORCL', user='myuser', password='mypassword', schema='MYSCHEMA'} description='Caslib pour la base Oracle' session=FALSE; run;

Une caslib nommée 'MyOracleCaslib' est créée et connectée à la base de données Oracle 'ORCL'. Les tables du schéma 'MYSCHEMA' sont maintenant listables et chargeables en mémoire CAS. Comme `session=FALSE`, cette caslib est persistante au-delà de la session actuelle, si l'utilisateur a les droits pour le faire.

### Ajouter une Caslib de session pour un chemin avec sous-répertoires
Cet exemple crée une caslib de type PATH qui est limitée à la session en cours (`session=TRUE`). De plus, elle permet d'explorer les sous-répertoires (`subDirectories=TRUE`), ce qui est utile pour organiser les fichiers de données dans une arborescence.
sas
proc cas; table.addCaslib / name='TempData' path='/cas/data/project_alpha' dataSource={srcType='PATH'} description='Données temporaires pour le projet Alpha' session=TRUE subDirectories=TRUE; run;

Une caslib de session nommée 'TempData' est créée. Elle permet d'accéder aux fichiers dans '/cas/data/project_alpha' et tous ses sous-dossiers. La caslib sera automatiquement supprimée à la fin de la session CAS.
