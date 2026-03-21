---
file_id: 00-Index
description: Master index and entry point for the OpenClaw Brain knowledge base. Provides the global file registry, navigation, and system-level conventions.
status: active
authoritative: true
---

# 00 — Master Index

This is the **authoritative index** and entry point for the OpenClaw Brain knowledge base. It serves as a single entry point for both human readers navigating via GitHub and AI agents ingesting the repository for context. Every document in the system is registered here.

---

## Core Directives

- This repository uses a **Dewey Decimal file naming system** (`00–99`) to categorize all knowledge.
- Every file carries **YAML frontmatter** with `file_id`, `description`, `status`, and `authoritative` fields.
- Internal cross-referencing uses `[[file_id]]` notation for relational linking.
- The `README.md` at the repo root is the **GitHub-facing entry page** and mirrors this index for human browsing.
- The `AGENT.md` at the repo root defines the **self-maintenance protocol** for the Jarvis agent.

---

## File Registry

| Range | Category | File ID | Title | Status |
| :--- | :--- | :--- | :--- | :--- |
| `00–09` | Meta & System | **[[00-Index]]** | Master Index (this file) | `active` |
| `10–19` | Architecture | **[[10-Architecture]]** | Processes, Network, LLM Providers | `active` |
| `10–19` | Architecture | **[[11-Configuration]]** | `openclaw.json` Reference | `active` |
| `10–19` | Architecture | **[[12-Workspace]]** | Workspace Directory Layout | `active` |
| `20–29` | Projects | **[[20-Projects]]** | Application Portfolio & Infrastructure | `active` |
| `30–39` | Agent | **[[30-Skills]]** | Skill System & Creation Guide | `active` |
| `30–39` | Agent | **[[31-Personas]]** | Persona Model & Soul Files | `active` |
| `40–49` | Operations | **[[40-Cron-Automation]]** | Scheduled Jobs & Monitoring | `active` |
| `40–49` | Operations | **[[41-Plugins-Integrations]]** | Telegram, Discord, Browser Automation | `active` |
| `50–59` | Runbooks | **[[50-Maintenance]]** | Health Checks, Backups, Cleanup | `active` |
| `50–59` | Runbooks | **[[51-Troubleshooting]]** | Diagnostics & Emergency Recovery | `active` |
| `80–89` | Ideas | *(reserved)* | Feature Tracker & Experiments | — |
| `90–99` | Archives | *(reserved)* | Change History & Migration Logs | — |

### Special Files (Root)

| File | Purpose |
| :--- | :--- |
| `README.md` | GitHub landing page, mirrors this index |
| `AGENT.md` | Self-maintenance protocol for Jarvis |
| `.gitignore` | Git exclusion rules |

---

## Naming Convention

```
[Group Number]-[Name].md
────────────────────────
Examples:
  00-Index.md              ← Group 00: Meta (this file)
  10-Architecture.md       ← Group 10: First architecture file
  11-Configuration.md      ← Group 11: Second architecture file
  20-Projects.md           ← Group 20: First projects file

Ranges:
  00–09 → Meta & System (index, conventions)
  10–19 → Architecture (processes, config, workspace)
  20–29 → Projects (web apps, repos, infrastructure)
  30–39 → Agent (skills, personas)
  40–49 → Operations (cron, plugins, integrations)
  50–59 → Runbooks (maintenance, troubleshooting)
  60–79 → Reserved
  80–89 → Ideas & Experiments
  90–99 → Archives & Change History
```

---

## Entity Overview

| Entity | Type | Status |
| :--- | :--- | :--- |
| OpenClaw Daemon | Core process | ✅ Running |
| OpenClaw Gateway (Port 18789) | HTTP API | ✅ Running |
| Antigravity Runtime | Agent runtime | ✅ Running |
| Telegram Bot | Messaging plugin | ✅ Enabled |
| Discord Bot | Messaging plugin | ✅ Enabled |
| Ollama (qwen2.5:32b) | Local LLM | ✅ Running |
| Google Gemini | Cloud LLM | ✅ Connected |
| OpenRouter (Claude) | Cloud LLM | ✅ Connected |
| Homelab (192.168.0.140) | Monitored target | ✅ Reachable |

---

## Cross-Repository Links

This knowledge base complements the [documenationHub](https://github.com/jcriecken/documenationHub):

| This Repo (`OpenClawBrain`) | documenationHub |
| :--- | :--- |
| How the **agent platform** works | How the **web projects** work |
| OpenClaw config, skills, personas | Tech stacks, modules, hosting |
| Operations & maintenance | Business registration, infrastructure |
| Agent-facing documentation | Developer-facing documentation |

---

## Open Questions / TODOs

- [ ] Add `80-Ideas.md` for experimental features and planned improvements
- [ ] Add `90-Changelog.md` for tracking major instance changes over time
- [ ] Determine sync frequency for OpenClaw workspace pull
