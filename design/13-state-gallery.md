# Offshore Collective — State Gallery (Partner Mobile App)

**Consumed inputs:** `01-information-architecture.md` (Mobile IA — Core screens + already-identified System screens M20-M24), `11-component-specs.md` (CalendarDateCell, StatusBadge, PointsBalanceDisplay)

---

## 1. State Matrix

| Screen | loading | empty | partial | full | error | offline | success | no-permission |
|---|---|---|---|---|---|---|---|---|
| M02 Login | ✔ | — | — | ✔ | ✔ | ✔ | — | — |
| M03 Home | ✔ | ✔ (→ M21) | — | ✔ | ✔ | ✔ | — | — |
| M04 Booking Calendar | ✔ | — | — | ✔ | ✔ | ✔ | — | ✔ (→ M20) |
| M05 Boat Rules | ✔ | — | — | ✔ | ✔ | ✔ | — | — |
| M06 Notification Center | ✔ | ✔ | ✔ | ✔ | ✔ | ✔ | — | — |
| M07 Profile | ✔ | — | — | ✔ | ✔ | ✔ | — | — |
| M08 Booking Review & Confirm | — | — | — | ✔ | ✔ (→ M22) | ✔ | ✔ | — |
| M09 Booking Detail | ✔ | — | — | ✔ | ✔ | ✔ | — | — |
| M13 Christmas Window | ✔ | — | — | ✔ | ✔ | ✔ | ✔ | — |
| M28 Standby Claim | ✔ | — | — | ✔ | ✔ | ✔ | ✔ | — |

`no-permission` on M04 routes to the already-specified M20 Qualification Pending Gate — no new screen invented. `error` on M08 routes to the already-specified M22 Booking Blocked — likewise no new screen. M22 is now rendered in `screens-preview.html` as 7 variants, one per rule the engine enforces (points, 60-day window, 2-booking/7-day cap, back-to-back, long-weekend cap, named-holiday fairness, and Friday-alone per SOW C.4), so every rule this project enforces has an on-screen explanation to point to.

---

## 2. Per-State Spec

### M02 — Login

