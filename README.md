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
agent/             (part 2) skills, MCP servers, global rules
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

## Roadmap

1. Dev environment (this)
2. Agent core: skills, MCP list, global rules, harness-neutral
3. pi adapter
4. Work / personal split
