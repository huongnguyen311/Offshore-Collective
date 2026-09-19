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
Chars: 23 | Makes the consequence visible in the label itself (dates go back to the other partners), recommended for the *refund* case where the action feels low-stakes. Settled 2026-09-17: a *cancellation* that forfeits points uses the plain Variant A label and lets the body carry the warning, because a button that names a cost and an action at once ("Cancel, Forfeit 4 pts") reads as two instructions at the moment the partner can least parse one. The one exception is the Christmas release (M13), where the action is optional rather than forced by a change of plan, and the cost belongs on the button.

---

## Qualification Pending Gate (M20)

**Screen title**
> Almost there
Chars: 12 | Frames the wait as a final short step, not a rejection — directly serves persona Priya's first-session anxiety.

**Body copy**
> We're confirming your Powerboat Training NZ certification. Booking opens as soon as that's done. Nothing needed from you.
Chars: 137 | States what's happening, who's responsible, and that she doesn't need to do anything or come back and check — removes the "am I stuck?" feeling identified in FLOW-ONBOARD-01's UX notes.

**Secondary line (optional, below body)**
> In the meantime, have a look through the Boat Rules — it covers everything you'll need for your first booking.
Chars: 114 | Gives her something useful to do with the wait, and nudges toward M05 per the flow's noted first-timer behavior, without being pushy.

---

## Qualification Approved Welcome (M06, SOW A.2)

Confirmed by Matt 2026-09-18: a dedicated one-time notification fires the moment qualification is approved, and it must read and feel distinct from a routine entry. This is the direct answer to M20's "Almost there", so the two are written as a pair, and it is the one notification that resolves the anxiety M20 names rather than reporting an operational fact.

**Heading**
> You're approved. The boat is yours.
Chars: 35 | Two short sentences rather than one, so the approval lands before the invitation. "The boat is yours" uses the ownership language that is actually true here, these are co-owners, not customers, and the CTA below carries the "to book" part so the heading does not have to. Deliberately larger than a row title, because Matt asked for something that feels like a milestone. Wraps to 2 lines at 19px in a 329px card, which is what makes it read as a headline rather than a row.

**Body**
> Powerboat Training NZ qualification confirmed. 58 points for the year, and the calendar is open 60 days ahead.
Chars: 110 | Shortened from 182 on request (2026-09-18), measured in the browser: 3 lines instead of 5, card height 295px instead of 368px. Two things were cut, both because something else already says them. "so every part of the app is now open to you" went, because the heading's "You're approved" states it. "Your ... is" went from the opening clause, so it reads as the status line it actually is. What stays is the phrase that closes the loop on M20's "We're confirming your Powerboat Training NZ certification", so the partner recognises this as the answer to that screen, plus the two numbers that decide what they can do next, both in mono per the type system. No exclamation mark and no welcome-aboard language: the tone rule here is functional and restrained, and a 40s-to-60s co-owner reads celebration as marketing.

**Action**
> Book your first trip
Chars: 19 | Names the action, not the destination, and "first" is accurate exactly once, which suits a notification that fires exactly once. Routes to M04, since booking access is the thing that just changed.

**Timestamp**
> JUST NOW
Chars: 8 | Same mono treatment as every other entry's timestamp, which is the one respect in which this entry does behave like the rest of the list.

**Not written, and why:** no dismiss control and no second step. Matt's note asks for one message, not an elaborate build, and the entry ages out of relevance on its own as the list fills beneath it.

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

## Cancelling an Advance Booking (M17, SOW A.8)

Confirmation dialog, refund case. One shape now governs every cancel and release dialog in the app: state only the outcome that is true right now, in points, and leave the dates out, they are on the screen behind the dialog.

**Title**
> Cancel this booking?
Chars: 20 | Verb-phrase title per the confirmation-dialog template. No date in the title: the booking card behind the dialog already carries it.

**Body**
> Cancelling now is more than 48 hours ahead, so your 1 point comes straight back to you.
Chars: 87 | Rewritten 2026-09-17. The earlier version named the date and then described the forfeit branch as well. At the moment of the decision only one branch is true, and the one that is not is noise the partner has to rule out before acting.

