# obs-vault

Persistent, graph-structured memory that survives across Claude Code sessions. Capture architecture decisions, component notes, patterns, and TODOs in an Obsidian vault — and have the agent auto-orient from that knowledge every time you start a session.

## What It Does

- **Auto-orients at session start** — reads your TODOs and current project context from vault
- **Scaffolds project knowledge** — analyzes your codebase and populates the vault with what it finds
- **Stores architecture decisions, components, patterns** — organized and cross-linked with wikilinks
- **Searches across all knowledge** — `/obs lookup <query>` finds notes anywhere in the vault
- **Manages persistent TODOs** — tasks survive between sessions and appear at session start

## Install

Add the Beargle Industries marketplace and install:

```bash
claude plugin marketplace add BeargleIndustries/beargle-plugins
claude plugin install obs-vault
```

Restart Claude Code. The `/obs` command will be available immediately.

## Quick Start

```bash
/obs-setup                    # First-time setup wizard
/obs analyze                  # Deep-analyze current project
/obs lookup "architecture"    # Search the vault
/obs note adr "naming"        # Create an architecture decision note
/obs todo add "fix auth"      # Add a persistent TODO
```

## Setup

Run `/obs-setup` once to initialize your vault:

```
/obs-setup
/obs-setup --path ~/MyVault   # Custom vault location
```

The setup wizard will:
1. Create vault directories and templates
2. Auto-detect your project (if in a git repo)
3. Generate a CLAUDE.md snippet for auto-orient
4. Output instructions for the next steps

**It's safe to run multiple times** — never overwrites existing data.

## Commands

| Command | Description |
|---------|-------------|
| `/obs init [path]` | Create a new vault from template |
| `/obs analyze` | Deep-analyze current project, populate vault |
| `/obs recap` | Write session summary to vault |
| `/obs project [name]` | Scaffold a new project in vault |
| `/obs note <type> [name]` | Create note — types: component, adr, pattern, session |
| `/obs todo [action]` | Manage persistent TODOs |
| `/obs lookup <query>` | Search the vault for matching notes |
| `/obs relate <src> <tgt>` | Create wikilink relationships between notes |
| `/obs-setup` | Run vault setup and configuration |

Run any command with `--help` for more details.

## Vault Structure

```
vault/
├── Home.md               # Dashboard
├── projects/{name}/      # Per-project knowledge
│   ├── {name}.md         # Project overview
│   ├── architecture/     # ADRs (architecture decisions)
│   ├── components/       # Component notes
│   └── patterns/         # Project-specific patterns
├── domains/{tech}/       # Cross-project technology knowledge
├── patterns/             # Universal patterns
├── sessions/             # Session summaries
├── todos/Active TODOs.md # Persistent task list
├── templates/            # Note templates
└── inbox/                # Unsorted captures
```

Notes use Obsidian-flavored markdown: frontmatter for metadata, wikilinks `[[Note]]` for cross-linking, callouts for structured content.

## Configuration

### Vault Path

Resolution order:
1. `--path <vault-path>` flag in `/obs-setup`
2. `$OBSIDIAN_VAULT_PATH` environment variable
3. Default: `~/Documents/AgentMemory`

### Auto-Orient

After setup, paste the generated CLAUDE.md snippet into `~/.claude/CLAUDE.md` to enable auto-orient at session start. The agent will:
1. Read your TODOs
2. Detect current project from git repo
3. Load project context from vault
4. Greet with relevant context (2-3 lines)

## Note Types

### Project (`/obs project <name>`)
Overview of a tracked project with architecture, components, patterns, decisions, dependencies.

### Component (`/obs note component <name>`)
Single component: purpose, gotchas, dependencies, key files, layer in architecture.

### Architecture Decision (`/obs note adr <title>`)
ADR format: context, decision, consequences, alternatives considered.

### Pattern (`/obs note pattern <name>`)
Reusable pattern: problem statement, solution, when to use, examples.

### Session (`/obs recap`)
Session summary: goals, what happened, decisions made, open questions, next steps.

## Working with the Vault

### During a Session
- **Lookup knowledge:** `/obs lookup <query>` to search vault
- **Capture discoveries:** `/obs note <type> [name]` as you analyze code
- **Link ideas:** `/obs relate <src> <tgt>` to cross-reference notes
- **Track work:** `/obs todo add "task"` for items to remember next session

### At Session End
- **Record session:** `/obs recap` writes a timestamped summary
- Vault is auto-saved — nothing to commit

### Next Session
- Agent auto-orients from vault at startup
- TODOs appear in greeting
- Previous session context is available via `/obs lookup`

## Optional: Obsidian Desktop

Open the vault folder in Obsidian for visual graph navigation, backlink exploration, and rich editing. Not required — all operations work via file tools.

Steps:
1. Install [Obsidian](https://obsidian.md) (free)
2. Vault Switcher → Open folder as vault → select your vault path
3. Graph view to navigate knowledge visually

Obsidian syncs automatically with file changes from Claude Code.

## Integration with oh-my-claudecode

Works seamlessly with oh-my-claudecode (OMC) workflows:
- `/save-session` command syncs with vault
- Autopilot suggests `/obs-setup` at project start
- Team workflows auto-update project notes when major decisions are made

Not required — the plugin works standalone.

## FAQ

**Does it require Obsidian?** No. All operations use Claude Code's file tools. Obsidian is optional for visual browsing.

**Does it require oh-my-claudecode?** No. Fully standalone plugin.

**What if I have multiple projects?** Set `$OBSIDIAN_VAULT_PATH` per project, or use `--path` flag with `/obs-setup` in each project directory. All projects share the same vault by default (cross-linked).

**Can I sync the vault?** Yes. The vault is a normal folder — use git, Syncthing, or any file sync tool. Obsidian's sync feature (paid) also works.

**Is it safe to run setup twice?** Yes. Idempotent — never overwrites existing vault data.

**What if I delete a note by mistake?** The vault is just files. Recover from git history, or use Obsidian's daily snapshots (if enabled in settings).

## Requirements

- Claude Code (any version with plugin support)
- No external dependencies required

## License

MIT — see repository root.
