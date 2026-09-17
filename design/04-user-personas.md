# Offshore Collective — Partner Mobile App Personas

**Consumed inputs:** `03-design-brief-parsed.md` (Project Overview, target users), SOW booking-rule detail, qualification-gate mechanic, boat-ready notification behaviour (C.31)

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
- **Role:** Co-owner on Halcyon, infrequent app user
- **Location & usage context:** Uses the app rarely, maybe 3 or 4 times a year around his own bookings, often on a tablet at home rather than his phone, and dislikes multi-step processes
- **Tech comfort:** 2 — uses a smartphone mainly for calls, texts, and weather; finds unfamiliar app flows stressful and prefers calling Matt directly when confused
- **Core goal:** Turn up on the day he booked and find the boat ready, without having to work out from the app whether anything is still outstanding
- **Frustrations with current solutions:** Historically just called or texted Matt to check the boat was ready. Being told by an app, once, in a notification he may never see, is new and slightly unsettling to him
- **Key behaviors:**
  - Has push notifications switched off at OS level and does not know it, so the "boat is ready" push never reaches him
  - Least likely of the three personas to open the Notification Center proactively, since he won't think to go looking
  - Books rarely enough that he re-learns the rules each time, and is the partner most likely to be surprised by a rule that behaves differently from the one he remembers, such as an unclaimed weekend forfeiting its points on cancellation when he expects the 48-hour refund
- **Quote:** "Is she in the water or not? I'd rather just ring someone."
- **Design implication:** this persona is the whole argument for SOW C.31. A status that exists only as a push notification does not exist for Grant, so the boat-ready state has to be written onto the Dashboard boat card where he will see it next time he opens the app. The same logic drives the cancellation dialogs: each one states its own points outcome in full rather than assuming the partner remembers which rule applies to which booking type
- [Assumption: Grant's age/profile and preference for phone calls over app flows are inferred to satisfy the required edge case (lower tech comfort, different context), using notification delivery as the mechanic that most exposes an unfamiliar, ambiguous-feeling system state. Not stated in the SOW]

---

## Note
Admin (Matt) and Contractor (TMP/MDC) personas are out of scope for this pass — the design pipeline is now focused exclusively on the Partner Mobile App per user instruction. IA for those surfaces remains in `01-information-architecture.md` if revisited later.
