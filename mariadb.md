<h1 style="text-align:center"> Inception MariaDB </h1>

<h3 style="font-style: italic; margin-left:40px; color: #666666">#define BDD “base de donne” </h3>

## Mariadb:
<body style="">
<h4>#Installation</h4>

```➜  ~ apt-get install mariadb-server -y```

<h4>#Configuration de départ (pour automatisation regarde note Nginx << <p style="color:red; display:inline-block">EOF</p>)</h4>

```➜  ~ mysql_secure_installation ```
<article style="margin-left:20px">
Switch to unix_socket authentication [Y/n] y<br>
Change the root password? [Y/n] y<br>
<article style="margin-left:20px">New password: ….<br>
Re-enter new password: ….</article>
Remove anonymous users? [Y/n] y<br>
Disallow root login remotely? [Y/n] y<br>
Remove test database and access to it? [Y/n] y<br>
Reload privilege tables now? [Y/n] y<br>
</article>
<p style="color:red">EOF</p>

<h4>#Création d’une base de donnée d’un user et allocation des droits à la base de donnée au nouvel utilisateur</h4>

<h4 style="text-align:center; text-decoration: underline; font-weight:788; color:rgb(20, 33, 16)" >#Deux métope sont possibles </h4>

1) On rentre tous manuellement
2) On rentre toutes les informations dans un fichier .bdd que l’on inclura dans la base de donne qui va la crée automatiquement 

# #Manuellement dans MariaDB

<h4>#Connection à la BDD</h4>
Mysql -u USER -p database

<h5>#Dans notre cas on utilise root qui est l’admin<br>
#On ne spécifie aucune Database car root aura accès a toutes les BDD</h5>

```➜  ~ mysql -u root -p ```


<h4>#Création d’une Database</h4>

```MariaDB [(none)]>  CREATE DATABASE databaseName;```

<h4>#Création d’un Utilisateur</h4>

```MariaDB [(none)]> CREATE USER “UserName”@“%” IDENTIFIED BY “password”;```
<h5 style="margin-top:1px; color: rgba(0, 0, 0, 0.66)">#Dans la localisation (partie après @) on peut directement mettre “%”, qui signifie partout, équivalant à * en bash</h5>

<h4>#Accès a la base (Droits)</h4>

```MariaDB [(none)]> GRANT ALL PRIVILEGES ON databaseName.* TO “UserName”@“%”;```
<h5 style="margin-top:1px; color: rgba(0, 0, 0, 0.66)">#Ici on met tous les droits un total accès à la BDD et les tables qu’elles contiennent
#Pour mettre des droits plus précis après databaseName. Au lieux de mettre une étoile on spécifie ce dont l’user aura accès</h5>

<h4>#Mise a jour des changements effectués</h4>

```MariaDB [(none)]>  FLUSH PRIVILEGES;```

# #Création d’un ficher BDD.sql

```➜  ~ touch BDD.sql```

<h4>#Édition du fichier<br>
#On écrit exactement ce que l’on aurait écrit dans mysql</h4>

```
CREATE DATABASE databaseName;
CREATE USER “UserName”@“%” IDENTIFIED BY “password”;
GRANT ALL PRIVILEGES ON databaseName.* TO “UserName”@“%”;
FLUSH PRIVILEGES;
```
<h4>#Pour utiliser le fichier dans mysql (récupérer le fichier wordpress.sql)</h4>

```➜  ~ mysql -u root --skip-password < BDD.sql```

<h5>#Pour vérifier si la ou les BDD ont bien été créées </h5>

```➜  ~ mysqlshow```

<h5>#Pour voir les users<h5>
<h6 style="color: rgb(0,0,0)">#Se connecter</h6>

```
➜  ~ mysql -u root -p
MariaDB [(none)]> select user, host from mysql.user;
```

<h5>#Select user, host affichera les users et le host avec lequel ils ont été register <code>“USER”@“HOST”</code><br>
#Select peut afficher plein d’autres options, une souvent utilisé est password => select user, host, password from mysql.user;</h5>

<h4>#Les fichiers de configuration sont dans /etc/mysql</h4>
<h5><div style="margin-left:20px; display:inline-block">#my.cnf est le fichier principal</div><br>
/etc/mysql/my.cnf</h5>
<h6 style="color:black">#De ce fichier on peut changer le port de mariadb en supprimant le <div style="color: blue; display:inline-block">#</div> devant #port = 3306 (Généralement ligne 24)<br>
#Si cela est effectué il faut relancer le service => service mysql restart</h6>

<h4>#Pour changer le bind-address (accès à distance à la BDD) non natif</h4>
/etc/mysql/mariadb.conf.d/50-server.cnf<br>
Remplace la ligne : (Généralement ligne 30)

```
bind-address = 127.0.0.1
bind-address = 0.0.0.0
```
<h5>#Si le fichier est modifié, pense à relancer le service => service mysql restart
#Dans le cas ou ceci est modifier la BDD écoute toutes les IP qui lui envoient des requêtes, pense à faire un pare-feu</h5>
<code> ➜  ~ufw allow 3306</code><br>
<h6 style="color:black;">#Si une meilleur sécurité est nécessaire, <p style="display:inline-block; text-decoration: underline; font-weight:800">regarder IPTABLES</h6>
<h5>#Permettre l’accès au port spécifier</h5>
Utiliser la règle EXPOSE dans docker

```EXPOSE 3306```
</body>