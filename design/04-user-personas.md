# Offshore Collective — Partner Mobile App Personas

**Consumed inputs:** `03-design-brief-parsed.md` (Project Overview — target users), WBS booking-rule detail, qualification-gate mechanic, Rayglass 2400 towing workflow

---

**David Kearney, 52**
- **Role:** One of 6 co-owners on a Rayglass 3000, semi-retired business owner
- **Location & usage context:** Books trips from his laptop-brain-but-phone-hands habit — opens the app at his desk on a Sunday night planning the month, then checks it quickly from the car on departure day
- **Tech comfort:** 3 — comfortable with mainstream apps (banking, email, calendar) but has zero patience for anything that feels like it's making him work to book a boat he co-owns
- **Core goal:** Lock in his preferred weekends before they're gone, without needing to remember the exact rules (points cost, caps, blackout windows) every time
- **Frustrations with current solutions:** Before this app, coordination happened over a shared spreadsheet and group texts with the other 5 partners — double-bookings and "wait, how many points do I have left" arguments were common
- **Key behaviors:**
  - Checks his points balance before deciding whether a trip is "worth it," especially for a weekend block (5 pts)
  - Books close to the 60-day window opening, since desirable weekends go fast on a first-confirmed basis
  - Reads the plain-language rule explanation when blocked, but won't dig into the full Boat Rules section unless something goes wrong
- **Quote:** "I just want to see what I've got left and grab the weekend before one of the other five does."
- **Design implication:** The points balance and next-booking status must be the first thing visible on Home (M03) with zero taps — David should never have to navigate to "find out" his balance before deciding to book.
- [Assumption: "semi-retired business owner" demographic is inferred from the target-user description "adult professionals who can afford fractional boat ownership" — not stated explicitly in the WBS]

---

**Priya Nathan, 41** — newly qualified, first-ever booking
- **Role:** New partner on a Rayglass 3000, just completed Powerboat Training NZ certification
- **Location & usage context:** Downloaded the app the day Matt told her she'd passed — opens it eagerly, expecting to book immediately, from her phone at home
- **Tech comfort:** 3 — average consumer app fluency, but has never seen this specific points/booking system before, so everything is unfamiliar on day one
- **Core goal:** Make her first booking with confidence that she understands the rules before she "wastes" points or breaks something
- **Frustrations with current solutions:** None yet with this product specifically — her anxiety is about the unfamiliar system itself: she doesn't yet know what a "weekend block" or "Christmas Window" means in this context
- **Key behaviors:**
  - Opens the app before her qualification is confirmed and finds booking locked — needs to understand *why*, not just *that* she's blocked
  - Once unlocked, is likely to read the Boat Rules section in full before her first booking, unlike a veteran partner who skips it
  - Pays close attention to the points-cost breakdown shown before confirming, double-checking the math herself
- **Quote:** "I don't want my first booking to be the one where I get something wrong in front of the other owners."
- **Design implication:** The Qualification Pending gate (M20) must explain *what* is pending and roughly what happens next (Matt confirms it), not just block silently — and the first post-unlock session should make Boat Rules (M05) easy to find, not buried, since a first-timer actively seeks it out.
- [Assumption: Priya's specific emotional state ("anxious about getting it wrong in front of other owners") is inferred from the shared 6-partner ownership structure implying social visibility — not stated in the WBS]

---

**Grant Ashworth, 63** [Edge Case]
- **Role:** Co-owner on the Rayglass 2400 (the towing-capable boat), infrequent app user
- **Location & usage context:** Uses the app rarely — maybe 3-4 times a year around his own bookings — often on a tablet at home rather than his phone, and dislikes multi-step processes
- **Tech comfort:** 2 — uses a smartphone mainly for calls, texts, and weather; finds unfamiliar app flows stressful and prefers calling Matt directly when confused
- **Core goal:** Book a towing trip to a specific destination without the extra approval step feeling like a black box or a rejection
- **Frustrations with current solutions:** Historically just called or texted Matt to arrange towing manually — the idea of "entering a destination and waiting for approval" in an app, with no phone call to confirm, is new and slightly unsettling to him
- **Key behaviors:**
  - Enters a towing destination at booking and then doesn't know what "held, pending approval" means unless it's stated in plain language
  - Is the partner most likely to change his mind on a destination after submitting — which re-triggers approval, a rule he won't intuitively expect
  - Least likely of the three personas to check the in-app Notification Center proactively — relies on push notifications firing, since he won't think to go looking
- **Quote:** "I told the app where I'm towing to — now what? Am I booked or not?"
- **Design implication:** The "held / pending approval" state (M08 → M10 towing flow) needs an unambiguous, persistent status label wherever the booking appears (Home, Booking Detail) — Grant must never have to wonder whether he's confirmed or not, and changing the destination after approval must clearly warn him it resets to pending before he does it.
- [Assumption: Grant's age/profile and preference for phone calls over app flows are inferred to satisfy the required edge-case (lower tech comfort, different context) using the towing workflow as the mechanic that most exposes an unfamiliar, ambiguous-feeling system state — not stated in the WBS]

---

## Note
Admin (Matt) and Contractor (TMP/MDC) personas are out of scope for this pass — the design pipeline is now focused exclusively on the Partner Mobile App per user instruction. IA for those surfaces remains in `01-information-architecture.md` if revisited later.
