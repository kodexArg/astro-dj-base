# SPECS

## Project Tree
```
astro-dj-base/
├── backend/           # Django + DRF
├── frontend/          # Astro SSR
├── scripts/           # Aux files
├── docker-compose.yaml
├── backend/Dockerfile
└── frontend/Dockerfile
```

## Architecture
- **2 Docker pods**: backend + frontend
- **Backend**: Django + DRF, API only (local pod)
- **Frontend**: Astro SSR
- **Security**: Minimal (API local only)

## Docker Setup
- Single `docker-compose.yaml` at root
- Separate Dockerfiles per service
- Optimized for dev workflow