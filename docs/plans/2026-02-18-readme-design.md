# README Design

**Date:** 2026-02-18
**Status:** Approved

## Goal

Create a public-facing `README.md` for npm and GitHub, plus a `docs/COMMANDS.md` full command reference linked from the README.

## Files

- `README.md` — project root, npm-friendly, scannable
- `docs/COMMANDS.md` — full per-command reference with flags and real output examples

## README.md Structure

1. **Badge + description** — npm version badge, one-line description
2. **Install** — `npm install -g whoop-sync`
3. **Quick start** — 4 commands: login, summary, dashboard, chart
4. **Setup** — WHOOP app registration link, `.env` vars (`WHOOP_CLIENT_ID`, `WHOOP_CLIENT_SECRET`, `WHOOP_REDIRECT_URI`), `auth login` step, token storage location
5. **Commands** — all commands in 4 tables:
   - Auth: login, logout, status, refresh
   - Data: sleep, recovery, workout, cycle, profile, body
   - Analysis: summary, dashboard, trends, insights
   - Charts: chart (5 types)
   - Lookups: get, cycle-sleep, cycle-recovery
6. **Flags** — 3 tables: data flags, analysis flags, global flags
7. **Example output** — real terminal output for summary --color, dashboard, trends --days 7, insights + one JSON snippet
8. **Token management** — cron/automation note, `auth refresh` usage
9. **Exit codes** — table (0 success, 1 general, 2 auth, 3 rate limit, 4 network)
10. **Development** — `npm run dev`, `npm run build`, `npm test`
11. **Attribution** — "Built on top of [whoop-cli](https://github.com/xonika9/whoop-cli) by xonika9"
12. **License** — MIT

## docs/COMMANDS.md Structure

Full reference linked from README. One section per command group:

- **Auth commands** — each with all flags and example output
- **Data commands** — each with all flags, default JSON output note, `--pretty` example
- **Analysis commands** — summary (with/without --color), dashboard, trends (7/14/30), insights
- **Chart commands** — all 5 chart types, `-o` flag, browser behaviour
- **Lookup commands** — get sleep/workout/cycle, cycle-sleep, cycle-recovery; UUID vs integer ID distinction

## Real outputs to embed (captured 2026-02-17)

### summary --color
```
📊 7-Day Summary

🔴 Avg Recovery:  31.5%
💓 Avg HRV:       124.4ms
❤️  Avg RHR:       56.1bpm
🔴 Avg Sleep:     44.8% | 4.0h
🔥 Avg Strain:    6.8
```

### dashboard
```
📅 2026-02-17 | Ruben Khachaturov

── Recovery ──────────────────────────
🔴 13% | HRV: 129ms (↑ vs 124 avg) | RHR: 60bpm (↑ vs 56 avg)
   SpO2: 96% | Skin: 33.1°C | Resp: 15.6/min

── Sleep ─────────────────────────────
😴 27% | 2.0h total | Efficiency: 100%
   Deep: 1.0h (49%) | REM: 0.6h (30%) | Light: 0.4h
   Disturbances: 1 | Consistency: 66%
   💤 Sleep debt: 2.1h | Need tonight: 9.7h

── Strain ────────────────────────────
🔥 4.1 / 6 optimal | 767 cal

── 7-Day Trends ──────────────────────
   HRV:      148 → 129ms ↓  (range 66-179)
   RHR:      49 → 60bpm ↑  (range 49-64)
   Recovery: 44 → 13% ↓
   Sleep:    4.6 → 2.0h ↓
   Strain:   6.8 avg (range 4.1-16.4)
```

### trends --days 7
```
📊 7-Day Trends

💚 Recovery: 31.5% avg (5-85) ↓
💓 HRV: 124.4ms avg (66-179) ↓
❤️ RHR: 56.1bpm avg (49-64) ↑
😴 Sleep: 44.8% avg (25-78) ↓
🛏️ Hours: 4.3h avg (1.3-7.2) ↓
🔥 Strain: 6.8 avg (4.1-16.4) ↓
```

### insights
```
💡 Insights & Recommendations

🔴 Red Recovery
   Recovery at 13% — body needs rest.
   → Prioritize rest, hydration, and sleep tonight.

🔴 Significant Sleep Debt
   You have 2.1 hours of accumulated sleep debt.
   → Try to get to bed 30-60 min earlier for the next few days.
```

## Additional outputs to capture during implementation

- `auth status` — JSON output
- `sleep --pretty` — example sleep record
- `recovery --pretty` — example recovery record
- `workout --pretty` — example workout record (or note "no workout recorded" case)
- `whoop-sync --help` — help text
- JSON combined output snippet (from `dashboard --json` or `recovery` default)

## Constraints

- No fabricated data — all output examples must be captured live from the API
- No push to GitHub until ready for publishing
- Repo URL: https://github.com/mrkhachaturov/whoop-sync
- npm package name: whoop-sync
- Node.js requirement: >=22.0.0
