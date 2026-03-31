---
name: team:status
description: "Show what stage each piece of work is at by reading GitHub Issues, PRs, and artifact directories. Use when asking 'what is everyone working on', 'team status', 'what stage is X at', or 'show the board'."
argument-hint: "[optional: topic or issue number to focus on]"
---

# Team Status

Show the current state of all active work across the compound-engineering workflow stages.

## Execution

### Step 1: Gather data

Run these commands to collect the current state:

```bash
# Open Issues with workflow labels
gh issue list --state open --limit 20 --json number,title,labels,assignees,updatedAt

# Open PRs
gh pr list --state open --limit 20 --json number,title,author,labels,updatedAt,url

# Recent artifacts on main
git log --oneline --name-only -20 -- docs/brainstorms/ docs/plans/ docs/solutions/ docs/ideation/
```

### Step 2: Classify by stage

Map each item to a workflow stage:

| Stage | Signals |
|-------|---------|
| **Ideate** | Issue with `ideation` label, or recent file in `docs/ideation/` |
| **Brainstorm** | Issue with `brainstorm` label, PR touching `docs/brainstorms/`, or recent requirements doc |
| **Plan** | PR touching `docs/plans/`, or recent plan doc |
| **Work** | PR with code changes (not just docs), or Issue with `in-progress` label |
| **Review** | PR with review requests or review comments |
| **Compound** | PR touching `docs/solutions/`, or recent learning doc |

### Step 3: Present the board

Display a summary table:

```
Team Status
===========

| Stage      | Item                          | Owner   | Updated    |
|------------|-------------------------------|---------|------------|
| Brainstorm | #12 Better search             | @bob    | 2 days ago |
| Plan       | PR #15 Search plan            | @bob    | 1 day ago  |
| Work       | PR #18 Auth refactor          | @carol  | 3 hours ago|
| Review     | PR #16 Email templates        | @alice  | 1 day ago  |

No items in: Ideate, Compound
```

If the user passed a specific topic or issue number, focus on that item and show its full history across stages.

### Step 4: Suggest actions

Based on the board state, suggest what might need attention:
- PRs waiting for review with no reviewers assigned
- Issues in Ideate with no one picking them up
- Plans with no corresponding work PR yet
- Work items with no recent activity
