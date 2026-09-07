# Learning Model — How Students Learn on Forked

> **Status:** Proposal for officers. This is the current direction; the README's "Learning format" section and `fundamentals.md` follow it. The open decisions at the end are still open.

## The one-line version

Every lesson has two halves. **Learn** happens on the web platform: a short, interactive explanation of the concept, Brilliant-style, where the student predicts, fills in, and reorders rather than reads. **Do** happens inside the real tool — VS Code for code, Godot for game dev — guided step by step by a Forked companion extension that checks their work against the actual project state and pops up the next thing to do.

The web app is also home base: the path, progress, profile, and matching.

Think Brilliant for the explanation, Duolingo for the structure and feedback loop, and Unity's in-editor tutorials for the practice, in the tools the student will use at their internship.

## What it is not

- **Not Google Classroom / a gradebook.** No assignment inbox, due dates, or grades. Progress is mastery.
- **Not College Board / Albert.io.** No multiple-choice drills or essay prompts as the main activity.
- **Not video-then-quiz.** The Learn half is interactive explanation, not a video with a quiz after it. A question exists only to make the student think before the answer is shown.
- **Not a link list.** Nothing sends the student to an outside site to learn. External resources are reference material for the officers who author lessons.
- **Not a sandbox.** No fake terminal, fake git, or in-browser code runner. The real terminal, real git, and real runtime are the sandbox.
- **Not screen recording.** The extension reads the tool's state through its API. It never captures the screen.

## Principles (rules the platform enforces)

1. **Explain by asking.** In the Learn half, no screen is a wall of text. Every screen has one idea, a picture or example, and something the student does with it before moving on.
2. **Practice in the real tool.** In the Do half, every step is an action in VS Code or Godot: create this file, run this command, make this test pass, add this node.
3. **Checks are facts, not opinions.** Each Do step has a completion check against real state: a file exists, a test passes, a branch is merged, a node has a child. Deterministic, instant, no grading.
4. **Wrong answers teach.** A wrong prediction or failing check says what is wrong and, if the student asks, gives a hint. No hearts, no penalties.
5. **Every lesson leaves something real behind.** A file, a commit, a scene, a passing test, in a real project on the student's machine.
6. **Short sessions.** Learn is 3–5 minutes. Do is 5–15 minutes. A unit is 4–8 lessons.
7. **Review is a skill too.** Students read diffs and review code, not just write it, because that is what DevOps and internship work looks like.

## Architecture

```
  Web app (Next.js on Vercel)                 Companion extensions
  ──────────────────────────                  ───────────────────────────────
  • Sign in (Clerk)                           • VS Code extension
  • Path map, units, progress     ◄──sync──►    fundamentals, front end, AI, data science
  • LEARN: concept lessons                    • Godot editor plugin
  • Lesson 0: install & connect                 game dev
  • Student profile, matching
  • Officer view: who is stuck                DO: side panel with the current step,
                                              highlights/popups pointing at the UI,
                                              state checks, hints, progress sync.
```

**The web app** teaches the concept, then hands off: "Open VS Code — the extension has your next steps." It never tries to be an editor.

**The extension** is where practice happens. It shows the current step in a side panel, points at the relevant part of the editor (a highlighted file, a decorated line, a notification pointing to the terminal), watches the workspace, and advances when the step's check passes. Progress syncs back to the web app, which shows the lesson as done and offers the next one.

**Lesson 0** runs entirely on the web: install VS Code and Node, install the extension, sign in, open the first project. This first hour is where students are lost, so it gets its own polish.

## The Learn half (web)

A Learn lesson is a sequence of short screens built from a small set of blocks:

| Block | What the student does | Inspiration |
|---|---|---|
| **Explain** | One idea, a diagram or animation, a concrete example. Three to five sentences, never more. | Brilliant |
| **Predict** | Reads a snippet or a scenario and predicts what happens; the answer appears with the explanation of why. | Brilliant |
| **Fill the blank** | Completes a line of code or a command with one or two holes. | Duolingo |
| **Reorder** | Drags shuffled lines or steps into working order (a Parsons problem). | Research-backed; cheap to author |
| **Spot it** | Reads a short diff or snippet and taps the line with the bug or the concept being asked about. | Our own |
| **Visual** | Manipulates a diagram: drag commits in a git graph, resize a flexbox container, step through a loop. | Brilliant, Learn Git Branching (MIT) |

