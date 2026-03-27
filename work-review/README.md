# work-review

Perform a senior-level code review of the current branch and write the findings to `docs/review_<branch>.md`.

## What it does

`/work-review` reviews all changes on the current branch relative to `main`. It detects your tech stack and frames the review from the perspective of a senior engineer in that stack (e.g., "Senior SwiftUI Engineer", "Senior React Engineer").

The review covers:

- **Good Decisions** — patterns and implementations worth calling out positively
- **Improvements** — things that work but could be cleaner or more idiomatic
- **DRY Violations** — duplicated logic that should be extracted
- **Performance Concerns** — slow queries, unnecessary re-renders, blocking calls, etc.
- **Security Concerns** — unvalidated input, exposed credentials, missing auth checks, etc.
- **Breaking Changes** — anything that could break existing contracts or dependent systems
- **Nitpicks** — minor style issues not worth blocking a PR on

The skill also runs the project's test suite (if one exists) and includes the results.

Output is written to `docs/review_<branch>.md`.

## How to use

**Without requirements** — review the branch on its own merits:

```
/work-review
```

**With requirements** — validate compliance before the code review:

```
/work-review Users should be able to reset their password via email. The flow must handle expired tokens gracefully.
```

When requirements are provided, the review includes a Requirements Check table showing pass/partial/fail status per requirement before the code review sections.

## Install

**Global** (all projects):
```
~/.claude/skills/work-review/SKILL.md
```

**Project-only:**
```
.claude/skills/work-review/SKILL.md
```
