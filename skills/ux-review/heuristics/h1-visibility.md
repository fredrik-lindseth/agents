# H1: Visibility of System Status

## Definition

"The design should always keep users informed about what is going on, through appropriate feedback within a reasonable amount of time." — Jakob Nielsen, NN/g

## What to Look For

### Code Patterns

- **Async operations without loading indicators** — fetch/axios/useMutation/useQuery calls without corresponding `isLoading`/`isPending` state rendered in the UI. The user clicks and nothing visually changes.
- **Buttons without disabled/loading state on click** — submit buttons that stay enabled during async operations, risking double submission. Look for `onClick` handlers that trigger API calls without setting `disabled` or showing a spinner.
- **Multi-step flows without step indicators** — wizard/stepper/checkout components with no visual indicator of current step, total steps, or progress. User has no idea where they are in the process.
- **Form submission with no success/failure feedback** — form `onSubmit` handlers that call an API but show no toast, banner, alert, redirect, or inline message on completion or failure. The user submits and the form just sits there.
- **No active state on navigation items** — nav links/tabs that don't highlight or visually distinguish the current page/section. Look for missing `aria-current`, `active` class, or equivalent styling logic.
- **No breadcrumbs in nested hierarchies** — deeply nested routes (3+ levels) with no breadcrumb trail. User loses orientation within the information architecture.
- **Stale data without freshness indicator** — dashboards, lists, or data displays that auto-refresh or cache data without showing a "last updated" timestamp or refresh button. User can't tell if data is current.
- **Empty state indistinguishable from loading state** — components that render the same UI (blank area, no message) for both "zero results" and "still loading." Look for conditional rendering that doesn't differentiate `loading` vs `empty`.
- **Background save/sync with no confirmation** — auto-save, background sync, or optimistic updates that complete silently. User doesn't know if their changes persisted.
- **Tab/section changes with no active indicator** — tabbed interfaces or segmented controls where the selected tab has no visual differentiation (no border, background, or color change).

### What to Look For (Visual)

- No visible spinner, skeleton screen, or loading placeholder during data fetches — the page appears frozen or empty.
- Active navigation item looks identical to inactive items — no color, weight, border, or background difference.
- Progress indicators missing in multi-step flows — user sees form content but no "Step 2 of 4" or progress bar.
- No confirmation message (toast, banner, inline text) after completing an action like saving, deleting, or submitting.
- Empty page that could mean "no data exists" or "data is still loading" — nothing distinguishes the two states.
- File upload or long-running operation with no progress bar or percentage indicator.
- Search results appearing with no result count or "Showing X results" label.

## Severity Guide

Rate each finding 0–4 using three factors:

| Factor | 4 — Katastrofe | 3 — Stort | 2 — Mindre | 1 — Kosmetisk |
|--------|----------------|-----------|------------|---------------|
| Frekvens | Every user hits this on every session | Most users encounter it regularly | Some users hit it occasionally | Rare edge case |
| Innvirkning | User cannot tell if action succeeded — causes data loss or duplicates | User is confused and may retry or abandon task | User is momentarily uncertain but figures it out | Slight lack of polish, no real confusion |
| Persistens | Happens every time, user cannot learn to avoid it | Recurs often, workaround is non-obvious | Happens repeatedly but user adapts quickly | One-time annoyance |

### Examples

- **Severity 4:** Form submission shows no feedback at all — user doesn't know if it worked, submits again, causing duplicate records or payments.
- **Severity 3:** Search returns zero results but displays a completely blank area with no message — page looks broken or unresponsive.
- **Severity 2:** Background auto-save works correctly but gives no visual confirmation — user is unsure if changes were saved and may navigate away anxiously.
- **Severity 1:** "Last updated" timestamp missing on a dashboard that refreshes every 30 seconds.
