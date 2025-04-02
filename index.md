---
title: "🏠 Guide OpenMower"
nav_order: 0
has_children: true
permalink: /
layout: default
---

# Bienvenue sur le Guide DIY OpenMower Mowgli 🚜

Ce guide a pour but de vous accompagner pas à pas dans la configuration du robot tondeuse **OpenMower** basé sur le modèle **Yardforce 500 ou 500B**, en utilisant le fork **Mowgli** et le dépôt **mowgli-docker**. 

Ce projet repose sur mon expérience personnelle de construction d’un robot complet, que j’ai souhaité **documenter** et **vulgariser** au maximum afin d’en **faciliter la reproduction** par d’autres passionnés, débutants comme confirmés.

---

<div class="alert-red">
  <strong style="font-size: 1.2em;">⚠️ 👉 Commencez par lire les avertissements !</strong><br>
  Ce guide implique des modifications électroniques et logicielles importantes. Il est crucial de prendre connaissance des avertissements pour éviter les erreurs ou dommages.
</div>

---
<div class="alert-blue">
  <div class="alert-title">ℹ️ Ce projet est une adaptation</div>
  Ce guide est basé sur une <strong>adaptation du projet OpenMower original</strong> qui repose sur une carte mère dédiée (<em>OpenMower V1</em>) et un système OpenMower OS.

  ⚠️ <strong>Ce tutoriel n’est pas compatible avec OpenMower OS.</strong><br><br>

  Le projet ici présenté est conçu pour les modèles <strong>Yardforce 500 / 500B</strong> ou série SA ou similaire en utilisant leur <strong>carte mère d’origine</strong> (STM32 F103/F402).<br>
  👉 Il reprend certaines étapes du projet officiel mais adapté spécifiquement à l’architecture Yardforce.<br><br>

  🔗 Consultez le site officiel du projet OpenMower :  
  <a href="https://openmower.de" target="_blank">https://openmower.de</a>
</div>

---

## 📂 Sections disponibles

Ce guide est organisé en plusieurs parties accessibles via le menu latéral :

- [⚠️ Avertissements]({{ '/01_avertissements/' | relative_url }})
- [🧰 Matériel requis]({{ '/02_materiel_requis/' | relative_url }})
- [💾 Installation & préparation]({{ '/03_installation_preparation/' | relative_url }})
- [⚙️ Configuration]({{ '/04_configuration/' | relative_url }})
- [📥 Injection du firmware]({{ '/05_injection_firmware/' | relative_url }})
- [📱 Configuration OpenMower]({{ '/06_configuration_openmower/' | relative_url }})
- [🎁 Bonus : pièces 3D & firmwares]({{ '/07_bonus/' | relative_url }})
- [❓ FAQ]({{ '/08_faq/' | relative_url }})
- [💖 Soutenir]({{ '/09_soutenir/' | relative_url }})
- [🔗 Liens utiles]({{ '/10_liens_utiles/' | relative_url }})
- [📝 Mises à jour du guide]({{ '/mise_a_jour/' | relative_url }})


---

<div class="alert-orange">
  <div class="alert-title">📎 Ressources utiles et indispensables pour comprendre le tuto</div>
  Avant de vous lancer, je vous recommande vivement de consulter les ressources suivantes. Elles vous donneront une meilleure compréhension du projet OpenMower, du fork Mowgli et des outils utilisés tout au long de ce guide :

  <ul>
    <li>🌐 <a href="https://openmower.de/docs/">Page d’accueil OpenMower (officiel)</a></li>
    <li>🧠 <a href="https://github.com/cedbossneo/Mowgli">Fork Mowgli (par CedBossNeo)</a></li>
    <li>🐳 <a href="https://github.com/cedbossneo/mowgli-docker">mowgli-docker (Dockerisation complète de Mowgli)</a></li>
    <li>💬 <a href="https://t.me/+x6U3UwU5lB4yOWNk">Groupe Telegram - Entraide 🇫🇷 (Communauté francophone)</a></li>
  </ul>
</div>

---

## 📂 Rappels des Blocs

<div class="alert-red">
  <div class="alert-title">⚠️ Avertissement important</div>
  <p>Modification du matériel à vos risques. Peut annuler la garantie.</p>
</div>

<div class="alert-green">
  <div class="alert-title">💡 Astuce</div>
  <p>Utilisez une adresse IP fixe pour éviter les pertes de connexion.</p>
</div>

<div class="alert-orange">
  <div class="alert-title">📎 Ressources </div>
  <ul>
    <li><a href="#">Lien utile 1</a></li>
    <li><a href="#">Lien utile 2</a></li>
  </ul>
</div>

<div class="alert-blue">
  <div class="alert-title">ℹ️ Information complémentaire</div>
  <p>Ce projet est compatible avec OpenMower pour les carte d'origine yardfoce 500/500B</p>
</div>
