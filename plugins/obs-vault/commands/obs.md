---
description: "Obsidian vault memory commands — shorthand for all vault operations"
argument-hint: "<command> [args] — commands: analyze, recap, project, note, todo, init, lookup, relate, setup"
aliases: [obs]
---

# Obs Vault Command

[OBS VAULT ACTIVATED - OBSIDIAN MEMORY ROUTER]

You are now running the Obs Vault command router. This routes `/obs <subcommand>` to the appropriate obs-vault skill or operation.

## User Input

{{ARGUMENTS}}

## Routing Protocol

Execute these phases in order.

---

### Phase 1: Parse Input

Extract from `{{ARGUMENTS}}`:

- **subcommand**: The first word (e.g., `init`, `analyze`, `recap`)
- **args**: Everything after the subcommand

If `{{ARGUMENTS}}` is empty or blank, go to Phase 3 (show help).

---

### Phase 2: Route Subcommand

| Subcommand | What it does |
|------------|-------------|
| `init [path]` | Create a new vault (redirects to `/obs-setup`) |
| `analyze` | Deep-analyze current project, populate vault |
| `recap` | Write session summary |
| `project [name]` | Scaffold a new project in vault |
| `note <type> [name]` | Create note (component, adr, pattern, session) |
| `todo [action]` | Manage persistent TODOs |
| `lookup <query>` | Search vault notes (Claude Code handles this naturally via CLAUDE.md — this command is for explicit searches) |
| `relate <src> <tgt>` | Create relationships between notes (Claude Code creates wikilinks naturally when writing notes — this command is for manual relationship management) |

**Special cases — `setup` and `init`:**
If subcommand is `setup` or `init`, redirect: invoke the `obs-vault:obs-setup` command (note: this is a command, not a skill). If the user passed a path argument with `init` (e.g., `init ~/MyVault`), pass it as `--path ~/MyVault`. Do not continue with this command.

**All other subcommands:**
Load and follow the `obs-vault:obs-memory` skill, passing the full original arguments string `{{ARGUMENTS}}` as the input. Execute the matched skill operation directly — do not summarize or paraphrase the skill instructions.

Announce: `[OBS VAULT] Routing: {subcommand} {args}`

---

### Phase 3: Help (no subcommand provided)

If no subcommand was given, display:

```
[OBS VAULT] Available commands:

  /obs init [path]         Create a new vault from template
  /obs analyze             Deep-analyze current project, populate vault
  /obs recap               Write session summary to vault
  /obs project [name]      Scaffold a new project in vault
  /obs note <type> [name]  Create note — types: component, adr, pattern, session
  /obs todo [action]       Manage persistent TODOs
  /obs lookup <query>      Search the vault for matching notes
  /obs relate <src> <tgt>  Create wikilink relationships between notes
  /obs setup               Run vault setup and configuration

Run any command with --help for more details.
```

---

## Error Recovery

| Error | Action |
|-------|--------|
| Unrecognized subcommand | Show help table, suggest closest match |
| `obs-memory` skill unavailable | Fall back to direct vault operations per global CLAUDE.md vault instructions |
| `obs-setup` skill unavailable | Inform user and prompt manual setup |
