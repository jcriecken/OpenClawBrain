---
file_id: 50-Maintenance
description: Day-to-day operations, health checks, backup strategy, and cleanup procedures.
status: active
authoritative: true
---

# 🔧 Maintenance & Operations

> Day-to-day operations, health checks, backups, and cleanup tasks.

---

## Daily Checklist

```
□ Check that OpenClaw processes are running
□ Review CRON_STATUS.txt for any errors
□ Monitor disk space (especially workspace)
□ Check Telegram/Discord bot responsiveness
```

---

## Health Check Commands

### Process Health

```bash
# Verify all three processes are running
ps aux | grep -E 'openclaw|antigravity' | grep -v grep

# Expected output (3 processes):
# openclaw           — Core daemon
# openclaw-gateway   — HTTP gateway (port 18789)
# antigravity        — Agent runtime
```

### Gateway Health

```bash
# Check gateway is listening
ss -tlnp | grep 18789

# Test gateway (from the host or LAN)
curl -s http://192.168.0.217:18789/health
```

### Disk Space

```bash
# Workspace breakdown
du -sh ~/.openclaw/workspace/* | sort -rh | head -15

# System config size
du -sh ~/.openclaw/*/ | sort -rh

# Overall disk
df -h /home
```

### Cron Health

```bash
# Latest status
cat ~/.openclaw/workspace/CRON_STATUS.txt

# Recent log entries
tail -30 ~/.openclaw/workspace/CRON_UPDATES.md

# Check for consecutive errors
cat ~/.openclaw/cron/jobs.json | python3 -c "
import json, sys
data = json.load(sys.stdin)
for job in data['jobs']:
    errors = job.get('state', {}).get('consecutiveErrors', 0)
    status = job.get('state', {}).get('lastRunStatus', 'unknown')
    print(f'{job[\"name\"]}: {status} (errors: {errors})')
"
```

### Memory Database

```bash
# Check size
ls -lh ~/.openclaw/memory/main.sqlite

# Check integrity
sqlite3 ~/.openclaw/memory/main.sqlite "PRAGMA integrity_check;"
```

---

## Routine Maintenance

### Weekly: Trim Cron Logs

```bash
# Trim CRON_UPDATES.md to last 100 entries
tail -100 ~/.openclaw/workspace/CRON_UPDATES.md > /tmp/cron_trim.md
mv /tmp/cron_trim.md ~/.openclaw/workspace/CRON_UPDATES.md

# Clean old cron run files (older than 30 days)
find ~/.openclaw/cron/runs/ -name "*.jsonl" -mtime +30 -delete
```

### Weekly: Sync Repos

```bash
# Use the built-in sync script
bash ~/.openclaw/skills/jc-development/scripts/sync-repos.sh

# Or manually
cd ~/.openclaw/workspace/repo-jc-development && git pull
cd ~/.openclaw/workspace/repo-documentation-hub && git pull
```

### Monthly: Session Cleanup

Old conversation sessions accumulate in `~/.openclaw/agents/main/sessions/`:

```bash
# Count sessions
ls ~/.openclaw/agents/main/sessions/ | wc -l

# Remove sessions older than 60 days
find ~/.openclaw/agents/main/sessions/ -name "*.jsonl*" -mtime +60 -delete
```

### Monthly: Config Backup Cleanup

```bash
# Remove excess config backups (keep .bak only)
rm -f ~/.openclaw/openclaw.json.bak.[2-9]
```

---

## Starting and Stopping OpenClaw

### Check Running Status

```bash
ps aux | grep -E 'openclaw|antigravity' | grep -v grep
```

### Restart (Manual)

```bash
# Find and kill processes
pkill -f openclaw-gateway
pkill -f openclaw
# antigravity will terminate when openclaw stops

# Start fresh
openclaw &
```

> [!WARNING]
> Verify the correct startup command for your installation. OpenClaw may use a systemd service, a supervisor config, or a direct binary launch depending on installation method.

### Check if SystemD Managed

```bash
systemctl status openclaw 2>/dev/null
# If this returns a valid service, use:
# sudo systemctl restart openclaw
```

---

## Backup Strategy

### Critical Files to Back Up

| Priority | File/Directory | Why |
|:---|:---|:---|
| 🔴 Critical | `~/.openclaw/openclaw.json` | All configuration |
| 🔴 Critical | `~/.openclaw/workspace/credentials.env` | All API keys and tokens |
| 🔴 Critical | `~/.openclaw/agents/main/agent/auth-profiles.json` | LLM API keys |
| 🟡 Important | `~/.openclaw/memory/main.sqlite` | Persistent agent memory |
| 🟡 Important | `~/.openclaw/workspace/IDENTITY.md` | Agent identity |
| 🟡 Important | `~/.openclaw/workspace/SOUL.md` | Agent personality |
| 🟡 Important | `~/.openclaw/workspace/MEMORY.md` | Agent learned context |
| 🟢 Nice to have | `~/.openclaw/cron/jobs.json` | Cron job definitions |
| 🟢 Nice to have | `~/.openclaw/skills/` | Custom skill definitions |

### Backup Script

```bash
#!/bin/bash
BACKUP_DIR="/home/skilla/openclaw-backup/$(date +%Y-%m-%d)"
mkdir -p "$BACKUP_DIR"

# Critical
cp ~/.openclaw/openclaw.json "$BACKUP_DIR/"
cp ~/.openclaw/workspace/credentials.env "$BACKUP_DIR/"
cp ~/.openclaw/agents/main/agent/auth-profiles.json "$BACKUP_DIR/"

# Important
cp ~/.openclaw/memory/main.sqlite "$BACKUP_DIR/"
cp ~/.openclaw/workspace/IDENTITY.md "$BACKUP_DIR/"
cp ~/.openclaw/workspace/SOUL.md "$BACKUP_DIR/"
cp ~/.openclaw/workspace/MEMORY.md "$BACKUP_DIR/"

# Config
cp ~/.openclaw/cron/jobs.json "$BACKUP_DIR/"
cp -r ~/.openclaw/skills/ "$BACKUP_DIR/skills/"

echo "Backup complete: $BACKUP_DIR"
ls -la "$BACKUP_DIR"
```

---

## Monitoring Homelab

Jarvis monitors the Unraid homelab server (`192.168.0.140`) via cron:

| Service | Port | Expected State |
|:---|:---|:---|
| **Server (ping)** | ICMP | ✅ Reachable |
| **FileFlows** | `5000` | ⚠️ Currently closed |
| **Dev Server** | `3000` | ⚠️ Currently closed |

> [!NOTE]
> FileFlows and the dev server ports are frequently closed when not in active use. This is normal. Jarvis follows Rule #5: **don't message Carlos unless status changes**.
