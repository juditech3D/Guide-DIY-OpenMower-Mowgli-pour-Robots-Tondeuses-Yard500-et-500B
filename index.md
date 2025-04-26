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
  <div class="alert-title">⚠️ Important</div>
  <p>Ce guide implique des modifications électroniques et logicielles. Lisez attentivement les avertissements pour éviter toute erreur ou dommage matériel.</p>
</div>

---

<div class="alert-blue">
  <div class="alert-title">ℹ️ Ce projet est une adaptation</div>
  <p>
    Ce guide est basé sur une <strong>adaptation du projet OpenMower officiel</strong> qui repose sur une carte mère dédiée (<em>OpenMower V1</em>) et un système OpenMower OS.<br><br>
    ⚠️ <strong>Ce tutoriel n’est pas compatible avec OpenMower OS.</strong><br><br>
    Le projet ici est conçu pour les modèles <strong>Yardforce 500 / 500B</strong> ou série SA/similaire en utilisant leur <strong>carte mère d’origine</strong>.<br>
    🔗 Consultez le site officiel : <a href="https://openmower.de" target="_blank">openmower.de</a>
  </p>
</div>

---

<div class="alert-green">
  <div class="alert-title">🙏 Remerciements</div>
  <p>Un grand merci à tous les membres du <strong>groupe Telegram OpenMower France</strong> pour leur aide précieuse et indirecte qui m'a permis de compléter et améliorer ce guide ! 🚀</p>
</div>

---

## 📂 Sections disponibles

- [⚠️ Avertissements]({{ '/01_avertissements/' | relative_url }})
- [🧰 Matériel requis]({{ '/02_materiel_requis/' | relative_url }})
- [💾 Installation & préparation]({{ '/03_installation_preparation/' | relative_url }})
- [⚙️ Configuration]({{ '/04_configuration/' | relative_url }})
- [🔌 Interfaces Série et UART5 (optionnel)]({{ '/04_uart5_configuration/' | relative_url }})
- [📥 Injection du firmware]({{ '/05_injection_firmware/' | relative_url }})
- [📱 Configuration OpenMower]({{ '/06_configuration_openmower/' | relative_url }})
- [🎁 Bonus : pièces 3D & firmwares]({{ '/07_bonus/' | relative_url }})
- [❓ FAQ]({{ '/08_faq/' | relative_url }})
- [💖 Soutenir]({{ '/09_soutenir/' | relative_url }})
- [🔗 Liens utiles]({{ '/10_liens_utiles/' | relative_url }})
- [🛠️ Mises à jour du guide]({{ '/mise_a_jour/' | relative_url }})

---

<div class="cards-grid">
  <a href="{{ '/01_avertissements/' | relative_url }}" class="card">⚠️ Avertissements</a>
  <a href="{{ '/02_materiel_requis/' | relative_url }}" class="card">🧰 Matériel requis</a>
  <a href="{{ '/03_installation_preparation/' | relative_url }}" class="card">💾 Installation & préparation</a>
  <a href="{{ '/04_configuration/' | relative_url }}" class="card">⚙️ Configuration</a>
  <a href="{{ '/04_uart5_configuration/' | relative_url }}" class="card">🔌 Interfaces Série UART5</a>
  <a href="{{ '/05_injection_firmware/' | relative_url }}" class="card">📥 Injection du firmware</a>
  <a href="{{ '/06_configuration_openmower/' | relative_url }}" class="card">📱 Configuration OpenMower</a>
  <a href="{{ '/07_bonus/' | relative_url }}" class="card">🎁 Pièces 3D & firmwares</a>
  <a href="{{ '/08_faq/' | relative_url }}" class="card">❓ FAQ</a>
  <a href="{{ '/09_soutenir/' | relative_url }}" class="card">💖 Soutenir</a>
  <a href="{{ '/10_liens_utiles/' | relative_url }}" class="card">🔗 Liens utiles</a>
  <a href="{{ '/mise_a_jour/' | relative_url }}" class="card">🛠️ Mises à jour</a>
</div>

<style>
.cards-grid {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(200px, 1fr));
  gap: 1rem;
  margin-top: 2rem;
}

.card {
  display: flex;
  align-items: center;
  justify-content: center;
  padding: 1rem;
  background: #f9f9f9;
  border-radius: 12px;
  text-align: center;
  font-size: 1.1rem;
  font-weight: bold;
  color: #333;
  text-decoration: none;
  box-shadow: 0 2px 6px rgba(0, 0, 0, 0.1);
  transition: transform 0.3s ease, background 0.3s ease;
}

.card:hover {
  background: #e6f0ff;
  transform: scale(1.03);
}

.btn {
  display: inline-block;
  padding: 0.7rem 1.5rem;
  background: #007BFF;
  color: white;
  font-weight: bold;
  text-decoration: none;
  border-radius: 8px;
  transition: background 0.3s ease;
}

.btn:hover {
  background: #0056b3;
}
</style>

---

<div class="alert-orange">
  <div class="alert-title">📎 Ressources utiles pour réussir votre projet</div>
  <ul>
    <li>🌐 <a href="https://openmower.de/docs/" target="_blank">OpenMower Documentation officielle</a></li>
    <li>🧠 <a href="https://github.com/cedbossneo/Mowgli" target="_blank">Fork Mowgli - CedBossNeo</a></li>
    <li>🐳 <a href="https://github.com/cedbossneo/mowgli-docker" target="_blank">mowgli-docker - Dockerisation Mowgli</a></li>
    <li>💬 <a href="https://t.me/+x6U3UwU5lB4yOWNk" target="_blank">Groupe Telegram d'entraide 🇫🇷</a></li>
  </ul>
</div>

---

<div style="display: flex; justify-content: space-between; margin-top: 2rem;">
  <a href="{{ '/pages/sommaire/' | relative_url }}" class="btn">⬅ Sommaire</a>
  <a href="{{ '/01_avertissements/' | relative_url }}" class="btn">➡ Commencer : Avertissements</a>
</div>

<script>
  var beamer_config = {
    product_id : "YUkxsqqe75314"
  };
</script>
<script type="text/javascript" src="https://app.getbeamer.com/js/beamer-embed.js" defer="defer"></script>
