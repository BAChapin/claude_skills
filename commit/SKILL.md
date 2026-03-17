---
name: commit
description: Stage and commit changes using the structured diff-style commit message standard
disable-model-invocation: true
---

Stage and commit the current changes following the commit message standard below.

## Step 1 — Gather context

Run these in parallel:
- `git status` to see staged and unstaged changes
- `git diff` and `git diff --staged` to understand what changed
- `git log --oneline -5` to see recent commit style for reference

## Step 2 — Analyze changes and plan commits

Read the changed files in full to understand what was actually implemented — not just file names, but the actual logic and purpose of the changes.

Then decide: **one commit or multiple?**

Split into multiple commits when there are clearly separable features or concerns — for example, a new API integration alongside an unrelated UI fix, or a data model change alongside a separate auth flow update. Each commit should represent one coherent unit of work that could be understood and reviewed independently.

Keep as a single commit when the changes are tightly coupled and tell one story — touching many files is not itself a reason to split.

**If splitting:** plan the full set of commits before staging anything. Determine which files (and which changes within files) belong to each commit. Stage and commit them one at a time in logical order — foundational changes before dependent ones.

**If not splitting:** proceed to Step 3 with all changes as one commit.

## Step 3 — Stage files

Stage the files for the current commit. Prefer staging specific files by name over `git add -A` or `git add .` to avoid accidentally including unintended files.

## Step 4 — Write the commit message

Determine whether this commit involves **code changes** or is **non-code only** (docs, config, CI/CD, formatting).

---

### Non-code commits (docs, config, CI/CD, formatting-only)

Use a single descriptive title line. No body required.

```
Update README with local setup instructions
```

---

### Code commits — required format

**Title line rules:**
- Descriptive (not imperative)
- No trailing period
- No emojis
- No prefixes (feat:, fix:, etc.)
- One short line only

Then one blank line, then a structured body listing every changed file.

**Change symbols:**
- `+` Added (new files, methods, logic, types, tests)
- `-` Removed (dead code, deprecated logic, deleted files)
- `~` Modified (refactored, changed logic, updated signatures)

**Single change in a file:**
```
~ DirectoryService.updateVisibility
  * Added opt-in filtering logic.
```

**Multiple changes in a file:**
```
~ DirectoryService.ts
  + updateVisibility(): Added role-based filtering.
  - legacyVisibilityCheck(): Removed deprecated logic.
  ~ getVisibleMembers(): Improved role handling.
  * Consolidated directory visibility rules.
```

**Test files:** List the suite file once, methods indented underneath.

**Formatting rules (strict):**
- One blank line between title and body
- Two spaces before explanation bullets
- `*` used for explanations
- Every changed file must be listed — no grouping, no shorthand
- Explanations concise and technical
- No storytelling, no noise, no redundancy, no emojis
- No ticket references unless explicitly requested

**Full example:**
```
Directory opt-in filtering implemented

~ DirectoryService.ts
  + filterVisibleMembers(): Added opt-in constraint for Member role.
  ~ getAllMembers(): Applied visibility filtering.
  * Admin and Treasurer roles remain unrestricted.

~ User.ts
  + optedIn: boolean
  * Introduced directory visibility flag.

~ DirectoryServiceTests.ts
  + testOptInFiltering(): Verifies Members cannot see opted-out users.
  ~ testAdminAccess(): Updated to reflect unrestricted access.
```

---

## Step 5 — Commit

Pass the message via heredoc to ensure correct formatting:

```bash
git commit -m "$(cat <<'EOF'
<title>

<body>
EOF
)"
```

If creating multiple commits, repeat Steps 3–5 for each planned commit in order.

## Step 6 — Confirm

Run `git status` to verify the working tree is clean and all commits succeeded.
