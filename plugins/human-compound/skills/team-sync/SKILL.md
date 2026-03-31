---
name: team:sync
description: "Pull latest artifacts from teammates before starting work. Use when saying 'sync', 'pull latest', 'what did the team do', or before starting a new workflow stage."
---

# Team Sync

Pull the latest artifacts and context from teammates before starting work.

## Execution

### Step 1: Pull latest from main

```bash
git fetch origin
git log --oneline HEAD..origin/main --name-only -- docs/brainstorms/ docs/plans/ docs/solutions/ docs/ideation/
```

If there are new artifacts on main that are not in the current branch:

```bash
git merge origin/main
```

### Step 2: Show what's new

List artifacts that changed since the user's last activity:

```bash
# Recent artifact changes (last 7 days)
git log --since="7 days ago" --oneline --name-only -- docs/brainstorms/ docs/plans/ docs/solutions/ docs/ideation/
```

Present a summary:

```
Team activity (last 7 days)
===========================

New requirements:
  - docs/brainstorms/2026-04-01-search-requirements.md (by @bob, 2 days ago)

New plans:
  - docs/plans/2026-04-01-001-feat-search-plan.md (by @bob, 1 day ago)

New learnings:
  - docs/solutions/workflow/caching-strategy-2026-03-28.md (by @carol, 5 days ago)

Open PRs needing review:
  - PR #15: "Plan: search implementation" (by @bob, awaiting review)
```

### Step 3: Check for relevant context

If the user is about to start a specific task, search for related artifacts:

```bash
# Check if someone already brainstormed or planned this topic
gh issue list --search "<topic>" --state all --limit 5
```

Read any relevant requirements docs or plans to carry forward team decisions.

### Step 4: Report readiness

```
Sync complete!

Branch: <current branch>
Status: up to date with main
New artifacts: <count> since last sync

Ready to start work. Relevant context:
- <any related artifacts found>
```