**CTA 1 (proceed)**
> Cancel Booking
Chars: 14 | Variant A from the High-Stakes CTA set, plain, because nothing is at stake in points here.

**CTA 2 (keep as-is)**
> Keep Booking
Chars: 12 | Positive keep-verb per the template.

**Not yet written:** the forfeit case (inside 48 hours, or a no-show). It needs the treatment the Christmas release pair uses below, an error-tinted icon and a body that says forfeited rather than refunded. Flagged 2026-09-17, no screen designed for it yet.

---

## Cancelling an Unclaimed-Access Booking (SOW C.30)

Confirmation dialog. The partner already knows the 48-hour refund rule from the Boat Rules screen, so the job here is to say why it does not apply, before they find out from their balance.

**Title**
> Cancel this weekend?
Chars: 20 | Verb-phrase title per the confirmation-dialog template.

**Body**
> Dates claimed after they went unclaimed forfeit their points whenever you cancel, so your 4 points are gone. The dates go straight back to your other 5 partners.
Chars: 161 | Cut from 242 on 2026-09-17. The old version spent a whole sentence denying the 48-hour rule; that rule is taught on Boat Rules and applied in M17, and repeating it here only to say "not this one" made the shortest-fuse dialog in the app the longest to read.

**CTA 1 (proceed)**
> Cancel Booking
Chars: 14 | Changed from "Cancel, Forfeit 4 pts" on 2026-09-17. Matches the same button on M17 and the "Keep Booking" beside it; the red fill and the body sentence carry the forfeit.

**CTA 2 (keep as-is)**
> Keep Booking
Chars: 12 | Positive keep-verb per the template.

---

## Cancelling a Standby Claim (SOW C.30)

Confirmation dialog. The only cancellation in the app with nothing at stake in points, so the copy leads with the operational consequence instead.

**Title**
> Give up today's standby?
Chars: 24 | "Give up" rather than "cancel", because there is no booking to cancel in the partner's mental model, just a free day they claimed this morning.

**Body**
> A standby claim costs 0 points, so there is nothing to refund or forfeit. We’ll notify the team to stand down, or bring Halcyon back in if she is already in the water.
Chars: 167 | Says the points outcome first to close the question, then names the real cost, which is a wasted trip for whoever has to move the boat. Deliberately does not name the contractor: the partner has no relationship with Tamaki Marine Park and does not need one to understand the consequence.

**CTA 1 (proceed)**
> Give Up Standby
Chars: 15 | Matches the title's verb.

**CTA 2 (keep as-is)**
> Keep It
Chars: 7 | Deliberately light. Nothing is at risk here, so the keep option does not need the weight the other two dialogs give it.

---

## Releasing a Christmas Window Week (M13, SOW A.12 / C.9)

Two states of one dialog, chosen by the date, never both reachable. Written 2026-09-17, when the release action moved out of the year-card header into its own footer.

**In-card note, above the Release button**
> Not going to use it? Release the week back to the other five partners. Released more than 14 days ahead, your 14 points are refunded in full. Under 14 days, they are forfeited.
Chars: 176 | The full rule lives here, on the calm screen, so the dialog itself only has to state the outcome. Both branches appear because at this point the partner is reading, not deciding.

**Title (both states)**
> Release your week?
Chars: 18 | Same title in both states. The dates sit on the card behind the dialog, and the partner has exactly one week to release, so naming it adds nothing.

**Body, refund state (more than 14 days out)**
> Releasing today is more than 14 days ahead, so all 14 points come straight back to you. Your 5 partners are told at the same moment, and the week opens to them first-confirmed.
Chars: 176 | Points outcome first, because that is the question. The second sentence answers the one every partner asks next, who gets the week, and names the simultaneous notification rule (C.9) without using the word simultaneous.

**Body, forfeit state (inside 14 days)**
> Releasing today is inside 14 days, so your 14 points are forfeited, not refunded. Your 5 partners are told at the same moment, and the week opens to them first-confirmed.
Chars: 170 | "Forfeited, not refunded" rather than just "forfeited": the partner arrives carrying the refund rule from the other state and needs it contradicted explicitly, not merely omitted.

