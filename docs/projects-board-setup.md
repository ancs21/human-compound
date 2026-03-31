# GitHub Projects Board Setup

Set up a GitHub Projects v2 board to track work through the human-compound stages.

## Quick setup via CLI

Requires `gh` CLI with the `project` scope (`gh auth refresh -s project`).

```bash
# Replace YOUR-ORG with your GitHub org or username
OWNER="YOUR-ORG"

# Create the project
PROJECT_NUM=$(gh project create --owner "$OWNER" --title "Engineering Board" --format json | jq -r '.number')
echo "Created project #$PROJECT_NUM"

# Add a "Stage" field with workflow columns
gh project field-create "$PROJECT_NUM" --owner "$OWNER" \
  --name "Stage" \
  --data-type "SINGLE_SELECT" \
  --single-select-options "Ideate,Brainstorm,Plan,Work,Review,Done"

# Create labels in your repo
gh label create "ideation"    -c "7057ff" -d "Idea or proposal" --force
gh label create "brainstorm"  -c "0075ca" -d "Requirements exploration" --force
gh label create "planning"    -c "008672" -d "Implementation planning" --force
gh label create "in-progress" -c "e4e669" -d "Active development" --force
gh label create "review"      -c "d876e3" -d "Awaiting review" --force
gh label create "learning"    -c "0e8a16" -d "Knowledge capture" --force
```

Then open the board in your browser to switch to Board layout and group by "Stage":

```bash
gh project view "$PROJECT_NUM" --owner "$OWNER" --web
```

## Manual setup via GitHub UI

1. Go to your repo -> **Projects** tab -> **New project**
2. Choose **Board** layout
3. Name it: **Engineering Board** (or your preference)

### Configure columns

Create 6 columns (Status field values) in this order:

| Column | What goes here |
|--------|---------------|
| **Ideate** | Issues with `ideation` label |
| **Brainstorm** | Issues with `brainstorm` label, PRs with requirements docs |
| **Plan** | PRs with plan docs |
| **Work** | Code PRs, feature branches |
| **Review** | PRs awaiting review |
| **Done** | Merged PRs, captured learnings |

### Add labels

Create these labels under **Settings -> Labels** (or use the CLI commands above):

| Label | Color | Description |
|-------|-------|-------------|
| `ideation` | `#7057ff` | Idea or proposal |
| `brainstorm` | `#0075ca` | Requirements exploration |
| `planning` | `#008672` | Implementation planning |
| `in-progress` | `#e4e669` | Active development |
| `review` | `#d876e3` | Awaiting review |
| `learning` | `#0e8a16` | Knowledge capture |

## Usage tips

- **Don't over-manage the board.** For 2-3 people, a quick daily glance is enough.
- **Not everything needs to be on the board.** Small fixes and chores can skip it.
- **Move cards when you context-switch.** If you stop working on something, move it back so others know.
- **Archive Done items weekly** to keep the board clean.
