# Skills

A collection of reusable skills for AI agents. Skills provide procedural knowledge that helps agents accomplish specific tasks more effectively.

Compatible with [Claude Code](https://docs.anthropic.com/en/docs/claude-code), [Cursor](https://cursor.sh), [Windsurf](https://windsurf.com), [GitHub Copilot](https://github.com/features/copilot), and [other supported agents](https://skills.sh).

## Available Skills

| Skill | Description |
|-------|-------------|
| [code-review](./code-review/SKILL.md) | Review code changes for bugs, security issues, performance problems, and best practice violations. |

## Installation

Install skills using the [skills CLI](https://github.com/vercel-labs/skills):

```sh
npx skills add mokhovyk/skills
```

This will install all skills from this repository and make them available to your AI agent.

### Manual Installation

You can also install skills manually by copying them into your agent's skills directory:

**Claude Code (personal — all projects):**

```sh
cp -r code-review ~/.claude/skills/code-review
```

**Claude Code (project-scoped):**

```sh
cp -r code-review .claude/skills/code-review
```

## Usage

Once installed, invoke a skill using its slash command:

```
/code-review
```

Skills can also accept arguments:

```
/code-review focus on error handling and security
```

Your AI agent may also invoke skills automatically when the conversation context matches the skill's description.

## Skill Structure

Each skill lives in its own directory with a `SKILL.md` file:

```
skill-name/
├── SKILL.md          # Required — skill definition with YAML frontmatter
├── reference.md      # Optional — detailed reference material
├── examples.md       # Optional — usage examples
└── scripts/          # Optional — helper scripts the agent can execute
```

### SKILL.md Format

```markdown
---
name: skill-name
description: Short description of what the skill does.
---

Step-by-step instructions for the agent to follow when this skill is invoked.
```

## Contributing

1. Create a new directory for your skill (e.g., `my-skill/`).
2. Add a `SKILL.md` file with frontmatter and instructions.
3. Update this README table with the new skill.
4. Submit a pull request.

## Learn More

- [Skills Directory & Leaderboard](https://skills.sh)
- [Skills Documentation](https://skills.sh/docs)
- [Skills CLI (open source)](https://github.com/vercel-labs/skills)
