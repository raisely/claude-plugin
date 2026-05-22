---
name: raisely-context
description: Bootstraps Raisely agent context in the project (.raisely/CONTEXT.md and CLAUDE.md). Use whenever the user works with a Raisely campaign locally — init, pull, preview, edit pages or components, deploy, or any task in a campaign directory with .raisely.json or raisely CLI commands.
---

# Raisely project context

Run this skill at the start of any local Raisely campaign task, before editing files or running CLI commands. It ensures the project has persistent agent context so Claude knows how Raisely pages, the CLI, MCP tools, and documentation work.

Also read the bundled context files in this skill directory when answering questions, even before bootstrapping completes.

## When to run

Trigger when any of these apply:

- The user is working in a Raisely campaign directory (`.raisely.json` present, or they asked to init/pull/preview/edit/deploy a campaign).
- The task uses `raisely` CLI commands or edits campaign JSON, theme, or custom components.
- The user asks about Raisely page structure, blocks, or local development in this repo.

Skip only for pure MCP/API questions that never touch a local campaign directory.

## Bootstrap steps

Run these steps yourself. Do not ask the user to create files manually.

### 1. Confirm project root

Use the directory that contains `.raisely.json`, or the directory the user named as the campaign root. If neither exists yet, use the current working directory where `raisely init` will run.

### 2. Create `.raisely/` and sync context files

Create `<project_root>/.raisely/` if it does not exist.

Copy each source file from this skill's directory (same folder as this `SKILL.md` in the plugin) into `<project_root>/.raisely/`:

| Source (plugin)              | Destination (project)                    |
| ---------------------------- | ---------------------------------------- |
| `what-is-raisely.md`         | `.raisely/what-is-raisely.md`            |
| `raisely-docs.md`            | `.raisely/raisely-docs.md`               |
| `raisely-tool-usage.md`      | `.raisely/raisely-tool-usage.md`         |
| `raisely-guardrails.md`      | `.raisely/raisely-guardrails.md`         |
| `raisely-fundraising-domain.md` | `.raisely/raisely-fundraising-domain.md` |

Overwrite existing copies so project context stays aligned with the plugin version.

### 3. Write `.raisely/CONTEXT.md`

Create or overwrite `<project_root>/.raisely/CONTEXT.md` with:

```markdown
# Raisely agent context

Read these files before changing anything in this campaign. They cover platform basics, CLI and MCP usage, safety guardrails, and fundraising domain vocabulary.

@.raisely/what-is-raisely.md
@.raisely/raisely-docs.md
@.raisely/raisely-tool-usage.md
@.raisely/raisely-guardrails.md
@.raisely/raisely-fundraising-domain.md

## Related plugin skills

For live workflows, also follow these skills from the Raisely Claude plugin:

- `raisely-ensure-cli` — install/verify the CLI before any `raisely` command
- `raisely-docs` — fetch official docs via Raisely MCP resources
- `raisely-local-campaigns` — end-to-end local campaign workflow
- `raisely-start-local` — run `raisely local` as a background preview server
```

Use `@` imports exactly as shown so Claude Code loads them at session start.

### 4. Ensure `CLAUDE.md` references context

**If `<project_root>/CLAUDE.md` does not exist**, create it with:

```markdown
# Raisely campaign

This project is a Raisely fundraising campaign edited locally with the Raisely CLI.

@.raisely/CONTEXT.md
```

**If `CLAUDE.md` already exists**, read it first. Do not remove or rewrite unrelated content.

- If it already contains `@.raisely/CONTEXT.md` or a clear reference to `.raisely/CONTEXT.md`, leave it as-is.
- Otherwise append this block at the end:

```markdown

## Raisely

This project is a Raisely fundraising campaign. Read the full agent context before making changes:

@.raisely/CONTEXT.md
```

### 5. Continue with the user's task

After bootstrapping, proceed with the requested work. Use `raisely-ensure-cli` before CLI commands and `raisely-local-campaigns` for preview/edit workflows.

## Notes

- `.raisely/CONTEXT.md` and the synced files are safe to commit so the whole team shares the same agent context.
- If the user declines `@` import approval in Claude Code, tell them to approve `.raisely/` imports in the memory dialog, or set `hasClaudeMdExternalIncludesApproved` for the project in `~/.claude.json`.
- Do not put secrets in any bootstrapped file.
