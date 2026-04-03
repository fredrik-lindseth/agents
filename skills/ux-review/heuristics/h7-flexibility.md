# H7: Flexibility and Efficiency of Use

## Definition

"Shortcuts — hidden from novice users — may speed up the interaction for the expert user so that the design can cater to both inexperienced and experienced users. Allow users to tailor frequent actions." — Jakob Nielsen, NN/g

## What to Look For

### Code Patterns

- **No keyboard shortcuts for frequent actions** — data-heavy or productivity applications where common operations (save, search, navigate, toggle) lack keyboard shortcut bindings. Look for absence of `useHotkeys`, `keydown` event listeners, or `accessKey` attributes on frequently used actions.
- **No bulk/batch actions** — lists or tables where users must perform operations (delete, update, export, approve) one item at a time. Look for missing multi-select checkboxes, "Select All" controls, or batch action toolbars.
- **Filter/search state not reflected in URL** — filter selections, search queries, sort orders, or pagination state not serialized into URL query parameters or hash. Users cannot bookmark, share, or reload a specific view. Look for filter state managed only in component state (useState/Redux) without `URLSearchParams` or router integration.
- **Forced wizard for experienced users** — multi-step wizard or stepper flow with no option to skip steps, collapse sections, or fill a single comprehensive form. Experienced users who know what to enter are forced through the same slow flow every time.
- **No data export** — tables, lists, or chart data visible on screen with no export option (CSV, JSON, PDF, clipboard). Users must manually transcribe or screenshot displayed data.
- **No customization of views** — dashboards, tables, or list views without column pickers, layout options, saved views, or drag-to-reorder. Every user sees the same fixed layout regardless of their needs.
- **No copy-to-clipboard for displayed values** — IDs, reference numbers, URLs, codes, or other values displayed in the UI with no click-to-copy affordance. Users must manually select text and copy.
- **Frequent actions buried in deep menus** — common operations (e.g., create new item, edit, share) hidden behind 3+ clicks, nested dropdowns, or settings pages, instead of being accessible from the primary interface.
- **No power-user alternative paths** — absence of features like command palette (`Cmd+K`), quick-add, inline editing, or type-ahead navigation that let experienced users bypass standard UI flows.
- **User-configurable defaults missing** — default values (page size, sort order, view mode, notification preferences) that cannot be changed by the user. Every session starts from the same factory defaults.

### What to Look For (Visual)

- No keyboard shortcut hints visible anywhere in the interface — no shortcut badges on menu items, no shortcut legend, no tooltip hints showing key bindings.
- No multi-select UI for lists or tables — no checkboxes, no "Select All," no visible batch action toolbar.
- No "Export," "Download," or "Share" affordance near data displays (tables, charts, reports).
- No customization controls — no column picker icon, no layout toggle, no "Customize" button on dashboards or tables.
- Common actions requiring 3+ clicks to reach — deeply nested menu structures for everyday operations.
- No command palette or quick-search UI pattern for keyboard-driven navigation.

## Severity Guide

Rate each finding 0–4 using three factors:

| Factor | 4 — Katastrofe | 3 — Stort | 2 — Mindre | 1 — Kosmetisk |
|--------|----------------|-----------|------------|---------------|
| Frekvens | Every user performing core tasks is affected | Most regular users encounter it in daily use | Power users or specific workflows are affected | Rare edge case or nice-to-have |
| Innvirkning | Task is extremely slow or practically impossible to complete efficiently — orders of magnitude slower than it should be | User productivity is significantly reduced — workarounds exist but are painful | User is slowed down but can accomplish the task with moderate effort | Minor inconvenience, slightly less efficient than optimal |
| Persistens | Affects every single interaction with the feature, no workaround | Recurs every session, workaround is cumbersome | Happens regularly but user develops partial workarounds | One-time setup annoyance or rare frustration |

### Examples

- **Severity 4:** Data table with 500+ rows but no bulk actions, no search, and no filters — managing entries requires clicking into each row individually, making the feature practically unusable at scale.
- **Severity 3:** URL does not reflect current filters, search query, or sort order — users cannot bookmark or share a specific filtered view and must re-apply filters every time they return.
- **Severity 2:** No keyboard shortcuts in a productivity application used daily — experienced users must always use the mouse for operations that could be instant with key bindings.
- **Severity 1:** No "copy to clipboard" button next to displayed reference numbers or IDs — users must manually select and copy text.
