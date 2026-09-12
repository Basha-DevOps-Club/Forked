# Forked Design System

> **Status:** MVP implementation contract. This document defines the shared visual and interaction language for the Learn web app and the Forked companion experience in VS Code. [`design-tokens.json`](design-tokens.json) is the machine-readable source for token values; this document is the source for how those tokens and interaction states are used.

## Product character

Forked should feel like a focused workshop: clear enough for a beginner, credible enough to remain useful as the student grows, and encouraging without feeling childish. The branching path is the central motif because every student begins with shared fundamentals and then chooses a specialization.

The interface is **clean and tonal-flat**. Use solid color, hierarchy, spacing, borders, and purposeful motion. Do not use paper texture or a paper metaphor, glass effects, decorative gradients, ambient card shadows, or illustration as structural decoration. Shadows are reserved for overlays and an item while it is being dragged.

The permanent Forked brand mark is Cerulean, even when a student chooses a different personal accent. Until final logo artwork exists, use a rounded Lucide-style `GitFork` icon as the branching-mark placeholder. Product icons use the same rounded line style and must come from an icon library rather than improvised glyphs.

## Foundations

### Approved direction and reference

The accent interview settled on six different hues with a similar perceived brightness and intensity to Cerulean—not literally the same hue. Warm Sand stays fixed in each appearance; students choose only the accent. The exact approved palette and accessible variants below supersede exploratory values.

The original [interactive accent picker](references/forked-accent-picker.html) is preserved as a historical visual reference. It is an HTML fragment from the conversation visualization host, not a production screen or standalone app. Its host styles, optional Lucide runtime, and follow-up-message bridge are external dependencies. Exploratory shadows, radii, dynamic contrast adjustments, and sample copy are not authoritative; use this specification and the token JSON for implementation.

### Color architecture

Every Forked-owned surface uses the same neutral **Warm Sand** foundation. A student's theme changes the accent, not the canvas. Personal accents are intentionally limited to actions, progress, focus, and selections. They do not recolor the entire interface and never override semantic status colors or fixed track colors.

#### Warm Sand

| Role | Light | Dark | Use |
|---|---:|---:|---|
| Canvas | `#F7F1E8` | `#17120D` | Page and full-screen background |
| Surface | `#FFFCF7` | `#211A13` | Raised-by-border panels, controls, menus |
| Text | `#1C1813` | `#FAF4EC` | Primary copy and headings |
| Muted text | `#70675D` | `#BAAEA1` | Secondary copy and metadata |
| Border | `#E3D8C9` | `#3E3227` | Default separators and component outlines |
| Strong border | `#958A7F` | `#776B60` | Emphasized boundaries and inactive control outlines |
| Code surface | `#1A1611` | `#0C0906` | Code, terminal, and diff content |
| Code text | `#FAF4EC` | `#FAF4EC` | Text on a code surface |

The surface hierarchy is canvas → surface → overlay. Canvas and surface differ tonally and by border; neither receives a default shadow. Code remains dark in both appearances so code has one stable visual context.

#### Personal accents

| Accent ID | Base | Light action | Light text | Dark |
|---|---:|---:|---:|---:|
| `cerulean` | `#087EA4` | `#006E93` | `#006287` | `#51B1D9` |
| `violet` | `#7D63A8` | `#6D5397` | `#62488A` | `#AF95DE` |
| `rose` | `#A75664` | `#954655` | `#893B4A` | `#DE8895` |
| `coral` | `#A85A46` | `#964A37` | `#893E2B` | `#DF8C76` |
| `amber` | `#936B17` | `#825B00` | `#765000` | `#C79D51` |
| `emerald` | `#358557` | `#227548` | `#10693D` | `#6AB987` |

Use the named variant from `color.accent.<id>` rather than assuming the base swatch is safe for every role:

