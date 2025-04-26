---
title: "❓ FAQ"
nav_order: 8
permalink: /08_faq/
parent: "🏠 Guide OpenMower"
layout: default
---

# ❓ FAQ

Questions fréquentes, bugs rencontrés et solutions proposées.

---

## 📚 Sommaire rapide

- [🔌 Ports USB du Raspberry Pi](#-réinitialiser-les-ports-usb-du-raspberry-pi-sans-redémarrer)
- [🛠️ Problèmes de détection ST-Link sous VSCode](#-vscode-ne-détecte-pas-mon-st-link-que-faire)
- [📈 Déconnexions GPS](#-jai-des-déconnexions-fréquentes-du-gps-que-faire)
- [🗺️ Bugs lors de la fusion des cartes](#-openmower-gui-plante-lorsque-je-fusionne-plusieurs-cartes-de-tonte-pourquoi)
- [🚨 Mise à jour du tableau de bord Yardforce](#-peut-on-mettre-à-jour-ou-modifier-le-firmware-du-tableau-de-bord-panel-yardforce-500500b)
- [⚡ Précautions avant injection de firmware](#-quelle-précaution-prendre-avant-dinjecter-un-firmware-personnalisé)
- [🌱 Configuration correcte du GPS](#-comment-savoir-si-la-configuration-de-mon-gps-est-correcte)
- [🔧 Utilisation d'autres modules GPS](#-puis-je-utiliser-un-autre-module-gps-que-le-f9p-recommandé)
- [⚙️ Problèmes de détection ttyAMA5 ou GPS](#-pourquoi-je-narrive-pas-à-détecter-mon-devttyama5-ou-mon-gps-après-reboot)
- [🧹 Nettoyage du robot](#-comment-nettoyer-mon-robot-après-une-tonte)
- [🚀 Fonctionnement sans GPS](#-peut-on-tondre-sans-gps-rtk-déconnecté)

---

# 🔄 Réinitialiser les ports USB du Raspberry Pi sans redémarrer

<div class="alert-blue">
  <div class="alert-title">ℹ️ Information complémentaire</div>
  <p>Il est possible de réinitialiser les ports USB du Raspberry Pi sans redémarrer tout le système — super utile si un robot ou un périphérique ne répond plus.</p>
</div>

## ✅ Méthode douce : recharger les modules USB (recommandé)

```bash
sudo modprobe -r usbserial cdc_acm
sudo modprobe usbserial cdc_acm
```

Puis :

```bash
ls /dev/ttyA*
dmesg | tail -30
```

## 🧰 Méthode radicale : redémarrage du contrôleur USB

<div class="alert-red">
  <div class="alert-title">⚠️ Attention</div>
  <p>Cette commande va réinitialiser l’ensemble des périphériques USB connectés au Pi (clavier, souris inclus si vous êtes en local).</p>
</div>

```bash
echo '1-1' | sudo tee /sys/bus/usb/drivers/usb/unbind
sleep 2
echo '1-1' | sudo tee /sys/bus/usb/drivers/usb/bind
```

---

# 🛠️ VSCode ne détecte pas mon ST-Link, que faire ?

**Réponse :**
- Redémarrer complètement VSCode.
- Vérifier que les drivers ST-Link sont bien installés (STSW-LINK009).
- Si le problème persiste, utiliser STM32CubeProgrammer pour flasher directement.

---

# 📈 J'ai des déconnexions fréquentes du GPS, que faire ?

**Réponse :**
- Utiliser un câble USB blindé.
- Bien connecter la masse GND entre GPS et Raspberry Pi.
- Vérifier la stabilité de l’alimentation du GPS.

---

# 🗺️ OpenMower GUI plante lorsque je fusionne plusieurs cartes de tonte, pourquoi ?

**Réponse :**
Bug connu (issue #51 sur GitHub).

Solution : supprimer le fichier `map.bag` :

```bash
cd mowgli-docker
sudo rm ./ros/map.bag
```

Puis recréer la carte ou restaurer une sauvegarde.

---

# 🚨 Peut-on mettre à jour ou modifier le firmware du tableau de bord (panel) Yardforce 500/500B ?

<div class="alert-red">
  <div class="alert-title">⚠️ Très important</div>
  <p><strong>Ne jamais modifier ou mettre à jour le firmware du tableau de bord Yardforce 500/500B !</strong><br>Conserver impérativement le firmware d'origine sous peine de rendre votre robot inutilisable.</p>
</div>

---

# ⚡ Quelle précaution prendre avant d'injecter un firmware personnalisé ?

<div class="alert-blue">
  <div class="alert-title">💡 Conseils de sécurité</div>
  <ul>
    <li>Sauvegarder impérativement le firmware d'origine.</li>
    <li>Vérifier toutes les connexions du ST-Link.</li>
    <li>S'assurer de la stabilité de l'alimentation.</li>
  </ul>
</div>

---

# 🌱 Comment savoir si la configuration de mon GPS est correcte ?

**Réponse :**
- Vérifiez que vous avez un "Fix" RTK (RTK Float ou RTK Fix dans OpenMower).
- La précision affichée doit être inférieure à 1 mètre.
- Un mauvais fix = vérifier le branchement, l'alimentation, ou l'antenne.

---

# 🔧 Puis-je utiliser un autre module GPS que le F9P recommandé ?

**Réponse :**
- Oui, mais attention : il doit impérativement supporter le mode RTK (Real-Time Kinematic) pour atteindre la précision requise pour OpenMower.
- Modules testés : Ardusimple F9P, UM982 (avec configuration spécifique).

---

# ⚙️ Pourquoi je n'arrive pas à détecter mon /dev/ttyAMA5 ou mon GPS après reboot ?

**Réponse :**
- Le Raspberry Pi peut parfois changer les affectations de ports.
- Solution : vérifier le branchement UART, l'alimentation du GPS, et configurer correctement `/boot/firmware/config.txt` avec `enable_uart=1` et `dtoverlay=uart5`.

---

# 🧹 Comment nettoyer mon robot après une tonte ?

**Réponse :**
- Utiliser une brosse douce.
- Ne jamais utiliser un jet d'eau direct sur les moteurs ou l'électronique.
- Nettoyer les roues lestables et enlever les résidus sous la coque.

---

# 🚀 Peut-on tondre sans GPS (RTK déconnecté) ?

**Réponse :**
- Non, OpenMower nécessite une position GPS ultra précise pour fonctionner correctement.
- Sans RTK Fix, la trajectoire sera imprécise et dangereuse pour le robot et l'environnement.

---

<div style="display: flex; justify-content: space-between; margin-top: 2rem;">
  <a href="{{ '/07_bonus/' | relative_url }}" class="btn">⬅️ Retour : Bonus 3D & Firmwares</a>
  <a href="{{ '/10_liens_utiles/' | relative_url }}" class="btn">➡️ Suivant : Liens utiles</a>
</div>

---

