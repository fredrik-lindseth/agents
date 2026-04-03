# H2: Match Between System and the Real World

## Definition

"The design should speak the users' language. Use words, phrases, and concepts familiar to the user, rather than internal jargon. Follow real-world conventions, making information appear in a natural and logical order." — Jakob Nielsen, NN/g

## What to Look For

### Code Patterns

- **Developer/technical jargon in UI strings** — user-facing text containing words like "entity," "instance," "payload," "null," "undefined," "NaN," "token," "session," "query," or "parameter." Search string literals, i18n files, and component JSX text content.
- **HTTP status codes shown to users** — error handling that renders "Error 403," "Error 422," "Error 500," or "404 Not Found" as the primary user-facing message instead of a human-readable explanation.
- **Database field names leaking into UI** — snake_case identifiers like `created_at`, `updated_at`, `is_active`, `user_id` rendered in labels, table headers, or form fields. Also camelCase internal names (`firstName` instead of "First name").
- **Sort order by internal ID or database order** — lists sorted by auto-increment ID or insertion order instead of meaningful order (alphabetical, chronological, by relevance, by status).
- **Boolean values displayed as "true"/"false"** — raw boolean rendering instead of human-readable labels like "Yes"/"No," "Active"/"Inactive," or "Enabled"/"Disabled."
- **Date/number formats not matching locale** — dates shown as `2026-04-03` (ISO) or `04/03/2026` (US) in a Norwegian locale instead of `3. april 2026`. Numbers using period as thousands separator in a comma-locale context.
- **Unexplained abbreviations or acronyms** — domain-specific abbreviations (API, UUID, CORS, JWT, OIDC) in user-facing UI without tooltips or explanations.
- **Misleading action labels** — "Delete" when the action is really "Archive" or "Remove from list"; "Submit" when the action is "Save draft"; "Send" when nothing is actually sent.
- **Technical error messages** — raw error strings like `ECONNREFUSED`, `ETIMEDOUT`, `SQLITE_CONSTRAINT`, `NetworkError`, or stack traces shown to users.
- **Internal IDs or UUIDs visible** — database IDs, UUIDs, or hash strings displayed in the UI where users can see them (URLs are acceptable, but prominent display in content is not).
- **Unfamiliar icons without labels** — icon-only buttons using non-standard or ambiguous icons without text labels or tooltips. Users guess at meaning.

### What to Look For (Visual)

- Technical terms, codes, or jargon visible in buttons, labels, headings, or error messages.
- Information presented in an unnatural order — e.g., newest items at the bottom, alphabetical sort in a context where chronological makes more sense.
- Icons that don't match common mental models — e.g., a gear icon for "profile," a bell icon for "settings."
- Locale mismatch — English labels in a Norwegian app, US date format in a European context, or mixed languages in the same view.
- Raw data values (true/false, null, IDs, timestamps) visible where human-readable text should appear.
- Error messages that read like log output rather than user guidance.

## Severity Guide

Rate each finding 0–4 using three factors:

| Factor | 4 — Katastrofe | 3 — Stort | 2 — Mindre | 1 — Kosmetisk |
|--------|----------------|-----------|------------|---------------|
| Frekvens | Every user sees this in normal use | Most users encounter it in common flows | Occasional — only in specific flows or edge cases | Rare — only power users or unusual paths |
| Innvirkning | User cannot understand what to do — task blocked | User is confused, may make wrong choice or abandon task | User pauses, has to think, but eventually understands | Slight oddness, no real confusion |
| Persistens | Every interaction, cannot learn past it | Recurs frequently, hard to memorize meaning | Happens sometimes, user adapts after first encounter | One-time confusion |

### Examples

- **Severity 4:** Raw JSON error response (`{"error":"ECONNREFUSED","code":500}`) shown to the user when an API call fails — user has no idea what happened or what to do.
- **Severity 3:** Navigation uses internal module names ("EntityManager," "ResourcePool") instead of user-facing terms ("Contacts," "Documents").
- **Severity 2:** Dates displayed as `2026-04-03` (ISO format) instead of `3. april 2026` in a Norwegian locale — user can read it but it feels foreign.
- **Severity 1:** A tooltip uses a slightly technical abbreviation ("ID") where "identifikator" would be clearer.
