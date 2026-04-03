## Distillation Algorithm

After all agents complete, analyze the combined output:

1. Read through every finding from every agent and classify:
   - **Task blocker**: Users cannot complete their task — the UI is broken, unresponsive, or misleading enough to prevent task completion
   - **Friction**: Users can complete but with significant difficulty — confusing flows, hidden controls, unclear feedback
   - **Annoyance**: Users notice but work around it easily — minor inconsistencies, suboptimal defaults
   - **Cosmetic**: Only noticed upon close inspection — alignment, spacing, minor wording
   - **Noise**: Subjective preference or not a real problem — skip

2. Cross-reference with source: read only files explicitly referenced in findings (one per finding) to confirm line accuracy and that the issue still exists. For visual-only findings (screenshot-based, no file reference), trust the screenshot evaluation without code verification. Do not scan for additional patterns during distillation.

3. Deduplicate using the following algorithm:
   - **Pass 1 — File match**: Group findings that reference the same file and line range (within 10 lines). These are almost certainly the same issue.
   - **Pass 2 — Pattern match**: Within the same file, merge findings that describe the same type of problem (e.g., two heuristics both flagging "missing loading indicator" or "no error feedback" in the same component).
   - **Pass 3 — Semantic match**: Across different files, merge findings that describe the same systemic issue (e.g., "no undo capability" flagged by H3 pointing at a form and H9 pointing at a delete action).
   - After merging, mark cross-heuristic consensus with "flagget av N/{total} heuristikker" where {total} is the number of heuristic agents run.
   - Extract the `## Findings Summary` table from each agent's output as the primary dedup input. Normalize across different table schemas by extracting the common columns: Severity, File:Line, Issue, Suggestion.
   - **Conflict resolution**: When two heuristics disagree on the same element (e.g., H8 says "remove element" but H6 says "keep it visible for recognition"), flag the disagreement explicitly: `[CONFLICT] H{n} recommends X, H{m} recommends Y — resolved by: {reasoning}`. Resolution hierarchy: task completion > error prevention > learnability > efficiency > aesthetics.

4. Output as:

```
## UX Review Results

### 4 — Usability-katastrofe
Kritiske problemer som hindrer brukere i a fullfoere oppgaver.

1. [ ] **{tittel}** — {beskrivelse}
   `{file:line}` | {code/visual/both} | H{n}: {heuristikk} | {what to change}

### 3 — Stort usability-problem
Viktige problemer som skaper betydelig friksjon.

2. [ ] **{tittel}** — {beskrivelse}
   `{file:line}` | {code/visual/both} | H{n}: {heuristikk} | {what to change}

### 2 — Mindre usability-problem
Reelle problemer, men lavere prioritet.

3. [ ] **{tittel}** — {beskrivelse}
   `{file:line}` | {code/visual/both} | H{n}: {heuristikk} | {what to change}

### Skipped
{count} funn var kosmetiske (0-1) eller noise — ignorert.

## Utenfor scope

Denne reviewen dekker Nielsens 10 usability-heuristikker. For andre aspekter:
- Tilgjengelighet (a11y, WCAG) -> /audit
- Ytelse og loading -> /optimize
- Responsivt design -> /adapt
- Kodearkitektur -> /codehealth
```

Rules for distilling:
- **Number items sequentially across all sections** (1, 2, 3... not restarting per section) so the user can reference them by number
- If dcat issues were found earlier, exclude any action point that overlaps with an existing tracked issue
- Each item must have a file path or screenshot reference — no vague suggestions
- Each item must note which heuristic found it (H{n}: {name}). If multiple heuristics flagged the same issue, list all with consensus count
- Each item must note source type: `code`, `visual`, or `both`
- One line per fix — say what to change concretely
- No duplicates — if multiple heuristics flagged the same thing, merge into one item with consensus count
- Severity is based on user impact using Nielsen's three factors (frekvens, innvirkning, persistens), not how many heuristics mentioned it
- The "Utenfor scope" section is always included at the end

After outputting, ask the user if they want to start working on any of the items.
