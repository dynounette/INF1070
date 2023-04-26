# INF1070 Hiver 2023 - Travail pratique 1

## Énoncé

Ce court document contient l'énoncé du travail pratique 1 donné dans le cadre du cours INF1070 - Utilisation et administration des systèmes informatiques de l'Université du Québec à Montréal.

L'objectif de ce premier travail pratique est de vous familiariser avec une utilisation avancée du shell.

Le travail est à réaliser **seul·e** (**pas en équipe**).

Dans un premier temps, vous devez télécharger les fichiers de l'énoncé.

## Travail à réaliser

L'objectif du travail consiste à compléter chacune des missions présentées, et à expliquer, de manière claire et détaillée, votre démarche pour résoudre les problèmes, en remplissant le gabarit fourni sous le nom [solutions-tp1.md](solutions-tp1.md).


## Missions à résoudre

Chaque mission est indépendante et le niveau de difficulté variable. Si une mission vous semble difficile, vous pouvez donc passer à la suivante puis revenir plus tard.


### Mission M01


1. Utilisez l'une des deux méthodes suivantes, au choix, pour télécharger le code source de Moodle

1.1 méthode 1 : git

- Utilisez la commande suivante pour télécharger le code

>~~~csh
> git clone --depth 1 https://github.com/moodle/moodle.git -b v4.1.1  moodle
>~~~ 

- Allez dans le dossier `moodle` et donner le résultat de la commande suivante dans le rapport

>~~~csh
> git log
>~~~ 

1.2 méthode 2 : wget et tar

- Utilisez la commande suivante pour télécharger

>~~~csh
> wget https://github.com/moodle/moodle/archive/refs/tags/v4.1.1.tar.gz
>~~~ 

- Utilisez la commande suivante pour extraire suivante pour extraire le code

>~~~csh
> tar -zxf v4.1.1.tar.gz
>~~~ 


2.  Répondez aux questions suivantes.


 - M01.a Quelle méthode avez-vous utilisé pour télécharger le code ?
 - M01.b Quel est le nom du dossier créé et contenant le code de Moodle ?
 - M01.c Allez à la racine du dossier créé et donnez le résultat de la commande suivante :

 >~~~csh
 > ls -d .g*
 >~~~


3. Si vous ne pouvez pas  télécharger le code avec l'une ou l'autre méthode expliquées plus haut, vous pouvez copier et coller le lien suivant sur la barre d'adresse du navigateur pour télécharger le code et continuer le TP. Dans ces conditions aucun point ne sera accordé pour la mission M01.