Explanation and example are the majority of the screens; the interactive blocks are how the student proves they followed. A Learn lesson ends with a one-line summary card the student can reopen later as a cheat sheet.

Example, from Git & GitHub, lesson "Branches":

```
1. Explain  — A branch is a movable pointer to a commit. (diagram: main → C3)
2. Visual   — Drag to create a branch at C3. Watch the pointer split.
3. Predict  — After one commit on `feature`, where does `main` point?
4. Explain  — Merging brings a branch's commits back. (diagram: merge commit)
5. Reorder  — Put the commands in order: checkout, commit, merge, branch.
6. Summary  — "branch = pointer; merge = join." → Open VS Code to do it for real.
```

## The Do half (extension)

A Do lesson is a list of steps. Each step has:

- **Instruction.** One to three sentences of what to do and why.
- **Pointer.** What the extension highlights: a file, a line range, a terminal, a panel, a Godot dock or node.
- **Check.** A condition on real state that marks the step done.
- **Hint.** Optional, revealed on request or after the check has failed a few times.
- **Reveal.** Optional "show me": the extension applies the step's solution so the student can keep moving. Used for guided builds, not for checkpoints.

Example, continuing "Branches":

```
Step 3 of 7 — Create a branch
  Instruction: Make a branch called feature/card and switch to it.
  Pointer:     Terminal; Source Control branch picker
  Check:       current branch is feature/card
  Hint:        `git switch -c feature/card`
```

Step types the extension supports:

| Step type | Check reads | Example |
|---|---|---|
| **Create / edit file** | Workspace files | `index.html` exists and contains a `<main>` element |
| **Make the test pass** | Test runner output | `stack.test.js` passes |
| **Run a command** | Terminal command history and its effect on the filesystem | `mkdir`'d the right folder |
| **Git state** | Real git via the extension | Branch `feature/card` merged into `main`; no conflict markers left |
| **Fix the bug** | Test runner | Planted bug, failing test, make it green |
| **Godot: scene state** | Editor scene tree and node properties | `Player` has a `CollisionShape2D` child with a shape set |
| **Godot: run the scene** | Editor run signal and script output | Scene runs without errors and prints "ready" |

Six web blocks and seven tool step types, built once, reused in every lesson. Adding a block or step type is a platform change; adding a lesson is a content change.

## Help and AI

A **"Help me"** button, on both halves, sends the current screen or step, the student's attempt or failing check, and the relevant file (not a screenshot) to a model and returns a hint, not a solution. This is Duolingo's "explain my mistake." It is structured and cheap.

Officers can see, per student, which screen or step they've been stuck on and for how long. That is the whole "teacher view": a stuck list, not a gradebook.

## Progression

- **Path map.** A visible path per stage: Fundamentals, then the chosen track. Units are nodes, lessons are steps inside. Each lesson shows its two halves.
- **Checkpoints.** Each unit ends with a checkpoint: a Learn set with no answers shown until the end, then a Do set with no hints and no reveal. Passing unlocks the next unit. Students who already know the material can test out by passing the checkpoint.
- **Spaced review.** Blocks and steps a student got wrong come back in later lessons as short review items.
- **Streak and XP, lightly.** A streak and XP per lesson, and an opt-in club leaderboard.
- **Explicitly skipped:** hearts / lives, timed tests, demotion leagues.
- **Club rhythm.** Weekly meetings map to units. Officers use the stuck list to pair people up.

## Tools by track

| Stage / track | Tool | Extension | Language |
|---|---|---|---|
| Fundamentals | VS Code | VS Code extension | JavaScript / Node, git, shell |
| Front End | VS Code | VS Code extension | HTML, CSS, JS, then a framework |
| Game Dev | Godot | Godot editor plugin | GDScript |
| AI Engineer | VS Code | VS Code extension | Python |
| Data Science | VS Code (notebooks) | VS Code extension | Python |

