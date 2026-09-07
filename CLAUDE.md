# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

**Forked** is the learning platform for DevOps Club. Members learn CS fundamentals, specialize in one of four tracks (Game dev, Front end, AI engineer, Data science), earn an optional industry certification per track, build a student profile from real project work, and get matched to an unpaid internship. The name is the metaphor: everyone starts from the same base, then the path forks.

## Current State

Planning stage. There is no source code, build system, lint, or test suite yet, so there are no commands to run. The repo holds the README, community-health files, and curriculum docs in `docs/`. Update this file as code lands.

## Read `docs/` before doing curriculum or design work

- `docs/learning-model.md` — the current direction. Every lesson has two halves: **Learn** on the web (short, interactive, Brilliant-style explanation built from six blocks) and **Do** inside the real tool (VS Code, or Godot for game dev) guided by a Forked companion extension that checks each step against actual editor, filesystem, and git state. Also covers progression, certifications, the MVP slice, risks, and the open decisions.
- `docs/fundamentals.md` — the ~8-week fundamentals curriculum: module goals, the Learn blocks and Do step types each module uses, per-module projects, the author-reference catalog with licenses, and the GitHub Foundations cert.
- The README's "Learning format" section summarizes the same model. All three are reconciled; keep them that way when one changes.

Things the model explicitly rejects: video-then-quiz, multiple-choice as a teaching device (it exists only in certification practice exams), link-out to external sites for learning, in-browser sandboxes for terminal/git/code, and screen capture. Don't write content or code that assumes any of those without checking with the officers.

## Licensing rule (hard constraint on all content work)

- Lessons are club-authored. External resources are **reference** for authors; lessons don't link students to them.
- **Incorporating** (copying material into the platform) is allowed **only for MIT or public-domain/CC0 content**: Exercism JS exercises, Learn Git Branching, MDN code samples, Hack Club workshop code.
- Notably **not** incorporable: The Odin Project, CS50, MIT Missing Semester (all CC BY-NC-SA), W3Schools, VisuAlgo, Hack Club prose (CC BY-SA). See the catalog in `docs/fundamentals.md`; re-verify a license before incorporating anything.

## Planned architecture (nothing built yet)

- **Web app**: Next.js + React + TypeScript, Tailwind + shadcn/ui, Clerk (student and business accounts), Zod, DynamoDB, PostHog, Vitest, npm, hosted on Vercel. Hosts the Learn half, the path map, progress, profiles, matching, and the officer "stuck list."
- **Companion extensions**: a VS Code extension (TypeScript) covering Fundamentals, Front End, AI, and Data Science; a Godot editor plugin (GDScript) for Game Dev. The VS Code extension is built first. Extensions read tool state via APIs, never the screen.
- **Lessons are data**: one folder per lesson under `content/` with `learn.mdx` (web blocks as components) and `do.yaml` (steps with type, instruction, pointer, check, hint). Six Learn blocks and seven Do step types are defined in `learning-model.md`; adding a block or step type is a platform change, adding a lesson is a content change. Certs live under `content/certs/`.
- **MVP slice**: web app with sign-in, path map, progress sync, and three Learn blocks; VS Code extension with four step types; modules 0, 2, and 3 as content.

## Conventions

- Curriculum ideas and track outlines go in `docs/`.
- Open an issue to propose changes to the learning flow before building against it.
- `AGENTS.md` points here; keep that pointer in place.

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
