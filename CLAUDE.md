# enjoy

A consumer (B2C) Chrome extension that makes AI calls. Open source (MIT); this repo is public, so no secrets, keys or private data in it. Stage: idea, no code yet.

<!-- BEGIN BEADS INTEGRATION v:1 profile:minimal hash:1105d646 -->
## Beads Issue Tracker

This project uses **bd (beads)** for issue tracking. Run `bd prime` to see full workflow context and commands.

### Quick Reference

```bash
bd ready              # Find available work
bd show <id>          # View issue details
bd update <id> --claim  # Claim work
bd close <id>         # Complete work
```

### Rules

- Use `bd` for ALL task tracking — do NOT use TodoWrite, TaskCreate, or markdown TODO lists
- Run `bd prime` for detailed command reference and session close protocol
- Use `bd remember` for persistent knowledge — do NOT use MEMORY.md files

**Architecture in one line:** issues live in a local Dolt DB; sync uses `refs/dolt/data` on your git remote; `.beads/issues.jsonl` is a passive export. See https://github.com/gastownhall/beads/blob/main/docs/core-concepts/sync-concepts.md for details and anti-patterns.

## Agent Context Profiles

The managed Beads block is task-tracking guidance, not permission to override repository, user, or orchestrator instructions.

- **Conservative (default)**: Use `bd` for task tracking. Do not run git commits, git pushes, or Dolt remote sync unless explicitly asked. At handoff, report changed files, validation, and suggested next commands.
- **Minimal**: Keep tool instruction files as pointers to `bd prime`; use the same conservative git policy unless active instructions say otherwise.
- **Team-maintainer**: Only when the repository explicitly opts in, agents may close beads, run quality gates, commit, and push as part of session close. A current "do not commit" or "do not push" instruction still wins.

## Session Completion

This protocol applies when ending a Beads implementation workflow. It is subordinate to explicit user, repository, and orchestrator instructions.

1. **File issues for remaining work** - Create beads for anything that needs follow-up
2. **Run quality gates** (if code changed) - Tests, linters, builds
3. **Update issue status** - Close finished work, update in-progress items
4. **Handle git/sync by active profile**:
   ```bash
   # Conservative/minimal/default: report status and proposed commands; wait for approval.
   git status

   # Team-maintainer opt-in only, unless current instructions forbid it:
   git pull --rebase
   git push
   git status
   ```
5. **Hand off** - Summarize changes, validation, issue status, and any blocked sync/commit/push step

**Critical rules:**
- Explicit user or orchestrator instructions override this Beads block.
- Do not commit or push without clear authority from the active profile or the current user request.
- If a required sync or push is blocked, stop and report the exact command and error.
<!-- END BEADS INTEGRATION -->

<!-- meerkit:section -->
## Team sessions
Roles: `pm`, `tech-lead`. Each lives in `.claude/skills/<role>/SKILL.md`; read yours first.
Task board: beads (`bd`). Cards are the spec and the record; chat is not.
Card statuses: `shaping`, `approved`, `ready_to_build`, `in_progress`, `in_review`, `blocked`, `waiting_owner`, `closed`. Pull cards with `bd list --status=approved --sort priority`; `bd ready` lists only `open` tasks.
Labels: `track:fast|full`, `appetite:small|medium|large`, `decision`, `eng-task`, `eng-found`, `oncall`, `tl-state`.
Owner question: a card with label `decision` and status `waiting_owner`: `bd create --type=task --status=waiting_owner -l decision --title ... --description ...`.
Settings: `.claude/meerkit/settings.json` overrides the plugin defaults; read a value with the settings command in your role's Project section.
Folders: `docs/adr/`, `docs/design/mocks/`, `docs/retro/`, `docs/forum/`. Evidence at `settings.evidence_dir`, secrets at `settings.secrets_dir` (names only in the repo).
<!-- /meerkit:section -->
