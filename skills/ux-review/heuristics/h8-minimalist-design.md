# H8: Aesthetic and Minimalist Design

## Definition

"Interfaces should not contain information that is irrelevant or rarely needed. Every extra unit of information in an interface competes with the relevant units of information and diminishes their relative visibility." — Jakob Nielsen, NN/g

## What to Look For

### Code Patterns

- **Dashboard overloaded with metrics/widgets** — dashboard rendering 10+ cards, charts, or metrics when users typically only check 2–3. No progressive disclosure, collapsible sections, or "show more" pattern. Everything competes for attention equally.
- **Persistent onboarding/marketing content** — welcome banners, feature announcements, or marketing text that remain visible after first use, taking prime screen real estate away from task-relevant content. Look for missing dismissal logic or "seen" state persistence.
- **All table columns shown by default** — tables rendering every available column with no column picker, no sensible default column set, and no responsive hiding. Users must scan through irrelevant columns to find what matters.
- **Redundant labels** — section headers that repeat the same information as field labels within them (e.g., section "User Settings" containing "User Name," "User Email," "User Role"). Look for label text that adds no information beyond what the parent section already communicates.
- **Excessive visual chrome** — heavy use of borders, shadows, dividers, card outlines, and decorative elements where simpler visual separation (whitespace, subtle background differences) would suffice. Look for multiple layered shadow/border utilities in Tailwind or nested container elements with borders.
- **No information hierarchy** — low-priority details rendered with the same font size, weight, color, and spacing as critical information. Look for flat component structures where metadata, timestamps, IDs, and primary content share identical styling.
- **Decorative elements consuming space** — illustrations, hero images, icons, or graphics that take significant space without aiding task completion. Check for large SVGs or images in task-oriented screens that serve no functional purpose.
- **Multiple competing CTAs** — several call-to-action buttons with equal visual weight on the same screen. Look for multiple primary-styled buttons (`variant="default"` or primary color) where only one action is the main intended path.
- **Verbose instructions that could be progressive** — long blocks of explanatory text rendered inline and always visible, when a tooltip, expandable section, or "Learn more" link would serve occasional needs without cluttering the default view.
- **Rarely-used features given equal prominence** — secondary or advanced features occupying the same visual weight and position as primary features. Look for settings toggles, advanced options, or rarely-clicked actions placed alongside core functionality without visual differentiation.

### What to Look For (Visual)

- Cluttered layout with no clear visual hierarchy — everything appears equally important, making it hard to identify the primary content or action.
- Low signal-to-noise ratio — user must scan through irrelevant information to find what they need. Key data is lost among secondary details.
- Insufficient whitespace or cramped layout — elements packed tightly with no visual breathing room, making the interface feel dense and overwhelming.
- Rarely-needed information competing with primary content — timestamps, metadata, technical IDs, or administrative info displayed as prominently as core content.
- Excessive decoration that doesn't serve the user's task — ornamental borders, heavy shadows, background patterns, or large illustrations on task-focused screens.
- Multiple buttons or links with equal visual emphasis — user can't quickly identify the primary action.

## Severity Guide

Rate each finding 0–4 using three factors:

| Factor | 4 — Katastrofe | 3 — Stort | 2 — Mindre | 1 — Kosmetisk |
|--------|----------------|-----------|------------|---------------|
| Frekvens | Every user sees this on the primary screen every session | Most users encounter it on key pages | Some users notice it on secondary pages | Rare edge case or barely noticeable |
| Innvirkning | User cannot find critical action or information due to visual noise — task completion is blocked or severely delayed | User is significantly slowed down scanning through irrelevant content to find what matters | User is mildly distracted but locates information with some effort | Slight visual annoyance, no functional impact |
| Persistens | Permanent fixture on a primary page, user encounters it every time | Appears on frequently visited pages with no way to dismiss or customize | Appears regularly but user learns to ignore it over time | One-time or occasional distraction |

### Examples

- **Severity 4:** Critical action (submit, pay, confirm) is buried among dozens of equally-styled elements on the page — users cannot find it and may abandon the task entirely.
- **Severity 3:** Dashboard overwhelms with 15 metrics, charts, and widgets when users only need 3 — key information is lost in the noise and finding it requires extensive scanning.
- **Severity 2:** Marketing banner persists after onboarding, consuming 20% of screen space on every visit — users cannot dismiss it and must scroll past it to reach their content.
- **Severity 1:** Decorative border and shadow applied to every card when a flat design with whitespace separation would be cleaner and less visually heavy.
