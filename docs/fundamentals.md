# Fundamentals — Starter Curriculum Plan

Everyone starts here before picking a track. Scope (from the README): programming fundamentals, data structures, git, terminal, plus web basics (HTML/CSS/JS, how the web works).

**Format:** every lesson has a **Learn** half on the web (short interactive explanation) and a **Do** half in VS Code (extension-guided steps checked against the real project), per [learning-model.md](learning-model.md). Each module ends with a project the student builds in a real folder on their machine. ~8 weeks at a club pace (2–4 hrs/week), self-paced friendly.

## Ground rule: reference vs. incorporate

Lessons are club-authored and live in this repo. External resources are used two ways:

- **Reference** — officers read it while writing a lesson. Any resource qualifies, regardless of license. Lessons do not link students to it.
- **Incorporate** — we copy material into our lessons. Only allowed for **public domain (CC0) or MIT-licensed** content.

Licenses below were verified against each project's repo/site (Sep 2026). Note: several famous "free" curricula (Odin Project, CS50) are **CC BY-NC-SA** — fine to read while authoring, but their content can't be copied into our platform. Re-check before incorporating anything.

## Author references

| Resource | What it covers | License | Mode |
|---|---|---|---|
| [MDN Learn Web Development](https://developer.mozilla.org/en-US/docs/Learn_web_development) | HTML/CSS/JS, how the web works | Prose CC BY-SA 2.5+; **code samples (post-2010) CC0** | Reference; **code samples incorporable** (Explain screens) |
| [Exercism](https://exercism.org) | Practice problems (JS track) | JS track repo **MIT** | **Exercises incorporable** (fix-the-bug, make-the-test-pass steps) |
| [Learn Git Branching](https://learngitbranching.js.org) | Interactive git visualization | **MIT** | **Incorporable** (basis for the git Visual block) |
| [Hack Club Workshops](https://workshops.hackclub.com) | Beginner project workshops | Content CC BY-SA 4.0; **code MIT** | Reference; **code incorporable** (starter projects) |
| [freeCodeCamp](https://www.freecodecamp.org) | HTML/CSS, JS, DSA | Code BSD-3; curriculum © freeCodeCamp | Reference only |
| [The Odin Project](https://www.theodinproject.com) | Full web path incl. foundations | CC BY-NC-SA 4.0 | Reference only |
| [W3Schools](https://www.w3schools.com) | HTML/CSS/JS reference | Proprietary (free to use) | Reference only |
| [CS50x](https://cs50.harvard.edu/x/) | CS fundamentals, deeper theory | CC BY-NC-SA 4.0 | Reference only |
| [MIT Missing Semester](https://missing.csail.mit.edu) | Terminal, shell, git, tooling | CC BY-NC-SA 4.0 | Reference only |
| [VisuAlgo](https://visualgo.net) | Data structure/algorithm visualizations | Free for education; no self-hosting or forking | Reference only |

## Module plan

Learn blocks and Do step types are defined in [learning-model.md](learning-model.md).

### 0. Install & connect — before week 1
Goal: VS Code, Node, and the Forked extension installed; signed in; first project open.
- **Learn:** Explain only — what each tool is for, in plain words.
- **Do:** web-only onboarding checklist; the extension's first message confirms the connection.
- Officers run this live at the first meeting. This is the biggest drop-off point.

### 1. Terminal & dev setup — week 1
Goal: comfortable in a shell.
- **Learn:** Explain, Fill the blank, Reorder — what a shell is, paths, the handful of commands that matter.
- **Do:** Run a command, Create / edit file — in the real VS Code terminal.
- **References:** Missing Semester shell lecture; freeCodeCamp command line sections.
- **Project:** scavenger hunt — navigate, create, and inspect files entirely from the terminal; checked from the resulting filesystem state.

### 2. Git & GitHub — week 2
Goal: clone, branch, commit, push, open a PR.
- **Learn:** Explain, Visual (git graph), Predict, Spot it — commits as snapshots, branches as pointers, merges.
- **Do:** Git state, Run a command — against a real repo.
- **References:** Learn Git Branching (MIT, incorporable); GitHub's Hello World guide.
- **Project:** fork the club's practice repo, add a profile card file, open a PR. (On-theme for "Forked." Needs a GitHub account — open decision 2 in learning-model.md.)

### 3. Programming fundamentals (JavaScript) — weeks 3–4
Goal: variables, conditionals, loops, functions, arrays/objects.
- **Learn:** Explain, Predict, Fill the blank, Reorder — one construct per lesson.
- **Do:** Make the test pass, Fix the bug — Exercism JS exercises (MIT) as the seed.
- **References:** freeCodeCamp JS sections; W3Schools JS as reference.
- **Project:** small CLI game in Node (guess-the-number or hangman) with tests.
- *Open decision:* JS chosen so fundamentals flow into web basics and the Front End track; AI/Data Science tracks introduce Python at specialization time. Revisit if officers prefer Python-first.

### 4. Data structures basics — week 5
Goal: arrays, maps/objects, stacks, queues; Big-O intuition (awareness, not proofs).
- **Learn:** Explain, Visual (stack/queue), Predict, Spot it.
- **Do:** Make the test pass, Fix the bug.
- **References:** freeCodeCamp DSA sections; VisuAlgo and CS50 shorts for authors.
- **Project:** implement a stack and queue in JS with tests; use one to build an undo feature for the week-3 game.

### 5. HTML & CSS — week 6
Goal: build and style a static page; flexbox basics.
- **Learn:** Explain, Visual (flexbox), Fill the blank.
- **Do:** Create / edit file, with live preview (open decision 3 in learning-model.md).
- **References:** MDN "Structuring content with HTML" and "CSS styling basics" (code samples CC0, incorporable).
- **Project:** personal page — name, photo, interests, links — as a guided build.

### 6. How the web works + JS in the browser — week 7
Goal: what happens when you load a URL (DNS → HTTP → render); DOM manipulation.
- **Learn:** Explain, Visual (request lifecycle), Predict.
- **Do:** Fix the bug, Create / edit file.
- **References:** MDN "How the web works", "What is a web server?", DOM scripting intro; a Hack Club workshop (code MIT) as the starter.
- **Project:** add interactivity to the week-5 page (dark-mode toggle, or fetch + display a public API).

### 7. Capstone: portfolio v0 — week 8
Combine everything: a personal portfolio site in a real repo with a real commit history, listing the fundamentals projects.
- **Learn:** summary cards from modules 1–6.
- **Do:** open project with automated checks; Git state, Make the test pass; officer or peer review.
- **This becomes the seed of the member's Student Profile** — the platform's profile feature imports it.

Completion bar: capstone reviewed → member picks a track.

## Certification: GitHub Foundations (optional)

The fundamentals certification is [GitHub Foundations](https://resources.github.com/learn/certifications/). Module 2 covers most of the exam objectives; a short cert-prep lesson after the capstone fills the gaps (GitHub features beyond git: issues, projects, Actions basics, Copilot) and includes a practice exam. Verified students can get a free exam voucher through GitHub Education. Earning it is optional and does not gate picking a track. See "Industry certifications" in [learning-model.md](learning-model.md).

## Open questions
- JS-first vs. Python-first (see module 3).
- Whether members' devices can run desktop VS Code (learning-model.md, decision 1). If not, module 0 and every Do half need a fallback.
- Hack Club content is CC BY-SA, not MIT — if we ever want to incorporate their prose, officers must either relicense our lesson pages CC BY-SA or keep it reference-only.
