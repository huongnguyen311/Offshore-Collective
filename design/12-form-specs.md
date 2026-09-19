# Offshore Collective — Form Specs (Partner Mobile App)

**Consumed inputs:** `05-user-flows.md` (FLOW-CHECKLIST-01, FLOW-STANDBY-01), `11-component-specs.md` (StatusBadge, typography.dataInline)
**Note on sequencing:** SKILL 09 (UX Writing) runs after this skill in the pipeline order, so error/help copy below is drafted as reasonable placeholder text, not final — flag to SKILL 09 to confirm/refine wording rather than treating it as locked.

---

# Form 1 — Pre-Departure Checklist (M15)

## 1. Field Inventory

| Field | Label | Input type | Required? | Default | Placeholder | Help text |
|---|---|---|---|---|---|---|
| tankFull | "Is the tank full?" | segmented yes/no toggle | Yes | — | — | — |
| fuelLitres | "Litres reading" | number (dataInline styled) | Conditional — required only if `tankFull = No` | — | "0" | "Read directly from the fuel gauge" |
| fuelPhoto | "Photo of fuel gauge" | photo capture/upload | Asked for always (even when tank is full), but **never blocking** — see the waiver below | — | — | "Uploaded at full resolution, not compressed" |
| notFullReason | "Why isn't it full?" | short text (single line) | Conditional — required only if `tankFull = No` | — | "e.g. used half a tank on the way out" | — |
| hasDamage | "Any damage?" | segmented yes/no toggle | Yes | — | — | — |
| damageDescription | "Describe the damage" | multi-line text | Conditional — required only if `hasDamage = Yes` | — | "What happened and where" | — |
| damagePhoto | "Photo of the damage" | photo capture/upload | Conditional — required only if `hasDamage = Yes` | — | — | "Uploaded at full resolution, not compressed" |
| estimatedReturnTime | "Estimated return time" | three linked dropdowns (hour / :00 or :30 / AM-PM) | Yes | 4:00 PM | — | "An estimate only, shared with us to help plan your turnaround" |

## 2. Validation Rules

