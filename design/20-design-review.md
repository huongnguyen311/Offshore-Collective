# Offshore Collective — Design Review (Partner Mobile App)

**Consumed inputs:** Full pipeline output `06-art-direction.md` through `19-a11y-spec.md`, plus the rendered screen artifact
**Scope:** Design-system compliance, accessibility, and consistency only — no marketing/CRO scoring, per this app's non-conversion, internal-tool-adjacent nature.

---

## What's working well

The system shows genuine internal coherence rather than being assembled from disconnected steps: the navy/gold palette is sampled directly from the client's own brand files (not invented), the WCAG contrast matrix in `07-color-system.md` proactively caught and fixed a real accessibility failure (white text on gold fill, 2.8:1) before it could reach a component spec, and the calendar/points/status components consistently pull from the same semantic token groups all the way through the component specs, Figma specs, and accessibility spec. The a11y spec also correctly adapted the skill's web-oriented structure (skip links, DOM tabindex) to native mobile idioms rather than forcing an ill-fitting checklist onto a mobile app.

---

## Findings

*(Both [MAJOR] findings below were fixed immediately after this review — see the "Fixed" line under each.)*

**[MAJOR]** `tokens/colors.json` + `tokens/foundations.json` / icon tokens → `16-icon-asset-spec.md` references `color.icon.default`, `color.icon.active`, `color.icon.accent`, `color.icon.inverse`, and `size.icon.sm/md/lg/xl` throughout its Icon System and Icon Inventory sections, but none of these were ever actually added to the token JSON files — they exist only as prose in that one document. A developer grepping `tokens/*.json` for `color.icon` or `size.icon` will find nothing. → Add a real `color.icon` group to `colors.json`'s `light`/`dark` layers (referencing the existing `text`/`action` tokens as that doc specifies) and a real `size.icon` group to `foundations.json`'s `global` layer, so these are actual tokens, not just documented intentions.
**Fixed:** `color.icon.{default,active,accent,inverse}` added to both `light` and `dark` layers in `colors.json`; `size.icon.{sm,md,lg,xl}` added to `global.size.icon` in `foundations.json`.

**[MAJOR]** `tokens/foundations.json` line 94, `component.booking-calendar-date.radius` → set to `{global.border-radius.sm}` (10px), but `11-component-specs.md` (Component 1, Section 4, both the sm and md spec tables) and `18-figma-specs.md` (Component 1, Token References Table) both state the CalendarDateCell Root radius should be `border-radius.xs` (6px). Three documents, two different values for the same token reference. → Pick one and align all three — recommend `border-radius.xs` (6px), matching the two component-level docs; update `foundations.json` to reference `{global.border-radius.xs}` instead.
**Fixed:** `foundations.json`'s `component.booking-calendar-date.radius` now references `{global.border-radius.xs}`.

**[MINOR]** `17-screen-design.md` HTML artifact, `.cal-cell` CSS → hand-authored at `border-radius:8px`, which matches neither the `xs` (6px) nor `sm` (10px) value at the center of the finding above. Most other hand-authored radii in the artifact do land on real token values (`.card` at 20px matches `border-radius.lg`; `.quicklink` at 14px matches `border-radius.md`), so this looks like an isolated slip rather than a systemic drift. → Once the finding above is resolved, update `.cal-cell{border-radius}` in the artifact to match the corrected token value exactly.
**Fixed:** artifact republished with `.cal-cell{border-radius:6px}`.

**[MINOR]** `11-component-specs.md` Component 3 (PointsBalanceDisplay) ticking animation → uses `duration.slow` (360ms) against `06-art-direction.md` Signature Moment #5's "~400ms" target. The component spec already self-flags this as a deliberate rounding to an existing token rather than an oversight, but `15-motion-design.md` Section 3 restates the 360ms value without repeating that flag. → Add a one-line cross-reference in the motion doc ("360ms — see component spec's note on rounding from the art direction's ~400ms target") so a reader arriving at the motion doc first doesn't mistake it for an unflagged inconsistency.

**[SUGGESTION]** `13-state-gallery.md` — the Notification Center (M06) and Payments & Invoices (M11) empty states are documented exceptions to the "empty states always need a CTA" rule, and the reasoning given (nothing plausible to point to) is sound. Consider getting a one-line confirmation from Matt that "there's genuinely nothing to do yet" reads correctly to a first-time partner, since this is a judgment call rather than something derivable from the WBS.

**[SUGGESTION]** `16-icon-asset-spec.md` — the platform app-icon background color (navy vs. light) is already flagged as an open question for the client. Carrying it forward here only to make sure `13-design-qa.md`'s readiness gate treats it as a known, accepted non-blocker rather than something QA should independently flag as a surprise.

---

## Not re-flagged here
Every item already caught and resolved earlier in the pipeline — the white-on-gold contrast failure (`07-color-system.md`), the CalendarDateCell 41pt touch-target shortfall (mitigated with invisible hit-area padding in `11-component-specs.md`/`18-figma-specs.md`), and the deliberate "no spring/bounce motion anywhere" guardrail — are confirmed still correctly applied throughout the downstream docs and are not repeated as new findings.

## Handoff
Feeds `alice-design-qa` (SKILL 13) — the two [MAJOR] findings above should be resolved (or explicitly accepted with a stated reason) before that gate returns READY.
