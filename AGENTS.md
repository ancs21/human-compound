# human-compound Plugin Instructions

Human-first, team-driven engineering plugin. These instructions apply to all skills and agents in this plugin.

## Workflow

The team follows six stages. Each produces a durable artifact visible to the whole team via GitHub.

```
Ideate -> Brainstorm -> Plan -> Work -> Review -> Compound
```

| Stage | Skill | Artifact | Directory |
|-------|-------|----------|-----------|
| Ideate | `/he:ideate` | Ideation doc | `docs/ideation/` |
| Brainstorm | `/he:brainstorm` | Requirements doc | `docs/brainstorms/` |
| Plan | `/he:plan` | Implementation plan | `docs/plans/` |
| Work | `/he:work` | Code on feature branch | Feature branch |
| Review | `/he:review` | Review findings | PR comments |
| Compound | `/he:compound` | Learning doc | `docs/solutions/` |

## Team Coordination

- **Before starting work:** Check GitHub Issues for existing team discussion (`/team:sync`)
- **After creating an artifact:** Push as PR so the team has visibility (`/team:handoff`)
- **To see what's happening:** Run `/team:status` to see all active work across stages
- **After dismissing review findings:** Record as agent learnings (`/he:agent-learn`)

## Command Naming

Commands use the `he:` prefix (short for human-compound) to avoid conflicts with built-in Claude Code commands like `/plan` and `/review`.

Team coordination commands use the `team:` prefix: `/team:status`, `/team:handoff`, `/team:sync`.

## Artifact Naming

- Requirements: `docs/brainstorms/YYYY-MM-DD-<topic>-requirements.md`
- Plans: `docs/plans/YYYY-MM-DD-NNN-<type>-<name>-plan.md`
- Learnings: `docs/solutions/<category>/<slug>.md`
- Ideation: `docs/ideation/YYYY-MM-DD-<topic>-ideation.md`

## Agent Learning

Each agent has a `## Learnings` section at the bottom of its `.md` file. These are calibration corrections from past reviews — false positives, missed findings, wrong severity, wrong advice.

When spawning agents, pass their learnings so they do not repeat known mistakes. Use `/he:agent-learn` to record new learnings after reviewing agent output.

## Directory Structure

```
agents/
├── review/           # Code review agents (19)
├── document-review/  # Plan and spec review agents (7)
├── research/         # Research and analysis agents (6)
└── workflow/         # Workflow automation agents (4)

skills/
├── he-*/             # Core workflow skills (he:ideate, he:brainstorm, etc.)
├── team-*/           # Team coordination skills (team:status, etc.)
└── */                # Git, quality, and utility skills
```

## GitHub Integration

The `.github/` directory contains team coordination configs meant to be copied into project repos:

- `.github/CODEOWNERS` — auto-assign reviewers by artifact type
- `.github/workflows/notify-artifact.yml` — Slack notifications on artifact push
- `.github/ISSUE_TEMPLATE/` — structured ideation and brainstorm forms
- `.github/PULL_REQUEST_TEMPLATE/` — stage-specific PR templates

## Scratch Space

Use `.context/human-compound/` for ephemeral workflow artifacts. Clean up after successful completion.

## Key Principles

- **Human-first:** AI assists, humans decide. Every product and architectural decision is made by a person.
- **Visibility over gates:** No mandatory approvals. Work is visible to the team but not blocked by process.
- **Knowledge compounds:** Every solved problem is documented. Future work gets cheaper over time.
- **Agents learn:** Review agents improve through calibration feedback. They do not repeat known mistakes.
