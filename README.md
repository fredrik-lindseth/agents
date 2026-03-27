# agents

Reusable AI agent skills and personas for Claude Code and OpenCode.

## Skills

| Skill | Description |
|-------|-------------|
| [envision](skills/envision/SKILL.md) | Describe the unconstrained ideal version of a project — a north-star vision document |
| [ove](skills/ove/SKILL.md) | Set up new Python projects or migrate existing ones to a modern 2025+ tooling stack (uv, ruff, pyright, hatchling) |

## Agents

| Agent | Description |
|-------|-------------|
| [hakon](agents/hakon.md) | Streng infrastruktur-reviewer som validerer Linux, Ansible, Docker, Kubernetes, sikkerhet og nettverk med nulltoleranse |

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

### OpenCode

Copy agents into your OpenCode agents directory and adjust the frontmatter format as needed.
