# CAM SRE Claude Code Plugin

## Team Conventions

### Jira
- **Story Points:** Always use `customfield_10008` (float). Fibonacci scale: 1, 2, 3, 5, 8. Never use `story_points` MCP param — it doesn't map correctly. Use `additional_fields={"customfield_10008": <number>}`.
- **Issue types that get SP:** Story, Task, Defect, Epic, Parent Epic, Operational Request. Never: Subtask, Bug, Incident, Change Request.
- **Priority:** Set contextually. Planned work = relative to epic scope. Unplanned = deduce from context or ask reporter.
- **Description formatting:** Use Jira wiki markup, not markdown. Use actual newlines, not `\\n`.
- **Projects:** SRE, CAMSRE, NDS, DO, MC, MCS, CM1, INC, COCD, JOPS, PTO, RDINTAKE, RP — all at `boomii.atlassian.net`
- **Hierarchy:** Initiative → Parent Epic → Epic → Story → Sub-task. Epics are the currency of quarterly planning.
- **Bulk operations:** Test on 2 tickets first, show results, wait for confirmation before proceeding with the rest.

### AWS
- **Always use ReadOnlyAccess SSO profiles** for audits, investigations, and read operations. Never AdminAccess unless explicitly instructed.
- **Account IDs:** Staging `566161767908`, QA `680833432085`, Production `809167139867`
- **AWS CLI in .app bundles:** Use `/usr/local/bin/aws` — PATH is stripped in macOS app bundles.

### MCP Servers
Use the configured MCP tools for all external service calls. Never curl APIs manually.
- Jira/Confluence: `mcp__mcp-atlassian__*`
- Jenkins: `mcp__jenkins-mcp__*`
- GitHub: `mcp__github__*`
- Bitbucket: `mcp__bitbucket__*`
- Grafana: `mcp__grafana__*`

### Zscaler SSL
All HTTP from corporate machines goes through Zscaler. Use the Zscaler root CA cert for HTTPS calls:
- `curl --cacert ~/zscaler-root.pem` for curl commands
- Custom URLSession trust in Swift apps
- `-sk` only for internal endpoints that don't need cert verification

### Development Workflow
1. **Always brainstorm first** — use planning mode before writing code
2. **Write a PRD** for any new feature before implementation
3. **Agent Teams** for development: Backend Dev → Frontend Dev → QA Engineer
4. **Verify before done** — run build/test/check, never claim success without proof
5. **Update CLAUDE.md** after significant changes to a codebase

### Bitbucket
- Workspace: `boomii`
- Credentials expire frequently. Alert user on auth errors rather than retrying.

### Jenkins
- Primary: `jenkins-master.mashspud.com`
- USW2: `jenkins-master-usw2.mashspud.com`
