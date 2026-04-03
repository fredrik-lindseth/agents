# H6: Recognition Rather Than Recall

## Definition

"Minimize the user's memory load by making elements, actions, and options visible. The user should not have to remember information from one part of the interface to another. Information required to use the design should be visible or easily retrievable." — Jakob Nielsen, NN/g

## What to Look For

### Code Patterns

- **Icon-only buttons without text labels or tooltips** — buttons or toolbar items rendering only an icon (`<Icon />` or SVG) with no `aria-label`, `title`, or adjacent text. Users must memorize what each icon means through trial and error.
- **Cross-page memory dependency** — a flow where the user must remember an ID, reference number, or value from one page/screen to enter it on another, with no way to view both simultaneously. Look for multi-step forms or workflows where earlier selections are not summarized.
- **Placeholder-only form labels** — input fields using `placeholder` as the only label, with no persistent `<label>` element or floating label. Once the user starts typing, they lose context about what the field expects.
- **Hamburger menu hiding primary navigation on desktop** — primary nav items hidden behind a toggle/hamburger on viewports where there is ample horizontal space. Users must remember that navigation exists and recall how to open it.
- **No recent items or search history** — search or navigation features with no "recently viewed," "recent searches," or "frequently used" section. Users must remember and re-find items manually every time.
- **No autocomplete or suggestions** — search inputs or text fields where the system could offer suggestions or autocomplete based on available data but doesn't. Users must recall exact values from memory.
- **Settings without current values displayed** — configuration/settings pages where the current or default value is not shown alongside each option. Users must remember what they previously chose or leave the page to check.
- **Hidden navigation requiring path memorization** — navigation structures where reaching a feature requires knowing a specific sequence of clicks or menu paths, with no visible breadcrumbs, sitemap, or search.
- **Large dropdowns without search/filter** — `<select>` or custom dropdown components with many options (20+) and no search, filter, or grouping. Users must scroll through and recall which option they need.
- **Forms referencing external data without displaying it** — fields like "Enter your reference number" or "Enter the code from your email" without any means to view or retrieve that data from within the interface.

### What to Look For (Visual)

- Icons without text labels anywhere in the interface — toolbar buttons, action buttons, or navigation items showing only icons.
- No recently-viewed or frequently-used items section on dashboards, search pages, or landing pages.
- Form fields with only placeholder text and no persistent label visible — especially problematic when the field already has content.
- Complex navigation menus with deep nesting but no breadcrumbs or "you are here" indicator.
- Configuration or settings panels that don't display current/active values — only showing options to change without context.
- Dropdown or select lists with many options but no visible search or filter mechanism.

## Severity Guide

Rate each finding 0–4 using three factors:

| Factor | 4 — Katastrofe | 3 — Stort | 2 — Mindre | 1 — Kosmetisk |
|--------|----------------|-----------|------------|---------------|
| Frekvens | Every user encounters this in core workflow | Most users hit this regularly | Some users encounter it occasionally | Rare edge case |
| Innvirkning | User cannot complete task without memorizing info across pages — causes errors or task failure | User is significantly slowed down and must rely on recall instead of recognition | User is momentarily confused but finds the info after some effort | Slight inconvenience, user adapts quickly |
| Persistens | Happens every time, no workaround exists within the interface | Recurs frequently, workaround is cumbersome (e.g., writing things down) | Happens repeatedly but user learns patterns over time | One-time confusion that user resolves |

### Examples

- **Severity 4:** User must remember a reference code from page A to type into a field on page B, with no way to see both pages or copy the value — task fails if they misremember.
- **Severity 3:** Primary navigation hidden behind a hamburger menu on desktop with plenty of horizontal space — users forget where features live and waste time hunting.
- **Severity 2:** Icon-only toolbar with no tooltips — users must trial-and-error each icon to discover what it does, but eventually memorize the layout.
- **Severity 1:** Search field doesn't show recent searches — users must retype queries they've used before.