- `base` is the approved identity swatch for non-text accents, diagrams, badges, and progress.
- `action` is the accessible light-appearance fill for controls; its foreground is `color.onAccent.light` (`#FFFCF7`).
- `text` is the accessibility-safe light-appearance value for links, labels, and other small text on Warm Sand.
- `dark` is the accessible dark-appearance value for fills, focus, progress, links, and labels; text placed on that color uses `color.onAccent.dark` (`#17120D`).
- Focus indicators use the focus-ring semantic token and must remain distinguishable from both the control and its surroundings.

#### Track colors

Track color is content metadata, not a personal preference. The mapping is fixed:

| Track | Fixed accent |
|---|---|
| Front End | Cerulean |
| Game Dev | Violet |
| AI | Emerald |
| Data | Amber |

Use track colors only on roadmap branches, track badges, and explanatory diagrams. Always pair track color with a text label, icon, or branch name. Fundamentals remains neutral with the student's accent used only for the current position and progress.

Semantic success, warning, and error colors are separate token roles. Do not derive them from the student's accent, and never communicate a state through color alone.

### Typography

- **Interface and learning copy:** Manrope.
- **Code, commands, file names, branch names, and technical values:** JetBrains Mono.
- **Default body:** `16px / 24px` with normal weight.
- **Lesson measure:** `720px` maximum for the instructional column; interactive diagrams may use a wider bounded stage when the task requires it.
- Keep headings short, use sentence case, and preserve a clear step between display, section, body, and supporting text sizes defined in the token file.
- Never shrink essential lesson copy to fit. Reflow the layout instead.

### Spacing, shape, and elevation

- All spacing values use a 4px base grid and the named scale in the token file.
- Controls use an `8px` radius. Containers, panels, and dialogs use a `12px` radius.
- Interactive targets are at least `44 × 44px`, even when their visible icon is smaller.
- Use a one-pixel default border for separation. A strong border is for selected, emphasized, or high-attention boundaries.
- Only overlays and the actively dragged item may use elevation tokens. Cards, navigation, lesson blocks, and code panels remain flat.

### Motion and sound

Motion explains state change; it does not decorate idle screens. Use short transitions for selection, reveal, panel, and progress changes. A completed lesson may animate the newly unlocked roadmap branch. Confetti is reserved for a major stage or capstone completion, never a routine answer or lesson.

Respect `prefers-reduced-motion`: remove travel, branch drawing, and confetti; replace them with an immediate state change and a concise text announcement. Sound is optional, defaults off, and is controlled only in Settings. No essential feedback depends on motion or sound.

## Token contract

[`design-tokens.json`](design-tokens.json) is platform-agnostic JSON and is the canonical value source for web CSS variables, Tailwind/shadcn theme bindings, Stitch generation, and extension theme adapters. Its stable namespaces are `color.neutral`, `color.code`, `color.onAccent`, `color.accent`, `color.track`, `color.status`, `typography`, `spacing`, `radius`, `size`, `motion`, `breakpoints`, `elevation`, and `defaults`.

Consumer adapters map those source paths to semantic roles—canvas, surface, text, border, action, focus, progress, feedback, code, and overlay—for the active appearance and accent. Components consume those semantic aliases rather than repeating JSON paths or hex values. A component-specific token may alias a semantic role, but it must not introduce an unregistered color.

Appearance defaults to the operating system. Theme preference is synchronized through the student profile and cached locally so web and extension can render the last known choice before remote data arrives. Remote `updatedAt` resolves conflicts; while offline, use the cached preference and queue the next sync. Never flash a different accent or appearance during hydration.

The VS Code host is the one deliberate surface exception: editor chrome continues to use native `--vscode-*` theme colors. The Forked panel uses native structure and typography where required by the host, then applies the student's Forked accent to actions, focus, progress, and selections. Do not recolor the surrounding editor or simulate VS Code in the web app.

## Responsive product shell

### Desktop and tablet, `≥768px`

- Use a collapsible left navigation rail. It is `72px` collapsed and `256px` expanded.
- At `768–1023px`, start collapsed. At `≥1024px`, start expanded. Preserve the student's rail choice locally after they change it.
- Expanded destinations show icon and label; collapsed destinations keep an accessible name and expose the label on hover and keyboard focus.
- The standard destination order is **Learn, Roadmap, Projects, Profile**. Settings is reached from Profile rather than added as a fifth primary destination.
- Main content remains fluid, with the instructional column capped at `720px` and centered within the available content area.

