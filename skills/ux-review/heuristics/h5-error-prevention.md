# H5: Error Prevention

## Definition

"Good error messages are important, but the best designs carefully prevent problems from occurring in the first place. Either eliminate error-prone conditions, or check for them and present users with a confirmation option before they commit to the action." — Jakob Nielsen, NN/g

## What to Look For

### Code Patterns

- **Free-text input where constrained input would prevent errors** — text fields for dates (should be date picker), numbers (should be `type="number"` or stepper), selections from a known list (should be dropdown/select/combobox), email (should be `type="email"`). Look for `<input type="text">` where a more specific type exists.
- **No confirmation before destructive actions** — delete, overwrite, send, publish, or reset handlers that execute immediately without a confirmation dialog or "Are you sure?" prompt. Look for `DELETE`/`PUT` API calls in `onClick` without an intermediate confirmation step.
- **Forms submittable with empty required fields** — submit buttons that are always enabled, with validation only running after submission. Look for forms where `required` attributes, Zod schemas, or validation checks are missing or only checked on submit.
- **No inline/real-time validation** — validation errors only shown after the user submits the entire form. No `onBlur` or `onChange` validation to catch mistakes early. User fills 10 fields, submits, then discovers errors.
- **Number inputs accepting letters, date inputs accepting invalid dates** — missing `type`, `min`, `max`, `pattern`, or `inputMode` attributes that would constrain input at the browser level. Free-form text where the backend expects structured data.
- **No autosave for long forms** — multi-step forms or lengthy input flows with no periodic save to localStorage, sessionStorage, or server draft. A page refresh or accidental navigation loses all work.
- **Double-click causing duplicate submissions** — submit buttons without debounce, `disabled` state after click, or request deduplication. Look for `onClick` or `onSubmit` handlers that fire API calls without guarding against rapid repeated clicks.
- **No character limit enforcement on constrained fields** — text inputs without `maxLength` where the database column has a character limit. User types a long value, submits, and gets a server error.
- **Dangerous actions styled identically to safe actions** — "Delete" button using the same `variant="default"` as "Save" button, placed adjacent to it. No color differentiation (red for destructive, primary for safe) and no spatial separation.
- **Submit button enabled before prerequisites are met** — a button that should only be clickable after required fields are filled, terms are accepted, or a selection is made, but is enabled from the start. Clicking it produces an error instead of being prevented.
- **No type-ahead or fuzzy matching on search** — exact-match-only search that returns zero results for typos, when a fuzzy match or "did you mean?" suggestion would prevent the error.
- **Clipboard/paste not handled for formatted inputs** — phone number, credit card, or date fields that break when the user pastes a formatted value (e.g., pasting "+47 123 45 678" into a digits-only field).

### What to Look For (Visual)

- No inline validation visible while the user fills in form fields — errors only appear after clicking submit.
- Destructive buttons (delete, remove, reset) visually identical to safe buttons (save, continue, submit) — same color, size, and style.
- Required fields not visually marked — no asterisk, no "required" label, no differentiation from optional fields.
- Input fields without visible constraints — no placeholder showing expected format, no helper text, no character counter.
- Dangerous action button placed directly adjacent to a safe action button with no visual or spatial separation.
- Free-text input field where a structured control (dropdown, date picker, slider) would be more appropriate and error-resistant.
- Submit button appears enabled/clickable when the form is clearly incomplete.

## Severity Guide

Rate each finding 0–4 using three factors:

| Factor | 4 — Katastrofe | 3 — Stort | 2 — Mindre | 1 — Kosmetisk |
|--------|----------------|-----------|------------|---------------|
| Frekvens | Every user encounters this in normal use | Common flow, most users affected | Occasional — specific paths or inputs | Rare edge case |
| Innvirkning | Causes data loss, financial error, or unrecoverable mistake | User submits invalid data, must redo work, or triggers unintended action | User makes a correctable mistake, wasting time | Minor inconvenience, easily noticed and fixed |
| Persistens | Happens every time, no way to avoid without external knowledge | Recurs in common workflows, workaround is non-obvious | Happens sometimes, user learns to avoid it | One-time issue |

### Examples

- **Severity 4:** "Delete" button placed directly next to "Save" button with identical styling and no confirmation dialog — accidental clicks cause permanent data loss.
- **Severity 3:** Form allows submission with invalid data (e.g., letters in a phone number field), then shows a server error and clears all input — user must re-enter everything.
- **Severity 2:** No inline validation on a 10-field form — user fills everything out, submits, then discovers 3 fields have errors. Must scroll back to find and fix them.
- **Severity 1:** Optional fields not visually distinguished from required fields — user fills in everything "just in case," wasting time but causing no errors.
