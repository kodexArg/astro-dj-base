# Astro + Django REST Framework Base

Astro SSR frontend + Django REST Framework backend in Docker containers.

```bash
docker-compose up --build
```

## Structure
```
astro-dj-base/
├── backend/     # Django + DRF (8000)
├── frontend/    # Astro SSR (4321)
├── scripts/
└── docker-compose.yaml
```

## License
MIT