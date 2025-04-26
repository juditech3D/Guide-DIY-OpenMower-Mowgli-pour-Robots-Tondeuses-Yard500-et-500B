---
title: "🔗Options"
nav_order: 11
permalink: /options.md/
parent: "🏠 Guide OpenMower"
layout: default
---

# 🔗 OPTIONS disponibles

{: .toc }
* TOC
{:toc}

<div class="alert-red">
  <strong style="font-size: 1.2em;">⚠️ ATTENTION : Manipulation Avancée</strong><br>
  Cette page concerne la modification de paramètres critiques du robot. 
  <br><br>
  ❗ Une mauvaise configuration peut entraîner des dysfonctionnements, endommager votre matériel ou compromettre la sécurité.  
  <br><br>
  👉 Ne modifiez ces paramètres que si vous comprenez parfaitement leur impact.
</div>

> ℹ️ Ce document rassemble différentes options avancées que vous pouvez activer manuellement dans le fichier `mowgli-docker/config/om/mower_config.sh`.
>
> ⚠️ **Attention** : Certaines options ont été recueillies d'échanges Telegram. **Elles n'ont pas toutes été testées personnellement**.

---

## 🌧️ Gestion de la pluie

```bash
# Rain mode
#  0 - Ignore (Ignore la pluie)
#  1 - Dock (Retourne au dock dès qu'il pleut)
#  2 - Dock Until Dry (Dock + attente sèche)
#  3 - Pause Automatic Mode (Pause l'automatique jusqu'au redémarrage manuel)
export OM_RAIN_MODE=0

# Délai d'attente après la pluie si mode "Dock Until Dry"
export OM_RAIN_DELAY_MINUTES=30
```

- **1** → Retour au dock dès détection de pluie. Repart quand batterie pleine et plus de pluie.
- **2** → Retour au dock avec un délai d'attente après la pluie.
- **3** → Pause de la tonte, nécessite un redémarrage manuel.

---

## ⚙️ Autres Options Connues (non testées)

| Variable | Description | Exemple |
|:--------|:------------|:--------|
| OM_NO_CUTTING | Si activé (1), la lame ne tourne jamais | `export OM_NO_CUTTING=1` |
| OM_BATTERY_CUT_OFF_VOLTAGE | Tension d'arrêt automatique du robot | `export OM_BATTERY_CUT_OFF_VOLTAGE=21.5` |
| OM_SPEED_NORMAL | Vitesse normale en m/s | `export OM_SPEED_NORMAL=0.5` |
| OM_SPEED_MAPPING | Vitesse pendant l'enregistrement de carte | `export OM_SPEED_MAPPING=0.4` |
| OM_SPEED_DOCKING | Vitesse pendant l'approche du dock | `export OM_SPEED_DOCKING=0.3` |
| OM_SPEED_MOWING | Vitesse pendant la tonte | `export OM_SPEED_MOWING=0.5` |
| OM_TRACTION_CONTROL | Active/désactive la gestion automatique du glissement | `export OM_TRACTION_CONTROL=1` |

---

## 🔋 Paramètres de Charge & Batterie

<div class="alert-red">
  <div class="alert-title">⚠️ Avertissement important</div>
  <p>Modifier ces paramètres peut impacter la durée de vie de la batterie ou entraîner des risques d'endommagement. À utiliser avec <strong>extrême prudence</strong>.</p>
</div>

```bash
export MAX_CHARGE_CURRENT=1.0
export LIMIT_VOLTAGE_150MA=29.05
export MAX_CHARGE_VOLTAGE=29.2
export BAT_CHARGE_CUTOFF_VOLTAGE=29.15
export CHARGE_END_LIMIT_CURRENT=0.08
export MIN_CHARGE_CURRENT=0.1
```

---

## 🛞 Paramètres Cinématiques (mouvement du robot)

```bash
export WHEEL_BASE=0.325
export MAX_MPS=0.5        # Mètres par seconde max
export PWM_PER_MPS=300.0  # PWM par mètre/seconde
export TICKS_PER_M=300.0  # Tick par mètre
```

---

## 🚨 Sécurité et Timeout

<div class="alert-red">
  <div class="alert-title">⚠️ Avertissement extrême</div>
  <p>Ces réglages concernent la sécurité du robot (arrêt d'urgence, détection de basculement, etc.). Une mauvaise configuration peut entraîner des situations dangereuses. À utiliser avec <strong>extrême prudence</strong>.</p>
</div>

```bash
export I_DONT_NEED_MY_FINGERS=0 # Si activé, désactive la sécurité du moteur (DANGER !)
export IMU_ONBOARD_INCLINATION_THRESHOLD=56  # Seuil d'inclinaison
export ONE_WHEEL_LIFT_EMERGENCY_MILLIS=10000  # Temps levée 1 roue avant alarme
export BOTH_WHEELS_LIFT_EMERGENCY_MILLIS=1000 # Temps levée 2 roues avant alarme
export TILT_EMERGENCY_MILLIS=500
export STOP_BUTTON_EMERGENCY_MILLIS=100
export PLAY_BUTTON_CLEAR_EMERGENCY_MILLIS=2000
```

---

## 🔧 Options additionnelles

```bash
export EMERGENCY_DEBUG=0  # Active le mode debug d'urgence
export OPTION_BUMPER=0    # Activation du pare-choc si capteur installé
export OPTION_ULTRASONIC=0 # Activation de capteurs ultrasoniques si présents
```

---

## 💡 Exemples pratiques

### ➡️ Exemple 1 : Forcer la tondeuse à se docker à la moindre pluie

```bash
export OM_RAIN_MODE=1
export OM_RAIN_DELAY_MINUTES=0
```
Le robot retourne immédiatement au dock si la pluie est détectée.

---

### ➡️ Exemple 2 : Réduire la vitesse pendant l’enregistrement de la map

```bash
export OM_SPEED_MAPPING=0.3
```
Cela permet un mapping plus précis en roulant lentement.

---

### ➡️ Exemple 3 : Désactiver la lame pendant les tests

```bash
export OM_NO_CUTTING=1
```
Pratique pour valider la navigation sans démarrer la lame motorisée.

---

📢 **Pensez à sauvegarder votre fichier `mower_config.sh` après toute modification pour éviter toute perte !** 🚀

👉 Ces options avancées permettent de mieux personnaliser votre robot, **à utiliser avec extrême prudence** selon votre matériel ! 🚜

N'hésitez pas à revenir consulter cette page, elle sera mise à jour si d'autres options sont découvertes !
