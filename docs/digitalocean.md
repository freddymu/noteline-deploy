# Deploying Noteline on DigitalOcean App Platform

> **Unverified — please test before relying on this.** DigitalOcean's public docs confirm the `image:` (Docker
> Hub) service syntax and `${APP_URL}` / `${component.DATABASE_URL}` bindable variables used below, but do **not**
> confirm whether the classic "Deploy to DO" clickable button (the `deploytodo.com` badge +
> `cloud.digitalocean.com/apps/new?repo=...` URL) supports image-sourced apps — that flow's documented examples
> are all git-repo-based. Until verified, use the CLI/dashboard path below instead of assuming the badge works.

## Deploying

**Via doctl (recommended until the button is verified):**

```bash
doctl apps create --spec .do/deploy.template.yaml
```

**Via dashboard:** Apps → Create App → "I have an app spec already" → paste the contents of
[`deploy.template.yaml`](../.do/deploy.template.yaml).

## Before you deploy

`SECRET_KEY` and `NOTELINE_ENCRYPTION_KEY` are placeholders in the spec (`REPLACE_ME_openssl_rand_base64_32`) —
App Platform's `SECRET` env type doesn't support auto-generating a value from the spec file itself. Generate real
values and replace them before deploying:

```bash
openssl rand -base64 32
```

`PUBLIC_BASE_URL` is wired to DigitalOcean's built-in `${APP_URL}` variable, so it should resolve automatically
once the app has a domain — no manual step needed there, but confirm it looks right after the first deploy.

## Trying the "Deploy to DO" button anyway

If you want to test whether the button flow accepts this image-based spec:

```markdown
[![Deploy to DO](https://www.deploytodo.com/do-btn-blue.svg)](https://cloud.digitalocean.com/apps/new?repo=<this-repo-git-url>/tree/main)
```

If DigitalOcean's button flow rejects it (since it may expect a git-buildable app), fall back to the `doctl`/
dashboard path above and drop the button from the README.
