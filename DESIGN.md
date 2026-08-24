---
name: Keto GERD Plan
description: Warm ceramic-sand meal tool keyed to 3-stripe palette (sand / clay / terracotta)
colors:
  sand: "#E3CFBB"
  clay: "#DBBF9F"
  terra-brand: "#CC956B"
  terracotta: "#9A6240"
  terracotta-deep: "#7A4A2E"
  terracotta-bright: "#7A4A2E"
  page: "#E3CFBB"
  raised: "#E0C9B1"
  card: "#DBBF9F"
  ink: "#2A1C14"
  muted: "#5C4335"
  line: "#D4BFA6"
  on-terracotta: "#FFFFFF"
  risk-low: "#3F6B45"
  danger: "#A33A2E"
typography:
  body:
    fontFamily: "Source Sans 3, ui-sans-serif, system-ui, sans-serif"
    fontSize: "16px"
    fontWeight: 400
    lineHeight: 1.55
    letterSpacing: "0.01em"
  heading:
    fontFamily: "Source Sans 3, ui-sans-serif, system-ui, sans-serif"
    fontSize: "1.5rem"
    fontWeight: 600
    lineHeight: 1.25
    letterSpacing: "-0.035em"
  mono:
    fontFamily: "ui-monospace, SF Mono, Menlo, monospace"
    fontSize: "0.875rem"
    fontWeight: 600
    lineHeight: 1.3
    letterSpacing: "normal"
rounded:
  sm: "8px"
  md: "10px"
  pill: "999px"
spacing:
  xs: "4px"
  sm: "8px"
  md: "12px"
  lg: "16px"
  xl: "24px"
components:
  button-primary:
    backgroundColor: "{colors.terracotta}"
    textColor: "{colors.on-terracotta}"
    rounded: "{rounded.md}"
    padding: "12px 16px"
  chip-selected:
    backgroundColor: "{colors.terracotta}"
    textColor: "{colors.on-terracotta}"
    rounded: "{rounded.pill}"
    padding: "8px 12px"
  tab-active:
    backgroundColor: "{colors.raised}"
    textColor: "{colors.terracotta-deep}"
    rounded: "{rounded.md}"
    padding: "8px 4px"
---

## Overview

Product UI on ceramic-sand (`#E3CFBB`) with clay surfaces (`#DBBF9F`) and terracotta clay accent (`#CC956B` brand hue). Soft brand fails AA on sand for small text and white-on-brand, so interactive fills use deepened `--terra` (`#9A6240`) and labels use `--terra-bright` (`#7A4A2E`). Self-hosted Source Sans 3 VF. Document scroll; sticky header; fixed tab bar.

## Colors

Sampled triad (3-stripe PNG, exact):

| Role | Hex | Notes |
|---|---|---|
| Page `--bg` / `--sand` | `#E3CFBB` | Left stripe |
| Card `--bg-card` / `--clay` | `#DBBF9F` | Middle stripe |
| Brand hue `--terra-brand` | `#CC956B` | Right stripe (tints / hover) |
| Fill `--terra` | `#9A6240` | Deepened for white ≥4.5:1 |
| Accent text `--terra-bright` | `#7A4A2E` | Labels on sand ≥4.5:1 |
| Raised `--bg-raised` | `#E0C9B1` | Step between sand and clay |
| Ink `--ink` | `#2A1C14` | Warm near-black |
| Muted `--muted` | `#5C4335` | Dark clay tint |
| Line `--line` | `#D4BFA6` | Quiet separators |

## Contrast rules

- Fills (day chip on): `--terra` + `--on-terra` (~5.0:1)
- Body ink on sand: `--ink` (~10.9:1)
- Muted on sand: `--muted` (~6.0:1)
- 12px labels on sand: `--terra-bright` (~4.9:1)
- Soft brand `#CC956B` is not used for text or primary fills

## Typography

One family: Source Sans 3 VF (`./fonts/SourceSans3VF-Upright.woff2`), SIL OFL, `font-display: swap`. Weight 400 body / 600 headings. Scale ~1.2. Heading tracking ≥ −0.035em; body line-height 1.55.

## Spacing

4 / 8 / 12 / 16 / 24. Fewer borders — surface color separates cards. Quieter pills (no stroke).

## Components

- **Tabs:** 4-item bar; selected `--terra-bright`
- **Day chips:** selected fill `--terra`, label `--on-terra`
- **Meals:** tappable; dialog for recipe detail
- **Grocery:** weekly qty chips; qty in `--terra-bright`

## Do's and Don'ts

**Do:** keep relative URLs (`./`), bump `CACHE` after visual deploys, cache font woff2 in SW.

**Don't:** add Google Fonts CDN, bundlers, cool-gray ink, or use soft `#CC956B` for small text/fills without deepening.
