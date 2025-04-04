---
title: "🛠️ Ser2net"
nav_order: 97
parent: "🏠 Guide OpenMower"
permalink: /ser2net/
layout: default
---

# Exposer les ports série du robot Mowgli via Ser2Net

> Ce tutoriel a été réalisé par Julien Guy et adapté à partir du travail de [cedbossneo](https://github.com/cedbossneo/mowgli-docker.git). Le fichier Docker Compose incluant Ser2Net et une partie de la documentation originale ont été créés par cedbossneo. Merci à lui pour cette base de travail.

## Attention

- **Connexion Wi-Fi** : Une faible réception Wi-Fi peut entraîner des problèmes de connexion avec le serveur, impactant les performances. Mais le même problème se pose dans l’autre cas avec les paquets de correction GPS RTK, qui auraient également du mal à atteindre le Raspberry Pi dans ces conditions. Assurez-vous d’avoir une connexion Wi-Fi stable pour éviter les interruptions.

Ce tutoriel explique comment déplacer tous les conteneurs Docker sur une autre machine, telle qu’un serveur OpenMower. Le Raspberry Pi expose les ports série `/dev/gps` et `/dev/mowgli` sur le réseau. Le serveur utilise ces ports pour créer deux ports série virtuels, utilisables dans les conteneurs Docker (ROS Serial et ROS OpenMower).

## Pré-requis

- Aisance avec Linux, la ligne de commande et les transferts de fichiers (SFTP/SCP)
- Une machine ou une VM disponible (par exemple sous Proxmox)

## Avantages constatés

- Moins de consommation énergétique (objectif : Pi Zero Gen 1 à la place du Pi 4)
- Calculs déportés sur une machine plus puissante (VM)
- Moins d'usure de la carte SD (moins de cycles `up/down` Docker)
- Configuration GPS à distance via U-center
- Gestion facile des conteneurs (Portainer, Dozzle)
- Déploiements rapides (&lt;2min) et redémarrage &lt;10s

## Mise en œuvre

### Sur le Raspberry Pi

```bash
# Arrêter les conteneurs Docker si déjà installés
docker compose down
```

- Sauvegardez `ros/map.bag`
- Éditez `/etc/udev/rules.d/50-mowgli.rules` :

```bash
SUBSYSTEM=="tty" ATTRS{product}=="Mowgli", SYMLINK+="mowgli"
SUBSYSTEM=="tty" ATTRS{idVendor}=="1546" ATTRS{idProduct}=="01a9", SYMLINK+="gps"
SUBSYSTEM=="tty" ATTRS{idVendor}=="303a" ATTRS{idProduct}=="4001", SYMLINK+="gps"
```

- Installez et activez ser2net :

```bash
apt install -y ser2net
systemctl enable ser2net
```

- Configurez `/etc/ser2net.yaml` :

```yaml
connection: &mowgli01
  accepter: tcp,4001
  enable: on
  options:
    banner: "ser2net - MOWGLI"
    kickolduser: false
    telnet-brk-on-sync: true
  connector: serialdev, /dev/mowgli, 115200n81,local

connection: &gps01
  accepter: tcp,4002
  enable: on
  options:
    banner: "ser2net - GPS"
    kickolduser: false
    telnet-brk-on-sync: true
  connector: serialdev, /dev/gps, 460800n81,local
```

```bash
# Redémarrer le Pi
sudo reboot
```

### Sur le serveur (VM Debian sous Proxmox ou Ubuntu)

```bash
# Installation de Docker
curl -sSL https://get.docker.com | sh

# Clonage du dépôt (par défaut cedbossneo)
git clone https://github.com/cedbossneo/mowgli-docker.git
cd mowgli-docker
```

- Éditez `.env` :

```env
ROS_IP=127.0.0.1
MOWER_IP=192.168.1.66
IMAGE=ghcr.io/cedbossneo/mowgli-docker:cedbossneo
```

- Ajoutez dans `docker-compose.ser2net.yaml` :

```yaml
gui:
  container_name: openmower-gui
  image: ghcr.io/cedbossneo/openmower-gui:master
  restart: unless-stopped
  network_mode: host
  privileged: true
  environment:
    ROS_IP: ${ROS_IP}
    ROS_MASTER_URI: http://${ROS_IP}:11311
    MOWER_CONFIG_FILE: /config/mower_config.sh
    DOCKER_HOST: unix:///var/run/docker.sock
    DB_PATH: /db
  depends_on:
    - roscore
  volumes:
    - /dev:/dev
    - ./config/db:/db
    - ./config/om:/config
    - /var/run/docker.sock:/var/run/docker.sock
```

- Copiez `map.bag` dans `ros/`
- Lancez les conteneurs :

```bash
docker compose -f docker-compose.ser2net.yaml up -d
```

## Bonus : Configuration GPS du F9P via U-center

Pour configurer le F9P via U-center, vous devez **arrêter les conteneurs Docker** pour libérer le port série :

```bash
docker compose down
```

Ensuite, connectez-vous au port TCP `4002` avec U-center pour changer les paramètres du GPS (ex : fréquence), et réinjectez la configuration si besoin.

## Conclusion

Cette configuration vous permet de délester le Raspberry Pi, centraliser la gestion via un serveur, et simplifier le déploiement ou les tests. Elle est particulièrement utile pour configurer à distance le GPS, tester différentes fréquences, et préserver la durée de vie de votre matériel.