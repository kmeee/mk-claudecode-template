# Project Template for Claude Code

A reusable project template with pre-configured Claude Code agents, commands, and workflows.

## Quick Start

1. **Copy this template** to your new project:
   ```bash
   cp -r template/ /path/to/your-new-project/
   cd /path/to/your-new-project/
   git init
   ```

2. **Start Claude Code** and tell it what you want to build:
   ```
   claude
   > "I want to build a REST API for a task management app using FastAPI and PostgreSQL"
   ```

3. **Claude will automatically**:
   - Read the CLAUDE.md and understand the project setup flow
   - Ask for any missing information (tech stack, commands, etc.)
   - Update CLAUDE.md with your project details
   - Be ready to use the RPI workflow for feature development

## What's Included

### Directory Structure

```
your-project/
├── .claude/
│   ├── settings.json              # Team-shared Claude Code settings
│   ├── agents/                    # 8 specialist agents
│   │   ├── code-reviewer.md
│   │   ├── constitutional-validator.md
│   │   ├── documentation-analyst-writer.md
│   │   ├── product-manager.md
│   │   ├── requirement-parser.md
│   │   ├── senior-software-engineer.md
│   │   ├── technical-cto-advisor.md
│   │   └── ux-designer.md
│   ├── commands/
│   │   └── rpi/                   # RPI workflow commands
│   │       ├── research.md        # /rpi:research - GO/NO-GO gate
│   │       ├── plan.md            # /rpi:plan - Planning docs
│   │       └── implement.md       # /rpi:implement - Phased implementation
│   └── skills/                    # Domain knowledge (extensible)
│       └── _example-skill/        # Template — rename and customize
│           └── SKILL.md
├── CLAUDE.md                      # Project guidance (auto-filled by Claude)
├── .gitignore
└── rpi/                            # RPI feature folders (created per feature)
    └── .gitkeep
```

### Architecture: Command → Agent → Skills

This template follows a three-tier architecture:

- **Commands** (`.claude/commands/`): Entry points that orchestrate multi-step workflows
- **Agents** (`.claude/agents/`): Specialist roles that execute tasks within a workflow
- **Skills** (`.claude/skills/`): Domain knowledge preloaded into agent context

Commands invoke agents. Agents use skills for domain-specific knowledge. This progressive disclosure pattern keeps each component focused and reusable.

### RPI Workflow (Research → Plan → Implement)

A structured development workflow with validation gates:

1. **Describe**: Write a feature request in natural language
2. **Research** (`/rpi:research`): Multi-agent analysis with GO/NO-GO decision
3. **Plan** (`/rpi:plan`): Generate PM, UX, engineering, and implementation docs
4. **Implement** (`/rpi:implement`): Phased implementation with code review gates

### Agents

| Agent | Purpose |
|-------|---------|
| **requirement-parser** | Extracts structured requirements from feature descriptions |
| **product-manager** | Creates PRDs with acceptance criteria and scope |
| **senior-software-engineer** | Plans, implements, and ships with tests |
| **ux-designer** | Produces UX briefs with flows and accessibility notes |
| **technical-cto-advisor** | Evaluates technical decisions against engineering principles |
| **code-reviewer** | Reviews for correctness, security, and maintainability |
| **documentation-analyst-writer** | Creates documentation following project standards |
| **constitutional-validator** | Validates features against project principles |

## RPI Workflow Example

```bash
# Step 1: Describe the feature
# Write your feature description, then have Claude create the request file

# Step 2: Research viability
/rpi:research user-authentication

# Step 3: Plan (after GO recommendation)
/rpi:plan user-authentication

# Step 4: Implement (after plan review)
/rpi:implement user-authentication
```

## Customization

### CLAUDE.md

The `CLAUDE.md` file is designed to be auto-filled by Claude on first use. You can also edit it manually to add:
- Project-specific architecture details
- Build and test commands
- Project constitution/principles
- Custom conventions

### Adding Custom Agents

Create new agent files in `.claude/agents/`:

```markdown
---
name: my-custom-agent
description: What this agent does and when to use it.
model: opus
---

[Agent instructions here]
```

### Adding Custom Commands

Create new command files in `.claude/commands/`:

```markdown
---
description: What this command does
argument-hint: "<required-arg>"
---

[Command workflow instructions here]
```

### Adding Skills

Create domain knowledge in `.claude/skills/<name>/SKILL.md`:

```markdown
---
name: my-skill
description: "When to use this skill — loaded into context for auto-discovery"
model: sonnet
# disable-model-invocation: true   # Require explicit invocation
# context: fork                     # Isolated subagent context
# allowed-tools: Read,Grep,Glob     # Restrict available tools
---

[Domain knowledge and instructions here]
```

To preload a skill into an agent, add a `skills:` field to the agent's frontmatter:

```yaml
---
name: my-agent
skills:
  - my-skill
---
```

See `.claude/skills/_example-skill/SKILL.md` for a complete template.

### Adding Hooks (Event-Driven Automation)

Hooks let you run scripts on Claude Code events. Create `.claude/hooks/` and configure in `settings.json`:

```json
{
  "hooks": {
    "PostToolUse": [
      {
        "matcher": "Bash",
        "hooks": [{ "type": "command", "command": ".claude/hooks/on-bash.sh" }]
      }
    ]
  }
}
```

Available events: `PreToolUse`, `PostToolUse`, `UserPromptSubmit`, `Notification`, `Stop`, `SubagentStart`, `SubagentStop`, `PreCompact`, `SessionStart`, `SessionEnd`.

## Design Principles

- **Commands for workflows** — always use commands as entry points, not standalone agents
- **Feature-specific agents** — create focused specialists, not general-purpose agents
- **Progressive disclosure** — agents get only the skills/context they need
- **Explicit tool definitions** — avoid vague terms like "launch" in agent definitions; be specific about which tools agents can use
- **Subagents use Task tool** — subagents cannot invoke other subagents via bash

## Best Practices

- **Keep CLAUDE.md under 150 lines** for reliable Claude adherence
- **Use `/compact` at ~50% context** to free up space
- **Start complex tasks in plan mode** before implementation
- **Use RPI for features, not bug fixes** — RPI is for substantial work
- **Break work into small subtasks** completable in under 50% context

## Configuration

### Personal Settings (git-ignored)

Create `.claude/settings.local.json` for personal preferences:

```json
{
  "permissions": {
    "allow": ["Bash(npm:*)"]
  }
}
```

### Team Settings

Edit `.claude/settings.json` for shared team configuration.

### Configuration Hierarchy

1. `.claude/settings.local.json` — personal (git-ignored), highest priority
2. `.claude/settings.json` — team-shared
3. For hooks: `hooks-config.local.json` overrides `hooks-config.json`
