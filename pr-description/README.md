# pr-description

Generate a detailed PR description for the current branch and write it to a markdown file.

## What it does

`/pr-description` detects the repo's default branch, reads all commits and diffs on the current branch relative to it, understands the purpose of each change, and produces a structured PR description with three sections:

- **Overview** — 2–4 sentence summary of what the PR does and why
- **Changes** — high-level breakdown by logical area; skips cosmetic noise (single renames) and calls out patterns and fundamental logic changes instead of line-by-line detail
- **Testing Plan** — a path to the relevant part of the app, concrete steps to test the new/changed functionality, and a list (not a script) of edge cases to check — no repo setup or CI-covered automation

The output is written to `docs/pr_<branch>.md` (with `/` and special characters in the branch name replaced by `_`). If `README.md` hasn't been updated to reflect the change, a warning callout is added at the top of the file.

## How to use

Run `/pr-description` while on the branch you want to describe:

```
/pr-description
```

The skill will:
1. Detect the repo's default branch and current branch, then gather all commits/diffs relative to the default branch
2. Read changed files in depth to understand intent, not just line diffs
3. Draft the Overview, Changes, and Testing Plan sections
4. Check whether `README.md` is up to date with the changes
5. Write the PR description to `docs/pr_<branch>.md`
6. Report the output path and a one-line summary of what the PR does

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
