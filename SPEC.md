# SPEC

**MANDATORY INSTRUCTIONS - FOLLOW STRICTLY**

## Project Context

### Purpose
Full-stack web application with Django REST API backend and Astro SSR frontend, designed for AWS cloud deployment with containerized architecture.

### Application Type
- **Backend**: RESTful API service providing data endpoints
- **Frontend**: Server-side rendered web application with dynamic content
- **Deployment**: Cloud-native application optimized for AWS infrastructure

### Target Audience
- End users: Web application consumers
- Developers: Team members working on feature development
- DevOps: Infrastructure and deployment management

### Primary Use Cases
1. Serve dynamic web content through Astro SSR
2. Provide REST API endpoints for data operations
3. Handle user authentication and authorization
4. Manage static assets through CDN
5. Scale automatically based on traffic demands


## Architecture

### System Overview
```
[User] → [CloudFront CDN] → [Load Balancer] → [Elastic Beanstalk]
                ↓                              ↓
         [S3 Static Assets]              [Docker Containers]
                                              ↓
                                    [Django API] ↔ [Astro SSR]
                                              ↓
                                         [Database]
```

### Components
- **Backend**: Django + DRF (API-only, internal communication)
- **Frontend**: Astro SSR (server-side rendering, public-facing)
- **Database**: PostgreSQL (managed via AWS RDS)
- **Cache**: Redis (managed via AWS ElastiCache)
- **Storage**: S3 for static assets, ECR for container images
- **CDN**: CloudFront for global content delivery
- **Runtime**: Elastic Beanstalk with Docker platform

### Data Flow
1. User requests → CloudFront → Load Balancer
2. Dynamic content → Astro SSR → Django API → Database
3. Static assets → CloudFront → S3
4. API responses → Django → Astro → User

### Design Patterns
- **API Gateway Pattern**: Astro acts as gateway to Django API
- **Microservices**: Separate containers for frontend/backend
- **CDN Pattern**: Static assets served from edge locations
- **Health Check Pattern**: Monitoring endpoints for all services

### Technical Decisions Rationale
- **Astro SSR**: Better SEO and initial page load performance
- **Django API-only**: Separation of concerns, scalability
- **Docker containers**: Consistent environments, easy deployment
- **AWS managed services**: Reduced operational overhead 


## Development Workflow

### Local Development Process
1. **Setup**: `docker-compose up -d` (starts both services)
2. **Backend development**: Work in `/backend` directory
3. **Frontend development**: Work in `/frontend` directory
4. **Database**: Automatic initialization via scripts
5. **Testing**: Run tests before committing changes

### Essential Commands
```bash
# Development
docker-compose up -d          # Start all services
docker-compose logs -f        # View logs
docker-compose down           # Stop services

# Backend specific
docker-compose exec backend python manage.py shell
docker-compose exec backend python manage.py test

# Frontend specific
docker-compose exec frontend npm run dev
docker-compose exec frontend npm run build
```

### Project Structure
```
astro-dj-base/
├── docker-compose.yaml       # Development orchestration
├── backend/
│   ├── Dockerfile           # Backend container
│   ├── pyproject.toml       # Dependencies & config
│   ├── manage.py
│   └── apps/                # Django applications
├── frontend/
│   ├── Dockerfile           # Frontend container
│   ├── package.json
│   ├── astro.config.mjs
│   └── src/                 # Astro source code
└── supabase/
    └── migrations/          # Database migrations
```

### Naming Conventions
- **Files**: snake_case for Python, kebab-case for frontend
- **Variables**: snake_case (Python), camelCase (JavaScript)
- **Classes**: PascalCase
- **Constants**: UPPER_SNAKE_CASE
- **URLs**: kebab-case endpoints
- **Docker images**: project-name/service-name

## Prohibitions (With Context)

### Command Restrictions
- **NEVER execute with `uv`**: 
  - *Why*: Project uses pip/poetry for dependency management
  - *Alternative*: Use `pip install` or `poetry add`
  - *Consequence*: Dependency conflicts and build failures

