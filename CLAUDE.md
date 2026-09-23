# Plate 78 - project memory

Vanilla PWA. No framework, no backend, no build. Source files + icon PNGs + self-hosted font.

## Files

| File | Role |
|---|---|
| `index.html` | All UI, CSS, data, JS |
| `manifest.json` | PWA: `Plate 78`, standalone, theme/background `#E3CFBB` |
| `service-worker.js` | Cache `plate78-v18`; bump name after every deploy |
| `fonts/SourceSans3VF-Upright.woff2` | Source Sans 3 VF (SIL OFL); cached by SW |
| `icon-192.png` / `icon-512.png` / `icon-maskable-512.png` | Home-screen icons |
| `README.md` | GitHub Pages install notes |

Palette (sampled 3-stripe PNG): sand `#E3CFBB` / clay `#DBBF9F` / brand `#CC956B`. Interactive fills deepen to `#9A6240` for AA. Accent text `#65422A` on clay. Self-hosted Source Sans 3 (no Google Fonts). Light-only.

Edit meals → change `RECIPES` / `WEEKDAY_MEALS` near top of `<script>` in `index.html`. Keep ingredient strings matchable by `MACRO_RULES` (day kcal/protein is computed, not hand-typed). Weekly grocery rebuilds from `PLAN` (Mon–Fri only). Workout → `WORKOUT` (+ `MORNING_BLOCK` / `HOME_BAND_BLOCK` / `HOME_A` / `HOME_B` / `GYM_A` / `GYM_B`). Workout tab uses Mon–Sun day chips (`wkDaySwitch`); gym days include `homeAlt` for miss-gym swaps.

## Product

GERD-safe Indian home cooking for fat loss. 1 person. **95 kg → 80–84 kg** (track midpoint 82) by `END_DATE` (`2027-02-01`) from `START_DATE` (`2026-10-01`). **~1,450 kcal/day**, **~150 g protein**. Morning incline treadmill + home core daily; commercial gym hypertrophy **Mon/Tue/Thu/Fri** (A/B). Proteins: chicken breast, eggs, toned dahi, buttermilk (tetra), whey isolate in breakfast. Carbs: muesli bowl. Olive oil ≤ 3 level tsp/day (plate uses 1.5). No cauliflower, no lauki, no tomato/raw onion/garlic/red chili/mint. Chicken marinated with toned dahi only (no packaged marinade brands).

**Meal prep:** Sunday batch → Mon–Fri same 4 meals. Soft weekends (wake drink + breakfast bowl + one protein meal; not in grocery).

## Tabs (bottom nav)

1. **Today** — meal chips (`MEAL_SLOTS`: Breakfast / Lunch / Snack / Dinner), tap → recipe `<dialog>` with Sunday batch + day-of steps, reflux rules, optional hard-session add-ons
2. **Workout** — Mon–Sun day chips; morning incline + core + Boldfit + pull-up ladder; Gym A/B evenings Mon/Tue/Thu/Fri with Home A/B miss-gym swap; Wed/Sat home band block; Sun meal prep
3. **Grocery** — weekly buy list in shop packs from Mon–Fri `PLAN` × 5; tap to check; persist locally

## Data shape

```js
MACRO_RULES[]           // regex → kcal/protein from ingredient lines (raw weights)
RECIPES[id]             // { type, risk, name, ing[], steps[] } — B1 L1 S1 D1
ENERGY_ADDONS[id]       // hard-session only; macros shown, not in grocery
WEEKDAY_MEALS           // ["B1","L1","S1","D1"]
PLAN[Mon…Fri]           // WEEKDAY_MEALS; Sat/Sun = []
MEAL_SLOTS[]            // { id, chip, time } — Today toolbar
BUY_CATALOG[]           // match RECIPES ing lines → group/name/shop pack
GROCERY = buildWeeklyGrocery()  // { protein|veg|pantry|dairy: [{name, qtyLabel, key}] }
MORNING_BLOCK / HOME_BAND_BLOCK / HOME_A / HOME_B / GYM_A / GYM_B
WORKOUT[]               // { day, title, type, exercises, homeAlt? }
```

Sunday batch quantities in `steps[]` must equal `ing × 5` for that recipe.

## State (`safeStorage`)

Try `localStorage`, else in-memory (`memoryStore`). Device-local, no sync.

- `plate78_grocery` — `{ "group:name:qtyLabel": true }`
- `plate78_energy` — `'1'` when hard-session add-ons shown

Header: `daysLeft` to `END_DATE` + on-track weight (`START_KG - LOSS_KG * elapsed/SPAN_DAYS`).

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
- Don't keep unused `MACRO_RULES` / `BUY_CATALOG` entries for dropped recipes
