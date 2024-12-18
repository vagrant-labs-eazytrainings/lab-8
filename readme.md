# Lab 8: Configuration d'un Cluster de 3 VMs avec Provisioning Shell

Ce laboratoire consiste à créer un cluster de 3 machines virtuelles (« VMs ») utilisant Vagrant et VirtualBox. Chaque machine est provisionnée avec un script shell externe, et les ressources sont configurées pour une RAM de 1024 Mo et un CPU.

---

## Structure du Projet

Le cluster comporte trois VMs :

1. **Load Balancer (lb)** : Responsable de la répartition du trafic entre les machines web.
2. **Web Server 1 (web1)** : Premier serveur web.
3. **Web Server 2 (web2)** : Deuxième serveur web.

---

## Configuration des Machines

### Paramètres Généraux

- **RAM** : 1024 Mo
- **CPU** : 1
- **Réseau** : Chaque machine est connectée à un réseau privé avec une adresse IP unique.

### Adresses IP

- **lb** : `10.0.0.10`
- **web1** : `10.0.0.11`
- **web2** : `10.0.0.12`

### Provisioning Shell

Chaque machine utilise un script shell externe pour son provisioning :

- **lb** : `lb.sh`
- **web1** : `web.sh` avec l'argument `1`
- **web2** : `web.sh` avec l'argument `2`

---

## Vagrantfile

Voici le contenu du fichier `Vagrantfile` utilisé pour déployer le cluster :

```ruby
Vagrant.configure("2") do |config|
    # Configure load balancer machine
    config.vm.define "lb" do |lb|
        lb.vm.box = "ubuntu/xenial64"
        lb.vm.hostname = "lb"
        lb.vm.network "private_network", ip: "10.0.0.10"
        lb.vm.provision :shell do |shell|
            shell.path = "https://raw.githubusercontent.com/PacktPublishing/Hands-On-DevOps-with-Vagrant/master/Chapter07/lb.sh"
        end
    end
    config.vm.provider "virtualbox" do |v|
        v.memory = 1024
        v.cpus = 1
    end

    # Configure first web machine
    config.vm.define "web1" do |web1|
        web1.vm.box = "ubuntu/xenial64"
        web1.vm.hostname = "web1"
        web1.vm.network "private_network", ip: "10.0.0.11"
        web1.vm.provision :shell do |shell|
            shell.path = "https://raw.githubusercontent.com/PacktPublishing/Hands-On-DevOps-with-Vagrant/master/Chapter07/web.sh"
            shell.args = "1"
        end
    end
    config.vm.provider "virtualbox" do |v|
        v.memory = 1024
        v.cpus = 1
    end

    # Configure second web machine
    config.vm.define "web2" do |web2|
        web2.vm.box = "ubuntu/xenial64"
        web2.vm.hostname = "web2"
        web2.vm.network "private_network", ip: "10.0.0.12"
        web2.vm.provision :shell do |shell|
            shell.path = "https://raw.githubusercontent.com/PacktPublishing/Hands-On-DevOps-with-Vagrant/master/Chapter07/web.sh"
            shell.args = "2"
        end
    end
    config.vm.provider "virtualbox" do |v|
        v.memory = 1024
        v.cpus = 1
    end
end
```

---

## Instructions pour Exécuter le Projet

1. **Prérequis** :
   - Installer [Vagrant](https://www.vagrantup.com/downloads).
   - Installer [VirtualBox](https://www.virtualbox.org/).

2. **Cloner les Scripts** :
   - Les scripts de provisioning sont disponibles sur le dépôt GitHub :
     - `lb.sh` : [lb.sh](https://raw.githubusercontent.com/PacktPublishing/Hands-On-DevOps-with-Vagrant/master/Chapter07/lb.sh)
     - `web.sh` : [web.sh](https://raw.githubusercontent.com/PacktPublishing/Hands-On-DevOps-with-Vagrant/master/Chapter07/web.sh)

3. **Démarrer le Cluster** :

   ```bash
   vagrant up
   ```

4. **Vérifier les Machines** :
   - Connectez-vous à chaque VM avec `vagrant ssh [nom_de_vm]`.
   - Testez la connectivité entre les machines (ex. `ping 10.0.0.11` depuis `lb`).

---

## Objectifs d'Apprentissage

- Création et configuration d'un cluster multi-noeuds avec Vagrant.
- Utilisation de scripts shell pour le provisioning.
- Gestion des réseaux privés dans VirtualBox.
- Compréhension du rôle des load balancers dans un cluster.

---

**Note** : Assurez-vous d'avoir une connexion Internet active pour télécharger les scripts de provisioning lors du démarrage des machines.
