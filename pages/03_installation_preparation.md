---
title: "💾 Installation & préparation"
nav_order: 3
parent: "🏠 Guide OpenMower"
layout: default
permalink: /03_installation_preparation/
---

# 💾 Installation & Préparation du Raspberry Pi

Cette section vous guide pour préparer le Raspberry Pi qui hébergera le système OpenMower basé sur le fork Mowgli. Vous apprendrez à graver le système sur une carte SD ou SSD, configurer le Wi-Fi et SSH, et installer les outils nécessaires.

## 🧱 Étapes principales

1. Préparer la carte SD ou SSD
2. Configurer l'accès Wi-Fi et SSH
3. Installer Docker et les dépendances
4. Cloner le dépôt `mowgli-docker`
5. Lancer les conteneurs

---

## 📦 Image Raspberry Pi OS

Téléchargez Raspberry Pi Imager et suivez les étapes pour graver une image Raspberry Pi OS 64 bits sur la carte SD.

[Télécharger Raspberry Pi Imager](https://www.raspberrypi.com/software/)

## 📶 Configuration Wi-Fi et SSH

Deux options :

- **Avec Pi Imager** (plus simple)
- **Manuellement** : créer `wpa_supplicant.conf` + fichier `ssh` dans la partition `boot`.

<details>
<summary>Voir instructions manuelles</summary>

Créer un fichier `wpa_supplicant.conf` avec :

```txt
country=FR
ctrl_interface=DIR=/var/run/wpa_supplicant GROUP=netdev
update_config=1

network={
    ssid="Votre_SSID"
    psk="Votre_Mot_de_passe"
    key_mgmt=WPA-PSK
}
```

Et un fichier vide `ssh` dans la partition `boot`.

</details>

## 🛠️ Connexion et mise à jour

Après démarrage du Pi, connectez-vous via SSH et mettez le système à jour :

```bash
sudo apt update && sudo apt upgrade -y
```

## 🐳 Installation de Docker

```bash
curl -fsSL https://get.docker.com | sh
```

Et ajoutez votre utilisateur au groupe docker :

```bash
sudo usermod -aG docker $USER
```

## 📂 Clonage du dépôt Mowgli Docker

```bash
sudo apt install git
git clone https://github.com/cedbossneo/mowgli-docker
cd mowgli-docker
```

## ⚙️ Configuration du fichier `.env`

Modifiez les IP dans le fichier `.env` :

```bash
nano .env
```

Remplacez par l'adresse IP du Raspberry Pi (Wi-Fi) pour `ROS_IP` et `MOWER_IP`.

## 🚀 Lancement des conteneurs

```bash
docker compose up -d
```

> ☕ Cette étape peut prendre du temps : les images sont téléchargées depuis internet.

## 💡 Astuce

Pensez à fixer l'IP du Raspberry Pi dans votre box internet pour éviter qu'elle change.

---

[🔙 Revenir à la page Bonus pour télécharger les pièces imprimables 3D personnalisées](../07_bonus/)
