---
file_id: AGENT
description: Self-maintenance protocol for the OpenClaw Jarvis agent. Read this file to understand how and when to update this knowledge base.
status: active
authoritative: true
---

# 🤖 Agent Self-Maintenance Protocol

> **This file is for you, Jarvis.** It tells you how to keep this knowledge base (`_brain`) accurate and up to date.

---

## What Is This Repo?

This is your **external brain** — a git-tracked knowledge base that documents how you work, what you manage, and what your operator (Carlos) has configured. It lives at:

```
~/.openclaw/workspace/_brain/
```

It is pulled from: `https://github.com/jcriecken/OpenClawBrain.git`

---

## When to Update This Repo

You should update this repo whenever you **detect a meaningful change** to the OpenClaw instance that is not yet reflected in the documentation. This includes changes made through:

- **Discord / Telegram commands** from Carlos
- **Manual config edits** to `openclaw.json`
- **New skills** added to `~/.openclaw/skills/`
- **New personas** added to `personas/`
- **Cron job changes** in `cron/jobs.json`
- **Plugin enable/disable** changes
- **New project repos** added to the workspace
- **LLM model changes** (new providers, swapped models)
- **Infrastructure changes** (new servers, port changes, DNS updates)

---

## How to Detect Changes

### Quick Diff Check

Compare the current live state against what this repo documents:

```bash
# Check current config
cat ~/.openclaw/openclaw.json

# Compare against documented state in:
# - docs/11-Configuration.md (config reference)
# - docs/41-Plugins-Integrations.md (plugins)
# - docs/10-Architecture.md (models, providers)

# Check current skills
ls ~/.openclaw/skills/

# Compare against docs/30-Skills.md

# Check current personas
ls ~/.openclaw/workspace/personas/

# Compare against docs/31-Personas.md

# Check cron jobs
cat ~/.openclaw/cron/jobs.json

# Compare against docs/40-Cron-Automation.md
```

### Change Indicators

If any of these are true, an update is needed:

| Signal | What Changed | Update Target |
|:---|:---|:---|
| New directory in `~/.openclaw/skills/` | Skill added | `docs/30-Skills.md` |
| New directory in `personas/` | Persona added | `docs/31-Personas.md` |
| `openclaw.json` has a newer `meta.lastTouchedAt` | Config changed | `docs/11-Configuration.md` |
| Plugin `enabled` status differs | Plugin toggled | `docs/41-Plugins-Integrations.md` |
| New model in `agents/main/agent/models.json` | Model added | `docs/10-Architecture.md` |
| New repo directory in workspace | Project added | `docs/20-Projects.md` |
| New cron job in `cron/jobs.json` | Job added | `docs/40-Cron-Automation.md` |

---

## How to Make Updates

### Step 1: Pull Latest

```bash
cd ~/.openclaw/workspace/_brain
git pull origin main
```

### Step 2: Edit the Relevant File

Follow these rules when editing:

1. **Preserve YAML frontmatter** — every file in `docs/` starts with a `---` YAML block. Keep it.
2. **Use the Dewey Decimal naming** — if adding a new file, pick the right range:
   - `10–19`: Architecture & Configuration
   - `20–29`: Projects
   - `30–39`: Agent (Skills, Personas)
   - `40–49`: Operations (Cron, Plugins)
   - `50–59`: Runbooks (Maintenance, Troubleshooting)
   - `80–89`: Ideas & Experiments
   - `90–99`: Archives & Change History
3. **Update the index** — if you add a new file, add it to:
   - `docs/00-Index.md` (file registry table)
   - `README.md` (Knowledge Base Index table)
4. **Be factual** — only document what IS, not what should be. Use TODOs for aspirational items.
5. **Include dates** — when documenting a change, note when it happened.

### Step 3: Commit and Push

```bash
cd ~/.openclaw/workspace/_brain
git add -A
git commit -m "docs(<scope>): <what changed>

<optional body explaining why>"
git push origin main
```

**Commit message conventions:**
- `docs(skills): add new homelab skill`
- `docs(config): update gateway port to 18790`
- `docs(projects): add whisper-sota repo link`
- `docs(cron): document new daily backup job`

---

## When NOT to Update

- **Transient state** — don't document temporary cron errors, brief outages, or in-progress experiments
- **Secrets** — never commit API keys, tokens, or passwords to this repo
- **Session-specific context** — don't document one-off conversation details
- **Speculative changes** — only document what's actually deployed and running

---

## Periodic Review Cadence

| Frequency | Action |
|:---|:---|
| **Every session** | Check if any recent changes are undocumented |
| **Weekly** | Full diff check (see "How to Detect Changes" above) |
| **After config changes** | Immediately update the relevant docs |
| **After skill/persona additions** | Immediately update `30-Skills.md` or `31-Personas.md` |

---

## File Registry Quick Reference

| File | Documents |
|:---|:---|
| `docs/10-Architecture.md` | Processes, network, LLM providers |
| `docs/11-Configuration.md` | `openclaw.json` settings |
| `docs/12-Workspace.md` | Directory layout, git workflow |
| `docs/20-Projects.md` | Web apps, repos, tech stacks |
| `docs/30-Skills.md` | Skill definitions, creation guide |
| `docs/31-Personas.md` | Persona model, soul files |
| `docs/40-Cron-Automation.md` | Scheduled jobs, delivery |
| `docs/41-Plugins-Integrations.md` | Telegram, Discord, auth |
| `docs/50-Maintenance.md` | Health checks, backups |
| `docs/51-Troubleshooting.md` | Diagnostics, recovery |

---

## Example: Documenting a Discord-Triggered Change

**Scenario:** Carlos sends a Discord message: "Enable the WhatsApp plugin."

After you enable it, update `docs/41-Plugins-Integrations.md`:

```diff
 | **WhatsApp** | ❌ Disabled | `plugins.entries.whatsapp` | Available, not configured |
+| **WhatsApp** | ✅ Enabled | `plugins.entries.whatsapp` | Messaging channel |
```

Then commit:
```bash
cd ~/.openclaw/workspace/_brain
git add docs/41-Plugins-Integrations.md
git commit -m "docs(plugins): enable WhatsApp plugin per Carlos request"
git push origin main
```

---

> [!IMPORTANT]
> **You are the primary maintainer of this repo.** Carlos will add major documentation, but day-to-day accuracy depends on you detecting and documenting changes. Treat this repo as your ground truth — if it's wrong, fix it.
