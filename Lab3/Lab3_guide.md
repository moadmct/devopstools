# LAB3 - (Git - Jenkins - SonarQube - Nexus) : Docker compose pour déployer SonarQube et Nexus et Gitlab
**Objectifs :**
* Configurer Sonarqube
* Configurer Nexus
* Installer Jenkins sur windows
* Configurer Jenkins
* Créer une application de test

**IMPORTANT** : Veuillez créer un compte sur Github.com et Dockerhub.com pour ce lab

## Configurer Sonarqube
1. Via le navigateur accèder à : 
>http://172.x.x.x:9000

2. Changer le mot de passe admin.

## Configurer Nexus
1. Via le navigateur accèder à : 
>http://172.x.x.x:8081

2. Changer le mot de passe admin.

## Créer une application de test 
1. Cloner l'application à partir : 
    >https://github.com/moadmct/Boardgame.git
2. Ouvrir le code dans Codespace sur Github

## Jenkins
### Installer Jenkins sur windows
1. Installer jdk17
    >https://www.oracle.com/java/technologies/javase/jdk17-archive-downloads.html

2. Télécharger et installer Jenkins :
    >https://ftp.belnet.be/mirror/jenkins/windows-stable/2.452.2/jenkins.msi

### Configurer Jenkins
1. Réinitaliser le mot de passe admin
2. Installer les plugins
3. Configurer les outils
4. Configurer le système
5. Créer un onfiguration Global-Settings
6. Création des credentials (Github - Dockerhub - Sonarqube)

### Création de Pipeline CI/CD
1. Créer une nouvelle Pipeline CI/CD sous Jenkins 'cicd_app1'
1. Copie le contenu du fichier suivant dans votre pipeline :
    > cicd.yml
1. Démarrer la construction *build* et voir le résultat
1. Va dans dockerhub.com et voir si l'image a été bien présente dans votre répértoire.


