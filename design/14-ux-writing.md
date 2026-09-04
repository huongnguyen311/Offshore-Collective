# Offshore Collective — UX Writing (Partner Mobile App)

**Consumed inputs:** `03-design-brief-parsed.md`, `04-user-personas.md`, `11-component-specs.md`, `12-form-specs.md` (placeholder copy), `13-state-gallery.md` (placeholder copy)
**Tone:** Calm, unhurried, quietly confident (Direction B "Quiet Harbour") — every piece below was checked against this: nothing alarming, nothing cutesy.

---

## High-Stakes CTAs (2 variants required)

**Booking Confirm button (M08)**

Variant A
> Confirm Booking
Chars: 16 | Verb-first, plain — the safest, most literal option; matches David's want for zero-friction speed.

Variant B
> Lock In These Dates
Chars: 20 | More evocative of what's actually happening (dates become unavailable to the other 5 partners) — leans into the scarcity/fairness stakes without sounding alarming. Recommended: this reinforces the "trustworthy fairness system" differentiation angle from `06-art-direction.md` better than the generic Variant A.

**Cancel Booking confirm button (M17)**

Variant A
> Cancel Booking
Chars: 15 | Plain, matches the screen's own name — lowest-risk choice.

Variant B
> Cancel & Release Dates
Chars: 23 | Makes the consequence visible in the label itself (dates go back to the other partners) — recommended for the *refund* case where the action feels low-stakes; for the *forfeit* case (see confirmation dialog below), the body copy carries the real warning, so either variant is acceptable there.

---

## Qualification Pending Gate (M20)

**Screen title**
> Almost there
Chars: 12 | Frames the wait as a final short step, not a rejection — directly serves persona Priya's first-session anxiety.

**Body copy**
> Matt is confirming your Powerboat Training NZ certification. Booking opens automatically the moment he does — no action needed from you.
Chars: 137 | States what's happening, who's responsible, and that she doesn't need to do anything or come back and check — removes the "am I stuck?" feeling identified in FLOW-ONBOARD-01's UX notes.

**Secondary line (optional, below body)**
> In the meantime, have a look through the Boat Rules — it covers everything you'll need for your first booking.
Chars: 114 | Gives her something useful to do with the wait, and nudges toward M05 per the flow's noted first-timer behavior, without being pushy.

---

## Booking Blocked (M22) — rule-violation messages

Each message must name the specific rule per the WBS requirement — never a generic "can't book this."

**Insufficient points**
> This booking needs [X] points, but you only have [Y] left.
Chars: variable | States the exact shortfall the WBS requires (needed vs. remaining), no rounding or vague language.

**60-day advance window**
> These dates are more than 60 days away. You can book them once they're within the 60-day window.
Chars: 98 | Names the specific rule (60-day window) and tells the partner exactly when they'll be able to try again — actionable, not just a block.

**2-booking / 7-day cap**
> You can hold up to 2 bookings at a time, or one booking of up to 7 days. Cancel or complete an existing booking first.
Chars: 116 | States both halves of the rule (2 bookings OR 7 days) since either can trigger this block, and gives the specific unblocking action.

**7-Day Back-to-Back Block**
> You can't book again until 7 days after your last trip ends, to give the boat a break between long bookings.
Chars: 111 | Names the rule and briefly explains its purpose (the "why" reduces the feeling of an arbitrary block) — matches the calm, explain-don't-just-block tone.

**Long-weekend cap**
> You've already used your 2 long weekends for this rolling 12-month period.
Chars: 76 | Precise about the cap (2) and the timeframe (rolling 12 months) — no ambiguity about when it resets.

