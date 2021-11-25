<h1 style="text-align:center">Inception Nginx</h1>
<body>
<h4>#Update des paquets</h4>
<code>➜  ~ apt-get update -y && apt-get upgrade -y</code>

<h4>#Installation des paquets requis<h4>
<h5 style="font-style: italic;">#Nginx et Openssl => nginx == service web ; Openssl == Certificat ssl, Tls<h5>

```➜  ~ apt-get install -y nginx openssl```

<h4>#Création certificat Tls, ils seront créés dans le dossier /root<h4>

```
➜  ~openssl req -nodes -x509 -newkey rsa:4096 -keyout /root/key.pem -out /root/cert.pem -days 365 <<EOF
FR
Lyon
Lyon
42
42
42
pseudo@student.42lyon.fr
EOF
```
<h5>#Les informations entrées correspondent aux questions:<h5>
<p style="margin-left:20px">
Country Name (2 letter code) [AU]: FR<br>
State or Province Name (full name) [Some-State]: Lyon<br>
Locality Name (eg, city) []: Lyon<br>
Organization Name (eg, company) [Internet Widgits Pty Ltd]: 42<br>
Organizational Unit Name (eg, section) []: 42<br>
Common Name (e.g. server FQDN or YOUR name) []: 42<br>
Email Address []: pseudo@student.42lyon.fr<br>
-nodes = Clé privé non crypte <br>
-x509 = Utilitaire de certificat openssl<br>
-newkey rsa:4096 = Création de la clé chiffré en RSA avec une taille de 4096Byte<br>
-keyout /root/key.pem = Chemin ou la clé privée sera créée<Br>
-out /root/cert.pem = Chemin ou le certificat sera créée<Br>
-days 365 = Période d’activation de la clé<Br>
EOF
Permet de pré remplir les questions
</p>

<h4>#Déplacement du certificat et de la clé à leur emplacement final</h4>

```
➜  ~mv cert.pem /etc/ssl/certs/
➜  ~mv key.pem /etc/ssl/private/
```
<h4>#Suppression des fichiers contenus dans /var/www/html</h4>
<code>➜  ~rm -rf /var/www/html/*</code>

<h4>#Configuration nginx /etc/nginx/sites-available/default</h4>
<code>➜  ~rm -rf /etc/nginx/sites-available/default</code>

<h4>#Mettre le fichier config au préalable en /etc/nginx/sites-available/default<h4>
<p style="margin-left:15px; font-style: italic;">Docker:</p>
<code>COPY ./ConFile/default /etc/nginx/sites-available/default</code>

<div style="diplay:inline-block; font-style: italic">Example de default: </div>

```
server {
	listen 443 default_server;
	listen [::]:443 default_server;
	ssl_certificate /etc/ssl/certs/cert.pem;
	ssl_certificate_key /etc/ssl/private/key.pem;

	root /var/www/html;
	index index.html index.htm index.nginx-debian.html index.php;

	location / {
			try_files $uri $uri/ =404;
	}
	location ~ .php$ {
			include snippets/fastcgi-php.conf;
			fastcgi_pass wordpress:9000;
	}
}
```
<h4>#Permettre l’accès au port spécifié</h4>
<p style="display: inline-block; font-weight: 600">Utiliser la règle EXPOSE dans docker</p>
<code>EXPOSE 3306</code>

<h4>#Démarrage de Nginx sans deamons</h4>

```➜  ~nginx -g 'daemon off;'```
</body>