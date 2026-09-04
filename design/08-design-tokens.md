# Offshore Collective — Design Tokens (Partner Mobile App)

**Consumed inputs:** `06-art-direction.md` (Direction B "Quiet Harbour" — spacing/radius/motion constraints from its Handoff table), `tokens/colors.json` (SKILL CS output, reused verbatim)

**Token output:** Two files, both part of the same Token Studio set collection:
- `tokens/colors.json` — `global.color`, `light.color`, `dark.color` (from SKILL CS, unchanged)
- `tokens/foundations.json` — `global.spacing/border-radius/border-width/shadow/opacity/duration/easing` + all `component.*` tokens

**Note on typography:** fontSize/fontWeight/lineHeight/letterSpacing and the composite `typography.*` styles are intentionally NOT included here — SKILL 06 (Typography System, next) is the single source of truth for those, per the token skill's own merge-collision warning.

---

## What serves the art direction

Every non-color foundation value below was chosen to serve **Direction B — Quiet Harbour**, not a generic default:

| Decision | Value chosen | Why (ties to `06-art-direction.md`) |
|---|---|---|
| Spacing scale | Extended past the standard 4pt scale to `20` (80px) and `24` (96px) | Quiet Harbour calls for generous, spacious composition — the Home screen's points-balance hero moment needs room the standard scale tops out too early for |
| Border radius | Soft, generous (`md`=14px default, up to `xl`=28px), `full` reserved for pills/badges only | Echoes the brand icon's rounded-square geometry; a sharp-corner (0–2px) system would contradict both the mark and the "calm luxury" personality |
| Shadow | Exactly one token (`shadow.subtle`, 6% opacity) — everything else is `shadow.none` | Anti-template guardrail: "every card = same white rectangle with the same drop shadow" is explicitly rejected — depth comes from spacing and tonal contrast, not elevation |
| Motion easing | `cubic-bezier(0.33, 1, 0.68, 1)` gentle ease-out for standard transitions; **no spring/bounce curve anywhere** | Direction B's motion character is "fluid, restrained" — this deliberately rejects Direction A's "snappy, mechanical" character and any spring physics |
| `duration.confirm` / `easing.confirm` | 560ms, slow settling deceleration — distinctly slower than the 220ms standard | Reserved exclusively for the signature gold-chevron confirmation motif (Signature Moment #4) — it should read as considered, not routine |

## Component tokens worth flagging

- **`component.button-accent`** exists specifically so the gold accent color can be used on *at most one* CTA per screen (per the art direction's "gold stays rare" guardrail) without every button defaulting to it — `component.button.primary-bg` (navy) remains the default for everything else.
- **`component.booking-calendar-date`** wires the four calendar states (available/booked/held/blocked) plus the Christmas Window highlight directly to the semantic color groups defined in `colors.json`'s `calendar.*` — this is the token layer for Signature Moment #3 (rounded-square date glyphs).
- **`component.confirmation-motion`** is a dedicated token group (not folded into a generic "motion" component) so SKILL 10 (Motion Design) has one unambiguous place to pull the signature moment's color/duration/easing from.
- **`component.card`** defaults to `shadow.none` — a card only gets `shadow.subtle` if it must visually float (e.g. a bottom sheet), which should be the exception, not the rule, on every screen.

## Handoff
- SKILL 06 (Typography) — needs to define the wordmark-derived monospace numeral treatment referenced by `component.points-balance-display`.
- SKILL 07 (Responsive Layout) — should build its grid/margins from `global.spacing`, favoring the larger steps (`5`–`24`) over the smaller ones for primary layout rhythm.
- SKILL 08 (Component Design) — the 3 critical components chosen there should draw radius/shadow/spacing from these tokens, not introduce new raw values.
- SKILL 10 (Motion Design) — wires directly from `component.confirmation-motion` and `global.easing`.
