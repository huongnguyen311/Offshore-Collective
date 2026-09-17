# Offshore Collective — Figma / Developer Handoff Specs (Partner Mobile App)

**Consumed inputs:** `11-component-specs.md`, `15-motion-design.md`, `17-screen-design.md` (HTML artifact reference)
**Note:** No live Figma file was generated for this pass (see `17-screen-design.md`) — every value below is specified directly so a developer can build without opening Figma, per this skill's own core rule.

---

# Component 1 — CalendarDateCell

## 1. Component Specs
- **Component name:** `CalendarDateCell/sm-md/available-holiday-unclaimed-standby-blocked-booked`
- **Figma location (target, if generated later):** Design System > Components > Booking Calendar > CalendarDateCell
- **Variant properties:** State (available/named-holiday-long-weekend/unclaimed/standby/not-bookable/blocked/booked) × Selected (true/false) × Disabled (true/false) × Size (sm/md). There is no `held` state and no `christmas-window` state: `held` was a leftover from the removed Rayglass 2400 towing-approval flow, and the Christmas Window is a one-time admin draw presented on M13, never a selectable cell on this grid.

## 2. Auto Layout Table

| Property | Value | Token |
|---|---|---|
| Direction | — (single cell, no internal auto-layout stack; centered content) | — |
| Alignment | Center / Center | — |
| Gap | n/a | — |
| Padding | top 4pt / right 4pt / bottom 4pt / left 4pt [sm]; 8pt all sides [md] | `spacing.1` [sm] / `spacing.2` [md] |
| Width | Fixed 41pt [sm] / Fixed 55pt [md] | per `10-responsive-layout.md` Calendar Grid spec |
| Height | Fixed 41pt [sm] / Fixed 55pt [md] (always square) | — |

## 3. Layer Structure
```
CalendarDateCell/
├── Root (frame, rounded-square, fill = state background)
│   ├── DayNumber (text, typography.dataInline, centered)
│   ├── StateGlyph (rectangle, 5×5pt, rounded, absolute bottom-right, visible ONLY for booked/blocked)
│   └── SelectionRing (rectangle, stroke only, absolute, inset 0, hidden by default)
```

## 4. Constraints
- Fixed size, does not stretch — the parent Calendar Grid frame controls column distribution (7 fixed columns, `spacing.1` gutter between cells).
- Min touch target: cell is 41pt at `sm`, below the 44×44pt [iOS HIG] / 48×48pt [Android Material] minimum — **add invisible hit-area padding of +1.5pt per side [iOS] / +3.5pt per side [Android]** around the visual cell so the tappable area meets the platform minimum without growing the visual 41pt square. At `md` (55pt), no padding needed — already exceeds both minimums.

## 5. Interactive States

| State | Visual Change | Behavioral Change |
|---|---|---|
| default (available) | `color.calendar.available-bg`, no border, no StateGlyph | Tappable |
| booked | `color.calendar.booked-bg` (solid navy fill), stroke `color.calendar.booked-border`, DayNumber inverts to white, StateGlyph visible | Tappable → navigates to M09 |
| standby | `color.calendar.standby-bg`, 1.5pt stroke `color.calendar.standby-border`, **no StateGlyph**, plus a `TODAY` label above the DayNumber | Tappable → navigates to M28 |
| unclaimed | `color.calendar.available-bg` (identical fill to available, by design), **dashed** stroke `color.calendar.unclaimed-border`, **no StateGlyph** | Tappable → selects to the end of the block, repriced per C.5 |
| not-bookable | `color.calendar.not-bookable-bg`, muted `DayNumber` color, **no stroke**, no StateGlyph | Past: not tappable. Beyond the 60-day window: tappable only to surface the explanation |
| blocked | `color.calendar.blocked-bg` + 45° hatch fill, `DayNumber` in `color.calendar.blocked-text`, StateGlyph visible (the shared "cannot select" mark it carries with booked) | **Not tappable**, `aria-disabled="true"` [equivalent: Android `clickable=false`, iOS `isUserInteractionEnabled=false`] |
| named-holiday-long-weekend | `color.calendar.holiday-block-bg`, 1pt stroke `color.calendar.holiday-block-border` on all four sides with the **top edge at 3pt**, no StateGlyph | Tappable → selects the whole Fri-to-Mon span at once and prices it as one Long Weekend, labelled with the holiday name (C.7) |
| selected | `SelectionRing` visible, `border-width.thick` stroke `color.border.focus` | Part of active range selection |
| pressed | Background overlay at `opacity.pressed-tint` | Immediate tap feedback |
| focus-visible | `SelectionRing`-equivalent ring in `color.border.focus`, 2pt offset | Keyboard/switch-control focus only |
| disabled (pre-qualification) | Entire cell at `opacity.disabled` | No interaction at all, regardless of underlying state |

