# Offshore Collective — Responsive Layout (Partner Mobile App)

**Consumed inputs:** `01-information-architecture.md` (Mobile IA — 5-tab bar: Home/Book/Rules/Alerts/Profile), `tokens/foundations.json` (spacing scale), `09-typography-system.md` (Data Hero + Eyebrow Label hero block)

**Scope note:** The WBS explicitly confirms portrait-only ("The mobile application under development will support portrait mode only"). Landscape and tablet/desktop breakpoints are therefore **out of scope** for this surface — this document covers phone-width variance only (small phone → large phone), not orientation or device-class variance.

---

## 1. Grid System (phone-width variance only)

| Breakpoint | Width | Columns | Margin | Gutter |
|---|---|---|---|---|
| xs (small phone, e.g. iPhone SE-class) | 320–374pt | 4 | `spacing.4` (16px) | `spacing.3` (12px) |
| sm (standard/large phone, e.g. iPhone 14/15, large Android) | 375–430pt | 4 | `spacing.5` (20px) | `spacing.4` (16px) |

Only two breakpoints exist for this surface — there is no `md`/`lg`/`xl` tier, since tablet and desktop are not in scope. The margin step-up from `xs` to `sm` (16px → 20px) reflects Quiet Harbour's spacious rhythm scaling *up* on larger phones rather than staying fixed, so the layout never feels cramped relative to the extra width.

---

## 2. Mobile (iOS/Android)

### Safe Area Insets
| Edge | Token / value |
|---|---|
| Top | Device safe-area-inset-top (notch/Dynamic Island — OS-provided, not a fixed token) |
| Bottom | Device safe-area-inset-bottom (~34pt iPhone home indicator; ~0–24dp Android gesture bar) |
| Left/Right | 0 — portrait-only, no side insets apply |

### Standard Component Heights
| Component | Height |
|---|---|
| Status bar | OS-provided (~54pt iOS, ~24dp Android) |
| Navigation bar (screen title + back) | 44pt (iOS) / 56dp (Android) |
| Tab bar (5 items: Home/Book/Rules/Alerts/Profile) | 49pt (iOS) / 56dp (Android), + safe-area-inset-bottom |
| List row (booking list, notification list) | `spacing.12` (48px) minimum, using `typography.bodyMedium` |
| Calendar date cell (M04) | Square, sized to fit 7 columns within content width minus margins — see Layout Patterns below |

### Thumb-Reach Zones (portrait, single-hand use)
| Zone | Region | Guidance for this app |
|---|---|---|
| Comfortable | Bottom 40% | Tab bar, primary Confirm/Submit buttons (M08 Confirm, M15/M16 Submit) |
| Stretch | Middle 40% | Calendar date selection (M04), list row taps |
| Hard to reach | Top 20% | Screen title, back navigation, Data Hero display (M03) — display-only content belongs here since it's read, not tapped |

This maps cleanly onto the art direction's hero-at-top composition: the Data Hero points balance lives in the "hard to reach" zone deliberately, because it's a glanceable read, not an interactive target — every actual tap target (quick-links row, tab bar) sits lower, in the comfortable zone.

---

## 3. Layout Patterns Used in This App

