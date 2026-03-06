# Beargle Industries Claude Code Plugins

A collection of Claude Code plugins by [Beargle Industries](https://github.com/BeargleIndustries).

## Available Plugins

| Plugin | Description |
|--------|-------------|
| [focusgroup](plugins/focusgroup/) | AI-powered focus group reviews with deep protocols, advanced methodologies, quality control, and rich output formats |

## Install

Add this marketplace to Claude Code:

```bash
claude plugin marketplace add BeargleIndustries/claude-plugins
```

Then install any plugin:

```bash
claude plugin install focusgroup
```

Restart Claude Code after installing. The `/focusgroup` command (plus `/fg` and `/focus-group`) will be available immediately.

## Update

```bash
claude plugin marketplace update BeargleIndustries/claude-plugins
claude plugin update focusgroup
```

## License

[MIT](LICENSE)