### Mobile, `<768px`

- Use a fixed bottom bar in this order: **Learn, Roadmap, Projects, Profile**.
- Include the device safe-area inset and reserve matching bottom padding in scrollable content so the bar never covers a control.
- Each item includes an icon and visible label. Selected state uses accent, weight, and a non-color cue.
- Do is not reproduced on mobile; mobile supports Learn, roadmap, progress, projects, and profile/settings. A Do handoff clearly says that a desktop editor is required.

### Focused lesson mode

Hide both the desktop rail and mobile bottom bar for a Learn lesson. The focused header contains only:

- **Back**, returning to the lesson or unit overview without discarding saved progress.
- The lesson title as a concise accessible label.
- Step progress, expressed visually and in text, such as “3 of 6.”

Do not add streaks, XP, profile controls, or unrelated navigation to this mode. When the lesson or handoff completes, restore the product shell.

### Core destinations

- **Learn/Home:** resume-first. The first actionable region resumes the exact lesson half and step. Secondary content shows the next unit and recent progress, not a generic feature dashboard.
- **Roadmap:** a guided vertical path. Fundamentals is a shared trunk; specialization creates clearly labeled track branches. Units are nodes and lessons are steps inside them. Locked, current, available, and complete states use icon/text cues in addition to color.
- **Projects:** guided builds and open projects with factual completion checks. It is present in navigation for the MVP shell even while the initial inventory is small.
- **Profile:** account settings and progress first. Portfolio material can extend it later.
- **Settings:** the only place to change personal accent, Light/Dark/System appearance, and sound. Changes apply account-wide.

## Learn and Do interaction rhythm

### Learn: think before reveal

A Learn session lasts about 3–5 minutes. Each screen teaches one idea, shows one concrete example or functional diagram, and asks for one meaningful action before moving on. Avoid walls of text, illustration-led layouts, fake terminals, and conventional multiple-choice drills.

The six supported blocks remain Explain, Predict, Fill the blank, Reorder, Spot it, and Visual. MVP implementation begins with Explain, Predict, and Fill the blank, but shared visual rules should not prevent the remaining blocks from being added later.

The normal feedback sequence is:

1. `idle`: the prompt and student action are available; Continue is absent or disabled.
2. `checking`: preserve the attempt, disable duplicate submission, and announce that it is being checked.
3. `incorrect`: keep the attempt visible and editable. Add calm, specific inline coaching that explains what to reconsider. Do not remove progress, shake the screen, consume a life, or show a punitive red takeover.
4. `correct`: acknowledge success without moving the student away from the result.
5. `revealed`: show the explanation of **why**, then enable a clear Continue action. The student controls when the next screen appears.

Code-focused Learn screens show one focused snippet at a time. Use diagrams to communicate real behavior, such as branches pointing to commits; do not substitute decorative illustration. A lesson ends with a one-line summary the student can reopen later.

“Help me” has a reserved placement on Learn and Do states but is not active in the MVP. Its future response is a hint based on the current step, attempt, failing check, and relevant file—not a solution and never a screenshot.

### Do: act in the real tool

A Do session lasts about 5–15 minutes and runs in VS Code for the MVP. The Forked panel presents one current step with instruction, pointer, factual check, optional Hint, and optional Show me where the lesson type allows it. The editor, terminal, filesystem, tests, and git state remain real.

When a check passes:

1. Mark the instruction and check with icon, text, and color.
2. Briefly confirm what fact passed.
3. Auto-advance to the next step after the confirmation is perceivable.
4. Announce the new step to assistive technology; reduced motion removes the animated transition without skipping the confirmation.

A failed check remains on the current step, explains the observed mismatch, and offers the existing Hint without penalizing the student. Checks validate behavior and state rather than brittle exact text.

### Web-to-editor handoff

The handoff carries the student to the exact lesson and Do step, not merely to the extension home. Preserve the identifiers through retry, reconnect, and progress sync. The web state and editor state must never both claim different current steps.

