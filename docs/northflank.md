# Deploying Noteline on Northflank

> **Unverified.** Northflank's public docs didn't fully confirm the exact field names for referencing an addon's
> connection string (`${refs.noteline-db.envs.POSTGRES_URI}`) or the `${fn.randomSecret(...)}` helper's output
> format. Test a real deploy from [`template.json`](../northflank/template.json) before relying on this, and
> adjust the reference paths to match whatever Northflank's UI shows once the Postgres addon is created.

Like Railway, Northflank doesn't expose a purely file-based one-click button from an arbitrary public repo — you
import the template into Northflank once, then share the resulting link.

## One-time setup (repo owner)

1. In the Northflank dashboard, go to **Templates → New template → Import from JSON/code** and paste in
   [`template.json`](../northflank/template.json).
2. Run it once to confirm the Postgres addon connects and Noteline boots. If the `DATABASE_URL` reference path
   doesn't resolve, check the addon's **Info** tab in the dashboard for its actual connection-string variable
   name and fix the reference in `template.json`.
3. After the first deploy, set `PUBLIC_BASE_URL` to the DNS Northflank assigned the `noteline` service's `web`
   port (**Ports** tab), then restart the service so it takes effect.
4. Once it works end-to-end, use **Share → Generate one-click deploy link** on the saved template to get a
   shareable URL you can turn into a deploy button.
