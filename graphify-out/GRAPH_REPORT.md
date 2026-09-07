# Graph Report - Forked  (2026-09-06)

## Corpus Check
- Corpus is ~7,330 words - fits in a single context window. You may not need a graph.

## Summary
- 119 nodes · 185 edges · 9 communities
- Extraction: 81% EXTRACTED · 19% INFERRED · 0% AMBIGUOUS · INFERRED: 36 edges (avg confidence: 0.93)
- Token cost: 182,408 input · 0 output

## Community Hubs (Navigation)
- Learn Blocks & Do Steps
- Fundamentals Modules & References
- Platform Architecture & Agent Docs
- Progression, MVP & Open Decisions
- Curriculum Flow Diagram
- Tracks & Certifications
- Code of Conduct
- Projects, Capstone & Matching
- Security Policy

## God Nodes (most connected - your core abstractions)
1. `Reference vs Incorporate (Licensing Ground Rule)` - 13 edges
2. `Two Halves (Learn + Do)` - 10 edges
3. `The Do Half (extension)` - 10 edges
4. `Coding Curriculum Brainstorm Diagram` - 10 edges
5. `Four Tracks` - 9 edges
6. `The Learn Half (web)` - 9 edges
7. `Authoring Model (content/ folder)` - 8 edges
8. `Industry Certifications (per track)` - 7 edges
9. `Student Profiles` - 7 edges
10. `Community Impact Enforcement Guidelines` - 6 edges

## Surprising Connections (you probably didn't know these)
- `Four Specialization Tracks` --conceptually_related_to--> `Four Tracks`  [INFERRED]
  CLAUDE.md → README.md
- `Authoring Model (content/ folder)` --conceptually_related_to--> `Lessons Are Data (content/ model)`  [INFERRED]
  docs/learning-model.md → CLAUDE.md
- `Module 2: Git & GitHub` --conceptually_related_to--> `Forked (Project Name & Metaphor)`  [INFERRED]
  docs/fundamentals.md → README.md
- `Tools by Track` --conceptually_related_to--> `Four Tracks`  [INFERRED]
  docs/learning-model.md → README.md
- `The Learn Half (web)` --conceptually_related_to--> `Learn Half (web)`  [INFERRED]
  docs/learning-model.md → README.md

## Hyperedges (group relationships)
- **Curriculum Flow: Fundamentals -> Tracks -> Profiles -> Matching** — readme_fundamentals_stage, readme_four_tracks, readme_student_profiles, readme_matching [EXTRACTED 1.00]
- **Six Learn Blocks** — docs_learning_model_explain_block, docs_learning_model_predict_block, docs_learning_model_fill_the_blank_block, docs_learning_model_reorder_block, docs_learning_model_spot_it_block, docs_learning_model_visual_block [EXTRACTED 1.00]
- **Seven Do Step Types** — docs_learning_model_create_edit_file_step, docs_learning_model_make_test_pass_step, docs_learning_model_run_command_step, docs_learning_model_git_state_step, docs_learning_model_fix_the_bug_step, docs_learning_model_godot_scene_state_step, docs_learning_model_godot_run_scene_step [EXTRACTED 1.00]
- **Specialize branches into four tracks feeding Student Profiles** — docs_curriculum_flow_specialize, docs_curriculum_flow_game_dev, docs_curriculum_flow_front_end, docs_curriculum_flow_ai_engineer, docs_curriculum_flow_data_science, docs_curriculum_flow_student_profiles [EXTRACTED 1.00]
- **AI Matching and Businesses converge on Unpaid Internships** — docs_curriculum_flow_student_profiles, docs_curriculum_flow_ai_matching, docs_curriculum_flow_businesses, docs_curriculum_flow_unpaid_internships [EXTRACTED 1.00]

## Communities (9 total, 0 thin omitted)

### Community 0 - "Learn Blocks & Do Steps"
Cohesion: 0.09
Nodes (25): Create / Edit File Step, The Do Half (extension), Explain Block, Fill the Blank Block, Fix the Bug Step, Git State Step, Godot: Run the Scene Step, Godot: Scene State Step (+17 more)

### Community 1 - "Fundamentals Modules & References"
Cohesion: 0.18
Nodes (20): Licensing Rule (Incorporate vs Reference), CS50x, Exercism (JS Track), freeCodeCamp, Hack Club Workshops, Learn Git Branching, MDN Learn Web Development, MIT Missing Semester (+12 more)

