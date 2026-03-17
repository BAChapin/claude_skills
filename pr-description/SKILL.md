---
name: pr-description
description: Review branch changes and generate a detailed PR description markdown file with Overview, Changes, and Testing Plan sections
disable-model-invocation: true
---

Generate a detailed PR description for the current branch and write it to a markdown file.

## Step 1 — Gather branch context

Run these in parallel:
- `git branch --show-current` to get the current branch name
- `git log main...HEAD --oneline` (or `git log origin/main...HEAD --oneline` if needed) to see all commits on this branch
- `git diff main...HEAD --stat` to get a high-level summary of changed files

If `main` doesn't exist, try `master` or `origin/HEAD` as the base branch.

## Step 2 — Read the changes in depth

Run these in parallel:
- `git diff main...HEAD` to see the full diff of all changes
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

A detailed breakdown of what was changed, organized by logical area or file group. For each area:
- Describe what changed and why
- Note any non-obvious decisions or trade-offs
- Call out breaking changes, renamed interfaces, or removed behavior

Use bullet points or sub-sections as appropriate. Be specific — reference file names, function names, or component names where relevant.

### Testing Plan

A concrete checklist of how to verify this PR works correctly. Include:
- Manual testing steps (user-facing flows to exercise)
- Automated tests added or updated
- Edge cases or failure scenarios to check
- Any environment setup, feature flags, or seed data required

---

## Step 4 — Write the file

Determine the output filename from the branch name:
- Take the current branch name from Step 1
- Replace any `/` or special characters with `_`
- File name format: `pr_<branch>.md`
- Write the file to the current working directory

Write the full markdown file with this structure:

```markdown
# PR: <branch name>

## Overview

<overview content>

## Changes

<changes content>

## Testing Plan

<testing plan content>
```

## Step 5 — Confirm

Report the filename that was written and give a one-line summary of what the PR does.
