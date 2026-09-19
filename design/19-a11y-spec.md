# Offshore Collective — Accessibility Spec (Partner Mobile App)

**Consumed inputs:** `17-screen-design.md`, `11-component-specs.md` + `18-figma-specs.md`, `12-form-specs.md`, `07-color-system.md` (WCAG matrix — referenced, not recomputed)

**Platform adaptation note:** this is a native mobile app, not a web page — there is no DOM `tabindex`, no HTML landmarks (`header`/`nav`/`main`), and no "skip to content" link pattern. Section 2–4 below translate the skill's web-oriented structure to its native mobile equivalents: **Focus Order** = VoiceOver (iOS) / TalkBack (Android) swipe-navigation order; **Keyboard Map** = external keyboard + Switch Control (iOS) / Switch Access (Android) map; **Landmarks** = accessibility container groupings + heading-level semantics via `accessibilityTraits`/`accessibilityHeading` (iOS) and `accessibilityHeading`/content grouping (Android), not HTML tags.

---

## 1. Conformance Target
**WCAG 2.2 Level AA** for all screens. One AAA target called out explicitly: the M08 Booking Confirm flow and M22 Booking Blocked messaging (per `07-color-system.md`, several of these color pairs already reach AAA — hold that bar since this is the flow where a misread has real financial-fairness consequences for the partner).

---

## 2–5. Per-Screen Specs

### M03 — Home

**Focus order (VoiceOver/TalkBack swipe order):**
```
1. Support, button (top bar)
2. Notifications, button (top bar; unread state announced, never signalled by the dot alone)
3. Eyebrow Label + Data Hero, announced together as one unit ("Points balance, 5 of 58 points left this year")
4. Boat identity + ready status, announced as one unit ("Halcyon, Rayglass 3000, ready for your trip Sat 14 Mar")
5. Upcoming booking card 1 (date, booking type, StatusBadge)
6. Upcoming booking card 2
7. Tab bar: Home (current) → Book → Rules → Profile
```

**Keyboard/Switch Control map:** external keyboard Tab moves through items 1–7 in the same order; Enter/Space activates a focused button, booking card or tab; Switch Control scanning follows the identical order (no custom scan groups needed — the screen is a single linear flow).

**Landmarks + heading outline:**
```
Heading level 1: "Home" (screen title, announced but not visually duplicated — the Data Hero is content, not the heading)
  Heading level 2: "Upcoming — Halcyon" (the card title)
```

**Screen-reader flow:**
```
On load: "Home, heading level 1. Support, button. Notifications, 1 unread, button.
          Points balance, 5 of 58 points left this year, resets 12 June.
          Halcyon, Rayglass 3000, ready for your trip Sat 14 Mar.
          Upcoming, heading level 2. Sat 14 Mar, single day, Confirmed. Fri 20 to Sun 22 Mar, weekend block, Confirmed."
On tab change (to Book): focus moves to the M04 heading, per Section 6.
```

**Contrast:** Data Hero (`color.text.brand` on `color.bg.default`) and body text all pass AA per `07-color-system.md`; no new combinations introduced on this screen.

**Boat-ready status (SOW C.31):** this status exists precisely because a push notification may never reach the partner, so it must carry a text label alongside its icon and tint, and be part of the boat card's single announced unit. It must never be conveyed by fill colour alone. It has three states since 2026-09-19 (third state added per the C.31 comment on SOW row 94):

| State | Label | Icon | Tint | When |
|---|---|---|---|---|
| Ready | "Ready for your trip" + trip date | tick | success | the trip's ready trigger has fired |
| Not ready yet | "Boat not ready yet" + trip date, plus the line "We'll let you know as soon as it's ready." | clock | warning | there is a booking today and its ready trigger has not fired |
| Absent | no row at all | none | none | no booking today |

The two visible states differ in wording and icon shape, not only in tint, so they survive a monochrome or colour-blind reading. The original ban still holds in its real form: "not ready" is never a muted or greyed rendering of "ready", it is its own labelled state or nothing at all. Absence now means only "you have no trip today", which is why it is safe to leave silent.

