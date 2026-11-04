# TaskFlow

Application de gestion de projet : création de projets, tableau Kanban, assignation de tâches et suivi de l'avancement.

Frontend : React. Backend : Node.js / Express. Base de données : MongoDB.

## Fonctionnalités

- Authentification (JWT)
- Gestion de projets et de tâches
- Tableau Kanban (A faire / En cours / Terminé)
- Gestion d'équipe et assignation de tâches

## Structure

```
taskflow/
├── client/    # Application React
└── server/    # API Express
```

## Prérequis

- Node.js v18 ou supérieur
- MongoDB (local ou Atlas)

## Installation

```bash
git clone https://github.com/votre-utilisateur/taskflow.git
cd taskflow

cd server && npm install
cd ../client && npm install
```

## Configuration

Créer `server/.env` :

```env
PORT=5000
MONGODB_URI=mongodb://localhost:27017/taskflow
JWT_SECRET=une_cle_secrete
CLIENT_URL=http://localhost:5173
```

Créer `client/.env` :

```env
VITE_API_URL=http://localhost:5000/api
```

## Lancer l'application

```bash
# Backend
cd server && npm run dev

# Frontend (dans un autre terminal)
cd client && npm run dev
```

Le frontend est accessible sur http://localhost:5173 et l'API sur http://localhost:5000.

## API

| Méthode | Route | Description |
|---------|-------|-------------|
| POST | `/api/auth/register` | Créer un compte |
| POST | `/api/auth/login` | Se connecter |
| GET | `/api/projects` | Lister les projets |
| POST | `/api/projects` | Créer un projet |
| GET | `/api/projects/:id/tasks` | Lister les tâches |
| POST | `/api/projects/:id/tasks` | Créer une tâche |
| PUT | `/api/tasks/:id` | Modifier une tâche |
| DELETE | `/api/tasks/:id` | Supprimer une tâche |

Les routes protégées nécessitent l'en-tête `Authorization: Bearer <token>`.

## Licence

MIT
