# obs-vault

Persistent memory for Claude Code using an Obsidian vault. Your AI agent remembers project context, architecture decisions, and TODOs across sessions.

## How It Works

The plugin gives Claude Code a knowledge vault — a folder of interconnected markdown notes. At the start of every session, Claude Code reads your TODOs and current project context. As you work, it writes discoveries back. Next session, it picks up where you left off.

The magic is in the CLAUDE.md snippet generated during setup. It tells Claude Code:
- Auto-orient at session start — read TODOs and current project context (2 reads max)
- Detect current project from working directory (git repo name or folder name)
- Write notes with consistent frontmatter when it discovers something worth remembering
- Search the vault naturally via file tools — no special commands needed
- Follow Obsidian conventions (wikilinks, frontmatter, callouts)

You don't need to learn commands to use it. Claude Code handles vault reads/writes naturally. The `/obs` commands are helpers for specific tasks.

## Install

```bash
claude plugin marketplace add BeargleIndustries/beargle-plugins
claude plugin install obs-vault
```

## Setup

```
/obs-setup
/obs-setup --path ~/MyVault   # custom location
```

This creates the vault, scaffolds your current project, and generates a CLAUDE.md snippet. Paste the snippet into `~/.claude/CLAUDE.md`. That's it.

Safe to run multiple times — never overwrites existing data.

## What Happens Next

1. **Start a new session** — Claude Code reads your TODOs and project context
2. **Work normally** — Claude Code writes notes when it finds something worth remembering
3. **End a session** — Ask Claude to save a summary: "save a session summary" or `/obs recap`
4. **Next session** — Claude Code picks up right where you left off

## Helper Commands

These are optional — Claude Code handles most vault operations naturally through the CLAUDE.md instructions.

| Command | When to use it |
|---------|---------------|
| `/obs-setup` | First-time setup |
| `/obs analyze` | Deep-analyze a new project and populate the vault |
| `/obs recap` | Save a session summary |
| `/obs project [name]` | Manually scaffold a new project |
| `/obs note <type> [name]` | Create a specific note (component, adr, pattern) |
| `/obs todo [action]` | Add/complete/view TODOs |
| `/obs init [path]` | Create a vault at a specific path |

## Vault Structure

```
vault/
├── Home.md               # Dashboard
├── projects/{name}/      # Per-project knowledge
│   ├── {name}.md         # Project overview (start here)
│   ├── architecture/     # Architecture decision records
│   ├── components/       # Component notes
│   └── patterns/         # Project-specific patterns
├── domains/{tech}/       # Cross-project technology knowledge
├── patterns/             # Universal patterns
├── sessions/             # Session summaries
├── todos/Active TODOs.md # Persistent task list
├── templates/            # Note templates
└── inbox/                # Unsorted
```

All projects share one vault. Notes cross-link with wikilinks.

## Configuration

### Vault Path

Resolution order:
1. `$OBSIDIAN_VAULT_PATH` environment variable
2. Path referenced in your CLAUDE.md
3. Default: `~/Documents/AgentMemory`

### Project Detection

Claude Code detects your current project from the working directory name (git repo name preferred, folder name as fallback). It looks for a matching folder in `vault/projects/`. If no match is found, it offers to scaffold a new project.

## Optional: Obsidian Desktop

Open the vault folder in [Obsidian](https://obsidian.md) for visual graph navigation and backlink browsing. Not required — everything works via file tools.

## Optional: oh-my-claudecode

Works alongside OMC if you have it. Not required — the plugin is fully standalone.

## FAQ

**Do I need Obsidian installed?** No. Claude Code reads and writes the vault files directly.

**Do I need oh-my-claudecode?** No. Fully standalone.

**How do I save a session?** Ask Claude to save a summary, or run `/obs recap`. It's not automatic.

**Multiple projects?** All projects share one vault and cross-link naturally. No extra config needed.

**Can I sync the vault?** Yes — it's just a folder. Use git, Syncthing, iCloud, whatever.

## License

MIT
