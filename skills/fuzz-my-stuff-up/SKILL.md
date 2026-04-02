---
name: fuzz-my-stuff-up
description: "Adversarial code exploration. Spins up ~34 parallel agents — each trying to break the code from a different angle (empty inputs, unicode chaos, race conditions, injection, state machine abuse, crypto misuse, serialization attacks, supply chain, etc.) — then distills all findings into prioritized action points."
args:
  - name: area
    description: The directory, feature, or component to fuzz (optional)
    required: false
user-invokable: true
---

# Fuzz My Stuff Up

Launch ~34 parallel adversarial agents, each trying to break the codebase from a different angle — like fuzzing but with reasoning. Each agent thinks like an attacker, a confused user, a hostile environment, or an edge-case machine. Then distill all findings into prioritized action points.

## Workflow

### Step 1: Choose Mode

Ask the user which mode they want:

- **Full** — Run all 34 fuzzers in parallel, then distill. Maximum chaos.
- **Quick** — Run 7 high-impact fuzzers (empty-inputs, type-confusion, injection, state-machine, malformed-input, api-abuse, adversarial-user), then distill. Faster.
- **Pick** — Let the user choose which fuzzers to run.

Available fuzzers:

| Fuzzer | Attack Angle |
|--------|-------------|
| empty-inputs | Empty strings, null, None, zero, empty arrays, missing keys |
| boundary-values | INT_MAX, INT_MIN, huge strings, negative numbers, off-by-one |
| type-confusion | Wrong types where duck typing or loose validation allows it |
| unicode-chaos | Emoji, RTL text, zero-width chars, combining marks, mojibake |
| concurrency | Race conditions, parallel calls, shared mutable state, TOCTOU |
| path-traversal | `../` escape, symlinks, special paths, null bytes in filenames |
| injection | SQL, command, template, header injection vectors |
| state-machine | Out-of-order operations, double-submit, re-entry, partial completion |
| resource-exhaustion | Huge payloads, deeply nested structures, quadratic blowup, infinite loops |
| time-travel | Timezone edge cases, DST transitions, leap seconds, clock skew, far-future dates |
| permission-escalation | Accessing resources across privilege boundaries, role bypass, IDOR |
| malformed-input | Invalid JSON, truncated data, wrong encoding, BOM, mixed line endings |
| network-failure | Timeout handling, partial responses, DNS failure, retry storms |
| config-chaos | Missing config keys, wrong types in config, env var conflicts, defaults |
| dependency-failure | External service down, wrong version responses, missing optional deps |
| locale-chaos | Different number formats, date formats, currency symbols, RTL layouts |
| filesystem-edge | Read-only fs, full disk, long paths, special chars in filenames, case sensitivity |
| api-abuse | Missing required fields, extra unknown fields, wrong HTTP methods, huge headers |
| upgrade-path | Data from old versions, schema drift, backwards compat gaps, migration holes |
| adversarial-user | Intentionally hostile inputs, CSRF scenarios, replay attacks, parameter tampering |
| serialization-attacks | Insecure deserialization, pickle bombs, YAML anchor bombs, prototype pollution, JSON parser differential quirks |
| floating-point-chaos | NaN, Infinity, -0, precision loss, float comparison gotchas, rounding errors in money/accumulation |
| regex-dos | Catastrophic backtracking (ReDoS), exponential regex patterns, untrusted input in regex compilation |
| error-leakage | Stack traces in responses, verbose errors exposing internals, version/path/schema disclosure via errors |
| crypto-misuse | Weak/predictable randomness, hardcoded keys/IVs, timing side-channels, nonce reuse, insecure hashing |
| log-injection | CRLF injection in logs, log forging, sensitive data in log lines, log-based XSS in log viewers |
| cache-poisoning | Cache key collisions, stale data serving, unbounded cache growth, cache timing side-channels |
| signal-interrupt | SIGTERM during writes, signal races, unhandled signals, graceful shutdown gaps, partial cleanup |
| supply-chain | Dependency confusion, typosquatting in imports, lock file manipulation, vendored code integrity, private vs public package name collision |
| resource-leak | Unclosed file handles, DB connections not returned to pool, event listeners never removed, growing collections that never shrink |
| reflection-abuse | getattr/eval/exec/__import__ chains, dynamic dispatch, runtime code generation, Class.forName, prototype chain walking |
| data-truncation | Silent data loss from DB column limits, string slicing, lossy type conversions (float→int), precision loss in aggregation |
| encoding-roundtrip | Data that survives encode but breaks on decode→re-encode cycles: URL encoding, base64 padding, HTML entities, escape nesting, double-encoding |
| event-ordering | Messages arriving out of order, dropped events, duplicate delivery, at-least-once vs exactly-once semantics, async race conditions |

