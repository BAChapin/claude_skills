# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## What This Repo Is

A collection of custom Claude Code skills — reusable prompt files that extend Claude Code with custom slash commands. Each skill lives in a directory as `<skill-name>/SKILL.md`.

## Skill Structure

Each skill directory contains a single `SKILL.md` with YAML frontmatter:

```markdown
---
name: <skill-name>
description: <one-line description shown in skill list>
disable-model-invocation: true   # optional, prevents recursive invocation
---

<skill prompt content>
```

## Installing Skills

Skills can be installed at two scopes:
- **Global** (all projects): `~/.claude/skills/<skill-name>/SKILL.md`
- **Project** (current project only): `.claude/skills/<skill-name>/SKILL.md`

To install, copy the `SKILL.md` file exactly as-is to the appropriate path.

## Maintaining the Skill Index

`SKILL_LIST.md` is the canonical index of all available skills. When adding or updating a skill, update `SKILL_LIST.md` to reflect the skill name, invoke command (`/<skill-name>`), and description.

## Commit Message Standard

This repo uses a structured diff-style commit format (see `commit/SKILL.md` for the full spec). Key rules:
- Descriptive title (not imperative), no trailing period, no prefixes like `feat:`/`fix:`
- Body lists every changed file with `+` (added), `-` (removed), `~` (modified) symbols
- Non-code changes (docs, config) use a single title line with no body
- Pass message via heredoc to preserve formatting