| State | Required presentation and behavior |
|---|---|
| `connected` | Confirm the signed-in extension and workspace destination. Enable **Open in VS Code** and name the exact next step. |
| `opening` | Show “Opening VS Code…” with a progress indicator and prevent duplicate launches. Preserve a manual fallback path. |
| `disconnected` | Explain whether VS Code, the extension, or sign-in is missing. Offer a single primary connection action and short ordered setup guidance. |
| `syncing` | Keep completion visible, show that progress is returning to the web app, and prevent conflicting navigation until the result resolves. |
| `complete` | Confirm the lesson and unlocked progress, restore navigation, and offer the next lesson. |
| `error` | Preserve all known progress. Explain the failed operation in plain language, offer Retry, and expose a copyable/manual open fallback. |

Deep-link failure must not strand the student. The manual fallback identifies the lesson and step to open from the extension, and Retry reuses the same context rather than creating a duplicate session.

## Shared TypeScript contracts

The web app, profile persistence, and VS Code extension share these names and values. Implementations may add transport metadata around them, but must not rename or reinterpret the public values.

```ts
type AccentId =
  | "cerulean" | "violet" | "rose"
  | "coral" | "amber" | "emerald";

type Appearance = "system" | "light" | "dark";

interface ThemePreference {
  accent: AccentId;
  appearance: Appearance;
  sound: boolean;
  updatedAt: string;
}

type HandoffState =
  | "connected" | "opening" | "disconnected"
  | "syncing" | "complete" | "error";

type FeedbackState =
  | "idle" | "checking" | "incorrect"
  | "correct" | "revealed";
```

Defaults are `{ accent: "cerulean", appearance: "system", sound: false }`; `updatedAt` is set when the preference is first persisted. `Appearance.system` resolves independently on each device while the selected accent and sound preference stay account-wide.

## Accessibility contract

Forked targets WCAG 2.2 AA across all six accents in both light and dark appearance.

- Normal text reaches at least `4.5:1`; large text and meaningful interactive boundaries reach at least `3:1`.
- Validate the complete semantic pairing matrix, not isolated palette swatches. Accent base may not be substituted for an accessibility-safe accent text token.
- Every interactive target is at least `44 × 44px` and has a visible keyboard focus indicator.
- Reading and focus order match the visual flow. Opening an overlay moves focus into it; closing it returns focus to the trigger.
- Current, locked, complete, correct, incorrect, warning, and error states always include text or iconography in addition to color.
- Reorder and drag interactions include keyboard controls such as Move up, Move down, and Place here, with position changes announced.
- Progress is exposed as text and an appropriate progress semantic, not only a colored line.
- Motion respects reduced-motion preferences; sound is never required and defaults off.
- Any optional short concept media includes captions and a transcript and is an enhancement inside an Explain block, not a replacement for the interaction.
- Error messages identify the problem next to the relevant control and preserve the student's work.
- The code surface supports horizontal scrolling without forcing the whole page to scroll sideways, and code meaning never depends only on syntax color.

## Stitch screen briefs

Create one Stitch project named **Forked — Design System**. Upload this specification as `DESIGN.md`, create one agnostic design-system asset from it, and use that asset for every screen. Generate the six anchor screens in light appearance with Cerulean as the personal accent, then generate the two named dark variants. Use exact tokens, Manrope, JetBrains Mono, rounded Lucide-style icons, and the `GitFork` placeholder. Do not add texture, paper styling, glass, decorative gradients, ambient card shadows, or unrequested illustration.

### 1. Desktop Home — resume first

- **Frame:** `1440 × 1024`, light, desktop rail expanded.
- **Primary outcome:** the student immediately resumes the exact active lesson and sees what comes next.
- **Content:** Forked mark; Learn selected in the rail; “Welcome back” greeting; dominant resume region for “Git & GitHub · Branches,” showing Learn/Do status and one primary Resume action; compact weekly/unit progress; a small preview of the next roadmap nodes.
- **Hierarchy:** resume is the only primary action. Progress is mastery-oriented; do not show grades, due dates, lives, or a feature-dashboard grid.
- **Visual check:** Warm Sand canvas, bordered surfaces, Cerulean action/progress only, fixed track color only where a track is explicitly labeled.

