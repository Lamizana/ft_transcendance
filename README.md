<div align="center">

<!-- ▸ Badges cliquables -->
<p align="center">
  <a href="https://www.djangoproject.com/"><img src="https://img.shields.io/badge/Django-4.2-092E20?style=for-the-badge&logo=django&logoColor=white" alt="Django"></a>
  <a href="https://www.python.org/"><img src="https://img.shields.io/badge/Python-3.12-3776AB?style=for-the-badge&logo=python&logoColor=white" alt="Python"></a>
  <a href="https://www.postgresql.org/"><img src="https://img.shields.io/badge/PostgreSQL-15-4169E1?style=for-the-badge&logo=postgresql&logoColor=white" alt="PostgreSQL"></a>
  <a href="https://docs.docker.com/"><img src="https://img.shields.io/badge/Docker-2496ED?style=for-the-badge&logo=docker&logoColor=white" alt="Docker"></a>
  <a href="https://nginx.org/en/docs/"><img src="https://img.shields.io/badge/Nginx-009639?style=for-the-badge&logo=nginx&logoColor=white" alt="Nginx"></a>
  <a href="https://developer.mozilla.org/fr/docs/Web/API/Canvas_API"><img src="https://img.shields.io/badge/HTML5%20Canvas-E34F26?style=for-the-badge&logo=html5&logoColor=white" alt="HTML5 Canvas"></a>
  <a href="https://github.com/Lamizana/ft_transcendance"><img src="https://img.shields.io/badge/Projet-42-000000?style=for-the-badge&logo=42&logoColor=white" alt="Projet 42"></a>
</p>

<h1>Transcendance</h1>

<p><strong>Application web multijoueurs permettant de jouer à Pong en ligne, avec gestion de profils, statistiques, amis et tournois.</strong></p>

<p><em>Projet final de 42</em></p>
<p>Créé en mai 2024 · Dernière mise à jour : 16/10/2026</p>

<a href="https://lamizana.github.io/ZehdBox/">
  <img src="https://img.shields.io/badge/Portfolio-ZehdBox-8A2BE2?style=for-the-badge&logo=githubpages&logoColor=white" alt="Fiche du projet">
</a>

</div>

---

## Sommaire

<details>
<summary>Afficher le sommaire</summary>

