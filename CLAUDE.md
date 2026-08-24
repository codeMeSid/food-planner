# Keto GERD Plan - project memory

Vanilla PWA. No framework, no backend, no build. Source files + icon PNGs + self-hosted font.

## Files

| File | Role |
|---|---|
| `index.html` | All UI, CSS, data, JS |
| `manifest.json` | PWA: `KetoGERD`, standalone, theme/background `#E3CFBB` |
| `service-worker.js` | Cache `keto-gerd-v6`; bump name after every deploy |
| `fonts/SourceSans3VF-Upright.woff2` | Source Sans 3 VF (SIL OFL); cached by SW |
| `icon-192.png` / `icon-512.png` / `icon-maskable-512.png` | Home-screen icons |
| `README.md` | GitHub Pages install notes |

Palette (sampled 3-stripe PNG): sand `#E3CFBB` / clay `#DBBF9F` / brand `#CC956B`. Interactive fills deepen to `#9A6240` for AA. Self-hosted Source Sans 3 (no Google Fonts). Light-only.

Edit meals → change `PLAN` / `RECIPES` near top of `<script>` in `index.html`. Weekly grocery rebuilds from PLAN. Workout → `WORKOUT`.

## Product

Chicken-only keto for GERD. 1 person. **95 kg → 78 kg** in 120 days from `START_DATE` (`2026-08-24`). ~1950 kcal/day, <45g net carbs, gym 4x/week. Calories in copy, not computed from recipes.

## Tabs (bottom nav)

1. **Today** - weekday chips, 5 meals from `PLAN` + `SLOT_TIME`, tap meal → recipe `<dialog>`, reflux rules
2. **Workout** - Mon-Sun split + GERD training notes
3. **Grocery** - weekly buy list in shop packs (bottles, kg, dozen), derived from Mon-Sun PLAN; tap chips, persist checks

`PLAN` repeats: Fri=Mon, Sat=Tue, Sun=Wed. Those days still count toward weekly qty (two days of those meals). JS Sunday → `DAYS[6]` (`Sun`).

## Data shape

```js
RECIPES[id] = { type, risk: "low"|"med", name, ing[], steps[] }
PLAN[Mon…Sun] = [recipeId, …]  // 5 slots
SLOT_TIME = {0:"7:30 AM", 1:"11:00 AM", 2:"1:30 PM", 3:"4:30 PM", 4:"7:30 PM"}
GROCERY = buildWeeklyGrocery()  // { protein|veg|pantry|dairy: [{name, qtyLabel, key}] }
WORKOUT[] = { day, title, type, exercises: [[name, sets]] }
BUY_CATALOG[] // match recipe ing lines → group/name/unit/qty
```

## State (`safeStorage`)

Try `localStorage`, else in-memory (`memoryStore`). Device-local, no sync.

- `ketogerd_grocery` - `{ "group:name:qtyLabel": true }`

Header: `END_DATE = START_DATE + 120`, `daysLeft` in pill strip.

## Patterns

- Tabs: add/remove `.active` on `<section>`, `.on` on tab button
- Re-render = rewrite `innerHTML`
- Document scroll: `html,body` auto height; sticky header; fixed tabbar; body padding clears tabbar + safe-area
- Meal modal: native `<dialog showModal>`; close via button, backdrop, Escape; restore focus
- SW: cache-first, then network + `cache.put`; `skipWaiting` + `clients.claim`
- After file edits + Pages push: bump `CACHE` in `service-worker.js` or installed clients stay stale

## Don't

- Don't add a bundler unless asked
- Don't invent a backend / sync
- Don't move data out of `index.html` unless asked
- Don't skip `CACHE` bump on shipped HTML/CSS/JS/asset changes
- Don't load fonts from a CDN (breaks offline)
