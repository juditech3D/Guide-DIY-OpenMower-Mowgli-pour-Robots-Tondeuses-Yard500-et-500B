Ce projet est hébergé sur [GitHub](https://github.com/juditech3D/Guide-DIY-OpenMower).

Merci de votre intérêt pour ce guide ! 🙌


# Guide OpenMower Mowgli DIY Robot Tondeuse Yardforce 500 et 500B

Bienvenue dans ce guide détaillé en français pour la configuration et le déploiement de votre robot tondeuse OpenMower Mowgli Yardforce 500 et 500B. Ce tutoriel s'appuie sur mon expérience personnelle avec la construction du modèle Yardforce 500B.

**Ce guide est spécifiquement conçu pour les modèles Yardforce 500 et 500B équipés de leur carte mère d'origine (STM32 F103 et F402). Il peut éventuellement être applicable à d'autres modèles utilisant les mêmes cartes mères.**

## *⚠️ Avertissement : Dans le cadre de ce tutoriel, le clavier et les voyants situés sur le capot du Yardforce ne seront pas pleinement fonctionnels. Cela pourrait être problématique pour certains utilisateurs qui comptent sur ces fonctionnalités.⚠️* ##


**Pour le modèle Yardforce 500, certains voyants fonctionnent malgré tout sans modification du firmware du tableau de bord, notamment :**

- Le voyant de levage
- Le voyant de batterie faible
- Le voyant de charge

Ces fonctionnalités sont opérationnelles sans qu'il soit nécessaire de changer le firmware du tableau de bord.

## **⚠️ En revanche, pour le modèle Yardforce 500B, le clavier et les voyants sur le capot ne seront pas fonctionnels avec ce tutoriel. ⚠️** ##

## *⚠️Avertissement de non-responsabilité : Toute modification du firmware ou de la carte électronique peut entraîner des conséquences imprévues sur le fonctionnement de votre appareil, y compris des dommages potentiels. En suivant ce tutoriel, vous acceptez de procéder à vos propres risques. Les modifications électroniques sur la carte, en particulier, peuvent affecter le matériel de manière irréversible. De plus, ces modifications annuleront la garantie de votre appareil .Je décline toute responsabilité en cas de dysfonctionnement, dommage ou problème qui pourrait résulter de l'application de ces modifications.⚠️*

#  ⚠️ Important ⚠️

⚠️**Attention :** Ce guide est en cours de rédaction et évolue au fil du temps. Il est donc possible que certaines informations ne soient pas complètement à jour.⚠️

**Avant de commencer, il est essentiel de bien comprendre les avertissements et les prérequis nécessaires.**

Pour bien comprendre et réussir à suivre ce guide, il est important de visiter les ressources suivantes : 
- [Page d'accueil d'OpenMower](https://openmower.de/docs/robot-assembly/prepare-the-parts/prepare-the-robot/photo-guide/)
- [Fork Mowgli](https://github.com/cedbossneo/Mowgli)
- [Mowgli Docker](https://github.com/CedBossNeo/mowgli-docker)
- [Groupe d'entraide telegram français du fork Mowgli](https://t.me/+x6U3UwU5lB4yOWNk)

Ces liens vous fourniront des informations cruciales qui vous aideront à mieux comprendre les étapes et les concepts abordés dans ce guide.

## Prérequis pour utiliser ce guide (et matériels que j'ai utilisés)

- Module d'alimentation DC/DC [LM2596S](https://amzn.eu/d/jhNev6j)

- Raspberry Pi 4 conseillé https://amzn.eu/d/hwgFRWU  fonctionne normalement avec aussi un Pi 3 ou 5 )

- Carte GPS/Rtk [F9P ardusimple sans entête](https://fr.ardusimple.com/product/simplertk2b/?attribute_pa_header-options=without-headers) avec son [cable d'antenne](https://fr.aliexpress.com/item/1005004690761874.html?spm=a2g0o.order_list.order_list_main.23.27185e5bO83Ive&gatewayAdapt=glo2fra) ou sa base [BT-M61AJ3](https://fr.aliexpress.com/item/32991527632.html?spm=a2g0o.order_list.order_list_main.28.755e5e5bZP1cya&gatewayAdapt=glo2fra) et son antenne [Bt560](https://fr.aliexpress.com/item/32991527632.html?spm=a2g0o.order_list.order_list_main.28.755e5e5bZP1cya&gatewayAdapt=glo2fra) mais ayant eu des difficulté de réception ( arbres et buissons divers) de mon coté j'ai utilisé la [Bt603](https://fr.aliexpress.com/item/32991527632.html?spm=a2g0o.order_list.order_list_main.28.755e5e5bZP1cya&gatewayAdapt=glo2fra) qui est plus puissante mais plus chère.

- Putty ou similaire ( moi j'ai utilisé [mobaxterm]([[mobaxterm](https://mobaxterm.mobatek.net/download-home-edition.html)]), il y a aussi [Raspberry Pi Connect](https://connect.raspberrypi.com)

- Connaissances de base en ligne de commande conseillé ( pas obligatoire car je détails tout ici )

- Accès Internet (Pour une configuration plus rapide, vous pouvez le raccorder en Rj45 au début, mais attention l'IP qu'il faudra utiliser dans la suite du tuto est bien celle du wifi qui elle, est différente) 

# Voici un apercu du cablage complet du robot Mowgli (schématisé modifié par mes soins)

![alt text](image.png)
source d'origine : https://github.com/cedbossneo/mowgli-docker

# Important ⚠️

⚠️ Pour que le robot fonctionne, il faut OBLIGATOIREMENT un réseau Wifi disponible sur toute la surface qe vous voulez tondre ⚠️

# Préparation de la carte sd/SSD du raspberry pi

![Tuto raspberry pi imager](https://github.com/juditech3D/Guide-DIY-OpenMower-Mowgli-pour-Robots-Tondeuses-Yard500-et-500B/blob/main/M%C3%A9dia/Tuto%20raspberry%20pi%20imager%20%E2%80%90%20R%C3%A9alis%C3%A9e%20avec%20Clipchamp.gif)

### Étapes pour configurer un ou plusieurs réseaux Wi-Fi sur un Raspberry Pi 4

Pour le wifi et le SSH, 2 solutions : 
- Soit preconfigurer avec Pi Imager comme dans la vidéo ci-dessus
- soit le faire manuellement comme ci-dessous

#### 1. Préparation de la carte SD
<details>

  <summary> Dépliez pour voir </summary>

1. **Gravez l'image de Raspberry Pi OS (64 bits)** sur la carte SD en utilisant un outil comme Raspberry Pi Imager ou Balena Etcher.
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

</details>


#### 4. Activez SSH ( ⚠️ OBLIGATOIRE pour la suite du tutoriel ) Utile si vous ne l'avez pas activé dans Pi imager.

<details>
  <summary> Dépliez pour voir </summary>

1. **Pour activer SSH dès le premier démarrage**, créez un fichier vide nommé `ssh` (sans extension) dans la partition `boot`. Cela activera le service SSH automatiquement.

#### 5. Sauvegardez et éjectez

1. **Sauvegardez le fichier `wpa_supplicant.conf`** et fermez l'éditeur de texte.
2. **Éjectez la carte SD** de votre ordinateur en toute sécurité.

</details>

#### 6. Démarrez le Raspberry Pi 4

1. **Insérez la carte SD** dans le Raspberry Pi 4.
2. **Allumez le Raspberry Pi**. Lors du démarrage, il lira le fichier `wpa_supplicant.conf` et se connectera au(x) réseau(x) Wi-Fi configuré(s). Si vous avez activé SSH, vous pourrez vous connecter au Raspberry Pi via l'adresse IP sur le réseau : pour ça vous pouvez la trouver via votre box opérateur ou via un soft, moi j'utilise [Advanced IP Scanner](https://www.advanced-ip-scanner.com/fr/), il aura normalement le nom que vous lui aurez donner, avec comme nom de fabricant = Rasberry Pi ou similaire

![Advanced IP Scanner](https://github.com/juditech3D/Guide-DIY-OpenMower-Mowgli-pour-Robots-Tondeuses-Yard500-et-500B/blob/main/images/Advanced%20ip%20scanner/Advanced%20ip%20scanner.png)

Cette méthode vous permet de configurer un ou plusieurs réseaux Wi-Fi de manière flexible pour votre Raspberry Pi 4, avec la possibilité d'établir des priorités entre les réseaux si nécessaire.

# ⚠️ Important ⚠️

## **⚠️ N'oubliez pas de fixer l'adresse IP ou l'adresse mac de votre rasberry pi via votre box pour qu'il soit toujours accessible.⚠️**

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

## Étape 3 : Configuration de udev

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
# UM982 - WittMotion WTRTK-982
SUBSYSTEM=="tty" ATTRS{idVendor}=="1a86" ATTRS{idProduct}=="7523", SYMLINK+="gps"
```

(si GPS spécifique, utilisez la commande `lsusb` une fois votre appareil connecté afin de trouver les vendorId et productID)

Faites Ctrl "o" pour enregistrer et valider avec la touche "Entrée", puis Ctrl "x" pour sortir.

3. Rechargez les règles udev :

```sh
sudo udevadm control --reload-rules
sudo udevadm trigger
```

## Étape 4 : Clonage du dépôt

On récupère le code pour la génération des containers dans docker
Clonez le dépôt GitHub avec la commande suivante :

```sh
sudo apt install git
git clone https://github.com/cedbossneo/mowgli-docker
cd mowgli-docker
```

## Étape 5 : Configuration de l'environnement

1. Créez et éditez le fichier `.env` (il faut etre dans le répertoire mowgli-docker, si vous n'y etes pas faites "cd mowgli-docker") :

```sh
sudo nano .env
```

2. Remplacez les valeurs `ROS_IP` et `MOWER_IP` par l'adresses IP de votre raspberry (pour info le pavé numerique ne fonctionne pas , déplacer le curseur avec les fleche de navigation) :

```sh
# ROS_IP est l'IP de la machine exécutant le conteneur Docker
# MOWER_IP est l'IP de la tondeuse
# Lorsque vous n'êtes pas en mode ser2net, les deux IPs doivent être les mêmes
ROS_IP=192.168.X.XX
MOWER_IP=192.168.X.XX
IMAGE=ghcr.io/cedbossneo/mowgli-docker:cedbossneo
```

Faites Ctrl "o" pour enregistrer et valider avec la touche "Entrée", puis Ctrl "x" pour sortir.



## Étape 6 : Lancement des Conteneurs

1. Lancez les conteneurs Docker (assurez-vous d’être dans le répertoire « mowgli-docker ». Si ce n’est pas le cas, exécutez la commande « cd mowgli-docker ») :

```sh
sudo docker compose up -d
```

Prenez une pause café, car cette étape peut être longue. Le téléchargement des fichiers des conteneurs dépend de la vitesse de votre connexion Internet.

2. # ⚠️ Important ⚠️

⚠️ Attention : Depuis le 12/10/2024, il est nécessaire de mettre à jour un fichier de configuration pour assurer le bon fonctionnement de votre robot. Sans cela, le robot risque de ne pas fonctionner correctement. Pour ce faire, éditez le fichier `mower_config.sh` (assurez-vous d’être dans le répertoire « mowgli-docker ». Si ce n’est pas le cas, exécutez « cd mowgli-docker ») :

```sh
sudo nano config/om/mower_config.sh
```

Ajoutez la ligne suivante ou modifiez la si présente : 
```sh
export OM_NO_COMMS=true
```

Pour utiliser un UM-982, il est nécessaire de modifier le paramètre `OM_GPS_PROTOCOL` pour le mettre à la valeur `NMEA`:
```sh
# GPS protocol. Use UBX for u-blox chipsets and NMEA for everything else
export OM_GPS_PROTOCOL=NMEA
```

Ensuite, appuyez sur « Ctrl + o » pour enregistrer, validez avec la touche « Entrée », puis appuyez sur « Ctrl + x » pour quitter l’éditeur.

Enfin, redémarrez le Raspberry Pi :
```sh
sudo reboot
```


## Étape 7 : Comment arrêtez et mettre a jour les Conteneurs si besoin

Arrêter les conteneurs Docker :

```sh
sudo docker compose down
```
<details>
  <summary>Mettre à jour les conteneurs Docker ( Attention à utiliser que si vous savez ce que vous faites : peut faire planter openmower ) :</summary>
Mettre à jour les conteneurs Docker :

Faites un pull (pas besoin d'arreter les conteneurs)
```sh
sudo docker compose pull
```
Puis ensuite relancer les conteneurs
```sh
sudo docker compose up -d
```
</details>

## Étape 8 : Surveillance des Logs (optionnel)
<details>
  <summary>Surveillez les logs pour vérifier que tout fonctionne correctement : utile pour débugage</summary>


```sh
sudo docker compose logs -f
```
Pour sortir des logs, faites Ctrl "c"
</details>

## Étape 9 : Compilation et injection du Firmware Mowgli dans la carte mère du robot.

### 0. Sauvegarde du firmware initial
Brancher le STLINK avec la GND sur GND, le 3.3V sur le 3.3V, le SWCLK sur le SWCL, le SWDIO sur le SWDA.
La sauvegarde du firmware initial peut être faite avec le logiciel STM32CubeProgrammer (Connect -> ReadAll -> SaveAs)

### 1. Téléchargement du repo Github
Normalement on utilise le même pour les 2 versions mais un problème (au 25-08-2024) sur le 500B oblige a en utiliser un différent.J'ai référencé ici les liens que j'ai utilisé et qui fonctionne.

Pour le 500 : 
https://github.com/cedbossneo/Mowgli/tree/main

**Lien direct : https://github.com/cedbossneo/Mowgli/archive/refs/heads/main.zip
> ***réaliser la Suite...***

Pour le 500B : 
https://github.com/Nekraus/Mowgli/tree/yardforce-500b

**Lien direct : https://github.com/Nekraus/Mowgli/archive/refs/heads/yardforce-500b.zip

> ***réaliser la Suite...***

### 2. Compilation du repo Github

> ***réaliser la Suite...***

### 3. Injection du firmware

Cette étape est bien plus simple qu'il n'y paraît.
Pour cela on utilise [**Visual studio code**](https://code.visualstudio.com)

Pour une raison quelquonque chez certains l'injection du firmware via vscode ne fonctionne pas, il est possible de l'injecter via le soft  [**STM32 Cube Programmer**](https://mega.nz/file/vdtVUZRB#A5RcIabdxEIuN2u6PzWVmGQnhNl94SxUVcujhE44MvA) : 

Brancher le STLINK avec la GND sur GND, le 3.3V sur le 3.3V, le SWCLK sur le SWCL, le SWDIO sur le SWDA.

> ***réaliser la Suite...***

## Étape 10 : Configuration de démarrage du robot

Félicitations, votre robot tondeuse OpenMower Mowgli est maintenant préconfiguré et prêt à être utilisé, enfin presque car il reste la configuration du GPS, de l'application et la créations des cartes de tontes !

# Important ⚠️

⚠️ Attention : Ne modifiez que les paramètres spécifiquement indiqués dans ce tutoriel. Toute modification de tout autre paramètre pourrait entraîner un dysfonctionnement des applications et/ou du robot.⚠️


### 1. Configuration du gps (RTK F9P ou similaire)

Suivez ce [lien](https://openmower.de/docs/robot-assembly/prepare-the-parts/prepare-the-gps/) pour configurer la carte GPS (en anglais).

# TODO
Pour le UM982 suivre les [instructions](https://wiki.openmower.de/index.php?title=Unicore_GPS_modules)

### 2. Configuration du robot (Openmower App)

Maintenant vous devriez avoir accès a ces 2 adresses : 

```plaintext
Tableau de bord détaillé avec paramètre : 

** http://adresse IP de votre raspberry pi:4006 **

```

```plaintext

Tableau de bord simplifié (adapté pour tablette ou smartphone) pour piloter et lancer/arrêter les tontes manuellement : 

** http://adresse IP de votre raspberry pi:4005 **

```

1. Pour commencer il faut parametrer votre position/coordonnée GPS pour centrer la carte de tonte sur ce point, ça ce passe dans les settings a l'adresse : 

# Important ⚠️

⚠️ Attention : Pour minimiser les erreurs, je vous recommande d'effectuer la configuration via la page web "4006" en utilisant un PC plutôt qu'un téléphone.⚠️

```plaintext
Tableau de bord détaillé avec paramètre : 

** http://adresse IP de votre raspberry pi:4006 **

```
![capture ecran openmower](https://github.com/juditech3D/Guide-DIY-OpenMower-Mowgli-pour-Robots-Tondeuses-Yard500-et-500B/blob/main/images/Openmower%20app/capture%20ecran%20openmower%201.png)

2. Ensuite, entrer vos coordonnées gps dans "Datum" : latitude et longitude, placer le comme vous voulez en essayant de le placer le plus proche de votre futur zone de tonte, moi je l'ai placé au niveau de mon dock/chargeur.Pour les coordonnées j'ai utilisé ce site [Coordonnées GPS.fr](https://www.coordonnees-gps.fr). Vous pouvez utiliser aussi le site indiqué ci-dessous au point #3.

![capture ecran openmower](https://github.com/juditech3D/Guide-DIY-OpenMower-Mowgli-pour-Robots-Tondeuses-Yard500-et-500B/blob/main/images/Coordonn%C3%A9es%20GPS/coordonn%C3%A9es%20gps%20openmower%201.png?raw=true)

3. Après section suivante, "NTRIP" : ne modifer que la case "NTRIP server endpoint", c'est la ou vous allez renseigner la base rtk la plus proche de chez vous ( l'idéale est - de 20km), pour la trouver c'est [<< ICI - Liste des bases rtk le plus proche >>](https://lvawebprojects.ovh/rtk/rtk.php).Il suffira juste de rentrer l'identifiant de la base (**en gras** colonne du milieu).Il parait evident de choisir ceux qui sont actif.

![capture ecran openmower](https://github.com/juditech3D/Guide-DIY-OpenMower-Mowgli-pour-Robots-Tondeuses-Yard500-et-500B/blob/main/images/Coordonn%C3%A9es%20GPS/coordonn%C3%A9es%20gps%20openmower%202.png?raw=true)

# Important ⚠️

⚠️ **Attention :  Pour des raisons de sécurité évidente, ne partagez jamais votre position réelle avec d'autres personnes.** ⚠️

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

## **⚠️ Attention : si vous modifier votre map après la première tonte, il faudra redémarrer les dockers pour qu'elle soit pris en compte**⚠️

## ⚠️ Attention : je vous conseil de  sauvegarder votre map régulièrement a chaque changement important au cas ou ⚠️



# ** FAQ et Problèmes divers ** 

### 1- Un bug au niveau de la fusion des cartes de tontes (https://github.com/cedbossneo/openmower-gui/issues/51) fait planter openmower : pour résoudre le problème le seul moyen est de supprimer le fichier "map.bag" ( il faut etre dans le répertoire mowgli-docker, si vous n'y etes pas faites "cd mowgli-docker" ) :

Dont voici la commande : 
```sh
sudo rm ./ros/map.bag
```
Ensuite , il faudra recréer votre carte ou importer à nouveau celle que vous aviez sauvegardé en ayant préalablement enregistrer votre base AVANT (impératif).

# ** Bonus ** 

## ## Pièces 3D
Liste des modélisations 3D que j'ai réaliser pour mon robot (je vous conseil de les imprimer en PETG ou ABS)

- Support Raspberry [Pi4](https://makerworld.com/en/@juditech3d)
- Support [F9P](https://makerworld.com/en/@juditech3d)
- Roue lestable + pneu [Yardforce 500/500B](https://makerworld.com/en/@juditech3d)
- Gyrophare multifonction [Yardforce 500/500B](https://makerworld.com/en/@juditech3d)

Si vous n'avez pas d'imprimante 3D, je peux vous les imprimer : Faites une demande [ICI](https://t.me/+mOlwROGsP3AyYTlk) ou [Par mail](juditech3d@gmail.com)


## ## Firmware personnalisés Mowgli compilé par mes propre soins.

Les firmware disponible ici sont à utiliser a vos propre risques, mais ils ont été tous testé avec succès a la date indiqué.

Attention : veuillez prendre soin de choisir le firmware qui correspond au modèle de votre carte mère.

- ~~Firmware d'origine Yardforce 500 [Firmware Y500-Origine](#)~~
- Firmware d'origine Yardforce 500B [Firmware Y500B-Origine](https://mega.nz/file/LctRlDjA#o_DlA1pqDVFBnv7Dm9BDvJAm1jmBUdYOCP_2UW77QMc) mise à jour le 23/08/2024
- ~~Firmware personnalisé Yardforce 500 [Firmware Y500-Mowgli](#)~~
- ~~Firmware personnalisé Yardforce 500B [Firmware Y500-Mowgli](#)~~

Pour injecter le firmware d'origine j'ai utilisé le soft [**STM32 Cube Programmer**](https://mega.nz/file/vdtVUZRB#A5RcIabdxEIuN2u6PzWVmGQnhNl94SxUVcujhE44MvA)

  
## ## Support me / Soutenez moi

![https://buymeacoffee.com/juditech3d](https://github.com/juditech3D/Guide-DIY-OpenMower-Mowgli-pour-Robots-Tondeuses-Yard500-et-500B/blob/main/images/Soutien/bmc_qr-mini.png?raw=true)
[Buy me a coffee](https://buymeacoffee.com/juditech3d)

[Youtube @juditech3D](https://www.youtube.com/@juditech3d)

[TikTok @juditech3D](https://www.tiktok.com/@juditech3d)

[Instagram Juditech3D](https://www.instagram.com/juditech3d/)