- [https://github.com/moodle/moodle/archive/refs/heads/master.zip](https://github.com/moodle/moodle/archive/refs/heads/master.zip)


### Mission M02


Édition du fichier de configuration


- Allez à la racine du dossier contenant le code source téléchargé (créé à la mission M01)
- À partir de cet endroit, effectuez l'opération suivante et donnez les commandes et les informations demandées

> M02.a Copier le fichier `config-dist.php` vers le dossier parent sous le nom `config-CODEPERMANENT.php` (remplacer CODEPERMANENT par votre code permanent).

> M02.b Utilisez la commande `vim`  pour éditer les lignes suivantes du fichier `config-CODEPERMANENT.php` crée précédemment en remplaçant  respectivement les mots 'moodle', 'username' et 'password' par votre code permanent en minuscule.

>>~~~csh
>> $CFG->dbname    = 'moodle';
>> $CFG->dbuser    = 'username';
>> $CFG->dbpass    = 'password';
>>~~~

> M02.c  Toujours à partir du même endroit, donnez la commande qui permet de trouver toutes les lignes du fichier config-CODEPERMANENT.php qui contiennent votre code permanent en minuscule. La commande doit respecter les contraintes suivantes:

>> - Pas de tube (Ne pas utiliser '|' )
>> - Afficher seulement les lignes du fichier qui contiennent votre code permanent en minuscule


### Mission M03

Donnez la commande pour créer le lien symbolique selon la spécification suivante.

 - Allez dans le dossier racine de code source téléchargé à la mission M01 et créer un lien symbolique nommé `config.php` et pointant vers le fichier `config-CODEPERMANENT.php` créé à la mission M02
 - Si vous n'avez pas réussi la mission M02, le lien doit pointer vers le fichier [configtest.php](configtest.php) fourni avec cet énoncé.


### Mission M04

Donnez la commande pour trouver le nombre suivant.

 - Soit l'arborescence du dossier de code source téléchargé à la mission M01. Si on ne considère que les sous arbres de profondeur 4 (exactement 4),  au total, il y a combien des dossiers (par opposition au fichier régulier)? La commande doit respecter les contraintes suivantes:

 >> - Pas de tube (Ne pas utiliser '|' )
 >> - On ne veut pas connaitre le nombre des fichiers (réguliers). On veut savoir seulement le nombre des dossiers.

### Mission M05 

Trouver toutes les occurence du mot 'Dougiamas' dans le sous dossier `admin` du dossier contenant le code source téléchargé à la mission M01. Pour chaque occurrence on affichera le chemin et le nom du fichier suivi de ' -> Ligne: ', suivi numéro de la ligne contenant cette occurence. La recherche doit être lancée  à partir de la racine du dossier de code source et se faire seulement dans le sous dossier `admin` (et ses descendants).  Voici un extrait de l'affichage attendu.

>~~~csh
> admin/settings/server.php -> Ligne: 22
> admin/settings/courses.php -> Ligne: 21
>~~~

La commande doit respecter les contrainte suivantes:


>> - La recherche doit être insensible à la casse (majuscule/minuscule) et se faire dans `admin` et tous les sous dossier de `admin`
>> - **Une seule conduite**
>> - **Au maximum** deux tubes
>> - L'affichage doit-être dirigé vers le fichier `M05.txt` à enregistré en déhors du dossier contennt le code source.

Donnez dans le rapport la commande tapée et le fichier M05.txt produit


### Mission M06


Extraction d'une fonction d'un gros fichier.

- soit le fichier [phptag.php](phptag.php) contenant la ligne suivante (téléchargez le dans votre machine virtuelle).


~~~csh
<?php
~~~

- Le dossier de code source téléchargé à la mission M01, a un fichier `lib/moodlelib.php` contient plusieurs fonctions avec souvent un commentaire de ce que fait la fonction. On aimerait extraire la fonction `get_home_page` avec les 5 lignes de commentaire avant sa déclaration et les 25 lignes de codes suivant la ligne de déclaration de la fonction pour former un nouveau fichier `get_home.php` ne contenant que cette fonction. 
- On demande la ligne de commande qui effectue ce qui suit:

> - Trouver la ligne qui contient *`function get_home`* dans le fichier  `lib/moodlelib.php` avec les 5 lignes précédentes et les 25 lignes suivantes
> - Afficher ces lignes précédées du contenu du fichier du fichier `phptag.php` 
> - Rediriger cet affichage vers  le fichier `get_home.php` qui sera placé dans le dossier parent du dossier des codes sources (la première ligne sera donc la ligne du fichier `phptag.php` et les lignes suivantes seront celles extraites du fichier `lib/moodlelib.php`)

- Il faudra respecter les contraintes suivantes.

>- Une seule conduite
>- Au **maximum** deux tubes
>- Ne pas utiliser des parenthèses


### Mission M07

Liste des fichier `.zip`

- Donner une commande qui permet de trouver la liste des tous les fichiers d'extension `.zip` dans le code source téléchargé à la mission M01
- Enregistrer cette liste dans le fichier M07.txt
- Donner le fichier M07.txt et la commande dans le rapport.
- Contraintes:

> - Pas de tube

### Mission M08

Compilation script 


- soit le fichier [moi.sh](moi.sh) contenant les lignes suivantes (téléchargez le dans votre machine virtuelle).


~~~csh
#!/bin/sh
id -un
~~~

- On vous demande d'utiliser la commande `shc` pour compiler le fichier `moi.sh` de sorte à enregistrer l'exécutable dans le fichier nommé `M08`.

>- Si vous n'avez pas la commande shc, veuillez installer le paquet correspondant
- Donner les précisions suivantes

>- M08.a: Donnez la commande que vous avez exécuté pour compiler ce script
>- M08.b: Après l'exécution de cette commande, combien des fichiers ont été créés ?
>- M08.c: Quel est le type de chacun des fichiers crées? Indiquez la commande qui vous a permis de trouver ces types
>- M08.d: Qu'est-ce que la commande `./M08`  affiche ?
>- M08.e: Donnez les fichiers créés lorsque vous remettez le rapport (voir les [modalités de remise](#modalités-de-remise))


### Mission M09

Manipulation des dates

- Le fichier `version.php` se trouvant dans le dossier racine du code source téléchargé à la mission M01 contient une variable `$release` dont la valeur a une parenthèse contenant le mot `Build` et qui indique la date à la quelle ce code à été rassemblé. ex:

~~~csh
$release  = '4.2dev (Build: 20230126)'
~~~

>> Dans cet exemple, le code a été rassemblé à la date du `20230126`. Dans ce format, les 4 premiers chiffre représente l'année, les deux suivants représentent le mois et es deux dernier représentent le jour.

>- M09.a À quelle date votre code à été rassemblé? (donner cette date telle qu'elle est écrite)
>- M09.b Combien de secondes se sont passées depuis **epoch** (`1970-01-01 00:00:00`) jusqu'à cette date? (on supposera que l'heure de construction du code est `00:00:00`)? Donner la commande utilisée pour déterminer ce nombre
>- M09.c Ajuster la date de dernière modification des données du fichier `version.php` pour correspondre à la date donnée au point `M09.a`. Donnez la commande utilisée.
>- M09.d Pour vérifier, affichez maintenant la date de dernière modification des données du fichier `version.php`. Donnez la commande utilisée et le resultat affiché
>- M09.e Combien de secondes se sont écoulées depuis *epoch* jusqu'à la dernière date de modification des données du fichier `version.php`? Donnez aussi la commande utilisée. 

>> Indice: M09.b et M09.e devrait donner le même nombre des secondes, via deux commandes différentes.

### Mission M10

Permissions

- Soit le fichier [moi.sh](moi.sh) contenant les lignes suivantes (téléchargez le dans votre machine virtuelle).

```csh
#!/bin/sh
id -un
```

- Donnez les commandes pour attribuer les permissions suivantes au fichier `moi.sh` selon la spécification de chaque sous question.
>- L'utilisateur propriétaire aura droit seulement à le lecture et exécution
>- Le groupe propriétaire aura droit à l'écriture seulement
>- Les autres utilisateur auront droit à la lecture seulement
>- On suppose que vous avez les privilèges nécessaire pour modifier les permissions du fichier.

>- M10.a: Donnez une commande qui attribue toutes ces permissions en une fois en utilisant les permissions en mode lettres (w,r,x)
>- M10.b: Donnez une commande qui attribut ces permissions en une fois en mode octal


### Mission M11

Taille des fichiers

- Trouvez les 5 plus gros fichiers du code source téléchargé à la mission M01 et dont le nom a l'extension ou contient `.php`. Il faut respecter les contraintes suivantes.
>- Utilisez la commande `du`
>- Une seule conduite et au maximum 3 tubes (c'est-à-dire, 3 pipes au maximum)
>- La taille des doit-être exprimé dans le système international. C'est à dire en puissance de 1000 et pas multiple de 1024.


>- M11.a: Donnez la commande utilisée
>- M11.b: Donnez le resultat de la commande

## Modalités de remise

Votre travail doit être remis au plus tard le **26 février 2023 avant 23H55** par l'intermédiaire de la plateforme [Moodle](https://www.moodle.uqam.ca/). Vous ne devez remettre qu'un seul fichier zip nommé exactement  `solutions-tp1.zip` et contenant le rapport `solutions-tp1.md` ainsi que d'autres fichiers demandés explicitement dans les missions.  Une pénalité de **2 points** par heure de retard sera appliquée, <u>jusqu'à un maximum de 24 heures</u>. **Après ce délai, AUCUN TP NE SERA ACCEPTÉ**.

**Aucune remise par courriel ne sera acceptée**, peu importe le motif. Plus précisément, **si vous envoyez votre travail par courriel, il sera considéré comme non remis**. Il est donc de votre responsabilité de vous assurer d'être capable de faire une remise par l'intermédiaire de Moodle quelques jours avant la remise.
<!--
Si vous êtes en équipe de deux, un seul des deux étudiants doit effectuer la remise.
-->
## Le format Markdown

Le gabarit `solutions-tp1.md` que vous allez remettre doit respecter le format Markdown, qui est un format texte à balisage léger. Le site officiel qui décrit ce format se trouve [ici](https://daringfireball.net/projects/markdown/).

Faites bien attention de respecter le format actuel du gabarit et d'utiliser une syntaxe comparable à celle employée dans la mission 1, dont la solution complète est décrite, pour vous donner un exemple de ce qui est attendu. Vous pouvez vérifier le rendu de votre fichier `solutions-tp1.md` sur ce [site](https://dillinger.io/) par exemple, ou en utilisant la commande `pandoc -t pdf -o solutions-tp1.pdf solutions-tp1.md` (n'envoyez pas le PDF lors de la remise, seulement le `.md`).

## Barème de correction

Les points suivants seront considérés lors de l'évaluation:

- Votre travail sera calculé sur un total de 100 points.
- Les missions M01, M03  valent 5 points chacune.
- Les missions M02, M04, M06 et M07, M10  valent 8 points chacune.
- Les missions M05, M11 valent 10 points chacune.
- Les missions M08, M09 valent 15 points chacune.
- Pour avoir la totalité des points d'une mission, il faut : avoir les bonnes commandes et donner une explication <u>détaillée</u> de la démarche suivie pour chaque mission ainsi que les fichiers éventuels lorsque c'est demandé.
- Une pénalité pouvant aller jusqu'à **10 points** pourra être appliquée si la qualité générale du fichier remis n'est pas adéquate (qualité des phrases, format du fichier, utilisation de Markdown, fautes de français, etc.)
- Les missions résolues mais sans explication et justification ne valent aucun points.
- Tel que mentionné plus haut, une pénalité de **2 points** par heure de retard sera appliquée, <u>jusqu'à un maximum de 24 heures</u>. **Après ce délai, AUCUN TP NE SERA ACCEPTÉ**.


