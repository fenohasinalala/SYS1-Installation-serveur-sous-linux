# SYS1 - INSTALLATION DU SERVEUR APACHE2
## Partie 1 - DEFINITION
**Apache** est un logiciel de serveur web gratuit et open-source qui alimente environ 46% des sites web à travers le monde. Le nom officiel est Serveur apache HTTP et il est maintenu et développé par Apache Software Foundation.

Il permet aux propriétaires de sites web de servir du contenu sur le web – d’où le nom « serveur web » -. C’est l’un des serveurs web les plus anciens et les plus fiables avec une première version sortie il y a plus de 20 ans, en 1995.

\(Source: [www.hostinger.fr, 2022](https://www.hostinger.fr/tutoriels/quest-ce-quapache-serveur-web-apache/)\)

## Partie 2 - INSTALLATION
Pour les manipulations sur le terminal, il est obligatoire de se connecter en tant que **root** soit d'utiliser la commande **sudo**.
### 2.1.	Installation du service apache2
Avant l'installation du service apache2, il est nécessaire de mettre à jour le système.
Sur le terminal et en tant que **root**, saisir la commande suivante:
```
apt update
apt install apache2
```

Pour vérifier l’état du service apache2:
```
systemctl status apache2
```

### 2.2.	Configuration du serveur apache2

La configuration du serveur permet de modifier le serveur pour qu'il corresponde à notre utilisation. 
Voir les détails sur les configurations possibles sur les liens ci-après [apache.org](https://httpd.apache.org/docs/2.4/) et [devdocs.io](https://devdocs.io/apache_http_server/).

#### 2.2.1  Les fichiers de configuration du serveur 
La configuration du serveur apache étant complexe, elle est séparée en plusieurs dossiers, ils se trouvent dans "/etc/apache2/".

![liste configuration](https://github.com/fenohasinalala/SYS1-Installation-serveur-sous-linux/blob/main/APACHE/img/apache2_config.png)
* **apache2.conf** : Fichier de configuration principale
* **mods-enabled** : Dossier qui contient les différents fichiers de configuration des différents modes
* **conf-enabled** : Dossier qui contient les fichiers de configuration supplémentaire
* **sites-enabled** : Dossier qui les fichiers de configuration des virtualhost 

Il est recommandé de sauvegarder la configuration par défaut:
```
cp /etc/apache2/apache2.conf /etc/apache2/apache2.conf.bak
```

#### 2.2.2  Ports
Les ports utilisés par apache sont accessibles par la commande : 
```
nano /etc/apache2/ports.conf
```
* **80** : port par défauts (utilisé automatiquement par les navigateurs si non spécifiés)
* **443** : port pour les sites https

#### 2.2.3 Configuration du virtualhost par défaut
Pour modifier la configuration du virtualhost par défaut:
```
nano /etc/apache2/sites-enabled/000-default.conf
```

![configuration virtualhost par défaut](https://github.com/fenohasinalala/SYS1-Installation-serveur-sous-linux/blob/main/APACHE/img/apache2_config_virtualhost.png)

#### *2.2.3.1. Description des paramètres du virtualhost*
Voici les paramètres de base de la configuration d'un virtualhost:
* **<VirtualHost *:80>** (Spécifier l’adresse IP et le port)
* **ServerAdimin : webmaster@localhost** (l’email à contacter en cas de problème)
* **DocumentRoot : /var/www/html** (dossier où se situent les fichiers qui constituent le site web)
* **ErrorLog** et **CustomLog** (dossiers d’emplacement des fichiers log)

#### *2.2.3.2. Changement du DocumentRoot pour un dossier spécifique*
Si nous voulons utiliser un dossier spécifique contenant les fichiers du site web. Il faut qu’apache a accès à ce dossier. Nous avons 02 options :
#### *a) Modification fichier de configuration apache2.conf*
Il faut spécifier le chemin du dossier dans le fichier de configuration 
"/etc/apache2/apache2.conf".
Pour qu’apache a accès au dossier "/home/lova/www/". Nous avons la commande suivante :
```
nano /etc/apache2/apache2.conf
```
Puis ajouter dans la partie **directory** :
```
<Directory /home/lova/www/>
    Options -Indexes +FollowSymLinks
    AllowOverride None
    Require all granted
</Directory>
```
Puis modifier le configuration du virtualhost "/etc/apache2/sites-enabled/000-default.conf" par:
```
nano /etc/apache2/sites-enabled/000-default.conf
```
Dans ce fichier de configuration, changer le chemin du dossier contenant les fichiers du site par :
```
DocumentRoot : /home/lova/www/
```

#### *b) Utilisation de lien symbolique :*
cette méthode est possible si et seulement si l'option FollowSymLinks est active sur le dossier accessible à apache, par exemple le dossier /var/www/ (voir image ci-dessous)

![configuration lien](https://github.com/fenohasinalala/SYS1-Installation-serveur-sous-linux/blob/main/APACHE/img/apache2_config_lien.png)

Pour ajouter un lien symbolique :
```
ln -s /home/lova/www /var/www/sitelova
```
Puis modifier le configuration du virtualhost "/etc/apache2/sites-enabled/000-default.conf" par:
```
nano /etc/apache2/sites-enabled/000-default.conf
```
Dans ce fichier de configuration, changer le chemin du dossier contenant les fichiers du site par :
```
DocumentRoot : /var/www/sitelova
```
#### *2.2.3.3. Application des changements effectués*
Pour appliquer les modifications apportées à un service, il faut recharger ou redémarrer le service:
```
systemctl reload apache2 #ou systemctl restart apache2
```
Pour vérifier l’état du service apache2:
```
systemctl status apache2 #ou service apache2 status
```

#### 2.2.4.	Créer et activer une nouvelle configuration virtualhost
Pour cet exemple nous supposons que nous possédons le nom de domaine "sitelova.mg" et qu'il est déjà hébergé sur un serveur.
Pour créer une nouvelle configuration virtualhost:
```
nano /etc/apache2/sites-enabled/001-sitelova.conf
```
Nous avons comme exemple de configuration
```
<VirtualHost *:80>
        ServerAdmin hei.lova.31@gmail.com
        ServerName sitelova.mg  #si possède le nom de domaine, déjà hébergé sur un serveur
        DocumentRoot : /var/www/sitelova
<Directory /var/www/sitelova>
        Options -Indexes +FollowSymLinks
        AllowOverride all
</Directory>

</VirtualHost>
```
Pour activer la configuration virtualhost:
```
a2ensite /etc/apache2/sites-enabled/001-sitelova.conf
```

Pour appliquer les changements de configuration sur le service apache2:
```
service apache2 reload
```

Vérification de la configuration virtualhost activée:
```
ls /etc/apache2/sites-enabled/ 
```

#### 2.2.5.	Outil de vérification d’erreur de configuration
Pour vérifier les erreurs de configuration, nous avons l’outil configtest d’apache2:
```
/user/sbin/apache2ctl configtest
```
Cette commande permet d'afficher les erreurs de configuration sur le serveur apache2.



## Partie 3 - ACCÈS AU SITE WEB 

Pour accéder au serveur, il faut connaitre l’adresse IP du serveur ou le nom de domaine si disponible.

l'adresse du serveur peut être récupérée par la commande suivant dans le terminal du serveur:
```
ip addr
```

À l'aide d'un navigateur, sur la barre d’adresse, il faut saisir l'adresse IP et le port du serveur (80 par défaut) soit le nom du domaine.
Exemple:
![navigateur](https://github.com/fenohasinalala/SYS1-Installation-serveur-sous-linux/blob/main/APACHE/img/apache2_navigateur.PNG)
