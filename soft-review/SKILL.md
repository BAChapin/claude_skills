---
name: soft-review
description: Last-minute pre-PR check — summarizes branch changes, validates against a goal if provided, flags performance/security/breaking concerns, and runs tests
disable-model-invocation: true
---

Perform a quick pre-PR sanity check on the current branch. Summarize what the changes accomplish, optionally validate against a stated goal, flag any performance, security, or breaking change concerns, and run the test suite.

The argument passed to this skill (if any) is the goal or task the branch is meant to accomplish: `$ARGUMENTS`

---

## Step 1 — Gather branch context

Run these in parallel:
- `git branch --show-current` to get the current branch name
- `git log main...HEAD --oneline` to see all commits on this branch (try `origin/main` if `main` fails)
- `git diff main...HEAD --stat` for a high-level summary of changed files
- `git diff main...HEAD` for the full diff

## Step 2 — Read changed files

Read each changed file in full to understand the actual implementation — not just what changed, but what it does and why.

## Step 3 — Summarize the branch

Write a clear, concise summary of what this branch accomplishes. Focus on:
- What feature, fix, or change is being introduced
- Which parts of the system are affected
- Any notable implementation decisions

Keep it to 3–6 sentences. This is the "what did we build" section.

## Step 4 — Goal validation (only if `$ARGUMENTS` is non-empty)

If a goal was provided, evaluate whether the branch fully accomplishes it:
- Does the implementation address everything stated in the goal?
- Is anything partially done or missing?
- Does anything in the code contradict or undermine the goal?

Produce a short **Goal Check** with a clear verdict: fully met, partially met, or not met — and explain any gaps.

## Step 5 — Concerns check

Review the diff for the following. Only report items that are actually present — skip any category where nothing was found.

### Performance Concerns
Anything that could cause slowness, excessive memory use, unnecessary re-renders, blocking calls, N+1 queries, or redundant expensive operations.

### Security Concerns
Unvalidated input, exposed credentials, insecure storage, missing auth checks, injection risks, or overly permissive access.

### Breaking Changes
Anything that could break existing functionality, API contracts, data models, or dependent systems. Flag these prominently — they are the most important category.

If none of these concerns are found, state clearly: **No concerns found.**

## Step 6 — Run tests

Look for tests in the repo by checking for common test indicators:
- Test directories: `Tests/`, `test/`, `tests/`, `spec/`, `__tests__/`
- Test config files: `Package.swift`, `jest.config.*`, `vitest.config.*`, `pytest.ini`, `go.mod`, `.rspec`, `Makefile`, etc.
- Test scripts in `package.json` under `"scripts"` (e.g., `"test"`)

If tests are found, determine the correct command and run them:
- **Swift/Xcode**: `xcodebuild test -scheme <SchemeName> 2>&1`
- **Node.js**: `npm test 2>&1` or `yarn test 2>&1`
- **Go**: `go test ./... 2>&1`
- **Python**: `pytest 2>&1` or `python -m pytest 2>&1`
- **Ruby**: `bundle exec rspec 2>&1` or `bundle exec rake test 2>&1`
- **Rust**: `cargo test 2>&1`
- **Other**: use the test command from the project's README, Makefile, or CI config

If all tests pass, note the count and mark as passing.

If any tests fail, list each failing test by name and include the relevant error output. Be specific — the goal is to make failures immediately actionable.

If tests cannot be run (missing environment, credentials, etc.), note the reason clearly.

If no tests exist, note that no test suite was found.

## Step 7 — Output

Print the full review to the terminal in this format:

```
## Branch Summary

<3–6 sentence summary of what the branch accomplishes>

---

## Goal Check
*(omit this section entirely if no goal was provided)*

**Verdict:** Fully Met / Partially Met / Not Met

<explanation of any gaps or mismatches>

---

## Concerns

### Performance
<findings, or omit section if none>

### Security
<findings, or omit section if none>

### Breaking Changes
<findings, or omit section if none>

No concerns found.
*(use this line only if all three categories are clear)*

---

## Test Results

**Command:** `<command run>`
**Result:** All X tests passed / X of Y tests failed

<if failures: list each failing test name and the error>
```

Keep the output scannable and direct. No storytelling, no padding. A developer should be able to read this in under two minutes and know whether the branch is ready to PR.