### Community 2 - "Platform Architecture & Agent Docs"
Cohesion: 0.13
Nodes (18): CLAUDE.md (Agent Guidance), Forked (Learning Platform), Four Specialization Tracks, Godot Editor Plugin, Planned Web App Stack, VS Code Companion Extension, Platform Architecture, Godot Editor Plugin (+10 more)

### Community 3 - "Progression, MVP & Open Decisions"
Cohesion: 0.18
Nodes (13): Lessons Are Data (content/ model), MVP Slice, Rejected Teaching Patterns, Fundamentals Open Questions, Checkpoints, MVP Scope, Open Decisions, Path Map (+5 more)

### Community 4 - "Curriculum Flow Diagram"
Cohesion: 0.42
Nodes (11): AI Engineer Track, AI Matching, Businesses, Data Science Track, Front End Track, Fundamentals, Game Dev Track, Coding Curriculum Brainstorm Diagram (+3 more)

### Community 5 - "Tracks & Certifications"
Cohesion: 0.36
Nodes (9): GitHub Foundations Certification, Industry Certifications (per track), AI Engineer Track, Data Science Track, Four Tracks, Front End Track, Fundamentals Stage, Game Dev Track (+1 more)

### Community 6 - "Code of Conduct"
Cohesion: 0.25
Nodes (7): Contributor Covenant v2.1, Correction (Enforcement Level 1), Community Impact Enforcement Guidelines, Mozilla Code of Conduct Enforcement Ladder, Permanent Ban (Enforcement Level 4), Temporary Ban (Enforcement Level 3), Warning (Enforcement Level 2)

### Community 7 - "Projects, Capstone & Matching"
Cohesion: 0.33
Nodes (7): Module 7: Capstone (Portfolio v0), Capstone with Review, Guided Builds, Open Projects with Checks, Projects (three types), Matching (AI / Business Browsing), Student Profiles

### Community 8 - "Security Policy"
Cohesion: 0.50
Nodes (3): GitHub Private Vulnerability Reporting, Supported Versions Policy, Vulnerability Reporting Process

## Knowledge Gaps
- **31 isolated node(s):** `Contributor Covenant v2.1`, `Mozilla Code of Conduct Enforcement Ladder`, `Correction (Enforcement Level 1)`, `Warning (Enforcement Level 2)`, `Temporary Ban (Enforcement Level 3)` (+26 more)
  These have ≤1 connection - possible missing edges or undocumented components. (Counts symbols only; 40 node(s) total have ≤1 connection when file, concept and rationale nodes are included.)

## Suggested Questions
_Questions this graph is uniquely positioned to answer:_

- **Why does `Two Halves (Learn + Do)` connect `Learn Blocks & Do Steps` to `Progression, MVP & Open Decisions`?**
  _High betweenness centrality (0.152) - this node is a cross-community bridge._
- **Why does `The Do Half (extension)` connect `Learn Blocks & Do Steps` to `Progression, MVP & Open Decisions`?**
  _High betweenness centrality (0.094) - this node is a cross-community bridge._
- **Why does `The Learn Half (web)` connect `Learn Blocks & Do Steps` to `Progression, MVP & Open Decisions`?**
  _High betweenness centrality (0.076) - this node is a cross-community bridge._
- **Are the 2 inferred relationships involving `Reference vs Incorporate (Licensing Ground Rule)` (e.g. with `Licensing Rule (Incorporate vs Reference)` and `Authoring Model (content/ folder)`) actually correct?**
  _`Reference vs Incorporate (Licensing Ground Rule)` has 2 INFERRED edges - model-reasoned connections that need verification._
- **Are the 10 inferred relationships involving `Coding Curriculum Brainstorm Diagram` (e.g. with `AI Engineer Track` and `AI Matching`) actually correct?**
  _`Coding Curriculum Brainstorm Diagram` has 10 INFERRED edges - model-reasoned connections that need verification._
- **Are the 2 inferred relationships involving `Four Tracks` (e.g. with `Four Specialization Tracks` and `Tools by Track`) actually correct?**
  _`Four Tracks` has 2 INFERRED edges - model-reasoned connections that need verification._
- **What connects `Contributor Covenant v2.1`, `Mozilla Code of Conduct Enforcement Ladder`, `Correction (Enforcement Level 1)` to the rest of the system?**
  _31 weakly-connected nodes found - possible documentation gaps or missing edges._