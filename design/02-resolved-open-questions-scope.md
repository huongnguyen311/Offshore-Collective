# Offshore Collective — Resolved Scope for Provisional WBS Items

**Source:** Independent analysis of the "Open Questions for Client" tab (19 questions), resolved by precedent already established elsewhere in the 90-item WBS — **not** based on client answers (none received yet).
**Prepared:** 2026-08-31
**Status:** Draft for internal/client review — replaces the provisional placeholders from `01-information-architecture.md` (A11, A12) and updates A09/row 49.

> Each item below states the reasoning first (grounded in an existing, already-agreed pattern from the WBS), then gives ready-to-paste **Scope details** and **AC** text in the same format and voice as the rest of the Scope & Quotation sheet. Treat these as InApps' proposed defaults — flag to Matt as "assumed unless corrected," not as confirmed answers.

---

## 1. Business Milestones Panel — B.25 (row 53)

### Reasoning
- **Boat delivery date, not partner contract date** — the WBS already establishes one shared, boat-level clock ("Each partner gets 58 points a year, resetting on the boat's delivery anniversary — shared reset date for all 6 partners on that boat"). There's no precedent anywhere in the WBS for a per-partner clock; reusing the existing shared clock avoids inventing a second, parallel time system.
- **Term length is 3 years** — the Christmas Window Display is explicitly "a read-only view of all 3 years of the partner's term." Month 36 lines up exactly with the end of that term, which is the strongest signal for what these four checkpoints represent: a countdown toward end-of-term.
- **Milestone meaning** — modeled on the WoF pattern (an early warning, then a closer warning: 30 days / 14 days) but stretched across a 3-year term instead of weeks: Month 24 (early), Month 30 (follow-up), Month 35 (final, 1 month out), Month 36 (term complete).
- **Appears in Needs Your Action feed** — every other admin alert type in the WBS (WoF, engine servicing, fuel shortfalls, damage reports, payments) surfaces there; nothing suggests Business Milestones should be the one exception.
- **Reminder-only, not action-tied** — the feed's own definition separates items that trigger a system action (e.g. confirming a payment) from items that are just cleared once handled. Renewal/end-of-term conversations are Matt's own relationship work, not something the system can execute — so reminder-only fits.
- **Admin-only, no partner notification** — the partner app's scoped notification types (booking confirmed, boat ready, payment due, access opened, release notifications) are all transactional. Term-planning reminders don't fit that pattern, and adding a new partner-facing notification type isn't supported elsewhere in the WBS.
- **Upgrading partners need no special handling** — the milestone clock belongs to the *boat*, not the partner. Each boat already has its own independent delivery date, so an upgrading partner's new boat simply starts its own separate Month 24/30/35/36 timeline — no interaction with the parallel-pipeline-card mechanic used for Pipeline stage tracking.

### Scope details
- The admin sees an alert for each boat at Month 24, 30, 35, and 36 of that boat's 3-year ownership term, calculated from the boat's delivery date — the same shared clock already used for the annual points reset, so all 6 partners on a boat stay aligned to one timeline.
- Each milestone marks a stage in winding toward the end of the term: Month 24 is an early reminder to start renewal/next-boat conversations with the partners; Month 30 is a follow-up if no decision has been made yet; Month 35 is a final, one-month-out reminder to confirm the renewal-or-exit decision and prepare the end-of-term reserve settlement; Month 36 marks the term as complete and prompts Matt to finalize the reserve balance and next steps for the boat.
- Like every other admin alert (WoF, engine servicing, fuel shortfalls, damage reports, payments), each milestone also appears in the Needs Your Action feed — the standalone panel is the full read-only record; the feed is where Matt actually clears them.
- Each milestone is reminder-only: marking it done simply clears the reminder once Matt has had the relevant conversation — the system does not perform any other action automatically.
- These alerts are admin-only; partners are not notified in-app when a boat reaches a milestone.
- Because each boat has its own independent delivery date, an upgrading partner's new boat starts its own separate Month 24/30/35/36 timeline — the milestone clock is unaffected by a partner running two parallel pipeline cards during an upgrade.

