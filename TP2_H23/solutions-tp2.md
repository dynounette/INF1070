# Remise du travail pratique 2

Travail individuel.

## Identification
- Cours      : Utilisation et administration des systèmes informatiques
- Sigle      : INF1070
- Session    : Hiver 2023
- Groupe     : `020`
- Enseignant : `Quentin Stiévenart`
- Auteur     : `Dyna Mehenni` (MEHD84580003)


## Solution de la mission M01

### État de la mission : résolue

### Démarche

1.J'ai utilisé la commande `sudo apt-get install docker-compose docker curl` pour installer `docker-compose`, `docker` et `curl` .
2.J'ai récupérer l'archive `tp2.tar.gz`
3.J'ai extrait l'archive avec la commande `tar -xzf`
4.J'ai accedé au fichier tp2 avec la commande `cd tp2`
5.J'ai exécutée la commande `sudo chown -R 33:33 data/nextcloud`
6.J'ai lancé la commande `docker-compose up -d`
7.J'ai utilisé la commande `sudo nano /etc/hosts` pour editer le fichier text hosts, (j'ai préféré utiliser l'editeur de texte nano car je le connais mieux) et j'ai ajouté la ligne suivante : `127.0.0.1 nextcloud.localhost`.
8.J'ai utilisé la commande `bash tp2/checkM01.sh` vérifier que la mise en place de l'environnement s'est bien déroulée et la commande m'a retournée : `Tout fonctionne correctement.` ce qui a conclu la mission01.

## Solution de la mission M02

### État de la mission : résolue

### Démarche

M02.1:J'ai utilisé la commande `sudo docker ps -a` pour afficher tous les containers qui sont present grace à l'option `-a` ce qui ma affiché qu'il y a trois containers Docker présents.
Ensuite, j'ai utilisé la commande `sudo docker ps` pour afficher les containers qui sont entrain de tourner, ce qui ma affiché qu'il y a deux containers Docker en train de tourner.

M02.2: Pour lister les adresses IPv4 trouvées dans le résultat de la commande `docker network inspect tp2_default`, j'ai utilisé le regex suivant `(25[0-5]|2[0-4][0-9]|[01]?[0-9][0-9]?)(.(25[0-5]|2[0-4][0-9]|[01]?[0-9][0-9]?)){3}`
Le résultat de cette conduite est: 
```sh
Subnet: 172.19.0.0/16, Gateway: 172.19.0.1 IPv4Address: 172.19.0.2/16,
```
Ce qui conclu la mission02

## Solution de la mission M03

### État de la mission : résolue

### Démarche

M03.1 Pour obtenir les en-têtes HTTP de la réponse du serveur web, j'ai utilisé la commande `curl` avec l'option `-I` pour n'obtenir que les en-têtes. En exécutant la commande suivante : `curl -I -k https://localhost` Le résultat est: 
```sh
alt-svc: h3=":443"; ma=2592000 content-type: text/plain; charset=utf-8 server: Caddy content-length: 13 date: Mon, 10 Apr 2023 08:00:58 GMT
```
Ainsi, le server utilisé est `Caddy`

M03.2 J'ai utilisé la commande suivante : `ssh -p 2222 root@127.0.0.1` pour me connecter en SSH sur la machine du serveur web. J'ai indiqué le port de `localhost`, soit `2222`, l'identifiant que l'on m'a donnée `root` et l'adresse IP de localhost, soit `127.0.0.1`. En entrant la commande, on ma demandé de rentrer un mot de passe, soit `root`. 

M03.3 J'ai utilisé la commande `ps aux` pour trouver les processus actifs sur le serveur web. Cette commande nous affichera la liste de tous les processus actifs sur le système, y compris ceux du serveur web. Les processus actifs sont: `sshd` avec PID 1 `caddy` avec PID 8 `sshd` pour la session SSH avec PID 25 `un shell interactif` avec PID 27 `la commande ps` avec PID 28

M03.4 J'ai lancé la commande `cd /etc/` pour me diriger vers le répértoire où se trouve les fichiers de configuration . J'ai utilisé la commande `ls` pour lister les fichiers de configuration dans le répertoire `/etc/`.J'ai utilisée la commande `cd conf.d` suivi de `cat sshd` pour aller dans le fichier en question. Dans le fichier sshd, on nous donne le chemin vers le fichier de configruation du démon SSH: `/etc/init.d/sshd`. Ainsi, le chemin pour trouver le fichier de confugration est `/etc/conf.d/sshd`

Ce qui conclu la mission03

## Solution de la mission M04

### État de la mission : résolue

### Démarche

