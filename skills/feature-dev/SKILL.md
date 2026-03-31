---
name: feature-dev
description: Mandatory workflow for all new features — brainstorm into a PRD, then hand off to an Agent Team (Backend Dev, Frontend Dev, QA Engineer). Invoke this before writing any application code.
---

# Feature Development Workflow

This skill enforces the mandatory development workflow for all new features and significant changes. It ensures every feature goes through brainstorming, produces a PRD, and is built by an Agent Team.

**Do NOT skip steps. Do NOT write application code before the PRD is approved.**

## Phase 1 — Brainstorm

Invoke the `superpowers:brainstorming` skill to explore the idea collaboratively.

This phase produces an approved design spec. It includes:
- Understanding the current project context (read CLAUDE.md, check recent commits)
- Asking clarifying questions one at a time
- Proposing 2-3 approaches with trade-offs and a recommendation
- Presenting the design section by section for user approval

**Hard gate:** Do not proceed to Phase 2 until the user has approved the design.

## Phase 2 — Write the PRD

Take the approved design and produce a structured PRD (Product Requirements Document). The PRD is the contract between the user and the Agent Team — it defines exactly what gets built and how QA verifies it.

### PRD Structure

```markdown
# PRD: <Feature Name>

## Problem Statement
What's broken or missing and why it matters.

## Requirements
Numbered, testable acceptance criteria. Each requirement should be verifiable by QA.
- R1: ...
- R2: ...
- R3: ...

## Design
Which files to create/modify, data flow, UI layout, architecture decisions.
Reference the project's CLAUDE.md for existing patterns and conventions.

## Dependencies
What existing services, models, views, or external APIs are involved.

## Out of Scope
Explicit boundaries — what this feature does NOT include.

## Test Plan
How QA verifies each requirement. Map test cases to requirement numbers.
- T1 (verifies R1): ...
- T2 (verifies R2): ...
```

### Save the PRD

- Save to `docs/prd-<feature-name>.md` in the project repo
- Commit the PRD before starting implementation
- Present the PRD to the user for final review

**Hard gate:** Do not proceed to Phase 3 until the user has approved the written PRD.

## Phase 3 — Agent Team

Spawn three agents sequentially using the plugin's agent definitions. Each agent receives the PRD and must read the project's CLAUDE.md before writing any code.

### Step 1: Backend Dev

Spawn the `backend-dev` agent with:
- The full PRD content
- Instruction to read the project's CLAUDE.md first
- Specific requirements from the PRD that involve services, models, API integration, or data flow
- Instruction to verify the build compiles before reporting done

The Backend Dev builds first. Services and models must exist before the Frontend Dev can consume them.

### Step 2: Frontend Dev

Spawn the `frontend-dev` agent with:
- The full PRD content
- Instruction to read the project's CLAUDE.md first
- Specific requirements from the PRD that involve views, ViewModels, navigation, or UX
- Instruction to use the services and models created by Backend Dev — do not create duplicates
- Instruction to verify the build compiles before reporting done

### Step 3: QA Engineer

Spawn the `qa-engineer` agent with:
- The full PRD content (especially the Test Plan section)
- Instruction to read the project's CLAUDE.md first
- Instruction to build the project and confirm zero errors
- Instruction to test each numbered requirement and report PASS/FAIL with evidence
- Instruction to check for regressions in existing functionality

### After QA

- Review QA results with the user
- If QA found issues, dispatch the appropriate agent (Backend or Frontend) to fix them, then re-run QA
- Once all requirements pass, update the project's CLAUDE.md if architecture or patterns changed
- Commit, push, and release

## Exceptions

This workflow is **not required** for:
- Bug fixes (single-issue, clear root cause)
- Config changes (environment variables, build settings)
- One-line tweaks (typos, constant changes)
- Documentation-only changes

If you're unsure whether something qualifies as an exception, it doesn't — use the full workflow.

## Quick Reference

```
1. Brainstorm  →  superpowers:brainstorming  →  approved design
2. PRD         →  docs/prd-<feature>.md      →  approved requirements
3. Agent Team  →  Backend Dev → Frontend Dev → QA Engineer
4. Review      →  QA pass + user sign-off
5. Ship        →  update CLAUDE.md, commit, push, release
```
