# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## What this is

A single-file, no-build World Cup 2026 live dashboard: live scores, group standings, team formations on a pitch view, recent results, and a scrolling ticker. Everything — HTML, CSS, and JavaScript — lives in `index.html`. There is no package.json, no bundler, no framework, and no test suite.

## Files

- `index.html` — the entire app (styles in a `<style>` block, markup, and vanilla JS in a `<script>` block at the bottom)
- `favicon.ico`
- `README.md` — feature list, customization notes, and dated session notes; check it for the latest known state of the app before making changes

## Development

There is no build step. Serve the file and edit directly:

```bash
python3 -m http.server 8000
# open http://localhost:8000
```

Changes to `index.html` are visible on browser refresh — no compile/watch process exists. There are no lint or test commands configured; verify changes by loading the page in a browser and exercising the UI (select teams, check the live window, watch the ticker).

Deployment is to Vercel (or any static host) — just the two files, no build command needed.

## Architecture

Everything is driven from the bottom `<script>` block in `index.html`:

- **State**: module-level globals `matches`, `groups`, `selectedTeam` hold all fetched/derived state (no framework, no reactive store — DOM is re-rendered imperatively via `innerHTML`/`textContent` on state changes).
- **Data source**: Zafronix World Cup API (`API_BASE = https://api.zafronix.com/fifa/worldcup/v1`), authenticated with a hardcoded `X-API-Key` header (`API_KEY` near the top of the script). Two endpoints are used:
  - `/tournaments/2026` — teams + group assignments (`fetchTeams`)
  - `/matches?year=2026` — all matches, including live status (`fetchMatches`)
- **Data flow** (see `init()`): on load, fetch teams (grouped A–L) and matches once; if a live match exists, render it; then poll `fetchMatches` every 30s to refresh live status. There is no polling for team/group data after initial load.
- **Rendering functions** are one-per-panel and are called directly after state changes (no diffing):
  - `renderGroups()` — sidebar list of all 12 groups/48 teams, wires click handlers to `selectTeam(team)`
  - `renderFormation(team)` — builds the SVG-less pitch view; looks up `formations[team]`, falling back to `defaultFormation` (4-4-2) for any team not explicitly listed; player coordinates are computed by `getFormationPositions(formationString)` which parses `"D-M-A"` (e.g. `"4-3-3"`) into three evenly-spaced rows
  - `renderTeamResults(team)` — derives recent results and full group standings by filtering the in-memory `matches` array and computing W/D/L/points client-side (the API does not provide standings directly)
  - `showLiveMatch(match)` — toggles the live-match banner and fills in score/team/minute
  - `updateTicker()` — static scrolling ticker text (not derived from live data)
- **Lookup tables** near the top of the script are the main places to extend team-specific data:
  - `countryFlags` — country name → emoji flag, used everywhere a team is displayed; unmapped teams fall back to 🏳️
  - `formations` — only a handful of teams (Brazil, England, Germany, France, Argentina, Spain) have explicit formations; everyone else uses `defaultFormation`
- **Theming**: CSS custom properties in `:root` (`--accent`, `--accent-alt`, `--live`, `--gold`, etc.) control the color scheme; change these rather than hardcoding colors elsewhere.

## Known constraints (see README for full detail)

- Match/team names must match between the `countryFlags`/`formations` tables and whatever strings the Zafronix API returns — there's no normalization layer.
- Live updates are polling-based (30s interval), not push/websocket.
- The API key is currently committed in plaintext in `index.html`; treat it as a low-value free-tier key, not a secret to protect further — but don't introduce *new* hardcoded secrets.
