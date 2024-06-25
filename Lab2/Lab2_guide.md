# LAB2 - (Docker - Docker Compose - Docker Swarm) : 
**Objectifs :**
* Utiliser Docker Desktop créer Hello-world
* Utiliser Docker Desktop Gitlab dans le port 80
* Utiliser les VMs crééés par Vagrant pour le Docker swarm
* Utiliser docker compose pour déployer Sonarqube et Nexus

**Guide**
Ouvrir PowerShell en tant qu'administratuer
## 1 - Utiliser Docker Desktop créer Hello-world
*1* Vérifier que Docker est déjà installé sur vos machine (docker) par la commande 
>***docker -v***

*2* - Créer un conteneur Hello-World 
> ***docker run hello-world***

## 2 - Utiliser Docker Desktop Gitlab dans le port 80
Utiliser la commande suivante :
> docker run -d --hostname gitlab.demo.com -p 80:80  -p 443:443 --name gitlab -v gitlab-data:/opt/gitlab/data -v /etc/gitlab:/etc/gitlab   -v /var/log/gitlab:/opt/gitlab/logs gitlab/gitlab-ce

## 3 - Utiliser les VMs crééés par Vagrant pour le Docker swarm
1. Démarrer et connectez-vous dans les 3 VMs.
2. Va dans uvm1 et executer la commande :
    > docker swarm init

    Comme résultat vous allez voir une chaine avec un token pour l'utilser à faire joindre les autres VMs (workers).
3. Copie la chaine 
    >***docker swarm join --token SWMTKN-1-4foj.....***
4. Va dans chaque VM et coller la chaine pour faire joindre la machine au cluster.
5. Vérifier en utilisant :
    >docker nods ls
6. Maintenant on va créer un service **Nginx** pour tester :
    > create a service = docker service create --name webapp1 --replicas=3 nginx

7. Installer un le visualizateur web (https://github.com/dockersamples/docker-swarm-visualizer) pour voir le résultant en mode graphique :
    > docker run -it -d -p 8080:8080 -v /var/run/docker.sock:/var/run/docker.sock dockersamples/visualizer
8. Voir dans le navigateur l'état de changement ***172.XX.XX.XX:8080*** (adresse du master node)

9. Mettre à jour le nombre des réplicas utilisant la commande :
    > docker service update --replicas=9 webapp1
10. Surveiller le changement via le visualisateur.


## 4 - Utiliser docker compose pour déployer Sonarqube et Nexus

Démarrer Powershell as admin

### Sonarqube

1. Création de dossier
    > C:\DevOps-labs\Lab2\Sonarqube

2. Créer le fichier suivant dans le dossier
    > docker-compose.yml

3. Copier le contenu du et coller le dans votre docker-compose.yml :
    >sonar_docker-compose.yml

4. Après déplacer vers 
    > C:\DevOps-labs\Lab2\Sonarqube 

5. Rouler la commande : 
    > docker-compose up -d

6. Vérifier que Sonarqube a été bien déployé
    > docker ps

### Nexus
1. Création de dossier
    > C:\DevOps-labs\Lab2\Nexus

2. Créer le fichier suivant dans le dossier
    > docker-compose.yml

3. Copier le contenu du et coller le dans votre docker-compose.yml :
    >nexus_docker-compose.yml

4. Après déplacer vers 
    > C:\DevOps-labs\Lab2\Nexus 

5. Rouler la commande : 
    > docker-compose up -d

6. Vérifier que Nexus a été bien déployé
    > docker ps

