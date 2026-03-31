# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## What This Repo Is

**human-compound** is a Claude Code plugin for team-coordinated AI-assisted engineering. It provides skills (slash commands) and agents (specialized sub-agents) that follow a six-stage workflow, plus GitHub automation for team visibility.

This is a **plugin repo**, not an application. There is no build system, runtime, or test suite. The deliverables are markdown-based skill and agent definitions consumed by Claude Code's plugin system.

## Plugin Structure

- `.claude-plugin/plugin.json` — Plugin manifest (name, version, MCP servers)
- `skills/` — 32 skill definitions, each in its own directory with a `SKILL.md` frontmatter file
- `agents/` — Agent definitions organized by role: `review/`, `research/`, `document-review/`, `workflow/`
- `.github/` — CODEOWNERS, issue templates, PR templates, notification workflow
- `docs/` — Artifact directories for the workflow (brainstorms, plans, solutions, ideation)

## Skill Naming Convention

Core workflow skills use the `he:` prefix (e.g., `/he:brainstorm`, `/he:plan`, `/he:work`, `/he:review`, `/he:compound`). Team skills use `team:` prefix. Git skills use `git-` prefix. All other skills are unprefixed.

## How Skills and Agents Connect

Skills are orchestrators invoked by the user (e.g., `/he:review`). They spawn agents as parallel sub-agents for focused work. For example, `/he:review` dynamically selects from ~19 review agents based on the diff content, runs them in parallel, then merges and deduplicates their findings.

Agent definitions in `agents/` use frontmatter to specify: `name`, `description`, `model`, `tools`, and `color`. The markdown body is the agent's system prompt.

## Key Workflow Skills

| Skill | What it does |
|-------|-------------|
| `/he:ideate` | Generates grounded improvement ideas from codebase analysis |
| `/he:brainstorm` | Explores requirements through AI-guided dialogue → requirements doc |
| `/he:plan` | Researches codebase and creates implementation plan |
| `/he:work` | Executes a plan or bare prompt with complexity-based routing |
| `/he:review` | Spawns tiered reviewer agents, merges findings, supports autofix mode |
| `/he:compound` | Captures learnings from solved problems → solution doc |
| `/he:agent-learn` | Records agent calibration data (false positives, missed findings) |

## Team Conventions

@AGENTS.md

## Working on This Repo

When modifying skills or agents:
- Skill definitions live in `skills/<name>/SKILL.md` with YAML frontmatter (`name`, `description`, `argument-hint`)
- Agent definitions live in `agents/<category>/<name>.md` with YAML frontmatter (`name`, `description`, `model`, `tools`, `color`)
- Reference files used by skills go in `skills/<name>/references/`
- Asset templates go in `skills/<name>/assets/`
- The `docs/` directories are for workflow artifacts, not plugin source
