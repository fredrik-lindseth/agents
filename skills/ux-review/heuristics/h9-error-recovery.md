# H9: Help Users Recognize, Diagnose, and Recover from Errors

## Definition

"Error messages should be expressed in plain language (no error codes), precisely indicate the problem, and constructively suggest a solution." — Jakob Nielsen, NN/g

## What to Look For

### Code Patterns

- **Generic error messages** — catch blocks or error handlers rendering vague messages like "Something went wrong," "An error occurred," "Error," or "Invalid input" with no specifics about what failed or why. Look for hardcoded generic strings in catch blocks, error boundaries, or toast calls.
- **Technical error codes/messages exposed to users** — raw error codes ("Error 500," "ECONNREFUSED," "SQLITE_CONSTRAINT_UNIQUE"), stack traces, or exception class names rendered in the UI. Look for `error.message` or `error.toString()` passed directly to user-facing components without sanitization.
- **Problem stated without recovery guidance** — error messages that say what's wrong but not how to fix it: "Invalid email" without "Please enter a valid email address (e.g., name@example.com)." Check validation messages for presence of constructive next steps.
- **Form errors shown only at page/form top** — validation errors collected into a summary banner or alert at the top of the form, without inline error messages next to the specific fields that have problems. Users must scroll and match error descriptions to fields.
- **Raw API/JSON error responses in UI** — error response bodies rendered directly in the interface, showing JSON objects, HTTP status text, or internal error structures. Look for `JSON.stringify(error)` or raw response text in rendered output.
- **Auto-dismissing error toasts** — error notifications using toast/snackbar with auto-dismiss timeout under 5 seconds. Errors require user acknowledgment — they need time to read and understand the problem. Look for `duration`, `timeout`, or `autoClose` values on error toasts.
- **Dead-end error pages** — 404, 500, or generic error pages that provide no navigation path forward: no search bar, no home link, no "go back" button, no suggested alternatives. User is stranded.
- **Raw network errors shown to users** — "Failed to fetch," "Network Error," "TypeError: Failed to fetch," or "net::ERR_CONNECTION_REFUSED" rendered as-is in the UI. Look for fetch/axios error messages passed through without human-readable transformation.
- **No distinction between user-fixable and system errors** — the same error component and messaging style used for both input validation errors (user can fix) and server/infrastructure errors (user cannot fix). Users can't tell if they should retry, change their input, or wait.
- **Validation errors missing constraint details** — messages like "Too long," "Too short," "Invalid format" without specifying the actual constraint (max length, min length, expected format). Look for validation libraries where constraint values aren't interpolated into messages.

### What to Look For (Visual)

- Error messages not visually prominent — no red/warning color, no error icon, no distinct styling that draws the eye. Errors blend in with regular content.
- Error text too small, low contrast, or poorly positioned — easy to miss entirely, especially on a busy page.
- No suggested action or "what to do next" in error states — message describes the problem but leaves the user without guidance.
- Error page is a dead end — no navigation links, no search bar, no way to recover except using the browser back button.
- Error toasts or alerts that disappear quickly — error notifications visible for only a few seconds before auto-dismissing.
- Inline field errors missing — errors shown only in a top-level summary, not next to the problematic fields.

## Severity Guide

Rate each finding 0–4 using three factors:

| Factor | 4 — Katastrofe | 3 — Stort | 2 — Mindre | 1 — Kosmetisk |
|--------|----------------|-----------|------------|---------------|
| Frekvens | Every user who encounters an error sees this | Most error scenarios produce this problem | Some error paths are affected | Rare error condition |
| Innvirkning | User cannot determine what happened or what to do — may cause data loss, duplicate transactions, or complete task abandonment | User is confused, may retry incorrectly, or abandons the task after failed troubleshooting | User is momentarily confused but eventually figures out the problem or works around it | Slightly unhelpful but user understands the situation |
| Persistens | Every error occurrence produces the same unhelpful message — user can never self-diagnose | Recurs on common error paths, no alternative way to get information | Happens on some errors, user learns patterns over time | One-time confusion or cosmetic issue |

### Examples

- **Severity 4:** Payment failure shows "Error 500" with no explanation — user doesn't know if they were charged, has no guidance on next steps, and may retry causing a double charge.
- **Severity 3:** Form validation shows "Invalid input" on 3 fields simultaneously without indicating what's wrong with each — user must guess which constraint they violated for each field.
- **Severity 2:** Error toast auto-dismisses after 3 seconds — not enough time to read the message, and there's no notification history to review it later.
- **Severity 1:** 404 page exists and looks designed but doesn't suggest where to go next — user must manually navigate via the browser address bar or back button.
