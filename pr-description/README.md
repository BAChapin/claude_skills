# pr-description

Generate a detailed PR description for the current branch and write it to a markdown file.

## What it does

`/pr-description` reads all commits and diffs on the current branch relative to `main`, understands the purpose of each change, and produces a structured PR description with three sections:

- **Overview** — 2–4 sentence summary of what the PR does and why
- **Changes** — detailed breakdown by logical area, referencing file and function names
- **Testing Plan** — concrete checklist of manual steps, automated tests, and edge cases

The output is written to `docs/pr_<branch>.md` (with `/` and special characters in the branch name replaced by `_`).

## How to use

Run `/pr-description` while on the branch you want to describe:

```
/pr-description
```

The skill will:
1. Detect the current branch and gather all commits/diffs relative to `main`
2. Read changed files in depth to understand intent, not just line diffs
3. Draft and write the PR description to `docs/pr_<branch>.md`
4. Report the output path and a one-line summary of what the PR does

The `docs/` directory is created automatically if it does not exist.

## Install

**Global** (all projects):
```
~/.claude/skills/pr-description/SKILL.md
```

**Project-only:**
```
.claude/skills/pr-description/SKILL.md
```
