Authentik for ZimaOS (Docker)

Network-wide identity provider (IdP) using Authentik, packaged as a minimal Docker Compose stack.
Easy to deploy, easy to back up. Built for ZimaOS but works anywhere with Docker.

Highlights

✅ Latest Authentik (server + worker)

✅ PostgreSQL 16 with persistent volume

✅ Redis (AOF) with password

✅ One-file compose; no Cloudflare/Argo required

✅ Example .env for safe config

Live ports (default): UI on :9000 (http)
Change ports in .env (HTTP_PORT) or under ports: in docker-compose.yml.

Table of Contents

Included Files

Requirements

Quick Start

ZimaOS notes

Environment Variables

Managing the Stack

Backups & Restore

Reverse Proxy (Optional)

Troubleshooting

Uninstall / Clean Up

License

Included Files
docker-compose.yml          # Authentik + Postgres + Redis
.env.example                # Safe defaults; copy to .env and edit
.gitignore                  # Keeps runtime data out of git
LICENSE                     # MIT
custom-templates/           # (Optional) place Authentik templates here


Data lives at (by default)

./postgres_data – PostgreSQL data

./redis – Redis AOF/RDB

./media – Authentik media/uploads

These paths are bind-mounted by docker-compose.yml. Change them if you prefer a different location.

Requirements

ZimaOS with Docker & Docker Compose (default on ZimaOS)

Or any Linux with Docker Engine 20.10+ and Compose v2

Open TCP 9000 (or your chosen port)

Quick Start
1) Clone the repo
git clone https://github.com/Jacko88888/authentik-docker.git
cd authentik-docker

2) Copy env template and edit
cp .env.example .env
# Open .env and change every CHANGE_ME value

3) Start the stack
docker compose up -d


Open: http://<your-host>:9000/

First run may take a minute while images download and the DB initializes.

ZimaOS notes

Put the project anywhere under your ZimaOS data (e.g. /DATA/AppData/authentik), then run the Quick Start above from that directory.

If you use ZimaOS App Store networking helpers (Tailscale/Cloudflare/etc.), you don’t need them for Authentik. Expose 9000 locally or front it with your reverse proxy of choice (see below).

Environment Variables

Copy .env.example → .env and edit:

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


Tips

Generate a strong AUTHENTIK_SECRET_KEY (>=32 chars, letters/numbers/symbols).

Use unique passwords for Postgres and Redis.

Change HTTP_PORT if 9000 is taken.

Managing the Stack
# Start
docker compose up -d

# Stop
docker compose down

# See logs
docker compose logs -f

# Update to latest images and restart
docker compose pull && docker compose up -d

# Shell into a container (example: server)
docker exec -it authentik-server /bin/sh


Container names (by default): authentik-server, authentik-worker, authentik-postgres, authentik-redis.

Backups & Restore

Back up

# Stop the stack (optional but safest)
docker compose down

# Archive data volumes
tar -czf authentik-backup-$(date +%F).tgz postgres_data redis media docker-compose.yml .env


Restore

# From a clean folder:
tar -xzf authentik-backup-YYYY-MM-DD.tgz
docker compose up -d


If you change volume paths in compose, adjust backup/restore accordingly.

Reverse Proxy (Optional)

You can put Nginx/Traefik/Caddy in front of Authentik for HTTPS and a pretty domain:

Point your proxy upstream to http://HOST:HTTP_PORT (default :9000).

Forward these headers: X-Forwarded-Proto, X-Forwarded-For, Host.

In Authentik’s settings, set the external URL to https://id.example.com.

Troubleshooting

Login page not loading?
Check docker compose logs -f for server and worker.

Port already in use (9000)?
Change HTTP_PORT in .env and docker compose up -d.

Email not sending?
Confirm SMTP values in .env. Some providers require ports and TLS flags; set those in Authentik’s UI after first login.

Ran git add on data by mistake?
The repo’s .gitignore prevents this, but if it happens, run:

git rm -r --cached postgres_data redis media
git commit -m "Untrack runtime data"
git push

Uninstall / Clean Up
# Stop and remove containers
docker compose down

# Remove data (IRREVERSIBLE)
rm -rf postgres_data redis media

License

MIT — see LICENSE.

Notes

This repo intentionally avoids Cloudflare/Argo. Keep it simple, secure your host, and add a reverse proxy if you need TLS.

PRs and issues welcome. Enjoy! 🎉