**Loading**
- Pattern: spinner on the Sign In button only (not a skeleton — there's no content to shimmer on a login screen), button disabled during the request.
- Timeout: after 10s with no response, surface the error state below rather than spinning indefinitely.

**Error**
- Type: validation (bad credentials) — already specified in the IA as M23, inline on M02: plain-language message that doesn't reveal which field was wrong. Recovery: edit and retry, no other action needed.
- Type: network — if the sign-in request itself can't reach the server (distinct from bad credentials), show "Can't connect right now — check your connection and try again" with a Retry action. Preserve the entered email so only the password needs re-entry.

**Offline**
- Persistent banner: "You're offline — sign in once you're back online." Sign In button stays disabled while offline is detected, rather than letting the request fail silently.

---

### M03 — Home

**Loading**
- Pattern: skeleton mirroring the final layout — a placeholder block where the Eyebrow Label + Data Hero points balance sits, a placeholder line for the next-booking summary, and three placeholder pills for the quick-links row. Shimmer, not spinner, per the skeleton-preferred rule.
- Timeout: after 8s, fall back to the error state.

**Empty**
- This is M21 (Empty — No Upcoming Booking), reached via the dashed IA connection from M03, and now rendered in `screens-preview.html`. Per `06-art-direction.md` Signature Moment #2: a line-art rendering of the icon's chevron pointing toward the Book tab, with copy "Nothing booked yet — your next trip starts here."
- CTA: taps through to M04 Booking Calendar (already specified in the IA, satisfying the mandatory-CTA rule).
- The points balance (Data Hero) still renders normally in the empty state — only the next-booking line is replaced; the balance is never itself an "empty" concept.

**Boat-ready status (three states, SOW C.31)**
- Added 2026-09-19 per the C.31 comment on SOW row 94, replacing a present/absent pair. Built as part of the boat card, not as a screen-level state, so it composes with every state below.
- `ready`: "Ready for your trip" + trip date, tick icon, success tint. Set by the last turnaround step for that trip: MDC's Mark Job Complete on a back-to-back, TMP's Confirm Launch on a haul-out (C.10).
- `not-ready-yet`: "Boat not ready yet" + trip date, clock icon, warning tint, plus the line "We'll let you know as soon as it's ready." Condition: there is a booking today and that trigger has not fired. No CTA: nothing is being asked of the partner, and there is no partner-facing view of the turnaround to link to.
- `absent`: no row. Condition: no booking today. This is the only case left where the card says nothing, which is what stops the row becoming permanent furniture.
- Not a loading state: it reflects a real-world trigger, not a fetch, so it never renders as a skeleton and never times out into the error state.

**Error**
- Type: server (points balance or booking data fails to load). Message: "Couldn't load your dashboard right now." Recovery: Retry button, re-fetches without a full app restart.
- What's preserved: nothing to preserve (Home has no in-progress user input).

**Offline**
- Show the last successfully-cached points balance and next-booking summary with a persistent banner: "Showing your last known balance — reconnect to update." Quick-links to Book/Checklist remain tappable but the destination screens handle their own offline state (e.g. M04 blocks new bookings offline, see below).

---

### M04 — Booking Calendar

**Loading**
- Pattern: skeleton calendar grid — 7×5 placeholder rounded-square cells shimmering, matching the CalendarDateCell component's exact shape so there's no layout jump on load.

**No-permission**
- Routes to the already-specified M20 (Qualification Pending Gate) rather than rendering a blank/blocked calendar. Per FLOW-ONBOARD-01: reassuring copy explaining that booking unlocks once Matt confirms the Powerboat Training NZ certification — never a bare "access denied."

**Error**
- Type: server (calendar data fails to load). Message: "Couldn't load the calendar." Recovery: Retry, reloads the same boat's calendar.
- Type: rule-violation on a selection — routes to the already-specified M22 (Booking Blocked), which is a state of the *selection*, not the whole screen; the calendar itself remains rendered underneath.

**Offline**
- The calendar can render from cache (last-fetched availability) but with a persistent banner: "Offline — showing last-known availability. You can't confirm a booking until you're back online." The Confirm action at M08 is disabled/unreachable while offline, preventing a booking from being made against stale availability data.

---

### M05 — Boat Rules

**Loading**
- Pattern: skeleton with placeholder topic-group headers and a few placeholder text lines per group, mirroring the grouped-by-topic layout.

**Error**
- Type: server (content fails to load). Message: "Couldn't load the boat rules." Recovery: Retry.

**Offline**
- Boat Rules content is static/rarely-changing — cache aggressively and show cached content with no banner at all if a recent cache exists (this is the one screen where offline degradation should be invisible to the partner, since stale rules content carries little risk of harm, unlike stale calendar availability).

---

### M06 — Notification Center

**Loading**
- Pattern: skeleton list — 5-6 placeholder rows shimmering, matching the final list-row height.

**Empty**
- First-time user with no notifications yet: icon + "Nothing here yet — you'll see booking updates, alerts, and reminders as they happen."
- CTA: none required here specifically — per the IA audit finding in `01-information-architecture.md`, this screen has no further action to point to (it's not a dead end in the harmful sense; there's simply nothing to do until a notification arrives). This is the one exception to the "always a CTA" rule, since there's no plausible next action beyond "wait" — flag this exception explicitly rather than inventing an artificial CTA.

**Partial**
- Pagination: infinite scroll, loading the next page of older notifications as the partner scrolls down. Loading indicator: a small inline spinner at the list's bottom edge (not a full skeleton) while the next page fetches.

**Error**
- Type: server (list fails to load). Message: "Couldn't load your notifications." Recovery: Retry.

**Offline**
- Show cached notifications (already delivered/seen ones) with a banner: "Offline — you may be missing recent updates."

---

### M07 — Profile

**Loading**
- Pattern: skeleton, with placeholder blocks for the identity card (name, boat, then the qualification, phone and email rows) and the "Secondary operator" card. The Edit control is not drawn until the real values are in, so nothing invites a tap into a card that has no content yet.

**Edit mode** *(SOW A.16)*
- One field: `name`, as an input in place at the top of the identity card. Qualification, phone, email and the whole Secondary operator card render exactly as they do in view mode (phone joined email as admin-maintained on 2026-09-19).
- No explanatory line beneath the phone and email rows: one was specified and then removed on 2026-09-19 at the client's direction. The rows state the values and nothing else, in both modes.
- The control that opens this mode is an unlabelled pencil sitting against the name itself, not at the card corner. It governs one field and its position is what says so.
- Cancel/Save appear beneath the card. Cancel with an unsaved change raises the discard confirmation; Cancel with nothing changed exits silently.
- Error: empty name, inline beneath the input. It is the only error this screen can produce.

**Secondary operator, not set** *(the default for most partners)*
- "None added", followed by one line saying what the field is for and how to add one: "Someone else can operate Halcyon on your behalf. Contact us to add one."
- This is not an error or a warning state, and must not be styled as one. Most partners will never add a secondary operator.

**Secondary operator, set**
- Three rows, per A.16: Name, Contact, Powerboat Training NZ status. The status is a StatusBadge, not plain text, because it is the one value a partner comes here to check.
- Closing line: "Contact us to change any of these."
- Both states keep the padlock on the group heading. It is the heading, not the body copy, that carries the read-only signal.

**Editing** *(a screen state, not a field state)*
- Reached from the Edit control at the top right of the identity card. The three values become inputs in place and a Cancel/Save pair appears beneath them; nothing else on the screen moves or changes.
- The qualification row and the "Secondary operator" card render identically to view mode. This is the state's main job: it shows the partner exactly where their control ends.
- Error variant: any field can carry an inline error beneath it, with the field keeping its error border even while focused.
- Exit states: Save (returns to view with the new values), Cancel with nothing changed (returns straight to view), Cancel with changes (confirmation dialog first, see `12-form-specs.md` Form 4).

**Error**
- Type: server (profile fails to load). Message: "Couldn't load your profile." Recovery: Retry.

**Offline**
- Show cached profile data read-only; the Edit control is disabled while offline with a note: "You can't edit your profile while offline." If the partner is already in edit mode when the connection drops, stay in edit mode and keep what they typed, disabling Save only; dropping them back to view mode would discard work they can still see on screen. The qualification row and the secondary operator card need no offline treatment, they are read-only on every connection state.

---

### M08 — Booking Review & Confirm

**Error**
- Type: rule-violation — routes to the already-specified M22 (Booking Blocked), per FLOW-BOOK-01 Case 2.
- Type: network (the confirm action itself fails to reach the server after the partner taps Confirm). Message: "Couldn't confirm your booking — nothing has been booked or deducted yet." Recovery: Retry. Critically, the points balance must NOT be deducted client-side optimistically before server confirmation, so a failed confirm leaves the partner's balance untouched — state this explicitly since it's the one place a "success-looking then silently failing" bug would be most damaging to trust.

**Offline**
- Confirm button disabled while offline, with inline copy: "You need to be online to confirm a booking."

**Success**
- Per `11-component-specs.md`'s PointsBalanceDisplay count-down tween: the deduction animates on this same screen before transitioning to M09, rather than an intermediate "toast" — the ticking number IS the success confirmation (Signature Moment #5), paired with the gold-chevron motion (Signature Moment #4) as the transition into M09.

---

### M09 — Booking Detail

**Loading**
- Pattern: skeleton mirroring booking summary layout (boat name, dates, StatusBadge placeholder, action buttons placeholder).

**Error**
- Type: server (booking detail fails to load). Message: "Couldn't load this booking." Recovery: Retry.

**Offline**
- Show cached booking detail read-only; Cancel Booking (M17) and checklist-start actions are disabled while offline with copy: "You need to be online to cancel a booking or start a checklist."

---

### M13 — Christmas Window

The screen has one structural state the others do not: exactly one of the three year cards is the partner’s, and the other two are permanently empty. That is the full state, not a partial load, so nothing on this screen ever renders as "not yet assigned" once the draw has happened.

**Loading**
- Pattern: skeleton on the three year cards. The cost readout (14 pts) renders immediately, it is a fixed rule, not fetched data.

**Full, before the draw**
- All three years render in the not-applicable treatment with a single line: the draw happens once the boat is delivered and all six partners are Stage 3 Confirmed. No release action exists on the screen in this state.

**Full, after the draw**
- One year card carries the assigned window, the raised elevation, the "Your week" check on Window 1 or Window 2, and the release footer. The other two years are flat, tinted, and carry no window and no action. The release button appears on the assigned year only, never on all three.

**Success (release confirmed)**
- The assigned year card drops back to the not-applicable treatment, the release footer disappears, and the points either return to the balance or do not, per the branch taken. A partner has no second week to release, so there is no path back into this state.

**Error**
- Type: server (release fails after confirm). Message: "We couldn’t release your week. Nothing has changed, please try again." Recovery: Retry. Stating that nothing changed matters more here than on any other error in the app, because the partner has just been told their points are at stake.

**Offline**
- Year cards render from cache. The release button is disabled with: "You need to be online to release your week." The release notifies five other partners at the moment it lands (C.9), so it cannot be queued for later.

**Confirmation dialog, two states**
- Refund (more than 14 days out): navy icon, navy confirm button, copy in [`14-ux-writing.md`](14-ux-writing.md).
- Forfeit (inside 14 days): error-tinted icon, red confirm button carrying the cost. Which one renders is decided by the date, not by the partner, so the two are never both reachable.

---

### M16 — Post-Use Checklist

**Success** *(added 2026-09-19, closing the gap logged against M16 in `01-information-architecture.md`)*
- A submitted state, not a toast: two facts have to survive the screen, that the trip is closed and that the turnaround has been started with the contractors, and neither was stated anywhere before.
- Quiet, not celebratory. The partner has finished tidying up after a trip, not achieved something.
- **Fuel acknowledgement (A.11):** one note, shown whenever the tank was reported as not full. Never shown on a full-tank submission, and never varied by how short the tank was. It carries no litres, no dollar figure, and nothing about whether a charge follows.
- The note's visibility is keyed to the fuel answer, never to the tolerance result. Keying it to the tolerance would publish the 20-litre figure by implication, which C.19 forbids.
- Single CTA back to Home. No path onward into anything else: the trip is over.

### M28 — Standby Claim

This screen is unusual in that it can go stale between opening and submitting: standby is first-confirmed (SOW A.19), so another partner can take the day while this one is deciding. That race is the screen's real state problem, not loading.

**Loading**
- Pattern: skeleton on the summary card and the points rows. The zero-cost hero renders immediately, since 0 is not data that has to be fetched.

**Empty**
- Not applicable. The screen is unreachable unless a standby day exists; there is no version of it with nothing to show.

**Taken while open (app-specific state, not in the standard set)**
- Trigger: another partner confirms the claim first, or 11:59pm passes and the day lapses.
- Treatment: replace the claim button with a non-dismissible inline notice, "Someone else claimed today first. Standby is first come, first served." Single action: Back to Calendar.
- The partner must never be able to tap Claim and receive a failure afterwards; the button goes before the tap does.

**Error**
- Type: server, on submitting the claim. Message: "Couldn't claim today. Nothing has been taken from your points." Recovery: Retry.
- The second sentence is load-bearing. With a zero-point claim there is nothing to reverse, and saying so prevents the partner from checking their balance in alarm.

**Offline**
- Block the claim outright rather than queueing it: "You need to be online to claim standby." A queued same-day claim could land after the day has gone, and SOW C.28 fires a real-time SMS to Tamaki Marine Park on confirmation, so a deferred claim would page a contractor about a day that no longer exists.

**Success**
- Routes to the booking detail in its Claimed state, with the confirmation that Tamaki Marine Park has been notified. See `screens-preview.html`, M28.

---

## 3. Transitions Between States

| Transition | Motion |
|---|---|
| Skeleton → loaded content (any screen) | `duration.normal` (220ms) fade + slight upward settle, `easing.standard` |
| Error → loading, on Retry tap | Immediate — no fade, spinner appears instantly on the Retry button itself |
| M04 selection → M22 Booking Blocked | Bottom sheet slides up, `duration.normal`, `easing.enter` |
| M08 Confirm → success tween → M09 | PointsBalanceDisplay count-down (`duration.slow`, 360ms) completes, then the signature gold-chevron transition (`duration.confirm`, 560ms, `easing.confirm`) carries into M09 |
| Online → offline banner appearing | Banner slides down from top, `duration.fast` (120ms) |

All durations/easings reference `tokens/foundations.json` — no new raw values introduced here.

---

## 4. First-Run vs Returning User

| Screen | First-run difference |
|---|---|
| M03 Home | A brand-new partner (persona Priya) lands on M03 immediately after first sign-in with qualification still Pending — the points balance still renders (58 points, full allocation) even though booking is locked at M04; this is intentional, so she can see what she's working with while waiting on Matt. |
| M06 Notification Center | First-run empty state copy (above) is written for a partner who has genuinely never received a notification — distinct from a returning user's ordinary "no new notifications right now" case, which would show the existing (now-read) list rather than the empty state at all. |
| M28 Standby Claim | No first-run difference. A partner can claim standby from their first day qualified, and there is no cap or history that changes the screen over time (SOW A.19). The screen is only reachable Monday to Thursday (2026-09-19), so a partner qualified on a Friday first sees it the following Monday at the earliest. |

---

## Handoff
Feeds `alice-screen-design` (SKILL SC), which renders each of these states as real screens, and `alice-design-qa` (SKILL 13), which checks every state specified here actually got built.
