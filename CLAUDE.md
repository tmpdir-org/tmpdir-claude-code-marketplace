# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with
code in this repository.

## Repository Overview

This is a Claude Code plugin repository for "doc-driven-coding" - a plugin that
implements documentation-driven development workflow where markdown
documentation drives code implementation rather than the reverse.

## Architecture

### Plugin Structure

Claude Code plugins follow a specific directory structure:

```
doc-driven-development/
├── .claude-plugin/
│   └── plugin.json          # Plugin metadata (name, version, description, author)
├── commands/
│   ├── plan.md              # /plan command definition
│   ├── implement.md         # /implement command definition
│   ├── implement-tests.md   # /implement-tests command definition (TDD)
│   ├── execute.md           # /execute command definition
│   ├── update-docs.md       # /update-docs command definition
│   ├── review.md            # /review command definition (multi-agent code review)
│   ├── release.md           # /release command definition
│   └── skip.md              # /skip command definition
└── plans/                   # Implementation plans
    └── plans.md             # Plan tracking (one **IN PROGRESS** at a time)
```

**Key architectural points:**

1. **Command definitions are markdown files** - Each `.md` file in `commands/`
   becomes a slash command. The filename becomes the command name (e.g.,
   `plan.md` → `/plan`)

2. **Commands are prompts** - The markdown content is the prompt given to Claude
   Code when the command is invoked. Commands use `$ARGUMENTS` to access user
   input.

3. **Plugin metadata** - The `.claude-plugin/plugin.json` file defines the
   plugin's identity and must include: name, version, description, author
   fields, and optional keywords.

## Doc-Driven Development Workflow

The plugin implements a three-phase workflow:

1. **Documentation First** - Users update markdown files (README.md, design
   docs) to describe desired functionality
2. **Planning** - `/plan <description>` creates a plan file in
   `plans/<YYYY-MM-DD>-<description>.md` by analyzing git diff and documentation
3. **Implementation** - `/implement` reads documentation changes and plans, then
   implements corresponding code

**Important:** In this workflow, documentation drives code changes, not vice
versa. When implementing, prioritize what's documented over existing code
patterns.

### Plan Tracking

Plans are tracked in `plans/plans.md` using a markdown table with three columns:

```markdown
| Plan | Status | Started At |
|------|--------|------------|
| 2026-01-17-feature.md | **IN PROGRESS** | abc1234 |
```

The "Started At" column records the commit hash when the plan was created,
providing a reference point for reviewing all implementation changes.

Valid statuses:
- **IN PROGRESS** - Currently active plan
- **Paused** - Interrupted when a new plan started (can be resumed)
- Completed - Fully implemented and reviewed

Status transitions:
- `/plan` marks any existing in-progress plan as **Paused**, then adds the new
  plan as **IN PROGRESS** with the current commit hash.
- `/review` prompts user to mark plan as Completed when implementation is done.
- `/update-docs` finds the in-progress plan and updates it with implementation
  details.

### Commit Tracking

Individual plan files track implementation commits in a `## Commits` section:

```markdown
## Commits

| Hash | Description |
|------|-------------|
| abc1234 | feat: add authentication module |
| def5678 | test: add authentication tests |
```

`/implement` and `/implement-tests` automatically create granular commits and
record them in the plan file. Commits use conventional commit format (`feat:`,
`fix:`, `test:`, `docs:`, `refactor:`).

### Inline Instructions

The `/execute` command enables surgical code changes via inline markers:

- Insert `@CLAUDE:` tags in code comments or documentation with instructions or
  pseudo code
- Run `/execute` to find and implement all marked instructions
- Markers are automatically removed after execution
- Uncommitted plans are updated to reflect changes

This allows precise, targeted modifications without full documentation rewrites.

### Code Review

The `/review` command uses Claude Code's Task tool to spawn parallel review
agents. Each agent focuses on one aspect of code quality:

1. Formatting, Architecture, Documentation, Bugs, Code Clarity, Comments.
2. Results are appended to the **IN PROGRESS** plan file under `## Review (<date>)`.
3. After review, prompts for commit decision (auto-commit, manual, skip).
4. Then prompts for completion decision (mark Completed or continue iterating).
5. The Formatting agent auto-creates `FORMAT.md` if missing.

## Local Plugin Development

To test plugins locally during development:

1. Create a marketplace file at `/scratch/claude/marketplace.json`:

```json
{
	"name": "tmpdir",
	"plugins": [
		{
			"source": "file:///scratch/claude/doc-driven-coding"
		}
	]
}
```

2. Add marketplace: `/plugin marketplace add /scratch/claude/marketplace.json`
3. Install plugin: `/plugin install doc-driven-coding`

## Git Integration

Commands rely on `git diff` to detect documentation changes. The repository must
be a git repository for the `/plan` and `/implement` commands to function
properly.

## Release Management

The `/release` command automates version releases by:
- Updating the CHANGELOG.md from `[Unreleased]` to the release version
- Adding a new `[Unreleased]` section for future changes
- Updating version numbers in manifest files
- Creating a git tag

**Manifest files** (updated by `/release`):
- `.claude-plugin/plugin.json` - Plugin metadata containing `"version"` field
