# Guide OpenMower Mowgli DIY Robot Tondeuse Yard 500 et 500B

Bienvenue dans ce guide complet pour configurer et déployer votre robot tondeuse OpenMower Mowgli Yard 500 et 500B en français, basé sur mon expérience de la construction du robot Yardforce 500B.

**Attention :** Ce guide est un work in progress et est en cours de rédaction.

**Avant de commencer, assurez-vous de bien comprendre les avertissements et les prérequis.**

Pour plus d'informations, visitez les pages :  
- [Page d'accueil d'OpenMower](https://openmower.de/docs/robot-assembly/prepare-the-parts/prepare-the-robot/photo-guide/)
- [Fork Mowgli](https://github.com/cedbossneo/Mowgli)
- [Mowgli Docker](https://github.com/CedBossNeo/mowgli-docker)
- [Groupe d'entraide telegram français du fork Mowgli](https://t.me/+x6U3UwU5lB4yOWNk)

## Prérequis (et matériels que j'ai utilisés)

- Raspberry Pi avec Pi OS installé ( cela suppose que vous avez déja créer votre sd ou ssd (pour le pi 4) et qu'il démarre sans problème sur le pi)
- Carte GPS/Rtk [F9P](https://fr.ardusimple.com/product/simplertk2b/?attribute_pa_header-options=without-headers) avec son [cable d'antenne](https://fr.aliexpress.com/item/1005004690761874.html?spm=a2g0o.order_list.order_list_main.23.27185e5bO83Ive&gatewayAdapt=glo2fra) et son antenne [Bt560](https://fr.aliexpress.com/item/32991527632.html?spm=a2g0o.order_list.order_list_main.28.755e5e5bZP1cya&gatewayAdapt=glo2fra) mais ayant eu des difficulté de réception de mon coté j'ai utilisé la Bt603 qui est plus puissante mais plus chère.
- 
- Putty ou similaire (moi j'ai utilisé [solarputty](https://www.solarwinds.com/free-tools/solar-putty))
- Connaissances de base en ligne de commande ( pas obligatoire car je détails tout ici )
- Accès Internet (Pour une configuration plus rapide, vous pouvez le raccorder en Rj45 au début, mais attention l'IP qu'il faudra utiliser dans la suite du tuto est bien celle du wifi qui elle, est différente) 

# Préparation de la carte sd/SSD du raspberry pi

![Tuto raspberry pi imager](https://github.com/juditech3D/Guide-DIY-OpenMower-Mowgli-pour-Robots-Tondeuses-Yard500-et-500B/blob/main/M%C3%A9dia/Tuto%20raspberry%20pi%20imager%20%E2%80%90%20R%C3%A9alis%C3%A9e%20avec%20Clipchamp.gif)

### Étapes pour configurer un ou plusieurs réseaux Wi-Fi sur un Raspberry Pi 4

#### 1. Préparation de la carte SD

1. **Gravez l'image de Raspberry Pi OS** sur la carte SD en utilisant un outil comme Raspberry Pi Imager ou Balena Etcher.
2. **Insérez la carte SD** dans votre ordinateur après avoir gravé l'image.

#### 2. Accédez à la partition `boot`

1. **Accédez à la partition `boot`** de la carte SD (cette partition est visible sous Windows, macOS et Linux).
2. **Ouvrez un éditeur de texte** comme Notepad++ (Windows), TextEdit (macOS), ou Nano (Linux).

#### 3. Créez le fichier `wpa_supplicant.conf`

1. **Dans la partition `boot`, créez un nouveau fichier texte** nommé `wpa_supplicant.conf`

2. **Ajoutez la configuration Wi-Fi** :

   - **Pour un seul réseau** :
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

   - **Pour plusieurs réseaux** :
     ```sh
     country=FR
     ctrl_interface=DIR=/var/run/wpa_supplicant GROUP=netdev
     update_config=1

     network={
         ssid="Premier_SSID"
         psk="Premier_Mot_de_passe"
         key_mgmt=WPA-PSK
     }

     network={
         ssid="Deuxieme_SSID"
         psk="Deuxieme_Mot_de_passe"
         key_mgmt=WPA-PSK
     }
     ```

   - **Explication des paramètres** :
     - **`country=FR`** : Remplacez `FR` par le code de votre pays si nécessaire (par exemple, `US` pour les États-Unis).
     - **`ssid="Votre_SSID"`** ou **`ssid="Premier_SSID"`** et **`ssid="Deuxieme_SSID"`** : Remplacez ces valeurs par le(s) nom(s) des réseaux Wi-Fi.
     - **`psk="Votre_Mot_de_passe"`** ou **`psk="Premier_Mot_de_passe"`** et **`psk="Deuxieme_Mot_de_passe"`** : Remplacez ces valeurs par le(s) mot(s) de passe respectif(s).
     - **`key_mgmt=WPA-PSK`** : Ce paramètre indique que le réseau utilise WPA2, ce qui est standard pour la plupart des réseaux Wi-Fi domestiques.

3. **(Facultatif) Priorité des réseaux** :
   - Si vous avez configuré plusieurs réseaux et souhaitez que le Raspberry Pi préfère certains réseaux lorsqu'ils sont disponibles, vous pouvez ajouter une ligne `priority=N` dans chaque bloc `network`, où `N` est un nombre entier. Plus le nombre est élevé, plus la priorité est haute.

   Exemple :

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

   Dans cet exemple, `Premier_SSID` sera préféré à `Deuxieme_SSID` si les deux sont disponibles.

#### 4. Activez SSH (OBLIGATOIRE)

1. **Pour activer SSH dès le premier démarrage**, créez un fichier vide nommé `ssh` (sans extension) dans la partition `boot`. Cela activera le service SSH automatiquement.

#### 5. Sauvegardez et éjectez

1. **Sauvegardez le fichier `wpa_supplicant.conf`** et fermez l'éditeur de texte.
2. **Éjectez la carte SD** de votre ordinateur en toute sécurité.

#### 6. Démarrez le Raspberry Pi 4

1. **Insérez la carte SD** dans le Raspberry Pi 4.
2. **Allumez le Raspberry Pi**. Lors du démarrage, il lira le fichier `wpa_supplicant.conf` et se connectera au(x) réseau(x) Wi-Fi configuré(s). Si vous avez activé SSH, vous pourrez vous connecter au Raspberry Pi via l'adresse IP sur le réseau : pour ça vous pouvez la trouver via votre box opérateur ou via un soft, moi j'utilise [Advanced IP Scanner](https://www.advanced-ip-scanner.com/fr/), il aura normalement le nom que vous lui aurez donner, avec comme nom de fabricant = Rasberry Pi ou similaire

![Advanced IP Scanner](https://github.com/juditech3D/Guide-DIY-OpenMower-Mowgli-pour-Robots-Tondeuses-Yard500-et-500B/blob/main/images/Advanced%20ip%20scanner/Advanced%20ip%20scanner.png)

Cette méthode vous permet de configurer un ou plusieurs réseaux Wi-Fi de manière flexible pour votre Raspberry Pi 4, avec la possibilité d'établir des priorités entre les réseaux si nécessaire.

```plaintext
N'oubliez pas de fixer l'adresse IP ou l'adresse mac de votre rasberry 
pi via votre box pour qu'il soit toujours accessible.
```

# Préparation du raspberry pi et configuration

## Étape 1 : Mise à jour de Pi OS

Avant de commencer, assurez-vous que votre système est à jour.

```sh
sudo apt update && sudo apt upgrade -y
```

## Étape 2 : Installation de Docker

Installez Docker en exécutant la commande suivante :

```sh
curl https://get.docker.com | sh
```

## Étape 3 : Installation de Docker Compose

Installez Docker Compose avec la commande suivante :

```sh
sudo apt install docker-compose -y
```

## Étape 4 : Configuration de udev

1. Créez et éditez le fichier de règles udev :

```sh
sudo nano /etc/udev/rules.d/50-mowgli.rules
```

2. Ajoutez les règles suivantes :

```sh
SUBSYSTEM=="tty" ATTRS{product}=="Mowgli", SYMLINK+="mowgli"
# simpleRTK USB
SUBSYSTEM=="tty" ATTRS{idVendor}=="1546" ATTRS{idProduct}=="01a9", SYMLINK+="gps"
# ESP USB CDC - RTK1010Board
SUBSYSTEM=="tty" ATTRS{idVendor}=="303a" ATTRS{idProduct}=="4001", SYMLINK+="gps"
```

Faites Ctrl "o" pour enregistrer et valider avec la touche "Entrée", puis Ctrl "x" pour sortir.

3. Rechargez les règles udev :

```sh
sudo udevadm control --reload-rules
sudo udevadm trigger
```

## Étape 5 : Clonage du dépôt

On récupère le code pour la génération des containers dans docker
Clonez le dépôt GitHub avec la commande suivante :

```sh
git clone https://github.com/cedbossneo/mowgli-docker
cd mowgli-docker
```

## Étape 6 : Configuration de l'environnement

1. Créez et éditez le fichier `.env` (il faut etre dans le répertoire mowgli-docker, si vous n'y etes pas faites "cd mowgli-docker") :

```sh
sudo nano .env
```

2. Remplacez les valeurs `ROS_IP` et `MOWER_IP` par l'adresses IP de votre raspberry (pour info le pavé numerique ne fonctionne pas , déplacer le curseur avec les fleche de navigation) :

```sh
# ROS_IP est l'IP de la machine exécutant le conteneur Docker
# MOWER_IP est l'IP de la tondeuse
# Lorsque vous n'êtes pas en mode ser2net, les deux IPs doivent être les mêmes
ROS_IP=192.168.1.34
MOWER_IP=192.168.1.34
IMAGE=ghcr.io/cedbossneo/mowgli-docker:cedbossneo
```

Faites Ctrl "o" pour enregistrer et valider avec la touche "Entrée", puis Ctrl "x" pour sortir.

## Étape 7 : Mise à jour de docker-compose.yaml ( mettre à jour suite a une erreur comme quoi le fichier était obsolète donc obligatoire si vous avez une erreur à l'étapes 8 sinon vous ne pourrez pas démarrer les dockers)

![Erreur docker-compose.yaml](https://github.com/juditech3D/Guide-DIY-OpenMower-Mowgli-pour-Robots-Tondeuses-Yard500-et-500B/blob/main/images/Solarputty/Erreur%20docker-compose.yaml.png)


Ouvrez le fichier `docker-compose.yaml` :

```sh
sudo nano docker-compose.yaml
```

Remplacez le contenu par le suivant ou modifier simplement les lignes indiquer ci-dessous par cette fleche <============= (pour info le pavé numerique ne fonctionne pas , déplacer le curseur avec les fleche de navigation):

```yaml
version: '3'

services:
  watchtower:
    image: containrrr/watchtower
    restart: unless-stopped
    volumes:
      - /var/run/docker.sock:/var/run/docker.sock
    environment:
      WATCHTOWER_CLEANUP: 'true'
      WATCHTOWER_POLL_INTERVAL: 14400
      WATCHTOWER_DEBUG: 'false'
      WATCHTOWER_LABEL_ENABLE: 'true'
  gui:
    container_name: openmower-gui
    labels:
      "com.centurylinklabs.watchtower.enable": true
      project: openmower
      app: gui
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
  web:
    container_name: mowgli-web
    labels:
      project: openmower
      app: web
    image: nginx
    ports:
      - 4005:80
    volumes:
      - ./web:/usr/share/nginx/html:ro
    restart: unless-stopped

  mosquitto:
    container_name: mowgli-mqtt
    labels:
      project: openmower
      app: mqtt
    hostname: mosquitto
    image: eclipse-mosquitto:latest
    ports:
      - 1883:1883
      - 9001:9001
    volumes:
      - ./config/mqtt/mosquitto.conf:/mosquitto/config/mosquitto.conf:ro
    restart: unless-stopped

  roscore:
    container_name: mowgli-roscore
    labels:
      project: openmower
      app: roscore
    image: ${IMAGE}
    network_mode: host
    tty: true
    privileged: true
    command:
      - /opt/ros/noetic/bin/roscore
    environment:
      ROS_IP: ${ROS_IP}
      ROSCONSOLE_CONFIG_FILE: /config/rosconsole.config
      ROSOUT_DISABLE_FILE_LOGGING: "true" # ou modifier cette ligne : true en "true" <==================
    tmpfs: /root/.ros/log/
    volumes:
      - ./config/om:/config
      - ./ros:/root/.ros/
      - /etc/timezone:/etc/timezone:ro
    restart: unless-stopped

  rosserial:
    container_name: mowgli-rosserial
    labels:
      project: openmower
      app: rosserial
    image: ${IMAGE}
    network_mode: host
    tty: true
    privileged: true
    logging:
      driver: "json-file"
      options:
        max-file: "5"
        max-size: 10m
    environment:
      ROS_MASTER_URI: http://${ROS_IP}:11311
      ROS_IP: ${ROS_IP}
      ROSCONSOLE_CONFIG_FILE: /config/rosconsole.config
      ROSOUT_DISABLE_FILE_LOGGING: "true" # ou modifier cette ligne : true en "true" <==================
    command:
      - /opt/ros/noetic/bin/rosrun
      - rosserial_server
      - serial_node
      - _port:=/dev/mowgli
      - _baud:=115200
    tmpfs: /root/.ros/log/
    volumes:
      - ./ros:/root/.ros/
      - ./config/om:/config
      - /etc/timezone:/etc/timezone:ro
      - /dev:/dev
    depends_on:
      - roscore
    restart: unless-stopped

  openmower:
    container_name: mowgli-openmower
    labels:
      project: openmower
      app: openmower
    image: ${IMAGE}
    network_mode: host
    tty: true
    privileged: true
    logging:
      driver: "json-file"
      options:
        max-file: "5"
        max-size: 10m
    environment:
      ROS_MASTER_URI: http://${ROS_IP}:11311
      ROS_IP: ${ROS_IP}
      ROSCONSOLE_CONFIG_FILE: /config/rosconsole.config
      ROSOUT_DISABLE_FILE_LOGGING: "true" # ou modifier cette ligne : true en "true" <==================
    tmpfs: /root/.ros/log/
    volumes:
      - ./config/om:/config
      - ./mower_params:/root/mower_params:ro
      - ./params:/opt/open_mower_ros/src/open_mower/params:ro
      - ./ros:/root/.ros/
      - /etc/timezone:/etc/timezone:ro
      - /dev:/dev
    depends_on:
      - rosserial
    restart: unless-stopped
```

Faites Ctrl "o" pour enregistrer et valider avec la touche "Entrée", puis Ctrl "x" pour sortir.

## Étape 8 : Démarrage des Conteneurs

Démarrez les conteneurs Docker (il faut etre dans le répertoire mowgli-docker, si vous n'y etes pas faites "cd mowgli-docker") :

```sh
sudo docker-compose up -d
```
Allez vous faire un café car cette partie peut etre longue, ça télécharge tout les fichier des conteneurs et donc dépend de la vitesse de votre connexion internet.

## Étape 9 : Comment arrêtez et mettre a jour les Conteneurs si besoin

Arrêter les conteneurs Docker :

```sh
sudo docker-compose down
```

Mettre à jour les conteneurs Docker :

Faites un pull (pas besoin d'arreter les conteneurs)
```sh
sudo docker-compose pull
```
Puis ensuite relancer les conteneurs
```sh
sudo docker-compose up -d
```

## Étape 10 : Surveillance des Logs

Surveillez les logs pour vérifier que tout fonctionne correctement :

```sh
sudo docker-compose logs -f
```
Pour sortir des logs, faites Ctrl "c"

## Étape 11 : Compilation et injection du Firmware Mowgli dans la carte mère du robot.

Cette étape est bien plus simple qu'il n'y paraît.
Pour cela on utilise [**Visual studio code**](https://code.visualstudio.com)
### 1. Téléchargement du repo Github
Normalement on utilise le même pour les 2 versions mais un problème (au 25-08-2024) sur le 500B oblige a en utiliser un différent.J'ai référencé ici les liens que j'ai utilisé et qui fonctionne.

Pour le 500 : 
https://github.com/cedbossneo/Mowgli/tree/main

**Lien direct : https://github.com/cedbossneo/Mowgli/archive/refs/heads/main.zip
> ***réaliser la Suite...***

Pour le 500B : 
https://github.com/jeremysalwen/Mowgli/tree/yardforce-500b

**Lien direct : https://github.com/jeremysalwen/Mowgli/archive/refs/heads/yardforce-500b.zip

> ***réaliser la Suite...***

### 2. Compilation du repo Github

> ***réaliser la Suite...***

### 3. Injection du firmware

Pour une raison quelquonque chez certains l'injection du firmware via vscode ne fonctionne pas, il est possible de l'injecter via le soft  [**STM32 Cube Programmer**](https://mega.nz/file/vdtVUZRB#A5RcIabdxEIuN2u6PzWVmGQnhNl94SxUVcujhE44MvA) : 

> ***réaliser la Suite...***

## Étape 12 : Configuration de démarrage du robot

Félicitations, votre robot tondeuse OpenMower Mowgli est maintenant préconfiguré et prêt à être utilisé, enfin presque car il reste la configuration su GPS, de l'application et la créations des cartes de tontes !

### 1. Configuration du gps (RTK F9P ou similaire)

Suivez ce [lien](https://openmower.de/docs/robot-assembly/prepare-the-parts/prepare-the-gps/) pour configurer la carte GPS (en anglais)

### 2. Configuration du robot (Openmower App)

Maintenant vous devriez avoir accès a ces 2 adresses : 

```plaintext
Tableau de bord détaillé avec paramètre : 

** http://adresse IP de votre raspberry pi:4006 **

```

```plaintext

Tableau de bord simplifier pour piloter et lancer/arrêter les tontes manuellement : 

** http://adresse IP de votre raspberry pi:4005 **

```

1. Pour commencer il faut parametrer votre position/coordonnée GPS pour centrer la carte de tonte sur ce point, ça ce passe dans les settings a l'adresse : 

```plaintext
Tableau de bord détaillé avec paramètre : 

** http://adresse IP de votre raspberry pi:4006 **

```
![capture ecran openmower](https://github.com/juditech3D/Guide-DIY-OpenMower-Mowgli-pour-Robots-Tondeuses-Yard500-et-500B/blob/main/images/Openmower%20app/capture%20ecran%20openmower%201.png)

2. Ensuite, entrer vos coordonnées gps dans "Datum" : latitude et longitude, placer le comme vous voulez en essayant de le placer le plus proche de votre futur zone de tonte, moi je l'ai placé au niveau de mon dock/chargeur.Pour les coordonnées j'ai utilisé ce site [Coordonnées GPS.fr](https://www.coordonnees-gps.fr).

![capture ecran openmower](#)

3. Après section suivante, "NTRIP" : ne modifer que la case "NTRIP server endpoint", c'est la ou vous allez renseigner la base rtk la plus proche de chez vous, pour la trouver c'est [<< ICI - Liste des bases rtk le plus proche >>](https://lvawebprojects.ovh/rtk/rtk.php).Il suffira juste de rentrer l'identifiant de la base (**en gras** colonne du milieu).Il parait evident de choisir ceux qui sont actif.

![capture ecran openmower](#)

4. Partie "Mower"
> ***réaliser la Suite...***

![capture ecran openmower](#)

5. Partie "Positioning"
> ***réaliser la Suite...***

![capture ecran openmower](#)

6. Partie "Docking"
> ***réaliser la Suite...***

![capture ecran openmower](#)



7. Et pour finir rendez vous  [ici](https://openmower.de/docs/software-setup/record-a-map/)  en version anglaise pour plus de détail sur le site officiel mais je vais vous détailler les grandes ligne ici : 

> ***réaliser la Suite...***

### ## **# Attention : si vous modifier votre map après la première tonte, il faudra redémarrer les dockers pour qu'elle soit pris en compte**


# ** Bonus ** 

## ## Pièces 3D
Liste des modélisations 3D que j'ai réaliser pour mon robot (je vous conseil de les imprimer en PETG ou ABS)

- Support Raspberry [Pi4](https://makerworld.com/en/@juditech3d)
- Support [F9P](https://makerworld.com/en/@juditech3d)

Si vous n'avez pas d'imprimante 3D, je peux vous les imprimer : Faites une demande [ICI](https://t.me/+mOlwROGsP3AyYTlk)


## ## Firmware personnalisés Mowgli compilé par mes propre soins.

Les firmware disponible ici sont a utilisés a vos propre risques, mais ils ont été tous testé avec succès a la date indiqué.

Attention : veuillez prendre soin de choisir le firmware qui correspond au modèle de votre carte mère.

- Firmware d'origine Yardforce 500 [Firmware Y500-Origine](#)
- Firmware d'origine Yardforce 500B [Firmware Y500B-Origine](https://mega.nz/file/LctRlDjA#o_DlA1pqDVFBnv7Dm9BDvJAm1jmBUdYOCP_2UW77QMc) mise à jour le 23/08/2024
- Firmware personnalisé Yardforce 500 [Firmware Y500-Mowgli](#)
- Firmware personnalisé Yardforce 500B [Firmware Y500-Mowgli](#)

Pour injecté le firmware d'origine j'ai utilisé le soft [**STM32 Cube Programmer**](https://mega.nz/file/vdtVUZRB#A5RcIabdxEIuN2u6PzWVmGQnhNl94SxUVcujhE44MvA)

  
## ## Support me / Soutenez moi

![https://buymeacoffee.com/juditech3d](https://github.com/juditech3D/Guide-DIY-OpenMower-Mowgli-pour-Robots-Tondeuses-Yard500-et-500B/blob/main/images/Soutien/bmc_qr-mini.png?raw=true)
[Buy me a coffee](https://buymeacoffee.com/juditech3d)

[Youtube @juditech3D](https://www.youtube.com/@juditech3d)

[TikTok @juditech3D](https://www.tiktok.com/@juditech3d)

[Instagram Juditech3D](https://www.instagram.com/juditech3d/)