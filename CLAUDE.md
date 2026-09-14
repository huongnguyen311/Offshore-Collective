# Offshore Collective, Partner Mobile App: Design Deliverables

Client: Offshore Collective (Matt Flanagan, Founder). Prepared by InApps Technology.
This repo holds the design deliverables (screens, specs, tokens) for the Partner App, Admin Portal, and Contractor Mini-Portal.

## Scope authority

The **SOW spreadsheet, tab "Scope of Work (v1.4.2026), After kickoff"** is the primary, focal scope reference. It is dated 7 Sept 2026, "intended for development following the confirmation on September 5, 2026," and supersedes every earlier scope note (WBS v1.3.2026, the 02-resolved-open-questions-scope.md draft, and anything in the `design/*.md` pipeline that predates it).

Supporting references, both consistent with the SOW's booking logic:
- PRD v1.5, Final Confirmed Scope (English), Google Doc `1y489436K4T1s2GN97QGz7HS4NB1uuNDI`
- Project overview / business logic doc (Vietnamese), Google Doc `1coZ_imZoCMPaiNH5yh2GgRl8QIadpFXn`

When the SOW and an older doc disagree, the SOW wins. Items the SOW marks "Removed" are treated as gone, not as background context, do not reintroduce them in copy, specs, or explanations.

## Fleet: Rayglass 3000 only

Offshore Collective launches exclusively with the Rayglass 3000. All boat-model-specific logic is consolidated into one Boat Model structure so a future model is a contained, developer-led addition rather than a cross-system change.

**Explicitly removed from scope, do not reference these as current features:**
- Rayglass 2400 and every 2400-only feature: Towing Workflow (booking-time destination field + pre-departure disclaimer), Towing Approval Action, WoF Register & Alerts
- $500 online reservation deposit, Stripe integration, Squarespace payment webhook, any in-app payment (all fees are invoiced via Xero, settled by bank transfer)
- Automated Pipeline stage progression (every stage move is manual, made by Matt)
- A separate damage checklist/form for Marine Detailing Co (MDC reports damage to Matt directly, outside the platform)
- AI chatbot for partners
- Partner-to-partner photo sharing / community feed
- GPS tracking (handled by the client outside the InApps system; a future Victron Energy integration is noted but unscoped)
- Mid-term share transfers, insurance integration, Instagram/YouTube embedding, website listing-status sync

## Core booking logic (confirmed)

