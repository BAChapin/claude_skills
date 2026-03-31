# soft-review

Last-minute pre-PR check — summarizes branch changes, validates against a goal if provided, flags performance/security/breaking concerns, and runs tests.

## What it does

`/soft-review` is a quick sanity check to run before opening a pull request. It reads all changes on the current branch and produces a focused, scannable report printed directly to the terminal.

The review covers:

- **Branch Summary** — a concise description of what the branch accomplishes and which parts of the system are affected
- **Goal Check** — if a goal is provided, validates whether the branch fully meets it and calls out any gaps
- **Performance Concerns** — slow queries, unnecessary re-renders, blocking calls, redundant expensive operations, etc.
- **Security Concerns** — unvalidated input, exposed credentials, missing auth checks, injection risks, etc.
- **Breaking Changes** — anything that could break existing functionality, API contracts, data models, or dependent systems
- **Test Results** — runs the project's test suite and lists any failing tests by name with their error output

Only concerns that are actually present are reported. If the branch is clean, it says so.

## How to use

**Without a goal** — review the branch on its own merits:

```
/soft-review
```

**With a goal** — validate that the branch accomplishes the stated task:

```
/soft-review Add user authentication with JWT tokens and protect all private routes
```

When a goal is provided, the review includes a Goal Check section with a verdict of fully met, partially met, or not met, along with an explanation of any gaps.

## Install

**Global** (all projects):
```
~/.claude/skills/soft-review/SKILL.md
```

**Project-only:**
```
.claude/skills/soft-review/SKILL.md
```
