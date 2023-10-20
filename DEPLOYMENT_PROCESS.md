# PROCESS DE DEPLOIEMENT 🚀 D'UNE APPLICATION
Pour déployer votre application, vous avez plusieurs techniques et outils possible selon votre type d'application et votre catégorie de serveur (serveur mutualisé, serveur dédié ...) et vos besoins. <br> 
NB : 
 - Si vous utilisez un serveur mutualisé(achat d'une partie de l'espace d'hébergement avec un fornisseur de service), généralement aucune config n'est néccessaire car votre founisseur se charge d'installer et de partager les parts de chaque utilisateur;
 - Si vous utilisez un serveur dédié, le serveur vous appartient, vous devez donc faire toutes les config dont vous avez besoin 
    
## Cas pratique de déploiement d'une application Java/Spring 🌱 et Angular 🚀 sur un serveur dedié (Oracle Linux)
#### Par où commencer ?
Vous pouvez commencer par installer les outils dont vous avez besoin sur votre serveur.  <br>
Dans notre cas particulier nous aurons besoin de :
-  Vérifier la version du serveur Oracle Linux;
-  Installer un JDK (prenez la même version que celle de votre application );
-  Une version de PHP;
-  Installer et configurer un serveur Nginx (Ce que nous allons utiliser ) | ou apache;

NB : Pour un serveur Linux nous avons 2 commandes principales selon la version :
- yum (pour oracle 7 )
- dnf (pour Oracle 8+ ) <br>
#### Vérifiez la version de votre serveur
``` 
cat /etc/oracle-release
```
Un aperçu en image <br>
<img align="center" src="https://github.com/alban-okoby/devops-pratices/blob/main/images/oracle_version.PNG" /> <br>
Nous utilisons un serveur Oracle Linux 8 ce qui veut dire que les commandes vont commcer par ``` dnf ```

### I- Installation des outils
#### 1- Installez un JDK (Java 11 dans notre cas)
```
  sudo dnf install java-11-openjdk-devel
```
 
<img align="center" src="https://github.com/alban-okoby/devops-pratices/blob/main/images/jdk11_install.png" />

#### 2- Vérifiez si le JDK est parfaitement installé
```
  java -version
```
Un aperçu 👇👇
<img align="center" src="https://github.com/alban-okoby/devops-pratices/blob/main/images/jdk_version.png" />

#### 3- Installation de PHP 7 (version 7.2 dans notre cas)
```
sudo dnf install php
```
- Taper y pour accepter. A la fin de l'installation 👇 
- Vérifiez si php est bien installé :
```
php -v
```
<img align="center" src="https://github.com/alban-okoby/devops-pratices/blob/main/images/php_version.png" />

#### 4- Installation et configuration du serveur Nginx

Étape 4-1: Installer Nginx
```
sudo dnf install nginx
```
Étape 4-2: Démarrer le service Nginx
Une fois intallé, démarrez le serveur <br>
```
sudo systemctl start nginx
```
Étape 4-3: Activer le démarrage automatique de Nginx (pas obligatire mais recommandé ✅) <br>
Si vous souhaitez que Nginx démarre automatiquement à chaque démarrage du système(après coupure d'électricité par exemple), exécutez la commande suivante pour activer le service au démarrage :
```
sudo systemctl enable nginx
```
Étape 4-4: Vérifier l'état de Nginx
```
  sudo systemctl status nginx
```
 Vous devez voir 👇👇
 <img align="center" src="https://github.com/alban-okoby/devops-pratices/blob/main/images/nginx_status.png" />

Bien ! A ce stade vous devez pouvoir voir sur un navigateur web la page d'acceuil de votre serveur nginx à l'adresse de votre serveur exemple ```192.168.25.25```. <br> 🚧 Cependant si votre serveur contient un pare-feu 🚦 , il peut vous empêcher d'avoir accès ❌ 

Alors
 - Vérifiez d'abord l'existence d'un pare-feu. 
```
    sudo systemctl status firewalld
 ```
- [x] S'il n'existe aucun pare-feu 🚦❌ 👇
 <img align="center" src="https://github.com/alban-okoby/devops-pratices/blob/main/images/firewall_not_running.png" />
 
 - [x] Si le pare-feu existe 🚦 ✅ 👇
 <img align="center" src="https://github.com/alban-okoby/devops-pratices/blob/main/images/firewall_status.png" />

- NB : Si vous n'avez pas de pare-feu en exécution vous pouvez passer directement à l'étape II - DEPLOIEMENT DES PPLICATIONS

Au cas où il existe un parfe-feu sur votre serveur : 
Étape 4-5: Configurer le pare-feu (firewalld) pour Nginx (En cas de besoin)
Si vous avez activé le pare-feu firewalld sur votre système, vous devrez peut-être configurer les règles pour permettre le trafic HTTP (port 80) et le trafic HTTPS (port 443) pour Nginx. Pour autoriser le trafic HTTP, utilisez la commande suivante :

```
sudo firewall-cmd --add-service=http --permanent
```
Si tout s'est bien passé, vous devriez voir un message indiquant que le service est actif et en cours d'exécution.
Sinon si aucun pare-feu ne bloque l'accès, vous devrez voir un message du genre 👇👇 :

<img align="center" src="https://github.com/alban-okoby/devops-pratices/blob/main/images/firewall_not_running.png" />

Vous pouvez avoir besoin d'autoriser le trafic HTTPS également; <b>
- Pour autoriser le trafic HTTPS, utilisez la commande suivante :
```
sudo firewall-cmd --add-service=https --permanent
```
Après avoir ajouté les règles, rechargez votre pare-feu (firewalld) pour qu'elles prennent effet :
```
sudo firewall-cmd --reload
```
Bien ! A ce stade vous devez pouvoir voir sur un navigateur web la page d'acceuil de votre serveur nginx à l'adresse de votre serveur exemple ```192.168.25.25```.
👇👇
<img align="center" src="https://github.com/alban-okoby/devops-pratices/blob/main/images/ngin_accueil.png" />

Super, votre environement est prêt à recevoir vos applications (java et Angular)
### II- Déploiement des applications
Cette partie considère que vous avez déjà vos applications prêts pour la production (build déjà ok); <br>

Vous devez envoyer vos applications(java et angular) sur votre serveur, pour ce faire vous pouvez utiliser des outils tels : <br>
- [x] [MobaXterm](https://mobaxterm.mobatek.net/download.html) 
- [x] [FileZilla](https://filezilla-project.org/download.php?platform=win64)

Dans cet exemple nous utiliserons ``` MobaXterm ```
#### 1- Uploder le(s) fichier(s) de deploiement back (java)
Une fois connecté avec ``` MobaXterm ```, vous pouvez cliquez sur cette flêche comme le montre l'image :
<img align="center" src="https://github.com/alban-okoby/devops-pratices/blob/main/images/graphique_deploy1.PNG" />

Choisissez le(s) fichier(s) à uploder. 
<img align="center" src="https://github.com/alban-okoby/devops-pratices/blob/main/images/choose_jar.png" />

Après chargement.. <br>
Vous devez avoir le(s) fichier uplodé à la racine de votre serveur comme sur l'image 👇
<img align="center" src="https://github.com/alban-okoby/devops-pratices/blob/main/images/uploaded_jar.png" />

Vous pouvez le déplacer dans un dossier de votre choix, ensuite exécutez la commande suivante pour démarrer l'application java : <br>
```
java -jar <nom_de_votre_jar_ou_war>
```
exemple, dans notre cas le fichier est un OfarmJuillet2023.jar donc on aura : <br>
```
java -jar OfarmJuillet2023.jar
```
Ce qui donne le résultat suivant : <br>
<img align="center" src="https://github.com/alban-okoby/devops-pratices/blob/main/images/spring_running.png" />
Super ! votre application java est lancée, maintenant, faite pareil pour votre application angular <br>

Dans notre nous avons importé le fichier angular(le build compressé) sous le nom de uiJuillet2023.zip <br> Nous devons le déplacer dans un dossier de notre choix sur le serveur, mais il est recommandé de mettre les interfaces dans le dossier ``` var/www/html ```. <br>
- Deplacez donc votre fichier dans le repertoire en question : <br>
```
sudo mv <NOM_DU_BUILD_COMPRESSE_DE_VOTRE_APPLI> /var/www/html
```
Dans notre cas(nous considérons que votre fichier est à la racine du serveur : 
```
mv /root/uiJuillet2023.zip /var/www/html
```
Ensuite entrez dans le repertoire où vous avez déplacé le compressé de l'application
```
cd /var/www/html
```

- Puis, décompressz le fichier (unzip pour .zip et unrar pour .rar) <br>
```
unzip <NOM_DU_BUILD_COMPRESSE_DE_VOTRE_APPLI>
```
Ce qui donne dans notre cas : 
```
unzip uiJuillet2023.zip
```
- Entrez dans le dossier décompressé pour lancer l'application : <br>
```
cd /var/www/html/uiJuillet2023
```
- Lancez l'application Angular 🚀
```
php -S 0.0.0.0:PORT_LIBRE_DE_VOTRE_CHOIX
```
Dans notre cas lançons l'appli sur le port 87;
```
php -S 0.0.0.0:87
```
Sur votre navigateur aller à l'adresse 👉:  ``` adresse_de_votre_serveur:port_choisi ```
exemple : 192.168.25.25:87 votre application est bien lancée 🚀

Bravo 👏🏼👏🏼! vous venez de déployer votre application sur un serveur dédié. 

#### Have Happy Works <\🎉>    <br> Alban 🐱‍👤
