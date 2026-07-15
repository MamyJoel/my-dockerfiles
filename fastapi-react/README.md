# FastAPI + React (Vite) — Docker Setup

Conteneurisation d'une stack FastAPI (backend) + React/Vite (frontend), sans base de données (stockage en mémoire côté backend).

## Structure

fastapi-react/
├── fastapi-backend/
│   └── Dockerfile
├── react-vite-frontend/
│   └── Dockerfile
└── docker-compose.yml

## Stack

- **Backend** : FastAPI (Python 3.13), servi avec `fastapi run` sur le port 8000
- **Frontend** : React + Vite (build multi-stage), servi par Nginx sur le port 80 (mappé sur 3001 en local)

## Lancer le projet

```bash
docker compose up -d --build
```

- Frontend : http://localhost:3001
- Backend / Swagger docs : http://localhost:8000/docs

## Points d'attention

- Le backend doit écouter sur `0.0.0.0` (pas `127.0.0.1` par défaut) pour être accessible depuis l'extérieur du conteneur : `fastapi run main.py --host 0.0.0.0 --port 8000`
- Le frontend utilise un **multi-stage build** : une étape Node pour builder (`vite build` → dossier `dist/`), puis une image `nginx:alpine` finale qui ne sert que les fichiers statiques (image plus légère)
- Le frontend appelle l'API via `127.0.0.1:8000` (variable `VITE_API_BASE_URL` dans `.env`), car le code s'exécute dans le navigateur, en dehors du réseau Docker interne — pas via le nom du service `backend`
- Vérifier le nom du dossier de sortie du build frontend avant d'écrire le Dockerfile (`dist/` pour Vite, `build/` pour Create React App) — ne pas assumer

## Commandes utiles

```bash
docker compose ps          # état des services
docker compose logs -f     # logs en direct
docker compose down        # arrêt + suppression des conteneurs et du réseau
```
