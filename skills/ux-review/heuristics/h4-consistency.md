# H4: Consistency and Standards

## Definition

"Users should not have to wonder whether different words, situations, or actions mean the same thing. Follow platform and industry conventions." — Jakob Nielsen, NN/g

## What to Look For

### Code Patterns

- **Same action, different labels** — the same operation uses "Save" in one view, "Submit" in another, "Confirm" in a third, and "OK" in a dialog. Search for button text across components to find label inconsistencies for equivalent actions.
- **Inconsistent button styling for same action type** — primary action is blue on one page, green on another; destructive action uses red in one modal but default styling in another. Check for inconsistent use of `variant="destructive"` vs `variant="default"` for delete/remove actions.
- **Mixed capitalization in headings and labels** — Title Case in some headings, sentence case in others, ALL CAPS in yet another section. Search headings (`<h1>`–`<h6>`, `<Label>`, section titles) for inconsistent casing patterns.
- **Date format inconsistency** — `3. april 2026` in one view, `2026-04-03` in another, `03/04/2026` in a third. Search for date formatting functions, `toLocaleDateString`, `format()`, `dayjs`, `date-fns` calls to identify inconsistent format strings.
- **Color semantics inconsistency** — red means "error" in one component but is used as decorative accent in another; green means "success" in forms but "active" in status badges. Check CSS custom properties and Tailwind classes for semantic color usage.
- **Link styling varies** — some links are underlined, some rely on color alone, some look like plain text. Check for inconsistent use of `<a>` styling, `underline`, `hover:underline`, or `text-blue-*` classes.
- **Inconsistent spacing/padding** — similar card components use `p-4` in one place and `p-6` in another; list items have `gap-2` in one list and `gap-4` in another. Look for similar components with different spacing values.
- **Mixed icon sets or styles** — outline icons in some components, filled icons in others; different icon sizes (16px, 20px, 24px) used inconsistently; icons from different libraries (Lucide + Heroicons + FontAwesome).
- **Inconsistent form patterns** — labels above inputs in some forms, labels inline (placeholder-as-label) in others; some forms use floating labels, others don't. Look for different `<Label>` positioning across form components.
- **Inconsistent empty states** — some lists show a friendly "No items yet" message with an illustration, while others show a blank area or a generic "No data" string.
- **Inconsistent table patterns** — some tables have sortable columns, others don't; some show row counts, others don't; pagination styles differ between views.

### What to Look For (Visual)

- Different button styles (color, size, border-radius, shadow) for the same type of action across different pages or sections.
- Typography hierarchy inconsistency — heading sizes, font weights, or line heights that don't follow a consistent scale.
- Spacing and grid inconsistency — noticeably different padding, margins, or gap values between similar components on different pages.
- Non-standard placement of common elements — search bar in the header on one page, in the sidebar on another; login button in different corners.
- Mixed visual language — rounded corners in some components, sharp corners in others; shadows on some cards, flat on others.
- Inconsistent data display — the same data type (dates, currency, status) formatted differently across views.

## Severity Guide

Rate each finding 0–4 using three factors:

| Factor | 4 — Katastrofe | 3 — Stort | 2 — Mindre | 1 — Kosmetisk |
|--------|----------------|-----------|------------|---------------|
| Frekvens | Inconsistency appears on every page or interaction | Appears in multiple common workflows | Noticeable in a few places | Isolated to one rarely-visited page |
| Innvirkning | User can't identify primary actions or misinterprets UI — causes errors | User is confused, hesitates, or has to re-learn patterns per page | User notices the inconsistency but isn't confused | Slight visual imperfection, no functional impact |
| Persistens | User encounters it every session, can't predict behavior | Recurs regularly, user has to re-adapt | Happens occasionally, user adjusts after noticing | One-time observation |

### Examples

- **Severity 4:** Primary action button (save, submit, confirm) styled completely differently on each page — different colors, sizes, and positions. Users can't identify what to click to proceed.
- **Severity 3:** Date format changes between views — "3. april 2026" on the detail page vs "04/03/2026" on the list page — creating genuine confusion about which date is which.
- **Severity 2:** Inconsistent capitalization in section headings — "Eiendomsinformasjon" (sentence case) in one section, "Arealplan Detaljer" (Title Case) in another.
- **Severity 1:** Slightly different padding between cards on different pages — `p-4` on one page, `p-5` on another. Noticeable if comparing side-by-side but not in normal use.
