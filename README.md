# Veliora

Mental-health and emotional-wellness web app. This is the **orchestration repo** — it pulls in two submodules (`veliora-be`, `veliora-fe`) and runs everything together via Docker Compose.

```
veliora-app/                 ← this repo
├── veliora-be/              ← submodule: NestJS backend (port 3001)
├── veliora-fe/              ← submodule: Next.js frontend (port 3000)
├── docker-compose.yml       ← Mongo + Redis + backend + frontend
├── docker-compose.ngrok.yml ← optional ngrok overlay
├── nginx/                   ← reverse proxy config
└── .env.example             ← copy to .env and fill in
```

## Prerequisites

- Git
- Docker + Docker Compose
- (Backend prod features only) Google Cloud service-account JSON for Vertex AI

## Get the code (one command)

```bash
git clone --recurse-submodules git@github.com-veliora:usernameadmin5-cyber/veliora-app.git
cd veliora-app
```

Forgot `--recurse-submodules` on a previous clone?

```bash
git submodule update --init --recursive
```

Make recursion automatic for future `git pull` / `git checkout`:

```bash
git config submodule.recurse true
```

## Configure

```bash
cp .env.example .env
# Edit .env and fill in: JWT secrets, Google OAuth, Vertex AI project,
# SMTP credentials. See comments inside the file.
```

If you'll use Vertex AI (Gemini chat), drop the service-account key into `_key/` at the repo root — it's mounted read-only into the backend container at `/keys`. The `_key/` directory is gitignored.

## Run

```bash
docker compose up -d --build
```

Services:

| Service  | URL                              |
| -------- | -------------------------------- |
| Frontend | http://localhost:3000            |
| Backend  | http://localhost:3001/v1         |
| API docs | http://localhost:3001/docs       |
| Mongo    | localhost:27017 (veliora/secret) |
| Redis    | localhost:6379                   |

Stop everything:

```bash
docker compose down
```

Reset data volumes too:

```bash
docker compose down -v
```

## Pull updates across all three repos

```bash
git pull --recurse-submodules                 # parent + submodule contents at pinned commit
git submodule update --remote --merge         # advance submodules to each child's latest main
```

## Repos

- Parent: https://github.com/usernameadmin5-cyber/veliora-app
- Backend: https://github.com/usernameadmin5-cyber/veliora-be
- Frontend: https://github.com/usernameadmin5-cyber/veliora-fe
