# H10: Help and Documentation

## Definition

"It's best if the system doesn't need any additional explanation. However, it may be necessary to provide documentation to help users understand how to complete their tasks." — Jakob Nielsen, NN/g

## What to Look For

### Code Patterns

- **Complex features with no contextual help** — specialized or domain-specific features with no tooltips, info icons (`<InfoIcon />`), inline help text, or contextual guidance. Look for complex forms, configuration panels, or domain-specific views that render only inputs and labels with no supporting explanation.
- **No help or support link in navigation** — header, footer, sidebar, and navigation components that contain no "Help," "Support," "Documentation," or "Contact" link. Users have no discoverable path to assistance when they get stuck.
- **No onboarding for first-time users** — no welcome flow, getting-started guide, feature tour, or empty-state guidance for new users. Look for absence of "first visit" detection (no localStorage/cookie check for `hasSeenOnboarding`, no onboarding component, no empty-state with guidance).
- **Help organized by architecture instead of tasks** — documentation or help sections structured around internal module names, API endpoints, or technical components instead of user tasks and goals. Users must understand the system's architecture to find relevant help.
- **No way to contact support** — absence of any contact mechanism: no support email, no feedback form, no chat widget, no issue reporter. When self-service fails, users have no escalation path.
- **Tooltips too long or too ephemeral** — tooltips containing paragraph-length text that's impossible to read before the tooltip closes, or tooltips with very short display timeouts. Look for tooltip components with long content strings or short `delay`/`timeout` values.
- **Onboarding that can't be replayed** — onboarding tour or tutorial that fires once and has no way to revisit it. No "Replay tour," "Getting started," or "What's new" option in menus or settings. Look for onboarding state stored with no reset mechanism exposed to the user.
- **No FAQ or common questions addressed** — complex flows (registration, payment, data import) with no inline FAQ, help section, or answers to predictable questions that users will have.
- **Search with no "no results" guidance** — search functionality that shows an empty state on zero results without suggesting alternative queries, checking for typos, or offering help. Look for empty-state rendering in search result components.
- **Complex form fields without help text** — fields expecting specific formats (dates, tax numbers, reference codes, regex patterns) or domain-specific values with no `helperText`, description, format example, or "where to find this" guidance.

### What to Look For (Visual)

- No visible help affordance anywhere in the interface — no question mark icon, no "Help" link in the header/footer, no info buttons next to complex elements.
- Complex features with no contextual guidance — specialized screens or forms where a new user would not understand what to enter or what the feature does.
- No getting-started or onboarding for new users — a first-time user lands on a page with no guidance, tour, or welcome flow.
- Help is available but hard to find — buried in a footer link, deep in settings, or hidden behind an obscure icon that users wouldn't think to click.
- No field-level hints in complex forms — input fields for specialized values (codes, formatted numbers, technical identifiers) with no description, example, or format hint.
- Search results page with zero results showing no suggestions, no help link, and no alternative actions.

## Severity Guide

Rate each finding 0–4 using three factors:

| Factor | 4 — Katastrofe | 3 — Stort | 2 — Mindre | 1 — Kosmetisk |
|--------|----------------|-----------|------------|---------------|
| Frekvens | Every new user and many returning users are affected | Most first-time users or users attempting complex tasks are affected | Some users occasionally need help that isn't available | Rare scenario, most users manage without help |
| Innvirkning | Users cannot figure out how to use core features at all — complete task failure | Users are significantly delayed or make errors because guidance is missing for important tasks | Users are somewhat confused but eventually figure it out through experimentation | Minor inconvenience, user finds the answer quickly elsewhere |
| Persistens | Affects every session until user learns through external means (asking others, searching the web) | Affects new users and recurs when attempting infrequent complex tasks | First-time confusion that resolves after initial learning | One-time minor friction |

### Examples

- **Severity 4:** Complex professional tool with zero documentation, no tooltips, and no onboarding — users cannot figure out how to use core features and must seek external help or give up entirely.
- **Severity 3:** First-time user lands on an empty dashboard with no guidance on how to get started — no empty-state message, no "create your first item" prompt, no getting-started link.
- **Severity 2:** Complex form fields (tax codes, reference numbers, coordinate formats) with no help text explaining the expected format or where to find the value.
- **Severity 1:** Help section exists but is organized by internal module names instead of user tasks — users can find help but must translate between their goals and the system's terminology.
