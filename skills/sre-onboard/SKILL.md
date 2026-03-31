---
name: sre-onboard
description: Set up a new SRE team member's Claude Code environment — discovers credentials, validates connectivity, and walks through team configuration. Run when someone joins or sets up Claude Code for the first time.
---

# SRE Onboarding

Walk a new team member through setting up their Claude Code environment for Boomi SRE.

## Steps

1. **Identify the user's team** — ask which SRE team they belong to (CAM SRE, Platform SRE, etc.) and what services they work with (Jira, AWS, Jenkins, Grafana, GitHub, Bitbucket, Confluence).

2. **Discover existing credentials** — scan for MCP credential files across the home directory:
   - `~/.claude/mcp_credentials/`
   - `~/.kiro/mcp_credentials/`
   - `~/.amazonq/mcp_credentials/`
   - Any other dot-directories with `.env` files containing known token patterns (JIRA_API_TOKEN, GITHUB_TOKEN, JENKINS_TOKEN, GRAFANA_TOKEN, etc.)

3. **Validate found credentials** — for each discovered service, test connectivity:
   - Jira: GET `/rest/api/3/myself` with the Atlassian token
   - GitHub: GET `https://api.github.com/user` with the GitHub token
   - Bitbucket: GET `https://api.bitbucket.org/2.0/user` with Bitbucket creds
   - Jenkins: GET `{JENKINS_URL}/api/json` with Jenkins creds
   - Grafana: GET `{GRAFANA_URL}/api/org` with the API key
   - Use `--cacert ~/zscaler-root.pem` for all HTTPS calls (Zscaler proxy)

4. **Consolidate to canonical location** — copy all valid credentials into `~/.claude/mcp_credentials/`:
   - `mcp-atlassian.env` (Jira + Confluence URL, username, API token)
   - `bitbucket.env` (workspace, username, app password)
   - `github.env` (personal access token)
   - `grafana.env` (URL, API key)
   - `jenkins.env` (URL, username, token)

5. **Check Zscaler cert** — verify `~/zscaler-root.pem` exists. If not, guide the user to export the Zscaler root CA from Keychain Access.

6. **Check AWS SSO** — verify `aws sso login` works and ReadOnlyAccess profiles are configured for their team's accounts.

7. **Generate team config** — using the `templates/team-config.md` template, prompt the user for their team-specific details (Jira projects, AWS account IDs, Jenkins URLs, etc.) and save it to their `~/.claude/` memory or project CLAUDE.md.

8. **Explain conventions** — walk through the plugin's CLAUDE.md and highlight:
   - Story points field (`customfield_10008`) and Fibonacci scale
   - Jira hierarchy and quarterly planning
   - Always verify work before claiming done
   - Development workflow: brainstorm → PRD → Agent Teams → verify
   - MCP tools over manual curl

9. **Summary** — show what was configured, what's working, and any manual steps remaining.
