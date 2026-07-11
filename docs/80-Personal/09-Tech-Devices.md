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
  EZ 26.06.2008, VIN `WF0HXXGAJH8K58112`. Ummeldung to Bonn ~06.07.2026.
  Current license plate: `BN JC1015` (was `RE-GK 5009` under previous owner).
  1.3L petrol (E10), 51 kW (~69 PS), manual 5-speed. **Steuerkette**
  (timing chain, confirmed by Carlos 2026-07-06).
  Version: 5BDBKC. EG-Typgenehmigung: e1\*98/14\*0191\*18 (01.08.2007).
  Emissions: EURO 4. Power: 51 kW @ 5,600 rpm. Tire size: 175/65 R14.
  (2026-07-08, told me directly)
  Bought 30.05.2026 from Lutz & Gudrun Kühnel (Oer-Erkenschwick) for 3.000 €.
  Mileage at purchase: 36.301 km. 1 owner (seller). TÜV clean through
  06.2027 (passed re-inspection 14.07.2025 after brake/light repairs).
  Insured (HP + VK) through 31.12.2026. Green card valid until 21.02.2027.
  Includes 2 keys, winter + summer tyres, battery charger. (2026-06-14)
- **Vehicle insurance provider:** Provinzial (HP + VK). Carlos coordinates
  this with his father (Papa). (2026-06-18, told me directly)
- **DIY maintenance project (active 2026-07):** Carlos researched and ordered
  a full tool + parts kit for a major service on the Fiesta. Total: ~680 €
  (Werkzeug ~439 €, Verbrauchsmaterial ~171 €, Ersatzteile ~70 €).
  Scope: gearbox seal (Simmerring) replacement + Getriebeöl
  change (Castrol Transmax 75W-90, ~2.3L), motor oil + filter (Liqui Moly
  5W-30, 4.35L), air + cabin filter, seat rail thread re-cut (M-Gewindebohrer),
  underbody wax (Dinitrol 4941). Gearbox is iB5 manual. ~~Simmerring not yet
  ordered as of 2026-07-06~~ **Simmerring ordered 2026-07-08** — Carlos
  confirmed "habe nun alles bestellt". Full inventory tracked at
  `~/.openclaw/workspace/fiesta-inventory.md` (21 items, categorized). Full
  task list with torque specs (Achsmutter ~230-250 Nm) saved in session.
  Facility docs now on hand: Haynes online for his Fiesta
  (`https://mole.haynes.com/manualOverview?chapterId=c_2135&sectionId=s_50080`)
  — note: behind a paid login wall, cannot be bulk-extracted offline like
  the Ford manual. Local "So wird's gemacht" Band 143 PDF (Fiesta 3/02–8/08).
  **Ford owner's manual fully extracted** (2026-07-10): 147 chapters from
  fordservicecontent.com, ~19MB, stored in jc-development repo at
  `data/garage/fiesta/manual/ford-owner-manual-gbr/` (Markdown + index,
  no raw HTML). Verified: 4.35L oil + WSS-M2C913-B spec present. Claude Code
  is currently building the repair To-Do app in the JC-development codebase;
  Jarvis stays in product/context support, no coding. Known tool gap: no
  recorded spark-plug socket. (2026-07-06 / updated 2026-07-10, told me directly)
- **Fiesta repair scope — right-hand driveshaft:** The specific repair is on
  the **right-hand (Beifahrerseite / passenger side)** driveshaft — the
  longer shaft with Zwischenlager at the engine block. The seal being
  replaced is the **Getriebeausgangs-Dichtring** (Wellendichtring am
  Differential), not the Radgelenk-Manschette. Confirmed by Carlos
  2026-07-10. (2026-07-10, told me directly)
