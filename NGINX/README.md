# SYS1 - INSTALLATION DU SERVEUR NGINX
## Partie 1 - DEFINITION
NGINX est un logiciel open source pour le service web, le reverse proxying, la mise en cache, l'équilibrage de charge, le streaming multimédia, etc. Au départ, il s'agissait d'un serveur web conçu pour des performances et une stabilité maximales. En plus de ses capacités de serveur HTTP, NGINX peut également fonctionner comme un serveur proxy pour le courrier électronique (IMAP, POP3 et SMTP) et comme un reverse proxy et un équilibreur de charge pour les serveurs HTTP, TCP et UDP.

\(Source: [www.nginx.com](https://www.nginx.com/resources/glossary/nginx/)\)

## Partie 2 - INSTALLATION
### 2.1.	Installation du service nginx
Avant l'installation du service apache2, il est nécessaire de mettre à jour le système.
Sur le terminal et en tant que **root**, saisir la commande suivante:
```
apt update
apt install nginx
```
Pour vérifier l’état du service nginx:
```
systemctl status nginx
```

### 2.2.	Configuration
#### 2.2.1. Fichier de configuration:
Le fichier de configurations se situ dans le dossier "/etc/nginx/sites-enabled/"; pour modifier des paramètres de configuration:
```
nano /etc/nginx/sites-enabled/default
```

Sans commentaire le fichier se présente comme suit : 

```
server {
       listen 80 default_server;
       listen [::]:80 default_server;

       server_name _;

       root /var/www/html;
       index index.html index.nginx-debian.html;

       location / {
               try_files $uri $uri/ =404;
       }
}
```

Ici,

* **listen 80** (par défaut le port 80 est utilisé par le serveur ngnix)

* **server_name _;** (définis le nom du serveur, ou encore le nom de domaine.)     

* **root /var/www/html;** (Dossier où se trouvent les fichiers à charger)   

* **index index.html index.nginx-debian.html;** (ce traduit par ouvrir le fichier "index" , si il n'existe pas, ouvrir le fichier "index.html", sinon, ouvrir le fichier "index.nginx-debian.html")     

#### 2.2.2. Modification du site :
Dans le cas où nous ne modifions pas le chemin du dossier "root /var/www/html;" dans le fichier "/etc/nginx/sites-enabled/default".
Alors, les fichiers du site doivent être placés dans le dossier « /var/www/html » et c'est toute modification qui doit être mise à jour dans ce dossier.      

Pour appliquer les modifications apportées à un service, il faut recharger ou redémarrer le service:
```
systemectl restart nginx 
```

#### 2.2.3. Modification du nom de domaine :
Pour simuler et tester un nom de domaine local, il faut modifier de fichier "/etc/hosts"
en y ajoutant "127.0.0.1       sitelova.mg"  

Cela permet d'accéder au site en saisissant « sitelova.mg » dans la barre d'adresse.      

Dans le fichier "/etc/nginx/nginx.conf" ; dans http, il est écrit : "include /etc/nginx/sites-enabled/*;"       

Ce sont les fichiers présents dans le dossier "/etc/nginx/sites-enabled/" qui seront pris en compte par le serveur pour la configuration du http ;             

Nous allons configurer le serveur pour qu'il prenne en compte seulement notre nom de domaine. Dans le dossier "/etc/nginx/sites-enabled", nous allons créer le fichier suivant :             

```
$toush newserver.conf
nano newserver.conf
```
Nous y ajoutons les configurations suivantes :
```
server {
        listen 80;
        root /var/www/html;
        index index.html index.htm index.nginx-debian.html;
        server_name sitelova.mg;
        location / {
                try_files $uri $uri/ =404;
        }
```

## Partie 3 - ACCÈS AU SITE WEB 

Pour accéder au serveur, il faut connaitre l’adresse IP du serveur ou le nom de domaine si disponible.

l'adresse du serveur peut être récupérée par la commande suivant dans le terminal du serveur:
```
ip addr
```

À l'aide d'un navigateur, sur la barre d’adresse, il faut saisir l'adresse IP et le port du serveur (80 par défaut) soit le nom du domaine.           


[Retour à l'accueil](https://github.com/fenohasinalala/SYS1-Installation-serveur-sous-linux)