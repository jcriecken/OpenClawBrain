---
file_id: 80-Personal/09-Tech-Devices
status: active
updated: 2026-06-14
---

# Tech & devices

## Known

- **Main workstation:** Linux box (6.17.0-29-generic, Ubuntu-flavoured per
  `/home/skilla`). User `skilla`. Python 3.11 venv via Hermes. (2026-05-26)
- **Homelab server (JC):** Intel Core i7-9700 @ 3.00GHz, 32GB RAM, Unraid
  (~30TB array), IP `192.168.0.140`, Tailscale installed. (2026-05-26)
- **Home Assistant VM:** 32GB vDisk (20GB allocated), `192.168.0.35`. (2026-05-26)
- **Container stack:** Binhex Plex, Sonarr, Radarr (+ Radarr-german), Lidarr,
  Prowlarr, deluge-vpn, nzbget-vpn, FileFlows. (2026-05-26)
- **Dev workstation IP (when active):** `192.168.0.217:3000`. (2026-05-26)
- **Gaming setup:** Sim racing rig with steering wheel, pedals, RGB accents;
  dark-walled room. (2026-05-27, inferred from session)
- **Smart home:** Home Assistant controls "Office Beleuchtung" (office lighting).
  (2026-05-27, told me directly)
- **Plex client in HA:** `media_player.plex_plex_web_microsoft_edge_windows`
  registered 2026-05-27. ~~Wants cinema-mode automation for ALL Plex clients
  (future-proofed via template matching `media_player.plex_*`).~~ Superseded:
  automation implemented 2026-06-14. Dims office lights to 1% when any Plex
  client starts playing (template matches `media_player.plex_*`), restores
  previous brightness on stop/pause. Only triggers after sunset and only if
  lights were already on. (2026-05-27 / 2026-06-14, told me directly)
- **Remote access:** Accesses Hermes dashboard from other machines via SSH tunnel
  (`ssh -L 9119:127.0.0.1:9119`). (2026-05-27, told me directly)
- **Dev tool:** Uses Claude Code CLI with Opus 4.7 as default model for coding
  tasks. (2026-05-27, told me directly)
- **Vehicle:** Ford Fiesta (Typ JH1, Variant A9JA1), grey, 5-door hatchback.
  EZ 26.06.2008, VIN `WF0HXXGAJH8K58112`, license plate `RE-GK 5009`.
  1.2L petrol (E10), 51 kW (~69 PS), manual 5-speed.
  Bought 30.05.2026 from Lutz & Gudrun Kühnel (Oer-Erkenschwick) for 3.000 €.
  Mileage at purchase: 36.301 km. 1 owner (seller). TÜV clean through
  06.2027 (passed re-inspection 14.07.2025 after brake/light repairs).
  Insured (HP + VK) through 31.12.2026. Green card valid until 21.02.2027.
  Includes 2 keys, winter + summer tyres, battery charger. (2026-06-14)
- **Vehicle insurance provider:** Provinzial (HP + VK). Carlos coordinates
  this with his father (Papa). (2026-06-18, told me directly)
- **Local AI inference stack:** Ollama with two models:
  - `batiai/qwen3.6-27b:q4` (~16 GB, workhorse for general tasks)
  - `bazobehram/qwen3-coder-next:latest` (~48 GB, heavy coding tasks)
  (2026-06-20, told me directly)
- **GPU:** NVIDIA GeForce RTX 3080 (10 GB VRAM). Driver 595.84, CUDA 13.2.
  Ollama 0.20.4 as systemd service. (2026-06-20, inferred from session)
- **GPU constraint:** ~10 GB VRAM limits local inference to 1 concurrent
  subagent at a time to avoid OOM crashes. (2026-06-20, inferred from session)
- **Search tool:** Self-hosted SearxNG instance on `localhost:8080`, used as
  the `browser-use` start page instead of Google. (2026-06-20, inferred from session)
- **Web automation:** Uses `browser-use` via Hermes for web tasks and
  data extraction. (2026-06-20, inferred from session)

## To learn

- Daily driver: laptop or desktop? Brand/model?
- Phone OS (iOS / Android, model)?
- Tablet?
- TV / display he watches Plex on (relevant to direct-play vs transcode
  decisions for Lean Stream)?
- Audio setup (headphones for coding, speakers for music)?
- 3D printer make/model (he has a 3D-print portfolio idea).
- Any other smart-home devices beyond Hue / HA-tracked?
