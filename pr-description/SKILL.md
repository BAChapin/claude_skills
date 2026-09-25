---
name: pr-description
description: Review branch changes and generate a detailed PR description markdown file with Overview, Changes, and Testing Plan sections
disable-model-invocation: true
---

Generate a detailed PR description for the current branch and write it to a markdown file.

## Step 1 — Gather branch context

First, determine the repo's default branch — don't assume it's `main` or `master`:
- Try `git symbolic-ref refs/remotes/origin/HEAD --short` and strip the `origin/` prefix
- If that fails, try `gh repo view --json defaultBranchRef -q .defaultBranchRef.name`
- If both fail, fall back to `main`, then `master`

Then run these in parallel, using that default branch as `<default>`:
- `git branch --show-current` to get the current branch name
- `git log <default>...HEAD --oneline` to see all commits on this branch
- `git diff <default>...HEAD --stat` to get a high-level summary of changed files

## Step 2 — Read the changes in depth

Run these in parallel:
- `git diff <default>...HEAD` to see the full diff of all changes
- Read any changed files that need deeper understanding beyond the diff

Understand the *purpose* of each change, not just what lines changed. Look for:
- What problem is being solved
- What new behavior is introduced
- What existing behavior is modified or removed
- Any configuration, migration, or dependency changes

## Step 3 — Draft the PR description

Write a thorough PR description with the following sections:

---

### Overview

A concise summary (2–4 sentences) explaining:
- **What** this PR does at a high level
- **Why** it's needed (the problem or motivation)
- Any important context or constraints

### Changes

A high-level breakdown of what changed, organized by logical area, written with abstraction rather than a play-by-play of the diff. For each area, describe the fundamentals of what/how it changed — not a detailed line-by-line account. Apply this filter to decide what's worth mentioning:
- **Light, cosmetic changes** (a single variable, method, or class renamed) — skip it, unless the same kind of change repeats across several names, in which case call out the pattern (e.g. "renamed several methods in X for clarity") rather than listing each one.
- **Large logic changes** to a class or method — call out only what fundamentally changed (the approach, behavior, or responsibility), not a detailed change report.
- **New data types/models added** — no justification needed; assume it was necessary for the story. Only explain the addition if its purpose isn't obvious from its name/shape.
- **Breaking changes or removed behavior** — always call these out, since they affect consumers.

Use bullet points or sub-sections as appropriate. Reference file, function, or component names only where it aids understanding — don't restate the diff.

### Testing Plan

Instructions for testing the change, not the repo. Do not include:
- Standard repo setup or how to run the project — assume the reader already knows this
- Automated tests, unless this PR added automation where none previously existed — if the tests run in CI/the pipeline, a failure is already visible, so it doesn't need to be called out here

Structure the plan as:
- A high-level path to reach the part of the running app/project where the change lives (not a detailed setup guide)
- Detailed, concrete steps to exercise the new or modified functionality specifically
- A simple list of edge cases or failure scenarios worth checking — name each one, don't spell out the steps to test it

---

## Step 4 — Confirm the README is up to date

Check whether `README.md` reflects the changes made in this branch (new features, changed behavior, updated setup/usage, etc.). If it's out of date, note that — this will be flagged in Step 5.

## Step 5 — Write the file

Determine the output path from the branch name:
- Take the current branch name from Step 1
- Replace any `/` or special characters with `_`
- File name format: `pr_<branch>.md`
- Write the file to the `docs/` directory, creating it if it does not exist

Write the full markdown file with this structure. If Step 4 found the README out of date, add a callout at the very top of the file, above the title:

```markdown
> ⚠️ **README.md is out of date** — update it to reflect the changes in this PR.

# PR: <branch name>

## Overview

<overview content>

## Changes

<changes content>

## Testing Plan

<testing plan content>
```

Omit the callout entirely if the README is already up to date.

## Step 6 — Confirm

Report the filename that was written and give a one-line summary of what the PR does. If the README callout was added, mention that too.
