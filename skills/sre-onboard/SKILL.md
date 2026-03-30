---
name: sre-onboard
description: Set up a new team member's Claude Code environment — discovers credentials, configures MCP servers, validates connectivity, and explains team conventions. Run this when someone joins the team or sets up Claude Code for the first time.
---

# SRE Onboarding

Walk a new team member through setting up their Claude Code environment for the CAM SRE team.

## Steps

1. **Check existing credentials** — scan for MCP credential files across the home directory:
   - `~/.claude/mcp_credentials/`
   - `~/.kiro/mcp_credentials/`
   - `~/.amazonq/mcp_credentials/`
   - Any other dot-directories with `.env` files containing known token patterns

2. **Validate found credentials** — for each discovered service, test connectivity:
   - Jira: `curl -u user:token https://boomii.atlassian.net/rest/api/3/myself`
   - GitHub: `curl -H "Authorization: Bearer token" https://api.github.com/user`
   - Jenkins: `curl -u user:token https://jenkins-master.mashspud.com/api/json`
   - Grafana: test the Grafana URL with API key
   - Bitbucket: `curl -u user:token https://api.bitbucket.org/2.0/user`

3. **Copy to canonical location** — consolidate all valid credentials into `~/.claude/mcp_credentials/`:
   - `mcp-atlassian.env` (Jira + Confluence)
   - `bitbucket.env`
   - `github.env`
   - `grafana.env`
   - `jenkins.env`

4. **Check Zscaler cert** — verify `~/zscaler-root.pem` exists. If not, guide the user to export it from Keychain Access.

5. **Check AWS SSO** — verify `aws sso login` works and ReadOnlyAccess profiles are configured.

6. **Explain team conventions** — point the user to this plugin's CLAUDE.md and highlight:
   - Story points field (`customfield_10008`)
   - Jira hierarchy and quarterly planning
   - Always verify work before claiming done
   - Agent Teams workflow (Backend → Frontend → QA)

7. **Summary** — show what was configured, what's working, and any manual steps remaining.
