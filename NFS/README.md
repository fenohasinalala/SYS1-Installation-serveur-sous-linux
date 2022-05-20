# SYS1 - INSTALLATION DU SERVEUR NFS

## Partie 1 - DEFINITION
Le NFS, pour Network File System, est un protocole permettant à un ordinateur d'accéder à des fichiers extérieurs via un réseau. Développé dans les années 80, le NFS s'est ensuite imposé comme la norme en matière de serveur de fichiers.   

Le NFS permet à un utilisateur d’accéder, via son ordinateur (le client), à des fichiers stockés sur un serveur distant. Il est possible de consulter, mais aussi mettre à jour ces fichiers, comme s’ils étaient présents sur l’ordinateur client (c’est-à-dire comme des fichiers locaux classiques). Des ressources peuvent ainsi être stockées sur un serveur et accessibles via un réseau par une multitude d’ordinateurs connectés. Le NFS permet aussi un travail collaboratif sur un même document, ainsi que la sauvegarde et la centralisation de documents sur un même serveur. Ce système a été développé par l’ancien constructeur d’ordinateurs Sun Microsystems en 1984, aujourd’hui fusionné avec Oracle. Le NFS a été créé pour les systèmes Unix. Il existe maintenant des versions pour Windows et Mac OS. NFS est un standard ouvert, défini dans « Request for Comments » (RFC) permettant à quiconque de mettre en œuvre le protocole.

\(Source: [www.journaldunet.fr, 2019](https://www.journaldunet.fr/web-tech/dictionnaire-du-webmastering/1445280-nfs-en-informatique-definition-et-focus-sur-la-version-nfsv4/#:~:text=Le%20NFS%20permet%20%C3%A0%20un,comme%20des%20fichiers%20locaux%20classiques)\)

## Partie 2 - INSTALLATION DU SERVEUR
### 2.1.	Installation du service nfs
Avant l'installation du service nfs, il est nécessaire de mettre à jour le système.
Sur le terminal et en tant que **root**, saisir la commande suivante:
```
apt update
apt install nfs-kernel-server
```
Puis, créer un dossier de partage, pour notre cas nous allons le renommer en " dossier_partage " :
```
mkdir dossier_partage
```

### 2.2.	Configuration
Pour spécifier les autorisations et le dossier à partager, il faut modifier le dossier : /etc/exports :
```
cd /etc/
nano exports 
```
On y ajoute : 
```
/home/lova/dossier_partage/ *(sync,no_root_squash,rw)  # c'est le chemin du dossier partagé
```
            
* **\*** \(Liste des adresses IP qui y ayant accès\)
* **sync** \(permets au serveur NFS de répondre aux demandes juste après que la requête soit prise en charge par l’unité de stockage. C’est l'opposé de "**async**" \)             
* **no_root_squash** \(en tant que root, le client a les permissions root sur le répertoire\)
* **rw** \(Permission aux clients de lire et d'écrire\)
           
Pour démarrer le service nfs:           
```
systemctl start nfs-kernel-server          
```

## Partie 3 - INSTALLATION COTE CLIENT
### 3.1.	Installation
Pour accéder au serveur nfs, il faut installer le service suivant:
```
apt-get install nfs-common
```

### 3.2.	Monter le dossier
Il est nécessaire de créer un dossier de montage:
```
mkdir dossier_monte
```
Il faut vérifier s’il existe un disque à monter venant du serveur :
```
showmount -e 192.168.88.12 #(c'est l’adresse IP du serveur)  
```
Si le serveur a bien été installé, bien configuré et en marche, la réponse devrait montrer :
/home/lova/dossier_partage *              

Pour le monté :            
```
mount -t nfs 192.168.88.12:/home/lova/dossier_partage /home/lova/dossier_monte
```
Ici:            
* **192.168.88.12** (c'est l’adresse IP du serveur)           
* **:/home/lova/dossier_partage/** (c'est l’emplacement du dossier à partager dans le serveur )            
* **/home/lova/dossier_monte** (c'est l’emplacement choisi par le client pour monter le dossier)           

## Partie 4 - VÉRIFICATION
### 4.1.	Vérification du serveur vers les clients
Pour vérifier que l'installation fonctionne correctement, nous allons créer un fichier dans le dossier de partage du serveur et voir s’il sera accessible par le client.

Côté serveur, créons le fichier suivant:
```
nano /home/lova/dossier_partage/fichier_teste.txt
```
Côté client, si nous regardons dans le dossier monté, nous pouvons le fichier "fichier_teste.txt".   

### 4.2.	Vérification du client vers serveur
Autre vérification, nous allons modifier le fichier en tant que client et voir si les modifications seront appliquées et visibles par le serveur et par l'occasion d'autre client.

Ajoutons le texte "teste nfs - client vers le serveur" dans le fichier "fichier_teste.txt":              
```
nano /home/lova/dossier_monte/fichier_teste.txt
```

Côté serveur, nous pouvons constater que le fichier a bien été modifié, par la commande:             
```
cat /home/lova/dossier_partage/fichier_teste.txt
```


[Retour à l'accueil](https://github.com/fenohasinalala/SYS1-Installation-serveur-sous-linux)