### 2. Mobile Roadmap — shared trunk to branches

- **Frame:** `390 × 844`, light, fixed bottom navigation with Roadmap selected.
- **Primary outcome:** the student understands their current position and how Fundamentals leads to specialization.
- **Content:** compact title/status header; scrollable vertical Fundamentals path with completed, current, available, and locked nodes; a visible specialization fork into Front End, Game Dev, AI, and Data branches; bottom destinations Learn, Roadmap, Projects, Profile.
- **Behavior implied by the still:** the current node is actionable, labels accompany every state, content clears the safe-area-aware bottom bar.
- **Visual check:** track colors only begin at named branches; the current personal progress remains Cerulean.

### 3. Desktop Learn Predict — Git branches

- **Frame:** `1280 × 900`, light, focused lesson mode with no rail.
- **Primary outcome:** the student predicts where a branch points before the explanation appears.
- **Content:** Back; “Branches”; text progress “3 of 6”; one short prompt; one focused JetBrains Mono command or commit snippet; a functional commit graph with `main`, `feature`, and labeled commits; an interactive graph target for the prediction instead of a conventional multiple-choice list; Submit action; reserved Help me placement shown unavailable for MVP.
- **State:** render `idle`, before the answer is revealed. Leave room for calm incorrect coaching or the why-reveal without changing the page structure.
- **Visual check:** instructional column at or below `720px`; code is on the stable dark code surface; no unrelated navigation or gamification.

### 4. Desktop connected handoff — web to VS Code

- **Frame:** `1280 × 900`, light, focused lesson mode.
- **Primary outcome:** the student trusts that the exact lesson is connected and opens the real editor.
- **Content:** completed Learn summary “branch = pointer; merge = join”; connected status with non-color icon and signed-in context; next destination “Do · Step 1 of 7”; primary **Open in VS Code** action; concise description of what will open; quiet manual-open fallback.
- **State:** `connected`. Show how the same bounded layout accommodates `opening`, `disconnected`, and `error` messages without moving the primary context.
- **Visual check:** do not draw a fake editor or terminal on the web screen.

### 5. Desktop VS Code Do — Create a branch

- **Frame:** `1440 × 1024`, light VS Code theme, Forked panel open.
- **Primary outcome:** the student knows the one real action to take and how completion is checked.
- **Content:** credible native editor chrome and workspace; Source Control/terminal pointer treatment; Forked side panel with “Step 3 of 7 — Create a branch”; instruction to create and switch to `feature/card`; factual check “Current branch is feature/card”; collapsed Hint revealing `git switch -c feature/card`; reserved Help me placement unavailable for MVP.
- **State:** waiting on the check, with the student's real workspace visible. Include the progress indicator and a clear connected/sync status.
- **Visual check:** native VS Code structural colors remain intact; Cerulean is limited to Forked action, focus, progress, and selected pointer. The panel feels native rather than like a miniature website.

### 6. Mobile completion — branch growth

- **Frame:** `390 × 844`, light.
- **Primary outcome:** the student understands what was mastered and where progress goes next.
- **Content:** concise “Branches complete” confirmation; summary “branch = pointer; merge = join”; completed Learn and Do facts; roadmap graphic with exactly one newly grown/unlocked segment; primary Next lesson action; bottom navigation restored.
- **Motion intent:** the new segment draws once, then settles. Reduced motion shows the final branch immediately with a status announcement.
- **Visual check:** this routine lesson completion has no confetti; sound remains off unless enabled in Settings.

### Dark variant A. Desktop Learn Predict

Duplicate anchor 3 without changing frame, spacing, content, component placement, or interaction state. Switch only to dark semantic tokens and the dark code surface. Re-check every accent, focus, border, text, graph, and code pairing for contrast.

### Dark variant B. Desktop VS Code Do