### AC
- System raises an alert for a boat at Month 24, 30, 35, and 36, calculated from that boat's delivery date.
- System applies the same milestone dates to all 6 partners sharing a boat.
- System adds each milestone alert to the Needs Your Action feed in addition to the Business Milestones Panel.
- System treats each milestone as reminder-only — marking it done clears the reminder without triggering any other system action.
- System does not send any partner-facing notification for these milestones.
- System calculates an upgrading partner's new boat's milestones from that boat's own delivery date, independent of their previous boat's timeline.

---

## 2. Reports / Export — B.26 (row 54)

### Reasoning
- **Which reports** — rather than invent new report types, each proposed report maps 1:1 to data that's already fully specified elsewhere in the WBS (bookings/points, checklists, invoices, engine servicing, pipeline, fleet utilisation). This is the safest reading: exporting what already exists, not building new aggregations.
- **CSV/Excel, not PDF** — the open question itself frames PDF as "a materially larger piece of work." The WBS shows a repeated pattern of trimming scope aggressively (Stripe removed, AI chatbot removed, automated pipeline progression removed — see the Out of Scope notes). Defaulting to the leaner option is consistent with that pattern.
- **On-demand only, no scheduling** — there's no precedent anywhere in the WBS for a scheduled/emailed report; every automated communication that exists (weekly planner, daily action email) is contractor-facing operational messaging, not admin reporting. Adding a new scheduling subsystem isn't supported by existing scope.
- **Per-boat data separation applies to exports** — this is the one open question with a directly-stated answer already in the WBS: "System keeps financial data, partner records, and booking history separated by boat/Limited Partnership. System never mixes or cross-displays data belonging to two different boats." An export spanning multiple boats must therefore still be split into one file per boat.
- **Admin-only, no contractor export** — the Contractor Mini-Portal is explicitly described as never exposing "full admin/financial/partner data." Export access would break that restriction.
- **No Xero-reconciliation-specific format** — invoices and payments already sync directly to Xero, which is the system of record for accounting. Building a second, accounting-formatted export would duplicate that rather than extend it.

### Scope details
- The admin can export six report types, each drawn from data already tracked elsewhere in the Admin Portal: bookings & points used per partner, checklist history (including fuel shortfalls and damage flags), invoices & payment status, engine hours against the service schedule, sales pipeline snapshot, and boat utilisation.
- Exports download as CSV or Excel-compatible files — a data export for the admin's own use, not a formatted PDF document.
- The admin can filter an export by date range, boat, partner, or status (payment or booking), reusing fields already captured on bookings, checklists, and invoices elsewhere in the system.
- Exports are generated on demand only, when the admin requests them — there is no automatic scheduled or emailed report.
- In line with the strict per-boat/Limited Partnership data separation already enforced everywhere else in the system, an export covering more than one boat is produced as separate files per boat, never combined into one file.
- Export is available to the admin only — the Contractor Mini-Portal does not include any export capability.
- Exported financial data is a plain read-out of what the system already records — it is not formatted for accounting reconciliation; Xero remains the source of truth for accounting.

### AC
- User (admin) can export bookings & points usage, checklist history, invoices & payment status, engine servicing status, sales pipeline, and boat utilisation as separate report types.
- System produces a downloadable CSV or Excel-compatible file for each export — no PDF export is provided.
- User can filter an export by date range, boat, partner, or status before generating it.
- System generates exports only when the admin manually triggers them — no export is scheduled or emailed automatically.
- System splits any multi-boat export into one file per boat, never combining two boats' data into a single file.
- System does not expose any export capability in the Contractor Mini-Portal.

---

## 3. Financials Overview — reserve balance — B.21 (row 49)