**CTA 1, refund state**
> Release My Week
Chars: 16 | Navy, not red. Nothing is lost in this branch, and a red button here would teach the partner to fear an action that costs them nothing.

**CTA 1, forfeit state**
> Release and Forfeit 14 pts
Chars: 27 | The one place in the app where a button still carries its cost. Unlike a cancellation, releasing is optional and reversible right up to the tap, so the price belongs on the thing being pressed.

**CTA 2 (keep as-is, both states)**
> Keep My Week
Chars: 13 | Possessive, matching the "Your week" label on the assigned window.

---

## Standby Claim Screen (M28)

**Zero-cost hero label**
> Standby, Mon to Thu
Chars: 19 | Carries the eligibility rule (confirmed 2026-09-19), which is the one thing on this screen a partner cannot work out for themselves. It replaced "Standby, today only": same length, but the navbar ("Claim Today"), the date line ("Thu 12 Mar, today") and the button ("Claim Today, Free") already say same-day three times between them, so the eyebrow was the only line spending itself on a fact the screen had covered.

**Departure-time help text**
> We’ll notify the team the moment you claim, so give them as much notice as you can.
Chars: 83 | Explains why the app is asking, rather than just asking. The partner is doing someone a favour, not filling in a required field. Says "the team" rather than naming Tamaki Marine Park, which is a name the partner cannot act on.

**Body**
> Nobody booked today by 7am, so it is open to all six partners, free. First to claim gets it, and there is no limit on how many standby days you take in a year. Standby runs Monday to Thursday only. Halcyon cannot be turned around between trips at a weekend, so Saturday and Sunday are never standby days: they stay on unclaimed weekend rates.
Chars: 157 for the first three sentences | States the 7am trigger, the free price, the first-confirmed basis and the absence of a cap, which are the four things SOW A.19 makes true and none of which a partner would assume. The Monday-to-Thursday sentences were added 2026-09-19 and carry a reason and a destination, not just the limit: the reason, because a partner told only that weekends are out will ask why and the answer is a fact about the boat rather than a rule; the destination, because Saturday and Sunday are not refused, they are priced by C.5, and a limit with nowhere to go reads as the app being unhelpful.

> **Drift to fix:** the built screen still opens "Today went unclaimed at 7am, so it is open to all six partners at no point cost. First to claim gets it. There is no yearly limit." Same four facts, different words, and it predates this entry. The new sentences were added to both, identically, rather than quietly rewriting the screen to match here.

**CTA**
> Claim Today, Free
Chars: 17 | "Free" in the button, because a partner who has learned that everything costs points will otherwise hesitate.

---

## Status Badge Labels (ties to `11-component-specs.md`)

| Status | Label | Chars | Rationale |
|---|---|---|---|
| checked-out | `Checked Out` | 11 | The state between a submitted pre-departure checklist and a submitted post-use one. Plain-language, and it reads correctly whether the partner is on the water or simply hasn't finished the paperwork |
| claimed | `Claimed` | 7 | Used for a standby day rather than `Confirmed`, so a partner scanning Home can tell a free same-day claim from a booking that cost them points |
| qualification-pending | `Pending` | 7 | Shorter than "Qualification Pending" for badge-size constraints; full context is already given by the screen it appears on (Home, Profile) |
| booking-blocked | `Blocked` | 7 | Short, unambiguous; full explanation lives in the M22 messages above, not repeated in the badge |
| confirmed | `Confirmed` | 9 | Standard, matches the persistent-success semantic decided in `07-color-system.md` |

## Post-Use Submitted (M16, SOW A.11)

**Heading**
> Trip closed

Chars: 11 | Names what happened to the partner's thing, not what happened in the system. "Checklist submitted" describes the form; "Turnaround started" describes the contractors' day. The partner came here to finish a trip.

**Body**
> Thanks, that is everything we need. Halcyon is booked in for her turnaround and the team has been told.

Chars: 106 | Two facts, in the order the partner cares about: nothing more is owed by them, and someone else has already picked it up. "The team", never the contractor's name, per the Naming Rule.