Duplicate anchor 5 without changing frame, content, component placement, or interaction state. Switch VS Code to a credible native dark theme and use Forked's dark-compatible accent roles in the extension panel. Do not paint the editor chrome Warm Sand or replace host theme tokens.

## Stitch artifact registry

Update this registry after each successful creation. IDs are intentionally blank until Stitch returns them. If generation times out, do not submit a duplicate request; poll for the resulting artifact, then record it here.

| Artifact | Stitch name | ID | Status / notes |
|---|---|---|---|
| Project | Forked — Design System | `projects/16567696131374142893` | Created private; existing projects untouched |
| Design-system asset | Forked Design System | `assets/e22bf22d5c3a4f4e96cca0e66f454896` | Agnostic asset; corrective theme update submitted in session `9857132063308128776` |
| DESIGN.md upload | DESIGN.md | `8148082560703981018` | Uploaded specification |
| Screen 1 | Desktop Home — Resume first | `dcfee6c963254f22b2a04448adad58c3` | Latest refined light anchor; older draft `365473efec4f455dbd9dddad00dc92fe` |
| Screen 2 | Mobile Roadmap — Shared trunk | `cc8fdd048e1c4967b6067d4fb9ed5019` | Latest refined mobile light anchor; older draft `6d04089964e740d5b33815b4164458d7` |
| Screen 3 | Desktop Learn Predict — Git branches | `6198f6535fcb40629ac8215493efb2a9` | Latest refined light anchor; older draft `854559c60ecc4c4db353e72f71d59941` |
| Screen 4 | Desktop connected handoff | `822606bcb62a4939b108d673969f1420` | Latest refined light anchor; older draft `93bda04c1edd43e88466d966e2a72d9b` |
| Screen 5 | Desktop VS Code Do — Create a branch | `0e7e036dd4524780a781ebc42791219f` | Generated light anchor; refinement pending |
| Screen 6 | Mobile completion — Branch growth | `6e7ba679aff2487c9d3c69adf3113ad0` | Generated; returned desktop metadata conflicts with mobile brief |
| Variant A | Desktop Learn Predict — Dark | `9d5e3e2a7cba4dea9b2c77bc9ace9097` | Generated refined dark variant; verify exact layout parity |
| Variant B | Desktop VS Code Do — Dark | `cdf7527990ee4a11933064cd9866b438` | Generated refined dark variant; verify exact layout parity |

