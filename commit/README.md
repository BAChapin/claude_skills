# commit

Stage and commit changes using a structured diff-style commit message format.

## What it does

`/commit` analyzes your working tree, decides whether changes should be split into multiple commits or kept as one, stages the appropriate files, and writes a commit message that follows this repo's structured format.

The commit message format uses a descriptive title (no imperative mood, no `feat:`/`fix:` prefixes) followed by a per-file change log using `+`, `-`, and `~` symbols. Non-code changes (docs, config) get a single title line with no body.

Example output:

```
Directory opt-in filtering implemented

~ DirectoryService.ts
  + filterVisibleMembers(): Added opt-in constraint for Member role.
  ~ getAllMembers(): Applied visibility filtering.
  * Admin and Treasurer roles remain unrestricted.

~ User.ts
  + optedIn: boolean
  * Introduced directory visibility flag.
```

## How to use

Run `/commit` from any project with uncommitted changes:

```
/commit
```

The skill will:
1. Run `git status`, `git diff`, and `git log` to understand the current state
2. Read changed files to understand the purpose of the changes
3. Decide whether to create one commit or multiple (based on logical separation of concerns)
4. Stage files and write a structured commit message
5. Confirm the working tree is clean

No arguments needed. If there are multiple separable units of work, the skill will plan and execute them as separate commits automatically.

## Install

**Global** (all projects):
```
~/.claude/skills/commit/SKILL.md
```

**Project-only:**
```
.claude/skills/commit/SKILL.md
```
