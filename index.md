---
layout: default
title: "Guide OpenMower Mowgli DIY Robot Tondeuse Yard 500 et 500B"
nav_order: 1
---

# Guide OpenMower Mowgli DIY Robot Tondeuse Yard 500 et 500B

Bienvenue dans ce guide complet pour configurer et déployer votre robot tondeuse OpenMower Mowgli Yard 500 et 500B en français, basé sur mon expérience de la construction du robot Yardforce 500B.

## Table des matières
1. [Introduction](#introduction)
2. [Prérequis](#prérequis)
3. [Mise à jour de Pi OS](#mise-à-jour-de-pi-os)
4. [Installation de Docker](#installation-de-docker)
5. [Installation de Docker Compose](#installation-de-docker-compose)
6. [Configuration de udev](#configuration-de-udev)
7. [Clonage du dépôt](#clonage-du-dépôt)
8. [Configuration de l'environnement](#configuration-de-lenvironnement)
9. [Mise à jour de docker-compose.yaml](#mise-à-jour-de-docker-composeyaml)
10. [Démarrage des Conteneurs](#démarrage-des-conteneurs)
11. [Surveillance des Logs](#surveillance-des-logs)
12. [Félicitations](#félicitations)

## Introduction

Pour plus d'informations, visitez les pages :

- [Page d'accueil d'OpenMower](https://github.com/ClemensElflein/OpenMower)
- [Fork Mowgli](https://github.com/cedbossneo/OpenMower)
- [Mowgli Docker](https://github.com/cedbossneo/mowgli-docker)

## Prérequis

- Raspberry Pi avec Pi OS installé (cela suppose que vous avez déjà créé votre SD ou SSD (pour le Pi 4) et qu'il démarre sans problème sur le Pi)
- Carte GPS/RTK F9P avec son câble d'antenne et son antenne Bt560 (mais ayant eu des difficultés de réception de mon côté, j'ai utilisé la Bt603 qui est plus puissante mais plus chère)
- Putty ou similaire (moi j'ai utilisé Solarputty)
- Connaissances de base en ligne de commande (pas obligatoire car je détaille tout ici)
- Accès Internet (la configuration)

## Mise à jour de Pi OS

Avant de commencer, assurez-vous que votre système est à jour.

```bash
sudo apt update && sudo apt upgrade -y
