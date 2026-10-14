# Global agent rules

Applies to every project. A project's own AGENTS.md wins where it disagrees.

## Style

Responses and artifacts are reviewed by humans, so keep them easy to follow.
Be brief. No jargon, filler, or announcements. Say the thing and stop.

## Mechanical checks: use the pare MCP tools

Prefer these over shelling out whenever the matching tool and project config exist.
Fall back to the shell only when no tool covers the task.

- **Git** (status, diff, log, blame, ...): `pare-git`.
- **Lint / format**: `pare-lint`. Pick by config present: `biome.json[c]` -> biome;
  `.eslintrc*` / `eslint.config.*` -> eslint; `.prettierrc*` / `prettier.config.*` /
  `"prettier"` in `package.json` -> prettier format check.
- **Build / typecheck**: `pare-build` when a `tsconfig.json` exists, instead of running `tsc` by hand.
- **Tests**: `pare-test` (detects the runner), instead of calling the runner directly.

Skip any check whose config is absent. Don't fail a project for lacking one.
