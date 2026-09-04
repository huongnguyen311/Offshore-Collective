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
| M11 Payments & Invoices | ✔ | ✔ | ✔ | ✔ | ✔ | ✔ | — | — |

`no-permission` on M04 routes to the already-specified M20 Qualification Pending Gate — no new screen invented. `error` on M08 routes to the already-specified M22 Booking Blocked — likewise no new screen. M22 is now rendered in `screens-preview.html` as 6 variants, one per rule in FLOW-BOOK-01 Case 2 (points, 60-day window, 2-booking/7-day cap, back-to-back, long-weekend cap, named-holiday fairness), so every rule this project enforces has an on-screen explanation to point to.

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
- Pattern: skeleton — placeholder blocks for name/contact/qualification status/secondary operator section.

**Error**
- Type: server (profile fails to load). Message: "Couldn't load your profile." Recovery: Retry.

**Offline**
- Show cached profile data read-only; Edit Profile (M18) is disabled while offline with a note: "You can't edit your profile while offline."

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

### M11 — Payments & Invoices

**Loading**
- Pattern: skeleton list of placeholder invoice rows.

**Empty**
- First-time partner with no invoices yet: icon + "No invoices yet — they'll appear here once Matt sends your first one."
- CTA: none required (same exception as Notification Center — invoicing is entirely Matt-initiated; there's no partner action to prompt here). State the exception rather than inventing a fake CTA.

**Partial**
- Pagination: infinite scroll for invoice history, same pattern as Notification Center.

**Error**
- Type: server. Message: "Couldn't load your invoices." Recovery: Retry.

**Offline**
- Show cached invoice list (read-only, which this screen always is anyway) with a banner: "Offline — showing last-known invoices."

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
| M11 Payments & Invoices | First-run empty state (above) applies to any partner before their first Xero invoice is sent — not tied to onboarding specifically, since invoicing timing is Matt's, not the app's. |

---

## Handoff
Feeds `alice-screen-design` (SKILL SC), which renders each of these states as real screens, and `alice-design-qa` (SKILL 13), which checks every state specified here actually got built.
