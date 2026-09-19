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
- **Operating partner records (B.20, confirmed 2026-09-18):** Matt has full add, edit and delete on contractor records directly from the Partners & Services Directory, not just a read-only view. Records hold name/company, contact person, phone, email and service role, and none are optional: phone feeds C.28's standby SMS, email feeds C.11's contractor emails, and service role is what C.10 branches on to route a clean to MDC and a launch confirmation to TMP. Editing is in place, no separate edit screen. Delete is outright only while a contractor has no recorded activity; once they do, the record archives instead, keeping the activity log and dropping out of assignment lists, so the turnaround history in C.10/B.32/B.33 keeps the contractor it refers to. - **Pipeline board default view (B.18, confirmed 2026-09-18):** once a card reaches Active Partner it drops out of the board's default view, which shows only the 6 stages where something is still in flight. Nothing is deleted or archived: a toggle brings Active Partner cards back, and the permanent record is the Partner Directory (B.19). The one action this would have hidden, confirming an outgoing boat's settlement and closing its card so an upgrading partner's new card can reach Active Partner, is raised in the Needs Your Action feed instead (B.1), action-tied, logged to the Pipeline audit trail. Deliberately no board-level exception, so the board carries no special cases. The Partner Directory therefore carries a search by name, phone or email (B.19), which this change makes load-bearing.
- **Co-owner partner records (B.19, confirmed 2026-09-18):** Matt edits a partner's contact fields directly from the Partner Directory too, in place: name, phone, email, and the secondary operator that the partner can only view in-app (A.16), so this is where Matt actions the change the app tells them to phone in about. It is the only place a partner's details can be maintained once they are past onboarding. The two status fields are the exception and stay read-only here: qualification status and capital call stage come from confirming in B.21, whose Undo is the only reversal, so a direct edit would be a second unlogged path to the same state. No dollar figure is ever shown against a stage.
- **Confirmation Undo (B.21, confirmed 2026-09-18):** any item just confirmed in the Manual Confirmation Center carries an Undo. It returns the item to the pending queue, moves the partner's Pipeline card back to its previous stage, and writes the reversal to the audit trail against the admin who did it. This is the **only** way to reverse a confirmation. Dragging the Pipeline card backwards on the board (B.18) is a board-level move only, logged as a manual move, and does not touch the confirmation record, since that would leave the confirmation and the board disagreeing. Knock-ons: an undone qualification sign-off returns the partner to pending and re-locks booking access (A.2); an undone Stage 2 clears the boat's Ordered milestone until all six slots are confirmed again (B.10); an undone confirmation reappears in the Needs Your Action feed, and un-ticking a feed item never reverses anything (B.1).
- **Qualification welcome notification (A.2, confirmed 2026-09-18):** a dedicated one-time notification fires the moment Matt confirms qualification and booking access unlocks. It is presented distinctly from a standard Notification Center row, in content and framing, because it marks a milestone rather than a routine update. Not an elaborate build: one message, fired once per partner, never repeated. Delivered as a push and as its own entry in the Notification Center, deep-linking to the Booking Calendar.
- **Release notifications:** unclaimed weekend reopening, a cancellation, and a Christmas Window release each notify every eligible partner simultaneously, not one at a time.
- **Checklists:** Pre-Departure and Post-Use both require 4 fuel fields (full y/n, litres, gauge photo, reason if not full) and a damage field (y/n, description/photo if yes). 20-litre shortfall tolerance before anything flags, applied silently: the threshold is **never** stated in partner-facing copy (confirmed 2026-09-18, copy-only, C.19's calculation unchanged), because partners are expected to return the boat full and publishing the number invites them to treat it as an acceptable shortfall. No approval gate blocks departure. **Fuel gauge photo waiver (confirmed 2026-09-18):** on either checklist the gauge photo never blocks submission, since a partner who has already left the boat cannot take it. An absent photo is accepted and raises a reminder-only "submitted without fuel photo" item in the Needs Your Action feed; every other field stays mandatory and the turnaround runs as normal. The platform has no location awareness (GPS is out of scope), so the waiver is not location-detected, the missing photo is what carries it. Post-Use adds optional extra photos with confirmed copy ("Any great shots from your day out? Add them here, we'd love to see them" / "Photos won't be shared without your approval"), full-resolution upload with no client-side compression, and submitting it fires the turnaround workflow.
- **Checklist reminders (C.32, timings confirmed 2026-09-18):** pre-departure push fires 1 hour before the booking's estimated departure time (the field captured at booking confirmation, A.8). Post-use push fires 1 hour before the estimated return time (captured at pre-departure submission, A.9), so it can arrive while the partner is still on the water. If Post-Use is still unsubmitted 2 hours after the estimated return time, Matt is alerted in the Needs Your Action feed, reminder-only. The system never triggers the turnaround itself in place of a missing checklist.
- **Turnaround:** the next partner's notification trigger depends on the path (confirmed 2026-09-18). **Back-to-back** (boat never leaves the water): Marine Detailing Co's Mark Job Complete notifies the next partner directly; TMP has no step, because there is no launch to confirm. **Haul-out:** MDC completing their clean triggers hand-off only, and Tamaki Marine Park's distinct launch-confirmation action (the evening before the booking) notifies the next partner. 10am backstop alert to Matt if not actioned (changed from 12pm, per Matt's 2026-09-14 sheet comment). This resolved the open question flagged 2026-09-14; the old "MDC never notifies the next partner" rule is superseded and applies to the haul-out path only.

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

- Xero org structure, invoice late-logic, and financial reporting compliance, all explicitly blocked by Matt pending the joint session with accountant Guy Graham.

## Where things live

- [design/screens-preview.html](design/screens-preview.html): the current, authoritative rendered screens (start here for the visual design).
- [design/00-handoff-package.md](design/00-handoff-package.md): index into the full design pipeline (personas, IA, tokens, component specs, a11y, QA). Foundation references inside are stale, see above.
- [design/02-resolved-open-questions-scope.md](design/02-resolved-open-questions-scope.md): InApps' own reasoned defaults for the open questions above, not confirmed answers.
