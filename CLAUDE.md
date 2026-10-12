# Plate 78 - project memory

Vanilla PWA. No framework, no backend, no build. Source files + icon PNGs + self-hosted font.

## Files

| File | Role |
|---|---|
| `index.html` | All UI, CSS, data, JS |
| `exercises.json` | Exercise id → `{ name, youtubeId, skipVideo? }` for form videos |
| `manifest.json` | PWA: `Plate 78`, standalone, theme/background `#E3CFBB` |
| `service-worker.js` | Cache `plate78-v35`; bump name after every deploy |
| `fonts/SourceSans3VF-Upright.woff2` | Source Sans 3 VF (SIL OFL); cached by SW |
| `icon-192.png` / `icon-512.png` / `icon-maskable-512.png` | Home-screen icons |
| `README.md` | GitHub Pages install notes |

Palette (sampled 3-stripe PNG): sand `#E3CFBB` / clay `#DBBF9F` / brand `#CC956B`. Interactive fills deepen to `#9A6240` for AA. Accent text `#65422A` on clay. Self-hosted Source Sans 3 (no Google Fonts). Light-only.

Edit meals → change `RECIPES` / `WEEKDAY_MEALS` near top of `<script>` in `index.html`. Keep ingredient strings matchable by `MACRO_RULES` (day kcal/protein is computed, not hand-typed). Weekly grocery rebuilds from `PLAN` (Mon–Fri only). Workout → `WORKOUT` (`home` / `gym` / `alt` from `HOME_BASE` / `HOME_EXTRA` / `TREADMILL_BLOCK` / `GYM_LIFT_*` / `ALT_LIFT_*`). Day chips (`wkDaySwitch`) + Home/Gym/Alternative place chips (`wkPlaceSwitch`).

## Product

GERD-safe Indian home cooking for fat loss. 1 person. **95 kg → 85 kg** by `END_DATE` (`2027-02-15`) from `START_DATE` (`2026-10-12`). **~1,680 kcal/day**, **~134–143 g protein** (computed from ingredients). Morning incline treadmill + home core/bands daily; commercial gym lifts **Mon/Tue/Thu/Fri** (alternating lift blocks). Weekday plate: Yogabar protein smoothie · brown rice paneer pulao + salad + curd · fruit bowl + eggs (fruit rotates by day) · mild tandoori chicken + veggies + salad. Avoid tomato/raw onion/garlic/chili/mint/citrus/fried. Chicken marinated with toned dahi only.

**Meal prep:** Sunday ~45 min → marinate chicken (600 g Mon–Wed; rest Wed night), cook rice 2–3 days, boil eggs, soak moong, chop salad. Soft weekends (protein each meal, one relaxed meal; not in grocery).

## Tabs (bottom nav)

1. **Today** — meal chips (`MEAL_SLOTS`: Breakfast / Lunch / Snack / Dinner), tap → recipe `<dialog>` with Sunday batch + day-of steps, reflux rules, optional hard-session add-ons
2. **Workout** — Mon–Sun day chips; each day has **Home** / **Gym** / **Alternative**. Home = 6 daily (core + pull path); Wed/Sat +3 band compounds. Gym = treadmill + 6 lifts Mon/Tue/Thu/Fri. Alternative = miss-gym makeup. Sun home adds meal prep.
3. **Grocery** — weekly buy list in shop packs from Mon–Fri `PLAN` × 5; tap to check; persist locally

## Data shape

```js
MACRO_RULES[]           // regex → kcal/protein from ingredient lines (raw weights)
RECIPES[id]             // { type, risk, name, ing[], steps[] } — B1 L1 SM–SF D1 (+ S1 rotation overview)
ENERGY_ADDONS[id]       // catch-ups; macros shown, not in grocery
PLAN[Mon…Fri]           // ["B1","L1", snackId, "D1"] — snack fruit rotates SM/ST/SW/STh/SF
MEAL_SLOTS[]            // { id, chip, time } — Today toolbar; S1 resolves to today's snack
BUY_CATALOG[]           // match RECIPES ing lines → group/name/shop pack
GROCERY = buildWeeklyGrocery()  // { protein|veg|pantry|dairy: [{name, qtyLabel, key}] }
TREADMILL_BLOCK / HOME_BASE / HOME_EXTRA / ALT_LIFT_A|B / GYM_LIFT_A|B
WORKOUT[]               // { day, title, type, home[], gym[], alt[] }
                        // exercise row: [name, sets, id?] — id keys into exercises.json
                        // treadmill rows use skipVideo:true (no play icon)
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
