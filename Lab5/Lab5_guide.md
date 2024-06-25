# LAB5 - (Rancher - Jenkins - Argo CD - Kubectl - Helm): Déployer Rancher dans Docker Desktop - Importer le K8S Cluster - Installer ArgoCD - Configurer GitOps Boardgame via ArgoCD et voir les changements après le changement du manifest.

**Objectifs :**
* Déloiement de Rancher et ajout du Cluster
* Déploiement d'ArgoCD 
* Configuration d'ArgoCD
* Test GitOps


## Déloiement de Rancher et ajout du Cluster
### Déloiement Rancher via docker compose
Use the following to deploy the Rancher in docker desktop:
>rancher_docker-compose.yml

### Import du cluster Kubernetes dans Rancher
Import specifiying the name and information of the existing cluster.

## Déploiement d'ArgoCD 
The steps to install ArgoCD and retrieve the admin password:
Create Namespace for ArgoCD:
>kubectl create namespace argocd

Apply ArgoCD Manifests:
>kubectl apply -n argocd -f https://raw.githubusercontent.com/argoproj/argo-cd/v2.4.7/manifests/install.yaml

Patch Service Type to LoadBalancer:
> kubectl patch svc argocd-server -n argocd -p '{"spec": {"type": "LoadBalancer"}}'

Retrieve Admin Password:
>kubectl -n argocd get secret argocd-initial-admin-secret -o jsonpath="{.data.password}" | base64 -d

## Configuration d'ArgoCD
1. Login to ArgoCD
1. Change the password of ArgoCD
1. Create a Repository and use your "github.com/yourRepo/Boardgame"
1. Create an application and point it to 'argocd-man' folder
1. Verify the application is created and updated

## Test GitOps
1. go to your repo and update the file:
>argocd-man/k8-deployment.yaml
1. Change the replicas from 1 to 4
1. Verify the number of the pod created automatically in ArgoCD