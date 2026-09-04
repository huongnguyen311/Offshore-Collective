# Offshore Collective — Partner Mobile App — Design Handoff Package

## 1. Package Header

```
Product:            Offshore Collective — Partner Mobile App
Version:             v1.0-draft
Date:                2026-08-31
Platforms:           iOS + Android (cross-platform, portrait-only)
Design owner:        Alice (this session), for InApps / client Matt Flanagan
Changelog vs previous: n/a — first assembled package for this product
```

**Scope note:** This package covers the **Partner Mobile App only**. The Admin Portal and Contractor Mini-Portal were scoped in the Information Architecture (`01-information-architecture.md`) but design work on them was intentionally paused per user instruction to focus this pipeline on mobile.

---

## 2. Index

| Stage | Document |
|---|---|
| Scope source | Client's WBS Google Sheet ("Scope & Quotation v1.3.2026") — read at project start |
| Resolved open questions | [`02-resolved-open-questions-scope.md`](02-resolved-open-questions-scope.md) |
| Structured brief | [`03-design-brief-parsed.md`](03-design-brief-parsed.md) |
| Personas | [`04-user-personas.md`](04-user-personas.md) |
| Information architecture | [`01-information-architecture.md`](01-information-architecture.md) |
| User flows | [`05-user-flows.md`](05-user-flows.md) |
| Competitive research | [`R1-competitive-brief.md`](R1-competitive-brief.md) |
| Trend research | [`R2-trend-brief.md`](R2-trend-brief.md) |
| Art direction | [`06-art-direction.md`](06-art-direction.md) |
| Color system | [`07-color-system.md`](07-color-system.md) |
| Design tokens | [`08-design-tokens.md`](08-design-tokens.md) — JSON: [`tokens/colors.json`](tokens/colors.json), [`tokens/foundations.json`](tokens/foundations.json), [`tokens/typography.json`](tokens/typography.json) |
| Typography system | [`09-typography-system.md`](09-typography-system.md) |
| Responsive layout | [`10-responsive-layout.md`](10-responsive-layout.md) |
| Component specs | [`11-component-specs.md`](11-component-specs.md) |
| Form specs | [`12-form-specs.md`](12-form-specs.md) |
| State gallery | [`13-state-gallery.md`](13-state-gallery.md) |
| UX writing | [`14-ux-writing.md`](14-ux-writing.md) |
| Motion design | [`15-motion-design.md`](15-motion-design.md) |
| Icon & asset spec | [`16-icon-asset-spec.md`](16-icon-asset-spec.md) |
| Screen design (rendered) | [`17-screen-design.md`](17-screen-design.md) → **[`screens-preview.html`](screens-preview.html) (current, local — start here)**; [originally-published artifact](https://claude.ai/code/artifact/ace4a931-4551-4d95-8076-6dfc81e69daa) is now behind it — see `17-screen-design.md` |
| Figma / dev handoff specs | [`18-figma-specs.md`](18-figma-specs.md) |
| Accessibility spec | [`19-a11y-spec.md`](19-a11y-spec.md) |
| Design review | [`20-design-review.md`](20-design-review.md) |
| Design QA | [`21-design-qa.md`](21-design-qa.md) |

No generated code or Figma file exists for this pass (no live Figma session was run) — the HTML artifact and the written specs above are the complete design deliverable.

---

## 3. Foundations Summary

- **Color:** Navy `#142a3d` (dominant, sampled from the client's own `icon-navy.png`/`lockup-navy.png`) + Brass `#b8972e` (single rare accent). Full 10-step OKLCH scales, semantic aliases, and a passing WCAG AA/AAA contrast matrix in `tokens/colors.json` / `07-color-system.md`.
- **Typography:** IBM Plex Mono (headings, labels, and — critically — every numeral: points balance, dates, litres) + Fraunces (body reading copy). Full scale in `tokens/typography.json` / `09-typography-system.md`.
- **Spacing/Shape/Motion:** Generous, spacious spacing scale extended to 96px; rounded-square radii (6–28px) echoing the brand mark; exactly one shadow token in the whole system; no spring/bounce motion anywhere. `tokens/foundations.json` / `08-design-tokens.md`.

---

## 4. Component Index

| Component | Variants | Spec | Figma | Code |
|---|---|---|---|---|
| CalendarDateCell | 5 states × selected × disabled × 2 sizes | [`11-component-specs.md`](11-component-specs.md#component-1--calendardatecell) | ⏳ not generated (no live Figma session run) | ⏳ pending |
| StatusBadge | 4 statuses × leading-icon | [`11-component-specs.md`](11-component-specs.md#component-2--statusbadge) | ⏳ not generated | ⏳ pending |
| PointsBalanceDisplay | hero/inline × trend × blocked | [`11-component-specs.md`](11-component-specs.md#component-3--pointsbalancedisplay) | ⏳ not generated | ⏳ pending |

Full Auto Layout / layer-tree / token-reference detail for all 3 in [`18-figma-specs.md`](18-figma-specs.md) — written to be implementable without opening Figma at all.

---

## 5. Screen Index

**Corrected (this pass):** the table below previously omitted 4 screens that were, in fact, already rendered (M11, M13, M27, Legal) and pointed only at the originally-published artifact link. All rows below now point to the local, current file. See `17-screen-design.md` for the full explanation.

| Screen ID | Name | States covered | Preview |
|---|---|---|---|
| M02 | Login | default, error, offline (`13-state-gallery.md`) | [screens-preview.html](screens-preview.html) |
| M03 | Home / Dashboard | loading, empty, error, offline, full | [screens-preview.html](screens-preview.html) |
| M04 | Booking Calendar | loading, error, offline, no-permission, full | [screens-preview.html](screens-preview.html) |
| M08 | Booking Review & Confirm | full, error, offline, success | [screens-preview.html](screens-preview.html) |
| M22 | Booking Blocked | full (named-rule messages) | [screens-preview.html](screens-preview.html) |
| M20 | Qualification Pending Gate | full | [screens-preview.html](screens-preview.html) |
| M09 | Booking Detail (towing) | full, loading, error, offline | [screens-preview.html](screens-preview.html) |
| M05 | Boat Rules | loading, error, offline; now includes the Christmas Window release rule | [screens-preview.html](screens-preview.html) |
| M06 | Notification Center | loading, empty, partial, error, offline, full | [screens-preview.html](screens-preview.html) |
| M07 | Profile | loading, error, offline, full | [screens-preview.html](screens-preview.html) |
| M15 | Pre-Departure Checklist | full field spec in `12-form-specs.md` | [screens-preview.html](screens-preview.html) |
| M11 | Payments & Invoices | full, read-only | [screens-preview.html](screens-preview.html) |
| M13 | Christmas Window Display | full, read-only, 3-year view | [screens-preview.html](screens-preview.html) |
| M27 | Support Contact | full; persistent navbar icon on Home/Rules/Profile + Profile menu | [screens-preview.html](screens-preview.html) |
| M25/M26 | Terms & Privacy (combined) | full (client legal copy pending) | [screens-preview.html](screens-preview.html) |

**Remaining 11 of 27 mobile screens** — M01 Splash/Session Check, M10 Towing Destination Entry, M12 Invoice Detail, M14 Notification Detail, M16 Post-Use Checklist, M17 Cancel Booking, M18 Edit Profile, M19 Secondary Operator, M21 Empty state, and M23/M24 (inline error states, not full screens) — are fully defined by IA + component/token specs but were not individually rendered. No open design decision blocks building them; they reuse the same system.

Redlines: not separately produced — the token/spec docs above serve as the redline (every value is named, not eyeballed from an image).

---

## 6. Asset Manifest

See [`16-icon-asset-spec.md`](16-icon-asset-spec.md) Section 5 for the full export manifest. Everything in this app is vector (Phosphor Regular icons, the re-exported brand mark, one custom line-art illustration) — no raster @1x/2x/3x set is required, since the only raster content is user-uploaded checklist photos (explicitly exempt from the design asset pipeline). **No SVG files have been physically exported yet** — Section 5 specifies exactly what's needed; producing the literal files is a follow-up step, not a blocker to starting development on layout/logic.

---

## 7. Open Questions / Assumptions Log

**This log is not empty — the readiness gate below reflects that.**

| # | Item | Status | Default if unanswered |
|---|---|---|---|
| 1 | App-icon background color (navy vs. light) | **Open — no default given** | None — this is the one true blocker |
| 2 | Exact character caps for 3 free-text fields (notFullReason, damageDescription, towing destination) | Open, has a recommended default | 500 / 500 / 200 characters respectively (`12-form-specs.md`) |
| 3 | The original 19 client open-questions (Business Milestones, Reports/Export, reserve balance) | Resolved by InApps' own analysis, awaiting Matt's sign-off | `02-resolved-open-questions-scope.md`'s reasoned defaults — these are Admin Portal items and don't block Mobile App development regardless |
| 4 | Biometric login (Face ID/fingerprint) | Open, flagged in `03-design-brief-parsed.md` | Not built — current scope is credentials-only |
| 5 | Whether a boat-photography treatment is wanted for a future Home-screen enhancement | Open, explicitly deferred | Not built for this release (`16-icon-asset-spec.md`) |

---

## 8. Readiness Gate

```
❌ NEEDS 1 FIX — App-icon background color (navy vs. light) is undetermined
   and has no fallback default, unlike every other open item above.
   Once Matt answers that one question, this package is ready to move to build.
```

Pulled directly from `21-design-qa.md`'s verdict — **not** marked READY, per this skill's own non-negotiable rule that the assumptions log must be empty first (it isn't: 5 open items, 4 with safe defaults, 1 without).

---

## 9. Developer Quick-Start

**File structure:**
```
design/
├── 00-handoff-package.md      ← you are here
├── 01–21 *.md                 ← full spec pipeline, in order
├── screens-preview.html       ← current rendered screens (Gallery + Prototype views) — open this
├── R1/R2 *.md                 ← research briefs
└── tokens/
    ├── colors.json
    ├── foundations.json
    └── typography.json
```

**Opening the preview:** open [`screens-preview.html`](screens-preview.html) directly in any browser — no local setup needed. It has a Gallery view (15 static screens side by side) and a Prototype view (13 of those screens, linked and tappable). The originally-published artifact link in `17-screen-design.md` is now behind this local file; don't build from it.

**Light/dark:** both themes are fully specified in `tokens/colors.json` (`light`/`dark` layers) — the rendered preview shows the app's light mode only (a fixed "device screenshot" style choice, documented in `17-screen-design.md`); dark mode should be built directly from the JSON, not by inverting the preview's CSS.

**Dependencies/setup notes:**
- Fonts: IBM Plex Mono, Fraunces (both on Google Fonts — no licensing blocker).
- Icons: Phosphor Icons, "Regular" weight, MIT-licensed.
- No third-party component library assumed — this is a from-scratch native build per the tokens/specs above.

**Before writing code:** resolve Open Question #1 above with Matt. Everything else in this package is either finalized or has a stated, safe default a developer can build against today.
