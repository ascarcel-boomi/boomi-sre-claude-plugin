# Contributing to the Boomi SRE Claude Code Plugin

## Adding a Skill

1. Create `skills/<your-skill-name>/SKILL.md`
2. Add frontmatter:
   ```yaml
   ---
   name: your-skill-name
   description: One sentence describing when to use this skill
   ---
   ```
3. Write clear instructions for what Claude should do when the skill is invoked
4. Test locally: `claude --plugin-dir /path/to/this/repo`
5. Verify the skill appears in `/help` and works as expected
6. Open a PR

## Adding an Agent

1. Create `agents/<your-agent-name>.md`
2. Add frontmatter:
   ```yaml
   ---
   name: your-agent-name
   description: When and why to use this agent
   model: inherit
   ---
   ```
3. Write the agent's role, responsibilities, and constraints
4. Open a PR

## Adding a Command

1. Create `commands/<your-command-name>.md`
2. Add frontmatter:
   ```yaml
   ---
   name: your-command-name
   description: What this command does
   ---
   ```
3. Write what Claude should do when the command is invoked
4. Open a PR

## Guidelines

- **Keep it generic** — no team-specific Jira projects, AWS accounts, or URLs. Those go in team CLAUDE.md files.
- **Test before submitting** — use `claude --plugin-dir` to verify
- **One skill per PR** — makes review easier
- **Include a description** — explain who the skill is for and when to use it
