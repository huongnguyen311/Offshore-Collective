# Offshore Collective — Form Specs (Partner Mobile App)

**Consumed inputs:** `05-user-flows.md` (FLOW-CHECKLIST-01, FLOW-TOW-01), `11-component-specs.md` (StatusBadge, typography.dataInline)
**Note on sequencing:** SKILL 09 (UX Writing) runs after this skill in the pipeline order, so error/help copy below is drafted as reasonable placeholder text, not final — flag to SKILL 09 to confirm/refine wording rather than treating it as locked.

---

# Form 1 — Pre-Departure Checklist (M15)

## 1. Field Inventory

| Field | Label | Input type | Required? | Default | Placeholder | Help text |
|---|---|---|---|---|---|---|
| tankFull | "Is the tank full?" | segmented yes/no toggle | Yes | — | — | — |
| fuelLitres | "Litres reading" | number (dataInline styled) | Conditional — required only if `tankFull = No` | — | "0" | "Read directly from the fuel gauge" |
| fuelPhoto | "Photo of fuel gauge" | photo capture/upload | Yes (always — even when tank is full) | — | — | "Uploaded at full resolution, not compressed" |
| notFullReason | "Why isn't it full?" | short text (single line) | Conditional — required only if `tankFull = No` | — | "e.g. used half a tank on the way out" | — |
| hasDamage | "Any damage?" | segmented yes/no toggle | Yes | — | — | — |
| damageDescription | "Describe the damage" | multi-line text | Conditional — required only if `hasDamage = Yes` | — | "What happened and where" | — |
| damagePhoto | "Photo of the damage" | photo capture/upload | Conditional — required only if `hasDamage = Yes` | — | — | "Uploaded at full resolution, not compressed" |
| towingDisclaimer | "I understand the towing requirements" | checkbox acknowledgement | Conditional — required only if this booking is a Rayglass 2400 towing trip | unchecked | — | Full disclaimer text shown above the checkbox |

## 2. Validation Rules