Default to **Full** if the user doesn't specify.

### Scope Boundaries

Some fuzzers examine similar code from different angles. When findings overlap:
- **path-traversal** owns escape sequences (`../`, null bytes, symlink abuse). **filesystem-edge** owns OS-level limits (long paths, case sensitivity, full disk, read-only fs). Both may flag the same file-handling code — path-traversal focuses on malicious paths, filesystem-edge on environmental constraints.
- **injection** owns attacker-crafted payloads (SQL, command, template injection). **malformed-input** owns accidentally broken data (truncated JSON, wrong encoding). If input is both malformed AND injectable, injection takes precedence.
- **empty-inputs** and **type-confusion** both probe validation gaps. empty-inputs focuses on absence (null, zero, empty), type-confusion focuses on wrong-type presence. If the same validation function is missing both checks, empty-inputs takes the finding.
- **api-abuse** owns HTTP/API-level issues (wrong methods, missing fields). **adversarial-user** owns cross-request attacks (CSRF, replay, parameter tampering). If a finding spans both, adversarial-user takes precedence.
- **injection** owns attacker payloads entering through application logic. **log-injection** owns payloads targeting log infrastructure (CRLF forging, log viewer XSS). If a payload is injectable AND logs it unsafely, log-injection takes the finding.
- **boundary-values** owns integer extremes and string size. **floating-point-chaos** owns IEEE 754 special values (NaN, Infinity, -0, precision loss). If a numeric boundary involves float precision, floating-point-chaos takes the finding.
- **resource-exhaustion** owns payload size and algorithmic complexity. **regex-dos** owns catastrophic backtracking specifically in regex patterns. If a regex is also being fed a huge payload, regex-dos takes the finding.
- **malformed-input** owns broken/corrupt data formats. **serialization-attacks** owns deliberately crafted valid-looking serialized data designed to exploit deserialization (pickle RCE, YAML anchors, prototype pollution). If data is both malformed and weaponized, serialization-attacks takes precedence.
- **config-chaos** owns missing/wrong config values. **crypto-misuse** owns cryptographic configuration errors (weak algorithms, hardcoded keys, insecure defaults). If a config value controls crypto behavior, crypto-misuse takes the finding.
- **error-leakage** owns information disclosed through error responses. **error-gaps** (the separate skill) covers missing error handling. error-leakage focuses on what errors *reveal*, not whether they're *handled*.
- **concurrency** owns race conditions between parallel operations. **signal-interrupt** owns OS-level signal handling (SIGTERM, SIGHUP) and graceful shutdown. If a signal arrives during a concurrent operation, signal-interrupt takes the finding.
- **resource-exhaustion** owns unbounded growth in general. **cache-poisoning** owns cache-specific issues (key collisions, stale data, unbounded cache). If a cache grows unboundedly, cache-poisoning takes the finding.
- **dependency-failure** owns external services being down or returning wrong versions. **supply-chain** owns malicious or confused dependency resolution (typosquatting, name collision, lock file tampering). If a dependency is *wrong* rather than *unavailable*, supply-chain takes the finding.
- **resource-exhaustion** owns sudden blowup from large payloads. **resource-leak** owns slow accumulation over time (unclosed handles, unreturned connections, growing collections). If the leak requires many requests to manifest, resource-leak takes the finding.
- **injection** owns SQL/command/template payloads. **reflection-abuse** owns dynamic language features used as attack vectors (eval, getattr, __import__, runtime dispatch). If an injection payload reaches an eval/exec call, reflection-abuse takes the finding.
- **boundary-values** owns numeric extremes. **data-truncation** owns silent data loss where values fit but lose content (column limits, lossy conversions, precision loss in aggregation). If data is silently shortened rather than rejected, data-truncation takes the finding.
- **unicode-chaos** owns Unicode-specific encoding issues. **encoding-roundtrip** owns encode→decode→re-encode degradation across any encoding scheme (URL, base64, HTML entities). If data round-trips through multiple encodings and degrades, encoding-roundtrip takes the finding.
- **state-machine** owns API call ordering. **event-ordering** owns message/event system ordering (duplicate delivery, dropped events, async reordering). If the ordering issue involves a message queue or event bus rather than direct API calls, event-ordering takes the finding.