The label names the trip it belongs to ("Ready for your trip Sat 14 Mar", date in `font.mono`). A partner can hold 2 concurrent bookings and TMP confirms launch per booking, so an undated label is ambiguous as soon as there is a second upcoming trip. The date sits inside the same announced unit rather than in a separate element, so the screen reader still reads boat and status as one phrase: "Halcyon, Rayglass 3000, ready for your trip Sat 14 Mar", or "Halcyon, Rayglass 3000, boat not ready yet, Thu 12 Mar, we'll let you know as soon as it's ready". The ready state is set by the last step of the turnaround, and which step that is depends on the path: TMP's Confirm Launch (C05) on a haul-out, MDC's Mark Job Complete (C04) on a back-to-back, where the boat never leaves the water and TMP has no step at all (C.10, confirmed 2026-09-18; this corrects an earlier reading of C04 as an internal hand-off on every path). It is cleared when the partner checks out or when the post-use checklist starts the turnaround. On a trip day the row always describes that day's trip: a ready status belonging to a different booking is never shown on top of today, which is the ambiguity the third state exists to remove.

---

### M04 — Booking Calendar

**Focus order:**
```
1. Screen heading ("Booking Calendar")
2. Month label ("March 2026")
3. Month stepper (previous month, month label, next month; a disabled end is announced as disabled, not omitted)
4. Calendar grid — swipe order is row-by-row, left to right, each cell announced with its full state label
5. Rule note (live region; fires when a tap triggers an explanation, a repricing, or a block)
6. Selected-dates summary row (when a selection is active)
7. Points-required row (when a selection is active)
8. "Review Booking" button (disabled, and announced as disabled, until a selection is both present and permitted)
```

