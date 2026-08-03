# Deploying Noteline on Railway

Railway doesn't support a purely file-based "one-click deploy from a public repo" button — a real "Deploy on
Railway" button only works once you've generated a **template** through Railway's dashboard. That's a one-time
setup step for whoever owns this repo; end users then just click the resulting button.

## One-time setup (repo owner)

1. Create a new Railway project.
2. Add a **Postgres** database service (Railway's official Postgres plugin, not a custom image).
3. Add a second service with source **Docker Image** → `sololevelingquest/noteline:latest`.
4. On the Noteline service, set these variables:
   - `DATABASE_URL` → `${{Postgres.DATABASE_URL}}` (Railway's built-in cross-service reference)
   - `SECRET_KEY` → generate with `openssl rand -base64 32` and paste it (Railway doesn't auto-generate
     secrets from a config file the way Render does — use the "Generate" option in the variable editor, or
     paste your own)
   - `NOTELINE_ENCRYPTION_KEY` → same as above, another `openssl rand -base64 32` value
   - `PUBLIC_BASE_URL` → `https://${{RAILWAY_PUBLIC_DOMAIN}}` (Railway's built-in reference to this service's
     assigned public domain — resolves automatically, no manual step needed here)
   - `PORT` → `8080`
5. This repo's [`railway.json`](../railway.json) already sets the health check path (`/health`) and restart
   policy — Railway picks it up automatically if you connect this repo, though since we're deploying a prebuilt
   image rather than building from source, connecting the repo is optional.
6. Once it's running correctly, go to **Project Settings → Generate Template from Project**. This opens
   Railway's template composer, pre-filled from your working project. Publish it, then copy the generated
   template code.

## Deploy button

Replace `TEMPLATE_CODE` below with the code from step 6:

```markdown
[![Deploy on Railway](https://railway.com/button.svg)](https://railway.com/new/template/TEMPLATE_CODE?utm_medium=integration&utm_source=button&utm_campaign=noteline)
```
