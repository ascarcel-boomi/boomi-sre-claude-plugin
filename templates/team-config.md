# Team Configuration Template

Copy this to your project's CLAUDE.md or `~/.claude/` memory directory and fill in your team's details.

## Team: [YOUR TEAM NAME]

### Jira Projects
- Primary: `[PROJECT_KEY]` (e.g., CAMSRE, SRE, ENGP)
- Related: `[OTHER_KEYS]`

### AWS Accounts
| Environment | Account ID | SSO Profile |
|-------------|-----------|-------------|
| Staging | `[ACCOUNT_ID]` | `[profile]-ReadOnlyAccess` |
| QA | `[ACCOUNT_ID]` | `[profile]-ReadOnlyAccess` |
| Production | `[ACCOUNT_ID]` | `[profile]-ReadOnlyAccess` |

### Jenkins
- Primary: `[JENKINS_URL]`
- Secondary: `[JENKINS_URL]` (if applicable)

### Grafana Dashboards
- On-call: `[DASHBOARD_URL]`
- SLOs: `[DASHBOARD_URL]`

### Confluence Spaces
- Team KB: `[SPACE_KEY]`
- Runbooks: `[SPACE_KEY]`

### Team Members
- `[name]` — `[GitHub handle]` / `[Jira username]`