Two extensions cover all five stages. The VS Code extension is built first and carries four of them. The Learn half is the same web app for every track.

## Authoring model

- A lesson is one folder under `content/` with two files: `learn.mdx` (Markdown with the web blocks as components) and `do.yaml` (the step list). Officers write both; no UI work.
- Each Do lesson ships with, or builds on, a starter project the extension opens or scaffolds.
- Content goes through normal PR review, so lesson history is git history.
- Rough effort: a Learn half is 1–2 hours, a Do half is 1–2 hours once the starter project exists. Fundamentals is 7 modules × ~6 lessons ≈ 40 lessons ≈ 80–160 officer-hours, plus building the blocks and the extension. That is the cost of a platform that explains *and* practices. Where time is short, a lesson can ship Do-only with a one-screen Explain.

**Licensing.** Lessons are club-authored. Only MIT / CC0 material from the `fundamentals.md` catalog can be pulled in: Exercism JS exercises (MIT) as fix-the-bug and make-the-test-pass steps, MDN code samples (CC0) in Explain screens, Hack Club workshop code (MIT), Learn Git Branching (MIT) as the basis for the git Visual block. Everything else is reference for authors and is not linked from lessons.

## Projects

1. **Guided builds.** Do lessons with a check per step and "show me" available. The achievable version of Scrimba's editable screencasts.
2. **Open projects with checks.** A spec, a starter project, and automated checks on behavior. Students solve it their own way. These are the module-end projects in `fundamentals.md`.
3. **Capstone with review.** A real repo on the student's machine, pushed to GitHub if open decision 2 says yes. Automated checks plus an officer or peer review. The reviewed capstone becomes the Student Profile.

Because everything is real, every project is already a portfolio piece. The profile is built from repos, not from scores.

## Industry certifications

Each specialization track includes an industry certification. Forked cannot issue these; a vendor does. The platform's job is to prepare the student in-platform, get them to the exam, and put the result on their profile.

**How it fits the model:**

- **Prep is in-platform.** Each cert's exam objectives map to Learn/Do lessons in the track. Most objectives are covered by the track's normal lessons; the gaps get dedicated cert-prep lessons. No "go study on the vendor's site."
- **Practice exams are the one place multiple choice exists.** Every cert exam is multiple choice, so the prep has to include it. A practice-exam mode uses the checkpoint format, is clearly labeled as exam prep, and is never the way a concept is first taught.
- **The exam is external.** The student registers and sits the exam with the vendor. The platform tracks readiness (objectives covered, practice-exam scores) and links to registration.
- **The credential goes on the profile.** The student adds the credential URL or Credly badge; officers verify it. Certifications become a filter for businesses and an input to matching.
- **Optional, not gating.** A track is completed by its capstone, not by the cert. The cert is a recommended extra with a clear payoff.

**Proposed certification per stage.** Entry-level, recognized, and cheap or free for students. Prices and student vouchers change; verify before publishing.

| Stage / track | Certification | Vendor | Notes |
|---|---|---|---|
| Fundamentals | GitHub Foundations | GitHub | Free exam voucher for verified students via GitHub Education. Fits module 2 directly. |
| Front End | Meta Front-End Developer Professional Certificate | Meta via Coursera | Paid subscription; financial aid available. Alternative: freeCodeCamp certs (free, but earned on their site). |
| Game Dev | Unity Certified Associate: Game Developer | Unity | Paid. **Mismatch:** the track teaches Godot, which has no official cert. Options: skip the cert and treat a shipped game on itch.io as the credential, or add a short Unity bridge. Open decision 7. |
| AI Engineer | Azure AI Fundamentals (AI-900) | Microsoft | Paid; free vouchers sometimes available to students. Alternative: AWS Certified AI Practitioner. |
| Data Science | Azure Data Fundamentals (DP-900) | Microsoft | Paid; same voucher situation. Alternative: Google Data Analytics Professional Certificate (Coursera). |
| Cross-track (DevOps) | AWS Certified Cloud Practitioner (CLF-C02) | AWS | Paid. The DevOps Club cert; recommended for any track after the capstone. |