- **NEVER execute `git`**: 
  - *Why*: Version control handled externally
  - *Alternative*: Use IDE git integration
  - *Consequence*: Workflow disruption

- **NEVER execute migrations**: 
  - *Why*: Database schema managed via separate process
  - *Alternative*: Use provided migration scripts
  - *Consequence*: Database inconsistencies

### Code Quality Rules
- **NEVER write comments in code**: 
  - *Why*: Code should be self-documenting
  - *Alternative*: Use descriptive variable/function names
  - *Consequence*: Code review failures

- **NEVER write docstrings** (but preserve existing ones):
  - *Why*: Documentation maintained separately
  - *Alternative*: Update external documentation
  - *Consequence*: Documentation inconsistencies

- **ALWAYS remove comments from existing code**:
  - *Why*: Maintain clean, production-ready code
  - *Alternative*: Refactor code for clarity
  - *Consequence*: Code style violations

### Security Requirements
- **API internal only**: Secured through AWS VPC and security groups
- **HTTPS/SSL**: Termination at CloudFront level
- **No public API exposure**: All external access via Astro frontend


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


## AWS Deployment

### ECR (Elastic Container Registry)
- Repository naming: `project-name/backend` and `project-name/frontend`
- Image tagging strategy: `latest`, `staging`, semantic versioning
- Automated builds on code changes
- Image vulnerability scanning enabled

### S3 + CloudFront
- S3 bucket for static assets (CSS, JS, images)
- CloudFront distribution for global CDN
- Origin Access Control (OAC) for S3 security
- Cache policies optimized for static content
- HTTPS redirect and SSL certificate

### Elastic Beanstalk
- Platform: Docker running on Amazon Linux 2
- Environment types: staging and production
- Auto-scaling configuration
- Health checks on `/health` endpoint
- Rolling deployments with health monitoring


## Environment Variables

### AWS Configuration
- `AWS_REGION`: AWS deployment region
- `AWS_S3_BUCKET_NAME`: S3 bucket for static files
- `AWS_CLOUDFRONT_DOMAIN`: CloudFront distribution domain
- `AWS_ECR_REGISTRY`: ECR registry URL

### Application Environment
- `ENVIRONMENT`: production, staging, development
- `DEBUG`: false for production
- `ALLOWED_HOSTS`: Elastic Beanstalk environment URLs
- `DATABASE_URL`: RDS connection string
- `REDIS_URL`: ElastiCache Redis connection


## Docker Optimizations

### Multi-stage Builds
- Separate build and runtime stages
- Minimal base images (alpine variants)
- Layer caching optimization
- Security scanning integration

### Health Checks
- Backend: `HEALTHCHECK --interval=30s --timeout=3s CMD curl -f http://localhost:8000/health || exit 1`
- Frontend: `HEALTHCHECK --interval=30s --timeout=3s CMD curl -f http://localhost:4321/health || exit 1`

### ECR Specific
- Image size optimization (<500MB per image)
- Vulnerability scanning on push
- Lifecycle policies for old images


## Security

### IAM Roles and Policies
- Elastic Beanstalk service role
- EC2 instance profile for application
- S3 access policies (read-only for static files)
- ECR pull permissions

### VPC and Security Groups
- Private subnets for application instances
- Public subnets for load balancer
- Security groups: HTTP/HTTPS (80,443), SSH (22) restricted
- Database security group (5432) internal only

### SSL/TLS Configuration
- CloudFront SSL certificate (AWS Certificate Manager)
- HTTPS redirect at CloudFront level
- Secure headers configuration
- HSTS policy enforcement

## AI Assistant Guidelines

### Behavioral Requirements
1. **Always prioritize security**: Never expose internal APIs publicly
2. **Follow architecture patterns**: Maintain separation between frontend/backend
3. **Respect prohibitions**: Understand the reasoning behind each restriction
4. **Validate before changes**: Check existing code patterns before modifications
5. **Maintain consistency**: Follow established naming and structure conventions

