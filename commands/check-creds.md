---
name: check-creds
description: Validate all MCP server credentials and report which services are connected
---

Check connectivity to all configured MCP services for the CAM SRE team. For each service, test the actual API endpoint and report OK or FAILED with the HTTP status code.

Services to check:
1. **Jira** — `mcp-atlassian.env` → GET `/rest/api/3/myself`
2. **Confluence** — same token as Jira → GET `/wiki/rest/api/space?limit=1`
3. **Bitbucket** — `bitbucket.env` → GET `https://api.bitbucket.org/2.0/user`
4. **GitHub** — `github.env` → GET `https://api.github.com/user`
5. **Jenkins** — `jenkins.env` → GET `{JENKINS_URL}/api/json`
6. **Grafana** — `grafana.env` → GET `{GRAFANA_URL}/api/org`
7. **AWS SSO** — run `aws sts get-caller-identity` to check if session is active

Read credentials from `~/.claude/mcp_credentials/`. Use `--cacert ~/zscaler-root.pem` for all HTTPS calls (Zscaler proxy). Use `-sk` for Jenkins (self-signed cert).

Report results in a table format with service name, status, and details.
