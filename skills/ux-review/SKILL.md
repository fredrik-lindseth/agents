---
name: ux-review
description: "Usability evaluation against Jakob Nielsen's 10 heuristics. Spins up parallel agents — each evaluating through a different heuristic lens (visibility, real-world match, user control, consistency, error prevention, recognition, flexibility, minimalist design, error recovery, help & docs) — then distills all findings into prioritized action points using Nielsen's 0–4 severity scale."
args:
  - name: area
    description: The directory or area to review (optional)
    required: false
user-invokable: true
---

# UX Review

Launch parallel usability agents, each evaluating UI code and/or a running app through one of Jakob Nielsen's 10 usability heuristics, then distill all findings into unified, prioritized action points.

This skill evaluates **only** Nielsen's 10 heuristics — a focused usability evaluation. It does not cover accessibility (a11y/WCAG), performance, mobile responsiveness, or emotional design.

## Workflow

### Step 1: Choose Mode

Ask the user which mode they want:

- **Full** — Run all 10 heuristic agents in parallel, then distill.
- **Quick** — Run 4 grouped agents (heuristics combined per group), then distill. Faster.
- **Pick** — Let the user choose which heuristics to run.

If the user does not specify a mode, run Full mode automatically.

#### Quick Mode Groupings

In Quick mode, launch 4 agents instead of 10. Each agent receives multiple heuristic files:

| Group | Name | Heuristics |
|-------|------|------------|
| 1 | Feedback & Feil | H1 Visibility, H5 Error Prevention, H9 Error Recovery |
| 2 | Mental modell | H2 Real World Match, H6 Recognition, H10 Help & Docs |
| 3 | Kontroll & Konsistens | H3 User Control, H4 Consistency, H7 Flexibility |
| 4 | Estetikk | H8 Minimalist Design |

Each grouped agent reads all its assigned heuristic files and evaluates against all of them. The agent receives multiple `{heuristic_path}` values separated by newlines.

### Step 1.5: Choose Analysis Mode

Ask the user which analysis mode they want:

| Mode | What happens |
|------|-------------|
| **code** | Code analysis only — read components, CSS, routes, error handling |
| **visual** | Visual analysis only — navigate the app via Firefox MCP, take screenshots |
| **both** (default) | Code + visual analysis |

If the user does not specify, use **both**.

### Step 1.75: Choose Visual Scope

Only ask this if the analysis mode is **visual** or **both**:

| Scope | What happens |
|-------|-------------|
| **full** | Prescan discovers all routes from router config, navigates systematically |
| **target** | User specifies which pages/flows to inspect |

If the user chooses **target**, ask them to list the specific pages or user flows to inspect.

### Step 2: Determine Target

Ask the user (if not already clear from the invocation):

- **Path**: Which directory to review (default: current working directory)
- **App URL** (if visual/both): The URL of the running app (e.g., `http://localhost:5173`)
- **Focus** (optional): A specific area to concentrate on — e.g., `forms`, `navigation`, `search`, `onboarding`, `error-handling`. When set, agents spend ~3x more attention on this area.

### Step 2.5: Check for Existing Issue Tracker

Check if the project uses **dcat** — a local issue tracker (CLI tool). Run `which dcat`. If the command succeeds (exit code 0) AND a `.dogcats/` directory exists at the target path, run `dcat list --agent-only` to get tracked issues. Pass this issue list to each agent so they can skip concerns that are already tracked. If either check fails, skip this step.

### Step 3: Launch Prescan

Read `prescan.md` from this skill's directory (`~/.claude/skills/ux-review/prescan.md`).

Replace these placeholders:
- `{path}` — the target directory
- `{analysis}` — code/visual/both
- `{app_url}` — URL for visual inspection (or empty string if code-only)
- `{visual_scope}` — full/target (or empty string if code-only)
- `{target_flows}` — user-specified flows (or empty string)

Dispatch the prescan as an agent using the Agent tool. Wait for it to complete before proceeding.

The prescan agent outputs:
- `{file_catalog}` — structured file catalog of UI-relevant files
- `{screenshots}` — screenshots with URL, flow context, and state (or empty string if code-only)

### Step 4: Launch Heuristic Agents

Read `agent.md` from this skill's directory (`~/.claude/skills/ux-review/agent.md`).

**Placeholder resolution order:**

