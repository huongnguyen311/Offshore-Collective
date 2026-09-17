# Competitive Brief — Boat-Share / Fractional Ownership Platform / Dual Surface (Mobile + Admin) / 2026

**Products researched:** The Owners App, BoatPass Co-Ownership, SeaNet, GetMyBoat, Boatsetter, Freedom Boat Club, generic modern CRM+Kanban+Calendar admin pattern (fleet-ops SaaS), timeshare points-exchange platforms (RCI/Interval-style, generic)
**Searches run:**
1. "best boat club fractional ownership apps 2026 booking"
2. "Freedom Boat Club Boatsetter GetMyBoat app UX design review"
3. "points based timeshare exchange app booking UX design"
4. "fleet management operations dashboard CRM Kanban calendar SaaS design"

> **Data-gap note:** Public UX teardowns/screenshots for the direct fractional-ownership competitors (The Owners App, BoatPass, SeaNet) were not found — search returned functional/marketing descriptions, not design case studies. Per-product notes below state only what's actually evidenced; navigation/visual specifics that aren't publicly documented are marked "not evidenced" rather than guessed.

---

## The Owners App — owners.gr
**Type:** Direct competitor
**Market position:** Niche leader (fractional boat ownership specifically)
**Platform:** Mobile (not evidenced which OS)
**Target user:** Co-owners of a shared boat

### Core UX Patterns
- **Points-based reservation system with seasonal weighting**: peak season (Jul–Aug) = 3 pts/day, shoulder season = 2 pts/day, off-season = 1 pt/day — points cost varies by calendar demand, not a flat rate.
- **Guaranteed-use weeks**: each owner gets 1 guaranteed week in July and 1 in August regardless of points, on top of the points pool.
- Trip booking, maintenance tracking, and co-owner communication live in one app.

### Strengths
- Seasonal point-weighting directly solves the "everyone wants August" fairness problem — more sophisticated than Offshore Collective's flat per-booking-type cost (1/5/7/14 pts regardless of calendar date).
- Guaranteed weeks remove the anxiety of "what if I never get summer" — a trust-building mechanic absent from Offshore Collective's first-confirmed-only model.

### Weaknesses / Gaps
- Not evidenced (no review data found).

---

## BoatPass Co-Ownership — boatpassclub.com
**Type:** Direct competitor
**Market position:** Challenger, larger scale (3,000+ yachts, 50+ destinations — multi-boat marketplace, not single-syndicate)
**Platform:** Not evidenced
**Target user:** Fractional yacht co-owners across a large shared fleet

