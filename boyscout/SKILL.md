---
name: boyscout
description: Clean up files touched by the current branch — fix DRY violations, performance issues, and minor improvements without changing structure or behavior
disable-model-invocation: true
---

Apply the Boy Scout Rule to the current branch: leave the code cleaner than you found it. Identify and fix DRY violations, performance concerns, and minor code quality issues in files touched by this branch. Do not change overall structure, behavior, or architecture.

---

## Step 1 — Identify touched files

Run these in parallel:
- `git branch --show-current` to get the current branch name
- `git diff main...HEAD --name-only` to list all files changed on this branch (try `origin/main` if `main` fails)

## Step 2 — Read and analyze the files

Read each changed file in full. For each file, scan for the following categories of issues:

### DRY Violations
- Repeated logic, expressions, or constants that appear more than once and could be extracted into a shared variable, helper function, or constant
- Copy-pasted blocks that differ only in minor parameterization
- Inline values (magic numbers, strings) used in multiple places

### Performance Concerns
- Unnecessary work inside loops (recomputing values that don't change, redundant lookups)
- Repeated calls to expensive operations where the result could be cached in a local variable
- Inefficient data structure choices for the access pattern in use (e.g., repeated linear scans of a list when a set would do)
- Unnecessary object allocations or copies in tight paths

### Minor Code Quality Improvements
- Overly complex conditionals that can be simplified or early-returned
- Variables or parameters that are declared but never used
- Intermediate variables that add no clarity and can be inlined
- Negated conditions that are harder to read than their positive form
- Any patterns that are non-idiomatic for the language/framework in use

## Step 3 — Triage findings

For each finding, assess:
- **Safe to fix?** The change must not alter observable behavior, public APIs, data model shape, or architectural patterns. If fixing it requires understanding cross-file impact you cannot fully verify, skip it.
- **Worth fixing?** Skip trivial one-word renames or stylistic preferences that are purely subjective. Focus on changes that meaningfully reduce redundancy or improve clarity/performance.

Set aside anything that is a larger refactor, architectural concern, or behavior change — those belong in a separate conversation, not here.

## Step 4 — Apply fixes

For each approved fix, use the Edit tool to apply it directly to the file. Do not rewrite files wholesale — make targeted, minimal edits.

Work file by file. After finishing all edits to a file, briefly verify the changes make sense in context before moving on.

## Step 5 — Commit the changes

Stage and commit all edited files using the structured diff-style format, with `BOYSCOUT:` prepended to the title.

**Title line rules:**
- Starts with `BOYSCOUT:` followed by a space and a descriptive title
- Descriptive (not imperative), no trailing period, no emojis
- Example: `BOYSCOUT: DRY and minor quality cleanup across branch files`

Then one blank line, then a structured body listing every changed file using change symbols:
- `+` Added (new helpers, extracted constants, new functions)
- `-` Removed (dead code, inlined variables)
- `~` Modified (simplified logic, consolidated duplication)

**Per-file format:**

Single change:
```
~ path/to/file.ext
  * <concise description of what changed>
```

Multiple changes:
```
~ path/to/file.ext
  ~ methodName(): <what changed>
  - unusedVar: Removed unused variable.
  * <any additional context>
```

Pass the message via heredoc:

```bash
git commit -m "$(cat <<'EOF'
BOYSCOUT: <title>

~ <file>
  <changes>
EOF
)"
```

If no files were changed (no actionable fixes found), skip this step.

## Step 6 — Report

After committing, output a concise summary grouped by file:

```
<filename>
  - <what was fixed and why>
  - <what was fixed and why>

<filename>
  - <what was fixed and why>
```

If a file had no actionable findings, omit it from the report.

If any findings were identified but skipped (too risky, too large, out of scope), list them at the end under a **Skipped (out of scope)** section with a one-line reason each.
