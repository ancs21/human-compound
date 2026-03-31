---
name: team:handoff
description: "Push the current artifact to GitHub and notify the next person. Use when saying 'hand this off', 'pass to team', 'ready for review', or after completing a workflow stage."
argument-hint: "[optional: who to hand off to, e.g. @alice]"
---

# Team Handoff

Formalize the handoff between workflow stages by pushing the artifact and notifying the team.

## Execution

### Step 1: Detect what to hand off

Check what has changed locally:

```bash
git status --short
git diff --name-only HEAD
```

Classify the artifact:
- Files in `docs/brainstorms/` -> Requirements handoff (brainstorm -> plan)
- Files in `docs/plans/` -> Plan handoff (plan -> work)
- Code changes -> Implementation handoff (work -> review)
- Files in `docs/solutions/` -> Learning handoff (compound -> done)

### Step 2: Commit and push

```bash
git add <artifact files>
git commit -m "<type>(handoff): <brief description>"
git push -u origin <current-branch>
```

### Step 3: Create or update PR

If no PR exists for this branch, create one using the appropriate stage template:

| Artifact type | PR template | Suggested reviewers |
|--------------|-------------|---------------------|
| Requirements | `--template requirements.md` | Whole team |
| Plan | `--template plan.md` | Tech lead |
| Implementation | `--template implementation.md` | Engineers + tech lead |
| Learning | `--template learning.md` | Whole team |

```bash
gh pr create --title "<stage>: <description>" --template <template>.md
```

If a specific person was named in the arguments, request their review:

```bash
gh pr edit <pr-number> --add-reviewer <username>
```

### Step 4: Update the Issue

If a related GitHub Issue exists, post a status comment:

```bash
gh issue comment <issue-number> --body "Handoff: <stage> complete. PR #<pr-number> ready for <next stage>."
```

### Step 5: Confirm

```
Handoff complete!

PR: <url>
Stage: <current> -> <next>
Reviewers: <assigned reviewers>
Issue updated: #<number>

Next step: <who> picks up with /<next skill>
```
