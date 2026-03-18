---
name: work-review
description: Review branch changes against optional requirements, then perform a senior-level code review with findings written to docs/review_<branch>.md
disable-model-invocation: true
---

Perform a thorough code review of the current branch. If a prompt was provided when invoking this skill, treat it as the branch's requirements and validate compliance first. Then perform a senior-level code review based on the detected tech stack.

The argument passed to this skill (if any) is the requirements prompt: `$ARGUMENTS`

---

## Step 1 — Gather branch context

Run these in parallel:
- `git branch --show-current` to get the current branch name
- `git log main...HEAD --oneline` to see all commits on this branch (try `origin/main` if `main` fails)
- `git diff main...HEAD --stat` for a high-level summary of changed files
- `git diff main...HEAD` for the full diff

## Step 2 — Detect the tech stack

Read a representative sample of changed files to identify:
- Primary programming language(s)
- Frameworks and libraries in use
- Architectural patterns (MVC, MVVM, VIPER, etc.)
- Testing approach
- Any config, CI/CD, or infrastructure files

This determines how you'll frame your review (e.g., "Senior SwiftUI Engineer", "Senior React Engineer", "Senior Go Engineer").

## Step 3 — Requirements compliance check (only if `$ARGUMENTS` is non-empty)

If requirements were provided, evaluate the branch against each requirement:

- Go through each stated requirement one by one
- Determine whether the branch addresses it — fully, partially, or not at all
- For partially or unmet requirements, describe exactly what is missing or incomplete
- Note any code that appears to contradict or undermine a requirement

Produce a **Requirements Check** section with a clear pass/fail/partial status per requirement.

## Step 4 — Senior code review

Review the changed code as a Senior Engineer in the detected tech stack. Evaluate across these categories:

### Good Decisions
Things that were architecturally sound, well-implemented, or worth calling out as best practice. Be specific — name the file, function, or pattern.

### Improvements
Things that work but could be cleaner, more idiomatic, or better structured. Include concrete suggestions.

### DRY Violations
Logic or structure that is duplicated and should be extracted or consolidated. Reference specific locations.

### Performance Concerns
Anything that could cause slowness, excessive memory use, unnecessary re-renders, blocking calls, N+1 queries, etc.

### Security Concerns
Any potential vulnerabilities: unvalidated input, exposed credentials, insecure storage, missing auth checks, injection risks, etc.

### Breaking Changes
Anything that could break existing functionality, API contracts, data models, or dependent systems. Flag these prominently.

### Nitpicks (optional)
Minor style or naming issues not worth blocking a PR on, but worth mentioning for future polish.

## Step 5 — Run tests

Look for tests in the repo by checking for common test indicators:
- Test directories: `Tests/`, `test/`, `tests/`, `spec/`, `__tests__/`
- Test config files: `Package.swift` (Swift), `jest.config.*`, `vitest.config.*`, `pytest.ini`, `go.mod`, `.rspec`, `Makefile`, etc.
- Test scripts in `package.json` under `"scripts"` (e.g., `"test"`)

If tests are found, determine the correct command for the tech stack and run them:
- **Swift/Xcode**: `xcodebuild test -scheme <SchemeName> 2>&1` (find the scheme from `Package.swift` or the `.xcodeproj`)
- **Node.js**: `npm test 2>&1` or `yarn test 2>&1`
- **Go**: `go test ./... 2>&1`
- **Python**: `pytest 2>&1` or `python -m pytest 2>&1`
- **Ruby**: `bundle exec rspec 2>&1` or `bundle exec rake test 2>&1`
- **Rust**: `cargo test 2>&1`
- **Other**: use the test command from the project's README, Makefile, or CI config if available

Capture the full output including pass/fail counts, any failing test names, and error messages. If tests cannot be run (missing environment, credentials, simulator not available, etc.), note the reason clearly instead of failing silently.

If no tests exist in the repo, note that no test suite was found.

## Step 6 — Write the review file

Determine the output path:
- Branch name from Step 1, with `/` and special characters replaced by `_`
- File: `docs/review_<branch>.md`
- Create the `docs/` directory if it does not exist

Write the full review as a markdown file with this structure:

```markdown
# Code Review: <branch name>

**Reviewer Role:** Senior <Technology> Engineer
**Date:** <today's date>
**Branch:** <branch name>
**Commits Reviewed:** <count> commit(s)

---

## Requirements Check
*(omit this section entirely if no requirements were provided)*

| Requirement | Status | Notes |
|-------------|--------|-------|
| <requirement> | ✅ Met / ⚠️ Partial / ❌ Not Met | <details> |

<Any further detail on gaps or issues found during requirements review>

---

## Code Review

### Good Decisions

<findings>

### Improvements

<findings>

### DRY Violations

<findings>

### Performance Concerns

<findings>

### Security Concerns

<findings>

### Breaking Changes

<findings>

### Nitpicks

<findings>

---

## Test Results

*(omit this section if no test suite was found)*

**Command run:** `<test command>`
**Result:** ✅ All tests passed / ❌ Tests failed / ⚠️ Could not run tests

```
<full test output>
```

<If tests failed, summarize which tests failed and what the errors indicate>

---

## Summary

<2–4 sentence overall assessment: is this branch ready to merge, what are the most important things to address, and any final notes>
```

## Step 7 — Confirm

Report the path of the file written and give a one-line summary of the review's overall verdict.
