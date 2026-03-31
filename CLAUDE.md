# Boomi SRE Claude Code Plugin

## Boomi-Wide Conventions

### Jira (boomii.atlassian.net)
- **Story Points:** Always use `customfield_10008` (float). Fibonacci scale: 1, 2, 3, 5, 8. The MCP `story_points` param does NOT map correctly — use `additional_fields={"customfield_10008": <number>}`.
- **Issue types that get SP:** Story, Task, Defect, Epic, Parent Epic, Operational Request. Never: Subtask, Bug, Incident, Change Request.
- **Priority:** Set contextually. Planned work = relative to epic scope. Unplanned = deduce from context or ask reporter.
- **Description formatting:** Use Jira wiki markup, not markdown. Use actual newlines, not `\\n`.
- **Hierarchy:** Initiative → Parent Epic → Epic → Story → Sub-task. Epics are the currency of quarterly planning.
- **Bulk operations:** Test on 2 tickets first, show results, wait for confirmation before proceeding.

### AWS
- **Always use ReadOnlyAccess SSO profiles** for audits, investigations, and read operations. Never AdminAccess unless explicitly instructed.
- **AWS CLI in .app bundles:** Use `/usr/local/bin/aws` — PATH is stripped in macOS app bundles.

### MCP Servers
Use configured MCP tools for all external service calls. Never curl APIs manually with credential files.
- Jira/Confluence: `mcp__mcp-atlassian__*`
- Jenkins: `mcp__jenkins-mcp__*`
- GitHub: `mcp__github__*`
- Bitbucket: `mcp__bitbucket__*`
- Grafana: `mcp__grafana__*`

### Zscaler SSL
All HTTP from corporate machines goes through Zscaler. Use the Zscaler root CA cert:
- `curl --cacert ~/zscaler-root.pem` for curl commands
- Custom URLSession trust in native apps
- `-sk` only for internal endpoints that don't need cert verification

### Bitbucket
- Workspace: `boomii`
- Credentials expire frequently. Alert user on auth errors rather than retrying.

### Development Workflow — MANDATORY

**Every new feature or significant change MUST follow this workflow. Do NOT write application code without completing steps 1-2 first.**

1. **Brainstorm** — invoke the `feature-dev` skill (or `superpowers:brainstorming` directly). No code until the design is approved by the user.
2. **Write a PRD** — structured requirements doc saved to `docs/prd-<feature>.md`. This is the contract for the Agent Team. The user must approve the PRD before implementation begins.
3. **Agent Teams** — spawn Backend Dev → Frontend Dev → QA Engineer with the PRD. Each agent reads the project's CLAUDE.md first and verifies the build compiles before reporting done.
4. **Verify before done** — build must pass, QA must sign off, never claim success without proof.
5. **Update CLAUDE.md** — if architecture or patterns changed, update before closing the session.

**Exceptions:** Bug fixes, config changes, and one-line tweaks do not require this workflow. If you're unsure, use it.

## Team-Specific Configuration

Each SRE team should maintain their own details in their project's CLAUDE.md or `~/.claude/` memory. This includes:
- Jira project keys (e.g., CAMSRE, SRE, NDS)
- AWS account IDs
- Jenkins server URLs
- Team-specific Grafana dashboards
- Team-specific runbooks and Confluence spaces

See `templates/team-config.md` for a starting template.
