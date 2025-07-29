# SPEC

**MANDATORY INSTRUCTIONS - FOLLOW STRICTLY**


## Architecture
- **Backend**: Django + DRF, API local only
- **Frontend**: Astro SSR
- **Scripts**: Database initialization + development execution
- 2 Docker pods: backend & frontend
  - Single `docker-compose.yaml` file at root
  - Separate Dockerfiles per service in `frontend/Dockerfile` and `backend/Dockerfile`
- Security: API local only. 


## Prohibitions
- NEVER execute with `uv`
- NEVER execute `git`
- NEVER execute migrations
- NEVER write comments in code
- NEVER write docstrings in code (but leave the docstring you find)
- ALWAYS remove all comments found in existing code
- Do not expose API to public network.


## Response Format
- Always respond in Spanish (castellano)
- All code elements (variable names, file names, etc.) must be in strict English
- Avoid writing code snippets in responses, prefer links to files, classes and functions
- Decorate variables with backticks like `variable`


## Backend
- Use `uv` (never pip)
- Environment variables and requirements in `pyproject.toml`.
- Few secrets in .env file

### Versions
- Django: 5.2.4
- Django REST Framework: 3.16.0

### Prohibitions
- Never use Django 4.x or earlier versions
- Never use DRF 3.15.x or earlier versions

## Frontend
- Astro SSR

### Versions
- Astro: 5.12.4
- Tailwind CSS: 4.0.0

### Prohibitions
- Never create the `tailwind.config.js` file (Tailwind 3 configuration in Tailwind 4)
- Never use traditional CSS or style blocks
- Always use Tailwind 4 exclusively for styling
- Never use Tailwind 3.x or earlier versions