# story-summary

Analyze a Jira story or ticket and produce career-development artifacts.

## What it does

`/story-summary` takes a Jira work item — by ID or pasted content — and produces a structured analysis designed for resume writing, performance reviews, promotion packets, and annual reviews.

It produces seven sections:

1. **Who Is This Work For?** — identifies stakeholders and beneficiaries
2. **What Is The Ask?** — the business or technical problem being solved
3. **What Should The Solution Accomplish?** — intended outcomes and organizational value
4. **Single Paragraph Summary** — a polished paragraph ready for a performance review or promotion packet
5. **Resume Signal** — 1–3 action-oriented, accomplishment-focused resume bullet candidates
6. **Engineering Competencies Demonstrated** — which competency areas the work touches and why
7. **Future Resume Notes** — details to capture now before they're forgotten (scale, metrics, teams involved, etc.)

## How to use

Provide a Jira story ID:

```
/story-summary SWE-1234
```

Or paste the story content directly:

```
/story-summary

<paste ticket title, description, acceptance criteria, etc.>
```

If a Jira ID is provided, the skill checks the current conversation for story details before making an API call. If the story hasn't been fetched yet, it retrieves it from Jira automatically.

## Install

**Global** (all projects):
```
~/.claude/skills/story-summary/SKILL.md
```

**Project-only:**
```
.claude/skills/story-summary/SKILL.md
```
