# Offshore Collective — Art Direction (Partner Mobile App)

**Consumed inputs:** `03-design-brief-parsed.md` (Design Direction), `R1-competitive-brief.md` (differentiation angle), `R2-trend-brief.md` (Trend 01 — Calm Luxury), `04-user-personas.md`, and **client brand assets found in the project root**: `icon-navy.png` + `lockup-navy.png`.

**Correction to `03-design-brief-parsed.md`:** That doc's Open Questions listed brand assets as unconfirmed. They exist — `icon-navy.png` and `lockup-navy.png` in the project root — and are treated below as a hard constraint, not a free choice.

**What the mark tells us:**
- **Icon:** two interlocking rounded-corner squares (an "OC" monogram that reads as a chain-link / partnership motif — apt for 6 co-owners sharing one boat), with a gold/brass chevron breaking free at the bottom-right, suggesting forward motion, departure, "next."
- **Color:** deep desaturated navy (ink-blue, not pure black) as the dominant color, with a warm brass/mustard gold as a single accent — a classic nautical brass-fittings-on-a-navy-hull pairing. This is already a premium, non-generic palette; exact hex values to be sampled precisely in SKILL CS.
- **Wordmark:** wide-tracked, uppercase, monospace/technical letterforms — reads like ship's signage or coordinate/chart-plotter type, not a soft humanist sans. This is a strong, specific signal that rules out generic rounded-sans "friendly app" typography.

---

## 1. Design Personality

Personality: **precise · unhurried · quietly confident · nautical · trustworthy**
POV: "This product should feel like a well-kept ship's logbook, NOT like a booking app trying to sell you something."

---

## 2. Two to Three Distinct Directions

