# boyscout

Clean up files touched by the current branch — fix DRY violations, performance issues, and minor code quality improvements without changing structure or behavior.

## What it does

`/boyscout` applies the Boy Scout Rule: leave the code cleaner than you found it. It scans every file the current branch has touched and makes safe, targeted improvements across three categories:

- **DRY Violations** — repeated logic, magic values, or copy-pasted blocks that can be extracted into a shared variable, constant, or helper
- **Performance Concerns** — unnecessary work inside loops, redundant expensive calls, inefficient data structure choices, avoidable allocations
- **Minor Code Quality** — overly complex conditionals, unused variables, unreadable negations, non-idiomatic patterns

Anything that would change behavior, alter public APIs, or require broader architectural decisions is skipped and noted as out of scope.

After applying fixes, the skill commits the changes with a `BOYSCOUT:` prefixed title using the same structured diff-style format as `/commit`.

## How to use

Run `/boyscout` from any branch with changes relative to `main`:

```
/boyscout
```

The skill will:
1. Identify all files touched by the current branch
2. Read and analyze each file for DRY violations, performance concerns, and minor quality issues
3. Triage findings — skipping anything unsafe or out of scope
4. Apply fixes with targeted edits
5. Commit all changes with a `BOYSCOUT:` tagged commit message
6. Report what was fixed and anything that was skipped

No arguments needed.

## Install

**Global** (all projects):
```
~/.claude/skills/boyscout/SKILL.md
```

**Project-only:**
```
.claude/skills/boyscout/SKILL.md
```
