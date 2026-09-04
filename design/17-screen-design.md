# Offshore Collective — Screen Design (Partner Mobile App)

**Consumed inputs:** `06-art-direction.md`, `01-information-architecture.md` (Mobile IA), `05-user-flows.md`, `07-color-system.md`, `09-typography-system.md`, `10-responsive-layout.md`, `11-component-specs.md`, `12-form-specs.md`, `13-state-gallery.md`, `14-ux-writing.md`, `15-motion-design.md`, `16-icon-asset-spec.md`

**Rendered deliverable:** [Offshore Collective Screens — HTML Artifact](https://claude.ai/code/artifact/ace4a931-4551-4d95-8076-6dfc81e69daa) (as originally published) — **`design/screens-preview.html` in this repo is now the current source of truth**, not the link above. It's a de-framed copy of the same artifact plus fixes made in a later pass (Support Contact navbar icon on Home/Rules/Profile; a Christmas Window rule section added to Boat Rules) that were never pushed back to the published link. Treat the local file as canonical until it's republished.

---

## What's rendered

**Update (this pass):** the counts and table below were wrong in earlier versions of this doc — they described an 11-screen static gallery and undercounted by 4 screens that were, in fact, already rendered (Payments & Invoices, Christmas Window Display, Support Contact, Legal). Corrected below. A further pass added M21 (Empty — No Upcoming Booking), bringing the gallery to 16. A later pass rendered the 5 remaining M22 rule-violation variants (previously copy-only in `14-ux-writing.md`) plus a newly-authored 6th for named-holiday fairness, bringing the gallery to 21.

The file has **two view modes** (toggled via its own Gallery/Prototype switch, `.view-toggle`):
- **Gallery view — 21 screens**, static side-by-side phone frames, built with real content (not placeholder lorem) directly from the WBS, personas, and UX writing docs.
- **Prototype view — 13 linked screens** (`login`, `home`, `calendar`, `review`, `confirmed`, `rules`, `alerts`, `profile`, `checklist`, `payments`, `christmas`, `support`, `legal`) navigable by tapping through real UI — tab bar, quick-links, menu rows — via the page's own `showScreen()` JS. This directly supersedes `21-design-qa.md`'s H3 finding that no clickable prototype exists; it does, and has since before that QA pass was written.

| Screen | IA ID | Demonstrates |
|---|---|---|
| Login | M02 | Brand mark, credential-only auth |
| Home / Dashboard | M03 | Signature Moment #1 — Data Hero + Eyebrow Label; quick-links row |
| Home — Empty (No Upcoming Booking) | M21 | Signature Moment #2 — line-art rendering of the icon's chevron breaking free toward the Book tab; points balance still renders normally |
| Booking Calendar | M04 | Signature Moment #3 — rounded-square date-state glyphs; all 5 calendar states |
| Booking Review & Confirm | M08 | The one rare gold-accent CTA; tabular points breakdown |
| Booking Blocked | M22 | Named-rule violation message, rendered as 6 variants covering every rule in FLOW-BOOK-01 Case 2 — insufficient points, 60-day window, 2-booking/7-day cap, 7-day back-to-back, long-weekend cap, named-holiday fairness |
| Qualification Pending Gate | M20 | Reassurance copy for persona Priya; info-blue, not warning-amber |
| Booking Detail — Towing | M09 | Pending Approval status badge for persona Grant's edge case |
| Boat Rules | M05 | Grouped-by-topic content, gold used only as section-label accent; now includes the Christmas Window release rule |
| Notification Center | M06 | All WBS notification types with rounded navy-line icons |
| Profile | M07 | Secondary-menu hub (Payments/Christmas Window/Support/legal) |
| Pre-Departure Checklist | M15 | Progressive disclosure on the conditional fuel fields |
| Payments & Invoices | M11 | Read-only invoice list with Paid/Due status |
| Christmas Window Display | M13 | Read-only 3-year term view, assigned window highlighted gold |
| Support Contact | M27 | Name/phone/email, admin-managed; reachable via a persistent navbar icon on Home/Rules/Profile as well as the Profile menu |
| Terms & Privacy | M25/M26 | Combined static legal page (client-supplied copy pending) |

Every screen uses the exact tokens decided earlier in the pipeline — navy `#142a3d` / gold `#b8972e` sampled from the client's own brand files, IBM Plex Mono for headings/labels/every numeral, Fraunces for reading copy, the extended spacious spacing scale, and rounded-square (not sharp, not pill-default) container radii.

## Source of truth
The token JSON files (`tokens/colors.json`, `tokens/foundations.json`, `tokens/typography.json`) and the spec docs (`07`–`16`) are the source of truth for implementation — the artifact's hand-authored CSS is illustrative, not a literal build target. Per `20-design-review.md`/`21-design-qa.md`, one pixel value in the artifact already drifted from its token (fixed); treat any future discrepancy the same way — the token wins.

## What's not covered in this pass
- The remaining **10 of 27** mobile screens — M01 Splash/Session Check, M10 Towing Destination Entry, M12 Invoice Detail, M14 Notification Detail, M16 Post-Use Checklist, M17 Cancel Booking, M18 Edit Profile, M19 Secondary Operator, and M23/M24 (Sign-in Error, Offline — modeled as inline states per `01-information-architecture.md`, not full-screen navigations) — follow the same component/token system already specified in `11-component-specs.md` through `16-icon-asset-spec.md`. They weren't individually rendered here, but nothing about them requires a new design decision; extending the artifact to cover them is straightforward if wanted.
- **Figma frames:** the pipeline's SKILL SC step calls for both an HTML preview and Figma frames (via `figma-generate-design`). Only the HTML artifact was produced in this pass — generating matching Figma frames requires a target Figma file and a separate `use_figma` session, which wasn't set up as part of this request. Say the word if you want that run next.

## Handoff
Feeds `alice-figma-specs` (SKILL 11) for developer handoff documentation of the 3 critical components, and `alice-a11y-spec` (SKILL A11Y) for per-screen accessibility specs.