M04.1 J'ai utilisé la commande `docker network inspect default tp2_default`, ce qui m'a permis d'inspecter le serveur de base données MariaDB et pouvoir consulter les informations concernant la base données MaraiaDB comme son adresse IPv4. L'adresse IPv4 de cette base de donnée est : `172.18.0.2`. Ensuite, j'ai utilisé la commande `ping 172.19.0.2 -c 4` pour envoyer 4 paquets vers la machine. Le résultat est: 
```sh 
PING 172.19.0.2 (172.19.0.2) 56(84) bytes of data. 64 bytes from 172.19.0.2: icmp_seq=1 ttl=64 time=0.172 ms 64 bytes from 172.19.0.2: icmp_seq=2 ttl=64 time=0.104 ms 64 bytes from 172.19.0.2: icmp_seq=3 ttl=64 time=0.097 ms 64 bytes from 172.19.0.2: icmp_seq=4 ttl=64 time=0.097 ms

--- 172.19.0.2 ping statistics --- 4 packets transmitted, 4 received, 0% packet loss, time 3058ms rtt min/avg/max/mdev = 0.097/0.117/0.172/0.031 ms
```

M04.2 J'ai utilisée la commande `sudo apt-get update` pour mettre à jour les paquets disponibles. Ensuite, j'ai installé le client Mariadb avec cette commande: `sudo apt-get install mariadb-client`. 

M04.3 Je me suis connecté à la base de données avec le client MySQL avec comme nom d'utilisateur `root` : `mysql -h 172.18.0.2 -u root -p`, j'ai rentré le mot de passe `password` et je suis rentré dans la base de donnée nommé MariaDB. 

M04.4 Lorsque j'ai rentrée les commandes suivantes :
```sh
create database nextcloud;
create user 'Dyna'@'%' identified by 'password';
grant all privileges on nextcloud.* to 'Dyna'@'%';
flush privileges;
```
Les comamndes m'ont retournée : 
```sh
Query OK, 0 rows affected (le millième de secondes dépendamment de chaque commande Ex: 0,006 sec). 
```
Cette ligne signifie que la requête à été effectuée avec succès.

M04.5 Le réustlat de la commande `show grants for 'nextclouduser'@'%';`, soit `show grants for 'Dyna'@'%';`  dans mon cas. La commande ma retournée :
```sh
+------------------------------------------------------------------------------------------------------+
| Grants for Dyna@%                                                                                   |
+------------------------------------------------------------------------------------------------------+
| GRANT USAGE ON *.* TO `Dyna`@`%` IDENTIFIED BY PASSWORD '*2470C0C06DEE42FD1618BB99005ADCA2EC9D1E19' |
| GRANT ALL PRIVILEGES ON `nextcloud`.* TO `Dyna`@`%`                                                 |
+------------------------------------------------------------------------------------------------------+
2 rows in set (0,004 sec) 
```
Ce qui conclu la mission04

## Solution de la mission M05

### État de la mission : résolue, 

### Démarche

M05.1 J'ai utilisé la commande `docker network inspect default tp2_default`pour inspecter le serveur de base données Nextcloud et pouvoir consulter les informations concernant la base données MaraiaDB comme son adresse IPv4. L'adresse IPv4 de cette base de donnée est : `172.18.0.3` et l'adresse de MariaDB est : `172.18.0.2`

M05.2 J'ai utilisé la commande `docker` pour lister tous les conteneurs sur le sytème avec leur informations incluant leurs ports respectifs dans la colonne `port`. Ainsi, le port de MariaDB est: `3306` et celui de nextcloud est: `80`, soit un port utilisé pour les serveurs web. 

MO5.3 Je me suis connecter au serveur nextcloud avec la commande `docker exec-it nextcloud /bin/bash`. Le script `occ` se trouve dans le répértoire actuel.Le chemin absolu du script`occ` est : `/var/www/html/occ` . J'ai lancé la commande `ls` pour trouver le script et `occ` s'y trouvait juste en dessous de `nextcloud-init-sync.lock`.Ensuite, j'ai utlisée la commande `head -n 1 occ` pour trouver le shebang du script: `#!/usr/bin/env php`.  

M05.4 J'ai installé `sudo` avec la commande `apt install sudo` sachant que le serveur utlisait le gestionnaire de paquet APT. J'ai tapé la commande `sudo -u '#33' /var/www/html/occ` sur le terminal et cette commande m'a retournée les instructions du script en question. 

M05.5 J'ai utilisé la commande : `sudo -u www-data php occ db:convert-type --port 3306 --password password --clear-schema --all-apps --chunk-size CHUNK-SIZE --  mysql admin mariadb nextcloudm05` et cela m'a donc permis de migrer la base de donnée de façon efficace. 

Ce qui conclu la mission05

## Solution de la mission M06

