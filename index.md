---
title: Home
layout: home
---
Attention ce tuto n'est pas finalisé, il est en cours de rédaction

Guide OpenMower Mowgli DIY Robot Tondeuse Yard 500 et 500B
Bienvenue dans ce guide complet pour configurer et déployer votre robot tondeuse OpenMower Mowgli Yard 500 et 500B en français, basé sur mon expérience de la construction du robot Yardforce 500B.

Pour plus d'informations, visitez les pages :

Page d'accueil d'OpenMower
Fork Mowgli
Mowgli Docker
Prérequis (et matériels que j'ai utilisés)
Raspberry Pi avec Pi OS installé ( cela suppose que vous avez déja créer votre sd ou ssd (pour le pi 4) et qu'il démarre sans problème sur le pi)
Carte GPS/Rtk F9P avec son cable d'antenne et son antenne Bt560 mais ayant eu des difficulté de réception de mon coté j'ai utilisé la Bt603 qui est plus puissante mais plus chère.
Putty ou similaire (moi j'ai utilisé solarputty)
Connaissances de base en ligne de commande ( pas obligatoire car je détails tout ici )
Accès Internet (la configuration)
Étape 1 : Mise à jour de Pi OS
Avant de commencer, assurez-vous que votre système est à jour.

sudo apt update && sudo apt upgrade -y
Étape 2 : Installation de Docker
Installez Docker en exécutant la commande suivante :

curl -fsSL https://get.docker.com | sh
Étape 3 : Installation de Docker Compose
Installez Docker Compose avec la commande suivante :

sudo apt install docker-compose -y
Étape 4 : Configuration de udev
Créez et éditez le fichier de règles udev :
sudo nano /etc/udev/rules.d/50-mowgli.rules
Ajoutez les règles suivantes :
SUBSYSTEM=="tty" ATTRS{product}=="Mowgli", SYMLINK+="mowgli"
# simpleRTK USB
SUBSYSTEM=="tty" ATTRS{idVendor}=="1546" ATTRS{idProduct}=="01a9", SYMLINK+="gps"
# ESP USB CDC - RTK1010Board
SUBSYSTEM=="tty" ATTRS{idVendor}=="303a" ATTRS{idProduct}=="4001", SYMLINK+="gps"
Rechargez les règles udev :
sudo udevadm control --reload-rules
sudo udevadm trigger
Étape 5 : Clonage du dépôt
Clonez le dépôt GitHub avec la commande suivante :

git clone https://github.com/cedbossneo/mowgli-docker
cd mowgli-docker
Étape 6 : Configuration de l'environnement
Créez et éditez le fichier .env :
nano .env
Remplacez les valeurs ROS_IP et MOWER_IP par l'adresses IP de votre raspberry :
# ROS_IP est l'IP de la machine exécutant le conteneur Docker
# MOWER_IP est l'IP de la tondeuse
# Lorsque vous n'êtes pas en mode ser2net, les deux IPs doivent être les mêmes
ROS_IP=192.168.1.34
MOWER_IP=192.168.1.34
IMAGE=ghcr.io/cedbossneo/mowgli-docker:cedbossneo
Étape 7 : Mise à jour de docker-compose.yaml
Ouvrez le fichier docker-compose.yaml :

nano docker-compose.yaml
Remplacez le contenu par le suivant :

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
      ROSOUT_DISABLE_FILE_LOGGING: "true"
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
      ROSOUT_DISABLE_FILE_LOGGING: "true"
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
      ROSOUT_DISABLE_FILE_LOGGING: "true"
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
Étape 8 : Démarrage des Conteneurs
Démarrez les conteneurs Docker :

sudo docker-compose up -d
Étape 9 : Surveillance des Logs
Surveillez les logs pour vérifier que tout fonctionne correctement :

sudo docker-compose logs -f
Félicitations, votre robot tondeuse OpenMower Mowgli est maintenant configuré et prêt à être utilisé !