- **Local AI inference stack:** Ollama running on the Hermes box at
  `192.168.0.147:11434`. Models available (as of 2026-06-24):
  - `qwen2.5-coder:7b` (~4.7 GB, coding)
  - `gemma4:26b` (~17 GB, general)
  - `glm-4.7-flash:latest` (~19 GB, general)
  Earlier model list (qwen3.6-27b, qwen3-coder-next) has been rotated.
  (2026-06-24, inferred from session)
- **Dev setup (multi-machine):** Carlos writes code on a separate machine
  (not the Hermes box) using **VS Code** as his editor. Connects back to the
  Hermes/Ollama box remotely over LAN. Recommended path: Continue.dev
  extension pointing at the remote Ollama endpoint. (2026-06-24,
  inferred from session)
- **GPU:** NVIDIA GeForce RTX 3080 (10 GB VRAM). Driver 595.84, CUDA 13.2.
  Ollama 0.20.4 as systemd service. (2026-06-20, inferred from session)
- **GPU constraint:** ~10 GB VRAM limits local inference to 1 concurrent
  subagent at a time to avoid OOM crashes. (2026-06-20, inferred from session)
- **Search tool:** Self-hosted SearxNG instance on `localhost:8080`, used as
  the `browser-use` start page instead of Google. (2026-06-20, inferred from session)
- **Download-client preference (security):** Carlos deliberately uses Usenet
  (NZBGet) over torrents because he considers Usenet "safer" — no P2P
  exposure, no peers seeing his IP. When Deluge (torrent) was added to
  Sonarr/Radarr to fix failed grabs, he asked to disable it again because
  there's no VPN configured. Torrents should stay disabled unless a VPN is
  set up. (2026-06-26, told me directly)
- **Web automation:** Uses `browser-use` via Hermes for web tasks and
  data extraction. (2026-06-20, inferred from session)
- **LG OLED TV (primary):** OLED65C43LA — the real/main living-room TV.
  An older OLED55CX9LA also appears in HA entities but is a legacy/duplicate
  entry flagged for cleanup. Has an associated Plex client. (2026-07-05,
  inferred from session)
- **Home Assistant entity count:** 272 entities. Two rooms are both offices
  (renamed Office 1 / Office 2; previously mislabeled internally as
  "Bad"/"Aussen"). (2026-07-05, inferred from session)
- **Network topology note:** The Hermes terminal shares the LAN with HA
  (`192.168.0.35`) — Carlos confirmed "you are in the same network". However
  the `browser-use` tool is cloud-based (Browserbase) and cannot reach LAN
  addresses; only the `ha_*` gateway tools (list_entities, get_state,
  call_service) reach HA, via a preconfigured API token. (2026-07-05,
  told me directly)
- **HA dashboard preferences:** Wants tabs split by type — Power / Climate /
  Devices. 7-day default view for graphs. Full Energy Dashboard with cost
  tracking. Didn't know his electricity €/kWh rate offhand (€0.35/kWh
  placeholder used, adjustable later). (2026-07-05, told me directly)

## Workshop & tool inventory

