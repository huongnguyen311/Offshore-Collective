# Offshore Collective — Icon & Asset Spec (Partner Mobile App)

**Consumed inputs:** `06-art-direction.md` (icon/imagery guardrails), `tokens/foundations.json` (border-radius, navy/gold color tokens)

---

## 1. Icon System

**Library: Phosphor Icons, "Regular" weight**
Rationale: Phosphor's Regular weight uses consistently rounded joins and stroke caps at a 24px keyline grid — the closest off-the-shelf match to the brand mark's own rounded-square geometry, without needing a fully custom icon set (which the WBS budget doesn't scope). Its large, consistent glyph set also covers every icon this app needs (calendar, checklist toggles, camera/upload, notification types, profile) from one source — satisfying the one-library rule.

**Grid:** 24px keyline, all icons drawn/exported on this grid regardless of display size (scaled via the size tokens below, not re-drawn per size).

**Stroke width:** 1.5px at 24px base size (Phosphor Regular default) — kept slightly lighter than Phosphor's "Bold" weight to match the art direction's "delicate line weight" principle already established for border-width tokens in `foundations.json`.

**Corner style:** Rounded joins/caps throughout (Phosphor Regular's default) — never mixed with Phosphor's "Sharp" variant, which would contradict the rounded-square brand geometry.

**Sizing scale (token-based):**
| Token | Size | Usage |
|---|---|---|
| `size.icon.sm` | 16px | Inline icons within body text, StatusBadge leading icon |
| `size.icon.md` | 20px | Default UI icons — list rows, form field icons |
| `size.icon.lg` | 24px | Tab bar icons, screen-header icons |
| `size.icon.xl` | 32px | Empty-state icon (M21), onboarding moments |

**Color:** Always via `color.icon.*` tokens — new tokens needed (not yet defined in `colors.json`), added here:
```
color.icon.default   = color.text.secondary   (navy/neutral line icons — the default, per art direction: "mostly navy/neutral")
color.icon.active     = color.text.brand       (active tab-bar icon)
color.icon.accent     = color.action.accent    (gold — sparing use only: confirmation moments, Christmas Window icon accents)
color.icon.inverse    = color.text.inverse
```

---

## 2. Icon Inventory

| Semantic name | Where used | Phosphor glyph | Size token |
|---|---|---|---|
| icon-tab-home | Tab bar — Home | `house` | size.icon.lg |
| icon-tab-book | Tab bar — Book | `calendar-blank` | size.icon.lg |
| icon-tab-rules | Tab bar — Rules | `book-open` | size.icon.lg |
| icon-tab-alerts | Tab bar — Notification Center | `bell` | size.icon.lg |
| icon-tab-profile | Tab bar — Profile | `user-circle` | size.icon.lg |
| icon-fuel-full-yes | Checklist — tank full toggle (yes) | `check-circle` | size.icon.md |
| icon-fuel-full-no | Checklist — tank full toggle (no) | `x-circle` | size.icon.md |
| icon-damage-yes | Checklist — damage toggle (yes) | `warning-circle` | size.icon.md |
| icon-damage-no | Checklist — damage toggle (no) | `check-circle` | size.icon.md |
| icon-camera | Checklist — photo capture/upload | `camera` | size.icon.md |
| icon-photo-attached | Checklist — photo thumbnail overlay (retake affordance) | `arrows-clockwise` | size.icon.sm |
| icon-notification-booking | Notification Center — booking confirmed type | `calendar-check` | size.icon.md |
| icon-notification-boat-ready | Notification Center — boat ready type | `anchor` | size.icon.md |
| icon-notification-payment | Notification Center — payment due type | `receipt` | size.icon.md |
| icon-notification-access | Notification Center — access opened type | `lock-open` | size.icon.md |
| icon-notification-release | Notification Center — release notification type | `bell-ringing` | size.icon.md |
| icon-status-blocked | StatusBadge — booking-blocked leading icon | `warning` | size.icon.sm |
| icon-empty-boat | Home empty state (M21) — line-art chevron-pointing motif | Custom (see Section 3) | size.icon.xl |
| icon-offline | Offline banners | `wifi-slash` | size.icon.sm |
| icon-retry | Error states — retry action | `arrow-clockwise` | size.icon.md |
| icon-towing | Towing destination entry (M10) | `map-pin` | size.icon.md |
| icon-secondary-operator | Profile — add secondary operator | `user-plus` | size.icon.md |
| icon-invoice | Payments & Invoices | `file-text` | size.icon.md |
| icon-back | Navigation bar back action | `arrow-left` | size.icon.lg |

**Calendar date-state glyphs are NOT Phosphor icons** — per `11-component-specs.md`, `CalendarDateCell`'s StateGlyph is a bespoke rounded-square accent shape (echoing the brand icon directly), not a library glyph. It's specified as a component asset, not an icon-library entry — listed here for completeness only:

| Semantic name | Where used | Shape | Notes |
|---|---|---|---|
| glyph-calendar-booked | CalendarDateCell, booked state | Small filled rounded-square, `border-radius.xs`, `color.calendar.booked-border` | Custom, not from Phosphor |
| glyph-calendar-held | CalendarDateCell, held state | Same shape, `color.calendar.held-border` | Custom |
| glyph-calendar-christmas | CalendarDateCell, christmas-window state | Same shape, `color.calendar.christmas-window-border` (the one everyday gold glyph) | Custom |

---

## 3. Illustration / Imagery Style

**Style direction:** Two distinct registers, matching the art direction's "calm luxury" personality:

1. **Line-art brand illustrations** (custom, not photographic) — used exactly once, for the Home empty state (M21)'s Signature Moment #2: a single-weight line rendering of the icon's gold chevron pointing toward the Book tab, drawn at the same 1.5px stroke weight as the icon system for visual consistency, with the chevron rendered in `color.icon.accent` (gold) as the one deliberate everyday accent use outside the calendar's Christmas Window glyph.
   - Do: keep it to one shape, generous negative space around it, no additional decorative elements.
   - Don't: turn this into a full illustrated scene (boats, water, horizon) — that would contradict the restrained, single-motif signature-moment principle.

2. **Boat photography** — used only where the moodboard brief calls for it: potentially a hero image context (e.g. a subtle background treatment behind the Home hero block, if the client supplies real fleet photography later) — but **not currently required by any WBS-scoped screen**. The WBS's screen inventory (booking, checklists, points) doesn't call for photographic hero imagery anywhere in the Core screens actually specified. Flagging this explicitly: **no boat photography asset is required for MVP screens as scoped** — if Matt supplies fleet photography later for a Home-screen treatment, it should follow:
   - Aspect ratio: 16:9, cropped to keep the horizon line roughly in the lower third (per the moodboard's "coastal architecture" restraint)
   - Treatment: a subtle navy gradient overlay (`color.bg.inverse` at low opacity) if text needs to sit on top, never a busy/high-contrast crop
   - This entire category is a **future enhancement note**, not a current deliverable — do not build a photography pipeline for launch.

**Empty-state illustrations beyond M21:** per `13-state-gallery.md`, Notification Center (M06) and Payments & Invoices (M11) also have empty states, but neither was specified with a bespoke illustration in that document — they use a simple icon (`icon-tab-alerts` / `icon-invoice` at `size.icon.xl`, in `color.icon.default`) rather than custom line art, reserving the custom-illustration treatment for the Home screen only (the highest-traffic empty state, and the one tied to the brand's signature chevron motif).

---

## 4. Image Treatment

- **Checklist photo thumbnails** (fuel gauge, damage): aspect ratio native/uncropped (per WBS: "full resolution, not compressed" — cropping to a fixed ratio would work against that requirement). Display as a square thumbnail (`object-fit: cover`, 1:1 crop for the *thumbnail preview only* — the underlying stored file stays full, uncropped resolution).
- **Placeholder strategy:** dominant-color placeholder (sampled from the image once uploaded) while a thumbnail is generating/loading — not a generic gray box, and not a blur-up (blur-up needs a low-res variant, which conflicts with the no-compression requirement's spirit if implemented via an actual downsampled asset; a flat dominant-color swatch avoids that ambiguity).
- **Alt text:** every checklist photo needs alt text generated from context, not the filename — e.g. "Fuel gauge photo, [boat name], [date]" / "Damage photo, [boat name], [date]".
- **Boat photography (future, per Section 3):** if added later, `object-fit: cover` with focal point set to the horizon-lower-third crop described above; alt text should name the boat (e.g. "Rayglass 3000 at anchor").

---

## 5. Export Manifest

| Asset name (kebab-case) | Format | Sizes | Used on | Notes |
|---|---|---|---|---|
| logo-lockup-navy | SVG | vector | Login (M02), Splash (M01) | Existing client asset (`lockup-navy.png`) — re-exported as SVG for crisp scaling; PNG source retained as fallback |
| logo-icon-navy | SVG | vector | App icon source, in-app small brand mark if needed | Existing client asset (`icon-navy.png`) — re-exported as SVG |
| icon-* (all Section 2 entries) | SVG | vector | Throughout | Phosphor Regular, recolored via `color.icon.*` tokens at render time — never baked-in color |
| glyph-calendar-* (3 entries) | SVG | vector | CalendarDateCell | Custom, built from `border-radius.xs` shape token, not Phosphor |
| illustration-empty-boat-chevron | SVG | vector | M21 Home empty state | Custom line-art, gold stroke |
| hero-boat-photography-placeholder | N/A | N/A | None currently | **Not exported for this release** — see Section 3 future-enhancement note; no placeholder asset needed since no screen currently requires it |

**Raster note:** because every asset above is vector (SVG) — the icon system, the calendar glyphs, the one custom illustration, and the re-exported logo — **no @1x/2x/3x raster set is required for this release**. The only raster content in the entire app is user-uploaded checklist photos, which are user data, not design assets, and are explicitly exempt from any export/compression pipeline per the WBS's "full resolution, no compression" rule.

---

## 6. Platform Icons

| Asset | Sizes | Source |
|---|---|---|
| iOS App Icon | 1024×1024 (App Store), plus standard iOS icon size set generated from it | Derived from `logo-icon-navy` — the interlocking-squares mark alone (no wordmark, per standard app-icon convention), on a `color.bg.inverse` (navy) or white background — **flag to client/Matt which background they prefer**, not specified in the brand assets provided |
| Android Adaptive Icon | Foreground + background layers, standard adaptive icon size set | Foreground: `logo-icon-navy` mark; background: solid `color.bg.default` or navy — same open flag as iOS above |
| Splash / Launch Screen | Standard per-device launch sizes | `logo-lockup-navy` centered on `color.bg.default`, generous surrounding whitespace per Quiet Harbour restraint — no loading spinner on the splash itself (per `13-state-gallery.md`, loading states begin on M01 after the brand moment, not layered on top of it) |

**Open question carried forward:** App icon background color (navy vs. white/light) isn't determined by the existing brand assets alone — recommend a quick check with Matt before finalizing platform icon exports.

---

## Handoff
Feeds `alice-screen-design` (SKILL SC), where these icons/illustrations are placed into real screens, and asset download via the Figma MCP for the actual SVG files once screens are built in Figma.
