# Offshore Collective — Structured Design Spec

**Source:** Scope & Quotation (v1.3.2026) WBS + resolved open-questions scope + competitive/trend briefs
**Prepared:** 2026-08-31

---

## Project Overview
- Product: Offshore Collective — a boat-share fractional-ownership management platform, three surfaces: Partner Mobile App (native, iOS + Android), Admin Portal (web), Contractor Mini-Portal (restricted web)
- Problem: Six co-owners share one boat under a points-based booking allocation with complex fairness rules (seasonal caps, holiday priority, Christmas Window draw); the operator (Matt) currently manages fleet scheduling, sales pipeline, contractor turnaround, and finances without a unified system
- Target users:
  - Boat-owning partners — adult professionals who can afford fractional boat ownership, moderate-to-low tech literacy assumed (premium leisure users, not tech-first), use the app occasionally (per-trip, not daily)
  - Matt (admin/operator) — single power user, high tool literacy, uses the Admin Portal daily/operationally
  - Contractors (TMP, MDC) — field workers (launch/cleaning crews), need the mini-portal to be fast and low-friction, likely used on a phone/tablet in the field
- Business goal: Replace ad hoc/manual coordination with a system that enforces booking rules automatically, gives partners self-service confidence in a shared asset, and gives Matt one prioritised view of everything needing his attention — success looks like partners booking without calling Matt, and Matt clearing his day from the Needs Your Action feed alone

## Design Direction *(stated signals only — full POV comes from alice-art-direction)*
- Stated style signals: premium NZ marine-leisure brand tone (from R2 trend brief framing); no informal/gig-economy tone — the product must read as trustworthy and premium, not a rental marketplace
- Reference apps/products: none named directly by the client in the WBS itself — GetMyBoat, The Owners App, BoatPass, SeaNet, Freedom Boat Club surfaced only through independent competitive research (R1), not client-specified, so they inform pattern decisions but are not stated creative references
- Things to avoid: marketplace-style browsing/discovery chrome (partners only ever see their own single boat — not a multi-boat marketplace); AI chatbot UI (explicitly removed from scope by client); anything that makes the points/fairness system feel hidden or de-emphasized (this is the product's core trust mechanic)
- ➡️ Hand off to `alice-art-direction` (SKILL AD) to define the actual visual direction

## Scope
- Key screens/flows (full detail in `01-information-architecture.md`):
  - **Mobile:** Login, Home/Dashboard, Booking Calendar → Review & Confirm, Standby Claim, Boat Rules, Notification Center, Profile, Pre-Departure/Post-Use Checklists, Christmas Window Display, Cancel Booking (three variants per C.30), Qualification Pending gate
  - **Admin Portal:** Needs Your Action Feed, Tomorrow's Automations, Fleet Calendar (+ Priority Queue), Boats Registry, Pipeline Kanban Board, Partner Directory, Manual Confirmation Center, Content Management, Business Milestones Panel, Reports/Export
  - **Contractor Mini-Portal (6 screens):** Login, Job Queue/Fleet Calendar (restricted), Job Detail, Mark Job Complete (MDC), Confirm Launch (TMP)
- Platform: iOS + Android (mobile, portrait-only, cross-platform native feel per platform conventions) + Web (Admin Portal, desktop-first) + Web (Contractor Mini-Portal, restricted)
- Priority: Full product — not an MVP trim. The WBS is a fixed, fully-quoted scope (15–17 week timeline, milestone payment structure) with two sections (Business Milestones, Reports/Export) resolved by independent analysis rather than left as gaps

## Open Questions
- Should the mobile app support biometric login (Face ID/fingerprint), or credentials-only as currently scoped? (Not specified in WBS)
- ~~Are there existing Offshore Collective brand assets?~~ **Resolved:** Yes — `icon-navy.png` and `lockup-navy.png` found in the project root (navy + brass/gold mark, monospace wordmark). See `06-art-direction.md`, which is built directly from these assets.
- The 19 original client open-questions (Business Milestones, Reports/Export, reserve balance) were resolved by InApps' own analysis, not confirmed by Matt — should visual design proceed now on those assumptions, or wait for sign-off? *(Recommended: proceed — see `02-resolved-open-questions-scope.md`)*
- Is there a target device/screen-size list for the Admin Portal (e.g. minimum supported browser width), given it's described as "web, desktop-first" but no explicit breakpoint list exists in the WBS?