### État de la mission : résolue, partiellement résolue, non résolue
### État de la mission : résolue

### Démarche

M06.1 La commande docker volume `ls` m'a listé les volumes Docker existant sur ma machine local. Dans mon cas, j'en ai 3. Cette commande permet de lister chaque volume avec ses informations comme leurs nom et leur driver : 
```sh
 DRIVER    VOLUME NAME
local     tp2_caddy_data
local     tp2_mariadb
local     tp2_nextcloud
```

MO6.2J'ai écrit un script `backup.sh` qui fais toutes les tâches m'étant demandée dans les énoncés :
```sh
#!/bin/bash

# Création du répértoire "Backup" 

backup_dir="/backup"
sudo mkdir -p  $backup_dir

# Code principal

for container_id in $(docker ps -q); do


  container_name=$(docker inspect --format '{{.Name}}' "$container_id" | cut -c 2-)

  volume=$(docker inspect -f  "{{ range .Mounts }}{{ .Destination }} {{ end }}" "${container_name}")

 # Création d'un conteneur temporaire qui utilise le même volume que le conteneur d'origine

  docker create --volumes-from "$container_id" --name "${container_name}"_backup alpine >/dev/null

 # Créez une archive de sauvegarde nommée d'après le conteneur et contenant le contenu du volume monté

  docker run --rm --volumes-from "${container_name}"_tmp -v $backup_dir:/backup alpine tar -czf /backup/"${container_name}".tar "$volume"

# Créez une archive de sauvegarde nommée backup.tar.xz qui contient toutes les archives tar

tar -cJf /backup/backup.tar.xz ./*.tar

 # Supprimez le conteneur temporaire

  docker rm "${container_name}"_backup >/dev/null
```

MO6.3 J'ai exécuté mon script shell avec la commande `./backup.sh` et j'ai ensuite lancé la commande `tar tvf backup.tar.xz`. La commande `tar tvf backup.tar.xz` ne m'a rien retournée sur le terminal. 
""
MO6.4 J'ai lancée la commande `shellcheck backup.sh` pour m'assurer vous de n'avoir aucune erreur ni avertissement.

Ce qui a conclu la mission06.


## Solution de la mission M07

### État de la mission : résolue

### Démarche

M07.1 J'ai utilisé la commande `crontab -e` pour éditer la table des tâches de cron pour l'utilisateur, soit moi, et ce, concernant le script. Enfin, en tapant la commande le système m'a demandée de choisir un éditeur texte entre les 4 choix proposées et j'ai choisi l'éditeur vim, doncle choix 2. Puis, j'ai modifié la date laquelle le script sera exécuté, donc à 17h00 à chaque jour ainsi: `0 17 * * * /home/dh991951/backup.sh`. J'ai ensuite effectuer la commande `crontab -l`. L'option `-l` de crontab, ce qui m'a permis d'afficher la liste des tâches de cron précédemment modifié pour qu'elles soient exécutées chaque jour à 17h00 et la commande m'a sortie :`0 17 * * * /home/dh991951/backup.sh` . La modification a été effectuée avec succès. 

MO7.2 En tapant la commande `systemctl status cron`, la commande m'a donnée les informations et le statut de la commande à ce moment et la commande était active sur mon système : 
```sh
systemctl status cron
● cron.service - Regular background program processing daemon
     Loaded: loaded (/lib/systemd/system/cron.service; enabled; vendor preset: enabled)
     Active: active (running) since Wed 2023-04-19 13:04:12 EDT; 4 days ago
```

## Solution de la mission M08

### État de la mission : résolue

### Démarche

M08.1 J'ai ouvert un shell sur le docker du service nextcloud avec `docker exec -it nextcloud /bin/bash`. J'ai d'abord lancé la comamnde `grep admin /etc/passwd` pour trouver le chemin des fichiers de l'utlisateur admin. Le chemin où se trouve les fichiers admin sont sur ce chemin: `/var/www/html/data/admin'` et le point de montage est: `/dev/sda5/`. Je l'ai trouvée avec la commande `df -h /var/www/html/data/admin` . La commande `df -h` ma permis de receuillir de l'information sur les systèmes de fichiers comme le point de montage, la taille du systèeme de fichier en question, etc.

M08.2 Je me suis connectée avec l'utilisateur et le mot de passe admin sur l'interface web `https://nextcloud` pour retoruver les fichiers trouvés à la mission08.1 . J'ai essayé d'aller dans les applications de nextcloud, puis dans `files`pour voir si j'avais pu être en mesure de retrouver ces fichiers. Je n'ai pas été en mesure de les acceder. Il est possible que les fichiers n'aient jamais été téléchargées sur le serveur nextcloud ou qu'ils ont peut être été téléchargés, puis supprimés.
