# pisum-claude-code-plugins

The `pisum` marketplace of plugins for [Claude Code](https://claude.com/claude-code).

## Installation

Add the marketplace, then install a plugin from it:

```text
/plugin marketplace add mschnecke/pisum-claude-code-plugins
/plugin install pisum@pisum
```

## Plugins

| Plugin  | Version | Description                   |
| ------- | ------- | ----------------------------- |
| `pisum` | 3.0.0   | Pisum plugin for Claude Code. |

## Version history

### 2.0.0 — 2026-11-18

- `pisum` 3.0.0: Renamed the plugin from `planning` to `pisum`. Commands are now invoked as `/pisum:<name>`; uninstall `planning@pisum` and install `pisum@pisum`.

### 1.1.0 — 2026-11-18

- `planning` 2.2.0: Added the `code-review`, `git:clean_gone` and `git:commit-push-mr` commands.

## Development

Each plugin is a top-level directory with its own `.claude-plugin/plugin.json`, listed in [.claude-plugin/marketplace.json](.claude-plugin/marketplace.json). Validate changes before committing:

```sh
claude plugin validate .
claude plugin validate ./pisum
```

To try a plugin without installing it, start Claude Code with `claude --plugin-dir ./pisum`.

Before a release, run the project skill `/update-bump` in Claude Code. It bumps the changed plugins' versions in `plugin.json` and `marketplace.json` and updates this README and `CLAUDE.md`. It does not commit.

## License

[MIT](LICENSE)
