---
file_id: 40-Cron-Automation
description: Scheduled jobs, delivery channels, output files, and monitoring automation.
status: active
authoritative: true
updated: 2026-05-26
---

# ⏰ Cron & Automation

> Scheduled jobs and monitoring automation. Rebuilt 2026-05-26 around the
> token-efficient watchdog pattern (silent on green, message on red).

## Design principles (carry-over from cleanup pass)

1. **`no_agent=true` for binary checks** — disk space, service up/down. Zero LLM tokens per tick.
2. **Restrict `enabled_toolsets`** on LLM-driven jobs to the minimum.
3. **Pre-process in the `script:` field** before the LLM sees data. Keep stdout small.
4. **Daily, not hourly.** Almost nothing is real-time.
5. **One job per concern.** Easier to pause/fix one without re-running the others.

## Current Jobs

| Name | Schedule | Mode | Script | Toolsets | Cost/tick |
|:---|:---|:---|:---|:---|:---|
| `homelab-disk-watchdog` | `0 8 * * *` | no_agent | `disk_watchdog.sh` | — | **0 tokens** (silent on green) |
| `homelab-arr-health` | `5 8 * * *` | no_agent | `arr_health.sh` | — | **0 tokens** (silent on green) |
| `lean-stream-weekly` | `0 9 * * 0` | LLM (small) | `lean_stream_scan.sh` | terminal, file | ~$0.01 |
| `morning-briefing` | `30 7 * * *` | LLM (small) | `briefing_data.sh` | terminal | ~$0.005 |

Estimated total monthly cost: **~$0.20** at Opus pricing.

## Scripts

All scripts live under `~/.hermes/scripts/` and source secrets from
`~/.openclaw/workspace/credentials.env` (only well-formed `KEY=VALUE` lines —
the credentials file has narrative noise that breaks naive `source`).

- `disk_watchdog.sh` — disk %>=85 on local mounts + reachability of
  `192.168.0.140` (Unraid) and `192.168.0.35` (HA). Silent unless red.
- `arr_health.sh` — Plex `/identity`, Sonarr/Radarr `/system/status`, Sonarr
  queue stuck items, Prowlarr disabled indexers. Silent unless red.
- `lean_stream_scan.sh` — Radarr movies >50GB, weekly diff vs
  `~/.hermes/state/lean-stream-last.json`. Output is the LLM prompt context.
- `briefing_data.sh` — wttr.in Bonn + Sonarr last-12h grabs. Output is the
  LLM prompt context.

## Adding/pausing

```bash
# List
hermes cron list
# or via the cronjob tool inside a session: action='list'

# Pause a noisy job
cronjob action='pause' job_id='<id>'

# Remove
cronjob action='remove' job_id='<id>'
```

## Pitfalls

- **`source credentials.env` directly will break** — it has a `JSON = {...}` line.
  All current scripts use the grep-and-export pattern. New scripts should too.
- **API keys missing** — Sonarr/Radarr/Prowlarr API keys are NOT in credentials.env
  yet. Add `SONARR_API_KEY`, `RADARR_API_KEY`, `PROWLARR_API_KEY` to enable the
  full health checks and the lean-stream weekly report. Scripts degrade
  gracefully without them.
- **`no_agent=true` jobs fire alerts only on non-empty stdout.** If you change a
  watchdog and accidentally print on green, you'll spam every tick. Smoke-test
  with `bash script.sh` and confirm empty output before saving.

## Retired jobs (history)

- 2026-05-26 → present: 4 jobs wired (see table above). Live smoke-test
  uncovered three pitfalls now codified in the scripts:
  1. `source credentials.env` breaks on the JSON line — use a `grep | export` loop.
  2. Inline Python in `curl | python3 -c '...'` mustn't use f-string escaped quotes — the shell mangles them. Use `.format()` or extract variables first.
  3. For Radarr `/api/v3/movie` (~1MB JSON) and other large payloads, pass via a temp file. ARG_MAX kills env-var transport; `python3 - <<PY` consumes the here-doc as stdin so piping doesn't work either.
- 2026-03-21 → 2026-04-19: "Daily Morning Status Check" and "Daily Repo Sync"
  on Flash. Replaced by the watchdog-pattern set above.
- Pre-2026-03-21: 5 hourly jobs (overhauled into 2 daily for cost).