**Authoring.** A cert is a file under `content/certs/` listing the vendor's exam objectives, which lessons cover each, and the practice-exam question bank. Authoring the question bank is the main cost: roughly 60–100 questions per cert.

## Mapping to the fundamentals modules

| Module | Learn (web) | Do (extension) | Project |
|---|---|---|---|
| 0. Install & connect (new) | Explain only | Web-only onboarding | VS Code, Node, extension installed; first project open |
| 1. Terminal & dev setup | Explain, Fill the blank, Reorder | Run a command, create file | Scavenger hunt in the real terminal, checked from the filesystem |
| 2. Git & GitHub | Explain, Visual (git graph), Predict, Spot it | Git state, run a command | Fork the club repo, add a profile card, open a PR |
| 3. Programming fundamentals (JS) | Explain, Predict, Fill the blank, Reorder | Make the test pass, fix the bug | CLI game (guess-the-number / hangman) with tests |
| 4. Data structures basics | Explain, Visual (stack/queue), Predict, Spot it | Make the test pass, fix the bug | Stack and queue with tests; undo feature added to the game |
| 5. HTML & CSS | Explain, Visual (flexbox), Fill the blank | Create / edit file (with live preview) | Personal page, as a guided build |
| 6. How the web works + DOM | Explain, Visual (request lifecycle), Predict | Fix the bug, create / edit file | Dark-mode toggle or fetch-and-display on the week-5 page |
| 7. Capstone | Summary cards from modules 1–6 | Git state, make the test pass | Portfolio v0 as an open project; officer review |

## MVP scope

Prove the concept with the smallest slice that feels like the whole thing:

1. **Web app** with sign in, the path map, progress sync, and three Learn blocks: Explain, Predict, Fill the blank.
2. **VS Code extension** with the side panel, pointers, and four step types: create/edit file, run a command, git state, make the test pass.
3. **Modules 0, 2, and 3** as content (onboarding, git, JS fundamentals), each with both halves.

Explicitly later: Visual and Spot-it blocks, Godot plugin, Python tracks, spaced review, leaderboard, help-me AI, profile and matching, certification prep and practice exams.

## Risks

- **Device access.** Desktop VS Code and Godot do not run on school Chromebooks. The Learn half still works there, but the Do half does not. Confirm what members use before building.
- **Onboarding drop-off.** Installing tools is the first hour and the biggest leak. Lesson 0 has to be excellent and officers should run it live at a meeting.
- **Two surfaces to keep in sync.** A lesson now spans the web and the extension. Progress sync and the hand-off between them have to be smooth or it feels like two products.
- **Extensions are software.** Two extensions to maintain against editor updates, not just content.
- **Check brittleness.** Checks on real state must tolerate variation (different file names, different commit messages) or students get stuck on technicalities. Check behavior, not exact text.
- **Learn half drifting into a textbook.** The temptation is to write pages. The block limits (one idea per screen, three to five sentences) exist to stop that.

## Open decisions

1. **Devices.** What do members actually use? This decides whether the Do half is viable for everyone.
2. **Capstone and PRs on real GitHub.** Module 2 and the capstone use a real GitHub account. That's the strongest profile story, but it means every student needs an account and the club practice repo must be public. Confirm.
3. **Live preview for HTML/CSS.** Rely on an existing VS Code Live Server extension, or build preview into ours?
4. **Godot plugin timing.** Game dev is likely the most-requested track. Build the Godot plugin right after MVP, or run game dev in VS Code with a JS canvas library until it's ready?
5. **Help-me AI.** Ship in MVP or hold until there's usage data on where students get stuck?
6. **Learn-only fallback.** If some members can't install tools, do they get a Learn-only path with in-browser practice, or is the Do half required to progress?
7. **Game dev certification.** Unity cert with a Godot track, no cert, or a shipped game as the credential?
8. **Who pays for exams.** Student vouchers cover some; the rest is out of pocket or club-funded. Decide before promising a cert per track.