**Keyboard/Switch Control map:** Arrow keys move focus between adjacent calendar cells (per `11-component-specs.md` Section 5 — this is the one screen where arrow-key navigation is specified at the component level, not just linear Tab order); Enter/Space selects a cell; Escape cancels an in-progress range selection. Switch Control: group the calendar grid as a single scan group with row/column scanning enabled (Switch Control's built-in grid-scanning mode), rather than forcing a full linear scan through 30+ individual cells.

**Landmarks + heading outline:**
```
Heading level 1: "Booking Calendar"
  Heading level 2: "March 2026" (implicit via the section label — mark as heading level 2 if the component renders it as a visually distinct section label, per `10-responsive-layout.md`). The boat is not named here: the fleet is one boat, the partner co-owns exactly one, and M03 already carries that identity, so repeating it costs a swipe stop on every visit to the calendar.
```

**Screen-reader flow:**
```
On load: "Booking Calendar, heading level 1. March 2026.
          March 1, not bookable. ...
          March 4, out of service, not bookable. ...
          March 6, booked, not available. ...
          March 12, today, standby available, free, button. ...
          March 27, unclaimed weekend, lower rate, button. ..."
On selecting a range: "Selected, March 15 to March 16. Points required, 2."
On claiming from an unclaimed block: "Unclaimed weekend, March 28 to March 29. Points required, 4." followed by the live-region note explaining the claim-to-the-end-of-block rule.
On selecting a lone Friday: "Friday alone. Points required, unavailable." followed by the live-region note, and Review Booking is announced as dimmed.
On tapping a non-selectable cell: the live-region note fires with the explanation; the cell itself stays non-interactive, so no activation event is announced.
```

**Motion/2.3.3 check:** the selection-ring transition (`11-component-specs.md`, `duration.fast`/`easing.standard`) is a border-color change only, not a parallax/motion-triggered effect — it does not fall under 2.3.3's "motion actuation" concern at all, but the reduced-motion fallback (instant border-color change, per `15-motion-design.md` Section 4) is still honored for users with `prefers-reduced-motion`/`isReduceMotionEnabled` set, satisfying the stricter bar even though 2.3.3 itself doesn't strictly require it here.

---

### M05 — Boat Rules

**Focus order:**
```
1. Screen heading ("Boat Rules")
2. "Points & Reset" section heading → its body text
3. "Booking Types & Cost" section heading → its body text
4. "Timing Limits" section heading → its body text
5. "Cancellation" section heading → its body text
6. Tab bar
```

**Keyboard/Switch Control map:** standard linear Tab/scan order; no custom grouping needed (this is a long-form reading screen, not an interactive grid).

**Landmarks + heading outline:**
```
Heading level 1: "Boat Rules"
  Heading level 2: "Points & Reset"
  Heading level 2: "Booking Types & Cost"
  Heading level 2: "Timing Limits"
  Heading level 2: "Cancellation"
```
Each `rule-group` heading (`09-typography-system.md`'s `titleSmall`-equivalent) must be marked as an actual heading trait, not just styled text — this is the one screen most likely to have a developer skip semantic heading marking since the visual styling alone "looks like" a heading; call this out explicitly as a common implementation miss.

**Screen-reader flow:**
```
On load: "Boat Rules, heading level 1. Points and Reset, heading level 2. Each partner gets 58 points a year... "
Swiping through: each section heading followed immediately by its full body paragraph, read as one continuous unit (not broken into per-sentence stops).
```

---

### M06 — Notification Center

**Focus order:**
```
1. Screen heading ("Notifications")
2. Notification row 1 → row 2 → row 3 → ... (chronological, matches visual order)
3. (On scroll, next page loads — see Section 6, Focus Management)
4. Tab bar
```

**Keyboard/Switch Control map:** linear Tab/scan through rows; Enter/Space on a row opens that notification's destination directly (M09, M15, M16 or M04, per the IA). There is no intermediate detail screen, so every row must carry its full message as visible text: nothing is held back for a second screen to reveal.

**Landmarks + heading outline:**
```
Heading level 1: "Notifications"
```
No sub-headings — the list itself is the content; individual rows are list items, not headings (a common mistake would be marking every notification title as a heading, which produces a meaningless multi-hundred-heading outline over a partner's history — explicitly avoid this).

**Screen-reader flow:**
```
On load: "Notifications, heading level 1. List, [N] items.
          Booking confirmed, Halcyon, 6 to 8 March. 2 hours ago. Button.
          Halcyon is ready, TMP confirmed launch. Yesterday. Button. ..."
On reaching the end of the loaded list (infinite scroll): "Loading more notifications" announced via a live region before the next page's items are inserted, so a screen-reader user isn't left wondering why swiping stopped producing new items.
```

---

### M07 — Profile

**Focus order — view mode:**
```
1. Screen heading ("Profile")
2. Name + boat (announced as one unit, not two separate stops: "David Kearney, Halcyon, Rayglass 3000"). There is no avatar: initials in a coloured square carried no information a screen reader could use and no information the name below it did not already give. The boat mark drawn beside the vessel from 2026-09-19 is decorative and must carry `aria-hidden`: it repeats what the two lines next to it already say, and announcing it would split one unit into two stops
3. Edit (icon button, no visible label, `aria-label="Edit your name"`, opens edit mode in place; does not navigate). It follows the name in the DOM as well as on screen, so the value and the control that changes it are adjacent in the swipe order
4. Qualification status row
5. Phone row
6. Email row
7. Group heading: "Secondary operator" (padlock announced as part of the heading, see below)
8. Secondary operator value: "None added", or Name / Contact / Powerboat Training NZ status when one is set
9. Secondary-operator supporting text ("Someone else can operate Halcyon on your behalf. Contact us to add one.")
10. Menu: Christmas Window
11. Menu: Support
12. Menu: Terms & Privacy
13. Menu: Sign out
14. Menu: Delete Account
15. Support contact footer (name, phone, email, announced as one unit)
16. Tab bar
```

The Edit control now *follows* the name rather than preceding it, because it lost its visible label on 2026-09-19 and is an unlabelled pencil: sighted users read its scope from the fact that it sits against the name, and the swipe order has to reproduce that adjacency rather than announce a bare control first. Its accessible name, "Edit your name", is therefore doing all the work for anyone who cannot see where it sits, and must never be shortened to "Edit".

**Focus order — edit mode:**
```
1. Screen heading ("Profile")
2. Name (text input) ← focus lands here when Edit is activated, caret at end of value
3. Boat ("Halcyon, Rayglass 3000")
4. Qualification status row (unchanged, still static)
5. Phone row (static, not an input)
6. Email row (static, not an input)
7. Cancel (button)
8. Save (button)
9. Group heading: "Secondary operator" (unchanged, still padlocked)
10. Secondary operator value (unchanged, still static)
11. Secondary-operator supporting text
12-16. Menu rows, footer, tab bar as above
```

Items 4, 9 and 10 are the point: they are byte-for-byte what they were in view mode. A partner who wonders why their qualification or their secondary operator did not become typeable gets the answer from the screen itself.

**Mode changes:**
- Activating Edit moves focus to the name input (item 2). Announce the change, either with `aria-expanded` on the Edit button or a polite live-region message ("Editing your details").
- Save, Cancel, and dismissing the discard dialog all return focus to the Edit button.
- A failed Save moves focus to the first invalid field and announces its error.
- The discard confirmation is a modal dialog: focus trapped inside it, Escape maps to "Keep Editing", focus returns to Cancel's origin on close.

**Landmarks + heading outline:**
```
Heading level 1: "Profile"
  Heading level 2: "Secondary operator"
```

The editable group no longer carries a visible heading: it is the card that opens with the partner's own name, and the Edit button's `aria-label` names it instead. The padlocked group keeps its heading, moved inside its card, because that heading is what marks the boundary between what the partner can change and what they cannot.

**Note:** in view mode, items 4, 5, 6, 8, 9 and 15 are static content, not controls. They appear in the swipe order (so the information is reachable) but must not be exposed with a `button` role, and Tab must skip them on an external keyboard. Three points deserve care here. The secondary-operator helper text (9) is the one place a partner is most likely to reach for a control that does not exist, so it must never be given one: A.16 puts that change with Matt, off-platform. The "Secondary operator" heading (7) is now the only heading separating what the partner can edit from what they cannot, so it must be exposed as a heading (level 2), not as plain text, even though it sits inside its card rather than above it, or the distinction is lost to a screen reader. Its padlock is decorative to the eye but load-bearing to the ear: it must reach a screen reader as part of the heading's accessible name ("Secondary operator, not editable"), never as a bare `img` with no label and never as a separate stop. And the phone and email rows (5, 6) are static in both modes, not only in view mode: neither is editable from this screen at all (email from 2026-09-18, phone from 2026-09-19), so neither may ever be exposed as a control. A line explaining where those values are maintained was specified on 2026-09-19 and removed the same day, so the rows are announced as bare label-and-value pairs. This is the one regression in the screen's accessibility worth naming: a screen-reader user who wants a new phone number on file now hears no route at all, and the support contact in the footer (item 15) is the only one there is.

The phone number and email in the footer are the only static items that may become controls: if they are made tappable, each becomes its own labelled link.

**Screen-reader flow:**
```
On load: "Profile, heading level 1. David Kearney, Halcyon, Rayglass 3000.
          Edit your name, button, collapsed.
          Qualification, Approved.
          Phone, 021 555 0198. Email, david dot kearney at xtra dot co dot nz.
          Secondary operator, not editable, heading level 2. None added.
          Someone else can operate Halcyon on your behalf. Contact us to add one.
          Christmas Window, button. Support, button. Terms and Privacy, button. Sign out, button. Delete Account, button.
          Support. Offshore Collective, Owner Support. 021 555 0142. support at offshorecollective dot co dot nz."

On activating Edit: "Editing your name. Name, edit text, David Kearney."
                    (focus is in the name field; the rest of the screen is unchanged and
                     re-reads exactly as above, including the phone and email rows)

On a failed Save:   "Name, edit text, invalid entry, Enter your name."

On Save:            "Edit your name, button, collapsed."
                    (focus is back on Edit, and the rows below read the new values)
```

---

### M08 — Booking Review & Confirm

**Focus order:**
```
1. Screen heading ("Review & Confirm") + back button (announced before the heading, per standard nav-bar order)
2. Booking summary (boat, dates)
3. Points available row
4. This booking uses row
5. Balance after row
6. Disclaimer text ("Points are deducted immediately...")
7. "Lock In These Dates" (accent CTA)
```

**Keyboard/Switch Control map:** linear; Enter/Space on the CTA triggers confirmation (see Screen-reader flow below for the resulting announcement).

**Landmarks + heading outline:**
```
Heading level 1: "Review & Confirm"
```

**Screen-reader flow:**
```
On load: "Review and Confirm, heading level 1. Halcyon, Weekend Block, Fri 6 Mar to Sun 8 Mar.
          Points available, 42. This booking uses, 5. Balance after, 37.
          Points are deducted immediately on confirming... Lock In These Dates, button."
On tapping Confirm (success): "Points balance updating, 42 to 37." announced once via aria-live/live-region as the count-down tween completes (per `15-motion-design.md` Section 3's aria-live spec), THEN "Booking confirmed" announced as focus moves to the M09 heading.
On tapping Confirm (blocked, routes to M22): focus moves into the M22 sheet; see that screen's spec in `13-state-gallery.md`/`14-ux-writing.md` for its own announcement.
```

**Motion/2.3.3 check:** the gold-chevron confirmation transition (`15-motion-design.md` Section 1) is not triggered by device motion/tilt — it's triggered by a discrete tap, so 2.3.3 (Animation from Interactions) applies in its "can be disabled" sense, not its "must not trigger from incidental motion" sense. The reduced-motion fallback (static chevron, 80ms, per that section) satisfies this. **Confirmed: compliant.**

---

### M15 — Pre-Departure Checklist

Full field-level accessibility detail already lives in `12-form-specs.md` Section 6 — this section covers screen-level flow only, not duplicating the per-field spec.

**Focus order:**
```
1. Screen heading ("Pre-Departure Checklist") + back button
2. "Is the tank full?" toggle group
3. (conditional) Litres reading field
4. (conditional) Photo of fuel gauge
5. (conditional) Reason field
6. "Any damage?" toggle group
7. (conditional) Damage description
8. (conditional) Damage photo
9. Estimated return time, announced as a single labelled group of three controls (hour, minutes, AM/PM)
10. "Submit Checklist" button
```

**Keyboard/Switch Control map:** standard linear Tab/scan through currently-visible (non-hidden) fields only — hidden conditional fields must be removed from the accessibility tree entirely when hidden, not just visually hidden, or Switch Control/TalkBack will scan through empty stops.

**Landmarks + heading outline:**
```
Heading level 1: "Pre-Departure Checklist"
```

**Screen-reader flow:**
```
On load: "Pre-Departure Checklist, heading level 1. Is the tank full? radio group, 2 options."
On selecting "No": "Litres reading, edit text, required. Why isn't it full, edit text, required. Photo of fuel gauge, required." — these three announced as newly available, not silently appearing; if the platform doesn't auto-announce newly-inserted focusable content, fire a live-region announcement ("3 more fields required") as a fallback.
On submit with errors: focus moves to the first invalid/incomplete field (per `12-form-specs.md` Section 6), and the error is announced via `role="alert"`-equivalent (iOS: `UIAccessibility.post(.announcement)`; Android: `AccessibilityLiveRegion.ASSERTIVE`).
```

---

### M20 — Qualification Pending Gate

**Focus order:**
```
1. Screen heading ("Almost there")
2. Body copy
3. "Read the Boat Rules" secondary button
```

**Landmarks + heading outline:**
```
Heading level 1: "Almost there"
```

**Screen-reader flow:**
```
On load: "Almost there, heading level 1. We're confirming your Powerboat Training NZ certification.
          Booking opens as soon as that is done. Nothing needed from you.
          Read the Boat Rules, button."
```

**Note:** this screen has no icon-only signal for its "informational, not alarming" status (the `info` color is decorative here) — the heading + body copy alone carry the full meaning, so color is correctly never the sole signal per Rule 6, even without an explicit icon label, since sighted users get the same "info-blue, calm" read from the copy tone as from the color.

---

### M28 — Standby Claim

Reached only from the calendar's today cell, and only when SOW A.19's conditions hold (past 7am, nothing confirmed covering today).

**Focus order:**
```
1. Screen heading ("Claim Today") + back button
2. Claim summary card, announced as one unit ("Halcyon, standby claim, Thursday 12 March, today")
3. Points rows (available, this claim uses, balance after)
4. Zero-cost hero, announced as one unit ("Standby, today only. 0 points")
5. Estimated departure time, announced as a single labelled group of three controls (hour, minutes, AM/PM)
6. "Claim Today, Free" button
7. "Back to Calendar" button
```

**Keyboard/Switch Control map:** linear Tab/scan. The three time controls are individually focusable but share one group label, so the purpose of each is clear without repeating "estimated departure time" three times.

**Landmarks + heading outline:**
```
Heading level 1: "Claim Today"
```

**Screen-reader flow:**
```
On load: "Claim Today, heading level 1. Halcyon, standby claim. Thursday 12 March, today.
          Points available, 5. This claim uses, 0. Balance after, 5.
          Standby, today only. 0 points.
          Estimated departure time, 10, colon 00, A M.
          Claim Today, Free, button."
On claim: focus moves to the confirmation screen's heading, which announces "Claimed" as text, not as a colour change.
```

**Zero as a value, not an absence:** "0 points" must be announced, never skipped as an empty or null field. It is the whole point of the screen, and a partner who hears nothing cannot tell a free claim from a failed one.

**Contrast:** the zero-cost hero uses the success family on a tinted fill; per `07-color-system.md` that pairing passes AA at this text size. The tint never carries the meaning on its own, the words "0 points" do.

---

## 6. Focus Management & Skip Links

- **Skip links:** not applicable to native mobile navigation (no page-load skip pattern) — instead, the equivalent requirement is: **tab-bar switches move focus to the new screen's heading**, not leave focus stranded on the tapped tab icon. Applies to every tab switch (M03↔M04↔M05↔M07; the tab bar is 4 items, Notifications is reached from the Home top bar, not a tab).
- **Sheets/modals (M22 Booking Blocked, M29 Cancel confirmations, Christmas Window release):** focus traps inside the sheet while open; on dismiss, focus returns to the element that triggered it (e.g. the calendar cell selection that caused the block, or the Cancel Booking button on M09). The three cancellation dialogs differ in consequence, not in structure, so each one's body text must state the points outcome explicitly rather than relying on the partner remembering which booking type they are cancelling.
- **Route change (push navigation, e.g. M04→M08):** focus moves to the new screen's heading on arrival — never left on the now-invisible previous screen's last-focused element.
- **Infinite-scroll content (M06):** as new items load, focus is never moved automatically to them (that would be disorienting) — only the "Loading more..." live-region announcement fires, per M06's Screen-reader flow above.

## 7. Motion & Sensory
- All animations in `15-motion-design.md` have reduced-motion fallbacks (Section 4 of that doc) — confirmed compliant with 2.3.3 across every listed animation, including the signature gold-chevron and points-tick moments (see per-screen motion checks above for the two most safety-relevant instances).
- Color is never the sole signal anywhere in this app: every StatusBadge pairs color with a text label (`Confirmed`, `Claimed`, `Checked Out`, `Blocked`); every CalendarDateCell state pairs color with a second, non-colour signal AND a screen-reader label naming the state explicitly — never color alone. That second signal is specific per state: a solid thin border (available), a dashed border (unclaimed), a 3px rule across the top edge (named-holiday long weekend), no border at all (not bookable), a 45° hatch (blocked), a 2px border plus a TODAY label (standby), and an inverted white numeral on a solid fill (booked). This matters because the light states cannot all be separated by luminance alone without turning the month into a heat map, and it is why the holiday block is carried by its top rule rather than by its tint: measured, that tint sits at 1.24:1 against available, 1.01:1 against not-bookable and 1.10:1 against standby, and no light fill reaches 3:1 against the others. The holiday name is never left to the tint either: it is announced in the cell label and repeated in a visible key below the grid, where each row carries the same swatch its dates wear. The standby cell carries a visible `TODAY` marker as its whole cue, because SOW A.19 only ever opens the current date and the state is meaningless without that anchor. The corner glyph is deliberately **not** part of this set: since 2026-09-17 it is reserved for the two states a partner cannot select (booked, blocked), so it reads as one consistent "not yours" mark instead of a fourth decoration competing with the cues above.
- Nothing in this app auto-plays or is time-limited (no carousels, no auto-advancing content) — no additional user-control requirement applies.

## 8. Forms Accessibility
Fully specified per-field in `12-form-specs.md` Section 6 for all 4 forms (Pre-Departure Checklist, Post-Use Checklist, Standby Claim, and Edit Profile, which has no screen of its own and edits in place on M07) — this document doesn't duplicate that detail; see that file directly. Screen-level flow for the checklist form is covered in Section 5 (M15) above.

## 9. Contrast Summary

Pulled directly from `07-color-system.md`'s Step 7 matrix — no recomputation performed:

| Combination | Ratio | AA | AAA |
|---|---|---|---|
| Primary text on light background | 18.13:1 | PASS | PASS |
| Secondary text on light background | 6.81:1 | PASS | FAIL (acceptable — secondary text) |
| Action button text on navy fill | 13.53:1 | PASS | PASS |
| Gold accent text on navy | 5.25:1 | PASS | FAIL (acceptable — accent use only) |
| Navy text on gold fill (accent-text-on-fill token) | 7.03:1 | PASS | PASS |
| Status semantic text-on-bg (success/warning/error/info) | 7.02–7.82:1 | PASS | PASS |

**One item flagged for AAA hold (per Section 1):** the M08/M22 flow's text combinations already clear AAA (13.53:1 for the Confirm button, 7.82:1 for error text) — no additional design change needed, this is a confirmation that the existing tokens already meet the elevated bar set for this flow, not a new requirement.

**The one real risk already caught upstream:** `07-color-system.md` flagged white-text-on-gold-fill failing AA outright (2.8:1) and fixed it via the `accent-text-on-fill` token — re-confirmed here as the single most important contrast rule for engineering to actually enforce at implementation time (it's easy to default to white text on any colored button unless the token is used deliberately).

---

## Handoff
Feeds `alice-design-review` (SKILL 12) and `alice-design-qa` (SKILL 13), which check this spec was actually followed before shipping.
