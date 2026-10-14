# dotfiles

Agent-agnostic dev environment + (soon) agent config. Plain symlinks, zsh, mise.

## Layout

```
install            idempotent installer, run in parts
links.txt          <repo path> <destination> — the symlink manifest
shell/             zshrc, zshenv
git/               gitconfig (no identity), global ignore
config/            app configs (starship)
packages/          arch.txt, debian.txt (system), mise.toml (dev tools)
agent/             harness-neutral: AGENTS.md, skills/, agents/, mcp/servers.json, skills.list + skills.lock
scripts/           sync-skills (vendor upstream skills per agent/skills.list)
adapters/pi/       pi wiring: link script, models.conf, vendored subagent extension
```

## Usage

```sh
git clone <repo> ~/dotfiles && cd ~/dotfiles
./install -n all      # preview everything
./install all         # packages -> link -> mise -> git identity -> pi
./install link        # or any single step: packages | mise | link | git | pi
```

- Existing files are moved to `~/.dotfiles-backup/<timestamp>/`, never deleted.
- Git identity lives in `~/.gitconfig.local`; machine tweaks in `~/.zshrc.local` and `~/.zshenv.local`. Neither is tracked.
- Dev tools go in `packages/mise.toml`; only OS-level packages go in `packages/*.txt`.
- Supported: Arch-family (CachyOS, Arch) and Debian-family.

## Agent core

- `agent/skills/` is vendored from [mattpocock/skills](https://github.com/mattpocock/skills), pinned in `agent/skills.lock`.
  Edit `agent/skills.list`, run `scripts/sync-skills`, review the diff, commit.
- `agent/mcp/servers.json` uses the standard `mcpServers` format (pare git/lint/build/test, versions pinned).
- `agent/AGENTS.md` holds the global rules.

### Skill flow (the subset kept)

```
grill-with-docs  ->  to-spec  ->  to-tickets  ->  implement (drives tdd)  ->  code-review  ->  pr
```

`implement` alone is enough for single-session work. Standalone: `grill-me`/`grilling`, `domain-modeling`,
`wizard`, `wait-what`. Run `setup-matt-pocock-skills` once per repo to say where tickets live
(local `.scratch/` files for now; Jira would be a custom tracker).

## pi adapter (`./install pi`)

| Source | Lands in | How |
|---|---|---|
| `agent/skills/<n>` | `~/.agents/skills/<n>` | per-skill symlink (path other tools read too) |
| `agent/AGENTS.md` | `~/.pi/agent/AGENTS.md` | symlink |
| `adapters/pi/extensions/subagent` | `~/.pi/agent/extensions/subagent` | symlink (vendored from pi 1.2.0, see PROVENANCE.md) |
| `agent/agents/*.md` | `~/.pi/agent/agents/*.md` | rendered; `model:` added from `adapters/pi/models.conf` |
| `agent/mcp/servers.json` + `adapters/pi/mcp-overrides.json` | `~/.pi/agent/mcp.json` | merged; existing servers (e.g. radius) kept. Overrides expose a few read-only pare tools directly (pi hides MCP tools behind codemode by default) |

Agents: `scout` (deepseek-v4.1-flash), `standards-reviewer` and `spec-reviewer` (claude-sonnet-5-5), the pair the
`code-review` skill fans out to. Change models in `models.conf`, re-run `./install pi`, then `/reload` in pi.
Agent files are neutral (no model); the model line is the only pi-specific bit.

## Roadmap

1. Dev environment (this)
2. Agent core: skills, MCP list, global rules, harness-neutral (done)
3. pi adapter (done, applied, smoke-tested)

## Licenses and attribution

- `agent/skills/` is vendored from [mattpocock/skills](https://github.com/mattpocock/skills), MIT, Copyright (c) 2026 Matt Pocock (`agent/skills/LICENSE`).
- `adapters/pi/extensions/subagent/` is vendored from [pi](https://github.com/earendil-works/pi), MIT, Copyright (c) 2025 Mario Zechner (`./LICENSE` there).
- Everything else is my own configuration.
