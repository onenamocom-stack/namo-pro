# Namo — Consultant app

The consultant (astrologer) side of [Namo](https://1namo.com): earnings,
studio, incoming consultations, profile. A seeker asks a question; a
consultant answers it, live, for money — this repo hosts the second half
of that marketplace.

**This repo contains no application source.** The app is one codebase —
[onenamocom-stack/namoApp](https://github.com/onenamocom-stack/namoApp) —
with two build targets. The seeker app is the default build (`dist/`,
deployed to 1namo.com); the consultant app is the `--mode pro` build
(`dist-pro/`) and is deployed from *this* repo's workflow, which checks
out the main repo, builds the pro target, and publishes it to GitHub
Pages. One codebase, two apps, two deployments — nothing is duplicated.

**Live:** https://onenamocom-stack.github.io/namo-pro/

## What the consultant app carries

`/pro/earnings` · `/pro/studio` · `/pro/consult` · `/pro/profile` ·
`/pro/apply` (consultant onboarding — it sits outside the session gate and
signs a new consultant in), plus the two seeker screens the pro side links
out to: `/chart` (booking detail) and `/consult/:id` (previewing your own
public page). Everything else — seeker tabs, onboarding, wallet, shop —
is absent from this build by design.

## How it deploys

Pushing to `main` of **namoApp** does NOT redeploy this site (the workflow
there builds the seeker target only). To ship a consultant-app change,
re-run this repo's workflow (`Actions → Deploy consultant app → Run
workflow`) after the namoApp change lands — or push any commit here.
The build always tracks `onenamocom-stack/namoApp@main`.

Required repository secrets (Settings → Secrets → Actions):

| Secret | What |
|---|---|
| `VITE_SUPABASE_URL` | Supabase project URL (dev values today; production at cutover) |
| `VITE_SUPABASE_ANON_KEY` | The anon key — public by design, ships in the bundle |

## Current status

Built and deployable; pointed at the **namo-dev** Supabase project until
the production cutover (see `docs/07-DJANGO-MIGRATION.md` and `HANDOFF.md`
in the main repo). Auth is Supabase phone OTP — on dev, the configured
test numbers sign in with code `123456`, no SMS.

## Documentation lives in the main repo

`HANDOFF.md` (what is true right now) · `docs/01-07` (PRD, TRD, app flow,
UI/UX, schema, implementation, Django migration plan) ·
`backend/INSTRUCTIONS.md` (the engineering rules).