### Reasoning
- **Manually entered, not system-calculated** — every financial figure in the WBS is either fully automated from an already-scoped mechanism (points balance, fuel shortfall $) or fully manual (points override, engine service log). There is no scoped "recurring contribution" concept anywhere else in the WBS that a calculated reserve balance could be built on — inventing one would add an entire unscoped subsystem. The manual-entry pattern (Manual Points Override, with mandatory audit trail) is the closer, already-agreed precedent.
- **Admin-only** — the open question's own rationale already states it: "The Partner App currently has no financial view beyond read-only invoices." Adding reserve-balance visibility would introduce a new partner-facing financial data type that nothing else in the Partner App scope supports.

### Scope details
- The admin sees a financial summary showing invoices and their payment status, and each boat's end-of-term reserve balance.
- The reserve balance is a figure Matt enters and updates manually per boat — the same manual pattern already used for points overrides and engine service logging — with no automatic calculation behind it.
- The reserve balance is admin-only; it is not shown anywhere in the Partner App, which remains scoped to read-only invoice history only.

### AC
- User can view a list of invoices with their payment status.
- User can view and manually update the end-of-term reserve balance for each boat.
- System does not calculate the reserve balance automatically from any contribution schedule.
- System does not display the reserve balance, or any financial figure beyond invoice history, anywhere in the Partner App.

---

## 4. Long Weekend booking type — exact date span (Boat Rules / Booking Calendar)

### Reasoning
- **Not defined anywhere in the WBS or downstream docs** — confirmed by a full re-search of every design deliverable (`03-design-brief-parsed.md`, `05-user-flows.md`, `14-ux-writing.md`, `screens-preview.html`, etc.). The cost table fixes the price (7 pts) and the cap (2 per rolling 12 months), but no source states which days of the week the booking spans.
- **Weekend Block is the only sibling type with a confirmed span** — it is evidenced directly (M08 review copy and a11y spec both show "Fri 6 Mar → Sun 8 Mar" labeled "Weekend Block," 3 nights, 5 pts). Long Weekend is priced one tier above it, which reads as "the weekend plus one more day," not as an unrelated span length.
- **Thu→Sun over Fri→Mon** — extending the front of the weekend (Thu) keeps checkout on Sunday, matching Weekend Block's Sunday checkout and the operator's existing Fri/Sat/Sun turnover rhythm implied elsewhere in the WBS (boat swap timing isn't scoped for a Monday handover anywhere).
- **Do not round unmatched ranges up to a bundle** — a 2-day or 5-day selection that doesn't match either named pattern falls back to the Regular Day rate (1 pt × nights). Inventing a rounding rule not stated anywhere in the WBS risks charging a partner more than the system can justify — the confirmed "Regular day = 1 pt" rate is the one price explicitly safe to apply as a fallback.

### Scope details
- Long Weekend = a booking selected exactly Thursday → Sunday (4 consecutive nights), flat 7 pts, capped at 2 per rolling 12 months (cap already specified).
- Weekend Block = a booking selected exactly Friday → Sunday (3 consecutive nights), flat 5 pts (already confirmed elsewhere).
- Any other date range (any night count/day-of-week combination that isn't one of the two patterns above, and isn't the partner's assigned Christmas Window) is priced at the Regular Day rate: 1 pt per night.

### AC
- System prices a Thursday→Sunday (4-night) selection as a Long Weekend at a flat 7 pts.
- System prices a Friday→Sunday (3-night) selection as a Weekend Block at a flat 5 pts.
- System prices every other selectable date range at 1 pt per night, with no other bundle rounding applied.

### Flag to client
This assumption is unconfirmed — send to Matt for sign-off alongside the original 19 open questions. If the real span differs (e.g. Fri→Mon), only this section and the calendar's point-calculation logic in `screens-preview.html` need updating.

---

## Note

These are InApps' reasoned defaults, not confirmed client answers — the original 19 open questions are still worth sending to Matt for sign-off, but design and estimation work no longer needs to wait on them. If Matt's answers diverge from any assumption above, only the affected bullets need revision — the rest of the scope (and the IA already produced) is unaffected.
