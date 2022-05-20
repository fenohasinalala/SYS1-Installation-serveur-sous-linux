# SYS1 - INSTALLATION DU SERVEUR SAMBA

## Partie 1 - DÉFINITION
Une des méthodes les plus courantes pour mettre Ubuntu et Windows en réseau est de configurer Samba en serveur de fichiers.     

La licence publique générale GNU permet l’implantation du protocole SMB (Server Message Block), qui a donné le nom à la suite Samba, dans les systèmes d’exploitation Linux et Unix.  Ce protocole de partage des ressources (SMB), aussi connu dans sa version plus récente CIFS pour Common Internet File System ou système de fichier Internet commun, était à l’origine utilisé comme réseau local Windows établissant des interactions avec des serveurs de fichiers, d’imprimantes et autres services.Grâce à une telle mise en œuvre, des ordinateurs sous Windows, Linux et Unix peuvent être connectés, de manière à pouvoir échanger des données ou utiliser conjointement des imprimantes et autres services. Peu importe si le serveur est installé sous Linux ou Unix, car la quatrième version du logiciel prend en charge le rôle d’Active Directory Domain Controllers, par lequel une autorisation et authentification centralisée des différents ordinateurs et des utilisateurs est possible.
\(Source: [www.ionos.fr, 2020](https://www.ionos.fr/digitalguide/serveur/configuration/samba-server-une-plateforme-dinteroperabilite/)\)

## Partie 2 - INSTALLATION
### 2.1.	Installation du service samba
Avant l'installation du service samba, il est nécessaire de mettre à jour le système.
Sur le terminal et en tant que **root**, saisir la commande suivante:
```
apt update
apt install samba
```
Puis, nous vérifions si l'installation est réussie:
```
whereis samba
```
ce qui doit afficher:
```
samba: /usr/sbin/samba /usr/lib/samba /etc/samba /usr/share/samba /usr/share/man/man7/samba.7.gz /usr/share/man/man8/samba.8.gz
```

### 2.2.	Configuration
Il faut créer un répertoire de partage:
```
mkdir /home/lova/sambapartage/
```

Le fichier de configuration pour Samba se trouve dans: **/etc/samba/smb.conf**.

Pour définir le nouveau dossier comme un dossier de partage samba, il est nécessaire de modifier le fichier "/etc/samba/smb.conf" :

```
nano /etc/samba/smb.conf
```

dans ce fichier de configuration, nous ajoutons :
```
[sambapartage]
    comment = Samba on Linux
    path = /home/<username>/sambapartage
    read only = no
    browsable = yes
```

Pour appliquer les changements effectués, il faut redémarrer ou recharger le service samba :
```
service smbd restart
```
Paramétrage du pare-feu ufw pour autoriser Samba :
```
ufw enable   
ufw allow samba   
ufw reload   
```

### 2.3.	Configuration de sécurité
Nous allons ajouter un mot de passe Samba pour notre compte utilisateur pour que notre installation soit sécurisé:
```
smbpasswd -a username
```
Puis redémarrer ou recharger le service samba  pour appliquer les changements effectués.


[Retour à l'accueil](https://github.com/fenohasinalala/SYS1-Installation-serveur-sous-linux)