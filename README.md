# Claude Skills

A collection of custom Claude Code skills for use across projects.

## What are Skills?

Skills are reusable prompt files that extend Claude Code with custom slash commands. Each skill lives in a directory with a `SKILL.md` file and is invoked via `/skill-name` in any Claude Code session.

## Installing Skills

You can ask Claude to install any skill from this repo. Use the prompt below, filling in the scope and skill(s) you want:

```
Please install the following skill(s) from this repo into [scope]:
- [skill-name]
- [skill-name]

To install a skill, create a SKILL.md file at the appropriate path:
- Global (available in all projects): ~/.claude/skills/<skill-name>/SKILL.md
- Project (available only in this project): .claude/skills/<skill-name>/SKILL.md

Copy the SKILL.md content exactly as-is from the source file.
```

### Example prompts

**Install all skills globally:**
```
Please install all skills from this repo globally (~/.claude/skills/). For each skill directory, copy its SKILL.md to ~/.claude/skills/<skill-name>/SKILL.md.
```

**Install a single skill for the current project:**
```
Please install the "commit" skill for this project by copying commit/SKILL.md to .claude/skills/commit/SKILL.md.
```

**Install a single skill globally:**
```
Please install the "pr-description" skill globally by copying pr-description/SKILL.md to ~/.claude/skills/pr-description/SKILL.md.
```

## Available Skills

See [SKILL_LIST.md](./SKILL_LIST.md) for a full list of skills and how to invoke them.
