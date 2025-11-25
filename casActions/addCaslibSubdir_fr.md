# table.addCaslibSubdir

> L'action `addCaslibSubdir` permet de créer un nouveau sous-répertoire directement à l'intérieur du chemin physique d'une caslib existante. Cette fonctionnalité est particulièrement utile pour organiser les données et les artefacts au sein d'un projet sans avoir à définir de multiples caslibs. Elle est principalement utilisée avec des caslibs basées sur un chemin (type PATH, DNFS, etc.) où l'utilisateur dispose des permissions nécessaires sur le système de fichiers du serveur CAS pour créer des répertoires.

## Syntaxe
sas
table.addCaslibSubdir <result=results> <status=rc> / 
   name="string",
   path="string",
   permission="GROUPREAD" | "GROUPWRITE" | "GROUPWRITEPUBLICREAD" | "PRIVATE" | "PUBLICREAD" | "PUBLICWRITE" | integer;


## Paramètres
| Paramètre | Description |
|---|---|
| name | Spécifie le nom de la caslib dans laquelle le sous-répertoire sera ajouté. Alias : `lib`, `caslib`. |
| path | Spécifie le nom du sous-répertoire à créer. Ce chemin est relatif au chemin de base de la caslib. Ce paramètre est obligatoire. |
| permission | Définit les permissions d'accès au niveau du système de fichiers de l'hôte pour le nouveau sous-répertoire. Si non spécifié, les permissions sont déterminées par l'umask du processus de la session CAS. Les valeurs possibles sont : 'GROUPREAD' (Lecture pour le groupe, Lecture/Écriture pour le propriétaire), 'GROUPWRITE' (Lecture/Écriture pour le propriétaire et le groupe), 'GROUPWRITEPUBLICREAD' (Lecture pour tous, Lecture/Écriture pour propriétaire et groupe), 'PRIVATE' (Lecture/Écriture uniquement pour le propriétaire), 'PUBLICREAD' (Lecture pour tous, Lecture/Écriture pour le propriétaire), 'PUBLICWRITE' (Lecture/Écriture pour tous). Alias : `perms`. |

## Mise en Place de l'Environnement
Avant de pouvoir ajouter un sous-répertoire, nous devons d'abord définir une caslib de type PATH. Cette caslib pointera vers un répertoire sur le serveur CAS où nous avons les permissions nécessaires pour créer des sous-répertoires. Assurez-vous que le chemin '/cas/data/users/moi/temp' existe et que vous avez les droits d'écriture.
sas
cas; libname mycas cas; caslib mypathlib datasource=(srctype="path") path="/cas/data/users/moi/temp";


## Exemple Simple : Créer un sous-répertoire
Cet exemple montre comment créer un nouveau sous-répertoire nommé 'mon_projet' à l'intérieur de la caslib 'mypathlib' que nous avons précédemment définie.
sas
proc cas; table.addCaslibSubdir / caslib="mypathlib" path="mon_projet"; run;

### Résultat Attendu
L'action s'exécute avec succès. Un nouveau sous-répertoire nommé 'mon_projet' est maintenant créé dans le chemin physique associé à la caslib 'mypathlib'. Aucune table de résultat n'est retournée, mais une note de succès s'affiche dans le journal SAS.

## Exemple Détaillé : Créer un sous-répertoire avec des permissions spécifiques
Cet exemple va plus loin en créant un sous-répertoire nommé 'projet_partage' et en spécifiant des permissions d'accès. Nous utilisons l'option 'permission' avec la valeur 'GROUPWRITEPUBLICREAD' pour permettre au groupe propriétaire de lire et écrire, et à tous les autres utilisateurs de lire uniquement. C'est une pratique courante pour les espaces de travail collaboratifs.
sas
proc cas; table.addCaslibSubdir / caslib="mypathlib" path="projet_partage" permission="GROUPWRITEPUBLICREAD"; run;

### Résultat Attendu
L'action crée avec succès le sous-répertoire 'projet_partage'. Les permissions au niveau du système de fichiers du serveur CAS sont définies de manière à ce que le propriétaire et le groupe aient les droits de lecture/écriture, tandis que les autres n'ont que les droits de lecture. Le journal SAS affichera une note confirmant la réussite de l'opération.
