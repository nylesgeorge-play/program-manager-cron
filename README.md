# program-manager-cron

A free cron trigger for [program-manager](https://github.com/nylesgeorge-play/program-manager) (private repo, not this one).

This repo is deliberately public and contains **zero app code, zero client
names, zero business logic** — just one GitHub Actions workflow that calls
`POST /internal/trigger/{source}` on the deployed app on a schedule. It's
public specifically because GitHub disables scheduled workflows on private
repos on the free plan; public-repo Actions have no such restriction and
unlimited free minutes.

## Why this exists

The app is deployed on Render's free tier, which sleeps after 15 minutes of
inactivity. Rather than pay for an always-on server (which is how this whole
approach started, and which caused real unexpected billing harm on a previous
attempt at GCP), the app's background polling was redesigned as on-demand HTTP
endpoints instead of perpetual loops — see `ENABLE_BACKGROUND_POLLING` /
`app/routes/trigger_router.py` in the main repo. This workflow is what actually
calls them, and its scheduled runs also happen to wake Render if it's asleep.

## Secrets

One repo secret, `TRIGGER_SECRET`, matching the same value set on the Render
service's `TRIGGER_SECRET` env var.
