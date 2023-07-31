# PROCESS DE DEPLOIEMENT 🚀 D'UNE APPLICATION
Pour déployer votre application, vous avez plusieurs techniques et outils possible selon votre type d'application et votre catégorie de serveur (serveur mutualisé, serveur dédié ...) et vos besoins. <br> 
NB : 
 - Si vous utilisez un serveur mutualisé(achat d'une partie de espace d'hébergement avec un fornisseur de service), généralement aucune config n'est néccessaire car votre founisseur se charge d'installer et de partager les parts de chaque utilisateur;
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
Un aperçu en image 
<img align="center" src="https://github.com/alban-okoby/images_projects/blob/main/template_portfolio/home.JPG" />
Notre utilisons un serveur Oracle Linux 8 ce qui veut dire que les commande vont commcer par ``` dnf ```

### I- Installation des outils
#### 1- Installez un JDK (Java 11 dans notre cas)
```
  sudo dnf install java-11-openjdk-devel
```
Un aperçu en image 
<img align="center" src="https://github.com/alban-okoby/images_projects/blob/main/template_portfolio/home.JPG" />
#### 2- Vérifiez si le JDK est parfaitement installé
```
  java -version
```
Un aperçu en image 
<img align="center" src="https://github.com/alban-okoby/images_projects/blob/main/template_portfolio/home.JPG" />

##### 3- Installation de PHP 7 (version 7.2 dans notre cas)
```
sudo dnf install php
```
Taper y pour accepter.
<img align="center" src="https://github.com/alban-okoby/images_projects/blob/main/template_portfolio/home.JPG" />git
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
Étape 4-3: Activer le démarrage automatique de Nginx
Si vous souhaitez que Nginx démarre automatiquement à chaque démarrage du système, exécutez la commande suivante pour activer le service au démarrage :
```
sudo systemctl enable nginx
```
Étape 4-4: Vérifier l'état de Nginx
```
  sudo systemctl status nginx
```
Étape 4-5: Configurer le pare-feu (firewalld) pour Nginx (facultatif)
Si vous avez activé le pare-feu firewalld sur votre système, vous devrez peut-être configurer les règles pour permettre le trafic HTTP (port 80) et le trafic HTTPS (port 443) pour Nginx. Pour autoriser le trafic HTTP, utilisez la commande suivante :
Si tout s'est bien passé, vous devriez voir un message indiquant que le service est actif et en cours d'exécution.
```
sudo firewall-cmd --add-service=http --permanent
```
Exemple :

Pour autoriser le trafic HTTPS, utilisez la commande suivante :
```
sudo firewall-cmd --add-service=https --permanent
```
Après avoir ajouté les règles, rechargez firewalld pour qu'elles prennent effet :
```
sudo firewall-cmd --reload
```
### II- Déploiement des applications

Sur votre navigateur aller à l'adresse 👉:  ``` adresse_de_votre_serveur:port ```
exemple : 192.168.25.25:8081 votre application est bien lancée 🚀

Bravo 👏🏼👏🏼! vous venez de déployer votre application sur un serveur dédié. 

#### Have Happy Works <\🎉>    <br> Alban 👋