1. In `agent.md`: replace `{heuristic}` with the heuristic name (e.g., `h1-visibility`)
2. In `agent.md`: replace `{heuristic_path}` with the absolute path to the heuristic file
3. In `agent.md`: replace `{path}` with the target directory
4. In `agent.md`: replace `{app_url}` with the app URL (or empty string)
5. In `agent.md`: replace `{analysis}` with code/visual/both
6. In `agent.md`: replace `{visual_scope}` with full/target (or empty string)
7. In `agent.md`: replace `{file_catalog}` with the prescan file catalog output
8. In `agent.md`: replace `{screenshots}` with the prescan screenshots output (or empty string)
9. If the user specified a focus area, replace `{focus}` with the focus block below. If no focus was specified, replace `{focus}` with an empty string.
10. If dcat issues were found, replace `{known_issues}` with a `## Known Issues (skip these)` section listing them. Otherwise replace with an empty string.
11. Pass the result as the agent prompt

**Focus block** (inserted when focus is set):
```
## Focus Area: {area}

Concentrate your analysis primarily on **{area}**. During the scan, go deeper on {area}-related aspects (read more files, check more patterns, inspect more screenshots). In your findings, {area}-related issues should be thoroughly covered — don't just flag them, explain the specific impact.

Other issues are still worth mentioning but give {area} roughly 3x the attention and depth.
```

**Heuristic-to-file mapping:**

| Heuristic | Name | File |
|-----------|------|------|
| H1 | Visibility of System Status | `~/.claude/skills/ux-review/heuristics/h1-visibility.md` |
| H2 | Match Between System and Real World | `~/.claude/skills/ux-review/heuristics/h2-real-world-match.md` |
| H3 | User Control and Freedom | `~/.claude/skills/ux-review/heuristics/h3-user-control.md` |
| H4 | Consistency and Standards | `~/.claude/skills/ux-review/heuristics/h4-consistency.md` |
| H5 | Error Prevention | `~/.claude/skills/ux-review/heuristics/h5-error-prevention.md` |
| H6 | Recognition Rather Than Recall | `~/.claude/skills/ux-review/heuristics/h6-recognition.md` |
| H7 | Flexibility and Efficiency of Use | `~/.claude/skills/ux-review/heuristics/h7-flexibility.md` |
| H8 | Aesthetic and Minimalist Design | `~/.claude/skills/ux-review/heuristics/h8-minimalist-design.md` |
| H9 | Help Users Recognize, Diagnose, and Recover from Errors | `~/.claude/skills/ux-review/heuristics/h9-error-recovery.md` |
| H10 | Help and Documentation | `~/.claude/skills/ux-review/heuristics/h10-help-docs.md` |

All paths resolve to `~/.claude/skills/ux-review/heuristics/{heuristic}.md`.

**Mode-specific dispatch:**

- **Full mode**: Launch all 10 agents in parallel, one per heuristic.
- **Quick mode**: Launch 4 agents in parallel, one per group. Each grouped agent receives multiple heuristic file paths (separated by newlines) in `{heuristic_path}` and a combined group name in `{heuristic}`.
- **Pick mode**: Launch only the user-selected heuristics as individual agents.

### Step 5: Distill

After all agents complete, read `distill.md` from this skill's directory (`~/.claude/skills/ux-review/distill.md`) and follow the distillation algorithm.

## Severity Scale (Nielsen 0–4)

All heuristic agents use Nielsen's severity rating. Severity is determined by three factors:

- **Frekvens** — Does every user encounter this, or is it an edge case?
- **Innvirkning** — Does it block the user, or is it merely annoying? Can they work around it?
- **Persistens** — Is it a one-time issue the user learns past, or does it recur every time?

| Level | Label | Description | Priority |
|-------|-------|-------------|----------|
| 0 | Ikke et problem | Not a usability problem | Skip |
| 1 | Kosmetisk | Fix only if extra time is available | Skip |
| 2 | Mindre | Minor usability problem, low priority | Consider |
| 3 | Stort | Major usability problem, high priority | Should Address |
| 4 | Katastrofe | Usability catastrophe, must fix before release | Fix Now |

Individual heuristic files refine these levels with concrete examples for their domain. During distillation, use these universal definitions as the baseline.

## Error Handling

- If Firefox MCP is not available (tools not found, connection fails), automatically fall back to **code-only** analysis mode. Warn the user that visual analysis was skipped.
- If a heuristic file does not exist at the expected path, skip that heuristic and warn the user.
- If all agents return zero findings, output "No issues found" and skip the distill step.
- If some agents fail or timeout, distill with available results and note which heuristics were skipped.
- If `git ls-files` fails (not a git repo, permissions), fall back to `find {path} -type f` and filter by extension.

## Rules

- In Full mode, run all 10 agents in parallel for maximum throughput.
- Each agent scans independently — do not pass pre-scanned data between agents (they all receive the same prescan output, but scan code independently).
- Always run the distill step after all agents complete — raw agent output is overwhelming without deduplication and prioritization.
- The prescan must complete before launching heuristic agents (they depend on its file catalog and screenshots).
