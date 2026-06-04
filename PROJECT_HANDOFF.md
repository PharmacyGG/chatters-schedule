# Chatters Schedule — Project Handoff

> Read this first. This document brings a new Claude instance up to speed on the entire Chatters Schedule project. Everything else flows from here.

---

## 1. What this project is

A staffing and scheduling system for a 24/7 chat team that covers multiple OnlyFans-style models. The schedule is hosted as a self-contained web dashboard on GitHub Pages, with separate planning documents for capacity decisions and printed monthly schedules.

**Owner:** Wright (devanwright1@gmail.com)
**Repo:** https://github.com/PharmacyGG/chatters-schedule
**Live dashboard:** https://pharmacygg.github.io/chatters-schedule/
**Timezone:** PHT (UTC+8) for employees, MST (UTC-7) for owner view

---

## 2. Current state of the team

### Live team (as of May 28, 2026 — what's actually on the dashboard)

The dashboard right now has 6 chatters + 2 floaters, organized in two 3-person teams:

| Chatter | Shift (PHT) | Off Day | Team | Covered By |
|---|---|---|---|---|
| Moreno | 4pm–12am | Sunday | 1 | Floater 2 (after 5/28) |
| Aji | 12am–8am | Tuesday | 1 | Floater 2 (after 5/28) |
| Ace | 8am–4pm | Thursday | 1 | Joy (after 5/28) |
| Mary Anne | 12am–8am | Wednesday | 2 | Floater 2 (after 5/28) |
| Hershe | 8am–4pm | Friday | 2 | Joy (after 5/28) |
| Carlo *(chatter)* | 4pm–12am | Saturday | 2 | Joy |

Floaters: **Joy** (yellow, Team 2, started 5/28) and **Floater 2** (slate, Team 1, started 5/28, placeholder name). Original floater **Carlo** ended 5/27 when he moved to a full-time chatter slot on Team 2.

Chatter 3 (id `jan`) was end-dated 2026-05-27 when Carlo took over that slot.

### Planned 9-chatter expansion (what the staffing plan + June calendar use)

The next phase moves to 9 full-time chatters covering 4 models, with no floaters. Joy becomes a regular chatter (not a floater) in this model.

**9 chatters total:**

