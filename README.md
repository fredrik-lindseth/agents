# agents

Reusable AI agent skills and personas for Claude Code and OpenCode.

### Code quality & testing

| Name | What it gives you |
|------|-------------------|
| [codehealth](skills/codehealth/SKILL.md) | Full code quality audit — 12 parallelle agenter som distilleres til en prioritert actionliste |
| [dba](skills/dba/SKILL.md) | Database-spesialist — 12 SQL-fokuserte linser (injection, N+1, schema-drift, indekser, etc.) |
| [fuzz-my-stuff-up](skills/fuzz-my-stuff-up/SKILL.md) | Adversarial testing — ~34 agenter prøver å knekke koden fra alle vinkler |
| [should-i-abstract](skills/should-i-abstract/SKILL.md) | DRY-analyse som finner både kode som bør deles og abstraksjoner som bør inlines |
| [challenge-me](skills/challenge-me/SKILL.md) | Rask sanity-check — Sonnet-agent gir GOOD/TWEAK/RETHINK-verdict |
| [test-lead](skills/test-lead/SKILL.md) | Planlegger teststrategi og driver implementering av en lagdelt test suite |
| [audit-docs](skills/audit-docs/SKILL.md) | Auditerer CLAUDE.md/AGENTS.md for redundanser, motsetninger og gaps |

### Spesialist-review

| Name | What it gives you |
|------|-------------------|
| [scrutinize](skills/scrutinize/SKILL.md) | Stresstest via simulerte community-reaksjoner (Reddit, HN, Twitter, /g/, etc.) |
| [sweep](skills/sweep/SKILL.md) | Design-review — 10 parallelle linser (a11y, UX, polish, typografi, farge, ytelse) |
| [ux-review](skills/ux-review/SKILL.md) | Nielsens 10 heuristikker — parallelle agenter evaluerer brukervennlighet |
| [impeccable](https://impeccable.style/) | UI-skills for polishing, animasjon, farger, typografi, onboarding og mer (ekstern) |
| [hakon](agents/hakon.md) | Streng infra-reviewer — validerer Linux, Ansible, Docker, K8s, sikkerhet |

### Personlig økonomi

| Name | What it gives you |
|------|-------------------|
| [hallgeir](skills/hallgeir/SKILL.md) | Hallgeir fra Luksusfellen — analyserer forbruksmønstre, finner sløsing, leverer dommen på stavangersk |
| [pengeraadgiver](skills/pengeraadgiver/SKILL.md) | Finansiell rådgiver — hjelper med lån, pensjon, oppussing med tall fra din faktiske økonomi |

### Prosjekt & verktøy

| Name | What it gives you |
|------|-------------------|
| [envision](skills/envision/SKILL.md) | North-star visjonsdokument uten begrensninger |
| [ove](skills/ove/SKILL.md) | Python-prosjekt med moderne 2025+ tooling (uv, ruff, pyright) |
| [claude-delegate](scripts/claude-delegate) | Cross-repo agent-kommunikasjon via `claude -p` |

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
