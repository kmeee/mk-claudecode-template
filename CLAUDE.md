# CLAUDE.md

## Project Setup Status: NOT INITIALIZED

This project has not been initialized yet.
When the user gives their first instruction, follow this setup flow:

### First-time Setup Flow

1. **Ask what they want to build** — project overview and purpose
2. **Ask about tech stack** — language, framework, database, testing tools
3. **Ask about build/test commands** — install, dev, test, lint, build
4. **Ask about project principles** — quality standards, constraints (if any)

Once gathered, update this CLAUDE.md:
- Change "NOT INITIALIZED" to "INITIALIZED"
- Fill in the sections below with the gathered information
- Update `.claude/commands/rpi/implement.md` validation commands (replace `[your-linter-command]`, `[your-test-command]`, `[your-build-command]` with actual commands)
- Remove these setup instructions

If the user skips setup and jumps straight to a task, infer what you can from context and ask only the minimum needed to proceed.

---

## Project Overview

[What this project does — 1-2 sentences]

## Tech Stack

- Language: [e.g. TypeScript, Python]
- Framework: [e.g. Next.js, FastAPI, Rails]
- Database: [e.g. PostgreSQL, MongoDB, SQLite]
- Testing: [e.g. Jest, pytest, RSpec]
- Other: [e.g. Redis, Docker, etc.]

## Build & Test Commands

- Install: `[command]`
- Dev server: `[command]`
- Test: `[command]`
- Lint: `[command]`
- Build: `[command]`

## Architecture

[Key directories and their roles — fill in as project evolves]

## RPI Workflow

This project includes the RPI (Research → Plan → Implement) workflow for structured feature development:

- `/rpi:research <feature-slug>` — Analyze feature viability (GO/NO-GO gate)
- `/rpi:plan <feature-slug>` — Generate comprehensive planning docs
- `/rpi:implement <feature-slug>` — Execute phased implementation with validation gates

### When to Use RPI
- New features with meaningful scope
- Changes touching multiple components
- Features requiring research or design decisions

### When NOT to Use RPI
- Bug fixes, small tweaks, documentation-only changes
- Changes completable in under 30 minutes

### RPI Feature Folder Structure
```
rpi/{feature-slug}/
├── REQUEST.md              # Feature description
├── research/
│   └── RESEARCH.md         # GO/NO-GO analysis
├── plan/
│   ├── PLAN.md             # Implementation roadmap
│   ├── pm.md               # Product requirements
│   ├── ux.md               # UX design
│   └── eng.md              # Technical specification
└── implement/
    └── IMPLEMENT.md        # Implementation record
```

## Available Agents

| Agent | Role |
|-------|------|
| requirement-parser | Extract structured requirements from feature descriptions |
| product-manager | Create PRDs with acceptance criteria |
| senior-software-engineer | Plan and ship with tests, write PRs |
| ux-designer | Produce UX briefs with flows and accessibility |
| technical-cto-advisor | Align tech decisions with engineering principles |
| code-reviewer | Review for correctness, security, maintainability |
| documentation-analyst-writer | Write documentation per project standards |
| constitutional-validator | Validate against project constitution/principles |

## Design Principles

- **Commands for workflows, not standalone agents** — always use `.claude/commands/` as entry points
- **Feature-specific agents with skills (progressive disclosure)** — avoid general-purpose agents; create focused specialists
- **Command → Agent → Skills architecture** — commands orchestrate, agents execute, skills provide domain knowledge
- **Explicit tool usage in agent definitions** — avoid vague terms like "launch" that could be misinterpreted as bash commands
- **Subagents cannot invoke other subagents via bash** — always use the Task tool
- **Skills for domain knowledge** — put reusable knowledge in `.claude/skills/`; see `_example-skill` for template

## Workflow Best Practices

- Keep this CLAUDE.md under 150 lines
- Perform `/compact` at ~50% context usage
- Start with plan mode for complex tasks
- Use human-gated todo lists for multi-step tasks
- Break subtasks small enough to complete in under 50% context

## Project Constitution

[Project principles and constraints — fill in if applicable]
[e.g. "Type safety required", "All features must have tests", "No external API calls without approval"]

## Extending This Project

- **Add skills**: Create `.claude/skills/<name>/SKILL.md` for domain knowledge
- **Add hooks**: Configure `.claude/hooks/` for event-driven automation (e.g. notifications)
- **Add commands**: Create `.claude/commands/<name>.md` for reusable workflows
- **Browser debugging**: Use Playwright MCP or Chrome DevTools MCP for console log inspection

## Debugging Tips

- Use `/doctor` for diagnostics
- Run long-running commands as background tasks
- Provide screenshots when reporting visual issues
