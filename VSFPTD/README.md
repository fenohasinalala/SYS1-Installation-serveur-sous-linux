# SYS1 - INSTALLATION DU SERVEUR VSFTPD
## Partie 1 - DEFINITION
**VSFTPD** \(**Very Secure FTP Daemon**\), créé en 2000, est un serveur FTP qui mise beaucoup sur la sécurité développée par [Chris Evans](https://fr.wikipedia.org/wiki/Chris_Evans), chargé de la sécurité de [Google Chrome](https://fr.wikipedia.org/wiki/Google_Chrome). Il est l'un des premiers logiciels serveur à mettre en œuvre la séparation des privilèges, minimisant ainsi les risques de piratage.  

Fonctions de VSFTPd :
* Configuration accessible
* Utilisateurs virtuels
* Adresses IP virtuelles
* Limitation de la bande passante
* IPv6
* Support du chiffrement au travers de SSL intégré FTPS, méthodes explicites et implicites supportées.

Source: [Wikipedia](https://fr.wikipedia.org/wiki/VsFTPd)

## Partie 2 - INSTALLATION
Pour les manipulations sur le terminal, il est obligatoire de se connecter en tant que **root** soit d'utiliser la commande **sudo**.
### 2.1.	Installation du service vsftpd
Avant l'installation du service vsftpd, il est nécessaire de mettre à jour le système.
Sur le terminal et en tant que **root**, saisir la commande suivante:
```
apt update
apt install vsftpd
```

Pour vérifier la version installée:
```
vsftpd -v
```
Pour vérifier l’état du service vsftpd:
```
systemctl status vsftpd
```

### 2.2.	Ajout nouveau utilisateur et dossier dédié au serveur
Pour ajouter un nouvel utilisateur (ici le nom d'utilisateur est **ftpuser**):

```
adduser ftpuser
```
Pour ajouter un dossier dans le dossier personnel du nouvel utilisateur :

```
mkdir /home/ftpuser/ftp
```
Pour changer la permission du dossier créé:
```
chown nobody:nogroup /home/ftpuser/ftp
chmod a-w /home/ftpuser/ftp
```
Pour vérifier les changements effectués:
```
ls -al /home/ftpuser/ftp
```
### 2.3.	Configuration du serveur vsftpd
La configuration du serveur permet de modifier le serveur pour qu'il corresponde à notre utilisation. L'emplacement du fichier de configuration du serveur vsftpd se trouve dans le chemin "/etc/vsftpd.conf".
Il est recommandé de sauvegarder la configuration par défaut:
```
cp /etc/vsftpd.conf /etc/vsftpd.conf.bak
```
Voir les détails sur les configurations possibles sur le lien ci-après [vsftpd.beasts.org](http://vsftpd.beasts.org/vsftpd_conf.html).
Pour configurer le serveur : 
```
nano /etc/vsftpd.conf
```
Pour appliquer les modifications apportées à un service, il faut recharger ou redémarrer le service:
```
systemctl reload vsftpd #ou systemctl restart vsftpd
```
Pour vérifier l’état du service vsftpd:
```
systemctl status vsftpd
```

## Partie 3 - Accès au serveur pour les clients
Pour accéder à un serveur FTP, il est possible d'y parvenir par ligne de commande ou par l'intermédiaire d'un logiciel ftp-client.
La méthode la plus simple pour se connecter au serveur c’est de télécharger un logiciel ftp-client, par exemple le logiciel 
[Client Filezilla](https://filezilla-project.org/)

Après avoir téléchargé le logiciel, pour accéder au serveur, il faut connaitre l’adresse IP du serveur. Il faut entrer la commande suivante dans le terminal du serveur:
```
ip addr #ou ip a
```
Ainsi que l’identifiant et le mot de passe d’un compte utilisateur (notamment que nous avons créé précédemment) ; soit, on peut se connecter anonymement si le serveur le permet.