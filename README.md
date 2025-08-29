authentik-docker

Minimal, production-ready Docker Compose setup for Authentik
 with PostgreSQL and Redis.
✅ Works on ZimaOS, Ubuntu, or any Docker host.
❌ No Cloudflare/Argo required.

Features

Authentik server (latest release, containerized)

PostgreSQL 16 with persistent volume

Redis with password + AOF enabled

Example .env.example for quick setup

Clean .gitignore so runtime data never gets committed

Quick Start
# Clone the repo
git clone https://github.com/Jacko88888/authentik-docker.git
cd authentik-docker

# Copy env template and edit values
cp .env.example .env
nano .env   # change all CHANGE_ME values

# Start Authentik stack
docker compose up -d

Useful Commands
# Stop the stack
docker compose down

# Follow logs
docker compose logs -f

# Update images and restart
docker compose pull && docker compose up -d

Environment Variables

Edit .env (copied from .env.example) and replace placeholders:

# ===== Authentik =====
AUTHENTIK_SECRET_KEY=CHANGE_ME_TO_RANDOM_32+CHARS
AUTHENTIK_EMAIL__HOST=smtp.example.com
AUTHENTIK_EMAIL__USERNAME=no-reply@example.com
AUTHENTIK_EMAIL__PASSWORD=CHANGE_ME
AUTHENTIK_EMAIL__FROM=Authentik <no-reply@example.com>

# ===== Postgres =====
POSTGRES_DB=authentik
POSTGRES_USER=authentik
POSTGRES_PASSWORD=CHANGE_ME_DB

# ===== Redis =====
REDIS_PASSWORD=CHANGE_ME_REDIS

# ===== Ports =====
HTTP_PORT=9000

Volumes

All persistent data is stored locally:

postgres_data/ → PostgreSQL database

redis/ → Redis data

media/ → Authentik media uploads

custom-templates/ → Templates directory

These are mounted into containers and safe across restarts.

License

MIT License © 2025 Jacko88888

✅ Instructions to Update in GitHub

Go to your repo: authentik-docker
.

Click README.md → ✏️ Edit this file.

Delete everything inside.

Paste the content above.

Scroll down → write commit message:

Update README with full setup instructions


Select Commit directly to main branch.

Hit Commit changes.
