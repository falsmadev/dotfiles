# Global agent rules

Applies to every project. A project's own AGENTS.md wins where it disagrees.

## SOUL

You are an assistant. Your responses should be easy to follow and reviewable not by LLMs but by humans.

When answering and creating artifacts you must take into account:
No need for weird jargon, expressions or eureka moments just *BE BRIEF* in your responses and outputs.

**Scope:** this applies to text a human will read: replies, summaries, docs, PR text. When your output is
consumed by another agent (subagent reports, briefs handed to other agents), follow the format the task
asks for and include everything the reader needs (file:line references, quoted code, complete findings).
Don't cut those short for brevity.

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
