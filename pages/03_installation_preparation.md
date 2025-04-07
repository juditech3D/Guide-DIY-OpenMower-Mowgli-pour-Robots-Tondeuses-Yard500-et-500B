---
title: "💾 Installation & préparation"
nav_order: 3
parent: "🏠 Guide OpenMower"
layout: default
permalink: /03_installation_preparation/
---

# 💾 Installation et Préparation du Raspberry Pi

Cette section vous guidera pas à pas pour configurer votre Raspberry Pi afin qu’il soit prêt à exécuter les services nécessaires à OpenMower.

---

## 📀 Préparation de la carte SD/SSD

Avant toute chose, vous devez préparer votre carte SD ou SSD avec **Raspberry Pi OS Lite (64-bit)**.

### 🔧 Étapes :
- Utilisez [**Raspberry Pi Imager**](https://www.raspberrypi.com/software/) pour flasher Raspberry Pi OS Lite (64-bit).
- Vous pouvez suivre la vidéo ci-dessous pour voir comment faire :

![Tuto Raspberry Pi Imager](https://github.com/juditech3D/Guide-DIY-OpenMower-Mowgli-pour-Robots-Tondeuses-Yard500-et-500B/blob/main/M%C3%A9dia/Tuto%20raspberry%20pi%20imager%20%E2%80%90%20R%C3%A9alis%C3%A9e%20avec%20Clipchamp.gif)

---

## 📡 Configuration Wi-Fi et SSH

Pour connecter automatiquement votre Raspberry Pi à un réseau Wi-Fi et activer SSH dès le démarrage, deux méthodes s’offrent à vous :

✅ Via Raspberry Pi Imager (le plus simple, montré dans le GIF ci-dessus)  
🔧 Ou manuellement en éditant des fichiers sur la carte SD

---

### ⚙️ Configuration manuelle : Wi-Fi & SSH
<details>
<summary>📂 Dépliez pour voir</summary>

#### 1. Préparation de la carte SD

1. Gravez l’image Raspberry Pi OS (64 bits) sur la carte SD avec **Raspberry Pi Imager** ou **Balena Etcher**
2. Insérez la carte dans votre ordinateur
3. Accédez à la partition `boot` (visible depuis Windows/macOS/Linux)
4. Ouvrez un éditeur de texte (Notepad++, TextEdit, Nano…)

#### 2. Créez le fichier `wpa_supplicant.conf`

```sh
country=FR
ctrl_interface=DIR=/var/run/wpa_supplicant GROUP=netdev
update_config=1

network={
    ssid="Votre_SSID"
    psk="Votre_Mot_de_passe"
    key_mgmt=WPA-PSK
}
```

#### 💡 Pour plusieurs réseaux :
```sh
network={
    ssid="Premier_SSID"
    psk="Premier_Mot_de_passe"
    key_mgmt=WPA-PSK
    priority=2
}

network={
    ssid="Deuxieme_SSID"
    psk="Deuxieme_Mot_de_passe"
    key_mgmt=WPA-PSK
    priority=1
}
```

---

#### 3. Activez SSH

Créez un fichier vide nommé `ssh` (sans extension) dans la partition `boot`.

---

#### 4. Sauvegardez et éjectez

- Enregistrez les fichiers
- Éjectez proprement la carte SD ou SSD

</details>

---

## 🚀 Démarrer le Raspberry Pi et se connecter via SSH

Une fois la carte prête, placez-la dans le Raspberry Pi et branchez-le à l’alimentation (et à l’Ethernet en option).

---

### 🔍 Trouver l’adresse IP du Raspberry Pi
<details>
<summary>📡 Dépliez pour voir</summary>

#### 🌐 Méthode 1 – Depuis votre box Internet

- Accédez à votre box (ex : `192.168.1.1`)
- Repérez un appareil nommé **raspberrypi**

#### 🛠️ Méthode 2 – Avec [Advanced IP Scanner](https://www.advanced-ip-scanner.com/fr/)

- Téléchargez, scannez votre réseau
- Repérez un appareil nommé Raspberry Pi

![Advanced IP Scanner](https://github.com/juditech3D/Guide-DIY-OpenMower-Mowgli-pour-Robots-Tondeuses-Yard500-et-500B/blob/main/images/Advanced%20ip%20scanner/Advanced%20ip%20scanner.png)

</details>

---

### 🔑 Se connecter en SSH

Une fois l’IP identifiée, connectez-vous via SSH avec MobaXTerm, PuTTY ou en ligne de commande :

```sh
ssh pi@192.168.X.XX
```

Par défaut, le mot de passe est `raspberry`.

---

✅ Une fois connecté, passez à la configuration logicielle du système.