**Fuel acknowledgement, shown only when the tank was reported as not full**
> Fuel shortfall received. If anything needs sorting, we'll be in touch.

Chars: 71 | Matt asked for "Fuel shortfall received, OC will make contact with you." The contact promise was made conditional, and that is the one deliberate departure. The line has to appear on **every** not-full submission, because one that appeared only past the 20-litre tolerance would let a partner infer the figure C.19 says is never stated. Appearing every time, the unconditional promise would be a lie to the partner five litres short, whom nobody will ring. Conditional, it is true either way and still says the thing Matt wanted said: we have it, and you will hear from us if it matters.

Deliberately absent: the litres, any figure at all, and any statement about whether a charge follows. The partner typed the litres in themselves one screen ago; repeating the number back would add nothing and start a negotiation the app has no part in.

## Naming: Standby vs Unclaimed Weekend Access

Two features, never one. They are easy to confuse from 2026-09-19, when a same-day claim under Unclaimed Weekend Access started landing at zero points: to a partner, both then look like nothing was booked and the day came free that morning. Every surface names the feature it belongs to.

| Surface | Standby | Unclaimed Weekend Access |
|---|---|---|
| Calendar cell | green fill, `TODAY` label | dashed border, no TODAY treatment of its own |
| Claim screen | M28, "Standby, Mon to Thu" | the standard booking flow, M08 |
| Booking type | `Standby Claim` | `Unclaimed Weekend` / `Unclaimed Long Weekend` |
| Status badge | `Claimed` | `Confirmed` |
| Cancellation | M29, TMP stood down | M29, points forfeited |

Never write "free day", "free claim" or "today, free" as a name for either. Free is a price both can reach, not a feature, and a label built on the price is the one label that stops telling them apart on the day it matters.

## Boat-Ready Status on Home (M03, SOW C.31)

| State | Copy | Chars | Rationale |
|---|---|---|---|
| ready | `Ready for your trip` + trip date | 19 | Names the trip, not the boat, because a partner can hold 2 concurrent bookings and the trigger fires per booking |
| not ready yet | `Boat not ready yet` + trip date | 18 | Matt's own words, kept verbatim. Says the one thing the partner needs on the morning of a trip, without a time estimate the platform cannot honestly give: nothing in scope tracks how far through a turnaround a contractor is |
| not ready yet, supporting line | `We'll let you know as soon as it's ready.` | 41 | Tells the partner no action is theirs. Without it, a status with no CTA reads as a problem to chase, and the only thing to chase would be Matt's phone |

Deliberately not written: any wording that names the contractor, the turnaround step, or an expected time. The partner has no view of any of it (C.31 adds none), so naming it would raise a question the app cannot answer.

## Points Balance Eyebrow Label (ties to `09-typography-system.md`)

> Points Balance
Chars: 14 | Plain noun label, tracked-uppercase per the Mono treatment — kept short since Mono tracking doesn't wrap gracefully at this size, per that doc's own handoff note.

---

## Post-Use Checklist Photo Copy (M16)

