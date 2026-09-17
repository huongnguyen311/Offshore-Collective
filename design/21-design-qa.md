# Design QA Report — Offshore Collective Partner Mobile App

**Consumed inputs:** `18-figma-specs.md`, `20-design-review.md` (2 MAJOR findings already fixed), full pipeline `06`–`20`, `tokens/*.json`, rendered artifact
**Adaptation note:** no live Figma file exists for this pass (per `17-screen-design.md`) — "layer naming" checks below are evaluated against the ASCII layer trees in `18-figma-specs.md` and `11-component-specs.md` rather than an actual Figma file.
**Correction (later pass):** the original H3 finding below was wrong — it described the rendered artifact as a static gallery with no clickable prototype, but the file has always had a working `showScreen()`-based click-through prototype alongside the gallery view. Corrected to [PASS] below; the "wire it up" line item is removed from the Fix Estimate since that work was never actually outstanding. Separately, two content additions were made after this QA pass (a Christmas Window rule section in Boat Rules, a Support Contact navbar icon on Home/Rules/Profile) — both reuse existing tokens/components already covered by T1–T4 below, so they don't reopen this report.

### Results

| # | Check | Status |
|---|---|---|
| L1 | All layers named — no "Rectangle 47" | [PASS] |
| L2 | Layers grouped logically, match component structure | [PASS] |
| L3 | Hidden layers removed or documented with reason | [PASS] |
| L4 | Components detached only when intentional | [PASS] (N/A — no live Figma file to detach from) |
| C1 | All interactive states defined | [PASS] |
| C2 | All data states defined (empty/partial/full/error) | [PASS] |
| C3 | Responsive behavior specified (min/max width) | [PASS] |
| C4 | All variants documented | [PASS] |
| C5 | No `<iframe>` anywhere | [PASS] |
| T1 | No hardcoded colors — all semantic tokens | [PASS] |
| T2 | No hardcoded font sizes — all type styles | [PASS] |
| T3 | No off-grid spacing values | [WARN] |
| T4 | Components reference library, not detached copies | [PASS] (N/A) |
| N1 | No placeholder/lorem text | [PASS] |
| N2 | Realistic data used | [PASS] |
| N3 | Edge cases covered (long text, empty, max items) | [WARN] |
| H1 | All specs documented | [WARN] |
| H2 | Assets exported at correct resolutions | [WARN] |
| H3 | Prototype flows linked and working | [PASS] |
| H4 | Motion specs attached to animated components | [PASS] |

### Failures & Warnings

- [WARN] T3 — The hand-authored HTML artifact (`17-screen-design.md`) uses some pixel values not literally compiled from `tokens/foundations.json` (e.g. `.content{padding:20px 18px}` doesn't map to a single spacing step). Nothing observed is off the *scale* (all values are recognizable multiples/near-multiples of the 4pt grid), but a developer should be told explicitly: **the token JSON files and spec docs are the source of truth, not the artifact's literal CSS** — the artifact is illustrative, not a build target. Recommend adding this one-line disclaimer directly to `17-screen-design.md`.

- [WARN] N3 — Free-text fields (`notFullReason`, `damageDescription`) have a *recommended* soft character cap (e.g. 500 / 500 characters per `12-form-specs.md`) but no client-confirmed final number. A developer can proceed using the recommended defaults without blocking, but this should be tracked as an open confirmation, not treated as finalized.

- [WARN] H1 — `20-design-review.md`'s MINOR finding (PointsBalanceDisplay's 360ms vs. the art direction's ~400ms target) is fixed at the token level but the cross-reference note recommended in that review hasn't yet been added to `15-motion-design.md` Section 3. Small doc-polish item, not a functional gap.

- [WARN] H2 — Every icon/asset is fully *specified* (glyph name, size, color token) in `16-icon-asset-spec.md`, but no actual SVG files have been produced yet — normal at this stage of a design pipeline, but flagging explicitly so it isn't mistaken for a completed export pass. Also carries forward `16-icon-asset-spec.md`'s own open question: the platform app-icon background color (navy vs. light) has **no stated fallback** and needs Matt's input before that one asset can be finalized — this is the one item in this whole report with no recommended default to fall back on.

### Summary
**Status:** [FAIL] NEEDS 1 FIX
**Failures:** 0 | **Needs review:** 4 (3 have a recommended default and don't block a developer; H2's app-icon background color has no default and is the one genuine blocker)

### Fix Estimate

| Item | Estimated Fix Time |
|---|---|
| App-icon background color decision (needs Matt's input — not a design-side fix) | ~5 min (a question, not a task) |
| Add "artifact is illustrative, tokens are source of truth" disclaimer to `17-screen-design.md` | ~5 min |
| Add motion-doc cross-reference note to `15-motion-design.md` Section 3 | ~5 min |
| Confirm the 3 recommended character caps with the client | ~5 min (a question, not a task) |
| **Total blocking work** | **~5 min** (one question to Matt) |

---

## Verdict

```
[FAIL] NEEDS 1 FIX — App-icon background color (navy vs. light) has no confirmed value or fallback default; every other item in this report already has a recommended default a developer can proceed on. Once Matt answers that one question, this pipeline is READY TO SHIP.
```

## Handoff
Feeds `alice-handoff-package` (SKILL PKG) — the readiness gate there should reflect this verdict: not yet READY, blocked on exactly one open client question, with everything else already resolved or defaulted.
