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




# Préparation du raspberry pi et configuration

## Étape 1 : Mise à jour de Pi OS

Avant de commencer, assurez-vous que votre système est à jour.

```sh
sudo apt update && sudo apt upgrade -y
```

## Étape 2 : Installation de Docker

Installez Docker en exécutant la commande suivante :

```sh
curl -fsSL https://get.docker.com | sh
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
nano .env
```

2. Remplacez les valeurs `ROS_IP` et `MOWER_IP` par l'adresses IP de votre raspberry :

```sh
# ROS_IP est l'IP de la machine exécutant le conteneur Docker
# MOWER_IP est l'IP de la tondeuse
# Lorsque vous n'êtes pas en mode ser2net, les deux IPs doivent être les mêmes
ROS_IP=192.168.1.34
MOWER_IP=192.168.1.34
IMAGE=ghcr.io/cedbossneo/mowgli-docker:cedbossneo
```

Faites Ctrl "o" pour enregistrer et valider avec la touche "Entrée", puis Ctrl "x" pour sortir.

## Étape 7 : Mise à jour de docker-compose.yaml ( mettre à jour suite a une erreur comme quoi le fichier était obsolète donc obligatoire sinon vous aurez une erreur et vous ne pourrez pas démarrer les dockers)

![Erreur docker-compose.yaml](https://github.com/juditech3D/Guide-DIY-OpenMower-Mowgli-pour-Robots-Tondeuses-Yard500-et-500B/blob/main/images/Solarputty/Erreur%20docker-compose.yaml.png)


Ouvrez le fichier `docker-compose.yaml` :

```sh
sudo nano docker-compose.yaml
```

Remplacez le contenu par le suivant ou modifier simplement les lignes indiquer ci-dessous par cette fleche <============= :

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


Félicitations, votre robot tondeuse OpenMower Mowgli est maintenant configuré et prêt à être utilisé, enfin presque car il reste la configuration de l'application et la créations des cartes de tontes !

## Bonus : 

## Pièces 3D
Liste des modélisations 3D que j'ai réaliser pour mon robot (je vous conseil de les imprimer en PETG ou ABS)

- Support Raspberry [Pi4](https://makerworld.com/en/@juditech3d)
- Support [F9P](https://makerworld.com/en/@juditech3d)

Si vous n'avez pas d'imprimante 3D, je peux vous les imprimer : Rendez-vous [ICI](#)


## Firmware personnalisés Mowgli compilé par mes propre soins.

Les firmware disponible ici sont a utilisés a vos propre risques, mais ils ont été tous testé avec succès a la date indiqué.

Attention : veuillez prendre soin de choisir le firmware qui correspond au modèle de votre carte mère, choisir le modèle 500 ou 500B ne suffit pas.

- Firmware d'origine Yardforce 500 [Firmware Y500-Origine](#)
- Firmware d'origine Yardforce 500B [Firmware Y500B-Origine](#)
- Firmware personnalisé Yardforce 500 [Firmware Y500-Mowgli](#)
- Firmware personnalisé Yardforce 500B [Firmware Y500-Mowgli](#)

  
