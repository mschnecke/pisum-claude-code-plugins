# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## What this repo is

A Claude Code plugin marketplace named `pisum`, hosted on GitHub at `mschnecke/pisum-claude-code-plugins` (the git remote uses the SSH alias `github.pisum`; treat it as github.com). There is no build, lint or test toolchain — the repo is manifests plus Markdown plugin components.

- `.claude-plugin/marketplace.json` — the catalog: marketplace name, owner, and the list of plugins.
- `<plugin>/` — each plugin is a top-level directory next to `.claude-plugin/`, referenced from the catalog by a relative `source` (e.g. `"./pisum"`).
- `<plugin>/.claude-plugin/plugin.json` — the plugin's own manifest.

Currently there is one plugin, `pisum`, which provides an OpenSpec skill for exploring GitLab issues under `skills/` (invoked as `/pisum:<name>`).

## Plugin layout

Components are auto-discovered only from these conventional paths inside a plugin directory:

```
<plugin>/
  .claude-plugin/plugin.json
  skills/<skill-name>/SKILL.md
  commands/*.md
  agents/*.md
  hooks/hooks.json
  .mcp.json
```

Installed components are namespaced by plugin, e.g. a skill `foo` in `pisum` is invoked as `pisum:foo`.

## Versioning

A plugin's version is recorded in two places that must stay in sync: `version` in `<plugin>/.claude-plugin/plugin.json` and the plugin's entry in `plugins[]` of `marketplace.json`. The marketplace also has its own top-level `version`. When adding a plugin, add both the directory and its `plugins[]` entry.

The project skill `/update-bump` ([.claude/skills/update-bump/SKILL.md](.claude/skills/update-bump/SKILL.md)) bumps these versions from the uncommitted changes and updates README.md and this file to match. It does not commit.

## Validate and test

```sh
claude plugin validate .            # marketplace manifest
claude plugin validate ./pisum   # one plugin (manifest + skills/agents/commands)
claude plugin validate ./pisum --strict   # warnings fail too
```

`--profile directory` additionally fails on public plugin-directory submission rules (README of 40+ words, plugin icon); these are advisory unless publishing to the directory.

Try a plugin without installing it: `claude --plugin-dir ./pisum`. Try the whole marketplace: `/plugin marketplace add <path-to-this-repo>`, then `/plugin install pisum@pisum`.

## Conventions

- Use the `gh` CLI for GitHub operations.
