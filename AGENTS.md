# AGENTS.md

Guidance for AI coding agents working in this repository.

See [CLAUDE.md](CLAUDE.md) for the full guidance: project overview, current state, the learning-model docs in `docs/` and what they reject, the licensing rule for curriculum content, and the planned architecture. That file is the single source of truth; keep this pointer in place so the two stay in sync.

## graphify (mandatory for every agent, no need to ask)

This repo keeps a [graphify](https://github.com/Graphify-Labs/graphify) knowledge graph of its docs and code in `graphify-out/` (gitignored, so each clone builds its own). The `/graphify` skill is committed at `.claude/skills/graphify/SKILL.md` (Claude Code) and `.agents/skills/graphify/SKILL.md` (other agents), and `.claude/settings.json` has PreToolUse hooks that nudge you to query the graph before raw searches. Do all of the following automatically, without asking the user or announcing a plan:

- **Prerequisite**: the `graphify` CLI must be on PATH (`uv tool install graphifyy` or `pip install graphifyy`). The skill installs it on first `/graphify` run.
- **No graph yet?** If `graphify-out/graph.json` is missing, run `/graphify .` first, then continue with the task.
- **Before answering any question** about the project, its architecture, docs, or file relationships, run `graphify query "<question>"`. Use `graphify path "<A>" "<B>"` for relationships and `graphify explain "<concept>"` for a single concept. Read `graphify-out/GRAPH_REPORT.md` only for broad architecture review or when those don't surface enough. Only fall back to grep or raw reads after the graph has oriented you, or to edit specific lines.
- **After changing any `.md` or other doc file** (this repo is docs-only today), run `/graphify . --update` before you finish the task. This is incremental and re-extracts only changed files.
- **After changing code** (once code lands), run `graphify update .` (AST-only, no API cost). Docs still need `/graphify . --update`.
- **Never commit `graphify-out/`**; it stays gitignored. Dirty or missing graph files are not a reason to skip graphify.
- **Keep tooling out of the graph.** `.graphifyignore` excludes `.claude/` and `.agents/` (the vendored skill and hooks). Add other non-content paths there rather than letting them pollute the graph.
- When the user types `/graphify`, use the committed skill before doing anything else.
- **Don't re-run `graphify install --project` here.** The skill, hooks, and this section are already committed. The installer appends a second generic `## graphify` section with rules that are wrong for this docs-only repo. Ignore any "installed skill is older" warning from the CLI.
- CLAUDE.md and AGENTS.md carry the same graphify section; change both together.
