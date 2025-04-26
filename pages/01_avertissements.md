---
title: "⚠️ Avertissements"
nav_order: 1
permalink: /01_avertissements/
parent: "🏠 Guide OpenMower"
layout: default
---

# ⚠️ Avertissements ⚠️

<div class="alert-blue">
  <div class="alert-title">ℹ️ Introduction</div>
  Bienvenue dans ce guide détaillé en français pour la configuration et le déploiement de votre robot tondeuse <strong>OpenMower Mowgli</strong> sur les modèles <strong>Yardforce 500 et 500B</strong>. Ce tutoriel s’appuie sur mon expérience personnelle avec la construction du modèle Yardforce 500B.
</div>

---

## 🧩 Compatibilité

Ce guide est spécifiquement conçu pour :

- Yardforce 500 ✅
- Yardforce 500B ✅

Ces modèles doivent être équipés de leur carte mère d’origine **(STM32 F103 ou F402)**.
Il peut aussi être partiellement compatible avec d'autres modèles utilisant la même carte.

---

## 🚨 Fonctionnalités limitées selon le modèle

<div class="alert-orange">
  <div class="alert-title">🛑 Fonctionnalités réduites</div>
  Le clavier et les voyants situés sur le capot du robot <strong>ne seront pas pleinement fonctionnels</strong> avec cette configuration, sauf dans certains cas mis à jour.
</div>

### Yardforce 500 :

- ✅ Voyant de levage
- ✅ Voyant de batterie faible
- ✅ Voyant de charge  
Ces éléments restent fonctionnels **sans modification** du firmware.

### Yardforce 500B :

- 🆕 ✅ **Depuis le firmware Mowgli de Nekraus (25/04/2025)** :
  - Clavier pleinement fonctionnel ✅
  - Voyants pleinement fonctionnels ✅

> Si vous utilisez un firmware autre que celui de Nekraus, les fonctionnalités du panneau peuvent être limitées.

---

## 🖥️ Sérigraphie du panneau Yardforce (nouveauté)

Vous pourrez bientôt retrouver :

- 📷 Un aperçu visuel du panneau et de la disposition des boutons.
- 🖨️ Un modèle imprimable 3D de la sérigraphie pour moderniser votre robot.

[🔗 Aller voir la partie Schéma de câblage]({{ '/pages/schema/' | relative_url }})

---

## ⚠️ Avertissement de non-responsabilité

<div class="alert-red">
  <div class="alert-title">⚠️ Modifications à vos risques</div>
  Toute modification logicielle ou électronique est réalisée <strong>à vos risques et périls</strong>.  
  Cela peut entraîner :
  <ul>
    <li>Une dégradation ou panne matérielle</li>
    <li>L’annulation de la garantie constructeur</li>
    <li>Un comportement imprévu du robot</li>
  </ul>
  Je décline toute responsabilité en cas de dysfonctionnement ou dommage lié à l’application de ce guide.
</div>

---

## 📢 Mise à jour continue

<div class="alert-orange">
  <div class="alert-title">🔁 Guide en constante évolution</div>
  Ce guide est régulièrement mis à jour. Certaines étapes peuvent évoluer ou être améliorées selon les nouvelles versions du firmware ou des composants utilisés.
  <br>
  👉 Pensez à consulter la page <a href="{{ '/mise_a_jour/' | relative_url }}">📝 Mises à jour du guide</a> pour ne rien manquer.
</div>

[⬅ Retour à l'accueil]({{ '/' | relative_url }}) | [📑 Aller au Sommaire]({{ '/pages/sommaire/' | relative_url }})
