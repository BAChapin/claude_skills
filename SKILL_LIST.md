# Skill List

| Skill | Invoke | Description |
|-------|--------|-------------|
| commit | `/commit` | Stage and commit changes using a structured diff-style commit message format. Analyzes changes, decides whether to split into multiple commits, and writes detailed per-file change messages. |
| pr-description | `/pr-description` | Review all branch commits and generate a detailed PR description markdown file with Overview, Changes, and Testing Plan sections. |
| work-review | `/work-review` | Review branch changes against optional requirements, then perform a senior-level code review with findings written to docs/review_<branch>.md |
| boyscout | `/boyscout` | Clean up files touched by the current branch — fix DRY violations, performance issues, and minor improvements without changing structure or behavior |
| soft-review | `/soft-review` | Last-minute pre-PR check — summarizes branch changes, validates against a goal if provided, flags performance/security/breaking concerns, and runs tests |
