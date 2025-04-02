---
title: "🧰 Matériel requis"
nav_order: 2
parent: "🏠 Guide OpenMower"
layout: default
---

# 🧰 Matériel requis

Ce projet repose sur l’utilisation de la carte mère **d’origine** des modèles **Yardforce 500 / 500B**, sans modification matérielle profonde de celle-ci. Voici le matériel que j’ai personnellement utilisé, testé et validé pour réaliser mon robot tondeuse autonome.

---

## 🧩 Liste du matériel recommandé

| Équipement | Référence / Lien | Remarques |
|-----------|------------------|-----------|
| 🧠 **Raspberry Pi 4** | [Lien recommandé](https://raspberrytips.fr/amz/pi4) | 4 Go minimum – fonctionne aussi avec Pi 3 ou Pi 5 |
| 🔌 **Convertisseur DC/DC LM2596S** | [Amazon](https://amzn.eu/d/jhNev6j) | Alimentation 5V stable pour le RPi |
| 📡 **GPS RTK F9P (sans header)** | [Ardusimple](https://fr.ardusimple.com/product/simplertk2b/?attribute_pa_header-options=without-headers) | Pour une précision centimétrique |
| 🔌 **Câble d’antenne F9P vers antenne** | [Aliexpress](https://fr.aliexpress.com/item/1005004690761874.html) | Indispensable pour raccorder votre antenne |
| 📶 **Antenne BT-603** | [Aliexpress BT-603](https://fr.aliexpress.com/item/32991527632.html) | Plus puissante que la BT-560 |
| 🛜 **Connexion Internet** | RJ45 ou Wi-Fi | Nécessaire pour SSH et téléchargement de paquets |
| 🖥️ **Logiciel SSH (PC)** | [Mobaxterm](https://mobaxterm.mobatek.net/download-home-edition.html) ou [Raspberry Pi Connect](https://connect.raspberrypi.com) | Pour se connecter au Pi à distance |
| 💾 **Carte microSD / SSD** | min. 16 Go | Préférez un SSD pour de meilleures performances |

---

<div class="alert-orange">
  <div class="alert-title">💡 Recommandation</div>
  N'oubliez pas de prévoir un accès Wi-Fi **stable sur toute la surface de tonte**. OpenMower Mowgli a besoin de rester connecté en permanence au réseau local pour fonctionner correctement.
</div>

---

## 🖨️ Pièces imprimées en 3D personnalisées

Des pièces **sur mesure** (support RPi, support F9P, roues lestables, gyrophare, etc.) ont été **spécialement conçues** pour ce projet.

Elles sont disponibles **gratuitement** sur mon profil MakerWorld :

👉 [Profil MakerWorld Juditech3D](https://makerworld.com/en/@juditech3d)

Pour plus de détails sur les fichiers et impressions recommandées :  
📦 [Voir la section Bonus]({{ '/07_bonus/' | relative_url }})

---

## 🖼️ Schéma de câblage

Voici un aperçu du câblage complet du robot Mowgli, réalisé sur la base du schéma de <a href="https://github.com/cedbossneo/mowgli-docker" target="_blank">cedbossneo</a>, que j’ai **adapté pour plus de clarté** :

![Schéma de câblage Mowgli](../images/Diagramme%20sans%20nom.drawio.png)

> ✅ Ce schéma est également modifiable au format `.drawio` dans la page dédiée : [Voir le schéma interactif]({{ '/schema/' | relative_url }})

---

<div class="alert-green">
  <div class="alert-title">🧠 Astuce</div>
  Si vous n’avez pas d’imprimante 3D, je peux vous imprimer les pièces nécessaires à votre projet. Contactez-moi sur <a href="https://t.me/+mOlwROGsP3AyYTlk" target="_blank">Telegram</a> ou <a href="mailto:juditech3d@gmail.com">par email</a>.
</div>
