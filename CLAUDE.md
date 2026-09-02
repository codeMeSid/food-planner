# Plate 78 - project memory

Vanilla PWA. No framework, no backend, no build. Source files + icon PNGs + self-hosted font.

## Files

| File | Role |
|---|---|
| `index.html` | All UI, CSS, data, JS |
| `manifest.json` | PWA: `Plate 78`, standalone, theme/background `#E3CFBB` |
| `service-worker.js` | Cache `plate78-v1`; bump name after every deploy |
| `fonts/SourceSans3VF-Upright.woff2` | Source Sans 3 VF (SIL OFL); cached by SW |
| `icon-192.png` / `icon-512.png` / `icon-maskable-512.png` | Home-screen icons |
| `README.md` | GitHub Pages install notes |

Palette (sampled 3-stripe PNG): sand `#E3CFBB` / clay `#DBBF9F` / brand `#CC956B`. Interactive fills deepen to `#9A6240` for AA. Self-hosted Source Sans 3 (no Google Fonts). Light-only.

Edit meals → change `PLAN` / `RECIPES` near top of `<script>` in `index.html`. Keep ingredient strings matchable by `MACRO_RULES` (day kcal/protein is computed, not hand-typed). Weekly grocery rebuilds from PLAN. Workout → `WORKOUT`.

## Product

GERD-safe Indian home cooking for fat loss. 1 person. **95 kg → 78 kg** in 120 days from `START_DATE` (`2026-09-01`). **~1,500 kcal/day**, **~150 g protein**, gym 4x/week + 8–10k steps. Proteins: chicken, eggs, toned paneer, toned dahi. Carbs (rice/atta/jowar/puttu) at the 1:30 lunch slot only. Olive oil ≤ 3 level tsp/day. No cauliflower, no lauki, no tomato/raw onion/red chili/mint. Whey only when protein is short.

## Tabs (bottom nav)

1. **Today** - weekday chips, 5 meals from `PLAN` + `SLOT_TIME`, tap meal → recipe `<dialog>`, reflux rules, optional hard-session add-ons
2. **Workout** - Mon-Sun split + GERD training notes
3. **Grocery** - weekly buy list in shop packs, derived from Mon-Sun PLAN; tap chips, persist checks

`PLAN` is 7 unique days (no Fri=Mon repeat). JS Sunday → `DAYS[6]` (`Sun`).

## Data shape

```js
MACRO_RULES[] // regex → kcal/protein from ingredient lines (raw weights)
RECIPES[id] = { type, risk: "low"|"med", name, ing[], steps[] }
PLAN[Mon…Sun] = [recipeId, …]  // 5 slots: brunch, lunch, snack, salad/soup, dinner
SLOT_TIME = {0:"11:00 AM", 1:"1:30 PM", 2:"4:00 PM", 3:"7:00 PM", 4:"8:30 PM"}
GROCERY = buildWeeklyGrocery()  // { protein|veg|pantry|dairy: [{name, qtyLabel, key}] }
WORKOUT[] = { day, title, type, exercises: [[name, sets]] }
BUY_CATALOG[] // match recipe ing lines → group/name/unit/qty
```

## State (`safeStorage`)

Try `localStorage`, else in-memory (`memoryStore`). Device-local, no sync.

- `plate78_grocery` - `{ "group:name:qtyLabel": true }`
- `plate78_energy` - `'1'` when hard-session add-ons shown

Header: `END_DATE = START_DATE + 120`, `daysLeft` + on-track weight pill (`95 - 17 * elapsed/120`).

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
- Don't hand-type day kcal in copy — change ingredients and let `MACRO_RULES` recompute
