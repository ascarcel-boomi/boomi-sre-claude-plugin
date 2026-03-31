# Boomi SRE Claude Code Plugin

> **Status: POC** — This is a proof-of-concept plugin for Boomi SRE teams. It will grow over time as team members contribute skills, agents, and commands. Feedback and contributions welcome.

A Claude Code plugin that provides shared hooks, agents, skills, and conventions for all Boomi SRE teams. Team-specific configuration (Jira projects, AWS accounts, Jenkins URLs) lives outside the plugin — in each team's project CLAUDE.md or personal memory.

## What's Included

### Hooks
| Hook | What it does |
|------|-------------|
| **PostToolUse** | Auto-formats Swift files after every Write/Edit |
| **PostCompact** | Re-injects key reminders when context gets compressed mid-session |
| **Stop** | Runs build verification (Swift/Node/Python) when Claude finishes work |

### Agents
| Agent | Role |
|-------|------|
| **backend-dev** | Services, models, API integration — builds first |
| **frontend-dev** | Views, ViewModels, UX — builds on backend |
| **qa-engineer** | Build verification, acceptance testing — verifies both |

### Skills
| Skill | Purpose |
|-------|---------|
| **feature-dev** | Mandatory workflow for new features — brainstorm → PRD → Agent Team (Backend Dev → Frontend Dev → QA Engineer) |
| **sre-onboard** | New team member setup — credential discovery, validation, team config |

### Commands
| Command | Purpose |
|---------|---------|
| **/check-creds** | Validate all MCP server credentials |
| **/verify-build** | Run the appropriate build command for the current project |

### CLAUDE.md
Boomi-wide SRE conventions: Jira (story points, hierarchy, formatting), AWS (ReadOnlyAccess), MCP servers, Zscaler SSL, development workflow.

## Installation

Add to your `~/.claude/settings.json`:

```json
{
  "extraKnownMarketplaces": {
    "boomi-sre": {
      "source": {
        "source": "github",
        "repo": "ascarcel-boomi/boomi-sre-claude-plugin"
      }
    }
  },
  "enabledPlugins": {
    "boomi-sre@boomi-sre": true
  }
}
```

Then restart Claude Code. Run `/sre-onboard` to set up your environment.

## Team-Specific Configuration

This plugin contains **Boomi-wide** conventions only. Each team adds their own details separately:

1. Copy `templates/team-config.md` to your project's CLAUDE.md or `~/.claude/` memory
2. Fill in your Jira projects, AWS accounts, Jenkins URLs, etc.
3. The `/sre-onboard` skill will walk you through this interactively

## Contributing Skills

SRE team members are building skills today that will be added here over time. To contribute:

1. Create your skill in a directory under `skills/<skill-name>/SKILL.md`
2. Follow the existing pattern — frontmatter with `name` and `description`, then instructions
3. Test locally: `claude --plugin-dir /path/to/this/repo`
4. Open a PR with a description of what the skill does and who it's for

### Directory Structure

```
boomi-sre-claude-plugin/
  .claude-plugin/
    plugin.json              # Plugin manifest
  agents/                    # Shared agent definitions
    backend-dev.md
    frontend-dev.md
    qa-engineer.md
  commands/                  # Slash commands
    check-creds.md
    verify-build.md
  hooks/
    hooks.json               # PostToolUse, PostCompact, Stop hooks
  skills/                    # Contributed skills go here
    feature-dev/
      SKILL.md
    sre-onboard/
      SKILL.md
  templates/
    team-config.md           # Template for team-specific configuration
  CLAUDE.md                  # Boomi-wide SRE conventions
  README.md
```

### What Belongs Here vs. Team-Specific

| **In this plugin** | **In your team's CLAUDE.md / memory** |
|---|---|
| Jira field IDs and Fibonacci scale | Your Jira project keys |
| "Use ReadOnlyAccess for audits" | Your AWS account IDs and profile names |
| "Use MCP tools, not curl" | Your Jenkins server URLs |
| Development workflow conventions | Your team members and handles |
| Shared agents and hooks | Your team's Grafana dashboards |

## Roadmap

This is a POC. Planned growth areas:
- [ ] More contributed skills from SRE team members
- [ ] Runbook-generation agents
- [ ] Incident response workflows
- [ ] SLO calculation and monitoring skills
- [ ] Migration to Boomi org repo when mature