| Pattern | Where used | 320pt risk |
|---|---|---|
| **Single Column** | Home (M03), Profile (M07), Boat Rules (M05), all form/checklist screens (M15/M16/M18/M19), all detail screens (M09, M11-M14) | Safe |
| **Tab Bar** | Global navigation (5 items) | [WARN] see 320pt checklist below — 5 labelled items is tight at 320pt |
| **Calendar Grid** *(app-specific pattern, not in the skill's generic list)* | Booking Calendar (M04) | [WARN] 7-column grid is the tightest layout in the app at 320pt — see note below |
| **Bottom Sheet** | Booking Blocked (M22) rule-violation message, Cancel Booking (M17) confirmation | Safe |
| **Sticky Header + Scroll** | Boat Rules (M05, grouped-by-topic long content), Notification Center (M06) | Safe |

**No Card Grid, Split View, Master-Detail, Side Drawer, or FAB** — this app's IA (5-tab bar, no multi-pane needs, no single dominant recurring action outside the tab bar) doesn't call for any of them; introducing one would add unscoped complexity per the art direction's restraint principle.

### Calendar Grid — app-specific spec (not in the skill's default pattern list)
- 7 columns (days of week), portrait-only.
- At `xs` (320pt): content width = 320 − (2 × 16px margin) = 288px → each date cell ≈ 41px, with `spacing.1` (4px) gutter between cells. This is tight but workable for a single rounded-square glyph + `typography.dataInline` numeral (per `09-typography-system.md`).
- At `sm` (375–430pt): cells scale up proportionally to as much as ~55px, giving more breathing room for the rounded-square date-state glyph (Signature Moment #3).
- **320pt flag:** below 41px per cell, the rounded-square glyph and tabular numeral both risk feeling cramped — if a future device target goes narrower than 320pt (unlikely for current iOS/Android), collapse to a week-list view instead of a 7-column grid.

---

## 4. Spacing Rules (mobile-only — no desktop column since out of scope)

| Spacing type | Token | Value |
|---|---|---|
| Section spacing (e.g. between Home's hero block and quick-links row) | `spacing.10` | 40px |
| Component spacing (e.g. between form fields on a checklist) | `spacing.5` | 20px |
| Edge margin | `spacing.4` (xs) / `spacing.5` (sm) | 16px / 20px |
| Element gap (icon-to-label, inline) | `spacing.2` | 8px |
| Hero block internal gap (Eyebrow Label → Data Hero → next-booking line) | `spacing.3` | 12px |

These pull from the extended spacing scale in `tokens/foundations.json` (steps `5` through `24`), favoring the larger steps for primary rhythm exactly as that file's rationale specifies.

---

## 5. Auto Layout Spec (Figma)

### Single Column (Home, Profile, Boat Rules, forms/checklists, detail screens)
| Property | Value |
|---|---|
| Direction | Vertical |
| Alignment | Top · Center |
| Gap | `spacing.10` between major sections, `spacing.5` between fields within a section |
| Padding (H) | `spacing.4` (xs) / `spacing.5` (sm) |
| Padding (V) | `spacing.6` |
| Width resizing | Fill container |
| Height resizing | Hug contents (scroll enabled at frame level) |

### Home Hero Block (Eyebrow Label + Data Hero + next-booking line)
| Property | Value |
|---|---|
| Direction | Vertical |
| Alignment | Top · Center |
| Gap | `spacing.3` |
| Padding (top) | `spacing.10` (below the nav bar, generous per Quiet Harbour) |
| Padding (bottom) | `spacing.10` (before the quick-links row begins) |
| Width resizing | Fill container |

### Calendar Grid (M04)
| Property | Value |
|---|---|
| Direction | Grid, 7 fixed columns |
| Gap | `spacing.1` (4px) between cells |
| Padding (H) | `spacing.4` (xs) / `spacing.5` (sm) |
| Cell sizing | Square, `(content width − 6 × gutter) / 7` |
| Height resizing | Hug contents per row |

### Bottom Sheet (M22 Booking Blocked, M17 Cancel confirmation)
| Property | Value |
|---|---|
| Direction | Vertical |
| Alignment | Top · Center |
| Gap | `spacing.4` |
| Padding (H) | `spacing.4` |
| Padding (top) | `spacing.6` |
| Padding (bottom) | safe-area-inset-bottom + `spacing.4` |
| Width resizing | Fill container |
| Height resizing | Hug contents |

### Sticky Header + Scroll (Boat Rules, Notification Center)
| Property | Value |
|---|---|
| Header height | Fixed (44pt / 56dp) |
| Header padding (H) | `spacing.4` |
| Scroll area | Fill width, fill remaining height |

---

## 320pt Breakpoint Checklist

- [x] No horizontal overflow at 320pt on any single-column screen (verified against `spacing.4` margins)
- [x] Tab bar labels — **flagged**: 5 items ("Home", "Book", "Rules", "Alerts", "Profile") at 320pt risk clipping with labels. **Recommendation:** keep labels short (already ≤6 characters each) and test at 320pt in SKILL SC; fall back to icon-only tab bar below 340pt if clipping is confirmed in screen design.
- [x] Calendar grid — flagged above; workable at 320pt but the tightest layout in the app, no further compression possible without changing to a list view
- [x] Bottom sheets respect safe-area-inset-bottom
- [x] No FAB, split view, or master-detail patterns exist, so their respective 320pt risks don't apply

---

## Handoff
- SKILL 08 (Component Design) — the Calendar Grid's date-cell component and the Home Hero Block should be built as reusable components referencing the Auto Layout specs above.
- SKILL SC (Screen Design) — resolve the tab-bar label-clipping flag with an actual rendered check at 320pt before locking the tab bar component.
