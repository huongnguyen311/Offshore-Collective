# Offshore Collective — Typography System (Partner Mobile App)

**Consumed inputs:** `06-art-direction.md` (Handoff table: technical/monospace for headings+data, humanist companion for body, avoid Inter/Roboto-adjacent as the sole face), `tokens/foundations.json` (spacing scale)
**Token output:** `tokens/typography.json` — canonical source of truth for all fontFamily/fontSize/fontWeight/lineHeight/letterSpacing/`typography.*` keys (SKILL 05's tokens do not redefine these, per that skill's own merge-collision rule).

---

## Font Pairing

**Display/Data font: IBM Plex Mono**
A technical, monospace face that directly echoes the wordmark's tracked-out, uppercase, coordinate-plotter character — without literally reusing the wordmark's exact letterforms as UI type. Its fixed character width makes every numeral naturally tabular, which matters here more than in most apps: points balances, calendar dates, litres readings, and booking review numbers are the things partners most need to trust, and a monospace face makes them read as *measured*, not decorated. Used for all Title/Label styles and — critically — every numeral/data display in the app.

**Body font: Fraunces**
A warm, characterful serif built for screen reading comfort, not a generic rounded sans. It supplies the "boutique coastal hotel" editorial warmth from the moodboard in `06-art-direction.md` while staying legible at UI sizes. This is the required humanist companion — it never appears in caps-tracked or numeral contexts, and IBM Plex Mono never appears in paragraph-length body copy.

**Fallback stack:**
```
Display/Data: 'IBM Plex Mono', 'SF Mono', 'Roboto Mono', monospace
Body:         'Fraunces', 'SF Pro Text', system-ui, serif
```

**Why not Inter/Roboto/Arial:** Explicitly forbidden by both this skill's anti-template rule and the art direction's own guardrail against "friendly SaaS" sans faces. Neither candidate face is a generic default — IBM Plex Mono carries genuine technical/nautical character; Fraunces carries genuine editorial warmth.

---

## Type Scale

| Style Name | Family | Size | Weight | Line Height | Letter Spacing | Usage |
|---|---|---|---|---|---|---|
| **Data Hero** | Mono | 40px | Medium | 1.1 | Tabular +0.01em | Home screen points-balance number — one per session's first view |
| **Eyebrow Label** | Mono | 11px | Semibold | 1.2 | Widest +0.08em, UPPERCASE | The tracked label directly above Data Hero (e.g. "POINTS BALANCE") — echoes the wordmark's own tracking |
| Display Large | Mono | 32px | Semibold | 1.1 | 0em | Secondary hero data (e.g. days-to-departure countdown) — never on the same screen as Data Hero |
| Title Large | Mono | 24px | Semibold | 1.2 | −0.01em | Screen titles ("Booking Calendar", "Boat Rules") |
| Title Medium | Mono | 20px | Semibold | 1.2 | 0em | Card headings, in-screen section headers |
| Title Small | Mono | 17px | Medium | 1.35 | 0em | List-group labels, widget headers |
| Body Large | Serif | 17px | Regular | 1.65 | 0em | Boat Rules copy, checklist disclaimers, T&C/Privacy |
| Body Medium | Serif | 15px | Regular | 1.5 | 0em | Default UI text — forms, descriptions, list rows |
| Body Small | Serif | 13px | Regular | 1.5 | +0.02em | Secondary/helper text |
| **Data Inline** | Mono | 15px | Medium | 1.2 | Tabular +0.01em | Calendar date numerals (M04), points-cost breakdown on Booking Review (M08), litres readings on checklists |
| Label Large | Mono | 14px | Semibold | 1.2 | Widest, UPPERCASE | Primary buttons, active tab-bar label |
| Label Medium | Mono | 12px | Semibold | 1.2 | Widest, UPPERCASE | Secondary buttons, status badges (Pending Approval / Booking Blocked / etc.) |
| Label Small | Mono | 10px | Bold | 1.2 | Widest, UPPERCASE | Micro-badges only |
| Caption | Serif | 12px | Regular | 1.5 | +0.02em | Timestamps, metadata, fine print |

---

## Usage Rules

**When to use each style**
- **Data Hero / Eyebrow Label** — Reserved exclusively for the Home screen's points balance. This pairing is the app's single loudest typographic moment; using it elsewhere dilutes the signature.
- **Data Inline** — Use for *every* numeral a partner needs to trust or verify (points, dates, litres, booking codes) — never fall back to Body styles for these, even inline within a sentence, so numerals stay visually consistent app-wide.
- **Title Large** — One per screen, at the top. Never inside a card.
- **Title Medium/Small** — Nested groupings within a screen; Title Medium never appears without a Title Large already on screen.
- **Body Large** — Long-form reading only (Boat Rules, legal pages, checklist disclaimers). Never in dense list rows.
- **Body Medium** — The default for everything else.
- **Label styles** — Interactive affordances and status badges only. Always uppercase, always Mono, always tracked — never render a Label in the serif body face.
- **Caption** — Supplementary only, never a data point's sole representation (a timestamp is a caption; a points value is never a caption — it's Data Inline).

**Forbidden combinations**
| Forbidden | Reason |
|---|---|
| Fraunces (body face) set in uppercase + tracked | Serif faces lose legibility when tracked/uppercase — that treatment is reserved for IBM Plex Mono only |
| Data Hero used more than once per screen | Destroys the "one hero number" signature moment |
| A points/date/litres value set in Body Medium instead of Data Inline | Breaks the numeral-consistency rule — partners should always recognize "a number I can trust" by its monospace treatment |
| Label as body copy | All-caps at paragraph length reduces readability |
| Two consecutive Title styles with no content between them | False heading nesting |

**Maximum line length (measure)**
| Context | Max characters |
|---|---|
| Body Large (Boat Rules, legal) | 70 |
| Body Medium (UI) | 60 |
| Body Small | 50 |
| Caption | 55 |

Left-align only. Never justify body text.

**Minimum contrast** — inherits directly from `07-color-system.md`'s WCAG matrix; all text-color pairings there already meet or exceed AA (several meet AAA). No new contrast risk is introduced by font choice since color, not typeface, carries the contrast burden.

---

## Points-Balance & Calendar-Numeral Specifics (signature moments)

- **Home points balance:** `eyebrowLabel` ("POINTS BALANCE") directly above `dataHero` (the number itself), generous spacing below before the quick-links row (per `06-art-direction.md` Signature Moment #1 and the spacious tokens in `foundations.json`).
- **Calendar date numerals (M04):** each date cell's number renders in `dataInline`, sized down further at the component level if needed for grid density — but never swapped to the serif body face, since the rounded-square date glyph (Signature Moment #3) and the mono numeral together are what tie the calendar to the brand mark.
- **Booking Review points breakdown (M08):** the three numbers (available / this booking / balance after) all use `dataInline`, left-aligned in a simple three-row list rather than a table, so the tabular monospace figures visually line up without needing an actual `<table>`.

## Handoff
- SKILL 07 (Responsive Layout) should size the Data Hero + Eyebrow Label pairing as a fixed hero block at the top of Home, respecting the generous spacing tokens already defined.
- SKILL 08 (Component Design) — any component displaying a number (points, dates, litres) must reference `typography.dataInline` or `typography.dataHero`, never a raw font-size.
- SKILL 09 (UX Writing) — Eyebrow Label copy ("POINTS BALANCE" and any other tracked micro-labels) should be written short (1-3 words) since Mono tracking at Label sizes doesn't wrap gracefully.
