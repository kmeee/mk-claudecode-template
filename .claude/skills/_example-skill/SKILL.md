---
name: _example-skill
description: "Example skill template. Rename this directory and edit to create your own skill. Skills provide domain-specific knowledge that agents can use."
model: sonnet
# disable-model-invocation: true   # Uncomment to prevent auto-invocation (for dangerous operations)
# context: fork                     # Uncomment to run in isolated subagent context
# allowed-tools: Read,Grep,Glob     # Uncomment to restrict which tools this skill can use
---

# Example Skill

This is a template for creating skills. Delete this file and create your own.

## What Are Skills?

Skills provide **domain-specific knowledge** that Claude can use during a session. They follow the **Command → Agent → Skills** architecture:

- **Commands** (`.claude/commands/`): Entry points that orchestrate workflows
- **Agents** (`.claude/agents/`): Specialist roles that execute tasks
- **Skills** (`.claude/skills/`): Domain knowledge preloaded into agent context

## How Skills Are Loaded

- **Descriptions** are always in context (Claude knows what's available)
- **Full content** loads on-demand when the skill is invoked
- **Agents with `skills:` field** get full skill content preloaded at startup

## Creating a New Skill

1. Create a directory: `.claude/skills/my-skill-name/`
2. Add `SKILL.md` with YAML frontmatter and instructions
3. Keep descriptions concise (they consume context budget)

## YAML Frontmatter Fields

| Field | Required | Description |
|-------|----------|-------------|
| `name` | No | Identifier (defaults to directory name) |
| `description` | Recommended | When to invoke — loaded into context for discovery |
| `model` | No | Model to use (sonnet, opus, haiku) |
| `disable-model-invocation` | No | Set `true` to require explicit user invocation |
| `context` | No | Set `fork` for isolated subagent context |
| `allowed-tools` | No | Comma-separated list of permitted tools |

## Example: Linking a Skill to an Agent

```yaml
# In .claude/agents/my-agent.md
---
name: my-agent
skills:
  - my-skill-name
---
```

The agent receives full skill content at startup as preloaded knowledge.
