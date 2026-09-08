## Project background

This repository holds the owner's personal trip plans, built with the
open-source [skywain/trip-planner-skill](https://github.com/skywain/trip-planner-skill)
(MIT). Each trip lives under `travel/<planner>/<destination>-<year>/`, with
`plan.geo.json` as the single source of truth, rendered into themed HTML pages
(`trip-<theme>.html`), a printable summary (`daily-schedule.md` / `.html`), and
an offline `trip.kml` map.

Current trip (first draft): **Thailand (Phuket + Phi Phi), 2026-09-26 →
2026-10-04** — Mid-Autumn + National Day holiday, 2 adults. Start from
`travel/planner-a/thailand-2026/daily-schedule.md`.

For any travel-planning or trip-related task, use this background directly;
never ask the owner to restate the trip context.

## Research sources

Besides trip-planner-skill, 小红书 (Xiaohongshu) is a research channel. Two
ways to reach the logged-in session:

1. **CDP** — connect directly to the user's Chrome on its remote-debugging
   port; the user is already logged in to Xiaohongshu there.
2. **opencli** — installed at `~/.nvm/versions/node/v24.15.0/bin/opencli`
   (`opencli doctor` checks the bridge; v1.8.7 daemon + extension are
   connected). Drive the browser through named sessions, e.g.
   `opencli browser xhs open <url>` / `opencli browser xhs extract`.

Save Xiaohongshu findings under the trip's `xhs-research/` directory, as
already done in `travel/planner-a/thailand-2026/xhs-research/`.

## Agent skills

### Issue tracker

Issues and specs for this repo live as GitHub issues on `yizheloverice/plan`; use the `gh` CLI. See `docs/agents/issue-tracker.md`.

### Triage labels

The five canonical triage roles map to the default label strings: `needs-triage`, `needs-info`, `ready-for-agent`, `ready-for-human`, `wontfix`. See `docs/agents/triage-labels.md`.

### Domain docs

Single-context: one `CONTEXT.md` plus `docs/adr/` at the repo root. See `docs/agents/domain.md`.
