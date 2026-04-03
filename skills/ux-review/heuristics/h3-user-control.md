# H3: User Control and Freedom

## Definition

"Users often perform actions by mistake. They need a clearly marked 'emergency exit' to leave the unwanted action without having to go through an extended process." — Jakob Nielsen, NN/g

## What to Look For

### Code Patterns

- **Destructive actions without confirmation or undo** — delete, remove, clear, or reset handlers that execute immediately without a confirmation dialog (`window.confirm`, modal, or undo toast). Look for `DELETE` API calls triggered directly from button `onClick`.
- **Modals without Escape key or click-outside dismissal** — dialog/modal components that can only be closed via the X button. Check for missing `onKeyDown` Escape handler, missing backdrop click handler, or missing `Radix`/`shadcn` `onOpenChange` prop.
- **Multi-step forms that lose data on navigation** — wizard/stepper components without state persistence (localStorage, sessionStorage, URL params, or React state above the router). If the user presses browser back or refreshes, all input is lost. No `beforeunload` warning.
- **Browser back button breaking SPA state** — client-side navigation that doesn't use `history.pushState` or React Router properly, causing the back button to exit the app entirely or skip expected states.
- **No reset or clear affordance** — filters, search inputs, or multi-select components with no "Clear all," "Reset," or X button to return to the default state. User must manually undo each selection.
- **Dead-end pages** — error pages, empty states, or confirmation screens with no navigation links, no breadcrumbs, no back button, and no obvious way to continue. User is stranded.
- **Missing cancel buttons** — forms, modals, or edit modes that have a "Save" or "Submit" button but no "Cancel" or "Discard" button. User must find another way to abandon changes.
- **Forced flows with no skip or exit** — onboarding, setup wizards, or tutorial overlays that cannot be dismissed, skipped, or exited. User is trapped in the flow.
- **One-way toggles or selections** — UI controls that allow activation but not deactivation (e.g., a checkbox that can be checked but not unchecked, a selection that can't be cleared).
- **No "unsend" or undo window for consequential actions** — sending messages, publishing content, or triggering notifications with no brief undo window or draft state.

### What to Look For (Visual)

- No visible close, cancel, or back button on modals, drawers, or overlay panels.
- No confirmation dialog or "Are you sure?" prompt before irreversible actions (delete, send, publish).
- No "Undo" option after completing a destructive action — the action is immediately permanent.
- No "Clear" or "Reset" affordance on active filters, search bars, or form selections.
- Dead-end screens — a page with content but no links, buttons, or navigation to go anywhere else.
- Forced wizard/onboarding flow with no visible skip or exit option.

## Severity Guide

Rate each finding 0–4 using three factors:

| Factor | 4 — Katastrofe | 3 — Stort | 2 — Mindre | 1 — Kosmetisk |
|--------|----------------|-----------|------------|---------------|
| Frekvens | Every user encounters this in normal workflows | Common flow — most users will hit this | Occasional — specific paths or power-user actions | Rare — unusual edge case |
| Innvirkning | Permanent data loss or irreversible consequence with no recovery | User loses significant work or progress, must redo effort | User is inconvenienced, has to find a workaround | Minor annoyance, easy workaround exists |
| Persistens | Happens every time the action is performed, no way to avoid | Recurs in common workflows, difficult to work around | Happens sometimes, user adapts after learning | One-time issue, user adjusts |

### Examples

- **Severity 4:** Deleting a record with no confirmation dialog and no undo — permanent data loss that cannot be recovered. Every user who accidentally clicks "Delete" loses their data.
- **Severity 3:** Multi-step form (5+ fields across 3 steps) loses all input when the user presses the browser back button — user must re-enter everything from scratch.
- **Severity 2:** Modals can only be closed with the X button — no Escape key handler and no click-outside-to-close. Users expect both and have to hunt for the X.
- **Severity 1:** Search filters have no "Clear all" button — user must remove each filter individually, but can do so without difficulty.