### Step 2: Determine Target

Ask the user (if not already clear):
- **Path**: Which directory to fuzz (default: current working directory)
- **Focus** (optional): A specific feature, endpoint, or module to concentrate on — e.g., `auth`, `api`, `file-upload`, `payments`, `user-input`. When set, fuzzers spend ~3x more attention on this area.

### Step 3: Language Prescan

Detect which languages are in scope so agents fuzz all of them:

1. Run `git ls-files` in the target path (or cwd) and group files by extension
2. Map extensions to languages
3. Skip binary/asset files: `*.png`, `*.jpg`, `*.gif`, `*.svg`, `*.ico`, `*.woff*`, `*.ttf`, `*.lock`, `*.min.js`, `*.min.css`, and directories `node_modules/`, `vendor/`, `dist/`, `build/`
4. Present the detected languages to the user sorted by file count
5. Ask: "These the right languages? Any that need extra attention despite low file count?"
6. Pass the confirmed list to each agent

### Step 4: Check for Existing Issue Tracker

Check if the project uses **dcat**. Run `which dcat`. If the command succeeds AND a `.dogcats/` directory exists at the target path, run `dcat list --agent-only` to get tracked issues. Pass this list to each agent so they skip already-known problems. If either check fails, skip this step.

### Step 5: Launch Agents

Read `fuzzer-agent.md` from this skill's directory. For each selected fuzzer:

1. Replace `{fuzzer}` with the fuzzer name (e.g., `empty-inputs`)
2. Replace `{attack_angle}` with the attack angle description from the table above
3. Replace `{path}` with the target path
4. Replace `{languages}` with the confirmed language list
5. If the user specified a focus area, replace `{focus}` with the focus block below, replacing `{area}` within it with the user's specified area. If no focus was specified, replace `{focus}` with empty string.
6. If dcat issues were found, replace `{known_issues}` with a `## Known Issues (skip these)` section listing them. Otherwise replace with empty string.
7. Launch all agents in parallel using the Agent tool

**Focus block** (inserted when focus is set — replace `{area}` with the user's focus area):
```
## Focus Area: {area}

Concentrate your fuzzing primarily on **{area}**. Go deeper on {area}-related code paths (read more files, try more attack patterns). {area}-related findings should be thoroughly explored — trace how deep the vulnerability or gap goes.

Other areas are still worth probing but give {area} roughly 3x the attention.
```

**Launch all selected fuzzers in a single parallel batch.** Sequential launching wastes time and prevents detection of timing-related issues.

### Step 6: Distill

After all agents complete, read `distill.md` from this skill's directory and follow the distillation algorithm.

### Error Handling

- If `git ls-files` fails (not a git repo, permissions), fall back to `find {path} -type f` and filter by extension.
- If an agent returns zero findings, that is a valid result — note "{fuzzer}: no issues found" in the distill summary.
- If some agents fail or timeout, distill with available results and note which fuzzers were skipped.

## Rules

- **Launch all selected fuzzers in a single parallel batch.** Sequential launching prevents timeout-related race condition detection and wastes time.
- **Each agent discovers the codebase from scratch and probes for vulnerabilities independently.** Pre-scan context (languages, path, known issues) is passed to all agents, but agents do not share findings mid-run — shared vulnerability data biases results and hides attack surfaces.
- **Agents analyze code without modifying files.** This preserves codebase integrity and lets the user review findings before acting.
- **Run distillation only after all agents complete.** Distillation needs the full picture to deduplicate and prioritize accurately.
