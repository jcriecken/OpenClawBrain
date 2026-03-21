---
file_id: 11-Configuration
description: Complete reference for openclaw.json � every config key explained with examples and safety notes.
status: active
authoritative: true
---

# ⚙️ Configuration Reference

> Complete reference for `~/.openclaw/openclaw.json` — the master configuration file.

---

## Overview

The `openclaw.json` file controls **every aspect** of the OpenClaw instance. It is read at startup and can be modified while running (some changes require a restart).

> [!IMPORTANT]
> OpenClaw automatically creates backup copies (`openclaw.json.bak`, `.bak.1`, etc.) when changes are made. Up to 4 backups are kept.

---

## Top-Level Structure

```json
{
  "meta": { ... },
  "wizard": { ... },
  "gateway": { ... },
  "skills": { ... },
  "plugins": { ... }
}
```

---

## Section Reference

### `meta`

Tracks version and last modification.

| Key | Type | Description |
|:---|:---|:---|
| `lastTouchedVersion` | `string` | OpenClaw version that last wrote this file |
| `lastTouchedAt` | `ISO 8601` | Timestamp of last modification |

### `wizard`

Tracks setup wizard state.

| Key | Type | Description |
|:---|:---|:---|
| `lastRunAt` | `ISO 8601` | When the wizard last ran |
| `lastRunVersion` | `string` | Version that ran the wizard |
| `lastRunCommand` | `string` | Last wizard command (e.g., `"configure"`) |
| `headless` | `boolean` | If `true`, runs without GUI prompts |

### `gateway`

Controls the HTTP gateway for external access.

| Key | Type | Default | Description |
|:---|:---|:---|:---|
| `port` | `number` | `18789` | Gateway listen port |
| `mode` | `string` | `"local"` | `"local"` or `"public"` |
| `bind` | `string` | `"auto"` | Network interface to bind to |
| `auth.mode` | `string` | `"token"` | Authentication method |
| `auth.token` | `string` | — | Bearer token for API access |

<details>
<summary><b>Example: Gateway Configuration</b></summary>

```json
{
  "gateway": {
    "port": 18789,
    "mode": "local",
    "bind": "auto",
    "auth": {
      "mode": "token",
      "token": "your-secret-token-here"
    },
    "tailscale": {
      "mode": "off",
      "resetOnExit": false
    },
    "nodes": {
      "denyCommands": [
        "camera.snap",
        "camera.clip",
        "screen.record"
      ]
    }
  }
}
```

</details>

### `skills`

Manages external skill integrations (e.g., third-party skill APIs).

| Key | Path | Description |
|:---|:---|:---|
| `entries.<name>.apiKey` | `skills.entries.nano-banana-pro.apiKey` | API key for the skill |
| `entries.<name>.enabled` | `skills.entries.spotify.enabled` | Enable/disable toggle |

> [!NOTE]
> This section is for **external/marketplace skills**, not the local skills in `~/.openclaw/skills/`. Local skills are managed via the file system. See [Skills System](skills.md).

### `plugins`

Controls messaging platform integrations.

| Plugin | Status | Description |
|:---|:---|:---|
| `telegram` | ✅ Enabled | Primary communication channel |
| `discord` | ✅ Enabled | Secondary channel |
| `whatsapp` | ❌ Disabled | Available but not configured |
| `google-gemini-cli-auth` | ✅ Enabled | Google Gemini CLI authentication |

---

## Agent-Level Config

### `~/.openclaw/agents/main/agent/models.json`

Defines available LLM models and their capabilities:

```json
{
  "providers": {
    "ollama": {
      "baseUrl": "http://127.0.0.1:11434/v1",
      "api": "ollama",
      "models": [
        {
          "id": "qwen2.5:32b",
          "name": "qwen2.5:32b",
          "reasoning": false,
          "input": ["text"]
        }
      ]
    }
  }
}
```

### `~/.openclaw/agents/main/agent/auth-profiles.json`

Stores API key profiles for LLM providers:

| Profile | Provider | Description |
|:---|:---|:---|
| `openrouter:default` | OpenRouter | Anthropic Claude models via OpenRouter |
| `google:default` | Google | Google Gemini models |

> [!CAUTION]
> This file contains **API keys in plaintext**. It is stored outside the git-tracked workspace, but ensure file permissions are restricted (`chmod 600`).

---

## Modifying Configuration

### Safe Approach

1. **Stop OpenClaw** before editing `openclaw.json`
2. Make your changes
3. Restart OpenClaw

### Hot Reload

Some settings can be changed while running via the `configure` wizard command. Check the OpenClaw documentation for which settings support hot reload.

### Restoring from Backup

```bash
# List backups
ls -la ~/.openclaw/openclaw.json*

# Restore from backup
cp ~/.openclaw/openclaw.json.bak ~/.openclaw/openclaw.json
```