- **Points:** 58 points/year per partner, reset on the boat's delivery anniversary (shared across all 6 partners on that boat). Points deduct immediately on confirming; unused trips are not refunded.
- **Booking types & cost:** Regular day, 1 pt/night. Weekend Block (Fri to Sun, 3 nights), 5 pts, confirmed. Long Weekend, 7 pts, capped at 2 per rolling 12 months, Fri to Mon (4 days), confirmed. The 2-per-year cap does not apply to a Long Weekend claimed via Unclaimed Weekend/Long Weekend Access (C.5): a partner already at their cap can still claim any leftover portion of an unclaimed long weekend. Christmas Window, 21 pts, one-time admin draw, not partner-selectable.
- **Rules engine:** Indivisible booking blocks, first-confirmed basis (no priority/rotation), advance booking window (60 days, per the project overview doc), max 2 concurrent bookings or 7 consecutive days, hard block on any booking that would take points negative.
- **Unclaimed Weekend Access:** a weekend block unclaimed past midnight Friday reopens to everyone, repriced day-by-day instead of the block rate.
- **Seven-Day Back-to-Back Block:** hard block on a new booking within 7 days of a completed 7-day booking.
- **Named Holiday Fairness:** named public holidays prioritise partners who haven't had that holiday yet.
- **Cancellation:** more than 48 hours before departure, full point refund. Under 48 hours, or a no-show, points forfeited.
- **Christmas Window:** one-time manual draw at the Boats Registry once the boat is delivered and all 6 partners are Stage 3 Confirmed (corrected 2026-09-14 per Matt's sheet comment; not Stage 2 as previously documented); points deducted upfront; release if freed early gives more than 14 days' notice (refunded) vs less than 14 days (forfeited). The partner can trigger this release themselves in-app (the Christmas Window Display screen needs a release action added, it's currently scoped read-only, flagged 2026-09-14, not yet built).
- **Release notifications:** unclaimed weekend reopening, a cancellation, and a Christmas Window release each notify every eligible partner simultaneously, not one at a time.
- **Checklists:** Pre-Departure and Post-Use both require 4 fuel fields (full y/n, litres, gauge photo, reason if not full) and a damage field (y/n, description/photo if yes). 20-litre shortfall tolerance before anything flags. No approval gate blocks departure. Post-Use adds optional extra photos with confirmed copy ("Any great shots from your day out? Add them here, we'd love to see them" / "Photos won't be shared without your approval"), full-resolution upload with no client-side compression, and submitting it fires the turnaround workflow.
- **Turnaround:** Marine Detailing Co completing their clean triggers hand-off only, never the next partner's notification. Only Tamaki Marine Park's distinct launch-confirmation action (the evening before the booking) notifies the next partner. 10am backstop alert to Matt if not actioned (changed from 12pm, per Matt's 2026-09-14 sheet comment). **Open question, flagged 2026-09-14, not yet decided:** for back-to-back bookings (boat never leaves the water), TMP's Launch Confirmed step may not apply the same way, so whether MDC's Mark Job Complete should trigger the next partner's notification directly in that case only is still under review with Matt, do not treat the "MDC never notifies" rule above as settled for the back-to-back case.

## Design Foundation (PRD v1.5 section 6, current)

White background on every functional screen. Chart Paper `#EAE8E1` / Deep Navy `#142A3D` are full-bleed only (splash, onboarding, loading), never a card or panel. Standard traffic-light status colors. Inter for UI text (16px minimum), IBM Plex Mono for every number. Tone: functional, restrained, easy to navigate for a 40s to 60s demographic.

This replaced an earlier Navy/Brass + IBM Plex Mono/Fraunces "quiet luxury" foundation. [screens-preview.html](design/screens-preview.html) has been updated to the current foundation; the rest of the `design/*.md` pipeline (starting with `00-handoff-package.md`) still describes the older foundation and cites WBS v1.3.2026, treat those as historical process notes, not current scope.

## Business Milestones Panel (confirmed 2026-09-14, per Matt's sheet comments)

Months are **30/32/35/36** (not 24/30/35/36 as earlier drafted, Month 24 dropped). Per-boat, not per-partner (independent timelines if a partner runs two Pipeline cards). Admin-only, reminder-only (no partner notification), surfaced in the existing Needs Your Action feed (not a separate feed), and counted from the boat's delivery date, shared by all six partners on that boat regardless of individual contract dates.

What each milestone represents (confirmed 2026-09-14, SOW rows B.26/B.79 updated to match):
- **Month 30:** renewal conversations begin, OC personally contacts each of the six shareholders about their end-of-term intention (upgrade or exit).
- **Month 32:** firm decisions due, all six shareholders confirm their intention in writing; incoming-shareholder slots for the replacement boat cycle begin filling.
- **Month 35:** right-of-first-refusal trigger, if a new boat is on order and incoming slots are confirmed, OC formally offers Rayglass right of first refusal on the outgoing boat.
- **Month 36:** boat comes off the active fleet; Rayglass carries out the EOT refurbishment programme, completed ahead of Month 37 settlement/handover.

## Reports / Export (confirmed 2026-09-14, per Matt's sheet comments)

Two MVP report types: Bookings & utilisation, and Partner status. Format: Excel (.xlsx) primary, PDF secondary for partner-facing documents. Filters: date range, boat, partner. One file per boat only, never combined across boats (data-separation principle, matches the LP-per-boat structure). Contractor activity, exit-pipeline, and financial reports are deferred; financial reporting specifically is blocked pending a joint InApps/accountant (Guy Graham) session, do not start on it before then.

## Still open, unconfirmed by Matt

- Turnaround/notification trigger for back-to-back bookings (see Core booking logic above), flagged 2026-09-14, not yet decided.
- Xero org structure, invoice late-logic, and financial reporting compliance, all explicitly blocked by Matt pending the joint session with accountant Guy Graham.

## Where things live

- [design/screens-preview.html](design/screens-preview.html): the current, authoritative rendered screens (start here for the visual design).
- [design/00-handoff-package.md](design/00-handoff-package.md): index into the full design pipeline (personas, IA, tokens, component specs, a11y, QA). Foundation references inside are stale, see above.
- [design/02-resolved-open-questions-scope.md](design/02-resolved-open-questions-scope.md): InApps' own reasoned defaults for the open questions above, not confirmed answers.