| Field | Rule | Trigger | Error message | Clears when |
|---|---|---|---|---|
| tankFull | must be answered | onSubmit (it's a toggle, not left blank in practice, but the group is checked on submit) | "Let us know if the tank is full" | either option selected |
| fuelLitres | required, numeric, ≥ 0 when tankFull = No | onBlur | "Enter the litres reading from the gauge" | valid number entered |
| fuelPhoto | soft, never blocks submission | onSubmit | "No fuel gauge photo. If you have already left the boat, submit without it, we will let Matt know." (warning tone, not error) | a photo is attached |
| notFullReason | required, non-empty when tankFull = No | onBlur | "Tell us why the tank isn't full" | any text entered |
| hasDamage | must be answered | onSubmit | "Let us know if there's any damage" | either option selected |
| damageDescription | required, non-empty when hasDamage = Yes | onBlur | "Describe the damage" | any text entered |
| damagePhoto | required when hasDamage = Yes | onSubmit | "Add a photo of the damage" | a photo is attached |
| estimatedReturnTime | must be set (all three parts) | onSubmit | "Set the time you expect to be back" | all three dropdowns have a value |

**On the 20-litre tolerance:** this is a *system-side* check, not a field-level validation the partner sees — a small discrepancy between the reported litres and the expected reading never blocks submission or shows an error to the partner. It only ever triggers a Needs Your Action item for Matt in the Admin Portal, silently. Do not add any partner-visible tolerance messaging to this form.

**Confirmed by the client 2026-09-18, and the reason matters:** partners are expected to return the boat full every time, so publishing the threshold invites them to treat 20 litres short as an acceptable place to leave the tank. The rule above was already correct, but the screens were violating it: `screens-preview.html` carried "Up to 20 litres: no action. Over 20 litres: Admin notified." under the litres field on all four checklist screens, removed the same day. The `fuelLitres` help text ("Read directly from the fuel gauge") is the only copy that belongs there. C.19's calculation is untouched, this is copy-only.


## 2b. Fuel Gauge Photo Waiver

Confirmed by Matt 2026-09-18, applies to both checklists (SOW A.9 / A.11):

> If a partner submits either checklist after they have already left the boat's location, so a fuel gauge photo can no longer be taken, the photo requirement is waived for that submission, but the system flags it as "submitted without fuel photo" in the Needs Your Action feed. All other checklist fields remain mandatory and the turnaround workflow proceeds normally regardless of the missing photo.

**How the form honours that, and why it is shaped this way.** The platform has no location awareness: GPS sits outside the InApps system, so nothing in the app can verify that the partner has actually left the boat. The waiver therefore cannot be a condition the form evaluates. It is the absence of the photo that carries it:

1. `fuelPhoto` is still asked for, in the same position, with the same prominence. Nothing about the field suggests it is optional, because in the normal case it is not optional in spirit, only in enforcement.
2. It carries no `*` and no `aria-required`, since a required marker on a field that does not block is a lie to both sighted and assistive users.
3. Submitting with no photo attached shows the soft notice in §2 in the warning colour, not the error colour, and lets the submit through on the same tap. There is no second confirm tap: the notice states the consequence, and the consequence is already true by the time it is read.
4. The submission record carries a `fuelPhotoAbsent` flag, which raises a reminder-only "submitted without fuel photo" item in the Needs Your Action feed (SOW B.1).
5. A fuel shortfall flagged on a submission with no photo carries the litres and reason only. The feed item cannot show evidence that was never captured.

**Two consequences worth stating plainly for engineering and for the client.** Any partner can now omit the photo, not only one who has genuinely left the boat, and the feed flag is the only control on that. And a partner who submits without a photo gets no chance to change their mind, since the notice arrives with the submit rather than before it. Both follow directly from having no location signal to gate on. If either matters to the client, the fix is a declared reason on the field rather than more validation.

## 3. Input Masks & Formatting
- `fuelLitres`: plain integer or one-decimal number, no thousands separator (litres values are always small, e.g. "45" or "45.5"), styled with `typography.dataInline` per `09-typography-system.md`'s numeral-consistency rule.
- `estimatedReturnTime`: no free-text entry and therefore no mask. Three dropdowns (hour, :00 or :30, AM/PM) rather than a text field or a native time picker, confirmed 2026-09-14. Minutes are deliberately limited to two options: this is a planning estimate for a contractor, and false precision invites a partner to treat it as a commitment.

## 4. Field States
Per `alice-component-design` conventions: empty → focus → filled → valid → error → disabled.
- `fuelLitres` and `notFullReason`/`damageDescription` fields are **hidden entirely**, not just disabled, until their trigger condition (`tankFull = No` / `hasDamage = Yes`) is met — this is a progressive-disclosure pattern, not a grayed-out disabled state.
- `fuelPhoto` is **not** one of them, and must sit outside that disclosed block: it is asked for whether the tank is full or not, so placing it inside the `tankFull = No` group silently drops it every time the answer is Yes. Called out because that is exactly what the first build of the screen did, and it recurred in the two redline screens until 2026-09-18.
- Photo fields show a filled thumbnail once attached; tapping the thumbnail allows retake/replace before submit.
- `estimatedReturnTime` is never hidden or conditional. It defaults to a plausible value rather than an empty state, because SOW A.9 surfaces it to Marine Detailing Co the moment the checklist is submitted, and a blank planning reference is worse for them than an approximate one.

## 5. Form-Level Behavior
- **Submit enable rule:** the Submit button is always visible but tapping it while any required field (per current conditional state) is incomplete triggers validation and blocks submission — not a permanently-disabled button, since which fields are "required" changes dynamically as tankFull/hasDamage toggle.
- **Validation strategy:** text/number fields validate onBlur after first interaction; the two yes/no toggles and required photos validate onSubmit (since there's no natural "blur" for a toggle or an attachment).
- **Submit type:** blocking for every field except `fuelPhoto` — the checklist is not submitted until all currently-required fields pass, and a missing fuel gauge photo is not one of them (see the waiver below).
- **Success handling:** on successful submit, photo uploads at full resolution (no client-side compression, per WBS), the checklist record is created, any fuel shortfall (beyond 20L tolerance) or damage report is simultaneously flagged into Matt's Needs Your Action feed, and the user returns to Upcoming Booking Detail (M09) — no approval gate blocks departure (per FLOW-CHECKLIST-01).
- **Failure handling (upload failure):** if a photo fails to upload (network failure mid-submit), preserve all entered text-field values and the locally-held photo, show an inline retry affordance on the specific photo field rather than clearing the form.
- **Autosave:** not specified in the WBS — recommend a lightweight local draft (device-only, not synced) so backgrounding the app mid-checklist doesn't lose entered data, but this is a recommendation, not a confirmed requirement; flag as an open question if precision matters to engineering.

## 6. Accessibility
- Every field has an associated `<label>` (`for`/`id`); yes/no toggles use `role="radiogroup"` with each option as `role="radio"`.
- Errors: `aria-describedby` points to the inline error text; `aria-invalid="true"` set on the field.
- On submit-with-errors: focus moves to the first invalid/incomplete field in visual order (tankFull → fuelLitres/notFullReason → hasDamage → damageDescription/damagePhoto → estimatedReturnTime). `fuelPhoto` is skipped as a focus target: it is soft, so it is never what is holding the submit back.
- Errors announced via `role="alert"` live region.
- Required fields (fixed: hasDamage, tankFull; conditional: the rest) are marked with both a visible `*` and `aria-required="true"`. `fuelPhoto` carries neither, because it does not block submission; its soft notice is announced through the same `role="alert"` region as the errors.
- Photo fields: alt text/label describes purpose ("Fuel gauge photo", "Damage photo"), not filename.

## 7. Edge Cases
- Network failure mid-submit → preserve all entered values + attached photos locally, show retry, never silently drop data.
- Double-submit prevention: disable the Submit button and show a spinner on first tap until the request resolves.
- Partner backgrounds the app mid-checklist → per Autosave note above, recommend preserving the draft locally.
- Very long `notFullReason`/`damageDescription` text: no hard character cap specified in the WBS — recommend a generous soft cap (e.g. 500 characters) with no error before that point; flag to SKILL 09 for exact copy if a cap is added.
- Unsaved-changes guard: if the partner navigates away mid-checklist with any field filled in, confirm before discarding (standard pattern, not explicitly required by WBS but a sensible default given the stakes of a skipped pre-departure check).

---

# Form 2 — Post-Use Checklist (M16)

Identical field inventory, validation, masks, states, and accessibility to Form 1 above, with these deltas only (per FLOW-CHECKLIST-01 Case 6):

| Difference | Detail |
|---|---|
| Additional field | `engineHours` — "Reading from the hour meter", number with one decimal, **required**. Validation: required and numeric, checked onSubmit, error "Enter the reading from the hour meter", clears when a number is entered. Mandatory rather than optional because it feeds the Engine Servicing Registry (A.11/B.14) per FLOW-CHECKLIST-01 Case 6. Was missing from this table until 2026-09-17 even though the flow doc and the screen both carried it. |
| Additional field | `extraPhotos` — optional, multiple photo attachments beyond the required fuel/damage photos. No validation rule (always optional, no minimum/maximum count specified in WBS). |
| Heading/footer copy | A specific heading and footer disclaimer about photo usage is shown on this screen — not present on the Pre-Departure version. Final copy in `14-ux-writing.md` § "Post-Use Checklist Photo Disclaimer (M16)". |
| Success handling differs | Submitting this checklist triggers the turnaround process (Admin Portal territory) in addition to the same flagging behavior as Form 1. From 2026-09-19 it also has a partner-facing result of its own: a submitted state saying the trip is closed and the team has been told, rather than a silent return to M03. When the tank was reported as not full, that state carries the A.11 acknowledgement ("Fuel shortfall received. If anything needs sorting, we'll be in touch."). It shows on every not-full submission, not only past the 20-litre tolerance: a line that appeared only past the threshold would let a partner read the figure off its presence, which C.19 forbids. For the same reason the contact promise is conditional, since a partner a few litres short will not be contacted at all. |
| Additional field | `engineHours` — "Reading from the hour meter", number with one decimal, **mandatory**. Validation: required, numeric, must be greater than or equal to the last logged reading for that boat, onBlur, "Enter the hour meter reading" / "That reading is lower than the last one recorded. Check the meter and try again." Feeds the Engine Servicing Registry (SOW B.14), which is why a silently wrong value is costly: it drives the service-due calculation in C.17. |
| Field order | Engine Hours sits between the Damage card and the optional Photos card, so the two required cards stay together and the optional one is last. |
| Fuel photo waiver applies unchanged | The §2b waiver carries over as written, with one addition specific to this form: the turnaround workflow starts as normal when the fuel gauge photo is absent. A missing photo never holds the hand-off to MDC and TMP. |
| No return-time field | `estimatedReturnTime` is a pre-departure-only field. By the time this form is filled in, the partner is back, so the estimate has no remaining purpose. |

---

# Form 3 — Standby Claim (M28)

The shortest form in the app, and the only one that takes no points. SOW A.19 / C.28.

## 1. Field Inventory

| Field | Label | Input type | Required? | Default | Placeholder | Help text |
|---|---|---|---|---|---|---|
| estimatedDepartureTime | "Estimated departure time" | three linked dropdowns (hour / :00 or :30 / AM-PM) | Yes | 10:00 AM | — | "We’ll notify the team the moment you claim, so give them as much notice as you can" |

## 2. Validation Rules

| Field | Rule | Trigger | Error message | Clears when |
|---|---|---|---|---|
| estimatedDepartureTime | must be set (all three parts) | onSubmit | "Set the time you expect to head out" | all three dropdowns have a value |

No other validation. There is no points check to run, because the claim costs nothing, so the hard block on a negative balance (SOW C.4) does not apply to this form at all.

**Naming rule for this screen:** partner-facing copy says "the team", never "Tamaki Marine Park". The partner has no relationship with the contractor and no way to act on the name. The underlying behaviour is unchanged: SOW C.28 still fires an SMS and an email to Tamaki Marine Park specifically, and the spec prose below names them because a developer has to know which contractor is wired up.

## 3. Input Masks & Formatting
None applicable, the field is three dropdowns. Same trio as `estimatedReturnTime` on Form 1, so the two share one component.

## 4. Field States
Standard single-group states (empty → focus → filled → valid → error). No conditional sub-fields.

## 5. Form-Level Behavior
- **Submit enable rule:** Claim is enabled on arrival, since the one field is pre-filled with a default.
- **Submit type:** blocking. Unlike the other forms, this one races other partners: standby is first-confirmed, so the button must enter a pending state on tap and must not be re-tappable.
- **Success handling:** routes to the booking detail in its Claimed state, reading "Confirmed. The relevant team has been notified and will have Halcyon ready in the water for you." The partner is told someone has been notified, without naming the contractor, but the underlying behaviour is still SOW C.28: an SMS and an email fire immediately rather than batching into the nightly contractor email.
- **Failure handling:** two distinct failures, which must not share one message. A server error says nothing was taken from the partner's points. A lost race says another partner claimed it first. Only the second is final.
- **No offline queueing:** see `13-state-gallery.md`. A same-day claim submitted late could page a contractor about a day that has already gone.

## 6. Accessibility
- The three dropdowns share one group label, so "estimated departure time" is not announced three times.
- Error: `aria-describedby` → inline error, `aria-invalid="true"`.
- "0 points" is announced as a value, never skipped as an empty field. See `19-a11y-spec.md`, M28.

## 7. Edge Cases
- Another partner claims the day while this form is open → the claim button is replaced by an inline notice before the partner can tap it, rather than failing after the tap. See `13-state-gallery.md`, M28 "Taken while open".
- The partner sets a departure time that has already passed → allowed, and deliberately so. Someone claiming at 2pm for a 10am start is telling Tamaki Marine Park they want the boat now, and blocking that would be pedantic. The value is a planning reference, not a schedule.
- The day lapses at midnight while the form is open → same treatment as losing the race.
- The partner's points balance is zero or negative → the claim still proceeds. A standby claim is free, and SOW A.19 sets no balance condition on it.

---

# Form 4 — Edit Profile (in place on M07)

This form has no screen of its own. Editing happens on Profile (M07): name sits at the top of the identity card, phone and email are rows inside it, and an Edit control on that card's top right turns the editable one into an input where it already sits. Same order, same labels, same position, nothing moves. Cancel and Save appear beneath the fields, and the partner never leaves M07.

**One field, not three, from 2026-09-19.** Email stopped being partner-editable on 2026-09-18 and phone followed on 2026-09-19, leaving `name` as the only field on this form. B.19 makes Matt the maintainer of both a partner's phone and their email on the shareholder record, and A.16 puts a self-service change out of scope, so an input for either would offer something the platform will not accept. Both stay on screen as rows that render identically in view and edit mode, the same treatment Qualification and the Secondary operator already get. A line beneath them reading "Your phone and email are held on your shareholder record. Contact us to change them." was added on 2026-09-19 and removed the same day at the client's direction: the rows now carry no explanation at all. Worth knowing when this is built, because the consequence is real: nothing in the app tells a partner how to get either value changed. The support contact in the M07 footer (A.17) is the route, it is simply not labelled as one.

**The mode stays, the promise shrinks (decided 2026-09-19).** With one field left it was worth asking whether Edit / Cancel / Save is too much machinery for a name, and whether the name should simply be tappable and save itself on blur. It should not: a name sits on the shareholder record, and a silent save leaves a partner who mistyped with nothing to catch it with, which is the wrong trade for a value changed once in three years. What was costing the partner was never the two taps, it was the card-level Edit control promising to edit the card. So the control was narrowed to the name: first by label ("Edit name"), then, when the label itself came off later the same day, by position. A pencil at a card's top right edits the card; a pencil sitting against a name edits that name. The scope is now carried by where the control is, not by what it says, and its accessible name still spells it out for anyone who cannot see the position.

## 1. Field Inventory

| Field | Label | Input type | Required? | Default | Placeholder | Help text |
|---|---|---|---|---|---|---|
| name | "Name" | short text | Yes | current value | — | — |
| ~~contactPhone~~ | "Phone" | **read-only row, not a field** (from 2026-09-19) | n/a | current value | — | shares the line below with email |
| ~~contactEmail~~ | "Email" | **read-only row, not a field** | n/a | current value | — | none. The explanatory line was removed 2026-09-19 (see above) |

SOW A.16 scopes this as "view/edit personal profile info" without enumerating the fields beyond name and contact. The editable set is now name alone: B.19 gives Matt the job of maintaining a partner's phone and email on the shareholder record, and a second, unlogged path to the same values from the partner's own screen is exactly what that row was written to prevent. Phone was the last field inconsistent with that boundary and was closed on 2026-09-19. The secondary operator is not part of this form either, see the note at the end of this document.

## 2. Validation Rules

| Field | Rule | Trigger | Error message | Clears when |
|---|---|---|---|---|
| name | required, non-empty | onBlur | "Enter your name" | any text entered |
| ~~contactPhone~~ | no validation; the field no longer exists (2026-09-19) | — | — | — |
| ~~contactEmail~~ | no validation; the field no longer exists. Nothing on this form validates an email address | — | — | — |

One rule, on one field. A non-empty name is the only thing this form can refuse.

## 3. Input Masks & Formatting
- None. `name` is free text with no mask.
- ~~`contactPhone` mask and mono-while-editing rules~~: dropped 2026-09-19 with the field. The phone number still renders in IBM Plex Mono as a row, per the Design Foundation, it is simply never in an input on this screen.

## 4. Field States
Standard text-field states (empty/focus/filled/valid/error/disabled). `name` pre-fills with the current saved value, so it is never truly empty on entering edit mode unless the partner clears it. Phone and email are not fields and take no field states: both are rows in both modes. `name` keeps its display treatment while editable: heading weight and size, at the top of the identity card, because the point of editing in place is that the value does not jump somewhere else to be typed in. An errored field keeps its error border while focused, it is not repainted with the focus colour.

There is also a screen-level state to build here, not just field states: M07 in edit mode. See `13-state-gallery.md`.

## 5. Form-Level Behavior
- **Entering edit mode:** an Edit control (pencil icon, no visible label, `aria-label="Edit your name"`) sits **immediately after the name**, not at the card's top right, and it is the only Edit control on the screen. Position is what carries its scope now that the label is gone, which is why the position is not a detail: moved back to the card corner, an unlabelled pencil would claim the whole card again. Minimum target 44x44pt, held by padding with negative margins so the touch area does not push the name off its line. It is a real button with a 44pt target. Activating it swaps the name for an input and reveals the Cancel/Save pair. Nothing else on the card moves, which from 2026-09-19 means nothing else on the card changes at all.
- **What does not change:** the qualification row above and the padlocked "Secondary operator" card below stay exactly as they are in both modes. Neither is the partner's to change, and edit mode stopping visibly at those boundaries is what explains why.
- **Submit enable rule:** Save stays enabled; pressing it with an empty name shows the error rather than presenting a dead button. This differs from Forms 1 and 2 deliberately: a pre-filled field cannot be "not started yet", so a disabled Save would read as a broken control rather than as an instruction.
- **Validation strategy:** onSubmit, with the error clearing as soon as the field is edited. There is no onBlur pass, because tabbing out of a pre-filled field should not raise an error the partner has not caused yet.
- **Button treatment (2026-09-19):** Save and Cancel are content-width and right-aligned, not two full-width bars. Full width is a form's treatment and this is one field. Save is the filled primary; Cancel is text only, no outline, because an outlined button beside a filled one reads as an equal second choice, and backing out of an uncommitted change is not one. Both stay 44pt tall.
- **Submit type:** blocking — Save waits for confirmation before the card returns to view mode.
- **Success handling:** the rows come back showing the new values immediately. Focus returns to the Edit control.
- **Failure handling:** network failure → stay in edit mode, preserve entered values, show retry, do not silently discard edits.
- **Autosave:** no — explicit Save, and an explicit Cancel. A profile is not a live-autosave surface.
- **Cancel:** restores the values held when edit mode opened, then returns to view mode. If anything was changed, confirm first (see Edge Cases).

## 6. Accessibility
Standard labeled-input pattern per the rules above, plus what in-place editing adds:
- Activating Edit moves focus into `name`, the first field, with the caret at the end of the existing value. The button alone does not announce the mode change.
- Cancel, Save and a discard confirmation all return focus to the Edit control, never to the top of the screen.
- A failed Save moves focus to the first invalid field.
- The two group headings are real headings (level 2), not styled text. They are the only thing distinguishing the editable group from the admin-managed one.

## 7. Edge Cases
- **Cancel with unsaved changes:** confirm before discarding ("Discard your changes?" / Discard Changes / Keep Editing). Cancel with nothing changed exits straight away, with no dialog to dismiss.
- **Leaving M07 while editing** (tab bar, a menu row, system back): same unsaved-changes guard as Cancel.
- Double-submit prevention on Save.

---

# Not a form: Secondary operator (A.16)

There is no Secondary Operator form and no M19 screen. SOW A.16 scopes the secondary operator (name, contact, PTNZ status) as **view-only in the partner app**, and Matt reconfirmed this directly (Open Questions for Client #21): "There is no in-app workflow for adding or changing a secondary operator... the partner cannot edit it themselves."

What to build instead:
- Its own group on Profile (M07), headed "Secondary operator", holding the three values A.16 names: name, contact, and Powerboat Training NZ status.
- A padlock on the group heading, which is what carries the read-only signal. The body copy is then free to explain what the field is for rather than repeating that it cannot be edited.
- Two states, both to be built: **not set** ("None added", plus "Someone else can operate Halcyon on your behalf. Contact us to add one.") and **set** (three rows, Name / Contact / Powerboat Training NZ status, closing with "Contact us to change any of these."). All of this is static text, not controls, and none of it may carry a `button` role (see `19-a11y-spec.md`, M07).
- No entry for it in edit mode. Form 4 above is the complete set of fields the Edit control exposes, and the secondary operator row sits in the next card down, which edit mode never touches.

An earlier revision of this document specified a three-field editable form here with all-or-nothing validation. It was drafted before A.16 was settled and is now out of scope, do not build it.
