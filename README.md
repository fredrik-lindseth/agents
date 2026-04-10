# agents

Reusable AI agent skills and personas for Claude Code and OpenCode.

## Skills

### Code quality

| Skill | What it gives you |
|-------|-------------------|
| [codehealth](skills/codehealth/SKILL.md) | Full code quality audit — 12 parallelle agenter (duplicates, complexity, naming, error-gaps, etc.) som distilleres til en prioritert actionliste |
| [dba](skills/dba/SKILL.md) | Database-spesialist — 12 SQL-fokuserte linser (injection, N+1, schema-drift, transaksjoner, indekser, etc.) for prosjekter med relasjonsdatabaser |
| [fuzz-my-stuff-up](skills/fuzz-my-stuff-up/SKILL.md) | Adversarial testing — ~34 agenter prøver å knekke koden din fra alle vinkler (tomme inputs, unicode, race conditions, injection, state-abuse) |
| [should-i-abstract](skills/should-i-abstract/SKILL.md) | DRY-analyse som går begge veier — finner både kode som bør deles OG abstraksjoner som bør inlines tilbake |
| [scrutinize](skills/scrutinize/SKILL.md) | Stresstest via simulerte community-reaksjoner (Reddit, HN, Twitter, /g/, Stack Overflow, etc.) — action points destillert fra 10 parallelle personas |
| [sweep](skills/sweep/SKILL.md) | Design-review — 10 parallelle linser (a11y, UX, polish, typografi, farge, ytelse, etc.) som gir en prioritert fiks-liste med skill-anbefalinger |
| [ux-review](skills/ux-review/SKILL.md) | Nielsens 10 heuristikker — parallelle agenter evaluerer brukervennlighet og rangerer funn etter 0-4 severity |
| [challenge-me](skills/challenge-me/SKILL.md) | Rask sanity-check — en Sonnet-agent utfordrer tilnærmingen din og gir GOOD/TWEAK/RETHINK-verdict |

### Code quality (sub-skills brukt av codehealth)

| Skill | What it gives you |
|-------|-------------------|
| [complexity](skills/complexity/SKILL.md) | Finner lange funksjoner, dyp nesting og høy branching |
| [dead-code](skills/dead-code/SKILL.md) | Finner ubrukte funksjoner, unreachable branches og orphaned filer |
| [dep-hygiene](skills/dep-hygiene/SKILL.md) | Finner ubrukte imports, unødvendige pakker og utdaterte dependencies |
| [duplicates](skills/duplicates/SKILL.md) | Finner copy-paste-kode og nesten identiske blokker |
| [error-gaps](skills/error-gaps/SKILL.md) | Finner manglende, slukt eller inkonsistent feilhåndtering |
| [extract-logic](skills/extract-logic/SKILL.md) | Finner inline-operasjoner som burde vært funksjoner eller service-lag |
| [hardcoded](skills/hardcoded/SKILL.md) | Finner magic strings, tall, URLer og credentials i koden |
| [naming](skills/naming/SKILL.md) | Finner inkonsistente navnekonvensjoner og tvetydige identifiers |
| [query-smells](skills/query-smells/SKILL.md) | Finner N+1-queries, rå SQL i loops og manglende parameterisering |
| [simplify-code](skills/simplify-code/SKILL.md) | Finner over-engineered løsninger og unødvendige abstraksjoner |
| [test-gaps](skills/test-gaps/SKILL.md) | Finner kritiske kodestier som mangler testdekning |
| [type-structs](skills/type-structs/SKILL.md) | Finner raw dicts/lists som burde vært dataclasses eller typed structures |

### Documentation & review

| Skill | What it gives you |
|-------|-------------------|
| [audit-docs](skills/audit-docs/SKILL.md) | Auditerer CLAUDE.md, AGENTS.md og copilot-instructions for redundanser, motsetninger, gaps og misplassert innhold |

### Project setup

| Skill | What it gives you |
|-------|-------------------|
| [envision](skills/envision/SKILL.md) | Beskriver den ideelle versjonen av prosjektet — et north-star visjonsdokument uten begrensninger |
| [ove](skills/ove/SKILL.md) | Setter opp nye Python-prosjekter eller migrerer eksisterende til moderne 2025+ tooling (uv, ruff, pyright) |
| [test-lead](skills/test-lead/SKILL.md) | Planlegger teststrategi, teststruktur og driver implementering av en lagdelt test suite |

## Agents

| Agent | What it gives you |
|-------|-------------------|
| [hakon](agents/hakon.md) | Streng infra-reviewer — validerer Linux, Ansible, Docker, Kubernetes, sikkerhet og nettverk med nulltoleranse |

## Scripts

| Script | What it gives you |
|--------|-------------------|
| [claude-delegate](scripts/claude-delegate) | Cross-repo agent-kommunikasjon — delegerer oppgaver mellom Claude Code-sesjoner via `claude -p` |

## Usage

### Claude Code

Symlink skills og agents inn i `~/.claude/`:

```bash
# Skills (eksempel)
ln -s ~/dev/agents/skills/codehealth ~/.claude/skills/codehealth
ln -s ~/dev/agents/skills/dba ~/.claude/skills/dba

# Agents
ln -s ~/dev/agents/agents/hakon.md ~/.claude/agents/hakon.md
```

### claude-delegate

Installer scriptet og konfigurer agent-registeret:

```bash
# Kopier til PATH
cp scripts/claude-delegate ~/bin/claude-delegate
chmod +x ~/bin/claude-delegate

# Rediger REGISTRY-blokken i scriptet med dine repoer:
# repo-navn|/sti/til/repo|beskrivelse

# Tillat i Claude Code settings (~/.claude/settings.json):
# "allow": ["Bash(/Users/<bruker>/bin/claude-delegate *)"]

# Legg til i ~/.claude/CLAUDE.md så alle agenter vet om det
```

Bruk fra en Claude Code-sesjon:

```bash
~/bin/claude-delegate --list                              # Vis agenter
~/bin/claude-delegate serveren "deploy eiendomsoppslag"   # Send oppgave
```

Se [docs/claude-delegate.md](https://forgejo.serveren.0v.no/fredrik/serveren/src/branch/main/docs/claude-delegate.md) for full dokumentasjon.

### OpenCode

Copy agents into your OpenCode agents directory and adjust the frontmatter format as needed.
