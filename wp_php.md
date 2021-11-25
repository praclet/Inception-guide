<h1 style="text-align: center">Inception Wordpress & PHP</h1>
<body>
<h4>#Mise a jour des paquets</h4>

```➜  ~ apt-get update -y && apt-get upgrade -y```

<h4>#Installation des paquets nécessaires</h4>

```➜  ~ apt-get install -y wget php7.4-fpm php7.4-mysql```

<h3>#Spécifier la version de php qui vous est nécessaire, ici 7.4, mais cela peut être installé avec php7.3-fpm par exemple</h3>

<h4>#Création du dossier pour Wordpress</h4>

<code>➜  ~ mkdir -p /var/www/html</code>

<h6 style="color:black">#L’option -p va créer si nécessaire les parents au dossier demandé</h6>

<h4>#Téléchargement de wordpress</h4>

```➜  ~ curl -L https://wordpress.org/latest.tar.gz /root/latest.tar.gz```

<h4>#Décompression de l’archive</h4>

```➜  ~tar -xf /root/lastest.tar.gz /root/```

<h4>#Suppression de l’archive</h4>

```➜ ~rm -rf /root/lastest.tar.gz```

<h4>#Ajout du fichier wp-config.php modifier au préalable</h4>

<h4>#Le fichier doit être mis en /root/ option COPY docker les étapes qui suivent partiront du principe que cela est fait</h4>
<code>➜ ~ cp /root/wp-config.php /root/wordpress/</code><br>
<h6 style="color:black">#Le fichier wp-config.php s’appelle wp-config-sample.php par défaut</h6>

<h4>#Lancement des services php avec deamon puis fermeture pour ajout de config supplémentaire</h4>

```
➜ ~service php7.4-fpm start
➜ ~service php7.4-fpm stop
```
<h4>#Configuration php-fpm</h4>
<h5>#Modifier le fichier /etc/php/7.4/fpm/php-fpm.conf</h5>

<article style="font-weight: 600; font-style: italic; text-decoration: underline; diplay:inline-block;">Commenter la ligne pid = /run/php/php7.4-fpm.pid (les commentaire en php c’est un ; en début de ligne)</article>

<h4>#Modifier le fichier /etc/php/7.4/fpm/pool.d/www.conf</h4>

Remplacer la ligne
```
listen = /run/php/php7.4-fpm.sock
Par listen = wordpress:9000
```
Décommenter la ligne
```clear_env = no```<br>
<h5>#Définir sur "non" rendra toutes les variables d'environnement disponibles pour le code PHP</h5>
</body>