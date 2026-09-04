# Offshore Collective — Component Specs (Partner Mobile App)

**Consumed inputs:** `05-user-flows.md` (chose the 3 components appearing across the most flows), `tokens/colors.json` + `foundations.json` + `typography.json`, `06-art-direction.md` (Signature Moments #1 and #3)

---

# Component 1 — CalendarDateCell

## 1. Overview
- **Purpose:** Represents one bookable date on the Booking Calendar (M04), showing its state and — for the partner's own bookings — the day number, in a single tappable unit.
- **When to use:** Only within the Booking Calendar grid (M04). Not a general-purpose calendar component for other contexts.
- **When NOT to use:** Do not reuse for the Christmas Window Display (M13) full read-only view — that screen shows a date *range* summary, not an interactive grid of individual cells.
- **Related components:** `StatusBadge` (held/blocked states share color semantics), `PointsBalanceDisplay` (M08 shows cost after a cell is selected).

## 2. Anatomy
| Layer Name | Element Type | Required? |
|---|---|---|
| Root | container (rounded-square) | Required |
| DayNumber | text (dataInline) | Required |
| StateGlyph | icon (small rounded-square accent, Signature Moment #3) | Optional — present for booked/held/christmas-window, absent for available/blocked |
| SelectionRing | border overlay | Optional — visible only when the cell is part of an in-progress selection |

## 3. Variant Matrix
| Property | Values |
|---|---|
| State | available / booked / held / blocked / christmas-window |
| Selected | true / false |
| Disabled | true / false |
| Size | sm (xs breakpoint, ~41px) / md (sm breakpoint, ~55px) |

**Impossible combinations:** `blocked` + `Selected: true` is not supported — a blocked cell cannot enter a selection (it's not tappable at all; see States). `christmas-window` + `Selected: true` is not supported — Christmas Window dates are system-assigned, never partner-selected.

## 4. Specs

### Size: sm (320pt breakpoint)
| Token Category | Spec |
|---|---|
| Width / Height | Fixed, `41px` (computed per `10-responsive-layout.md` Calendar Grid spec) |
| Padding | `spacing.1` (4px) internal |
| Border radius | `border-radius.xs` (6px) |
| Typography | `typography.dataInline`, scaled to fit — see note below |
| Background (available) | `color.calendar.available-bg` |
| Background (booked) | `color.calendar.booked-bg` |
| Background (held) | `color.calendar.held-bg` |
| Background (blocked) | `color.calendar.blocked-bg` |
| Background (christmas-window) | `color.calendar.christmas-window-bg` |
| Border width | `border-width.base` (booked/held/christmas-window only; available/blocked have no border, per `component.booking-calendar-date` tokens) |
| Shadow | `shadow.none` |

### Size: md (375–430pt breakpoint)
Same token references as sm; only the fixed width/height differs (`~55px`, per layout spec) and internal padding steps up to `spacing.2` (8px).

**Note on DayNumber at sm:** `typography.dataInline` is specified at 15px in `09-typography-system.md`; at the 41px sm cell size this needs a component-level down-scale to ~13px to fit comfortably with padding — flag this to SKILL SC as a render-time adjustment, keeping the same font family/weight/tabular treatment, only the size token differs at this one breakpoint.

## 5. Accessibility
| Attribute | Value |
|---|---|
| ARIA role | `button` (each cell is an independent tappable element when available/held; `text`/non-interactive when blocked) |
| ARIA label | e.g. "March 14, available" / "March 15, your booking" / "March 16, pending approval" / "March 17, out of service, not bookable" / "December 24, Christmas Window, assigned to you" |
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
| held (pending approval) | `calendar.held-bg` + border + StateGlyph | Tappable — navigates to M09 showing Pending Approval status | — |
| blocked (out of service) | `calendar.blocked-bg`, muted text, no StateGlyph | **Not tappable** — `aria-disabled="true"`, no selection possible | — |
| christmas-window | `calendar.christmas-window-bg` (gold) + border + StateGlyph | Tappable — navigates to Christmas Window Display (M13) | — |
| selected (available cell mid-range) | `SelectionRing` visible, `border-width.thick` in `color.border.focus` | Part of an active date-range drag/tap selection | ⚡ `transition: border-color duration.fast ease.standard` on entering selection |
| disabled (no boat/no access) | `opacity.disabled` applied to entire cell | Cell present but fully non-interactive (e.g. before qualification approval) | — |
| pressed | Background shifts to `opacity.pressed-tint` overlay | Immediate tap feedback | ⚡ `transition: opacity duration.fast ease.standard` |

**Signature treatment carried through:** the rounded-square `border-radius.xs` shape on every cell state directly echoes the brand icon's interlocking-squares geometry (Signature Moment #3, `06-art-direction.md`) — this is the one non-negotiable visual signature for this component; no state should ever render as a plain colored circle or square-cornered rectangle.

---

# Component 2 — StatusBadge

## 1. Overview
- **Purpose:** Communicates a booking or account status at a glance (Pending Approval, Qualification Pending, Booking Blocked) using a consistent pill shape and semantic color, independent of where it appears.
- **When to use:** Anywhere a partner needs to see the current status of something without opening it — Booking Detail (M09), Home (M03) upcoming-booking summary, Qualification Pending Gate (M20).
- **When NOT to use:** Not for the one-time confirmation moment (M08→M09) — that uses the dedicated gold-chevron confirmation motion (see `component.confirmation-motion` token group), not a badge. Not for the persistent "Confirmed" state either — see note in Variant Matrix.
- **Related components:** `CalendarDateCell` (held state shares the same warning color semantic).

## 2. Anatomy
| Layer Name | Element Type | Required? |
|---|---|---|
| Root | container (pill) | Required |
| Label | text (labelMedium) | Required |
| LeadingIcon | icon | Optional — used for Booking Blocked (warning triangle) only |

## 3. Variant Matrix
| Property | Values |
|---|---|
| Status | pending-approval / qualification-pending / booking-blocked / confirmed |
| hasLeadingIcon | true / false |

**Note:** `confirmed` is included here as a variant (using `color.status.success-*`) even though it's not one of the three states named in the brief, because `05-user-flows.md`'s FLOW-BOOK-01 and FLOW-TOW-01 both end in a persistent "confirmed" label on M09 — per `07-color-system.md`'s explicit rule, this must stay the standard green success semantic, never the gold accent, so it's specified here for completeness rather than left undefined.

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
| Background (pending-approval) | `color.app-status.pending-approval-bg` |
| Text (pending-approval) | `color.app-status.pending-approval-text` |
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
| ARIA role | `status` (announces changes via live region when the badge's status updates while the screen is open — e.g. towing approval resolving in real time) |
| ARIA label | The Label text itself is sufficient (e.g. "Pending Approval") — no separate label needed |
| Minimum touch target | N/A — this is a non-interactive display element, never itself tappable |
| Contrast ratio | All four status pairs inherit from the WCAG matrix in `07-color-system.md` (pending-approval/booking-blocked pass AA+AAA; qualification-pending and confirmed inherit info/success pairs, both AA+AAA) |
| Focus indicator | N/A — not focusable |
| Keyboard navigation | N/A |
| Screen reader announcement | On the status changing while the screen is visible (e.g. FLOW-TOW-01 Case 1/approval resolving): announce the new status text via the `status` live region |

## 6. States
| State | Visual Change | Behavioral Change | Animation? |
|---|---|---|---|
| default | Per Status variant colors above | Purely informational | — |
| status-transition (e.g. pending-approval → confirmed) | Background/text color cross-fades to the new status's tokens | Triggers the ARIA live-region announcement | ⚡ `transition: background-color, color duration.normal ease.standard` |
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
