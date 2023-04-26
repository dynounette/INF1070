# INF1070 Hiver 2023 - Travail pratique 2

**Ce travail est individuel (à faire seul).**

# Introduction
Ce document contient l'énoncé du travail pratique 2 donné dans le cadre du cours *INF1070 - Utilisation et administration des systèmes informatiques de l'Université du Québec à Montréal*.

L'objectif de ce travail pratique est d'effectuer des tâches de découverte, d'installation et d'administration d'un réseau informatique.
Ce travail nécessitera de lire de la documentation technique et de suivre les procédures et les instructions données.
Ce travail permet d'expérimenter quelques tâches effectuées habituellement par un administrateur système.

# Instructions générales
Pour ce travail pratique, nous vous fournissons un environnement contenant déjà plusieurs services pré-configurés.
Le principe est de découvrir des informations sur cet environnement, d'effectuer un ensemble de tâches de maintenance, ainsi que d'installer de nouveaux services.

# Répartition des points
| Mission | Tâche                           | Points |
|---------|---------------------------------|--------|
| M01     | Configuration initiale          | 10     |
| M02     | Repérage                        | 10     |
| M03     | Configuration du serveur web    | 15     |
| M04     | Création d'une base de données  | 15     |
| M05     | Migration Nextcloud             | 15     |
| M06     | Script de sauvegarde            | 15     |
| M07     | Lancement automatique du script | 10     | 
| M08     | Liste des fichiers Nextcloud    | 10     |
| M09     | Mission secrète (optionnelle)   | 10     |
|         | Total                           | 110    |

**Note**: La mission M09 est optionnelle et vous permet de récupérez des points perdus dans d'autres missions. Le total du TP est sur 100 points. Si vous dépassez les 100 points, vous aurez 100/100. Un score de 42/110 sera transformé en un score final de 42/100.

# M01: Mise en place de l'environnement de travail (10 points)
Avant tout, il vous faut récupérer l'environnement de travail dans votre machine virtuelle.
Suivez la première méthode afin d'installer cet environnement de travail dans votre machine virtuelle.
Si cela ne fonctionne pas, afin de pouvoir continuer avec le TP (sans les points accordés pour cette question), vous pouvez utiliser la seconde méthode.

