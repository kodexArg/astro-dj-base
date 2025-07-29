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
- Avoid writing code snippets in responses, prefer links to files, classes and functions
- Decorate variables with backticks like `variable`

## Backend
- Use `uv` (never pip)
- Environment variables and requirements in `pyproject.toml`.
- Few secrets in .env file

## Frontend
- Astro SSR