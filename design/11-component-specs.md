# Offshore Collective — Component Specs (Partner Mobile App)

**Consumed inputs:** `05-user-flows.md` (chose the 3 components appearing across the most flows), `tokens/colors.json` + `foundations.json` + `typography.json`, `06-art-direction.md` (Signature Moments #1 and #3)

---

# Component 1 — CalendarDateCell

## 1. Overview
- **Purpose:** Represents one bookable date on the Booking Calendar (M04), showing its state and — for the partner's own bookings — the day number, in a single tappable unit.
- **When to use:** Only within the Booking Calendar grid (M04). Not a general-purpose calendar component for other contexts.
- **When NOT to use:** Do not reuse for the Christmas Window Display (M13) full read-only view — that screen shows a date *range* summary, not an interactive grid of individual cells.
- **Related components:** `StatusBadge` (blocked and standby-available share their colour semantics), `PointsBalanceDisplay` (M08 shows cost after a cell is selected).

## 2. Anatomy
| Layer Name | Element Type | Required? |
|---|---|---|
| Root | container (rounded-square) | Required |
| DayNumber | text (dataInline) | Required |
| StateGlyph | icon (small rounded-square accent, Signature Moment #3) | Optional, present for **booked and blocked only**. Narrowed from four states on 2026-09-17 so the glyph carries a meaning rather than decorating the grid: it marks a date the partner **cannot select**. Absent for available, standby-available, unclaimed, not-bookable and named-holiday-long-weekend |
| SelectionRing | border overlay | Optional — visible only when the cell is part of an in-progress selection |

## 3. Variant Matrix
| Property | Values |
|---|---|
| State | available / booked / standby-available / unclaimed / blocked / named-holiday-long-weekend |
| Selected | true / false |
| Disabled | true / false |
| Size | sm (xs breakpoint, ~41px) / md (sm breakpoint, ~55px) |

**Impossible combinations:** `blocked` + `Selected: true` is not supported — a blocked cell cannot enter a selection (it's not tappable at all; see States). `named-holiday-long-weekend` + `Selected: true` IS supported and is the normal path: selecting it applies SelectionRing to all four cells in the span at once, never to the single day tapped. There is no christmas-window state: the window is a one-time admin draw, not partner-selectable, and is presented on M13 instead of on this grid.

## 4. Specs

### Size: sm (320pt breakpoint)
| Token Category | Spec |
|---|---|
| Width / Height | Fixed, `41px` (computed per `10-responsive-layout.md` Calendar Grid spec) |
| Padding | `spacing.1` (4px) internal |
| Border radius | `border-radius.xs` (6px) |
| Typography | `typography.dataInline`, scaled to fit — see note below |
| Background (available) | `color.calendar.available-bg` (white, the lightest step) |
| Background (booked) | `color.calendar.booked-bg` (solid navy, the darkest step; numeral inverts to white) |
| Background (standby) | `color.calendar.standby-bg` |
| Background (unclaimed) | `color.calendar.available-bg` — same fill as available on purpose; the dashed border carries the state |
| Background (not-bookable) | `color.calendar.not-bookable-bg` (past dates and dates beyond the 60-day window share one treatment) |
| Background (blocked) | `color.calendar.blocked-bg` + a 45° hatch overlay |
| Background (named-holiday-long-weekend) | `color.calendar.holiday-block-bg` |
| Border width | `border-width.base` (booked/standby; `border-style: dashed` for unclaimed; `border-width.base` on three sides plus `component.booking-calendar-date.holiday-block-top-width` (3px) on the top edge for named-holiday-long-weekend; available/blocked/past have no border, per `component.booking-calendar-date` tokens) |
| Shadow | `shadow.none` |

### Size: md (375–430pt breakpoint)
Same token references as sm; only the fixed width/height differs (`~55px`, per layout spec) and internal padding steps up to `spacing.2` (8px).

**Note on DayNumber at sm:** `typography.dataInline` is specified at 15px in `09-typography-system.md`; at the 41px sm cell size this needs a component-level down-scale to ~13px to fit comfortably with padding — flag this to SKILL SC as a render-time adjustment, keeping the same font family/weight/tabular treatment, only the size token differs at this one breakpoint.

## 5. Accessibility
| Attribute | Value |
|---|---|
| ARIA role | `button` (each cell is an independent tappable element when available, booked, standby or unclaimed; `text`/non-interactive when blocked or past) |
| ARIA label | e.g. "March 14, available" / "March 15, booked" / "March 12, today, standby available, free" / "March 27, unclaimed weekend, lower rate" / "March 17, out of service, not bookable" / "March 3, not bookable" / "March 20, King's Birthday Weekend, long weekend block, Friday 20 to Monday 23, available". The holiday label names the holiday and the whole span on **every** cell in it, because the block is what the partner is selecting; the single day tapped is never the unit. A named holiday that formed no long weekend that year announces as an ordinary day with its name only, e.g. "March 18, Waitangi Day, available" |
| ARIA selected | `true` when part of the partner's in-progress date-range selection |
| Minimum touch target | Cell is 41–55px, already ≥44×44pt at the `md` breakpoint; at `sm` (41px) it falls just under 44pt — add invisible padding to reach the 44×44pt minimum without growing the visual cell |
| Contrast ratio | All state background/text pairs inherit from the WCAG matrix in `07-color-system.md` (all pass AA) |
| Focus indicator | `color.border.focus` (navy-500) ring, 2px offset, on keyboard/switch-control focus |
| Keyboard navigation | Arrow keys move focus between adjacent dates; Enter/Space selects; Escape cancels an in-progress range selection |
| Screen reader announcement | On focus: full ARIA label above. On selection: "Selected, [date]" appended |

## 6. States
| State | Visual Change | Behavioral Change | Animation? |
|---|---|---|---|
| available (default) | `calendar.available-bg`, no border | Tappable — starts or extends a date-range selection | — |
| booked (own) | `calendar.booked-bg` + border + StateGlyph | Tappable — navigates to Upcoming Booking Detail (M09) | — |
| standby-available | `calendar.standby-bg` + 1.5px border + a `TODAY` label, **no StateGlyph** | Tappable — opens Standby Claim (M28). Only ever appears on the current date, after 7am, when nothing confirmed covers it (A.19), and only when that date is a Monday to Thursday (2026-09-19). A Friday, Saturday or Sunday never renders in this state, whatever its booking status: those days reach the partner through `unclaimed` instead. The glyph came off on 2026-09-17: fill, border, bold numeral, TODAY label and glyph was five green signals on one 41px cell, the same pile-up that had already cost this cell its inset ring. The TODAY label carries the state on its own and is the cue shown in the legend | — |
| unclaimed | `calendar.available-bg` + **dashed** border, **no StateGlyph** | Tappable — selects from this day through to the end of the block and reprices from the C.5 table, rather than selecting the single day tapped. On the day itself after 7am that day is free and only the remainder of the block is charged (same-day rule, 2026-09-19), so the cell can price at zero: the review screen must show the day as free rather than omitting it. The dashed border is the cue and it is the one shown in the legend; the glyph was a second cue on a date the partner can still take, so it came off on 2026-09-17 | — |
| not-bookable | `calendar.not-bookable-bg`, muted numeral, **no border** | **Not tappable** for a past date; a date beyond the 60-day window stays tappable only to surface the explanation. One treatment for both, because the partner's options are identical either way | — |
| blocked (out of service) | `calendar.blocked-bg` + a 45° hatch + StateGlyph | **Not tappable** — `aria-disabled="true"`, no selection possible. The hatch is what makes this read as unavailable in greyscale; the glyph is the shared "cannot select" mark it carries with booked. Corrected 2026-09-17: this row previously said no glyph, which contradicted the rendered screens | — |
| named-holiday-long-weekend | `calendar.holiday-block-bg` + 1px `calendar.holiday-block-border` + a **3px top rule** on every cell in the span | Tappable — a tap anywhere inside the span selects all four days (Fri to Mon) and prices them as one Long Weekend, labelled with the holiday name. Applied only over `available` cells, so a holiday day already taken still renders and behaves as `booked` | — |
| selected (available cell mid-range) | `SelectionRing` visible, `border-width.thick` in `color.border.focus` | Part of an active date-range drag/tap selection | ⚡ `transition: border-color duration.fast ease.standard` on entering selection |
| disabled (no boat/no access) | `opacity.disabled` applied to entire cell | Cell present but fully non-interactive (e.g. before qualification approval) | — |
| pressed | Background shifts to `opacity.pressed-tint` overlay | Immediate tap feedback | ⚡ `transition: opacity duration.fast ease.standard` |

**State count (SOW A.6):** Six states. SOW A.6's AC names four (available / booked / blocked / standby-available); unclaimed is kept as a fifth because it changes the price, which a partner can act on, and the C.7 named-holiday long weekend is the sixth. Past and beyond-the-60-day-window merged into one "not bookable" treatment. The measured reason for the earlier cut from seven: every one of the 36 fill pairs sat below 3:1, four states shared one fill, and two failed AA on their own numeral. States are stepped by lightness, and each light state carries a non-colour cue (solid border, dashed border, no border, hatch, TODAY label, 3px top rule).

**Named-holiday long weekend (C.7), added 2026-09-17 per Matt's M04 comment:** one treatment for every named holiday, never a colour per holiday, so the legend stays one line however many land in a month. The fill alone cannot carry it, and that is measured rather than assumed: `calendar.holiday-block-bg` sits at 1.24:1 against available, 1.01:1 against not-bookable and 1.10:1 against standby. No light tint reaches 3:1 against the other light fills, which is why the first attempt at a holiday tint was cut. So the **shape** is the primary cue and the fill is secondary: a 3px rule across the top of all four cells in the span, which survives greyscale and colour-vision deficiency and carries the right meaning, that Fri to Mon is one indivisible unit under C.4. This is a display layer over holiday dates the system already tracks for Named Holiday Fairness; it adds no rule logic.

**Exception, per confirmed Open Questions for Client #17:** a named holiday that forms no long weekend in a given year (Waitangi Day or ANZAC Day falling midweek) takes **no cell treatment at all**. It is an ordinary single bookable day and is named only in the key below the grid, because styling it as a block would assert a booking rule that is not true that year.

**Holiday key (below the grid):** a key rather than a caption. Each row carries the same swatch its dates wear on the grid, followed by the holiday name, the date range, and whether it is a long weekend block or a single day. The tint above therefore resolves to a name instead of being one more colour to decode. Two swatch variants only, matching the two things a C.7 date can be.

**Signature treatment carried through:** the rounded-square `border-radius.xs` shape on every cell state directly echoes the brand icon's interlocking-squares geometry (Signature Moment #3, `06-art-direction.md`) — this is the one non-negotiable visual signature for this component; no state should ever render as a plain colored circle or square-cornered rectangle.

---

# Component 2 — StatusBadge

## 1. Overview
- **Purpose:** Communicates a booking or account status at a glance (Confirmed, Claimed, Checked Out, Qualification Pending, Booking Blocked) using a consistent pill shape and semantic color, independent of where it appears.
- **When to use:** Anywhere a partner needs to see the current status of something without opening it — Booking Detail (M09), Home (M03) upcoming-booking summary, Qualification Pending Gate (M20).
- **When NOT to use:** Not for the one-time confirmation moment (M08→M09) — that uses the dedicated gold-chevron confirmation motion (see `component.confirmation-motion` token group), not a badge. Not for the boat-ready status on the Home boat card either: C.31 makes that a persistent status with its own icon-and-label treatment, deliberately distinct from a booking's status badge so the two are not read as the same kind of thing. That status carries three states of its own, not two (ready / not ready yet / absent, added 2026-09-19), and the "not ready yet" state uses the warning tint with a clock icon. It stays outside this component: a StatusBadge describes a booking a partner made, the C.31 status describes the boat on the day, and collapsing them would make a contractor's progress look like something the partner did.
- **Related components:** `CalendarDateCell` (the standby state shares the same success color semantic as a Claimed badge, which is intentional: both mean "this day is yours, at no point cost").

## 2. Anatomy
| Layer Name | Element Type | Required? |
|---|---|---|
| Root | container (pill) | Required |
| Label | text (labelMedium) | Required |
| LeadingIcon | icon | Optional — used for Booking Blocked (warning triangle) only |

## 3. Variant Matrix
| Property | Values |
|---|---|
| Status | checked-out / qualification-pending / booking-blocked / confirmed |
| hasLeadingIcon | true / false |

**Note:** `confirmed` is included here as a variant (using `color.status.success-*`) because `05-user-flows.md`'s FLOW-BOOK-01 ends in a persistent "confirmed" label on M09. Per `07-color-system.md`'s explicit rule it must stay the standard green success semantic, never the gold accent.

**`Claimed` is not a fourth variant.** A standby claim (SOW A.19) uses the `confirmed` variant's tokens with a different label, because it means the same thing to the partner: the day is theirs. Only the label changes, so scanning Home stays consistent while still distinguishing a free same-day claim from a booking that cost points.

**Impossible combinations:** `hasLeadingIcon: true` is only defined for `booking-blocked` — the other three statuses render as text-only pills; adding icons to them is unsupported in this spec.

## 4. Specs
Single size only (no sm/md/lg tiers — this component doesn't scale with breakpoint).

| Token Category | Spec |
|---|---|
| Width | Hug contents |
| Height | Hug contents (Label line-height + vertical padding) |
| Padding (horizontal) | `component.status-badge.padding-x` (= `spacing.3`, 12px) |
| Padding (vertical) | `component.status-badge.padding-y` (= `spacing.1`, 4px) |
| Gap (icon to label) | `spacing.2` (8px) |
| Border radius | `component.status-badge.radius` (= `border-radius.full`) |
| Typography | `typography.labelMedium` (Mono, uppercase, tracked) |
| Background (checked-out) | `color.app-status.checked-out-bg` |
| Text (checked-out) | `color.app-status.checked-out-text` |
| Background (qualification-pending) | `color.app-status.qualification-pending-bg` |
| Text (qualification-pending) | `color.app-status.qualification-pending-text` |
| Background (booking-blocked) | `color.app-status.booking-blocked-bg` |
| Text (booking-blocked) | `color.app-status.booking-blocked-text` |
| Background (confirmed) | `color.status.success-bg` |
| Text (confirmed) | `color.status.success-text` |
| Shadow | `shadow.none` |

## 5. Accessibility
| Attribute | Value |
|---|---|
| ARIA role | `status` (announces changes via live region when the badge's status updates while the screen is open — e.g. a booking flipping from Confirmed to Checked Out once the pre-departure checklist is submitted) |
| ARIA label | The Label text itself is sufficient (e.g. "Confirmed", "Claimed") — no separate label needed |
| Minimum touch target | N/A — this is a non-interactive display element, never itself tappable |
| Contrast ratio | All four status pairs inherit from the WCAG matrix in `07-color-system.md` (checked-out/booking-blocked pass AA+AAA; qualification-pending and confirmed/claimed inherit info/success pairs, both AA+AAA) |
| Focus indicator | N/A — not focusable |
| Keyboard navigation | N/A |
| Screen reader announcement | On the status changing while the screen is visible (e.g. FLOW-CHECKLIST-01, a booking moving to Checked Out on submission): announce the new status text via the `status` live region |

## 6. States
| State | Visual Change | Behavioral Change | Animation? |
|---|---|---|---|
| default | Per Status variant colors above | Purely informational | — |
| status-transition (e.g. confirmed → checked-out) | Background/text color cross-fades to the new status's tokens | Triggers the ARIA live-region announcement | ⚡ `transition: background-color, color duration.normal ease.standard` |
| disabled | Not applicable — a badge has no disabled state since it's non-interactive | — | — |

**Note on the required-disabled rule:** this component is a pure display element (never interactive), so no meaningful "disabled" visual exists — this is stated explicitly rather than silently omitted, per the skill's own instruction to flag rather than skip.

---

# Component 3 — PointsBalanceDisplay

## 1. Overview
- **Purpose:** Shows a partner's points balance (or a points-related number: this-booking-cost, resulting-balance, refund/forfeit amount) with the app's signature tabular-numeral treatment, and animates when the underlying value changes.
- **When to use:** Home (M03) hero balance, Booking Review (M08) three-number breakdown, Cancel Booking (M17) refund/forfeit amount.
- **When NOT to use:** Not for non-points numerals like litres readings or dates — those use `typography.dataInline` directly without this component's ticking-animation behavior, since only points-balance changes are the "trust the fairness system" moment worth animating (per `06-art-direction.md` Signature Moment #5).
- **Related components:** `StatusBadge` (a Booking Blocked badge often appears alongside this component when the shown balance would go negative).

## 2. Anatomy
| Layer Name | Element Type | Required? |
|---|---|---|
| Root | container (vertical stack) | Required |
| EyebrowLabel | text (eyebrowLabel) | Optional — present in `hero` variant only |
| ValueNumber | text (dataHero or dataInline, ticking) | Required |
| SupportingLine | text (bodySmall or dataInline) | Optional — e.g. "next boat trip in 12 days" under the Home hero, or "= 3 points" under an inline breakdown row |

## 3. Variant Matrix
| Property | Values |
|---|---|
| Variant | hero (Home M03) / inline (Booking Review M08, Cancel Booking M17) |
| Trend | none / decreasing (booking cost) / increasing (refund) |
| Blocked | true / false — `true` only valid for `inline` variant, when the resulting balance would go negative |

**Impossible combinations:** `Variant: hero` + `Blocked: true` — the Home hero never shows a blocked state; blocking only ever happens mid-booking-flow on the `inline` variant (M08/M22).

## 4. Specs

### Variant: hero
| Token Category | Spec |
|---|---|
| Width | Fill container, center-aligned |
| EyebrowLabel typography | `typography.eyebrowLabel` |
| ValueNumber typography | `typography.dataHero` |
| SupportingLine typography | `typography.bodySmall` |
| Gap (EyebrowLabel → ValueNumber) | `spacing.3` |
| Gap (ValueNumber → SupportingLine) | `spacing.2` |
| Text color (ValueNumber) | `color.text.brand` |
| Text color (EyebrowLabel, SupportingLine) | `color.text.secondary` |
| Background | None — sits directly on `color.bg.default` |

### Variant: inline
| Token Category | Spec |
|---|---|
| Width | Fill container, left-aligned within a labeled row (e.g. "Points available") |
| ValueNumber typography | `typography.dataInline` |
| Text color (default) | `color.text.primary` |
| Text color (Blocked: true) | `color.app-status.booking-blocked-text` |
| Gap (label → ValueNumber, same row) | `spacing.2` |

## 5. Accessibility
| Attribute | Value |
|---|---|
| ARIA role | `text` container with `aria-live="polite"` on ValueNumber specifically, so a value change (e.g. after confirming a booking) is announced without interrupting other screen-reader activity |
| ARIA label | e.g. "Points balance, 42" / "Points this booking will use, 5" / "Balance after booking, 37" |
| Minimum touch target | N/A — display-only, never itself tappable |
| Contrast ratio | `color.text.brand` on `bg.default` and `color.app-status.booking-blocked-text` on its paired background both inherit passing AA/AAA ratios from `07-color-system.md` |
| Focus indicator | N/A |
| Keyboard navigation | N/A |
| Screen reader announcement | On value change: announce the new number and its label together (e.g. "Points balance updated: 37") rather than just the raw number, so context isn't lost |

## 6. States
| State | Visual Change | Behavioral Change | Animation? |
|---|---|---|---|
| static (initial render) | Value renders at its current number, no animation | — | — |
| value-increasing (e.g. refund on cancellation) | Number ticks upward from old to new value | Triggers the `aria-live` announcement once the tick completes | ⚡ Count-up tween, `duration.slow` (360ms), `easing.standard`, triggered on mount when a previous value exists in the same session |
| value-decreasing (e.g. booking cost deducted at M08 confirm) | Number ticks downward from old to new value | Same announcement behavior | ⚡ Count-down tween, `duration.slow` (360ms), `easing.standard` |
| blocked (inline variant only) | ValueNumber recolors to `app-status.booking-blocked-text`; typically paired with a `StatusBadge` (booking-blocked) alongside it | Signals the resulting balance would go negative — read-only, still just displays the (negative) number | — |
| disabled | Not applicable — same reasoning as StatusBadge: this is a display-only component with no interactive/disabled duality | — | — |

**Signature treatment carried through:** the count-up/count-down tween on ValueNumber is the direct implementation of Signature Moment #5 ("Micro-delight / data moment... the balance number ticks from old to new value over ~400ms") from `06-art-direction.md` — note the art direction specified ~400ms and this spec uses the token `duration.slow` (360ms) as the closest existing token rather than inventing a new one-off duration; flag this minor rounding to SKILL 10 (Motion Design) for confirmation rather than silently deviating.

---

## Handoff
Feeds `11-figma-specs.md` (SKILL 11) for full Figma Auto Layout/layer-tree documentation, and `12-screen-design` (SKILL SC) where these three components are actually placed into real screens.