## Docker-Compose
Effectuez les opérations suivantes, en donnant à chaque étape la ou les commandes effectuées.

  0. (Avant tout, assurez-vous d'avoir un environnement docker vide dans votre VM. Ce n'est pas un préréquis, mais c'est conseillé. Si apache est installé dans la VM, il est également conseillé de le désactiver ou de le désinstaller.)
  1. Installez le paquet `docker-compose`. Assurez vous également que `docker` et `curl` soient installés.
  2. Récupérez l'archive [tp2.tar.gz](https://soft.vub.ac.be/~qstieven/tp2.tar.gz) (cliquez pour télécharger).
  3. Extrayez l'archive
  4. Allez dans le dossier `tp2/` créé
  5. Exécutez `sudo chown -R 33:33 data/nextcloud`
  6. Lancez `docker-compose up -d`
  7. Éditez le fichier `/etc/hosts` afin d'y ajouter la ligne suivante :
  
```sh
127.0.0.1       nextcloud.localhost
```
  8. Vous pouvez vérifier que la mise en place de l'environnement s'est bien déroulée en lançant le script `tp2/checkM01.sh`. Exécutez ce script et donner son résultat dans votre rapport.

## Méthode de secours (0 points accordés)
Si vous n'arrivez pas à résoudre cette mission, téléchargez le binaire [m01](https://gitlab.info.uqam.ca/inf1070/231-tp2/-/blob/main/m01) et exécutez le depuis un terminal. Vous pouvez ensuite passer aux autres missions.

# M02: Repérage (10 points)

Faisons avant tout un bref repérage de notre installation.
Lors de la mission M01, plusieurs *containers* docker ont été créés.
Chaque *container* représente une machine d'un réseau dont vous êtes l'administrateur.
Répondez aux questions suivantes en donnant les commandes effectuées pour trouver ces informations.

  - **M01.1** Combien de *containers* dockers sont présents ? Combien sont actuellement en train de tourner ? Deux commandes sont attendues ici.
  - **M01.2** La plupart des services installés sont connectés dans un réseau docker appelé `tp2_default`. Vous pouvez inspectez les caractéristiques de ce réseau avec la commande `docker network inspect tp2_default`. Écrivez une expression régulière pour lister les adresses [IPv4](https://fr.wikipedia.org/wiki/IPv4) trouvées dans le résultat de cette commande. Donnez la conduite ainsi créée, ainsi que son résultat.
  
# M03: Configuration du serveur web (15 points)

Nous avons peu d'information sur le serveur web accessible dans ce réseau, à part qu'il est accessible sur `https://localhost`.
La commande `curl` va nous aider à effectuer des requêtes vers le serveur web.
Étant donné que les certificats dans notre réseau ne sont pas configuré, il faudra généralement passer l'option `-k` à `curl` pour désactiver la vérification de ces certificats.

  - **M03.1** Quel serveur web est utilisé ? Ces informations sont présentes dans les en-têtes (*headers*) de la réponse HTTP de celui-ci.
  - **M03.2** Connectez-vous en SSH sur la machine du serveur web, accessible sur le port `2222` de `localhost`. Connectez vous avec l'utilisateur `root`, dont le mot de passe est `root`. Quel commande avez-vous utilisé ?
  - **M03.3** Quels processus sont actifs sur le serveur web ? Quelle commande avez-vous utilisé ?
  - **M03.4** Où se trouve le fichier de configuration du serveur web ? Donnez le chemin ainsi que le contenu de ce fichier dans votre rapport et expliquez comment vous l'avez trouvé.
  
# M04: Création d'une base de données (15 points)
Un service de base de donnée est installé. Nous allons nous y connecter et y créer un nouvel utilisateur et base de donnée.

  - **M04.1**: Utilisez la commande `docker network inspect tp2_default` pour inspecter le réseau où se trouve le serveur de bases de données MariaDB. Quelle est l'adresse IPv4 de ce serveur ? Vérifiez que cette adresse soit accessible en utilisant la commande `ping`. Donnez la commande utilisée et son résultat, en envoyant 4 paquets vers cette machine.
  - **M04.2**: Installez un client MySQL ou MariaDB dans votre machine virtuelle (pas dans un conteneur). Après cette opération, vous devriez avoir un exécutable `/usr/bin/mysql` ou `/usr/bin/mariadb`.
  - **M04.3**: Connectez-vous à la base de donnée avec le client MySQL. Référez-vous à sa page de manuel ou à la documentation en ligne pour l'utilisation de celui-ci. Il vous faudra passer l'hôte (l'adresse IPv4) trouvée à l'étape M04.1, et les identifiants suivants :
    - Nom d'utilisateur : `root`
    - Mot de passe : `password`
  - **M04.4**: Ajoutez un utilisateur et une base de donnée en exécutant les commandes suivantes dans le client  (vous êtes libres d'adapter le nom d'utilisateur (`nextclouduser`), le mot de passe (`password`) et le nom de la base de donnée (`nextcloud`)):

```mysql
create database nextcloud;
create user 'nextclouduser'@'%' identified by 'password';
grant all privileges on nextcloud.* to 'nextclouduser'@'%';
flush privileges;
```
  Donnez bien le résultat de ces commandes dans votre rapport.
  - **M04.5**: Donnez le résultat de la commande suivante dans votre rapport (à lancer dans le client MySQL/MariaDB, en adaptant le nom d'utilisateur) : `show grants for 'nextclouduser'@'%';`

# M05: Migration de Nextcloud (15 points)
Un service Nextcloud (une plateforme d'hébergement de fichiers) est installé, mais est mal configuré : il utilise une base de donnée SQLite plutôt qu'une base de donnée MariaDB, ce qui risque de dégrader ses performances.
La mission ici est de migrer les données déjà enregistrée dans Nextcloud vers la base de donnée MariaDB.

  - **M05.1**: Utilisez la commande `docker network inspect tp2_default` pour inspecter le réseau où se trouve le serveur Nextcloud. Quelle est l'adresse IP du serveur Nextcloud ? Quelle est l'adresse IP du serveur MariaDB ?
  - **M05.2** Sur quels ports sont disponibles ces serveurs ? C'est-à-dire, quels ports sont accessibles sur leurs conteneurs respectifs.
  - **M05.3** Connectez-vous sur le serveur Nextcloud en utilisant docker, avec la commande `docker exec -it nextcloud /bin/bash`. La procédure de migration utilise un script nommé `occ`. Où se trouve le script `occ` ? Quel est le *shebang* de ce script ?
  - **M05.4** Installez le paquet `sudo`, sachant que ce serveur utilise le gestionnaire de paquets APT, et lancez le script `occ` avec `sudo -u '#33'` (c'est-à-dire, lancez la commande `sudo -u '#33' /chemin/vers/occ`). Cela devrait afficher les instructions du script sans rien lancer.
  - **M05.5** La documentation de Nextcloud contient une page [décrivant la procédure pour migrer la base de donnée](https://docs.nextcloud.com/server/stable/admin_manual/configuration_database/db_conversion.html). Effectuez cette procédure et donnez bien les commandes utilisées, en utilisant les informations suivantes :
    - Le nom d'hôte de la base de donnée (*hostname*) est `mariadb`
    - Le port utilisé par la base de donnée MariaDB est `3306`
    - Le type de base de donnée vers lequel nous convertissons est `mysql`
    - Si vous avez effectué la mission M04, utilisez le nom d'utilisateur, mot de passe, et nom de base de donnée créé lors de cette mission. Sinon, utilisez les informations suivantes :
      - Nom d'utilisateur de la base de donnée : `admin`
      - Mot de passe de la base de donnée : `password`
      - Nom de la base de donnée `nextcloudm05`
    
# M06: Script de backup (15 points)
Notre ensemble de serveur configuré avec docker utilise des *volumes* pour stocker les données importantes.
La liste des volumes docker peut être inspectée avec la commande `docker volume ls`.
Les points de montage des volumes d'un conteneur `<nom-du-conteneur>` peuvent être obtenus avec la commande `docker inspect -f "{{ range .Mounts }}{{ .Destination }} {{ end }}" <nom-du-conteneur>`

Nous allons mettre en place un système pour faire une sauvegarde automatique du contenu de chacun de ces volumes.

  - **M06.1** Donnez le résultat de la commande `docker volume ls` dans votre rapport.
  - **M06.2** Écrivez un script `backup.sh` qui effectue les tâches suivantes :
    - Pour chaque conteneur `<c-orig>` ayant un volume monté dans `<chemin>` :
      - Créer un conteneur temporaire `<c-tmp>` qui utilise le même volume que le conteneur en question
      - Faire une archive `.tar`, nommée avec le nom du conteneur et contenant tout le contenu de `<chemin>`, dans le dossier `/backup/`
      - Pour ce faire, vous pouvez utiliser la commande suivante pour créer le conteneur temporaire et exécuter une commande `cmd arg1 arg2` (à remplacer par ce que vous souhaitez exécuter) en dedans :
      ```sh
      docker run --rm --volumes-from <c-orig> -v $PWD:/backup alpine cmd arg1 arg2
      ```
      Le fichier créé dans `/backup` dans ce conteneur se retrouvera dans le répertoire courant, depuis où cette commande est exécutée.
    - Une fois tous les conteneur sauvegardés, regroupez les fichiers `.tar` dans une seule archive `backup.tar.xz`
    Donnez votre script shell dans votre rapport.
    - **Ne pas utiliser de *bashisme* dans votre script**
    - Vous pouvez faire les hypothèses suivantes :
      - Chaque conteneur a exactement un et un seul nom.
      - Chaque conteneur à sauvegarder dispose de 0 ou 1 volume.
      - Chaque volume est utilisé par exactement un seul conteneur.
    **Incluez votre script dans votre rapport directement** (collez-le dans votre fichier `.md`)
  - **M06.3** Exécutez votre script shell et donnez le résultat de la commande `tar tvf backup.tar.xz` dans votre rapport. (N'ajoutez pas de fichier, listez juste le résultat qu'affiche commande.)
  - **M06.4** Vérifiez la qualité de votre script avec la commande `shellcheck backup.sh` (il vous faudra installer un paquet pour obtenir cette commande). Assurez vous de n'avoir aucune erreur ni avertissement.
  
# M07: Lancement automatisé du script (10 points)
- **M07.1** On souhaite exécuter le script réalisé à la mission M06 tous les jours à 17:00, depuis la machine hôte (votre machine virtuelle). 
  Ajoutez une tâche `cron` pour faire cela. Référez vous aux pages de manuel de `cron` et `crontab`.
  - Si vous n'avez pas effectué la mission M06, lancez un script factice à la place.
  - Si vous n'avez pas la commande `crontab` sur votre système, assurez-vous d'installer le paquet `cron`.
- **M07.2** Est-ce que le service `cron` est activé dans votre machine virtuelle ? Donnez la commande utilisée pour trouver cete information.

# M08: Liste des fichiers Nextcloud (10 points)
L'ancien administrateur a mis un fichier très important dans Nextcloud qu'il souhaite récupérer. Il ne sait plus son nom et souhaiterait donc obtenir la liste des fichiers de son utilisateur Nextcloud.
Son nom d'utilisateur était `admin`, et son mot de passe, `admin`.

- **M08.1** Ouvrez un shell sur le docker du service Nextcloud avec `docker exec -it nextcloud /bin/bash`. Trouvez où sont stockés les fichiers de l'utilisateur `admin`. Où se situent ces fichiers sur le disque (donnez le chemin) ? Sur quel point de montage sont-ils ?
- **M08.2** Connectez-vous à l'interface web de Nextcloud en utilisant les identifiants de l'ancien administrateur. Pour cela, rendez-vous avec un navigateur dans votre VM sur `https://nextcloud.localhost` (il faudra accepter un certificat qui ne peut pas être vérifié, si vous avez un warning, cliquez sur "avancé" -> "accepter les risques et continuer"). Pouvez-vous retrouvez tous les fichiers listés à la mission M08.1 ? Si oui, comment; si non, pourquoi ?

# M09: Mission secrète (10 points, optionnel)
Une mission secrète est présente dans la VM. 
Elle requiert un peu de recherche, mais est entièrement solvable avec ce qui a été vu au cours.
Indice : il faudra aller voir dans les nuages.

# Modalités de remise

Votre travail doit être remis au plus tard le **dimanche 23 avril 2023 avant 23H55** par l'intermédiaire de la plateforme [Moodle](https://www.moodle.uqam.ca/). Vous ne devez remettre qu'un seul fichier Markdown nommé exactement  `solutions-tp2.md`.  Une pénalité de **2 points** par heure de retard sera appliquée, <u>jusqu'à un maximum de 24 heures</u>. **Après ce délai, AUCUN TP NE SERA ACCEPTÉ**.

**Aucune remise par courriel ne sera acceptée**, peu importe le motif. Plus précisément, **si vous envoyez votre travail par courriel, il sera considéré comme non remis**. Il est donc de votre responsabilité de vous assurer d'être capable de faire une remise par l'intermédiaire de Moodle quelques jours avant la remise.

# Le format Markdown

Le gabarit `solutions-tp2.md` que vous allez remettre doit respecter le format Markdown, qui est un format texte à balisage léger. Le site officiel qui décrit ce format se trouve [ici](https://daringfireball.net/projects/markdown/).

Faites bien attention de respecter le format actuel du gabarit et d'utiliser une syntaxe comparable à celle employée dans la mission 1, dont la solution complète est décrite, pour vous donner un exemple de ce qui est attendu. Vous pouvez vérifier le rendu de votre fichier `solutions-tp1.md` sur ce [site](https://dillinger.io/) par exemple, ou en utilisant la commande `pandoc -t pdf -o solutions-tp1.pdf solutions-tp1.md` (n'envoyez pas le PDF lors de la remise, seulement le `.md`).

# Barème de correction

Les points suivants seront considérés lors de l'évaluation:

- Votre travail sera calculé sur un total de 100 points.
- Les détails des points pour chaque mission sont repris plus haut dans ce fichier.
- Pour avoir la totalité des points d'une mission, il faut : avoir les bonnes commandes et donner une explication <u>détaillée</u> de la démarche suivie pour chaque mission ainsi que les fichiers éventuels lorsque c'est demandé.
- Une pénalité pouvant aller jusqu'à **10 points** pourra être appliquée si la qualité générale du fichier remis n'est pas adéquate (qualité des phrases, format du fichier, utilisation de Markdown, fautes de français, etc.)
- Les missions résolues mais sans explication et justification ne valent aucun points.
- Tel que mentionné plus haut, une pénalité de **2 points** par heure de retard sera appliquée, <u>jusqu'à un maximum de 24 heures</u>. **Après ce délai, AUCUN TP NE SERA ACCEPTÉ**.
