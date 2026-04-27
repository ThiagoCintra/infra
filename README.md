# itau-microservices-infra (clean template)

This repository is a clean, minimal reimplementation of the bootstrap/installation
experience from the original project. It's intended to be used as a starting point for
creating a new repository without carrying over the old project's history.

Features
- One-command bootstrap via `setup.sh`
- `make` targets: `bootstrap`, `up`, `down`, `health`, `e2e`, `logs`, `clean`
- Docker Compose stack with a simple Flask `app` service
- Health check waiting and optional E2E test execution

Added services in this template
- Traefik (reverse proxy + dashboard)
- Postgres (database)
- RabbitMQ (message broker with management UI)
 - MongoDB (document database)
 - H2 (embedded Java DB with optional web console)

Quick start (local)

1. Bootstrap locally (from the repo):

```bash
bash setup.sh
```

2. Bootstrap with ZIP download (if you host this template as a ZIP):

```bash
bash setup.sh https://github.com/youruser/yourrepo/archive/refs/heads/main.zip
```

3. Make helpers

```bash
make bootstrap   # runs setup.sh
make up          # docker-compose up -d --build
make health      # wait for health endpoint
make e2e         # run basic E2E tests
make down        # stop stack
make logs        # stream logs

Default service URLs and credentials
- App: http://localhost/  (routed through Traefik on port 80) and http://localhost:8080/ (direct)
- App health: http://localhost:8080/health
- Traefik dashboard: http://localhost:8081/
- Postgres: localhost:5432 (user: user, password: password, db: itau)
- RabbitMQ management: http://localhost:15672/ (user: user, password: password)
- MongoDB: localhost:27017
- H2: TCP 9092, Web console: http://localhost:8082/ (if enabled)

If you want to change credentials or ports, edit `docker-compose.yml` or set environment variables before running the scripts:

```bash
export POSTGRES_USER=myuser
export POSTGRES_PASSWORD=mypass
export RABBIT_USER=myuser
export RABBIT_PASS=mypass
export MONGO_INITDB_ROOT_USERNAME=user
export MONGO_INITDB_ROOT_PASSWORD=password
```
```

Notes
- `setup.sh` checks for `docker`, `docker-compose`, and `curl`. It will fail fast if missing.
- The template provides a minimal Flask app at port 8080 with `/` and `/health`.

How to create a new remote repository
1. Initialize a new git repository and push to GitHub as usual from this directory.
2. The new repository will be clean: no old commit history is preserved.

License
- Use as you wish; this is a template.
