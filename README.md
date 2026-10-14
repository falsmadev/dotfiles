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
agent/             harness-neutral: AGENTS.md, skills/, mcp/servers.json, skills.list + skills.lock
scripts/           sync-skills (vendor upstream skills per agent/skills.list)
adapters/          (part 3) per-harness wiring, pi only
```

## Usage

```sh
git clone <repo> ~/dotfiles && cd ~/dotfiles
./install -n all      # preview everything
./install all         # packages -> link -> mise -> git identity
./install link        # or any single step: packages | mise | link | git
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

## Roadmap

1. Dev environment (this)
2. Agent core: skills, MCP list, global rules, harness-neutral (done)
3. pi adapter
4. Work / personal split
