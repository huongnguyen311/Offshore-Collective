# Offshore Collective — Motion Design (Partner Mobile App)

**Consumed inputs:** `tokens/foundations.json` (duration/easing tokens), `11-component-specs.md` (CalendarDateCell, StatusBadge, PointsBalanceDisplay animation flags), `06-art-direction.md` (Signature Moment #4 — the gold-chevron confirmation motion)

**Token mapping note:** this skill's reference template names tokens `duration.fast/normal/complex` and `easing.decelerate/accelerate/standard/spring`. This project's tokens (`tokens/foundations.json`) use `duration.fast(120ms)/normal(220ms)/slow(360ms)/confirm(560ms)` and `easing.standard/enter/exit/confirm`. Mapping: `easing.enter` serves the decelerate role, `easing.exit` serves the accelerate role, `easing.standard` serves the standard/within-screen role. **There is no spring/bounce token anywhere in this system** — `06-art-direction.md`'s anti-template guardrail explicitly rejects spring physics even for interactive drag/snap-back, which deliberately deviates from this skill's own default recommendation of `easing.spring` for that case. Every interactive gesture in this app (calendar selection, sheet dismissal) uses `easing.standard` or `easing.exit` instead — never a spring curve.

---

## 1. Signature Transition — Gold-Chevron Booking Confirmation (M08 → M09)

**Animation Purpose**
- Category: delight / orientation (the one moment in the app engineered to be memorable)
- Message: "Your booking is confirmed — this took a moment on purpose, because it matters."

**Specification Table**

| Property | Value | Token |
|---|---|---|
| Duration | 560ms | `duration.confirm` |
| Easing | `cubic-bezier(0.16, 1, 0.3, 1)` | `easing.confirm` |
| Delay | 100ms (after the PointsBalanceDisplay count-down tween completes — see Section 3) | — |
| Properties animated | `transform: translateX/scale` (chevron shape), `opacity` (chevron + screen crossfade) | — |

**Keyframes**
```
Stage 1 (0–60% of duration): the icon's gold chevron glyph appears center-screen
  opacity: 0 → 1
  scale: 0.85 → 1.0
Stage 2 (60–100% of duration): the chevron travels forward and the screen crossfades to M09
  chevron translateX: 0 → +40px (moving in its own "forward" direction, echoing the mark)
  chevron opacity: 1 → 0
  M09 content opacity: 0 → 1
```

Only 2 properties animate at once per stage (`opacity` + one `transform` axis), per the max-3 rule.

**Platform Notes**
- iOS: implement with `useNativeDriver: true` (React Native) — `transform` and `opacity` only, consistent with the properties chosen above.
- Android: treat as a **container transform** (Material Motion) from M08 to M09, since it's a persistent-element-style transition (the booking itself persists across the two screens) rather than a fade-through (which is for unrelated destinations).
- Web (Admin Portal only, out of scope here): N/A.

**Reduced Motion**
- Skip Stage 1/2 entirely. Show the chevron at full opacity, static, for 80ms, then cut directly to M09 at full opacity. No translate, no scale — a simple acknowledgement, not the full sequence, per the skill's default reduced-motion fallback pattern.

---

## 2. Standard Screen Transitions

### Tab switches (Home ↔ Book ↔ Rules ↔ Alerts ↔ Profile)

**Animation Purpose**
- Category: navigation
- Message: "You've moved to a peer section, not drilled deeper."

**Specification Table**

| Property | Value | Token |
|---|---|---|
| Duration | 220ms | `duration.normal` |
| Easing | `cubic-bezier(0.33, 1, 0.68, 1)` | `easing.standard` |
| Delay | 0ms | — |
| Properties animated | `opacity` only | — |

**Keyframes**
```
outgoing tab content: opacity 1 → 0
incoming tab content: opacity 0 → 1
```
No transform — per Material's fade-through pattern for unrelated, peer-level destinations (tabs are not hierarchically related to each other).

**Platform Notes**
- iOS: standard tab-bar crossfade, no custom transform needed.
- Android: Material "fade through" — exactly matches this case (peer destinations, not shared-axis).

**Reduced Motion:** instant switch, `opacity 0 → 1` at 0ms (no fade at all, since even a fade is a transform-adjacent motion some reduced-motion users prefer to skip entirely for frequent, low-stakes navigation).

### Push navigation (e.g. M09 → M15 Pre-Departure Checklist, M04 → M08)

**Animation Purpose**
- Category: navigation
- Message: "You've drilled deeper into a related task."

**Specification Table**

| Property | Value | Token |
|---|---|---|
| Duration (enter) | 220ms | `duration.normal` |
| Easing (enter) | `easing.enter` — `cubic-bezier(0.22, 1, 0.36, 1)` | — |
| Duration (exit, on back) | ~170ms (≈ 75% of enter, per the "exit is shorter" rule) | — |
| Easing (exit) | `easing.exit` — `cubic-bezier(0.4, 0, 1, 1)` | — |
| Properties animated | `translateX`, `opacity` | — |

**Keyframes**
```
Enter: translateX 100% → 0%, opacity 0 → 1
Exit (back gesture): translateX 0% → 100%, opacity 1 → 0
```

**Platform Notes**
- iOS: standard push/pop navigation-stack transition; honor swipe-back interactively (the exit animation should be scrubbable, not just a fixed timeline, when the user is actively dragging back).
- Android: Material "shared axis" (same-hierarchy navigation) — X-axis. Honor predictive back gesture (Android 13+): begin the exit animation immediately on gesture initiation, not on gesture completion.

**Reduced Motion:** `opacity 0 → 1` only, 80ms, no translateX.

---

## 3. PointsBalanceDisplay Ticking-Number Animation

*(Full component context in `11-component-specs.md`, Component 3, States table — this section is the detailed motion spec that component's spec deferred here.)*

**Animation Purpose**
- Category: delight / feedback
- Message: "Your points balance genuinely changed — watch it happen, don't just believe a new number."

**Specification Table**

| Property | Value | Token |
|---|---|---|
| Duration | 360ms — see note | `duration.slow` |
| Easing | `cubic-bezier(0.33, 1, 0.68, 1)` | `easing.standard` |
| Delay | 0ms (starts immediately when the new value is known — e.g. right after M08's Confirm tap resolves) | — |
| Properties animated | Numeric value interpolation (not a CSS-animatable property in the traditional sense — implemented via a JS-driven counter, not `transform`/`opacity`) | — |

**Keyframes**
```
displayed number: [old value] → [new value], interpolated linearly across the duration,
  easing applied to the interpolation curve (not a transform), rounding to whole points at each frame
```

**Note on the 360ms value:** `06-art-direction.md`'s Signature Moment #5 describes this as "~400ms" — 360ms is the closest existing token (`duration.slow`) rather than a new one-off value, a deliberate rounding, not an oversight (flagged in `11-component-specs.md` and re-confirmed in `20-design-review.md`).

**Platform Notes**
- iOS/Android: implement as a driven value (e.g. `Animated.Value` in React Native) updating a text node each frame — this is the one animation in the system that is NOT a `transform`/`opacity` pair, since the "animated property" is the text content itself. Do not attempt to fake this with opacity crossfades between two static numbers — the counting motion is the point.
- On the `blocked` variant (inline, resulting balance would go negative): no ticking animation plays — the number simply renders in its final (blocked-styled) state immediately, since nothing is actually being confirmed/deducted in that case.

**Reduced Motion:** skip the interpolation entirely — render the new value instantly. This is one of the few places reduced-motion removes information-bearing motion rather than just decoration, but the number itself (the information) is unaffected either way, so this is a safe simplification.

---

## 4. Reduced-Motion Summary (all animations in this system)

| Animation | Reduced-motion fallback |
|---|---|
| Gold-chevron confirmation (Section 1) | Static chevron at full opacity for 80ms, then cut to M09 |
| Tab switches (Section 2) | Instant, no fade |
| Push navigation (Section 2) | 80ms opacity-only fade, no translateX |
| PointsBalanceDisplay tick (Section 3) | Instant final value, no interpolation |
| CalendarDateCell selection ring (`11-component-specs.md`) | Instant border-color change, no transition |
| StatusBadge status-transition (`11-component-specs.md`) | Instant color change, no crossfade |

**Detection:**
- iOS/React Native: `AccessibilityInfo.isReduceMotionEnabled()`, checked once on mount and subscribed to changes.
- Android: respect `Settings.Global.TRANSITION_ANIMATION_SCALE` via the platform's standard reduced-motion API.

---

## Handoff
Feeds `alice-screen-design` (SKILL SC), where these transitions are wired into the actual rendered screens/prototype, and `alice-a11y-spec` (SKILL A11Y), which should confirm the reduced-motion fallbacks above satisfy WCAG 2.3.3 (Animation from Interactions).
