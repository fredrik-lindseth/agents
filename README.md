# agents

Reusable AI agent skills and personas for Claude Code and OpenCode.

## Skills

| Skill | Description |
|-------|-------------|
| [envision](skills/envision/SKILL.md) | Describe the unconstrained ideal version of a project — a north-star vision document |
| [ove](skills/ove/SKILL.md) | Set up new Python projects or migrate existing ones to a modern 2025+ tooling stack (uv, ruff, pyright, hatchling) |
| [test-lead](skills/test-lead/SKILL.md) | Senior testleder som planlegger teststrategi, teststruktur og driver implementering av en lagdelt test suite |

## Agents

| Agent | Description |
|-------|-------------|
| [hakon](agents/hakon.md) | Streng infrastruktur-reviewer som validerer Linux, Ansible, Docker, Kubernetes, sikkerhet og nettverk med nulltoleranse |

## Scripts

| Script | Description |
|--------|-------------|
| [claude-delegate](scripts/claude-delegate) | Cross-repo agent-kommunikasjon — delegerer oppgaver mellom Claude Code-sesjoner via `claude -p` |

## Usage

### Claude Code

Copy or symlink skills into `~/.claude/skills/` and agents into `~/.claude/agents/`:

```bash
# Skills
ln -s /path/to/agents/skills/envision ~/.claude/skills/envision
ln -s /path/to/agents/skills/ove ~/.claude/skills/ove

# Agents
ln -s /path/to/agents/agents/hakon.md ~/.claude/agents/hakon.md
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
