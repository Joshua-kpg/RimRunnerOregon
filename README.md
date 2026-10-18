# RimRunnerOregon

Unofficial Oregon HS boys basketball ratings. Not affiliated with the OSAA or MaxPreps.
Status: **LIVE** at https://rimrunneroregon.web.app/ (Firebase Hosting, live since 2026-10-18). Backup copy: GitHub Pages.
created yet (Google project quota on the hosting account) and Cloudflare Pages is at its project limit. Working preview: this repo's GitHub Pages copy.
Until the site's Firebase project exists, accounts, picks and follows save only in the visitor's browser.

## Files
- `index.html` — the whole app (data embedded). `logos/` — small WebP school logos (from MaxPreps school profiles), served with the site.
- `data/results.json` — 2026-27 finals ledger written by the nightly updater.
- `firebase/` — Firestore rules and auth config for the site's Firebase project (accounts, picks, follows).

## Nightly scores

`.github/workflows/nightly-scores.yml` runs `scripts/update_scores.py` at 09:30 and 16:00 UTC
(1:30 am / 8:00 am Pacific standard time) and on manual dispatch. Before the Oregon boys season opens
(`SEASON_START` in index.html = Wed Dec 2, 2026, the OSAA first contest date) the script makes no requests.
From then on it reads boys basketball finals for the last 3 days from the MaxPreps Oregon scores-by-date page
(team-page fallback), matches them to the schedule by MaxPreps team id + date, records them in `data/results.json` and
bakes scores and W-L records into `index.html`. The page recomputes Elo from those finals. It commits only when data
changed, then deploys (index.html + logos/) to Cloudflare Pages if the `CLOUDFLARE_API_TOKEN` repository secret is set.
