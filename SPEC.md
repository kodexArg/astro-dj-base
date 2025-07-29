# SPEC

## Architecture
- **2 Docker pods**: backend + frontend
- **Backend**: Django + DRF, API local only
- **Frontend**: Astro SSR
- **Security**: Minimal (API local only)

## Docker
- Single compose file at root
- Separate Dockerfiles per service
- Optimized for dev workflow