### Core UX Patterns
- **Rules-based scheduling** with online booking up to a year in advance (vs. Offshore Collective's 60-day window).
- Marketplace model — many boats, many owners, cross-boat access — structurally different from Offshore Collective's one-boat-six-partners model.

### Strengths
- Long booking horizon (1 year) gives owners more trip-planning certainty than a 60-day window.

### Weaknesses / Gaps
- Not evidenced. Marketplace complexity (many boats) is a different problem than Offshore Collective's single-syndicate model, so patterns don't transfer 1:1.

---

## SeaNet — apps.apple.com
**Type:** Direct competitor
**Market position:** Niche (luxury fractional programs)
**Platform:** iOS (confirmed — has an App Store listing)
**Target user:** Owners in premium fractional boat programs

### Core UX Patterns
- Shows **yacht status** + **available reservation days** + lets owners **place new reservations** in one view — i.e., availability and booking are shown together rather than as separate steps.

### Strengths
- Combining "is it available" and "book it now" into one glanceable view reduces the steps Offshore Collective currently splits across Booking Calendar (M04) → Booking Review (M08).

### Weaknesses / Gaps
- Not evidenced.

---

## GetMyBoat — apps.apple.com
**Type:** Indirect competitor (rental marketplace, not fractional ownership) / design reference
**Market position:** Leader in boat rental — 3.8M downloads, 4.9★
**Platform:** iOS + Android
**Target user:** One-off renters + boat owners renting out

### Strengths
- Highest rating + largest download base of anything found in this search — strong signal of broad usability at scale.
- In-app itinerary + captain-led service booking shows the category can support layered, multi-step bookings without users churning.

### Weaknesses / Gaps
- Not evidenced beyond ratings. Different business model (marketplace rental vs. fixed 6-partner syndicate) limits pattern transfer — useful mainly as a "this category can be built cleanly at scale" reference.

---

## Boatsetter — apps.apple.com
**Type:** Indirect competitor / worst-in-class reference (by comparison, not confirmed complaints)
**Market position:** Smaller than GetMyBoat by downloads (1.2M vs 3.8M) despite an equal-or-higher star rating (5.0★ vs 4.9★) — the rating/reach gap itself is the signal
**Platform:** iOS + Android
**Target user:** One-off renters

### Weaknesses / Gaps
- No specific UX complaints were evidenced in search results — flagging this as a genuine data gap rather than asserting weaknesses that weren't found. Downgrading to "comparison reference" rather than a confirmed worst-in-class pick.

---

## Freedom Boat Club — freedomboatclub.com
**Type:** Indirect competitor (unlimited-access club model, not fractional ownership or points)
**Market position:** Market leader in the boat-club category generally
**Platform:** Not evidenced (marketing site only)
**Target user:** Members wanting boat access without ownership

### Core UX Patterns
- Unlimited-use membership model (no points/allocation system at all) — the opposite mechanic to Offshore Collective's constrained-points model.

### Strengths / Weaknesses
- Not evidenced (marketing content only, no product UX data found).

---

## Fleet-Ops Admin Dashboard Pattern (generic SaaS reference class, not a single product)
**Type:** Design reference (category pattern, not a named competitor)
**Market position:** N/A — represents the common shape of modern fleet/CRM/ops SaaS dashboards
**Platform:** Web

### Core UX Patterns
- Kanban board for stage-based workflows (matches Offshore Collective's 7-stage Pipeline board directly).
- Calendar as the central command surface for fleet/ops managers (matches Offshore Collective's Fleet Calendar directly).
- Modern SaaS dashboards commonly ship Overview / CRM / Kanban / Calendar / Finance as separate first-class sections rather than cramming them into one view — validates Offshore Collective's sidebar-per-module IA decision already made.

---

## Timeshare Points-Exchange Platforms (generic reference class — RCI/Interval-style)
**Type:** Design reference (points-based booking mechanic precedent)
**Market position:** N/A — established category (decades-old points-exchange model)
**Platform:** Web + mobile

### Core UX Patterns
- Points valued by **desirability of destination/unit size**, not a flat rate — same seasonal-weighting idea as The Owners App above; this is a well-established pattern in points-based accommodation booking, not a one-off.
- Users can save filter/search preferences for repeat searches.

---

## Pattern Extraction

### Industry-Standard Patterns (must-have)
| Pattern | Appears in | Why it's standard |
|---|---|---|
| Points/allocation balance shown before and during booking | The Owners App, SeaNet, timeshare exchanges | Users need to see cost-vs-balance before committing — Offshore Collective already has this at M08, confirmed as correct |
| Calendar as the primary booking surface | SeaNet, GetMyBoat, fleet-ops dashboards generally | Universal for anything date-based — Offshore Collective's M04/A04 calendars are on-pattern |
| Kanban-style stage board for sales/ops pipelines | Fleet-ops SaaS reference class | Standard for any admin tool tracking leads/records through stages — validates A06 Pipeline Board as-designed |

### Differentiator Patterns (choose to use or skip)
| Pattern | Used by | Competitive advantage |
|---|---|---|
| Seasonal/demand-weighted point cost (not flat rate) | The Owners App, timeshare exchanges | Offshore Collective currently uses a flat cost per booking type (1/5/7/11 pts) regardless of calendar demand — already locked into the WBS, not a change to propose, but worth flagging as a *future* differentiator if the client revisits pricing logic post-launch |
| Guaranteed-use weeks on top of the points pool | The Owners App | Offshore Collective has no guaranteed-week concept — again, not in current scope, but a retention-relevant idea worth noting for a future release |
| Combined availability+booking view (see status and book in one screen) | SeaNet | Directly actionable now: consider whether M04 (Calendar) and M08 (Booking Review) can be tightened into fewer steps — flag to alice-user-flow |

### Anti-Patterns to Avoid
| Anti-pattern | Found in | Why it's bad |
|---|---|---|
| Marketplace-scale complexity applied to a small fixed syndicate | BoatPass (3,000+ yachts, cross-boat browsing) | Offshore Collective is 6 partners per boat, not a marketplace — resist any temptation to add boat-browsing/discovery UI; the single-boat-per-partner-group model should stay simple, not marketplace-shaped |
| No visible points/allocation mechanic at all | Freedom Boat Club (unlimited-access model) | Confirms Offshore Collective should NOT hide or de-emphasize the points balance — it's the club's core fairness mechanic, unlike unlimited-access competitors, and should stay prominent on the Home dashboard (already planned at M03) |

---

## Gap Analysis

### Gap 1 — Flat-rate points cost misses demand fairness
**What's missing:** None of Offshore Collective's competitors with visible points systems (The Owners App, timeshare exchanges) use a flat rate — they weight by season/demand. Offshore Collective's flat 1/5/7/14-point cost (already fixed in the WBS) is simpler to build but doesn't solve the "everyone wants the same weekend" problem the way competitors do.
**Evidence:** The Owners App's explicit 3/2/1-point seasonal tiers; general timeshare-exchange practice of desirability-based point values.
**Opportunity:** Not a scope change now — this is fixed in the client's WBS — but worth a one-line note to Matt as a v2 consideration, since Offshore Collective's own Boat Rules content already explains long-weekend caps and holiday fairness rules as a *partial* answer to the same fairness problem via caps rather than pricing.

### Gap 2 — Availability and booking as two separate screens
**What's missing:** SeaNet shows availability and lets the owner book from the same view; Offshore Collective's IA currently splits this into M04 (Calendar, select dates) → M08 (Review & Confirm, see cost). This is a reasonable and common pattern (also matches GetMyBoat-style booking flows), so it's not a gap to fix, but confirms M08 should stay a lightweight single step, not a multi-page wizard, to keep the two-screen flow feeling like one continuous action.
**Opportunity:** Carry this into alice-user-flow and alice-screen-design as an explicit constraint: M08 must load pre-populated and require only a single confirm tap, no additional data entry.

### Gap 3 — No competitor evidence exists for the *admin/operator* side of fractional ownership specifically
**What's missing:** All direct fractional-ownership competitors found are partner/owner-facing apps; none publish anything about their internal ops tooling.
**Evidence:** Zero admin-side results for The Owners App, BoatPass, or SeaNet.
**Opportunity:** Offshore Collective's Admin Portal (Needs Your Action feed, Fleet Calendar with urgency flags, contractor coordination) has no direct fractional-ownership precedent to copy or avoid — this is genuinely novel ground in this niche. Fall back on the general fleet-ops/CRM SaaS pattern class (Kanban + calendar + alert feed) validated above, rather than any fractional-ownership-specific competitor.

---

## Top 3 Design Opportunities
1. **Keep the points balance emotionally central, not buried** — unlike Freedom Boat Club's unlimited-access model, Offshore Collective's whole value proposition runs on a scarce, fair allocation system; the Home dashboard and every booking step should treat the points balance as a first-class, always-visible element, not a settings-page number.
2. **Make booking feel like one motion, not a form** — following SeaNet's combined availability/booking pattern, design M04→M08 to feel like a single continuous selection-to-confirm gesture, even though they're two screens.
3. **Own the "small syndicate, not a marketplace" simplicity** — deliberately avoid BoatPass-style browsing/discovery chrome (search, filters across many boats); Offshore Collective partners only ever see their own boat, so the IA should stay flatter and calmer than a marketplace app.

## Visual Benchmark
- Most design-forward reference found: GetMyBoat (highest combined rating + scale in this category) — but detailed visual specifics weren't evidenced in search results, so this is a directional signal (clean, trustworthy, scales well) rather than a literal style reference.
- Reference for aesthetic direction: **No** — insufficient visual evidence found for any direct fractional-ownership competitor. Aesthetic direction should be driven by SKILL AD (Art Direction) using the client's own premium-NZ-marine-leisure brand context, not by copying a competitor's look.

## Recommended Differentiation Angle
Offshore Collective's competitive edge isn't a new booking mechanic — it's **trustworthy simplicity for a small, fixed group of co-owners**, in a category (BoatPass, timeshare exchanges) that mostly builds for marketplace-scale complexity. The differentiation angle is: *the calmest, most transparent points-and-calendar experience in the category, with zero marketplace chrome* — every screen should assume the partner already knows their boat, their 5 co-owners, and their points balance, and get them to "book or don't" in the fewest possible steps, while the Admin Portal (a genuinely uncharted space competitively) earns its complexity budget by being the one surface that's allowed to be data-dense.

---

Sources:
- [Top 10 Best Boat Club Software of 2026](https://wifitalents.com/best/boat-club-software/)
- [Fractional Boats Ownership — The Owners App](https://owners.gr/how-the-owners-app-makes-boat-sharing-easy/)
- [BoatPass Co-Ownership](https://boatpassclub.com/co-ownership)
- [Freedom Boat Club — The Boat Sharing Economy](https://www.freedomboatclub.com/learning-center/the-boat-sharing-economy--exciting-new-ways-to-get-on-the-water-)
- [SeaNet — App Store](https://apps.apple.com/us/app/seanet/id1536226905)
- [GetMyBoat — App Store](https://apps.apple.com/us/app/getmyboat/id673121605)
- [Top Boat Rental Apps 2025: Boatsetter, Click&Boat, GetMyBoat](https://aceplace.com/journal/top-boat-rental-apps-2025-boatsetter-clickandboat-getmyboat)
- [Boatsetter vs. GetmyBoat vs. Click&boat](https://www.boat-alert.com/blog/boatsetter-vs-getmyboat-vs-clickandboat/)
- [Timeshare Exchange: Points-Based vs. Deeded Weeks](https://www.timesharesonly.com/blog/timeshare-points-vs-weeks/)
- [How Do Timeshare Points Work?](https://www.timesharesonly.com/blog/how-do-timeshare-points-work/)
- [Redesigning a Complex Fleet Management Dashboard](https://medium.com/@sav.io/redesigning-a-complex-fleet-management-dashboard-ac2efc91868b)
- [22 Best SaaS Dashboard Templates & UI Design Examples 2026](https://adminlte.io/blog/saas-admin-dashboard-templates/)
