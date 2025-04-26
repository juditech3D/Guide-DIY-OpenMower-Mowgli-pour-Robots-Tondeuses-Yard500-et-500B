---
title: "🛠️ Ser2net"
nav_order: 97
parent: "🏠 Guide OpenMower"
permalink: /ser2net/
layout: default
---

# 🛠️ Exposer les ports série du robot Mowgli via Ser2Net

> 🙏 Ce tutoriel a été réalisé par **Julien Guy**, basé sur les travaux de [cedbossneo](https://github.com/cedbossneo/mowgli-docker.git) (Docker Compose original et documentation). Un grand merci à eux pour leur contribution précieuse.

---

## 📑 Sommaire rapide de cette page

- [Objectif](#-objectif)
- [Pré-requis](#-pré-requis)
- [Avantages constatés](#-avantages-constatés)
- [Mise en œuvre sur le Raspberry Pi](#-1️⃣-sur-le-raspberry-pi)
- [Mise en œuvre sur le serveur distant](#-2️⃣-sur-le-serveur-distant-vm-debianubuntu)
- [Bonus : Configurer le GPS via U-center](#-bonus--configurer-votre-f9p-via-u-center)
- [Conclusion](#-✅-conclusion)

---

<div class="alert-red">
  <div class="alert-title">⚠️ Attention importante</div>
  <p><strong>Connexion Wi-Fi :</strong> Une mauvaise réception Wi-Fi peut provoquer des déconnexions série ou des pertes de paquets RTK. Assurez-vous d’avoir une connexion stable avant d’utiliser cette méthode.</p>
</div>

---

# 🛠️ Objectif

Déporter l’exécution des conteneurs Docker OpenMower sur une autre machine (ex : serveur Proxmox) tout en exposant les ports série `/dev/gps` et `/dev/mowgli` via le réseau grâce à **Ser2Net**.

---

# 📋 Pré-requis

- ✅ Aisance avec Linux, SSH et la ligne de commande
- ✅ Une machine dédiée ou une VM disponible (Debian/Ubuntu recommandé)
- ✅ Savoir transférer des fichiers (SFTP/SCP)

---

# 🚀 Avantages constatés

- ⚡ Réduction de la consommation énergétique (possibilité d'utiliser un Pi Zero)
- 🖥️ Calculs ROS déportés sur une machine plus puissante
- 💾 Moins d'usure de la carte SD
- 🌐 Accès simplifié à la configuration GPS (ex : U-center)
- 📦 Gestion optimisée via Portainer ou Dozzle
- 🔁 Redémarrage ultra rapide des conteneurs (<10 secondes)

---

# 🛠️ 1️⃣ Sur le Raspberry Pi

## ⏹️ Arrêter les conteneurs Docker existants

```bash
docker compose down
```

---

## 📂 Sauvegarder votre carte de tonte

```bash
sudo cp ./ros/map.bag ~/backup_map.bag
```

---

## 🛠️ Modifier les règles UDEV pour nommer les périphériques

Éditez ou créez `/etc/udev/rules.d/50-mowgli.rules` :

```bash
SUBSYSTEM=="tty" ATTRS{product}=="Mowgli", SYMLINK+="mowgli"
SUBSYSTEM=="tty" ATTRS{idVendor}=="1546" ATTRS{idProduct}=="01a9", SYMLINK+="gps"
SUBSYSTEM=="tty" ATTRS{idVendor}=="303a" ATTRS{idProduct}=="4001", SYMLINK+="gps"
```

---

## ⚙️ Installer Ser2Net

```bash
sudo apt update
sudo apt install -y ser2net
sudo systemctl enable ser2net
```

---

## ✏️ Configurer `/etc/ser2net.yaml`

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

---

## 🔄 Redémarrer le Raspberry Pi

```bash
sudo reboot
```

---

# 🛠️ 2️⃣ Sur le serveur distant (VM Debian/Ubuntu)

## 📥 Installer Docker

```bash
curl -sSL https://get.docker.com | sh
```

---

## 📂 Cloner mowgli-docker

```bash
git clone https://github.com/cedbossneo/mowgli-docker.git
cd mowgli-docker
```

---

## ⚙️ Modifier le fichier `.env`

```env
ROS_IP=127.0.0.1
MOWER_IP=192.168.1.66
IMAGE=ghcr.io/cedbossneo/mowgli-docker:cedbossneo
```

---

## 🛠️ Ajouter OpenMower-GUI dans `docker-compose.ser2net.yaml`

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

---

## 🚀 Lancer les conteneurs

```bash
docker compose -f docker-compose.ser2net.yaml up -d
```

---

# 🎯 Bonus : Configurer votre F9P via U-center

Pour reconfigurer votre GPS :

1. **Arrêter les conteneurs Docker** :

```bash
docker compose down
```

2. **Utiliser U-center** pour se connecter sur l'IP du Pi port 4002.

---

<div class="alert-red">
  <div class="alert-title">⚠️ Important</div>
  <p>Après toute modification du GPS, assurez-vous de recharger la configuration dans le firmware si nécessaire !</p>
</div>

---

# ✅ Conclusion

Cette solution permet d’alléger votre Raspberry Pi, d'améliorer les performances réseau et d'assurer une gestion plus fiable de votre robot.

---

<div style="text-align: center;">
[⬅ Retour à la page d'accueil]({{ '/' | relative_url }})  
<br>
[📑 Aller au Sommaire]({{ '/pages/sommaire/' | relative_url }})
</div>
