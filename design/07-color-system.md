# Offshore Collective — Color System (Partner Mobile App)

**Consumed inputs:** `06-art-direction.md` (Direction B "Quiet Harbour"), exact pixel-sampled hex values from `icon-navy.png`/`lockup-navy.png`
**Token output:** `tokens/colors.json` (Token Studio-compatible)

---

## Step 1 — Brand Color Analysis

Sampled directly from the brand PNGs (Python/Pillow pixel histogram, not eyeballed):

```
Navy — Input: #142a3d
OKLCH: oklch(0.2767 0.0452 246.41)
Lightness: 0.277 → Dark
Chroma: 0.045 → Desaturated (a deliberately quiet, ink-like navy, not a saturated "brand blue")
Hue family: 246.4° → Blue

Gold — Input: #b8972e
OKLCH: oklch(0.6883 0.1247 90.69)
Lightness: 0.688 → Mid-tone
Chroma: 0.125 → Moderate
Hue family: 90.7° → Yellow/Amber (brass)
```

**Suitability check:**

| Check | Navy | Gold |
|---|---|---|
| L between 0.35–0.75 | [WARN] 0.277 — too dark for a "step-500" anchor; re-anchored at step 800 instead (see Step 2) | [PASS] 0.688 |
| C ≥ 0.06 | [WARN] 0.045 — nearly grey, but this is intentional: the brand navy is deliberately desaturated/ink-like, matching the "Quiet Harbour" restraint, not a flaw to fix | [PASS] 0.125 |
| Avoid H 60–75° as primary | [PASS] navy is 246.4° | [PASS] gold sits at 90.7°, outside the risky 60–75° band, and gold is an accent, not the primary anchor, so the risk this check guards against doesn't apply |

Both brand colors proceed as-is — they're fixed by the client's existing mark, not free choices.

---

## Step 2 — Primary Scales (anchor-corrected)

Both brand colors are far from the standard L≈0.55 midpoint the 10-step template assumes, so each anchors at its **closest actual step** rather than being forced into slot 500, per the skill's own override rule:

- **Navy** (L=0.277) anchors at **step 800** (template target 0.28 — a near-exact match)
- **Gold** (L=0.688) anchors at **step 400** (template target 0.65 — closest available step)

| Step | navy | gold |
|---|---|---|
| 50 | `#f2f6f9` | `#faf5e7` |
| 100 | `#e2e9ef` | `#f0e8d1` |
| 200 | `#c3cfdb` | `#dccda4` |
| 300 | `#a1b4c5` | `#c6af6f` |
| 400 | `#7b92a7` | **`#b8972e` ← brand anchor** |
| 500 | `#5b758c` | `#8e6d00` |
| 600 | `#3d5871` | `#6f5000` |
| 700 | `#284055` | `#533800` |
| 800 | **`#142a3d` ← brand anchor** | `#3b2400` |
| 900 | `#010c18` | `#1e0e00` |

## Step 3 — Neutral Scale

Warm neutral, derived from the navy hue (246.4°) at very low chroma (C=0.006) for subtle tonal coherence with the brand — not a true (C=0) grey.

| Step | Hex |
|---|---|
| 50 | `#f2f6f9` |
| 100 | `#e5e8ec` |
| 200 | `#cbced1` |
| 300 | `#aeb1b5` |
| 400 | `#8c9093` |
| 500 | `#6f7275` |
| 600 | `#535658` |
| 700 | `#3b3d40` |
| 800 | `#2b2e31` |
| 900 | `#090b0e` |

## Step 4 — Semantic Colors

| Semantic | Hue used | Note |
|---|---|---|
| Success | 145° | Standard — no clash with brand hues |
| Warning | **70°** (shifted from the standard 85°) | 85° sits too close to brand gold's 90.7° — shifted 15° to keep "warning amber" visually distinct from the brand's brass accent |
| Error | 25° | Standard — no clash |
| Info | **275°** (shifted from the standard 250°) | 250° sits too close to brand navy's 246.4° — shifted 25° to keep "informational blue-violet" distinct from brand navy |

Full 5-step (100/300/500/700/900) values for each are in `tokens/colors.json`.

## Step 5 & 6 — Light/Dark Semantic Aliases

See `tokens/colors.json` `light` and `dark` layers. Highlights:

