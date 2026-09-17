# Offshore Collective — Screen Design (Partner Mobile App)

**Consumed inputs:** `06-art-direction.md`, `01-information-architecture.md` (Mobile IA), `05-user-flows.md`, `07-color-system.md`, `09-typography-system.md`, `10-responsive-layout.md`, `11-component-specs.md`, `12-form-specs.md`, `13-state-gallery.md`, `14-ux-writing.md`, `15-motion-design.md`, `16-icon-asset-spec.md`

**Rendered deliverable:** [Offshore Collective Screens — HTML Artifact](https://claude.ai/code/artifact/ace4a931-4551-4d95-8076-6dfc81e69daa) (as originally published) — **`design/screens-preview.html` in this repo is now the current source of truth**, not the link above. It's a de-framed copy of the same artifact plus fixes made in a later pass (Support Contact navbar icon on Home/Rules/Profile; a Christmas Window rule section added to Boat Rules) that were never pushed back to the published link. Treat the local file as canonical until it's republished.

---

## What's rendered

**Update (2026-09-17):** the whole file was reconciled against the SOW's live Scope Details and Acceptance Criteria, skipping the struck rows. Screens for removed scope were deleted, screens for confirmed-but-undesigned scope were added, and several booking rules that the prototype was enforcing incorrectly were fixed. The counts below reflect that pass.

The file has **two view modes** (toggled via its own Gallery/Prototype switch, `.view-toggle`):
- **Gallery view — 26 screens**, static side-by-side phone frames, built with real content (not placeholder lorem) directly from the SOW, personas, and UX writing docs.
- **Prototype view — 14 linked screens** (`login`, `home`, `calendar`, `standby`, `review`, `confirmed`, `rules`, `alerts`, `profile`, `checklist`, `postcheck`, `christmas`, `support`, `legal`) navigable by tapping through real UI via the page's own `showScreen()` JS. This supersedes `21-design-qa.md`'s H3 finding that no clickable prototype exists.

| Screen | IA ID | Demonstrates |
|---|---|---|
| Login | M02 | Brand mark, credential-only auth |
| Home / Dashboard | M03 | Signature Moment #1, Data Hero + Eyebrow Label; boat card carries the persistent ready status (SOW C.31) |
| Home — Empty (No Upcoming Booking) | M21 | Signature Moment #2, line-art rendering of the icon's chevron breaking free toward the Book tab; points balance still renders normally |
| Booking Calendar | M04 | Signature Moment #3, rounded-square date-state glyphs; seven states including standby (today only) and unclaimed |
| Booking Review & Confirm | M08 | Tabular points breakdown; estimated departure time captured here per A.8 |
| Booking Blocked | M22 | Named-rule violation message, rendered as **7 variants**: insufficient points, 60-day window, 2-booking/7-day cap, 7-day back-to-back, long-weekend cap, named-holiday fairness, and Friday-alone (C.4) |
| Qualification Pending Gate | M20 | Reassurance copy for persona Priya; info-blue, not warning-amber |
| Booking Detail | M09 | Confirmed state. The cancellation rule is not stated here, it appears in the confirmation dialog at the moment of the decision |
| Cancel — Advance Booking | M17 | A.8: 48-hour refund rule, stated in the dialog |
| Boat Rules | M05 | Every booking rule grouped by topic, including the C.5 unclaimed rate tables and both cancellation regimes |
| Notification Center | M06 | Each notification type named in A.14, plus the C.32 checklist reminder. No payment-due entry exists |
| Profile | M07 | Rendered in both modes. One identity card (name, boat, Edit, then qualification / phone / email rows) which edits in place, then the padlocked "Secondary operator" card, view-only per A.16, rendered in both its empty and populated states. Also the secondary-menu hub; support contact sits in the footer (A.17) |
| Pre-Departure Checklist | M15 | Progressive disclosure on the conditional fuel fields; estimated return time |
| Post-Use Checklist | M16 | Same fields plus mandatory engine hours (A.11) and the optional photo card |
| Standby Claim | M28 | Zero-point same-day claim (A.19), with the departure time C.28 sends to Tamaki Marine Park |
| Cancel — Unclaimed-Access Booking | M29 | C.30: points forfeited whenever the partner cancels, with the reason the 48-hour rule does not apply |
| Cancel — Standby Claim | M29 | C.30: nothing to forfeit, but the contractor has to stand down |
| Christmas Window Display | M13 | 3-year term view, assigned window marked with icon and text, partner-triggered release (A.12) |
| Support Contact | M27 | Name/phone/email, admin-managed; reachable via a persistent navbar icon as well as the Profile menu and footer |
| Terms & Privacy | M25/M26 | Combined static legal page (client-supplied copy pending) |

**Design Foundation:** the file renders against the confirmed PRD v1.5 §6 foundation, white on every functional screen, Inter for UI text at 16px minimum, IBM Plex Mono for every number, standard traffic-light status colour, and Chart Paper / Deep Navy reserved for full-bleed moments only. This replaced the earlier Navy/Brass + Fraunces "quiet luxury" direction described in `06-art-direction.md` and `09-typography-system.md`; where those docs and this file disagree, this file and PRD v1.5 win.

## Source of truth
The token JSON files (`tokens/colors.json`, `tokens/foundations.json`, `tokens/typography.json`) and the spec docs (`07`–`16`) are the source of truth for implementation — the artifact's hand-authored CSS is illustrative, not a literal build target. Per `20-design-review.md`/`21-design-qa.md`, one pixel value in the artifact already drifted from its token (fixed); treat any future discrepancy the same way — the token wins.

## What's not covered in this pass
- The remaining mobile screens — M01 Splash/Session Check and the inline Sign-in Error / Offline states (M23/M24, modeled as inline states per `01-information-architecture.md`, not full-screen navigations) — follow the same component/token system already specified in `11-component-specs.md` through `16-icon-asset-spec.md`. They weren't individually rendered here, but nothing about them requires a new design decision.
- **Figma frames:** the pipeline's SKILL SC step calls for both an HTML preview and Figma frames (via `figma-generate-design`). Only the HTML artifact was produced in this pass — generating matching Figma frames requires a target Figma file and a separate `use_figma` session, which wasn't set up as part of this request. Say the word if you want that run next.

## Handoff
Feeds `alice-figma-specs` (SKILL 11) for developer handoff documentation of the 3 critical components, and `alice-a11y-spec` (SKILL A11Y) for per-screen accessibility specs.
