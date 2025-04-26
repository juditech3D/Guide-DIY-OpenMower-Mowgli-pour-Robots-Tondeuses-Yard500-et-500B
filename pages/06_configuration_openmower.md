---
title: "⚙️ Configuration OpenMower"
nav_order: 6
permalink: /06_configuration_openmower/
parent: "🏠 Guide OpenMower"
layout: default
---

# ⚙️ Configuration OpenMower

---

## Configuration de démarrage du robot

Félicitations, votre robot tondeuse OpenMower Mowgli est maintenant préconfiguré et prêt à être utilisé... ou presque ! Il reste à configurer le GPS, l'application et créer les cartes de tontes !

---

<div class="alert-red">
  <div class="alert-title">⚠️ Important</div>
  <p>Ne modifiez que les paramètres spécifiquement indiqués dans ce tutoriel. Toute modification non prévue pourrait entraîner un dysfonctionnement des applications ou du robot.</p>
</div>

---

## 📡 1. Configuration du GPS (RTK F9P ou similaire)

Suivez ce <a href="https://openmower.de/docs/robot-assembly/prepare-the-parts/prepare-the-gps/" target="_blank">lien officiel OpenMower</a> pour configurer votre carte GPS.

**TODO :** Pour le UM982, suivez <a href="https://wiki.openmower.de/index.php?title=Unicore_GPS_modules" target="_blank">cette documentation spécifique</a>.

---

## 🖥️ 2. Configuration du robot via OpenMower App

Vous devriez désormais avoir accès aux deux interfaces suivantes :

```plaintext
Interface avancée (PC) :
http://adresse-IP-du-raspberry:4006
```

```plaintext
Interface simplifiée (Smartphone/Tablette) :
http://adresse-IP-du-raspberry:4005
```

<div class="alert-red">
  <div class="alert-title">⚠️ Important</div>
  <p>Pour minimiser les erreurs, privilégiez la configuration sur la page "4006" via un ordinateur plutôt qu'un téléphone.</p>
</div>

![Capture écran OpenMower]({{ '/assets/img/openmower_app_capture1.png' | relative_url }})

### 2.1 Renseigner les coordonnées GPS de votre terrain (Datum)

Utilisez un site comme <a href="https://www.coordonnees-gps.fr/" target="_blank">Coordonnées-GPS.fr</a> pour obtenir votre latitude et longitude.

![Coordonnées GPS OpenMower]({{ '/assets/img/coordonnees_gps_openmower.png' | relative_url }})

### 2.2 Configuration NTRIP (correction GPS en temps réel)

Ne modifiez que **"NTRIP server endpoint"**.

Utilisez ce site pour trouver une base proche de chez vous : <a href="https://lvawebprojects.ovh/rtk/rtk.php" target="_blank">Bases RTK disponibles</a>.

![Configuration NTRIP]({{ '/assets/img/ntrip_config_openmower.png' | relative_url }})

<div class="alert-red">
  <div class="alert-title">⚠️ Confidentialité</div>
  <p>Pour votre sécurité, ne partagez jamais votre position GPS réelle.</p>
</div>

---

## 🛠️ TODO : Suite de la configuration OpenMower

### 🛠️ Partie "Mower"
> ***À compléter...***

![Image placeholder]({{ '/assets/img/mower_section_placeholder.png' | relative_url }})

### 🛠️ Partie "Positioning"
> ***À compléter...***

![Image placeholder]({{ '/assets/img/positioning_section_placeholder.png' | relative_url }})

### 🛠️ Partie "Docking"
> ***À compléter...***

![Image placeholder]({{ '/assets/img/docking_section_placeholder.png' | relative_url }})

---

## 🗺️ 3. Création et sauvegarde de la carte de tonte

Consultez la documentation officielle OpenMower :

➡️ <a href="https://openmower.de/docs/software-setup/record-a-map/" target="_blank">Créer et enregistrer une carte (record-a-map)</a>

> ***Résumé et simplification de la procédure en cours d'intégration ici...***

<div class="alert-red">
  <div class="alert-title">⚠️ Astuce importante</div>
  <p>Si vous modifiez votre carte après la première tonte, pensez à redémarrer les containers Docker pour que les changements soient pris en compte.</p>
  <p>💾 Sauvegardez régulièrement votre carte en cas de modifications majeures.</p>
</div>

---

# 📚 Navigation

<div style="display: flex; justify-content: space-between; margin-top: 2rem;">
  <a href="{{ '/pages/sommaire/' | relative_url }}" class="btn">⬅️ Retour au Sommaire</a>
  <a href="{{ '/07_bonus/' | relative_url }}" class="btn">➡️ Suivant : Bonus Pièces 3D & Firmwares</a>
</div>

---
