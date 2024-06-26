# LAB1 - (HyperV - Vagrant) Avoir un méchanisme pour créer plusieurs VM ubuntu rapidement
**Objectifs :**
* Création de VM en utilisant Hyper-V
* Configurer la VM comme template (installation des outils comme Docker pour l'avoir disponible dans toutes les nouvelles machines)
* Utiliser Vagrant pour créer un VM template (box)
* Utiliser Vagrant pour créer 3 VM utilisant la box


## Préparation du Lab (Installation Hyper-V - Vagrant - Jenkins - Git - VSCode)

* Installation Hyper-V role dans la machine
* Configuration de vSwitch (WAN - LAN)
* Télécharger ISO Ubuntu server 24.04
* Création d'une VM ubuntu24
* Dans HyperV :
    1. Créer une nouvelle VM utilisant le ISO et installer les outils :
        * Docker :
        > sudo apt install docker.io -y
        >
        > sudo chmod 666 /var/run/docker.sock
        * Configurer :
            1. IP
            1. Hostname
            1. Username
            1. Password
            1. Et autes selon votre besoin
        * Arrêter la VM
        * Déactiver "Snapshot"
        * Export the vm to you Vagrant folder (c:/vagrant/template)
* Installer VSCode

## Vagrant

1. Installer Vagrant for windows :
    > https://developer.hashicorp.com/vagrant/install?product_intent=vagrant#windows
1. Ouvrir VSCode (path: c:/vagrant)
1. Créer un fichier dqns c:/vagrant/template
    > metadata.json
    
    Contenu :
    >{ 
    >
    >      "provider": "hyperv"
    >
    >}

    Réf = https://developer.hashicorp.com/vagrant/docs/boxes/format> 
    
1. Déplacer les dossier "*Virtual hard Disks*" et "*Virtual Machines*" du c:/vagrant/template/ubuntu24 vers c:/vagrant/template. Notez que les deux dossier et le fichier *metadata.json* devrait être dans le même niveau sous c:/vagrant/template.
1. Naviger vers le chemin "C:/vagrant/template" via le terminal et executer la commande :
    > tar cvzf ../ubuntu24.box ./*
1. Après exécuter la commande pour décomprésser la box :        
    > vagrant box add --provider hyperv --name ubuntu24 ..\ubuntu24.box
1. Create a new folder "c:/vagrant/machines" and run the commande to initialize vagrant environment :
    > vagrant init
1. Mettez à jour le fichier c:/vagrant/machines/Vagrant :
    > config.vm.box = "ubuntu24"
    >
    > config.vm.hostname = "NOM-DE-LA-NOUVELLE-VM"
    >
    > config.vm.define "NOM-DE-LA-NOUVELLE-VM"
    >
    > config.vm.network "public_network", bridge: "Default Switch"
    >
    >
    > config.vm.provider "hyperv" do |h|
    >
    >     h.cpus= 1
    >
    >     h.memory= 1024
    >
    >     h.vmname= "NOM-DE-LA-NOUVELLE-VM"
    >
    > end

    Changer le nom "NOM-DE-LA-NOUVELLE-VM" et cr2er vos VMs.
1. Ouvre PowerShell en tant qu'administrateur
1. Déplacer vous dans le dossier c:/vagrant/machines
1. Créer une nouvelle VM avec la commande :
    > vagrant up
1. Créer 2 autres machines en changant la configuration du fichier *Vagrant*
1. En fin dans Hyper-v faire un checkpoint "point de contrôle" pour chaque machine (pour pouvoir les ré-utiliser dans les autres labs)
