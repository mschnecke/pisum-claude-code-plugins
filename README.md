# pisum-claude-code-plugins

The `pisum` marketplace of plugins for [Claude Code](https://claude.com/claude-code).

## Installation

Add the marketplace, then install a plugin from it:

```text
/plugin marketplace add mschnecke/pisum-claude-code-plugins
/plugin install planning@pisum
```

## Plugins

| Plugin | Version | Description |
| --- | --- | --- |
| `planning` | 0.1.0 | Planning workflows for Claude Code. Work in progress — no skills yet. |

## Development

Each plugin is a top-level directory with its own `.claude-plugin/plugin.json`, listed in [.claude-plugin/marketplace.json](.claude-plugin/marketplace.json). Validate changes before committing:

```sh
claude plugin validate .
claude plugin validate ./planning
```

To try a plugin without installing it, start Claude Code with `claude --plugin-dir ./planning`.

Before a release, run the project skill `/update-bump` in Claude Code. It bumps the changed plugins' versions in `plugin.json` and `marketplace.json` and updates this README and `CLAUDE.md`. It does not commit.

## License

[MIT](LICENSE)
