---
file_id: 40-Cron-Automation
description: Scheduled jobs, delivery channels, output files, and monitoring automation.
status: active
authoritative: true
updated: 2026-03-21
---

# ⏰ Cron & Automation

> Scheduled jobs and monitoring automation. Overhauled 2026-03-21 — switched from hourly to daily.

---

## Current Jobs

### Job 1: Daily Morning Status Check

| Property | Value |
|:---|:---|
| **ID** | `8253975e-da70-4a24-ac21-3e415eb023d5` |
| **Schedule** | Daily at 08:30 (Europe/Berlin) |
| **Expression** | `30 8 * * *` |
| **Model** | `google/gemini-flash-latest` |
| **Delivery** | `announce` via Telegram |
| **Status** | ✅ Enabled |

**What it does:**
1. Pings homelab server (`192.168.0.140`) on key ports:
   - Plex (32400), Home Assistant (8123), FileFlows (5000), Dev Server (3000)
2. Compares to yesterday's status
3. Only messages Carlos if something changed

### Job 2: Daily Repo Sync

| Property | Value |
|:---|:---|
| **ID** | `cecfa0ac-5643-4903-8084-d7d5a1f18728` |
| **Schedule** | Daily at 08:30 (Europe/Berlin) |
| **Expression** | `30 8 * * *` |
| **Model** | `google/gemini-flash-latest` |
| **Delivery** | `silent` |
| **Status** | ✅ Enabled |

**What it does:**
1. Runs `sync-repos.sh` from the jc-development skill
2. Pulls latest changes for development repos

---

## Removed Jobs (2026-03-21 Overhaul)

| Job | Reason |
|:---|:---|
| Hourly Carlos Status Update | Replaced by daily check — was burning tokens 24x/day |
| WebDev Persona Hourly Brainstorm | Spam — generated ideas nobody read |
| Daily-Model-Reset | Obsolete — model config now Flash-only, no resets needed |
| The Architect - Delayed Suggestion | Stale one-shot, had errors |

---

## Output Files

| File | Purpose |
|:---|:---|
| `CRON_UPDATES.md` | Append-only log of daily status results |
| `HEARTBEAT.md` | Task checklist that cron jobs reference |

---

## Maintenance

### Truncating CRON_UPDATES.md

```bash
tail -50 ~/.openclaw/workspace/CRON_UPDATES.md > /tmp/cron_trimmed.md
mv /tmp/cron_trimmed.md ~/.openclaw/workspace/CRON_UPDATES.md
```

### Cleaning Old Run Logs

```bash
find ~/.openclaw/cron/runs/ -name "*.jsonl" -mtime +30 -delete
```

### Disabling a Job

Edit `~/.openclaw/cron/jobs.json` and set `"enabled": false`.

> [!WARNING]
> Back up `jobs.json` before editing. OpenClaw validates this file at startup and may reset invalid entries.
