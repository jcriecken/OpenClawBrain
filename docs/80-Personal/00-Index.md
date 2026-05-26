---
file_id: 80-Personal/00-Index
description: Index for the personal knowledge base. What lives where.
status: active
authoritative: true
updated: 2026-05-26
---

# 🪪 Personal Knowledge Base — Index

> **Purpose.** Build a long-lived, structured picture of Carlos as a person —
> not just a Hermes user. Anything I learn about him in conversation that
> would still matter in a year goes here.
>
> **Loaded on demand**, not every turn. Memory tool stays small; this is the
> deep archive.

## Files

| File | What goes there |
|:---|:---|
| `01-Identity.md` | Name, age, location, family, relationship status, languages, pets |
| `02-Work-Career.md` | Job, employer, freelancing, income context, professional history, ambitions |
| `03-Health-Lifestyle.md` | Sleep, exercise, diet, mental health, habits, sport, hobbies |
| `04-People.md` | Friends, family members, colleagues, anyone he mentions by name |
| `05-Tastes.md` | Music, film, books, food, design preferences, things he hates |
| `06-Values-Goals.md` | What he's working toward, what he believes, what he's anxious about |
| `07-Routines-Calendar.md` | Weekly rhythms, recurring events, regular meetings, travel |
| `08-Finance-Logistics.md` | Banks, subscriptions, recurring bills (no credentials — pointers only) |
| `09-Tech-Devices.md` | Hardware he owns, OS preferences, daily-driver apps |

## Rules for entries

1. **Date-stamp every fact.** Format: `(YYYY-MM-DD)` at end of line. Lets us
   spot stale info and rebuild trust over time.
2. **Source-tag when non-obvious.** `(2026-05-26, told me directly)`,
   `(2026-05-26, inferred from session)`, `(2026-05-26, asked & confirmed)`.
3. **Plain prose with bullets, not tables.** Future-me re-reads these as
   context — bullets are easier to skim than markdown tables.
4. **Never delete — supersede.** If something changes, add a new line and
   strikethrough the old one with the date. History is the point.
5. **Don't make stuff up.** If I'm not sure, write it to
   `89-Open-Questions.md` instead and ask next time.
6. **Privacy.** Keep this in the `_brain/` git repo (private). Never paste
   into prompts going to external services beyond Hermes' own LLM calls.

## Dream protocol

A nightly cron job (`nightly-dream`, 03:00) reads the previous day's session
history, extracts personal facts, and appends them here. See the
`personal-knowledge-curator` skill for the protocol.

Also see: `89-Open-Questions.md` for the running list of things I want to
ask Carlos to fill in gaps.