| Field | Rule | Trigger | Error message | Clears when |
|---|---|---|---|---|
| tankFull | must be answered | onSubmit (it's a toggle, not left blank in practice, but the group is checked on submit) | "Let us know if the tank is full" | either option selected |
| fuelLitres | required, numeric, ≥ 0 when tankFull = No | onBlur | "Enter the litres reading from the gauge" | valid number entered |
| fuelPhoto | required, always | onSubmit | "Add a photo of the fuel gauge" | a photo is attached |
| notFullReason | required, non-empty when tankFull = No | onBlur | "Tell us why the tank isn't full" | any text entered |
| hasDamage | must be answered | onSubmit | "Let us know if there's any damage" | either option selected |
| damageDescription | required, non-empty when hasDamage = Yes | onBlur | "Describe the damage" | any text entered |
| damagePhoto | required when hasDamage = Yes | onSubmit | "Add a photo of the damage" | a photo is attached |
| towingDisclaimer | must be checked, when applicable | onSubmit | "Please confirm you understand the towing requirements before continuing" | checkbox is checked |

**On the 20-litre tolerance:** this is a *system-side* check, not a field-level validation the partner sees — a small discrepancy between the reported litres and the expected reading never blocks submission or shows an error to the partner. It only ever triggers a Needs Your Action item for Matt in the Admin Portal, silently. Do not add any partner-visible tolerance messaging to this form.

## 3. Input Masks & Formatting
- `fuelLitres`: plain integer or one-decimal number, no thousands separator (litres values are always small, e.g. "45" or "45.5"), styled with `typography.dataInline` per `09-typography-system.md`'s numeral-consistency rule.
- No other field in this form needs a mask (no phone/currency/card formatting applicable).

## 4. Field States
Per `alice-component-design` conventions: empty → focus → filled → valid → error → disabled.
- `fuelLitres` and `notFullReason`/`damageDescription` fields are **hidden entirely**, not just disabled, until their trigger condition (`tankFull = No` / `hasDamage = Yes`) is met — this is a progressive-disclosure pattern, not a grayed-out disabled state.
- Photo fields show a filled thumbnail once attached; tapping the thumbnail allows retake/replace before submit.
- `towingDisclaimer` checkbox is hidden entirely on non-2400 bookings — never shown as a disabled/unchecked no-op.

## 5. Form-Level Behavior
- **Submit enable rule:** the Submit button is always visible but tapping it while any required field (per current conditional state) is incomplete triggers validation and blocks submission — not a permanently-disabled button, since which fields are "required" changes dynamically as tankFull/hasDamage toggle.
- **Validation strategy:** text/number fields validate onBlur after first interaction; the two yes/no toggles and required photos validate onSubmit (since there's no natural "blur" for a toggle or an attachment).
- **Submit type:** blocking — the checklist is not submitted until all currently-required fields pass.
- **Success handling:** on successful submit, photo uploads at full resolution (no client-side compression, per WBS), the checklist record is created, any fuel shortfall (beyond 20L tolerance) or damage report is simultaneously flagged into Matt's Needs Your Action feed, and the user returns to Upcoming Booking Detail (M09) — no approval gate blocks departure (per FLOW-CHECKLIST-01).
- **Failure handling (upload failure):** if a photo fails to upload (network failure mid-submit), preserve all entered text-field values and the locally-held photo, show an inline retry affordance on the specific photo field rather than clearing the form.
- **Autosave:** not specified in the WBS — recommend a lightweight local draft (device-only, not synced) so backgrounding the app mid-checklist doesn't lose entered data, but this is a recommendation, not a confirmed requirement; flag as an open question if precision matters to engineering.

## 6. Accessibility
- Every field has an associated `<label>` (`for`/`id`); yes/no toggles use `role="radiogroup"` with each option as `role="radio"`.
- Errors: `aria-describedby` points to the inline error text; `aria-invalid="true"` set on the field.
- On submit-with-errors: focus moves to the first invalid/incomplete field in visual order (tankFull → fuelLitres/notFullReason/fuelPhoto → hasDamage → damageDescription/damagePhoto → towingDisclaimer).
- Errors announced via `role="alert"` live region.
- Required fields (fixed: fuelPhoto, hasDamage, tankFull; conditional: the rest) are marked with both a visible `*` and `aria-required="true"`.
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
| Additional field | `extraPhotos` — optional, multiple photo attachments beyond the required fuel/damage photos. No validation rule (always optional, no minimum/maximum count specified in WBS). |
| Heading/footer copy | A specific heading and footer disclaimer about photo usage is shown on this screen — not present on the Pre-Departure version. Final copy in `14-ux-writing.md` § "Post-Use Checklist Photo Disclaimer (M16)". |
| Success handling differs | Submitting this checklist triggers the turnaround process (Admin Portal territory) in addition to the same flagging behavior as Form 1 — the partner-facing result is the same (return to M09/M03), but a backend workflow starts that has no partner-visible form consequence. |
| No towing-disclaimer field | The towing disclaimer is a pre-departure-only acknowledgement — it does not repeat on the post-use checklist. |

---

# Form 3 — Towing Destination Entry (M10)

## 1. Field Inventory

| Field | Label | Input type | Required? | Default | Placeholder | Help text |
|---|---|---|---|---|---|---|
| destination | "Towing destination" | short text (single line) | Yes | — | "e.g. Lake Taupō boat ramp" | "Matt will review and approve this before your dates are confirmed" |

## 2. Validation Rules

| Field | Rule | Trigger | Error message | Clears when |
|---|---|---|---|---|
| destination | required, non-empty | onBlur | "Enter where you're towing to" | any text entered |

No format/mask validation — this is a free-text destination description, not a structured address field (not specified as structured in the WBS).

## 3. Input Masks & Formatting
None applicable.

## 4. Field States
Standard single-field states (empty → focus → filled → valid → error). No conditional sub-fields.

## 5. Form-Level Behavior
- **Submit enable rule:** Submit (which proceeds to Booking Review, M08) is blocked until `destination` is non-empty.
- **Validation strategy:** onBlur after first interaction, consistent with the general rule.
- **Submit type:** non-blocking on a network call at this step — entering a destination doesn't itself call the server; it's carried forward and submitted together with the booking confirmation at M08 (per FLOW-BOOK-01 Case 3 / FLOW-TOW-01).
- **Success handling:** proceeding confirms the booking as **held, Pending Approval** (not fully confirmed) — this is a deliberate different outcome from a normal booking's immediate confirmation, and the form should not imply full confirmation on submit.
- **Editing after approval (FLOW-TOW-01 Case 2):** if a partner edits `destination` on an already-approved booking from Booking Detail (M09), re-submitting this same field resets the booking to Pending Approval. **Per the UX Notes flagged in `05-user-flows.md`, show a confirmation prompt** ("Changing this will require Matt's approval again — continue?") before accepting the edit, since this is a non-obvious consequence.
- **Failure handling:** not applicable at this step (no server call here) — any failure handling belongs to the M08 confirm action downstream.

## 6. Accessibility
- `<label>` associated via `for`/`id`.
- Error: `aria-describedby` → inline error, `aria-invalid="true"`.
- Required marked with visible `*` and `aria-required="true"`.
- The re-approval confirmation prompt (edit case) uses `role="alertdialog"` with focus trapped until Confirm/Cancel is chosen.

## 7. Edge Cases
- Partner navigates away before submitting → no booking is held yet (per FLOW-BOOK-01 Case 4), so nothing to guard/recover.
- Partner pastes an extremely long destination string → no hard cap specified; recommend a generous soft cap (e.g. 200 characters), consistent with the checklist forms' approach.
- Partner edits destination, then immediately edits it again before Matt has acted on the first change → each edit re-triggers the same "requires approval again" prompt; the booking remains Pending Approval throughout (no double-submission risk since approval state, not a queue of edits, is what's tracked).

---

# Form 4 — Edit Profile (M18)

## 1. Field Inventory

| Field | Label | Input type | Required? | Default | Placeholder | Help text |
|---|---|---|---|---|---|---|
| name | "Name" | short text | Yes | current value | — | — |
| contactPhone | "Phone number" | tel | Yes | current value | "+64 21 123 4567" | — |
| contactEmail | "Email" | email | Yes | current value | — | — |

The WBS scopes this as "view/edit personal profile info" without enumerating exact fields beyond name/contact — name/phone/email are the reasonable minimum set; flag to confirm the full field list with the client if more profile data exists than the WBS states.

## 2. Validation Rules

| Field | Rule | Trigger | Error message | Clears when |
|---|---|---|---|---|
| name | required, non-empty | onBlur | "Enter your name" | any text entered |
| contactPhone | required, valid phone format | onBlur | "Enter a valid phone number" | valid format typed |
| contactEmail | required, valid email format | onBlur | "Enter a valid email address" | valid format typed |

## 3. Input Masks & Formatting
- `contactPhone`: NZ-style mobile format guidance in placeholder only (`+64 21 123 4567`) — no hard input mask enforced, since partners may have international numbers; validate format loosely (contains digits, reasonable length) rather than a rigid pattern.

## 4. Field States
Standard text-field states (empty/focus/filled/valid/error/disabled). All three fields pre-fill with the current saved value (never truly "empty" on entry to this form).

## 5. Form-Level Behavior
- **Submit enable rule:** Save is enabled once all three fields are valid; disabled if any is invalid.
- **Validation strategy:** onBlur per field after first interaction.
- **Submit type:** blocking — Save waits for confirmation before returning to Profile (M07).
- **Success handling:** returns to M07 with updated values reflected immediately.
- **Failure handling:** network failure → preserve entered values, show retry, do not silently discard edits.
- **Autosave:** no — explicit Save action required, consistent with a profile-edit pattern (not a live-autosave form).

## 6. Accessibility
Standard labeled-input pattern per the rules above; focus moves to the first invalid field on failed Save attempt.

## 7. Edge Cases
- Unsaved-changes guard: navigating away with unsaved edits should confirm before discarding.
- Double-submit prevention on Save.

---

# Form 5 — Secondary Operator (M19)

## 1. Field Inventory

| Field | Label | Input type | Required? | Default | Placeholder | Help text |
|---|---|---|---|---|---|---|
| operatorName | "Secondary operator name" | short text | **No — fully optional** | — | — | "Add a secondary operator if someone else may operate the boat on your behalf" |
| operatorContact | "Contact details" | text (phone or email) | **No — optional**, but see rule below | — | — | — |
| operatorPtnzStatus | "Powerboat Training NZ status" | select (e.g. Not certified / Pending / Certified) | **No — optional**, but see rule below | — | — | — |

## 2. Validation Rules

| Field | Rule | Trigger | Error message | Clears when |
|---|---|---|---|---|
| operatorName | none if empty; if filled, non-empty text | onBlur | — (no error state for empty; only relevant if partially filled oddly) | — |
| operatorContact | if `operatorName` is filled, `operatorContact` becomes required | onBlur | "Add contact details for your secondary operator" | non-empty value entered |
| operatorPtnzStatus | if `operatorName` is filled, becomes required | onSubmit | "Select the secondary operator's Powerboat Training NZ status" | a value selected |

**Design decision, stated explicitly:** the WBS says the whole field is optional ("the partner is not required to fill it in"), but doesn't specify partial-fill behavior (e.g. name filled, contact blank). This spec treats the group as **all-or-nothing** — once a name is entered, contact + status become required, since a secondary operator record with a name but no way to reach them or verify their qualification isn't a usable record. Flag this interpretation for confirmation.

## 3. Input Masks & Formatting
- `operatorContact`: same loose validation as `contactPhone` in Form 4 if a phone is entered; accepts email format too (single field, either type, per the WBS's "name, contact, PTNZ status" description not distinguishing phone vs email).

## 4. Field States
Standard states; the entire block can remain empty indefinitely with no error shown, satisfying the "optional" requirement — errors only appear once `operatorName` has been touched.

## 5. Form-Level Behavior
- **Submit enable rule:** Save is always enabled when the block is fully empty; once `operatorName` is filled, Save is blocked until `operatorContact` and `operatorPtnzStatus` are also valid (per the all-or-nothing rule above).
- **Validation strategy:** onBlur for text fields, onSubmit for the select.
- **Submit type:** blocking, same as Form 4 (this form is reached from within Edit Profile per the IA).
- **Success handling:** returns to Edit Profile (M18) / Profile (M07) with the secondary operator info saved or cleared.
- **Failure handling:** same network-failure preservation pattern as Form 4.

## 6. Accessibility
Standard labeled pattern; the conditional-required behavior on `operatorContact`/`operatorPtnzStatus` must update `aria-required` dynamically as `operatorName` is filled/cleared, not just visually.

## 7. Edge Cases
- Partner fills in `operatorName` then deletes it again → `operatorContact`/`operatorPtnzStatus` requirements should also clear, and any values already entered in those fields are preserved (not force-cleared) in case the partner re-adds the name.
- Unsaved-changes guard, double-submit prevention — same as Form 4.