**Named-holiday fairness**
> Named public holidays are held for a partner who hasn't had them yet. Check back closer to the date, or pick different ones.
Chars: 126 | Added in a later pass — the WBS lists this rule but no message existed for it. Frames the hold as fairness (a partner's turn) rather than a generic block, and offers the same "pick different dates" path as the other five messages, consistent with the calm, explain-don't-just-block tone.

---

## Home Empty State (M21)

**Headline**
> Nothing booked yet
Chars: 18 | Calm, factual — no exclamation marks, no false enthusiasm.

**Body**
> Your next trip starts here.
Chars: 27 | Short, forward-looking, pairs with the gold-chevron line-art (Signature Moment #2) pointing toward the Book tab — the copy and the visual motif say the same thing together rather than duplicating information.

**CTA button**
> Book a Trip
Chars: 12 | Verb-first, matches the Book tab's own action so the CTA doesn't introduce a third way to describe the same task.

---

## Towing Destination Re-Approval Warning (FLOW-TOW-01 Case 2)

Confirmation dialog, per the flow's own UX note recommending a warning before an unexpected consequence.

**Title**
> Change this destination?
Chars: 24 | Verb-phrase title per the confirmation-dialog template.

**Body**
> Changing the destination will send this booking back to Matt for approval. Your dates stay held while he reviews it again.
Chars: 121 | States the consequence (re-approval required) and reassures that the dates aren't lost in the process — directly addresses persona Grant's "am I booked or not?" anxiety from `04-user-personas.md`.

**CTA 1 (proceed)**
> Change Destination
Chars: 19 | Verb-first, matches the title's action.

**CTA 2 (keep as-is)**
> Keep Current Destination
Chars: 25 | Positive keep-verb per the template, explicit about what's being kept.

---

## Status Badge Labels (ties to `11-component-specs.md`)

| Status | Label | Chars | Rationale |
|---|---|---|---|
| pending-approval | `Pending Approval` | 17 | Matches the WBS's own naming exactly (row 34, 77) — no invented terminology |
| qualification-pending | `Pending` | 7 | Shorter than "Qualification Pending" for badge-size constraints; full context is already given by the screen it appears on (Home, Profile) |
| booking-blocked | `Blocked` | 7 | Short, unambiguous; full explanation lives in the M22 messages above, not repeated in the badge |
| confirmed | `Confirmed` | 9 | Standard, matches the persistent-success semantic decided in `07-color-system.md` |

## Points Balance Eyebrow Label (ties to `09-typography-system.md`)

> Points Balance
Chars: 14 | Plain noun label, tracked-uppercase per the Mono treatment — kept short since Mono tracking doesn't wrap gracefully at this size, per that doc's own handoff note.

---

## Post-Use Checklist Photo Disclaimer (M16)

Per `12-form-specs.md` Form 2's note, this heading + footer copy was left as an open item pending this skill. Content is grounded in the WBS's own out-of-scope line on checklist photos (Admin-only, 4-week auto-delete) — the goal is to reassure the partner about where the photo goes, since they're being asked to photograph fuel gauges/possible damage right after a trip.

**Screen heading (above the photo fields)**
> Photos help Matt keep Halcyon in shape
Chars: 39 | Frames the ask around the boat's upkeep, not surveillance — keeps the co-ownership "we all look after her" tone from `06-art-direction.md` rather than reading as a compliance checkbox.

**Footer disclaimer (below the photo fields, before Submit)**
> These photos go to Matt in the Admin Portal only — never shared with other partners — and are automatically deleted after 4 weeks.
Chars: 133 | States the three facts a partner would actually worry about (who sees it, whether other partners see it, how long it's kept) in one plain sentence, matching the WBS's own data-handling rule exactly — no rounding or vague "for a while."

---

## Confirmed As-Is (from earlier drafts)

The following copy drafted in `12-form-specs.md` and `13-state-gallery.md` was reviewed against the Quiet Harbour tone and is confirmed final without changes — listed here rather than duplicated:

- All Pre/Post-Departure Checklist field labels and error messages (`12-form-specs.md` Forms 1–2) — already actionable and jargon-free.
- Towing Destination Entry field copy (`12-form-specs.md` Form 3), except the re-approval warning, now finalized above.
- Edit Profile and Secondary Operator field copy (`12-form-specs.md` Forms 4–5).
- Sign-in error, offline banners, and empty-state copy for Notification Center and Payments & Invoices (`13-state-gallery.md`) — reviewed and kept as drafted; all already follow the "name the problem + give a recovery action" rule.

One small addition to `13-state-gallery.md`'s Notification Center empty state, tightened for tone consistency:

> Nothing here yet
Chars: 15 | Body: "You'll see booking updates and reminders here as they happen." (Chars: 62) | Matches the calm, factual register of the Home empty state above rather than the earlier draft's slightly longer phrasing — small tightening, not a rewrite.

---

## Handoff
Feeds `alice-motion-design` (SKILL 10, the gold-chevron confirmation motion pairs with the Booking Confirm CTA copy above) and `alice-screen-design` (SKILL SC, where all copy above gets placed into real screens).