Inventory maintained by Jarvis from photos/listings Carlos provides. Last
updated: 2026-07-06. Items Carlos owns — NOT the Fiesta-project purchase
(that's tracked separately). If Carlos mentions a tool, check here first.

### Power tools

- **Parkside Akku-Schlagbohrschrauber PSBSA 20-Li B2** (20V, Schnellspannbohrfutter)
  + 2× X20V-Team Akku + 2× Ladegerät. (2026-07-06)
- **Preciva 8786D 2-in-1 SMD-Rework-Station** (Heißluft + Lötkolben). (2026-07-06)
- **Parkside Akku-Gerät mit Laser Klasse 2** (pistolenförmig, vermutl.
  IR-Thermometer). (2026-07-06)
- **Bambu Lab P1S 3D-Drucker** + Bambu Lab AMS (4 Filament-Slots). (2026-07-06)

### Measuring & testing

- **Kaiweets KM601s Digital-Multimeter** (True RMS, 10000 Counts, Auto-Range)
  + Typ-K-Temperaturfühler. (2026-07-06)
- **MixcMax digitaler Messschieber** (150mm, LCD) — 2× vorhanden. (2026-07-06)
- Spannungsprüfer / Phasenprüfer. (2026-07-06)
- Wasserwaage (klein, rot). (2026-07-06)
- Zollstock (Holz, "Bonn-Tannenbusch"). (2026-07-06)

### Wrenches & drivers

- **Ratsche / Knarre** (1/4"-Antrieb, umschaltbar) + Stecknüsse,
  Verlängerungen, Adapter. (2026-07-06)
- **Maul-/Gabelschlüssel** (Chrom-Vanadium, DIN 3110, u.a. 13mm, 19mm) —
  groß + kleiner Satz. (2026-07-06)
- **Innensechskantschlüssel-Satz** (oraner Halter) + lose Inbus
  (Kugelkopf, schwarz) + lange Inbus. (2026-07-06)
- Schraubendreher (rot + gelb). (2026-07-06)
- **Bit-Sets:** Parkside Etui (~32-tlg.), Bit-Box, Sortiments-Kassette. (2026-07-06)
- **Präzisions-Schraubendreher-Set** (blaue Falttasche "Mobile phone repair
  tools"): Alu-Handgriff, 6 Bit-Leisten (Torx/Security, PH, PZ, Hex,
  Pentalobe, Tri-Point), magnetische Schraubenmatte, Spudger, Picks. (2026-07-06)

### Pliers & cutters

- Kombizange, Seitenschneider, Kneifzange (alle rote Griffe). (2026-07-06)
- Wasserpumpenzange (schwarz). (2026-07-06)
- 2× Parkside-Zangen (rot/schwarz). (2026-07-06)
- Cuttermesser (Parkside). (2026-07-06)
- Kleine Säge (gelber Griff, PUK-Säge). (2026-07-06)

### Striking, clamping, safety

- Schlosserhammer (Prägung "S+K"). (2026-07-06)
- Rote Zwinge. (2026-07-06)
- Schutzbrille (blau). (2026-07-06)
- "Dritte Hand" (Schwanenhals, Krokodilklemmen, Acryl-Schutzscheibe). (2026-07-06)

### Drilling

- Bosch Bohrer-Set (15-tlg., grüne Kassette). (2026-07-06)
- kwb Bohrer-Set (rote Kassette). (2026-07-06)

### Storage systems

- Einhell E-Case Systemkoffer-Turm (3 Koffer auf Rollen). (2026-07-06)
- Handwerkzeug-Einsatz im PXL-Koffer (3 Fächer). (2026-07-06)
- Parkside Akkuschrauber-Koffer. (2026-07-06)
- Werkzeugkasten mit Deckelfach. (2026-07-06)
- Sortimentsboxen & Schubladen-Organizer (grün/transparent). (2026-07-06)

### Consumables on hand

- fischer Dübel (2 Pack.), Dichtungs-Sortiment (Workzone), K3000 Schleifpapier,
  tesa Klebeband (2 Rollen), WD-40 Specialist Optik-Spray, Schraubhaken/
  Ösenschrauben/Metalldübel, 4 Filament-Spulen (im AMS). (2026-07-06)

### Known gaps (for car DIY)

- Kein Drehmomentschlüssel im Bestand → kommt mit Proxxon-Satz (bestellt).
- Kein Felgenkreuz → für Radwechsel empfohlen nachzubestellen.

## To learn

- Daily driver: laptop or desktop? Brand/model?
- Phone OS (iOS / Android, model)?
- Tablet?
- TV / display he watches Plex on (relevant to direct-play vs transcode
  decisions for Lean Stream)?
- Audio setup (headphones for coding, speakers for music)?
- ~~3D printer make/model (he has a 3D-print portfolio idea).~~
  → Answered 2026-07-06: Bambu Lab P1S + AMS (see Workshop inventory above).
- Any other smart-home devices beyond Hue / HA-tracked?
