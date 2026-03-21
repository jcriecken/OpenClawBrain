---
file_id: 51-Troubleshooting
description: Common issues and solutions � diagnostics, process failures, and emergency recovery.
status: active
authoritative: true
---

# 🐛 Troubleshooting

> Common issues and their solutions for the OpenClaw instance.

---

## Quick Diagnostics

```bash
# One-liner health check
echo "=== Processes ===" && ps aux | grep -E 'openclaw|antigravity' | grep -v grep && echo "=== Gateway ===" && ss -tlnp | grep 18789 && echo "=== Disk ===" && df -h /home | tail -1 && echo "=== Last Cron ===" && cat ~/.openclaw/workspace/CRON_STATUS.txt
```

---

## Common Issues

### OpenClaw Not Running

**Symptoms:** No response on Telegram/Discord, gateway unreachable.

```bash
# Check if processes exist
ps aux | grep openclaw | grep -v grep

# If empty, check for crash logs
ls -lt ~/.openclaw/logs/ | head -5
cat ~/.openclaw/logs/<latest-log>

# Restart
openclaw &
```

> [!TIP]
> If OpenClaw crashes repeatedly, check `~/.openclaw/openclaw.json` for syntax errors. Restore from backup if needed:
> ```bash
> cp ~/.openclaw/openclaw.json.bak ~/.openclaw/openclaw.json
> ```

---

### Gateway Not Accessible

**Symptoms:** Can't reach `http://192.168.0.217:18789`

```bash
# Check if gateway process is running
ps aux | grep openclaw-gateway

# Check if port is listening
ss -tlnp | grep 18789

# Check firewall
sudo ufw status
sudo iptables -L -n | grep 18789
```

**Common fixes:**
- Restart OpenClaw
- Check `openclaw.json` → `gateway.bind` is set to `"auto"` or `"0.0.0.0"`
- Ensure the port isn't in use by another process

---

### Telegram Bot Not Responding

**Symptoms:** Messages sent but no reply.

```bash
# 1. Check the process is running
ps aux | grep openclaw | grep -v grep

# 2. Check Telegram state
cat ~/.openclaw/telegram/update-offset-default.json

# 3. Verify bot token
grep TELEGRAM_BOT_TOKEN ~/.openclaw/workspace/credentials.env

# 4. Test token manually
TOKEN=$(grep TELEGRAM_BOT_TOKEN ~/.openclaw/workspace/credentials.env | cut -d= -f2)
curl -s "https://api.telegram.org/bot${TOKEN}/getMe"
```

**Common fixes:**
- Reset update offset: `echo '{}' > ~/.openclaw/telegram/update-offset-default.json`
- Verify bot token hasn't been revoked in BotFather
- Check if OpenClaw has network access to `api.telegram.org`

> [!WARNING]
> Resetting the update offset may cause duplicate message processing for recent messages.

---

### Cron Jobs Not Executing

**Symptoms:** `CRON_UPDATES.md` not updating, stale `CRON_STATUS.txt`.

```bash
# Check job status
cat ~/.openclaw/cron/jobs.json | python3 -c "
import json, sys
data = json.load(sys.stdin)
for j in data['jobs']:
    print(f'Job: {j[\"name\"]}')
    print(f'  Enabled: {j[\"enabled\"]}')
    print(f'  Status: {j.get(\"state\",{}).get(\"lastRunStatus\",\"unknown\")}')
    print(f'  Errors: {j.get(\"state\",{}).get(\"consecutiveErrors\",0)}')
    print()
"

# Check last run logs
ls -lt ~/.openclaw/cron/runs/ | head -5
```

**Common fixes:**
- If `consecutiveErrors` > 0, check the run log for error details
- Verify `"enabled": true` in `jobs.json`
- Restart OpenClaw to reset the scheduler

---

### High Disk Usage

**Symptoms:** Disk full errors, slow performance.

```bash
# Find the culprits
du -sh ~/.openclaw/workspace/* | sort -rh | head -10
du -sh ~/.openclaw/agents/main/sessions/ 

# Common offenders:
# 1. fileflows-node (101GB if still present)
# 2. Conversation sessions
# 3. CRON_UPDATES.md
```

**Cleanup actions:**
```bash
# Remove FileFlows (if still in workspace)
rm -rf ~/.openclaw/workspace/fileflows-node
rm -rf ~/.openclaw/workspace/fileflows-node-old

# Trim cron log
tail -100 ~/.openclaw/workspace/CRON_UPDATES.md > /tmp/cron_trim.md
mv /tmp/cron_trim.md ~/.openclaw/workspace/CRON_UPDATES.md

# Clean old sessions
find ~/.openclaw/agents/main/sessions/ -name "*.jsonl*" -mtime +60 -delete
```

---

### LLM Provider Errors

**Symptoms:** Agent responses fail or timeout.

```bash
# Check provider status in auth profiles
cat ~/.openclaw/agents/main/agent/auth-profiles.json | python3 -c "
import json, sys, datetime
data = json.load(sys.stdin)
for name, stats in data.get('usageStats', {}).items():
    last_fail = stats.get('lastFailureAt', 0)
    if last_fail:
        fail_dt = datetime.datetime.fromtimestamp(last_fail/1000)
    else:
        fail_dt = 'never'
    print(f'{name}: errors={stats.get(\"errorCount\",0)}, last_fail={fail_dt}')
"
```

**Common fixes:**
- Check API key validity for the affected provider
- If Ollama is the issue: `curl http://127.0.0.1:11434/v1/models` to verify it's running
- Ensure OpenRouter/Google API credits haven't been exhausted

---

### Agent Memory Issues

**Symptoms:** Jarvis "forgets" things or doesn't recognize user context.

```bash
# Check memory database integrity
sqlite3 ~/.openclaw/memory/main.sqlite "PRAGMA integrity_check;"

# Check memory file
cat ~/.openclaw/workspace/MEMORY.md

# Check memory database size
ls -lh ~/.openclaw/memory/main.sqlite
```

**Common fixes:**
- Manually update `MEMORY.md` with missing context
- If SQLite is corrupted: restore from backup or delete (Jarvis will rebuild)

---

## Log Locations

| Log Type | Location |
|:---|:---|
| System logs | `~/.openclaw/logs/` |
| Session logs | `~/.openclaw/agents/main/sessions/` |
| Cron run logs | `~/.openclaw/cron/runs/` |
| Cron status | `~/.openclaw/workspace/CRON_UPDATES.md` |
| Gateway logs | stdout of `openclaw-gateway` process |

---

## Emergency Recovery

### Full Config Reset

```bash
# 1. Stop OpenClaw
pkill -f openclaw

# 2. Back up current state
cp -r ~/.openclaw ~/openclaw-emergency-backup-$(date +%s)

# 3. Restore from latest good backup
cp ~/openclaw-backup/<date>/openclaw.json ~/.openclaw/openclaw.json

# 4. Restart
openclaw &
```

### Credential Rotation

If credentials are compromised:

1. **Telegram:** Revoke token in BotFather → `/revoke` → Create new token
2. **Discord:** Regenerate in Discord Developer Portal → Bot → Reset Token
3. **GitHub PAT:** Revoke in GitHub Settings → Generate new token
4. **LLM API Keys:** Regenerate in respective provider dashboards

Update all new values in `~/.openclaw/workspace/credentials.env` and `~/.openclaw/agents/main/agent/auth-profiles.json`.
