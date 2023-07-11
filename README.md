# devops-pratices
BEST PRATICES AND THINKS YOU ARE ASPIRING IN DEVOPS, LIKE DOCKER, JENKINS, GITHUB ACTIONS AND MANY MORE..

## I- DOCKER 
### 1 - La philosophie Docker ?
-  C'est quoi docker ?
-  Pourquoi l'utiliser ?
-  Comment l'utiliser ?
-  Dans quel cas l'utiliser favorablement ? <br>
Aller faut s'y mettre toute suite !

### 2- Installation de Docker
Selon votre système d'exploitation (Mac Win ou Linux)
#### 2-1 Windows
- Pour Windows : téléchargez-le sur
[docker hub](https://docs.docker.com/desktop/install/windows-install/)

Une fois le pack téléchargé, vous pouver lancer l'installation de plusieurs façons <br>
##### Soit en faisant un double click sur le pack téléchargé
##### Soit en utilisant Docker lui-même
``` 
"Docker Desktop Installer.exe" install"
```
##### Soit en utilisant l'invite de commande PowerShell de windows
```
Start-Process 'Docker Desktop Installer.exe' -Wait install
```
##### Soit en utilisant l'invite de commande natif CMD de windows
```
start /w "" "Docker Desktop Installer.exe" install
```
Une fois intallé, vous pouvez passer au cas pratique <br>

### 3- Cas pratique
Pour ce cas pratique vous aurez besoin de : 
- [x] GIT
- [x] DOCKER

#### Etape 1 Cloner le projet
Ici nous réccupérons le projet de démarrage de docker de cette façon : 
```
    git clone https://github.com/docker/getting-started.git
```
#### Etape 2 Créez l'image du conteneur de votre application
A la racine de votre projet créez un fichier nommé ``` Dockerfile ```
nous pouvons le faire manuellement ou en ligne de commande. Par exemple :
- Sous Mac ou Linux
  ``` touch Dockerfile ``` ou ``` echo > Dockerfile ```
- Sous Windows ``` echo Dockerfile ``` ou ``` type nul > Dockerfile ```
 <br> A l'intérieur du Dockerfile créé ajoutez les lignes suivantes :


```
# syntax=docker/dockerfile:1
FROM node:18-alpine
WORKDIR /app
COPY . .
RUN yarn install --production
CMD ["node", "src/index.js"]
EXPOSE 3000
```
#### Happy works <\🎉>