All three stay anchored to the confirmed navy + brass palette (that's fixed by the brand) — they differ in how *warm*, *dense*, and *technical* the interface feels.

| # | Concept name | Mood | Inspiration source | Color temperature intent | Type personality | Shape language | Density | Motion character | Signature move |
|---|---|---|---|---|---|---|---|---|---|
| A | Ship's Instrument | Precise, technical, instrumented | Marine chart plotters, cockpit gauge faces, coordinate readouts | Cool-dominant navy, gold used only as a signal (never decorative) | Technical/monospace throughout, even for body text | Sharp corners, ruled dividers | Compact, information-dense | Snappy, mechanical (tick, lock) | Points balance rendered like a gauge readout |
| B | Quiet Harbour | Calm, unhurried, editorial luxury | Boutique hotel wayfinding, coastal architecture, linen and brass hardware | Cool navy base, gold as generous warmth (not just signal) | Humanist body serif/sans for reading comfort, monospace reserved for numerals/data only | Soft rounded corners (echoing the icon's rounded squares) | Spacious, generous whitespace | Fluid, restrained (gentle ease, no snap) | Points balance and dates set in the wordmark's monospace numerals as a quiet recurring motif |
| C | Brass & Chart | Expressive, heritage, tactile | Varnished teak, ship's brass fittings, hand-drawn nautical charts | Warmer overall, deeper navy shadows, gold used liberally as texture/ornament | Technical display type for headings, warm serif for body | Organic, slightly ornamented dividers (rope/chart-line motifs) | Comfortable, occasional texture | Deliberate, weighty (heavier easing, more travel) | Chart-line dividers between sections instead of plain rules |

**Recommendation:** Direction B — *Quiet Harbour*. It's the only direction that simultaneously satisfies the persona insight (David and Priya are moderate-tech-comfort, occasional users who want reassurance, not a cockpit to operate — Direction A risks feeling like homework), the competitive differentiation angle from R1 ("calmest, most transparent... zero marketplace chrome"), and Trend 01 from R2 (Calm Luxury) — while Direction C's heavier ornamentation risks working against the "reduce cognitive load" trend also flagged in R2.

---

## 3. Chosen Direction (expanded)

## Chosen: Direction B — Quiet Harbour

**Feeling in one paragraph:** In the first three seconds, a partner should feel like they've opened a well-made logbook, not a booking app — generous space around a few trustworthy numbers (their points, their next trip), the navy-and-brass mark quietly present rather than shouting, and type that reads like it was set by someone who cares about craft. Nothing competes for attention; the points balance and the next booking are the only things that matter on Home, and everything else waits its turn.

**Why it fits:** Persona David wants his balance and next trip "with zero taps" — Quiet Harbour's spaciousness makes that one glance, not a search. Persona Priya is anxious about getting her first booking wrong — a calm, unhurried interface reduces the perceived stakes of every screen, rather than an instrumented, gauge-like Direction A that would make a first-timer feel like she needs training to read the UI. The competitive angle from R1 (avoid marketplace chrome, keep the points system prominent but not sterile) is best served by warmth, not by pure instrumentation.

**What it rejects:** The cockpit/gauge-panel technicality of Direction A (too cold and effortful for an occasional, moderate-tech-comfort user) and the ornamental heaviness of Direction C (risks reading as dated skeuomorphism rather than premium restraint, and works against the "reduce cognitive load" trend from R2).

---

## 4. Signature Moments

| Moment | Where it lives | The treatment | Why it matters |
|---|---|---|---|
| Hero / first-impression | Home (M03) | Points balance and next booking rendered in the wordmark's tracked-out monospace numerals, large and alone at the top of the screen, with generous whitespace below before the quick-links row | Sets the "logbook, not app" tone immediately — the first thing a partner sees is *their* number, not chrome |
| Empty state with personality | Home, no upcoming booking (M21) | Instead of a generic "no bookings" icon, use a single line-art rendering of the icon's chevron pointing toward the Book tab, with reassuring copy ("Nothing booked yet — your next trip starts here") | Turns a dead moment into a brand touchpoint instead of a apology |
| Signature component treatment | Booking Calendar (M04) date states | Available/booked/held/blocked dates distinguished by the brand's rounded-square motif (not generic colored dots) — a small rounded-square glyph per date state, echoing the icon's interlocking squares | Ties the calendar — the app's most-used screen — directly to the brand mark instead of a generic calendar library look |
| Signature transition | Booking confirmation (M08 → M09) | On confirm, the gold chevron from the icon animates a single quiet "forward" motion (not a checkmark burst) before settling into the confirmed state | Reuses the mark's own forward-motion element as the app's confirmation language — ownable, not a stock success animation |
| Micro-delight / data moment | Points balance update (after booking or cancellation) | The balance number ticks (counts) from old to new value over ~400ms rather than snapping instantly | Makes the app's core scarce resource feel tangible and honestly tracked, reinforcing trust in the fairness system |

---

## 5. Anti-Template Guardrails

- ❌ Default SaaS-blue (`#0070F3`/`#3B82F6`) as an accent → instead the only accent color is the brand's brass/gold, used sparingly and intentionally (CTAs, the points number, confirmation moments).
- ❌ Generic thin-line icon set with no relationship to the brand → instead icons should echo the rounded-square/chevron vocabulary of the mark where plausible (see SKILL IC).
- ❌ Every card = same white rectangle with the same drop shadow → instead Quiet Harbour uses generous whitespace and rounded-square containers rather than shadow-heavy card stacks; depth comes from spacing, not shadow.
- ❌ Bottom-tab icons as generic filled/outline pairs with no brand character → instead the active-tab state should use a subtle brass underline/dot rather than a full color fill, keeping gold rare and meaningful.
- ❌ Success = generic checkmark animation → instead reuse the mark's own gold chevron as the confirmation motif (see Signature Moments above) — never introduce a second, unrelated "success" visual language.
- ❌ Chasing Trend 04 (platform-native motion) into snappy, mechanical iOS/Material transitions everywhere → Quiet Harbour's motion character is fluid and restrained; apply platform-native *conventions* (swipe-back, system gestures) but keep custom transition *feel* unhurried, not snappy — this is a deliberate conflict with a literal reading of Trend 04, and Direction B's calmness wins.

---

## 6. Moodboard Brief

- "boutique coastal hotel wayfinding signage" — the calm, confident typographic restraint this app should borrow
- "brass ship fittings on varnished navy hull" — the exact color-pairing feeling already present in the logo
- "linen texture flat lay, neutral tones" — spacious, unhurried composition reference for backgrounds/negative space
- "architectural coastal house, minimal concrete and timber" — shape language reference for rounded-but-restrained containers
- "ship's logbook handwriting ledger page" — the emotional register of "a trustworthy record," relevant to the points balance and checklist screens
- "chart plotter coordinate readout, dim navy screen" — reference for the numeral/monospace treatment, used sparingly, not as the dominant UI language
- "slow tide timelapse, dawn harbour" — motion-pacing reference (fluid, unhurried, never abrupt)

---

## 7. Handoff to Tokens & Foundations

| Downstream skill | Constraint from this direction |
|---|---|
| alice-color-system | Sample exact navy + brass hex values directly from `icon-navy.png`/`lockup-navy.png` (do not invent a new blue). Navy is the dominant/base color; gold/brass is a single, rare accent — never a secondary "brand blue + brand gold in equal weight" palette. Contrast personality: high-contrast navy-on-white for text, gold reserved for accents/CTAs only, never for large fills. |
| alice-typography-system | Headings and data (points, dates, booking codes) should draw on the wordmark's tracked-out monospace/technical character; body text needs a separate, more humanist/readable companion face for comfort — do not use the technical face for paragraph copy. Avoid soft, rounded, "friendly SaaS" sans faces (Inter/Roboto-adjacent) as the sole typeface. |
| alice-responsive-layout | Spacious, generous margins and vertical rhythm (Quiet Harbour density = spacious, not compact) — resist the urge to pack more onto one mobile screen than the persona needs at once. |
| alice-component-design | Rounded-square containers (not sharp corners, not heavy shadow-card stacks); calendar date-state glyphs should reference the icon's interlocking-square motif per Signature Moments. |
| alice-motion-design | Fluid, restrained easing (gentle ease-in-out, no bounce/spring snap); the gold chevron confirmation motif is the one signature transition — reserve it for genuine confirmation moments only, not routine navigation. |
| alice-icon-asset-spec | Icon set should lean toward rounded-square/geometric forms consistent with the mark, not a generic thin-line library pulled as-is; sparing use of brass/gold fill, mostly navy/neutral line icons. |

---

## Note
Admin Portal and Contractor Mini-Portal are out of scope for this art direction pass, per user instruction narrowing the pipeline to the Partner Mobile App only.
