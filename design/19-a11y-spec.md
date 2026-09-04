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
1. Eyebrow Label + Data Hero, announced together as one unit ("Points balance, 37")
2. Next-booking supporting line ("Halcyon, 6 to 8 March, 5 days away")
3. Quick-link: Book
4. Quick-link: Checklist
5. Quick-link: Invoices
6. Upcoming booking card (boat name, dates, StatusBadge)
7. Tab bar: Home (current) → Book → Rules → Alerts → Profile
```

**Keyboard/Switch Control map:** external keyboard Tab moves through items 1–7 in the same order; Enter/Space activates a focused quick-link or tab; Switch Control scanning follows the identical order (no custom scan groups needed — the screen is a single linear flow).

**Landmarks + heading outline:**
```
Heading level 1: "Home" (screen title, announced but not visually duplicated — the Data Hero is content, not the heading)
  Heading level 2: "Upcoming — Halcyon" (the card title)
```

**Screen-reader flow:**
```
On load: "Home, heading level 1. Points balance, 37. Next: Halcyon, 6 to 8 March, 5 days away.
          Book, button. Checklist, button. Invoices, button.
          Upcoming, Halcyon, heading level 2. Fri 6 Mar to Sun 8 Mar, Confirmed."
On tab change (to Book): focus moves to the M04 heading, per Section 6.
```

**Contrast:** Data Hero (`color.text.brand` on `color.bg.default`) and body text all pass AA per `07-color-system.md`; no new combinations introduced on this screen.

---

### M04 — Booking Calendar

**Focus order:**
```
1. Screen heading ("Booking Calendar")
2. Month/boat label ("Halcyon, March")
3. Calendar grid — swipe order is row-by-row, left to right, each cell announced with its full state label
4. Selected-dates summary row (when a selection is active)
5. Points-required row (when a selection is active)
6. "Review Booking" button (when a selection is active; hidden/unreachable otherwise)
```

**Keyboard/Switch Control map:** Arrow keys move focus between adjacent calendar cells (per `11-component-specs.md` Section 5 — this is the one screen where arrow-key navigation is specified at the component level, not just linear Tab order); Enter/Space selects a cell; Escape cancels an in-progress range selection. Switch Control: group the calendar grid as a single scan group with row/column scanning enabled (Switch Control's built-in grid-scanning mode), rather than forcing a full linear scan through 30+ individual cells.

**Landmarks + heading outline:**
```
Heading level 1: "Booking Calendar"
  Heading level 2: "Halcyon, March" (implicit via the section label — mark as heading level 2 if the component renders it as a visually distinct section label, per `10-responsive-layout.md`)
```

**Screen-reader flow:**
```
On load: "Booking Calendar, heading level 1. Halcyon, March.
          March 1, available, button. March 2, available, button. ... 
          March 4, out of service, not bookable. ...
          March 6, your booking, button. ..."
On selecting a range: "Selected, March 14 to March 15. Points required, 1."
On tapping a blocked cell attempt: no announcement change (blocked cells are non-interactive, so no activation event fires) — the cell's state is already known from its label on focus.
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

**Keyboard/Switch Control map:** linear Tab/scan through rows; Enter/Space on a row opens the relevant notification detail/context (M14 or its target, per the IA).

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

**Focus order:**
```
1. Screen heading ("Profile")
2. Avatar + name + boat (announced as one unit, not three separate stops)
3. Qualification status row
4. Secondary operator row
5. Menu: Payments & Invoices
6. Menu: Christmas Window
7. Menu: Support
8. Menu: Terms & Privacy
9. Tab bar
```

**Landmarks + heading outline:**
```
Heading level 1: "Profile"
```

**Screen-reader flow:**
```
On load: "Profile, heading level 1. David Kearney, Halcyon, Rayglass 3000.
          Qualification, Approved. Secondary operator, Not set.
          Payments and Invoices, button. Christmas Window, button. Support, button. Terms and Privacy, button."
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
9. (conditional, 2400 only) Towing disclaimer checkbox
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
On load: "Almost there, heading level 1. Matt is confirming your Powerboat Training NZ certification.
          Booking opens automatically the moment he does — no action needed from you.
          Read the Boat Rules, button."
```

**Note:** this screen has no icon-only signal for its "informational, not alarming" status (the `info` color is decorative here) — the heading + body copy alone carry the full meaning, so color is correctly never the sole signal per Rule 6, even without an explicit icon label, since sighted users get the same "info-blue, calm" read from the copy tone as from the color.

---

## 6. Focus Management & Skip Links

- **Skip links:** not applicable to native mobile navigation (no page-load skip pattern) — instead, the equivalent requirement is: **tab-bar switches move focus to the new screen's heading**, not leave focus stranded on the tapped tab icon. Applies to every tab switch (M03↔M04↔M05↔M06↔M07).
- **Sheets/modals (M22 Booking Blocked, M17 Cancel confirmation):** focus traps inside the sheet while open; on dismiss, focus returns to the element that triggered it (e.g. the calendar cell selection that caused the block, or the Cancel Booking button on M09).
- **Route change (push navigation, e.g. M04→M08):** focus moves to the new screen's heading on arrival — never left on the now-invisible previous screen's last-focused element.
- **Infinite-scroll content (M06, M11):** as new items load, focus is never moved automatically to them (that would be disorienting) — only the "Loading more..." live-region announcement fires, per M06's Screen-reader flow above.

## 7. Motion & Sensory
- All animations in `15-motion-design.md` have reduced-motion fallbacks (Section 4 of that doc) — confirmed compliant with 2.3.3 across every listed animation, including the signature gold-chevron and points-tick moments (see per-screen motion checks above for the two most safety-relevant instances).
- Color is never the sole signal anywhere in this app: every StatusBadge pairs color with a text label (`Pending Approval`, `Blocked`, `Confirmed`); every CalendarDateCell state pairs color with the rounded-square StateGlyph (or its absence) AND a screen-reader label naming the state explicitly — never color alone.
- Nothing in this app auto-plays or is time-limited (no carousels, no auto-advancing content) — no additional user-control requirement applies.

## 8. Forms Accessibility
Fully specified per-field in `12-form-specs.md` Section 6 for all 5 forms (Pre-Departure Checklist, Post-Use Checklist, Towing Destination Entry, Edit Profile, Secondary Operator) — this document doesn't duplicate that detail; see that file directly. Screen-level flow for the checklist form is covered in Section 5 (M15) above.

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