- [Sommaire](#sommaire)
- [Aperçu](#aperçu)
- [Fonctionnalités](#fonctionnalités)
- [Stack technique](#stack-technique)
- [Architecture du projet](#architecture-du-projet)
- [Installation et lancement](#installation-et-lancement)
  - [Prérequis](#prérequis)
  - [Lancement](#lancement)
  - [Accès](#accès)
  - [Commandes utiles](#commandes-utiles)
- [Variables d'environnement](#variables-denvironnement)
- [Modes de jeu](#modes-de-jeu)
  - [Solo](#solo)
  - [2 Joueurs (local)](#2-joueurs-local)
  - [4 Joueurs (local)](#4-joueurs-local)
  - [Tournoi](#tournoi)
- [Multi-langue](#multi-langue)
- [Architecture Docker](#architecture-docker)
- [Documentation](#documentation)
  - [Audit du projet](#audit-du-projet)
  - [Suivi d'avancement](#suivi-davancement)
  - [Notes Django](#notes-django)
  - [Notes Web](#notes-web)
- [Équipe](#équipe)
- [Auteur](#auteur)

</details>

---

## Aperçu

**Transcendance** est une application web complète reproduisant le jeu classique **Pong** dans un navigateur. Les joueurs peuvent s'inscrire, créer un profil, affronter des adversaires en ligne ou en local, suivre leurs statistiques, gérer une liste d'amis et participer à des tournois.

Le projet repose sur une architecture **Django + PostgreSQL**, exposée via une **API REST** (Django REST Framework), servie par **Gunicorn** derrière un reverse proxy **Nginx** avec certificat SSL auto-signé. L'ensemble est conteneurisé avec **Docker Compose**.

Le rendu du jeu s'effectue directement sur un **Canvas HTML5** en Vanilla JavaScript, offrant une expérience fluide sans framework frontend lourd.

| Ressource | Lien |
| --- | --- |
| *Fiche du projet (portfolio)* | [ZehdBox](https://lamizana.github.io/ZehdBox/projets/transcendance/){:target="_blank"}|
| *Documentation technique* | [`docs/`](docs/) |

---

## Fonctionnalités

| Fonctionnalité | Description |
| ---------------- | ------------- |
| **Jeu Pong multijoueur** | Partie en temps réel rendue sur Canvas HTML5 avec physique de balle et collisions |
| **4 modes de jeu** | Solo (vs IA), 2 joueurs local, 4 joueurs local, Tournoi à 4 |
| **Comptes utilisateurs** | Inscription, connexion, déconnexion avec authentification Django |
| **Profils joueurs** | Pseudo, photo de profil, statistiques détaillées (taux de victoire, historique des duels) |
| **Système d'amis** | Ajout / suppression d'amis par pseudo, statut online / offline en temps réel |
| **Multi-langue** | Interface disponible en Français, Espagnol et Anglais |
| **Dashboard stats** | Graphique camembert (win rate), historique des duels avec dates, classement tournois |
| **Responsive** | Adaptation écran desktop, tablette et mobile avec boutons tactiles intégrés |
| **API REST** | Endpoints CRUD pour scores, tournois, profils, langue et statut |

---

## Stack technique

| Couche | Technologie | Version |
| -------- | ------------- | --------- |
| **Backend** | Django | `4.2.3` |
| **API REST** | Django REST Framework | `3.15.1` |
| **Langage** | Python | `3.12` |
| **Base de données** | PostgreSQL | `15` |
| **Serveur WSGI** | Gunicorn | `21.2.0` |
| **Reverse Proxy** | Nginx | latest |
| **SSL** | Certificat auto-signé (`TLS 1.2 / 1.3`) | - |
| **Frontend** | HTML5 Canvas, Vanilla JS, Bootstrap 5 | - |
| **Conteneurisation** | Docker + Docker Compose | - |
| **Gestion d'images** | Pillow | `10.3.0` |

---

## Architecture du projet

```
ft_transcendance/
├── apps/
│   ├── pong/                 # Application principale
│   │   ├── models.py         #   Modèle Player (stats, amis, duels)
│   │   ├── views.py          #   Vues + ViewSets REST
│   │   ├── urls.py           #   Routes application + API
│   │   ├── serializers.py    #   Sérialiseurs DRF
│   │   ├── templates/        #   Templates HTML (pong, profil, friends...)
│   │   └── static/           #   JS (jeu, router, SPA) + CSS
│   ├── utilisateurs/         # Authentification
│   │   ├── views.py          #   Inscription, connexion, déconnexion
│   │   ├── urls.py           #   Routes auth
│   │   └── templates/        #   Templates login, register
│   ├── website/              # Configuration Django
│   │   ├── settings.py       #   Paramètres projet
│   │   ├── urls.py           #   Routes racine + DRF router
│   │   └── wsgi.py           #   Point d'entrée WSGI
│   ├── Dockerfile            #   Image Python 3.12 + dépendances
│   ├── entrypoint.sh         #   Script de démarrage (migrations + Gunicorn)
│   └── requirements.txt      #   Dépendances Python
├── nginx/
│   ├── Dockerfile            #   Image Nginx + certs SSL auto-signés
│   └── nginx.conf            #   Configuration reverse proxy
├── docs/                     # Audits, notes et suivi d'avancement
├── docker-compose.yml        # Orchestration des 3 conteneurs
├── Makefile                  # Commandes de gestion du projet
├── .env                      # Variables d'environnement
└── README.md                 # Ce fichier
```

---

## Installation et lancement

### Prérequis

- [Docker](https://docs.docker.com/get-docker/) (version 20.10+)
- [Docker Compose](https://docs.docker.com/compose/install/) (version 2.0+)

### Lancement

```bash
# Cloner le dépôt
git clone https://github.com/Lamizana/ft_transcendance.git
cd ft_transcendance

# Construire et lancer les conteneurs
docker compose up --build
```

### Accès

Ouvrez votre navigateur et acceptez le certificat auto-signé :

```
https://localhost:8080
```

> [!NOTE]
> Le certificat SSL est généré automatiquement lors du build Docker. Votre navigateur affichera un avertissement de sécurité. Cliquez sur **Avancé** puis **Continuer vers le site**.

### Commandes utiles

```bash
# Arrêter les conteneurs sans supprimer les données
docker compose down

# Arrêter les conteneurs ET supprimer les données (reset complet)
docker compose down -v

# Voir les logs en temps réel
docker compose logs -f

# Rebuild après modification du code
docker compose up --build
```

---

## Variables d'environnement

Le fichier `.env` à la racine du projet contient les variables nécessaires au fonctionnement. **Ne jamais commiter ce fichier avec des valeurs réelles.**

| Variable | Description |
|----------|-------------|
| `POSTGRES_USER` | Utilisateur PostgreSQL |
| `POSTGRES_PASSWORD` | Mot de passe PostgreSQL |
| `POSTGRES_DB` | Nom de la base de données |
| `SECRET_KEY` | Clé secrète Django |
| `DEBUG` | Mode debug (0 ou 1) |
| `DJANGO_ALLOWED_HOSTS` | Hosts autorisés par Django |
| `SQL_ENGINE` | Moteur de BDD (`django.db.backends.postgresql`) |
| `SQL_HOST` | Host de la BDD (service Docker `db`) |
| `SQL_PORT` | Port de la BDD (`5432`) |

> [!NOTE]
> **Conseil :** Copiez le fichier `.env` en `.env.example` sans les valeurs sensibles pour documenter les variables nécessaires aux futurs contributeurs.

---

## Modes de jeu

### Solo
Affrontez une **intelligence artificielle** qui prédit la trajectoire de la balle avec un facteur aléatoire.

- Contrôles : `W` / `S` ou `Up` / `Down`.

### 2 Joueurs (local)
Deux joueurs sur le **même clavier**.
- Joueur 1 : `W` / `S`
- Joueur 2 : `Up` / `Down`.
- Premier à 3 points gagne.

### 4 Joueurs (local)
Quatre joueurs sur le **même clavier**, chacun avec une couleur distincte :

| Position | Contrôles | Couleur |
|----------|-----------|---------|
| Gauche | `W` / `S` | Rouge |
| Bas | `A` / `D` | Bleu |
| Droite | `Up` / `Down` | Vert |
| Haut | `Left` / `Right` | Jaune |

### Tournoi

Format bracket à 4 joueurs :
1. **Demi-finale 1**: Joueur 1 vs Joueur 2
2. **Demi-finale 2**: Joueur 3 vs Joueur 4
3. **Finale** : Les deux vainqueurs s'affrontent
4. Classement final (1er, 2e, 3e) sauvegardé dans la base de données.

---

## Multi-langue

L'interface supporte trois langues, sélectionnables depuis la barre de navigation :

- :fr: **Français**
- :es: **Espagnol**
- :gb: **Anglais**

La langue choisie est sauvegardée dans le profil utilisateur et appliquée automatiquement à chaque connexion.

---

## Architecture Docker

Le projet utilise **Docker Compose** pour orchestrer trois conteneurs :

```
┌──────────────────────────────────────────────────────┐
│                     HOST :8080                       │
│                        │                             │
│                  ┌─────▼──────┐                      │
│                  │   NGINX    │  Reverse proxy       │
│                  │  (SSL/TLS) │  + static files      │
│                  └─────┬──────┘                      │
│                        │ proxy_pass                  │
│                  ┌─────▼──────┐                      │
│                  │    WEB     │  Django + Gunicorn   │
│                  │  (Django)  │  Port 8000           │
│                  └─────┬──────┘                      │
│                        │ psycopg2                    │
│                  ┌─────▼──────┐                      │
│                  │     DB     │  PostgreSQL 15       │
│                  │ (Postgres) │  Volume persistant   │
│                  └────────────┘                      │
└──────────────────────────────────────────────────────┘
```

| Conteneur | Image | Port exposé | Rôle |
|-----------|-------|-------------|------|
| `nginx` | Build depuis `nginx/Dockerfile` | `8080` (HTTPS) | Reverse proxy, SSL, fichiers statiques |
| `web` | Build depuis `apps/Dockerfile` | `8000` (interne) | Application Django + Gunicorn |
| `db` | `postgres:15` | `5432` (interne) | Base de données PostgreSQL |

---

## Documentation

La documentation technique du projet est regroupée dans le dossier [`docs/`](docs/).

### Audit du projet

- [`docs/AUDIT.md`](docs/AUDIT.md) :

Audit complet du code source (Django, JavaScript, Docker, Nginx) réalisé le 26 août 2026 : diagnostic général, sécurité (`.env`, `ALLOWED_HOSTS`, API REST, CSRF/CORS), architecture et modèle de données, axes d'amélioration priorisés.

### Suivi d'avancement

- [`docs/AVANCEMENT.md`](docs/AVANCEMENT.md) :
  
Journal d'avancement de l'équipe (Mathys, Nil, Vincent, Alex, mai 2024), organisé par thème : règles de base, Web, gestion utilisateur et authentification, gameplay, IA, cybersécurité, DevOps, graphiques, accessibilité et orienté objet.

### Notes Django

- [`docs/DJANGO.md`](docs/DJANGO.md) :

Carnet d'apprentissage Django + SQL : environnement virtuel, installation, démarrage du projet, image Docker, base PostgreSQL, migrations, application `pong`, vues, modèles et templates.

### Notes Web

- [`docs/WEB.md`](docs/WEB.md):

Carnet d'apprentissage Web : naissance du Web, langages client et serveur, bases de données, réseau (serveurs, adresses IP et noms d'hôtes, protocoles) et métiers du Web.

---

## Équipe

- **Authentification OAuth 2.0** : Integration de l'authentification via l'API de l'ecole 42 pour simplifier l'inscription et la connexion.
- **WebSocket en temps reel** : Remplacer le polling HTTP par Django Channels pour les parties multijoueur en ligne synchronisees.
- **Refactor du modele de donnees** : Normaliser la base en separant les entites : `DuelResult`, `TournamentResult`, `Friendship`, `PlayerProfile`.
- **Tests unitaires et d'integration** : Couverture de code avec `pytest` et `pytest-django`.
- **CI/CD** : Pipeline d'integration continue (GitHub Actions) pour les tests automatiques et le deploiement.
- **Systeme de traduction** : Adopter `gettext` ou une solution JS legere au lieu de tripler le HTML.

---

## Équipe

| Membre | Rôle |
|--------|------|
| **Mathys** | Backend / Architecture |
| **Nil** | Frontend / Jeu |
| **Vincent** | Backend / Base de données |
| **Alex** | Frontend / Design |

---

## Auteur

**Alex Lamizana**, étudiant à 42 Angoulême, spécialisation Data & IA

- [Site](https://lamizana.github.io/ZehdBox/)
- [GitHub](https://github.com/Lamizana)
- [Contact](https://lamizana.github.io/ZehdBox/contact/)
