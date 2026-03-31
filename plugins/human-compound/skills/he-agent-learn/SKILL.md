---
name: he:agent-learn
description: "Record a learning for a specific agent — false positives, missed findings, calibration corrections. Use when saying 'that was wrong', 'false positive', 'the agent missed this', 'teach the agent', or after dismissing a review finding."
argument-hint: "[agent name and what it got wrong, e.g. 'security-reviewer flagged CSRF but Rails handles it']"
---

# Agent Learning

Record calibration feedback for a specific agent so it does not repeat the same mistake.

## How It Works

Each agent has a `## Learnings` section at the bottom of its `.md` file. This skill appends a new entry there. The agent reads its own file (including learnings) every time it runs.

Team-level knowledge goes in `docs/solutions/` via `/he:compound`. Agent-level calibration goes here — what the agent gets wrong and how to adjust.

## Input

<learning_input> #$ARGUMENTS </learning_input>

If the input is empty, ask: "Which agent got something wrong, and what happened?"

## Execution

### Step 1: Identify the agent

Parse the input to determine which agent needs the learning. Match against agents in the `agents/` directory.

If ambiguous, search for the agent:

```bash
find agents/ -name "*.md" -type f
```

Present matches and ask the user to confirm using the platform's question tool (`AskUserQuestion` in Claude Code, `request_user_input` in Codex, `ask_user` in Gemini).

### Step 2: Classify the learning

Determine the type:

| Type | Signal | Example |
|------|--------|---------|
| **False positive** | Agent flagged something that was not a real issue | "Security-reviewer flagged CSRF but Rails handles it" |
| **Missed finding** | Agent should have caught something but did not | "Correctness-reviewer missed the nil check on line 42" |
| **Wrong severity** | Agent flagged at wrong priority level | "Performance-reviewer flagged as P0 but it was P2" |
| **Wrong advice** | Agent suggested an incorrect fix | "Testing-reviewer suggested mocking the DB but we use integration tests" |

### Step 3: Read the agent file

Read the target agent's `.md` file. Check if a `## Learnings` section already exists at the bottom.

### Step 4: Write the learning

Append to the `## Learnings` section. If the section does not exist, create it at the end of the file.

Format each entry as:

```markdown
- YYYY-MM-DD [type]: Description of what happened and what the agent should do differently.
```

Examples:

```markdown
## Learnings

- 2026-04-01 [false-positive]: Do not flag CSRF token checks as missing in Rails apps. The framework handles this automatically via protect_from_forgery.
- 2026-04-05 [wrong-severity]: bcrypt cost factor 12 is acceptable for most apps. Only flag as P0 when the app handles financial or healthcare data.
- 2026-04-08 [missed]: When reviewing auth middleware, also check that API key rotation is handled. Missed this in PR #42.
- 2026-04-10 [wrong-advice]: Do not recommend mocking the database in this project. The team uses integration tests with real DB connections per docs/solutions/workflow/testing-strategy.md.
```

### Step 5: Check for duplicates

Before writing, scan existing learnings for semantic duplicates. If a similar learning already exists:
- Update the existing entry with the new context
- Do not create a duplicate

### Step 6: Confirm

```
Learning recorded for [agent-name].

Type: [false-positive | missed | wrong-severity | wrong-advice]
Entry: [the learning text]
File: agents/[category]/[agent-name].md

The agent will use this learning on its next run.
```

## Pruning

Over time, learnings may become outdated (e.g., the codebase changed, a dependency was removed). When an agent's learnings section grows beyond 15 entries:

- Review entries older than 90 days
- Remove entries that reference code, patterns, or dependencies that no longer exist
- Keep entries about general calibration that are still relevant

Pruning happens automatically when adding a new learning and the count exceeds 15. It can also be triggered manually: `/he:agent-learn prune [agent-name]`.

## Bulk Learning from Review

After `/he:review` completes in interactive mode, if the user dismisses findings or marks them as false positives, suggest: "Want to record these as agent learnings so they don't repeat?"

If yes, batch-create learnings for each dismissed finding, routing each to the correct agent.
