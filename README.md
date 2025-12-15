# ShporaAI Orchestrator

This repository is an orchestrator that combines three microservices via **git submodules** and runs everything with Docker Compose.

The services are:
- `user-service` (Java + Spring Boot)
- `ai-service` (Python + FastAPI)
- `frontend-service` (React + Vite)


## Cloning the Project (with submodules)

**Important:** This repo uses git submodules. Always clone with submodules or initialize them manually.

```bash
# Preferred: clone with submodules in one step
git clone --recurse-submodules https://github.com/StillNotEnough/shpora-ai-orchestrator-local.git

# If already cloned without submodules
git clone https://github.com/StillNotEnough/serviceName.git
cd ShporaAI-orchestrator
git submodule init
git submodule update
```

## Updating Submodules to Latest Versions

Submodules are pinned to specific commits. To pull the latest changes from their remote branches (usually `main`/`master`):

```bash
# Update all submodules to the latest commit on their remote
git submodule update --remote

# Update a single submodule
git submodule update --remote services/user-service

# After updating, commit the new submodule versions in the orchestrator
git add services/*
git commit -m "Update submodules to latest"
git push
```

Always rebuild Docker images after updating submodules.

## Environment Setup

Copy the example env file and fill in your values:

```bash
cp .env.example .env
```

Example `.env` content:

```env
DB_NAME=shpora_db
DB_USER=shpora_user
DB_PASSWORD=strong_password

JWT_SECRET=very-long-random-secret-key
JWT_ACCESS_EXPIRATION=1800000
JWT_REFRESH_EXPIRATION=1209600000

OPENROUTER_API_KEY=sk-or-your-key-here
```

## Running the Project

```bash
# First time or after code changes in submodules
docker-compose up --build -d

# Normal start (no rebuild)
docker-compose up -d
```

Access:
- Frontend: http://localhost (port 80)
- User Service API: http://localhost:8080/api/v1
- AI Service: http://localhost:8000
- Postgres: localhost:5433

## Useful Docker Compose Commands

```bash
# Stop everything
docker-compose down

# Stop and remove volumes (clears DB data!)
docker-compose down -v

# Rebuild images without cache (use after submodule updates if issues occur)
docker-compose build --no-cache

# Follow logs of a specific service
docker-compose logs -f user-service
docker-compose logs -f ai-service
docker-compose logs -f frontend

# Enter a container shell
docker-compose exec user-service sh
docker-compose exec ai-service bash
docker-compose exec frontend sh
```

## Quick Reminder Checklist

1. Clone with `--recurse-submodules`
2. Update submodules: `git submodule update --remote`
3. Copy/fill `.env`
4. Run `docker-compose up --build -d`
5. Rebuild after any submodule update

That's it — everything should be up and running.
