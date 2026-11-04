# TaskFlow - Application de gestion de projet

Application web de gestion de projet permettant de créer des projets, d'organiser des tâches sur un tableau Kanban, d'assigner des membres et de suivre l'avancement.

Frontend : React. Backend : Node.js / Express. Base de données : MongoDB (à adapter : PostgreSQL, MySQL...).

---

## Sommaire

- [Fonctionnalités](#fonctionnalités)
- [Stack technique](#stack-technique)
- [Architecture du projet](#architecture-du-projet)
- [Prérequis](#prérequis)
- [Installation](#installation)
- [Configuration](#configuration)
- [Lancer l'application](#lancer-lapplication)
- [API REST](#api-rest)
- [Scripts disponibles](#scripts-disponibles)
- [Tests](#tests)
- [Déploiement](#déploiement)
- [Contribuer](#contribuer)
- [Licence](#licence)

---

## Fonctionnalités

- Authentification (inscription, connexion, JWT)
- Gestion de projets (création, modification, archivage, suppression)
- Gestion de tâches (titre, description, priorité, échéance, statut)
- Tableau Kanban avec glisser-déposer (A faire / En cours / Terminé)
- Gestion d'équipe : inviter des membres et assigner des tâches
- Commentaires sur les tâches
- Recherche et filtres (statut, priorité, assigné)
- Tableau de bord avec statistiques d'avancement
- Interface responsive

## Stack technique

| Couche | Technologies |
|--------|--------------|
| Frontend | React, React Router, Axios, Context API / Redux Toolkit, CSS Modules / Tailwind |
| Backend | Node.js, Express, JWT, bcrypt, express-validator, CORS, dotenv |
| Base de données | MongoDB + Mongoose (ou PostgreSQL + Prisma/Sequelize) |
| Outils | ESLint, Prettier, Jest, Supertest, Nodemon |

## Architecture du projet

````
taskflow/
├── client/                  # Application React
│   ├── public/
│   └── src/
│       ├── components/      # Composants réutilisables
│       ├── pages/           # Pages (Login, Dashboard, ProjectBoard...)
│       ├── hooks/           # Hooks personnalisés
│       ├── context/         # Contextes (Auth, Projets...)
│       ├── services/        # Appels API (Axios)
│       ├── utils/
│       ├── App.jsx
│       └── main.jsx
│
├── server/                  # API Express
│   ├── src/
│   │   ├── config/          # Connexion BDD, variables d'environnement
│   │   ├── controllers/     # Logique des routes
│   │   ├── middlewares/     # Auth, gestion d'erreurs, validation
│   │   ├── models/          # Modèles de données
│   │   ├── routes/          # Définition des routes
│   │   ├── utils/
│   │   ├── app.js
│   │   └── server.js
│   └── tests/
│
├── .gitignore
├── package.json             # Scripts racine (optionnel)
└── README.md
````

## Prérequis

- Node.js v18 ou supérieur
- npm ou yarn
- MongoDB (local ou Atlas)
- Git

## Installation

````bash
# 1. Cloner le dépôt
git clone https://github.com/votre-utilisateur/taskflow.git
cd taskflow

# 2. Installer les dépendances du backend
cd server
npm install

# 3. Installer les dépendances du frontend
cd ../client
npm install
````

## Configuration

### Backend : `server/.env`

Créer un fichier `.env` à partir de l'exemple :

````bash
cp server/.env.example server/.env
````

````env
# Serveur
PORT=5000
NODE_ENV=development

# Base de données
MONGODB_URI=mongodb://localhost:27017/taskflow

# Authentification
JWT_SECRET=une_cle_secrete_longue_et_aleatoire
JWT_EXPIRES_IN=7d

# CORS
CLIENT_URL=http://localhost:5173
````

### Frontend : `client/.env`

````bash
cp client/.env.example client/.env
````

````env
VITE_API_URL=http://localhost:5000/api
````

Si vous utilisez Create React App, remplacer `VITE_API_URL` par `REACT_APP_API_URL`.

Ne jamais commiter les fichiers `.env`. Seuls les fichiers `.env.example` doivent être versionnés.

## Lancer l'application

Ouvrir deux terminaux :

````bash
# Terminal 1 - Backend (http://localhost:5000)
cd server
npm run dev
````

````bash
# Terminal 2 - Frontend (http://localhost:5173)
cd client
npm run dev
````

L'application est ensuite accessible sur http://localhost:5173.

## API REST

Base URL : `http://localhost:5000/api`

Les routes protégées nécessitent l'en-tête `Authorization: Bearer <token>`.

### Authentification

| Méthode | Route | Description | Auth |
|---------|-------|-------------|------|
| POST | `/auth/register` | Créer un compte | Non |
| POST | `/auth/login` | Se connecter | Non |
| GET | `/auth/me` | Profil de l'utilisateur connecté | Oui |

### Projets

| Méthode | Route | Description | Auth |
|---------|-------|-------------|------|
| GET | `/projects` | Lister mes projets | Oui |
| POST | `/projects` | Créer un projet | Oui |
| GET | `/projects/:id` | Détail d'un projet | Oui |
| PUT | `/projects/:id` | Modifier un projet | Oui |
| DELETE | `/projects/:id` | Supprimer un projet | Oui |
| POST | `/projects/:id/members` | Ajouter un membre | Oui |

### Tâches

| Méthode | Route | Description | Auth |
|---------|-------|-------------|------|
| GET | `/projects/:id/tasks` | Lister les tâches d'un projet | Oui |
| POST | `/projects/:id/tasks` | Créer une tâche | Oui |
| PUT | `/tasks/:taskId` | Modifier une tâche | Oui |
| PATCH | `/tasks/:taskId/status` | Changer le statut | Oui |
| DELETE | `/tasks/:taskId` | Supprimer une tâche | Oui |
| POST | `/tasks/:taskId/comments` | Ajouter un commentaire | Oui |

### Exemple de requête

````bash
curl -X POST http://localhost:5000/api/auth/login \
  -H "Content-Type: application/json" \
  -d '{"email": "alice@example.com", "password": "motdepasse123"}'
````

Réponse :

````json
{
  "token": "eyJhbGciOiJIUzI1NiIsInR5cCI6...",
  "user": {
    "id": "64f1a2b3c4d5e6f7a8b9c0d1",
    "name": "Alice",
    "email": "alice@example.com"
  }
}
````

## Scripts disponibles

### Backend (`/server`)

| Commande | Description |
|----------|-------------|
| `npm run dev` | Lance le serveur avec Nodemon (rechargement auto) |
| `npm start` | Lance le serveur en production |
| `npm test` | Exécute les tests |
| `npm run lint` | Vérifie le code avec ESLint |

### Frontend (`/client`)

| Commande | Description |
|----------|-------------|
| `npm run dev` | Lance le serveur de développement |
| `npm run build` | Génère le build de production |
| `npm run preview` | Prévisualise le build |
| `npm test` | Exécute les tests |
| `npm run lint` | Vérifie le code avec ESLint |

## Tests

````bash
# Backend (Jest + Supertest)
cd server && npm test

# Frontend (Vitest / Jest + React Testing Library)
cd client && npm test
````

## Déploiement

- Frontend : Vercel, Netlify ou GitHub Pages
  ````bash
  cd client && npm run build   # dossier généré : dist/
  ````
- Backend : Render, Railway, Fly.io ou un VPS
- Base de données : MongoDB Atlas

Définir les variables d'environnement sur la plateforme choisie et mettre à jour `CLIENT_URL` (CORS) et `VITE_API_URL`.

## Contribuer

1. Forker le projet
2. Créer une branche : `git checkout -b feature/ma-fonctionnalite`
3. Commiter les changements : `git commit -m "feat: ajoute ma fonctionnalité"`
4. Pousser la branche : `git push origin feature/ma-fonctionnalite`
5. Ouvrir une Pull Request

Convention de commits : Conventional Commits.

## Roadmap

- [ ] Notifications en temps réel (Socket.io)
- [ ] Pièces jointes sur les tâches
- [ ] Diagramme de Gantt
- [ ] Export PDF / CSV
- [ ] Mode sombre

## Licence

Distribué sous licence MIT. Voir le fichier LICENSE pour plus d'informations.

## Auteur

Votre Nom - https://github.com/votre-utilisateur
````
````
