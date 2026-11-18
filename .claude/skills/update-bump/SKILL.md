---
name: update-bump
description: Review uncommitted changes, bump the affected plugin's version in plugin.json and marketplace.json, and update README.md and CLAUDE.md to match. Use when the user wants to bump the version, prepare a release, or sync version numbers and docs after changing a plugin — triggers on "bump version", "update version", "version bump", "prep release". Does not commit.
---

# Update & Bump Version

Bump versions and bring the docs in line with the uncommitted changes in this marketplace repo.

## 1. Find what changed

Run `git status --short` and `git diff HEAD` (include untracked files). Map each change to a plugin by its top-level directory (`<plugin>/…`). A plugin is listed in `plugins[]` of `.claude-plugin/marketplace.json`.

- If no plugin has changes, say so and stop.
- If changes are only outside plugin directories (README, CLAUDE.md, `.claude/`), don't bump any plugin version.

## 2. Pick the new version per changed plugin

Semver, from the plugin's current `version` in `<plugin>/.claude-plugin/plugin.json`:

- **patch** — fixes and wording changes to existing skills/commands/agents.
- **minor** — new skills, commands, agents, hooks or MCP servers; new behavior in existing ones.
- **major** — removed or renamed components, or changed behavior users must adapt to.

A plugin new to the catalog keeps its initial version. If the user named a level or version, use that. State the chosen level and the reason in one line each.

## 3. Update the versions

For each changed plugin, set the same new version in both:

- `<plugin>/.claude-plugin/plugin.json` → `version`
- `.claude-plugin/marketplace.json` → the plugin's entry in `plugins[]` → `version`

Then bump the marketplace's top-level `version` in `marketplace.json` by the highest level used for any plugin (or minor when a plugin was added).

If a plugin's description changed, keep `description` identical in `plugin.json`, its `plugins[]` entry, and the README.

## 4. Update README.md

In the **Plugins** table, update the version and description of each changed plugin, and add a row for each new plugin. Remove "Work in progress" notes once a plugin has components. Keep install lines in sync with the plugin names.

## 5. Update CLAUDE.md

Correct any statement the changes made untrue, e.g. the plugin list or "still an empty scaffold". Only describe the big picture; don't list every skill.

## 6. Verify

```sh
claude plugin validate .
claude plugin validate ./<plugin>   # each changed plugin
```

Both must pass. Report the old → new versions and the files touched.

**Do not commit.**
