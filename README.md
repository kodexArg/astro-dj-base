# Astro Django Base

Full-stack web application with Django REST Framework backend and Astro SSR frontend.

## Stack

**Backend**
- Django 5.2.4
- Django REST Framework 3.16.0
- Python with uv package manager

**Frontend**
- Astro 5.12.4 (SSR)
- Tailwind CSS 4.0.0

**Deployment**
- AWS ECR (Docker images)
- AWS S3 + CloudFront (static assets)
- AWS Elastic Beanstalk (runtime)

## Development

```bash
docker-compose up --build
```

## License
MIT