# BizzFly marketplace

The single Claude Code plugin marketplace for BizzFly. It holds only the catalog (`.claude-plugin/marketplace.json`); each plugin's code lives in its own repo.

| Plugin | Repo | What it does |
|---|---|---|
| `testwright` | [VikramSBizzFly/testwright](https://github.com/VikramSBizzFly/testwright) | Claude-native QA: maps routes, writes test cases into an Excel workbook, runs them via curl and the Playwright MCP. |
| `bizzfly-rules` | [VikramSBizzFly/bizzfly-rules](https://github.com/VikramSBizzFly/bizzfly-rules) | Team rules in every session, plus a guard that blocks tool calls outside the launch directory. |

## Install

```
/plugin marketplace add VikramSBizzFly/bizzfly-marketplace
/plugin install testwright@BizzFly
/plugin install bizzfly-rules@BizzFly
```

To update later, run `/plugin marketplace update BizzFly`.

If you added the old `bizzfly` marketplace (from the testwright repo), remove it first with `/plugin marketplace remove bizzfly`.

## Enable for a whole project

Commit this to the project's `.claude/settings.json`. Team members are then prompted to install the marketplace and plugins when they trust the folder:

```json
{
  "extraKnownMarketplaces": {
    "BizzFly": { "source": { "source": "github", "repo": "VikramSBizzFly/bizzfly-marketplace" } }
  },
  "enabledPlugins": {
    "bizzfly-rules@BizzFly": true,
    "testwright@BizzFly": true
  }
}
```

## Adding a plugin

1. Put the plugin in its own repo, with `.claude-plugin/plugin.json` at the root.
2. Add an entry to `plugins` in `.claude-plugin/marketplace.json`:
   ```json
   { "name": "<plugin>", "source": { "source": "github", "repo": "VikramSBizzFly/<repo>" }, "description": "..." }
   ```
3. Run `claude plugin validate .` and push.

Plugin updates don't touch this repo. Push to the plugin's repo and bump its `version`.