- **`action.primary` = navy.800** in light mode, but **navy.400** in dark mode (not navy.800 flipped) — dark mode is derived independently to keep 4.5:1+ contrast, per the skill's non-inversion rule.
- **`action.accent` = gold.400 in both modes**, explicitly annotated `$note` in the JSON that gold is reserved for the chevron confirmation motif, the Christmas Window calendar highlight, and CTA micro-accents — never a large fill, per the art direction's anti-template guardrail.
- **App-specific semantic groups added** beyond the skill's standard template, per this app's actual states:
  - `calendar.*` — six states, stepped by lightness rather than hue: available (white) / unclaimed (same fill, dashed outline, lower rate per C.5) / **holiday-block** (info family, C.7 named holiday long weekend) / **standby** (success family, today only, per SOW A.19) / blocked (mid grey + 45° hatch) / booked (solid navy, inverted numeral). Plus `not-bookable` for past and beyond-the-window dates. SOW A.6's AC names four (available / booked / blocked / standby-available); unclaimed is kept as a fifth because it changes the price, which a partner can act on, and the named-holiday long weekend is the sixth, added 2026-09-17 per Matt's M04 comment. Past and beyond-the-60-day-window are merged into one "not bookable" treatment. The measured reason for the earlier cut from seven: every one of the 36 fill pairs sat below 3:1, four states shared one fill, and two failed AA on their own numeral. States are stepped by lightness, and each light state carries a non-colour cue (solid border, dashed border, no border, hatch, TODAY label, 3px top rule).
  - **On the holiday-block fill specifically:** it is deliberately the *secondary* cue, not the distinguishing one. Measured against the other light fills it sits at 1.24:1 on available, 1.01:1 on not-bookable and 1.10:1 on standby, and no light tint reaches 3:1 against them, which is why the first holiday tint was cut rather than recoloured. The distinguishing cue is the **3px top rule** repeated across all four cells of the span: a shape signal that survives greyscale and colour-vision deficiency, and one that carries the right meaning, that Fri to Mon is a single indivisible unit under C.4. One fill for every named holiday, never a colour per holiday, so the legend stays one line.
  - The former `christmas-window` calendar tokens are gone. They were the last everyday use of gold as a fill, which the PRD v1.5 foundation removed, and the state was never rendered on any calendar: the Christmas Window is a one-time admin draw, not partner-selectable, so it lives on M13 with its own components rather than as a cell state on the booking grid.
  - `app-status.checked-out` — amber/warning family, for the booking state between a submitted pre-departure checklist and a submitted post-use one
  - `app-status.qualification-pending` — **info (blue-violet), not warning/error** — a deliberate choice: this is a calm "Matt hasn't confirmed yet" wait, not an error or an urgent decision, and persona Priya's first-session anxiety argues against anything alarming-looking here
  - `app-status.booking-blocked` — error family, for the M22 rule-violation block
  - `app-status.confirmation-accent` — gold.400, the one-time chevron motion accent at the moment of confirming a booking. The persistent "Confirmed" badge shown afterward still uses standard `status.success` (green), so partners can scan booking statuses consistently (checked-out=amber, blocked=red, confirmed/claimed=green); gold marks only the celebratory instant, never becoming a fourth competing status color.

## Step 7 — WCAG Contrast Matrix

| Foreground | Background | Ratio | AA (4.5:1) | AAA (7:1) |
|---|---|---|---|---|
| neutral-900 (text.primary) | navy-50 (bg.default, light) | 18.13:1 | PASS | PASS |
| neutral-600 (text.secondary) | navy-50 (bg.default, light) | 6.81:1 | PASS | FAIL |
| navy-50 (action.primary-text) | navy-800 (action.primary, light) | 13.53:1 | PASS | PASS |
| gold-400 (accent, as text) | navy-800 (on navy) | 5.25:1 | PASS | FAIL |
| **white** (naive default) | gold-400 (if gold were ever a fill) | **2.8:1** | **FAIL** | FAIL |
| navy-900 (accent-text-on-fill) | gold-400 (if gold is a fill) | 7.03:1 | PASS | PASS |
| neutral-50 (text.primary) | navy-900 (bg.default, dark) | 18.11:1 | PASS | PASS |
| neutral-400 (text.secondary) | navy-900 (bg.default, dark) | 6.12:1 | PASS | FAIL |
| gold-400 (accent text) | navy-900 (dark mode) | 7.03:1 | PASS | PASS |
| success-700 | success-100 | 7.29:1 | PASS | PASS |
| warning-700 | warning-100 | 7.02:1 | PASS | PASS |
| error-700 | error-100 | 7.82:1 | PASS | PASS |
| info-700 | info-100 | 7.69:1 | PASS | PASS |

**One real failure caught and fixed:** white text on gold-400 fails AA outright (2.8:1). The token file encodes `action.accent-text-on-fill = navy-900` specifically so no component ever defaults to white-on-gold — any future gold fill (a badge, a button) must pull text color from that token, not assume white.

All secondary-text combinations pass AA but not AAA — acceptable for supporting/secondary text per standard WCAG guidance (AAA is only required for primary reading text in most contexts); flag to SKILL A11Y later in the pipeline if a stricter bar is wanted.

---

## Handoff
Feeds `tokens/colors.json` into SKILL 05 (Design Tokens), which merges it with spacing/radius/shadow/motion tokens. SKILL 06 (Typography) and SKILL 08 (Components) should reference these semantic aliases rather than raw hex values.
