# Deploying Noteline on Dokploy

Dokploy is self-hosted, so there's no central "click here to deploy" webpage the way there is for Render or
Railway — you point *your own* Dokploy instance at this repo.

## Deploying to your instance

1. In your Dokploy dashboard: **Create → Compose**.
2. Set the source to this repo's git URL (public). Dokploy will find [`docker-compose.yml`](../docker-compose.yml)
   at the root automatically.
3. Before deploying, set these environment variables in the **Environment** tab (Dokploy substitutes `${VAR}`
   references in the compose file from here):
   - `POSTGRES_PASSWORD` — any strong password
   - `SECRET_KEY` — `openssl rand -base64 32`
   - `NOTELINE_ENCRYPTION_KEY` — `openssl rand -base64 32`
   - `PUBLIC_BASE_URL` — the domain you're about to attach to the `noteline` service (set the domain first under
     **Domains**, then come back and fill this in before the first deploy)
   - Optionally `ADMIN_EMAIL` / `ADMIN_PASSWORD` (default to `admin@example.com` / `changeme` — change these)
4. Deploy.

## Getting a real one-click entry in Dokploy's built-in template gallery

The templates that show up inside Dokploy's own "one-click" template picker live in
[Dokploy/templates](https://github.com/Dokploy/templates) (a `blueprints/<id>/` folder with
`docker-compose.yml` + `template.toml` + `meta.json`, submitted via PR to their repo). That's a separate,
optional contribution outside this repo — let us know if you want to pursue it, since it means maintaining a
synced copy of the compose file in their monorepo rather than just this one.
