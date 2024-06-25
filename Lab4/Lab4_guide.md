# LAB4 - (Kubeadm - Kubectl - Helm) : Utiliser les 3 VMs - Installer Kubeadm/kubectl/Helm
**Objectifs :**
* Créer un cluster Kubernetes local avec 3 noeuds (1 Master et 2 Worker)
* Installer HELM 
* Installer Cert-manager
* Configurer le cluster kubernetes
* Mettre à jour le Pipepline pour prendre en charge Kubernetes

## Installation d'un nouveau Cluster via Kubeadm
### Créer un cluster Kubernetes local avec 3 noeuds (1 Master et 2 Worker)
1. Step 1: Update and Upgrade Ubuntu (all nodes)
> sudo apt update && sudo apt upgrade -y

2. Step 2: Disable Swap (all nodes)
> sudo swapoff -a
> 
> sudo sed -i '/ swap / s/^\(.*\)$/#\1/g' /etc/fstab

3. Step 3: Add Kernel Parameters (all nodes)
> sudo tee /etc/modules-load.d/containerd.conf <<EOF
> 
> overlay
> 
> br_netfilter
> 
> EOF
> 
> sudo modprobe overlay
> 
> sudo modprobe br_netfilter

Configure the critical kernel parameters for Kubernetes using the following:

> sudo tee /etc/sysctl.d/kubernetes.conf <<EOF
> 
> net.bridge.bridge-nf-call-ip6tables = 1
> 
> net.bridge.bridge-nf-call-iptables = 1
> 
> net.ipv4.ip_forward = 1
>
> EOF
>
> sudo sysctl --system
 
4. Step 4: Install Containerd Runtime (all nodes)
> sudo apt install -y curl gnupg2 software-properties-common apt-transport-https ca-certificates
 
Enable the Docker repository:

> sudo curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmour -o /etc/apt/trusted.gpg.d/docker.gpg
> 
> sudo add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable"

Update the package list and install containerd:
 
> sudo apt update
> 
> sudo apt install -y containerd.io
 
Configure containerd to start using systemd as cgroup:

> containerd config default | sudo tee /etc/containerd/config.toml >/dev/null 2>&1
> 
> sudo sed -i 's/SystemdCgroup \= false/SystemdCgroup \= true/g' /etc/containerd/config.toml
 
Restart and enable the containerd service:

> sudo systemctl restart containerd
> 
> sudo systemctl enable containerd
 
5. Step 5: Add Apt Repository for Kubernetes (all nodes)
> sudo apt-get update

 # apt-transport-https may be a dummy package; if so, you can skip that package
> sudo apt-get install -y apt-transport-https ca-certificates curl gpg

> curl -fsSL https://pkgs.k8s.io/core:/stable:/v1.30/deb/Release.key | sudo gpg --dearmor -o /etc/apt/keyrings/kubernetes-apt-keyring.gpg

> echo 'deb [signed-by=/etc/apt/keyrings/kubernetes-apt-keyring.gpg] https://pkgs.k8s.io/core:/stable:/v1.30/deb/ /' | sudo tee /etc/apt/sources.list.d/kubernetes.list
 
6. Step 6: Install Kubectl, Kubeadm, and Kubelet (all nodes)
> sudo apt update
> 
> sudo apt install -y kubelet kubeadm kubectl
> 
> sudo apt-mark hold kubelet kubeadm kubectl
 
7. Step 7: Initialize Kubernetes Cluster with Kubeadm (master node)
> ip addr

> kubeadm init --apiserver-advertise-address=<paste-the-apiserver-advertise-address-ip> --pod-network-cidr=192.168.0.0/16  --ignore-preflight-errors=all   

> Copy = kubeadm join 172.X.X.X:6443 --token f1h95l.u4nkex9cw8d0g63w \
        --discovery-token-ca-cert-hash sha256:6d15f2aerebdwewfdqwq666af50c85f060b9fadc73f13c932e0e2a9eeef08f51f91a

Run the following commands on the **master node**:

> mkdir -p $HOME/.kube
> 
> sudo cp -i /etc/kubernetes/admin.conf $HOME/.kube/config
> 
> sudo chown $(id -u):$(id -g) $HOME/.kube/config
 
Check node status :
> kubectl get nodes

### Installer HELM 
> curl -fsSL -o get_helm.sh https://raw.githubusercontent.com/helm/helm/main/scripts/get-helm-3

> chmod 700 get_helm.sh

> ./get_helm.sh

> helm version

1. Step 1: Add the NGINX Ingress Repository to Helm
> helm repo add ingress-nginx https://kubernetes.github.io/ingress-nginx

2. Step 2: Install NGINX Ingress Controller

> helm install ingress-nginx ingress-nginx/ingress-nginx \
--namespace ingress-nginx \
--create-namespace \
--set controller.service.type=NodePort \
--set controller.service.nodePorts.http=32686 \
--set controller.service.nodePorts.https=30156 \
--set controller.service.externalIPs=***{"YOUR_EXTERNAL_IP"}***


3. Step 3: Verify Installation
> kubectl get pods --namespace ingress-nginx

### Installer Cert-manager
helm repo add jetstack <https://charts.jetstack.io>
helm repo update
helm upgrade --install cert-manager jetstack/cert-manager  --namespace cert-manager --create-namespace --set installCRDs=true

## Configuration Kubernetes
### Préparation de l'intégration avec Jenkins
1. Copy and create the following files in the master node:
> svc.yml - role.yml - bind.yml - sec.yml

2. Create a new workspace in kubernetes webapps:
> kubectl create ns webapps  

3. Apply the configuration using the files copied from step1
> kubectl apply -f svc.yml

> kubectl apply -f role.yml

> kubectl apply -f bind.yml

> kubectl apply -f sec.yml -n webapps

### Mettre à jour le Pipepline pour prendre en charge Kubernetes
1. Create the credentials of kubernetes in Jenkins
Run in kubernetes master node :
> kubectl describe secret mysecretname -n webapps
1. Copy the secret and paste it in Jeknins as 'kube-key'
1. Update the jenkins CI/CD pipeline using the file:
> cicd2.yml