Open the private [Forked — Design System Stitch project](https://stitch.withgoogle.com/projects/16567696131374142893?pli=1) with an authorized signed-in account. Preserve screen names so future revisions can identify their source anchor unambiguously. The registry records remote artifacts; generated screen HTML and images have not been exported into this repository.

### Generation verification status

The generated artifacts are drafts, not accepted implementations. Screen resource names use `projects/16567696131374142893/screens/{ID}`. The authenticated project now exposes the two dark variants and refined light anchors; older drafts remain in the project and are preserved for traceability.

Stitch's imported asset introduced cool-gray generic surface tokens, JetBrains Mono interface-label defaults, and a paper-like description that conflict with this specification. The repository tokens remain authoritative. Generated copy also introduces advanced curriculum claims and developer-facing status language absent from the approved briefs; these require removal or simplification. The Home includes an extra workspace-log region. Mobile completion returned desktop dimensions/device metadata. Exact frame sizes, responsive behavior, semantic colors, icon choice, and dark-layout preservation remain acceptance gates.

The corrective theme update supplied this full specification, Manrope interface labels, Warm Sand neutral, accessible Cerulean actions, and 8px controls. Existing screen rendering still needs verification against that correction.

The token JSON parsed successfully. All 12 accent/appearance combinations passed the defined normal-text pairings: light action labels have at least 5.54:1, light accent text at least 6.01:1, and dark accent text at least 6.60:1. Strong neutral boundaries reach at least 3.01:1 in light appearance and 3.32:1 in dark appearance. Default decorative borders must not be used as the sole interactive boundary. Whitespace validation passed.

Browser interaction, keyboard, screen-reader, handoff-state, and five-viewport checks must be performed on runnable screens before claiming those checks pass. Static generation descriptions do not constitute verification.

Use the repository's agent-driven `/graphify . --update` skill for documentation indexing. The running agent performs semantic extraction when no Gemini key is configured; an API key is not required. The headless CLI is a separate execution path and must not be treated as a prerequisite. The filename filter can classify `design-tokens.json` as potentially sensitive; this file contains public design values, and the same values are documented here for semantic indexing.

## QA and acceptance criteria

The documented direction is sufficient to begin implementation. The following are acceptance gates for the runnable implementation and generated screens, not claims that static drafts have passed:

### Token and visual QA

- Validate all 12 personal-accent/appearance combinations against WCAG 2.2 AA: `4.5:1` normal text and `3:1` large text and meaningful interactive boundaries.
- Compare rendered semantic values with [`design-tokens.json`](design-tokens.json); no screen uses an unregistered hex value for product UI.
- Confirm Warm Sand remains fixed when changing accent, track colors remain fixed when changing personal theme, and the Forked brand mark remains Cerulean.
- Confirm every screen is tonal-flat: no texture, paper metaphor, glass effect, decorative gradient, ambient card shadow, decorative illustration, or improvised icon.
- Confirm code uses JetBrains Mono on the stable dark code surface and interface text uses Manrope.

### Responsive and navigation QA

- Inspect widths `360`, `390`, `768`, `1280`, and `1440px` with no clipped content or page-level horizontal scrolling.
- At `360` and `390px`, show the safe-area-aware bottom bar outside focused lessons. At `768px` and wider, show the left rail outside focused lessons.
- Verify rail collapsed/expanded behavior, persistent accessible names, content reflow, and a `720px` maximum instructional measure.
- Verify focused lessons hide both navigation variants and retain Back, lesson label, and text progress.

### Interaction and accessibility QA

- Test keyboard-only and screen-reader paths for navigation, lesson submission, reveal, Continue, handoff, Hint, retry, and dialogs.
- Test all five feedback states, preserving the attempt through incorrect and error states and waiting for explicit Continue after the why-reveal.
- Test reorder/drag controls with pointer and keyboard alternatives and announced position changes.
- Confirm `44 × 44px` targets, visible focus, logical focus order, non-color state cues, reduced motion, sound-off default, and captions/transcripts for optional media.
- Confirm ordinary completion grows one branch without confetti; only stage and capstone milestones may celebrate with confetti.

### Handoff and editor QA

- Exercise `connected`, `opening`, `disconnected`, `syncing`, `complete`, and `error` across web and extension.
- Verify Retry and the manual fallback preserve the same lesson and step, duplicate launch is prevented, and progress is never lost.
- Verify a passed Do check confirms the factual result, syncs, and auto-advances; a failed check stays on the step and gives calm actionable guidance.
- Verify the web app never presents a fake terminal/editor and the extension reads editor, filesystem, test, and git state rather than the screen.
- Confirm the light and dark Learn variants have identical layout and state, and the light and dark VS Code variants preserve native host styling and identical Forked panel structure.

## Implementation handoff

Start with shared token adapters, the responsive app shell, and resume-first Home: desktop rail at 768px and above, mobile bottom navigation below it, fixed Warm Sand, and six personal accents. Continue with Roadmap, Learn, then the web-to-editor handoff and VS Code Do. Validate responsive behavior and accessibility as each slice becomes runnable. Do not copy unresolved Stitch details into code merely because they appear in a draft.

This repository still contains planning documents and visual references, not the running web app or extension. Remaining screen QA includes mobile completion sizing, light/dark layout parity, exact token/icon usage, simplified instructional copy, and the interaction checks above.

## MVP boundaries

This design system covers the student journey for responsive Learn, roadmap/progress, projects/profile shell, and desktop VS Code Do. Officer tools, business matching, certification screens, Godot, and full portfolio profiles are later. Mobile may manage Learn and progress but does not reproduce Do. “Help me” is designed as a reserved state but is not active in MVP. The student profile begins with account settings and progress; portfolio material can be added without changing primary navigation.
