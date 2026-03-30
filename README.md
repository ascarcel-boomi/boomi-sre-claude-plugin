# CAM SRE Claude Code Plugin

A Claude Code plugin for the Boomi CAM SRE team. Provides shared hooks, agents, skills, and conventions so every team member gets the same baseline setup.

## What's Included

### Hooks
- **PostToolUse** — Auto-formats Swift files after every edit
- **PostCompact** — Re-injects key reminders when context gets compressed mid-session
- **Stop** — Runs build verification when Claude finishes work (Swift, Node, Python)

### Agents
- **backend-dev** — Builds services, models, and API integrations
- **frontend-dev** — Builds views, view models, and UX
- **qa-engineer** — Verifies builds, tests PRD requirements, files issues

### Skills
- **sre-onboard** — Walk through setting up Claude Code for the team (credentials, MCP, conventions)

### Commands
- **/check-creds** — Validate all MCP server credentials
- **/verify-build** — Run the appropriate build command for the current project

### CLAUDE.md
Shared team conventions for Jira, AWS, MCP servers, Zscaler, and development workflow.

## Installation

### Option 1: Via settings.json (recommended for teams)

Add to your `~/.claude/settings.json`:

```json
{
  "extraKnownMarketplaces": {
    "cam-sre": {
      "source": "github",
      "repo": "Mashery-Boomi/cam-sre-claude-plugin"
    }
  },
  "enabledPlugins": {
    "cam-sre@cam-sre": true
  }
}
```

### Option 2: Via /plugin command

```
/plugin install cam-sre from github:Mashery-Boomi/cam-sre-claude-plugin
```

## MCP Server Credentials

The plugin expects credentials in `~/.claude/mcp_credentials/`:

| File | Contents |
|------|----------|
| `mcp-atlassian.env` | ATLASSIAN_URL, ATLASSIAN_USERNAME, ATLASSIAN_API_TOKEN, CONFLUENCE_URL, CONFLUENCE_USERNAME, CONFLUENCE_TOKEN |
| `bitbucket.env` | BITBUCKET_WORKSPACE, BITBUCKET_USERNAME, BITBUCKET_APP_PASSWORD |
| `github.env` | GITHUB_PERSONAL_ACCESS_TOKEN |
| `grafana.env` | GRAFANA_URL, GRAFANA_API_KEY |
| `jenkins.env` | JENKINS_URL, JENKINS_USERNAME, JENKINS_TOKEN |

Run `/sre-onboard` to discover and configure credentials automatically.

## Development Workflow

1. **Brainstorm** — Plan before code (use planning mode)
2. **Write PRD** — Requirements doc with acceptance criteria
3. **Agent Teams** — Backend Dev → Frontend Dev → QA Engineer
4. **Verify** — Build/test before claiming done
5. **Update CLAUDE.md** — Keep project docs current

## Contributing

Add team conventions to `CLAUDE.md`, new skills to `skills/`, and new agents to `agents/`. Open a PR.
