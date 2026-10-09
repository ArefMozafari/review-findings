# review-findings as a cross-agent skill repository

Date: 2026-10-09

## Goal

Publish a personal code review skill so it installs in the widely used coding agents, the same
way [pr-evidence](https://github.com/ArefMozafari/pr-evidence) does.

## Decision

One repository with the pr-evidence layout, so each agent's installer finds the same `SKILL.md`:

```
.claude-plugin/marketplace.json   Claude Code marketplace; Copilot CLI reads it too
.claude-plugin/plugin.json        Plugin manifest; the plugin root is the repository root
.cursor-plugin/marketplace.json   Cursor marketplace
.cursor-plugin/plugin.json        Cursor plugin manifest
gemini-extension.json             Gemini CLI extension manifest
skills/review-findings/SKILL.md   The skill; `skills/*/SKILL.md` is what npx skills and gh skill discover
```

The reasons behind that layout, and the Cursor manifest constraints, are recorded in the
pr-evidence design document and hold here unchanged.

### Name

The personal skill was called `review-format`. The public name is `review-findings`: it names
what the skill produces, and "format" read as a formatter for source code.

### Generalized from the personal skill

- The `user-invocable` frontmatter key is gone; the Agent Skills reference validator rejects it.
- The personal copy named Claude Code-only tools: the `/code-review ultra` command, the
  `Workflow` tool, and the other review skills and formats it deferred to. The public text says "a multi-agent,
  background, or cloud review" and "other review skills, commands, or subagents" instead, so it
  reads the same in every agent.
- "`analyze`" became "the project's analyzer", since the original came from a Dart setup.
- "the MR" in the paste-ready comment became "the PR or MR".

The report shape itself, the four severities, and the scope-gap rule are unchanged.

## Alternatives rejected

- **One collection repository for several skills.** Each skill should install on its own, and the
  pr-evidence layout is already proven in each agent; repositories link to each other instead.
- **Keeping the name `review-format`.** See above.

## Verification

- `agentskills validate`, `claude plugin validate --strict` on both Claude Code manifests, the
  Gemini manifest field check, and the Cursor schema check at `cursor/plugins` commit `ecc249f`
  all pass locally.
- Installs checked against the local checkout: Claude Code (`claude plugin marketplace add` and
  `install` in a throwaway configuration folder; the skill lands in the plugin cache),
  `npx skills add` for the Codex and Cursor targets, and `gh skill install --from-local`.
- Gemini CLI, Copilot CLI, and the Cursor CLI install from GitHub and are not installed on the
  machine used, so their README commands are checked after the first push.