**Do not reword either string below.** Both are confirmed verbatim in the SOW (A.11) and are the only
copy in the app in that position. The draft that previously sat here ("Photos help Matt keep Halcyon in
shape" / "These photos go to Matt in the Admin Portal only…") was written before the client confirmed
the wording and is superseded: it named an individual, and it framed the ask as upkeep and surveillance
rather than as an invitation.

**Screen heading (above the optional photo fields)**
> Any great shots from your day out? Add them here, we'd love to see them
Chars: 71 | Confirmed by the client. Frames the extra photos as something the partner might want to share,
which is what they are: this field is optional, unlike the fuel and damage photos above it.

**Footer disclaimer (below the photo fields, before Submit)**
> Photos won't be shared without your approval
Chars: 44 | Confirmed by the client. Answers the one thing a partner actually worries about, in one line.

The 4-week auto-delete (A.11 / C.24) is deliberately **not** surfaced here. It is storage behaviour, not
something the partner acts on, and adding it would undercut an invitation with a retention notice.

---

## Profile (M07) — Secondary operator

A padlock on the group heading carries the "you cannot change this" signal, which frees every line below it to say something the partner does not already know. Earlier drafts spent the whole block restating that the field was locked.

**Group heading**
> Secondary operator

Chars: 18 | Named for what it holds, not for who administers it. "Managed by Offshore Collective" described the org chart; a partner scanning the screen is looking for the thing, not for its owner.

**Empty state value**
> None added

Chars: 10 | Not "Not set", which reads like a form the partner failed to finish. Most partners will never add one, so the empty state must not feel like an outstanding task.

**Empty state supporting line**
> Someone else can operate Halcyon when you are aboard. Contact us to add one.

Chars: 75 | What the field is for, then how to get one, in two short sentences. It read "on your behalf" until 2026-09-19, which stated the opposite of the rule the client then confirmed: a secondary operator may only operate Halcyon with the partner on board. "On your behalf" means in your place, so the line was promising a partner they could send someone in their stead, which is the single most expensive thing this block could get wrong. "Add one" also carries the cap without spending a sentence on it: one per partner (2026-09-19). "Contact us", never a name, per the Naming Rule below. An earlier draft also explained that we confirm the operator's Powerboat Training NZ status first; that is our process, not a step the partner takes, and it made a two-line block into a four-line one. A partner who adds an operator hears about the check from us.

**Populated state, closing line**
> They can operate Halcyon when you are aboard. Contact us to change any of these.

The rule is repeated here, not only in the empty state, because this is the state a partner reads just before relying on it. A partner with no operator is reading about a hypothetical; a partner looking at a name and a Certified badge is deciding what that person can do on Saturday.

Chars: 34 | "any of these" rather than "this", because three values are shown and a partner may want to change only the phone number.

 . ---

## Naming Rule (applies to every screen)

No individual is ever named in partner-facing copy, and no contractor is named either.

| Instead of | Write | Why |
|---|---|---|
| "Matt is confirming…" | "We're confirming…" | The partner's relationship is with Offshore Collective, not with a named employee. Copy that names a person has to be rewritten the day that person changes role. |
| "contact Matt" | "contact us" | Same reason, and it is shorter. |
| "we'll tell Tamaki Marine Park" | "we'll notify the team" | The partner has no relationship with the contractor and cannot act on the name. |
| "alerts Matt in the feed" | "alerts Admin" | In admin-facing or spec text, name the **role**. |

Two deliberate exceptions, both outside partner copy: the SOW and spec prose still name Tamaki Marine Park and Marine Detailing Co, because a developer has to know which contractor is wired to which trigger; and document headers still name the client.

The Support contact block is a role, not a person: **Offshore Collective / Owner Support**, with a support phone and inbox. SOW A.17 makes these fields admin-editable, so the design must not imply they belong to one individual.

---

## Confirmed As-Is (from earlier drafts)

The following copy drafted in `12-form-specs.md` and `13-state-gallery.md` was reviewed against the Quiet Harbour tone and is confirmed final without changes — listed here rather than duplicated:

- All Pre/Post-Departure Checklist field labels and error messages (`12-form-specs.md` Forms 1–2) — already actionable and jargon-free.
- Standby Claim field copy (`12-form-specs.md` Form 3), except the screen copy and the two cancellation dialogs, now finalized above.
- Edit Profile field copy (`12-form-specs.md` Form 4). The Secondary Operator form it used to sit beside is gone: the field is view-only per A.16, and its copy is finalized above under "Profile (M07), Secondary operator".
- Sign-in error, offline banners, and the Notification Center empty state (`13-state-gallery.md`) — reviewed and kept as drafted; all already follow the "name the problem + give a recovery action" rule.

One small addition to `13-state-gallery.md`'s Notification Center empty state, tightened for tone consistency:

> Nothing here yet
Chars: 15 | Body: "You'll see booking updates and reminders here as they happen." (Chars: 62) | Matches the calm, factual register of the Home empty state above rather than the earlier draft's slightly longer phrasing — small tightening, not a rewrite.

---

## Handoff
Feeds `alice-motion-design` (SKILL 10, the gold-chevron confirmation motion pairs with the Booking Confirm CTA copy above) and `alice-screen-design` (SKILL SC, where all copy above gets placed into real screens).