| Chatter | Shift (PHT) | Off Day | Color |
|---|---|---|---|
| Aji | 12am–8am | Tuesday | Blue (#1F4D7A) |
| Mary | 12am–8am | Wednesday | Teal (#0E5454) |
| Nics | 12am–8am | Friday | Orange (#E65100) |
| Ace | 8am–4pm | Thursday | Cyan (#00838F) |
| Hershe | 8am–4pm | Friday | Magenta-Purple (#8E24AA) |
| Joy | 8am–4pm | Saturday | Mint Green (#1B5E20) |
| Moreno | 4pm–12am | Sunday | Purple (#4A148C) |
| Carlo | 4pm–12am | Saturday | Pink (#A1185A) |
| Yastine | 4pm–12am | Monday | Slate (#3A4754) |

Mary = "Mary Anne" on the live site, just shortened on documents.

---

## 3. The 4 models

| Model | Traffic | Primary handlers |
|---|---|---|
| **Kay** | High | Aji, Ace, Moreno |
| **Charlie** | High | Mary, Hershe, Carlo |
| **Alexis** | Low | Nics, Joy, Yastine (paired with Sienna) |
| **Sienna** | New / lower | Nics, Joy, Yastine (paired with Alexis) |

> Note: "Charlie" is a model name AND a chatter name (different people). Earlier we tried using "Team Charlie" as a group label and it got confusing, so team labels were eventually removed entirely from documents. Don't reintroduce team names without confirming with Wright.

---

## 4. Coverage rules (9-chatter plan)

**Normal day (3 chatters on a shift):**
- Aji / Ace / Moreno → Kay
- Mary / Hershe / Carlo → Charlie
- Nics / Joy / Yastine → Alexis + Sienna (always dual)

**Coverage day (one chatter on a shift is off):**
- The same-shift colleague picks up the missing model
- Result: that chatter handles 2 models for the day
- Wright's people each have 2 coverage days per week (predictable, fixed)

**Sick day / emergency:**
- A single chatter can flex to 3 models for one shift
- Used rarely; the absent chatter's normal coverer takes 2 models, but with 2 people out the remaining 1 covers all 4 (or merges low-traffic with high-traffic if possible)

**Vacation days:**
- The vacationing chatter shows as "Name Vacation" in red
- Their same-shift colleague picks up their model (treated like a coverage day)
- For Team Wind chatters (Nics/Joy/Yastine), Alexis+Sienna is "split" between other Wind chatters

---

## 5. File map

All files live in `C:\Users\devan\OneDrive\Documents\Claude\Projects\Chatters Schedule\`

### Live deliverables (synced to GitHub)

| File | What it is |
|---|---|
| `website-v2/index.html` | Self-contained dashboard (HTML+CSS+JS in one file). Copy to repo root as `index.html` to deploy. |
| `website-v2/data.json` | Schedule data — chatters, floaters, overrides. The source of truth. |
| `website-v2/README.md` | Publishing workflow notes |

The repo root (PharmacyGG/chatters-schedule) contains the deployed versions of `index.html` and `data.json` — those are what GitHub Pages serves.

### Planning documents

| File | What it is |
|---|---|
| `Chatters_Staffing_Plan_9Person.docx` | Formal staffing plan for the 9-chatter expansion. Rosters, weekly grid, coverage check, escalation ladder. |
| `June_2026_Schedule.docx` | Day-by-day June 2026 calendar across 3 landscape pages. Each chatter color-coded; shows vacations, off-days, model assignments. |

### Generator scripts (in workspace outputs, regenerate the docx files)

- `outputs/staffing_final.js` — generates the staffing plan
- `outputs/june_final.js` — generates the June calendar (most up-to-date scripts)
- `outputs/data.json` and other intermediate files

---

## 6. Live dashboard features

The HTML dashboard supports:
- PHT and MST split-view calendars
- "Split per team" vs "Combined" layout toggle
- Manage Team modal (add/remove chatters and floaters, edit shifts/off-days)
- Day-edit modal (click any cell to override status)
- Floater coverage logic with `covered_by_changes` (temporal handoff between floaters)
- Auto-loads `data.json` from the repo on page load
- Local storage caches edits; user clicks "Download data.json" to export changes, then uploads to GitHub

Key technical notes:
- `chatters_schedule_v5` is the localStorage key
- 3-month navigation cap (no scrolling past current + 3 months)
- Floaters have `start_date`/`end_date` for temporal activation
- Chatters have `covered_by_changes: [{ from_date, floater_id }]` for floater handoffs

---

## 7. June 2026 calendar — overlays in effect

The June calendar has two hard-coded overrides baked into the generator script:

**Vacations:**
- Ace: June 1, June 2, June 21

**Make-up day:**
- June 4 (Thu — normally Ace's off day): Ace works Kay instead; Hershe doesn't pick up Kay

**Day moves:**
- June 1: Yastine works 8am–4pm (Joy's shift) covering Alexis+Sienna; Joy is off

**Total scheduled hours for June 2026: 1,840** (or 1,864 if Ace's 3 vacation days count as PTO)

---

## 8. Publishing workflow

When `data.json` changes:
1. Edit in the dashboard (Manage team / day cells) OR edit data.json directly
2. Click "Download data.json" in the dashboard header
3. Upload the new `data.json` to the GitHub repo root (overwrites the existing one)
4. Commit changes → GitHub Pages redeploys in ~1 minute

When `index.html` changes (rare):
1. Edit the local `website-v2/index.html`
2. Upload to GitHub via web editor — use UTF-8-safe paste (CodeMirror 6 needs Ctrl+A then dispatched paste event, not document.execCommand)
3. Watch for the closing `</script></body></html>` — easy to truncate

Past gotchas:
- UTF-8 mojibake when pasting through atob() — fix by using TextDecoder('utf-8').decode(Uint8Array.from(atob(b64), c => c.charCodeAt(0)))
- File got truncated once at line 906 (missing autoload IIFE) — make sure full file is pasted

---

## 9. Color reference (9-chatter plan)

Colors are per-chatter — same color wherever that chatter appears regardless of which model they're covering. This was a deliberate decision so each person can spot their own days at a glance.

```
Aji      #1F4D7A  blue
Mary     #0E5454  teal
Nics     #E65100  vivid orange
Ace      #00838F  vivid cyan
Hershe   #8E24AA  vivid magenta-purple
Joy      #1B5E20  mint green
Moreno   #4A148C  purple
Carlo    #A1185A  pink
Yastine  #3A4754  slate
Vacation #B33333  red
Off-day  #AAAAAA  gray italic
```

Earlier iterations:
- Hershe was coral (#A6391A), changed because too red — would clash with vacation
- Nics/Ace/Hershe were all warm earth tones at first, changed to spread across the color wheel
- Team labels (Fire/Water/Wind, Phoenix/Titan/Nova, Alpha/Bravo/Charlie) were tried and abandoned — Wright prefers no team labels on documents

---

## 10. Recent decisions and reasoning

- **9 chatters over 15**: Wright chose the leaner 9-person plan despite it having zero slack and forcing some 2-model days, because it requires fewer hires (3 instead of 9 new people). Acceptable trade-off given Ace's other 6 chatters already cover Teams 1 and 2.
- **Charlie moved from floater to chatter**: After scaling discussion, Carlo (the original floater, name reused) took over Chatter 3's 4pm–12am slot starting 5/28. Confusing because the model "Charlie" exists too.
- **Joy was a floater, becomes a chatter**: In the live state Joy is still a floater (started 5/28). In the 9-chatter plan she becomes a full-time chatter on the 8am–4pm shift. The transition hasn't been pushed to the live site yet.
- **Fixed shifts only**: Every chatter has ONE shift, every day. Never rotate graveyard/morning/evening. This was a hard requirement.
- **3-model emergency cap**: Sick-day flex allows one chatter to handle 3 models for a single shift, but only as a rare emergency.
- **Make-up days vs vacation banking**: Ace gave back one Thursday off (June 4) to recover one of his 3 vacation days. Not a generalized policy yet — was a one-off ask.

---

## 11. Outstanding / not yet done

- The 9-chatter plan is **planning only** — not pushed to the live dashboard. The live site still uses the 6-chatter + 2-floater structure with `covered_by_changes` for the May 28 transition.
- 3 new hires needed for the 9-chatter rollout: **Nics** (12am–8am), **Joy** (8am–4pm — already exists as a floater, would convert), **Yastine** (4pm–12am).
- The placeholder name **"Floater 2"** in the live dashboard hasn't been replaced with a real name.
- No July/August/etc. calendars built yet — only June 2026.
- The staffing plan document still references Team labels in a few places (verify and clean if Wright wants).

---

## 12. How to use this handoff

On a new computer:
1. Sign into OneDrive with the same account — the `Chatters Schedule` folder appears
2. Open Claude, point it at this folder
3. Say: *"Read PROJECT_HANDOFF.md in this folder, then we'll continue from where I left off"*
4. The new Claude will pick up the full project context

For ongoing edits, the dashboard at https://pharmacygg.github.io/chatters-schedule/ is always the live source of truth — the JSON it loads from the repo is what's actually deployed.

---

*Last updated: June 4, 2026. Maintained by Claude across sessions.*
