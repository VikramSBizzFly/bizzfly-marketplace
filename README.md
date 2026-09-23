# BizzFly marketplace

The single Claude Code plugin marketplace for BizzFly. This repo holds only the catalog (`.claude-plugin/marketplace.json`); each plugin's code lives in its own repo.

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

Restart Claude Code. If you installed bizzfly-rules, run `/bizzfly-rules:apply` in each project; it sets up `.claude/tmp/`, `.claude/memory/` and `.gitignore` for the rules. bizzfly-rules needs Node.js on `PATH` (check with `node --version`).

### Moving from the old `bizzfly` marketplace

The marketplace used to live inside the testwright repo under the name `bizzfly`. That one no longer has a catalog and can't update. Switch once:

```
/plugin marketplace remove bizzfly
/plugin marketplace add VikramSBizzFly/bizzfly-marketplace
/plugin install testwright@BizzFly
/plugin install bizzfly-rules@BizzFly
```

Your projects are untouched; testwright's `tests/` folder carries over as it is.

## Updating

Each plugin updates on its own:

| Plugin | Command |
|---|---|
| `bizzfly-rules` | `/bizzfly-rules:update` |
| `testwright` | `/plugin marketplace update BizzFly`, then `/plugin update testwright@BizzFly` |

`/bizzfly-rules:update` refreshes the catalog and updates bizzfly-rules only. It doesn't touch testwright or any other plugin. Refreshing the catalog alone (`/plugin marketplace update BizzFly`) doesn't install anything new.

Restart Claude Code after updating. A running session keeps the versions it started with.

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

List only the plugins that project needs.

## Adding a plugin

1. Put the plugin in its own repo, with `.claude-plugin/plugin.json` at the root.
2. Add an entry to `plugins` in `.claude-plugin/marketplace.json`:
   ```json
   { "name": "<plugin>", "source": { "source": "url", "url": "https://github.com/VikramSBizzFly/<repo>.git" }, "description": "..." }
   ```
3. Add a row to the table at the top of this README.
4. Run `claude plugin validate .` and push.

## Releasing a plugin update

This repo doesn't change. In the plugin's own repo:

1. Make the change and bump `version` in `.claude-plugin/plugin.json`. Claude Code only installs an update when the version changes.
2. Push to `main`.

Users then pick it up with the commands under [Updating](#updating).
