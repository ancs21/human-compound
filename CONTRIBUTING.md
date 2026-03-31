# Contributing

human-compound is a Claude Code plugin — all deliverables are markdown files (skills and agents), not application code. There is no build system or test suite.

## How to contribute

1. Fork the repo
2. Create a feature branch (`feat/your-change`)
3. Make your changes
4. Open a PR against `main`

## What you can contribute

- **New skills** — add a directory under `skills/<name>/` with a `SKILL.md` file
- **New agents** — add a markdown file under `agents/<category>/`
- **Agent learnings** — calibration corrections for existing agents
- **GitHub templates** — improvements to issue/PR templates
- **Documentation** — workflow guides, setup docs

## Skill and agent format

Skills use YAML frontmatter with `name`, `description`, and `argument-hint`. See any file in `skills/` for examples.

Agents use YAML frontmatter with `name`, `description`, `model`, `tools`, and `color`. See any file in `agents/` for examples.

## Conventions

- Use `he:` prefix for core workflow skills
- Use `team:` prefix for team coordination skills
- Use `git-` prefix for git utility skills
- Follow conventional commits: `feat:`, `fix:`, `docs:`

## License

By contributing, you agree that your contributions will be licensed under the MIT License.
