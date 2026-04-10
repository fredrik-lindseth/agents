# agents

Reusable AI agent skills and personas for Claude Code and OpenCode.

| Name | Type | What it gives you |
|------|------|-------------------|
| [codehealth](skills/codehealth/SKILL.md) | skill | Full code quality audit — 12 parallelle agenter som distilleres til en prioritert actionliste |
| [dba](skills/dba/SKILL.md) | skill | Database-spesialist — 12 SQL-fokuserte linser (injection, N+1, schema-drift, indekser, etc.) |
| [fuzz-my-stuff-up](skills/fuzz-my-stuff-up/SKILL.md) | skill | Adversarial testing — ~34 agenter prøver å knekke koden fra alle vinkler |
| [scrutinize](skills/scrutinize/SKILL.md) | skill | Stresstest via simulerte community-reaksjoner (Reddit, HN, Twitter, /g/, etc.) |
| [sweep](skills/sweep/SKILL.md) | skill | Design-review — 10 parallelle linser (a11y, UX, polish, typografi, farge, ytelse) |
| [ux-review](skills/ux-review/SKILL.md) | skill | Nielsens 10 heuristikker — parallelle agenter evaluerer brukervennlighet |
| [should-i-abstract](skills/should-i-abstract/SKILL.md) | skill | DRY-analyse som finner både kode som bør deles og abstraksjoner som bør inlines |
| [challenge-me](skills/challenge-me/SKILL.md) | skill | Rask sanity-check — Sonnet-agent gir GOOD/TWEAK/RETHINK-verdict |
| [audit-docs](skills/audit-docs/SKILL.md) | skill | Auditerer CLAUDE.md/AGENTS.md for redundanser, motsetninger og gaps |
| [envision](skills/envision/SKILL.md) | skill | North-star visjonsdokument uten begrensninger |
| [ove](skills/ove/SKILL.md) | skill | Python-prosjekt med moderne 2025+ tooling (uv, ruff, pyright) |
| [test-lead](skills/test-lead/SKILL.md) | skill | Planlegger teststrategi og driver implementering av en lagdelt test suite |
| [hakon](agents/hakon.md) | agent | Streng infra-reviewer — validerer Linux, Ansible, Docker, K8s, sikkerhet |
| [claude-delegate](scripts/claude-delegate) | script | Cross-repo agent-kommunikasjon via `claude -p` |

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