### Decision Making Priorities
1. **Security** > Performance > Developer Experience
2. **AWS best practices** > Custom solutions
3. **Existing patterns** > New implementations
4. **Documented standards** > Assumptions

### Problem Escalation
- **Security concerns**: Immediately flag and request guidance
- **Architecture changes**: Discuss before implementation
- **Dependency updates**: Verify compatibility first
- **Performance issues**: Profile before optimizing

### Required Validations
- Check existing code patterns before adding new features
- Verify AWS resource limits and costs
- Ensure Docker images build successfully
- Test health check endpoints
- Validate environment variable usage

## Code Examples

### Django API Endpoint Pattern
```python
# apps/api/views.py
class UserViewSet(viewsets.ModelViewSet):
    queryset = User.objects.all()
    serializer_class = UserSerializer
    permission_classes = [IsAuthenticated]
    
    def get_queryset(self):
        return self.queryset.filter(is_active=True)
```

### Astro Page Pattern
```astro
---
// src/pages/users.astro
const response = await fetch('http://backend:8000/api/users/');
const users = await response.json();
---
<html>
  <body>
    <div class="container mx-auto">
      {users.map(user => <div class="user-card">{user.name}</div>)}
    </div>
  </body>
</html>
```

### Environment Configuration
```bash
# .env.example
DJANGO_SECRET_KEY=your-secret-key
DATABASE_URL=postgresql://user:pass@localhost:5432/db
REDIS_URL=redis://localhost:6379/0
AWS_REGION=us-east-1
AWS_S3_BUCKET_NAME=your-bucket
```

### Docker Health Check
```dockerfile
# backend/Dockerfile
HEALTHCHECK --interval=30s --timeout=3s --start-period=5s --retries=3 \
  CMD curl -f http://localhost:8000/health/ || exit 1
```

## Troubleshooting

### Common Issues

#### Container Won't Start
- **Symptoms**: Docker container exits immediately
- **Check**: `docker-compose logs service-name`
- **Common causes**: Missing environment variables, port conflicts
- **Solution**: Verify `.env` file and port availability

#### API Connection Refused
- **Symptoms**: Frontend can't reach backend API
- **Check**: `docker-compose ps` (ensure both services running)
- **Common causes**: Network configuration, service discovery
- **Solution**: Use service names in URLs (`http://backend:8000`)

#### Static Assets Not Loading
- **Symptoms**: CSS/JS files return 404
- **Check**: Browser network tab, S3 bucket permissions
- **Common causes**: CloudFront cache, S3 permissions
- **Solution**: Invalidate CloudFront cache, check IAM policies

#### Database Connection Error
- **Symptoms**: Django can't connect to database
- **Check**: Database service status, connection string
- **Common causes**: Wrong credentials, network issues
- **Solution**: Verify `DATABASE_URL` environment variable

### Diagnostic Commands
```bash
# Check service health
docker-compose ps
docker-compose logs --tail=50 backend
docker-compose logs --tail=50 frontend

# Test API connectivity
curl http://localhost:8000/health/
curl http://localhost:4321/health/

# Check environment variables
docker-compose exec backend env | grep DJANGO
docker-compose exec frontend env | grep NODE

# Database connectivity
docker-compose exec backend python manage.py dbshell
```

### Important Logs
- **Backend**: Django application logs, database query logs
- **Frontend**: Astro build logs, Node.js runtime logs
- **Infrastructure**: CloudWatch logs, Elastic Beanstalk logs
- **Security**: VPC flow logs, CloudFront access logs

### Performance Monitoring
- **Metrics**: Response time, error rate, throughput
- **Tools**: CloudWatch, Elastic Beanstalk health dashboard
- **Alerts**: High error rate, resource utilization
- **Optimization**: Database query analysis, CDN cache hit ratio