# whoop-sync

[![npm version](https://img.shields.io/npm/v/whoop-sync.svg)](https://www.npmjs.com/package/whoop-sync)

CLI for WHOOP health data — fetch, analyze, and visualize via the WHOOP API v2.

```bash
npm install -g whoop-sync
```

## Quick start

```bash
whoop-sync auth login                                 # authenticate via browser
whoop-sync summary --color                            # color-coded daily snapshot
whoop-sync dashboard                                  # full dashboard + 7-day trends
whoop-sync chart dashboard -n 30 -o /tmp/whoop.html  # interactive HTML chart
```

## Setup

1. Register a WHOOP application at [developer.whoop.com](https://developer.whoop.com)
   - Apps with fewer than 10 users don't need WHOOP review (immediate use)

2. Set environment variables — create a `.env` file in your working directory:

```env
WHOOP_CLIENT_ID=your_client_id
WHOOP_CLIENT_SECRET=your_client_secret
WHOOP_REDIRECT_URI=https://your-redirect-uri.com/callback
```

3. Authenticate:

```bash
whoop-sync auth login
```

Tokens are stored at `~/.whoop-sync/tokens.json` and auto-refresh 15 minutes before expiry.

## Commands

### Auth

| Command | Description |
| --- | --- |
| `whoop-sync auth login` | OAuth flow — opens browser, paste callback URL |
| `whoop-sync auth logout` | Revoke access on WHOOP server + clear local tokens |
| `whoop-sync auth status` | Show token expiry (does not refresh) |
| `whoop-sync auth refresh` | Force refresh access token using refresh token |

### Data

Output is JSON by default. Add `--pretty` for human-readable formatted output.

| Command | Description |
| --- | --- |
| `whoop-sync sleep` | Sleep stages, efficiency, respiratory rate |
| `whoop-sync recovery` | Recovery score, HRV, RHR, SpO2, skin temperature |
| `whoop-sync workout` | Workouts with strain, HR zones, calories |
| `whoop-sync cycle` | Daily physiological cycle (strain, calories) |
| `whoop-sync profile` | User info (name, email) |
| `whoop-sync body` | Body measurements (height, weight, max HR) |

### Analysis

Output is pretty-printed by default. Add `--json` for raw JSON.

| Command | Description |
| --- | --- |
| `whoop-sync summary` | Multi-day averages: recovery, HRV, RHR, sleep, strain |
| `whoop-sync summary --color` | Same with 🔴🟡🟢 color-coded status indicators |
| `whoop-sync dashboard` | Full terminal dashboard with 7-day trends |
| `whoop-sync trends` | Multi-day trend analysis with direction arrows ↑↓→ |
| `whoop-sync insights` | Health recommendations based on your recent data |

### Charts

Interactive HTML charts, opened in browser automatically. Use `-o file.html` to save instead.

| Command | Description |
| --- | --- |
| `whoop-sync chart sleep` | Sleep performance and stage breakdown |
| `whoop-sync chart recovery` | Recovery score and HRV over time |
| `whoop-sync chart strain` | Daily strain and calorie burn |
| `whoop-sync chart hrv` | HRV trend |
| `whoop-sync chart dashboard` | Combined overview (recommended) |

### Lookups

Fetch a single record by ID. IDs come from collection endpoint responses or `dashboard --json`.

| Command | Description |
| --- | --- |
| `whoop-sync get sleep <uuid>` | Full detail for one sleep record (UUID string) |
| `whoop-sync get workout <uuid>` | Full detail for one workout record (UUID string) |
| `whoop-sync get cycle <id>` | Full detail for one cycle (integer ID) |
| `whoop-sync cycle-sleep <cycleId>` | Sleep record linked to a given cycle |
| `whoop-sync cycle-recovery <cycleId>` | Recovery record linked to a given cycle |

## Flags

### Data command flags

| Flag | Description |
| --- | --- |
| `-n, --days <number>` | Days of history (default: 7) |
| `-d, --date <date>` | Specific date (YYYY-MM-DD) |
| `-s, --start <date>` | Range start date |
| `-e, --end <date>` | Range end date |
| `-l, --limit <n>` | Max results per page (max: 25) |
| `-a, --all` | Fetch all pages |
| `-p, --pretty` | Human-readable output |

### Analysis command flags

| Flag | Applies to | Description |
| --- | --- | --- |
| `-n, --days <number>` | all analysis | Days of history (default: 7) |
| `-c, --color` | summary | Color-coded status indicators |
| `--json` | dashboard, trends, insights | Raw JSON output |

### Chart flags

| Flag | Description |
| --- | --- |
| `-n, --days <number>` | Days of history to chart (default: 7) |
| `-o, --output <file>` | Save HTML to file instead of opening browser |

### Global flags (combine data types)

| Flag | Description |
| --- | --- |
| `--sleep` | Include sleep data |
| `--recovery` | Include recovery data |
| `--workout` | Include workout data |
| `--cycle` | Include cycle data |
| `--profile` | Include profile |
| `--body` | Include body measurements |

Running `whoop-sync` with no arguments fetches all data types.

## Example output

`whoop-sync summary --color`:
```
📊 7-Day Summary

🔴 Avg Recovery:  31.5%
💓 Avg HRV:       124.4ms
❤️  Avg RHR:       56.1bpm
🔴 Avg Sleep:     44.8% | 4.0h
🔥 Avg Strain:    6.8
```

`whoop-sync dashboard`:
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

`whoop-sync trends --days 7`:
```
📊 7-Day Trends

💚 Recovery: 31.5% avg (5-85) ↓
💓 HRV: 124.4ms avg (66-179) ↓
❤️ RHR: 56.1bpm avg (49-64) ↑
😴 Sleep: 44.8% avg (25-78) ↓
🛏️ Hours: 4.3h avg (1.3-7.2) ↓
🔥 Strain: 6.8 avg (4.1-16.4) ↓
```

`whoop-sync insights`:
```
💡 Insights & Recommendations

🔴 Red Recovery
   Recovery at 13% — body needs rest.
   → Prioritize rest, hydration, and sleep tonight.

🔴 Significant Sleep Debt
   You have 2.1 hours of accumulated sleep debt.
   → Try to get to bed 30-60 min earlier for the next few days.
```

`whoop-sync recovery` (default JSON):
```json
{
  "cycle_id": 1317587415,
  "sleep_id": "5d225f07-8722-4dcf-be52-528e2500bad3",
  "user_id": 30398164,
  "created_at": "2026-02-17T20:02:32.740Z",
  "updated_at": "2026-02-17T20:02:32.740Z",
  "score_state": "SCORED",
  "score": {
    "user_calibrating": false,
    "recovery_score": 13,
    "resting_heart_rate": 60,
    "hrv_rmssd_milli": 128.51297,
    "spo2_percentage": 96,
    "skin_temp_celsius": 33.145668
  }
}
```

## Token management

`whoop-sync auth status` **does not refresh tokens** — it only reports expiry.

For cron jobs or automation:

```bash
# Recommended pattern
whoop-sync auth refresh   # ensure fresh token before fetching
whoop-sync dashboard --json
```

If refresh fails with an expired refresh token, re-authenticate interactively:

```bash
whoop-sync auth login
```

## Exit codes

| Code | Meaning |
| --- | --- |
| 0 | Success |
| 1 | General error |
| 2 | Authentication error |
| 3 | Rate limit exceeded |
| 4 | Network error |

## Development

```bash
git clone https://github.com/mrkhachaturov/whoop-sync.git
cd whoop-sync
npm install

npm run dev -- dashboard    # run without building
npm run build               # compile TypeScript → dist/
npm test                    # run test suite
```

Node.js 22+ required.

## Full command reference

→ [docs/COMMANDS.md](docs/COMMANDS.md)

## Attribution

Built on top of [whoop-cli](https://github.com/xonika9/whoop-cli) by [xonika9](https://github.com/xonika9).

## License

MIT
