# 📊 Dashboard d'analyse — data.gouv.fr

> Dashboard interactif d'analyse des résultats du [méta-moteur data.gouv.fr](https://github.com/gunout/moteur-api).

[![Python](https://img.shields.io/badge/Python-3.12+-3776AB?style=flat-square&logo=python&logoColor=white)](https://www.python.org/)
[![FastAPI](https://img.shields.io/badge/FastAPI-0.141-009688?style=flat-square&logo=fastapi&logoColor=white)](https://fastapi.tiangolo.com/)
[![DSFR](https://img.shields.io/badge/DSFR-1.11-000091?style=flat-square)](https://www.systeme-de-design.gouv.fr/)
[![Licence](https://img.shields.io/badge/Licence-etalab--2.0-blue?style=flat-square)](https://www.etalab.gouv.fr/licence-ouverte-open-licence/)
[![Statut](https://img.shields.io/badge/Statut-actif-success?style=flat-square)]()
[![Dernier commit](https://img.shields.io/github/last-commit/gunout/dashboard-analyse-data-gouv-fr?style=flat-square&color=000091)](https://github.com/gunout/dashboard-analyse-data-gouv-fr/commits/main)
[![Issues](https://img.shields.io/github/issues/gunout/dashboard-analyse-data-gouv-fr?style=flat-square&color=E1000F)](https://github.com/gunout/dashboard-analyse-data-gouv-fr/issues)
[![Stars](https://img.shields.io/github/stars/gunout/dashboard-analyse-data-gouv-fr?style=flat-square&color=f1c40f)](https://github.com/gunout/dashboard-analyse-data-gouv-fr/stargazers)
[![Taille](https://img.shields.io/github/repo-size/gunout/dashboard-analyse-data-gouv-fr?style=flat-square&color=1212ff)](https://github.com/gunout/dashboard-analyse-data-gouv-fr)

---

## 📖 Présentation

**Dashboard d'analyse data.gouv.fr** est une application web qui exploite les résultats du [méta-moteur data.gouv.fr](https://github.com/gunout/moteur-api) pour produire une **analyse visuelle complète** de n'importe quel thème.

Le dashboard interroge le backend du méta-moteur, agrège les résultats, puis les analyse selon des critères universels :

- **KPI dynamiques** : résultats, APIs, datasets, popularité, qualité, doublons
- **Graphiques interactifs** : donuts, bar charts, timeline, nuage de tags
- **Filtres cliquables** : type, source, organisation, année, tag
- **Détection de doublons** entre les sources v1 et v2
- **Score de qualité** /100 basé sur les métriques v2
- **Export PNG** de chaque section

L'interface suit la **charte Marianne** (DSFR) avec bandeau tricolore et bloc-marque officiel.

---

## 📸 Captures d'écran

### Vue d'ensemble du dashboard

<img width="1644" height="2636" alt="Screenshot 2026-09-28 at 19-40-41 📊 Dashboard — Analyse des résultats data gouv fr" src="https://github.com/user-attachments/assets/98beaa82-b189-4977-8de5-70a3ec666cee" />


*Analyse d'une requête avec KPI, graphiques et filtres interactifs.*

### Détail des graphiques

<img width="1644" height="2636" alt="Screenshot 2026-09-28 at 19-47-42 📊 Dashboard — Analyse des résultats data gouv fr" src="https://github.com/user-attachments/assets/9293a4cc-985a-4680-81c9-533bb4cb68a5" />


*Donuts interactifs, bar charts cliquables et nuage de tags.*

---

## ✨ Fonctionnalités

- 📊 **KPI en temps réel** : 8 indicateurs clés (résultats, APIs, datasets, popularité, qualité, doublons, erreurs)
- 🥧 **Donuts interactifs** : répartition par type et par source
- 📈 **Bar charts cliquables** : top 10 organisations et top 10 tags
- 📅 **Timeline** : distribution des mises à jour par année
- 🏷️ **Nuage de tags** : top 30 tags avec taille proportionnelle
- 🏆 **Top 10 popularité** : résultats les plus consultés
- 🎯 **Top 10 qualité** : meilleurs scores /100
- 🔁 **Détection de doublons** : identifie les datasets présents dans plusieurs sources
- 🎛️ **Filtres combinables** : type, source, organisation, année, tag
- 📷 **Export PNG** : chaque section peut être exportée en image
- 🎨 **Design Marianne** (DSFR) avec bandeau tricolore
- 📱 **Responsive** : s'adapte mobile, tablette, desktop

---

## 🏗️ Architecture

```
dashboard-analyse-data-gouv-fr/
├── main.py              # Backend FastAPI (proxy + analyse)
├── index.html           # Interface du dashboard (design Marianne)
├── renov.json           # Exemple de résultats (requête "france renov'")
├── LICENSE              # Licence etalab-2.0
└── README.md
```

### Flux de données

```
┌──────────────┐
│  Dashboard   │  (index.html · DSFR)
│  Navigateur  │
└──────┬───────┘
       │
       │ 1. Recherche multi-pages
       │    GET /search?q=...&page=1..10
       │
       ▼
┌──────────────┐
│ Méta-moteur  │  (port 8001)
│  Backend     │
└──────┬───────┘
       │
       │ 2. Interroge en parallèle
       ├──────────────► API Catalogue v1
       ├──────────────► API Catalogue v2
       └──────────────► API Dataservices
       │
       ▼
┌──────────────┐
│  Dashboard   │  3. Agrégation + Analyse
│  Rendu       │     - Dédoublonnage
│              │     - Calcul de popularité
│              │     - Score de qualité /100
│              │     - Détection de doublons
└──────────────┘
```

---

## 🚀 Installation

### Prérequis

- **Python 3.12+**
- **pip** et **venv**
- Le **méta-moteur data.gouv.fr** doit tourner sur `http://localhost:8001`

### Étapes

```bash
# 1. Cloner le dépôt
git clone https://github.com/gunout/dashboard-analyse-data-gouv-fr.git
cd dashboard-analyse-data-gouv-fr

# 2. Créer et activer un environnement virtuel
python3 -m venv venv
source venv/bin/activate

# 3. Installer les dépendances
pip install fastapi uvicorn httpx

# 4. Lancer le serveur
uvicorn main:app --reload --port 8002
```

Le dashboard démarre sur **`http://127.0.0.1:8002`**.

> ⚠️ **Important** : le dashboard se connecte par défaut au méta-moteur sur `http://localhost:8001`. Assurez-vous que ce dernier tourne bien avant de lancer le dashboard.

---

## 🖥️ Utilisation

### Interface web

Ouvrez dans votre navigateur :

```
http://127.0.0.1:8002/
```

Ou, si le montage `StaticFiles` n'est pas activé, ouvrez directement `index.html`.

### Configuration du backend cible

Si votre méta-moteur tourne sur un autre port, modifiez la constante dans la console du navigateur :

```javascript
localStorage.setItem('meta_api_base', 'http://localhost:8003');
location.reload();
```

Ou modifiez la constante dans `index.html` :

```javascript
const API_BASE = localStorage.getItem('meta_api_base') || 'http://localhost:8001';
```

---

## 🎛️ Sections du dashboard

| Section | Contenu |
| :--- | :--- |
| **KPI Cards** | 8 indicateurs clés (requête, résultats, APIs, datasets, popularité, qualité, erreurs, doublons) |
| **Donut type** | Répartition datasets vs APIs |
| **Donut source** | Répartition v1, v2, dataservices |
| **Top organisations** | Bar chart des 10 producteurs les plus actifs |
| **Top tags** | Bar chart des 10 tags les plus fréquents |
| **Timeline** | Distribution par année de mise à jour |
| **Nuage de tags** | Top 30 tags avec taille proportionnelle |
| **Top popularité** | Classement des résultats les plus consultés |
| **Top qualité** | Classement par score /100 |
| **Doublons** | Groupes de datasets présents dans plusieurs sources |
| **Erreurs** | Liste des erreurs sources (si présentes) |

---

## 🎯 Filtres interactifs

**Tous les graphiques sont cliquables** :

| Clic sur... | Filtre appliqué |
|---|---|
| Une barre du bar chart organisations | `org` |
| Une barre du bar chart tags | `tag` |
| Une légende de donut type | `type` |
| Une légende de donut source | `source` |
| Une ligne de la timeline | `year` |
| Une bulle du nuage de tags | `tag` |

**Comportement** :
- Les filtres se **combinent** (AND logique)
- Recliquer sur le **même filtre** le retire (toggle)
- Une **barre de filtres actifs** apparaît en haut avec des chips retirables
- Un bouton **« Tout effacer »** réinitialise tout
- Les graphiques se recalculent **instantanément** sur les données filtrées

---

## 📊 Score de qualité /100

Le score composite est calculé à partir des métriques v2 :

| Métrique | Poids max | Méthode |
|---|---|---|
| **Views** | 40 pts | Échelle logarithmique |
| **Downloads** | 25 pts | Échelle logarithmique |
| **Reuses** | 20 pts | Échelle logarithmique |
| **Followers** | 10 pts | Échelle logarithmique |
| **Discussions** | 5 pts | Indicateur d'engagement |

**Interprétation** :
- **70-100** : `excellent` (vert)
- **40-69** : `moyen` (jaune)
- **0-39** : `faible` (gris)

---

## 🔁 Détection de doublons

Le dashboard normalise les titres (lowercase, sans accents, sans ponctuation) et détecte les datasets présents dans **plusieurs sources** (v1 et v2). Ces doublons sont :

- Comptés dans un KPI dédié
- Listés dans une section spécifique
- Marqués d'un badge violet dans le Top popularité

---

## 📷 Export PNG

Chaque section dispose d'un bouton **📷 PNG** qui capture uniquement cette section (via `html2canvas`) et la télécharge en image haute résolution (×2).

Le nom du fichier contient la requête + le nom de la section : `france_renov_type.png`.

---

## 🛠️ Stack technique

| Composant | Technologie |
| :--- | :--- |
| **Backend** | Python 3.12, FastAPI |
| **Frontend** | HTML5, CSS3, JavaScript vanilla |
| **Graphiques** | CSS pur (conic-gradient, barres) + html2canvas pour l'export |
| **Design** | Système de Design de l'État (DSFR 1.11) |
| **API sources** | Méta-moteur data.gouv.fr (v1, v2, dataservices) |

---

## 📁 Structure du projet

```
.
├── main.py              # Serveur FastAPI du dashboard
├── index.html           # Interface d'analyse
├── renov.json           # Exemple de résultats (requête "france renov'")
├── LICENSE              # Licence etalab-2.0
├── README.md            # Ce fichier
└── venv/                # Environnement virtuel (non versionné)
```

---

## 🔗 Dépôts liés

Ce dashboard s'appuie sur les projets suivants :

- **[gunout/moteur-api](https://github.com/gunout/moteur-api)** — Méta-moteur data.gouv.fr (backend principal)
- **[gunout/monitor-moteur-api](https://github.com/gunout/monitor-moteur-api)** — Monitor de surveillance

---

## 🤝 Contribution

Les contributions sont bienvenues. Pour proposer une amélioration :

1. Forkez le projet
2. Créez une branche (`git checkout -b feature/amelioration`)
3. Committez vos changements (`git commit -m 'Ajout de…'`)
4. Poussez la branche (`git push origin feature/amelioration`)
5. Ouvrez une Pull Request

---

## 📜 Licence

Ce projet est distribué sous licence **etalab-2.0**, conformément à la politique d'ouverture des données publiques françaises.

Les données interrogées restent la propriété de leurs producteurs respectifs.

---

## 🔗 Ressources

- [data.gouv.fr](https://www.data.gouv.fr)
- [API data.gouv.fr — documentation](https://doc.data.gouv.fr/api/intro/)
- [Système de Design de l'État (DSFR)](https://www.systeme-de-design.gouv.fr/)
- [Licence etalab-2.0](https://www.etalab.gouv.fr/licence-ouverte-open-licence/)

---

<p align="center">
  <strong>République Française</strong><br>
  <em>Liberté · Égalité · Fraternité</em>
</p>

---

<div align="center">

### 🇫🇷 Gunout · 2026

![Made in France](https://img.shields.io/badge/Made_in-France-002395?style=flat-square&labelColor=FFFFFF&logo=data:image/svg+xml;base64,PHN2ZyB4bWxucz0iaHR0cDovL3d3dy53My5vcmcvMjAwMC9zdmciIHZpZXdCb3g9IjAgMCA5MDAgNjAwIj48cmVjdCB3aWR0aD0iOTAwIiBoZWlnaHQ9IjYwMCIgZmlsbD0iIzAwMjM5NSIvPjxyZWN0IHdpZHRoPSI5MDAiIGhlaWdodD0iNDAwIiB5PSIxMDAiIGZpbGw9IiNmZmYiLz48cmVjdCB3aWR0aD0iOTAwIiBoZWlnaHQ9IjIwMCIgeT0iNDAwIiBmaWxsPSIjZWQyOTM5Ii8+PC9zdmc+)
![GitHub](https://img.shields.io/badge/GitHub-gunout-181717?style=flat-square&logo=github&logoColor=white)
![Year](https://img.shields.io/badge/2026-ED2939?style=flat-square&labelColor=FFFFFF)

<sub>© 2026 <strong>Gunout</strong> — Tous droits réservés.</sub>

</div>