## 6. Interaction Notes
- Tap on `available`/`booked`/`standby`/`unclaimed`/`named-holiday-long-weekend` → see per-state target above. A tap inside a named-holiday long weekend selects all four cells in the span, never the single cell tapped, so the prototype must wire the whole variant set for that span together.
- Animation on entering `selected`: reference `15-motion-design.md` Section on CalendarDateCell (not separately spec'd there beyond the component doc's own state table) — `duration.fast` (120ms), `easing.standard`, animating `border-color` only. Reduced motion: instant, no transition.
- Pressed-state feedback: reference Platform Flags below.

## 7. Token References Table

| Element | Property | Token |
|---|---|---|
| Root | fill (available) | `color.calendar.available-bg` |
| Root | fill (booked) | `color.calendar.booked-bg` |
| Root | fill (standby) | `color.calendar.standby-bg` |
| Root | fill (not-bookable) | `color.calendar.not-bookable-bg` |
| Root | fill (blocked) | `color.calendar.blocked-bg` |
| Root | fill (named-holiday-long-weekend) | `color.calendar.holiday-block-bg` |
| Root | border-radius | `border-radius.xs` (6pt) |
| Root | stroke (booked/standby) | respective `*-border` token, `border-width.base` |
| Root | stroke (named-holiday-long-weekend) | `color.calendar.holiday-block-border` at `border-width.base`, with the top edge at `component.booking-calendar-date.holiday-block-top-width` (3pt). In Figma this is an individual-stroke override on the top side, not a uniform stroke |
| Root | stroke (unclaimed) | `color.calendar.unclaimed-border`, `border-width.base`, **dashed** |
| DayNumber | typography | `typography.dataInline` (scaled to ~13pt at `sm`, per `09-typography-system.md`'s render-time note) |
| DayNumber | color (booked) | `color.calendar.booked-text` |
| DayNumber | color (booked) | `color.calendar.booked-text` (white on the solid navy fill) |
| DayNumber | color (standby) | `color.calendar.standby-text` |
| DayNumber | color (not-bookable) | `color.calendar.not-bookable-text` |
| DayNumber | color (blocked) | `color.calendar.blocked-text` |
| StateGlyph | fill | booked: white with a `color.calendar.booked-border` ring. blocked: `color.calendar.blocked-text`. No other state shows it |
| SelectionRing | stroke | `color.border.focus`, `border-width.thick` |
| Root (pressed) | overlay opacity | `opacity.pressed-tint` |
| Root (disabled) | opacity | `opacity.disabled` |

## Platform Flags
- Pressed feedback: opacity-tint overlay, no scale change [iOS] / Material ripple contained to the rounded-square shape, clipped to `border-radius.xs` [Android — do not let the ripple bleed past the rounded corners].
- Min touch target: 44×44pt [iOS HIG] / 48×48pt [Android Material] — see the invisible hit-area note in Section 4.
- Accessibility label format (both platforms): `"[Month] [Day], [state]"` e.g. "March 14, available" / "March 6, your booking" — per `11-component-specs.md` Section 5.

---

# Component 2 — StatusBadge

## 1. Component Specs
- **Component name:** `StatusBadge/checked-out-qualification-pending-booking-blocked-confirmed`
- **Figma location (target):** Design System > Components > Status > StatusBadge
- **Variant properties:** Status (checked-out/qualification-pending/booking-blocked/confirmed) × hasLeadingIcon (true/false — true only valid with booking-blocked)

## 2. Auto Layout Table

| Property | Value | Token |
|---|---|---|
| Direction | Horizontal | — |
| Alignment | Center / Center | — |
| Gap | 8pt | `spacing.2` |
| Padding | top 4pt / right 12pt / bottom 4pt / left 12pt | `spacing.1` (vertical) / `spacing.3` (horizontal) |
| Width | Hug content | — |
| Height | Hug content | — |

## 3. Layer Structure
```
StatusBadge/
├── Root (frame, horizontal auto-layout, pill radius, fill = status background)
│   ├── LeadingIcon (16×16pt, SVG, hidden unless Status=booking-blocked AND hasLeadingIcon=true)
│   └── Label (text, typography.labelMedium, uppercase)
```

## 4. Constraints
- Hugs content in both axes — never stretches to fill a container.
- No min/max width specified — content-driven; longest label ("Qualification Pending" — note: rendered label is the shorter "Pending" per `14-ux-writing.md`, so this isn't a practical concern) still hugs naturally.
- Not a touch target — this component is non-interactive (see `11-component-specs.md` Section 5), so the 44/48pt minimum does not apply.

## 5. Interactive States

| State | Visual Change | Behavioral Change |
|---|---|---|
| default | Per Status variant colors (see Token References) | Purely informational, `role="status"` [Android: `AccessibilityLiveRegion.POLITE`] [iOS: `UIAccessibilityTraits` none, but wrap in a view posting `UIAccessibility.post(notification: .announcement, ...)` on change] |
| status-transition | Background/text color cross-fades to new status's tokens | Triggers the live-region announcement — reference `11-component-specs.md` Section 6 for the exact `duration.normal`/`easing.standard` cross-fade spec |
| disabled | **Not applicable** — this is a pure display element with no interactive/disabled duality, stated explicitly per the component spec's own note rather than omitted | — |
| hover / pressed / focus-visible | **Not applicable** — non-interactive, never receives tap/click/keyboard focus | — |

## 6. Interaction Notes
- Non-interactive — no tap target, no navigation on tap.
- The only "interaction" is a data-driven status change while the badge is on-screen (e.g. a booking flipping from Confirmed to Checked Out on checklist submission) — see State 2 above and `11-component-specs.md` Section 6 for the full animation spec.

## 7. Token References Table

| Element | Property | Token |
|---|---|---|
| Root | fill (checked-out) | `color.app-status.checked-out-bg` |
| Root | fill (qualification-pending) | `color.app-status.qualification-pending-bg` |
| Root | fill (booking-blocked) | `color.app-status.booking-blocked-bg` |
| Root | fill (confirmed) | `color.status.success-bg` |
| Root | border-radius | `border-radius.full` |
| Label | typography | `typography.labelMedium` |
| Label | color (checked-out) | `color.app-status.checked-out-text` |
| Label | color (qualification-pending) | `color.app-status.qualification-pending-text` |
| Label | color (booking-blocked) | `color.app-status.booking-blocked-text` |
| Label | color (confirmed) | `color.status.success-text` |
| LeadingIcon | color | matches Label color for the same variant |

## Platform Flags
- No platform-specific visual differences — this component renders identically on iOS and Android (it's a simple filled pill with no native-chrome dependency).
- Live-region/announcement mechanism differs by platform per Section 5 above — flag to engineering as the one implementation-level (not visual) platform difference.

---

# Component 3 — PointsBalanceDisplay

## 1. Component Specs
- **Component name:** `PointsBalanceDisplay/hero-inline`
- **Figma location (target):** Design System > Components > Data Display > PointsBalanceDisplay
- **Variant properties:** Variant (hero/inline) × Trend (none/decreasing/increasing) × Blocked (true/false — valid on `inline` only)

## 2. Auto Layout Table — Variant: hero

| Property | Value | Token |
|---|---|---|
| Direction | Vertical | — |
| Alignment | Center / Center | — |
| Gap | 12pt (EyebrowLabel→ValueNumber), 8pt (ValueNumber→SupportingLine) | `spacing.3` / `spacing.2` |
| Padding | 0 (padding lives on the parent Home hero block, per `10-responsive-layout.md`) | — |
| Width | Fill container | — |
| Height | Hug content | — |

## 2b. Auto Layout Table — Variant: inline

| Property | Value | Token |
|---|---|---|
| Direction | Horizontal (as part of a labeled row) | — |
| Alignment | Space-between | — |
| Gap | 8pt | `spacing.2` |
| Width | Fill container | — |
| Height | Hug content | — |

## 3. Layer Structure
```
PointsBalanceDisplay/
├── Root (frame, vertical [hero] or horizontal [inline] auto-layout)
│   ├── EyebrowLabel (text, typography.eyebrowLabel, hidden in `inline` variant)
│   ├── ValueNumber (text, typography.dataHero [hero] or typography.dataInline [inline], tabular-nums, animatable text content)
│   └── SupportingLine (text, typography.bodySmall or typography.dataInline, hidden if no supporting context)
```

## 4. Constraints
- `hero`: fills the width of its parent (the Home hero block), content center-aligned; height hugs.
- `inline`: fills the width of its parent row; `ValueNumber` right-aligns within that row in practice (tabular figures read best right-aligned in a list of rows), though the Auto Layout itself is space-between across the row's two ends.
- Not a touch target — display-only, same as StatusBadge; 44/48pt minimum does not apply.

## 5. Interactive States

| State | Visual Change | Behavioral Change |
|---|---|---|
| static | Value renders at current number, no animation | — |
| value-increasing | `ValueNumber` ticks upward — see `15-motion-design.md` Section 3 for the full count-up spec (`duration.slow`/360ms, `easing.standard`, driven-value text interpolation, NOT a transform/opacity animation) | Triggers `aria-live="polite"` [Android: live region] announcement on completion |
| value-decreasing | Same mechanism, counting down | Same announcement behavior |
| blocked (`inline` only) | `ValueNumber` recolors to `color.app-status.booking-blocked-text` | No animation — renders final value immediately, no tick, per `11-component-specs.md`'s explicit note |
| disabled | **Not applicable** — display-only, same reasoning as StatusBadge | — |

## 6. Interaction Notes
- No tap/click behavior anywhere on this component.
- The count-up/count-down tick is the Signature Moment #5 animation from `06-art-direction.md` — full spec lives in `15-motion-design.md` Section 3; this component doc only wires the color/typography tokens, not the animation math itself, to avoid duplicating the spec in two places.
- On the `hero` variant specifically, this component sits directly above the `PointsBalanceDisplay`'s sibling next-booking line within the Home hero block (see `10-responsive-layout.md` "Home Hero Block" Auto Layout spec) — not part of this component's own layer tree, but noted here so a developer building this component in isolation understands its expected context.

## 7. Token References Table

| Element | Property | Token |
|---|---|---|
| EyebrowLabel | typography | `typography.eyebrowLabel` |
| EyebrowLabel | color | `color.text.secondary` |
| ValueNumber (hero) | typography | `typography.dataHero` |
| ValueNumber (inline) | typography | `typography.dataInline` |
| ValueNumber (hero) | color | `color.text.brand` |
| ValueNumber (inline, default) | color | `color.text.primary` |
| ValueNumber (inline, blocked) | color | `color.app-status.booking-blocked-text` |
| SupportingLine | typography | `typography.bodySmall` |
| SupportingLine | color | `color.text.secondary` |

## Platform Flags
- Text-interpolation animation implementation differs by platform: `Animated.Value` + a rendered text listener [React Native/both], or `NSTimer`/`CADisplayLink`-driven label update [native iOS], or `ValueAnimator` on a `TextView` [native Android] — flag to engineering as an implementation choice, not a visual difference; the visual result (a counting tabular number) must be identical across platforms.
- No other platform-specific visual differences.
