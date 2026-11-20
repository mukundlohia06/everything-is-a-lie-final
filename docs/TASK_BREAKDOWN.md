# TASK_BREAKDOWN

> **Status:** ACTIVE (planning document).

Estimates are **ideal developer-days** (±30% for P1–P2; ±50% beyond). Task IDs follow `T-Pn-nn` (NAMING_CONVENTIONS §4). Deps list task IDs. Status column is maintained in the tracker, not here.

## Epics

| Epic | Description | Phases |
|---|---|---|
| E1 | Project foundation & tooling | P1 |
| E2 | Player & interaction | P1–P2 |
| E3 | Input, settings, accessibility | P1–P2 |
| E4 | UI/UX | P1–P2 |
| E5 | Truth/Lie system | P2–P3 |
| E6 | Behavior system | P4 |
| E7 | Save system | P1–P3 |
| E8 | Levels & puzzles | P2, P5 |
| E9 | Art, lighting, VFX | P2, P6 |
| E10 | Audio | P2, P7 |
| E11 | Performance & QA | P8–P9 |
| E12 | Build & release | P9–P10 |

## Phase 1 — Core interaction prototype (6 weeks)

| ID | Task | Epic | Est (d) | Deps | Acceptance |
|---|---|---|---|---|---|
| T-P1-01 | Verify repository initialization (skeleton, `.gitignore`, `.gitattributes`, LFS) and that `game/project.godot` opens in Godot 4.7.2; check that the chosen test framework (gdUnit4/GUT) and other plugins support 4.7.2 | E1 | 1 | — | Fresh clone + `git lfs pull` imports without errors; result recorded in DESIGN_DECISIONS |
| T-P1-02 | CI: extend `.github/workflows/godot.yml` (import check exists) with lint and headless test jobs | E1 | 2 | 01 | PR checks run |
| T-P1-03 | `EventBus`, `GameState` autoloads (+ `FlagStore`, `ChangeJournal`, `RngService`) and the static `Log` class | E1 | 2 | 01 | Signals typed; unit tests |
| T-P1-04 | `StateMachine` base | E1 | 1 | 01 | Tests pass |
| T-P1-05 | `InputService` + action map (KB/M + gamepad), actions created in the `game/project.godot` InputMap | E3 | 3 | 03 | All actions bound; glyph detect |
| T-P1-06 | First-person controller (move, look, step audio hook) | E2 | 3 | 04, 05 | No jitter at 60 FPS; Reduced Motion toggle for bob |
| T-P1-07 | `InteractionSystem` + `Interactable` base | E2 | 4 | 06 | Focus dot + verbs; 10 test props |
| T-P1-08 | Examine mode with detail layers | E2 | 3 | 07 | 3 layers on test object |
| T-P1-09 | Carry/drop with respawn anchor | E2 | 2 | 07 | Soft-lock guard works |
| T-P1-10 | Greybox room + `LevelBase` template | E8 | 2 | 06 | Loads via `LevelService` |
| T-P1-11 | `LevelService` (async load, transition stub) | E8 | 3 | 10 | No hitch > 33 ms in load |
| T-P1-12 | Ledger stub (SubViewport quad, mark/link UI mock) | E4 | 4 | 07 | Opens/closes; data-bound to stub model |
| T-P1-13 | `SaveService` stub (player pos + flags, atomic write) | E7 | 3 | 03 | Round-trip test |
| T-P1-14 | `DebugService` overlays (F1–F4) | E1 | 2 | 03 | Stripped in release |
| T-P1-15 | Spikes S1–S5 + reports | E9 | 8 | 10 | Reports in `docs/spikes/` (created with the first report) |
| T-P1-16 | Settings stub (`settings.cfg`, SettingDef list) | E3 | 2 | 05 | Defaults generated |
| T-P1-17 | UI motion spike: StampButton, Folder open, Ledger open (pixel-aligned), caption bar, loading strip | E4 | 6 | 12 | 60 FPS; matches UI_COMPONENT_SPEC states; reviewed at 3 text scales |
| T-P1-18 | Look-dev keyframes (Intake Hall, Mirror Gallery, Vault) + UI board | E9 | 8 | — | Art director approval before kit modeling |

## Phase 2 — Vertical slice L01–L04 (14 weeks)

| ID | Task | Epic | Est (d) | Deps | Acceptance |
|---|---|---|---|---|---|
| T-P2-01 | `StatementDef/SourceDef/EvidenceDef` resources (schema v1) | E5 | 3 | P1-03 | Validates sample data |
| T-P2-02 | Truth stub: `deliver/evaluate/export/import` | E5 | 3 | P2-01 | Unit tests |
| T-P2-03 | `StatementCarrier` (intercom, slip, plaque) | E5 | 4 | P2-02 | Delivers on triggers |
| T-P2-04 | `DialogueService` + captions system | E5/E3 | 4 | P2-03 | Captions per R12 |
| T-P2-05 | `EvidenceNode` + `PuzzleDef` + `PuzzleController` | E8 | 5 | P2-02 | L01 puzzle data-driven |
| T-P2-06 | Level L01 greybox → final | E8/E9 | 20 | P2-05 | Spec met; 60 FPS |
| T-P2-07 | Level L02 | E8/E9 | 15 | P2-06 | Omission fair; metrics |
| T-P2-08 | Level L03 (audio mismatch) | E8/E9/E10 | 18 | P2-06 | Door audio puzzle works |
| T-P2-09 | Level L04 (conflict) | E8/E9 | 15 | P2-06 | Cuff match visible |
| T-P2-10 | Main menu (Intake desk) | E4 | 8 | P1-05 | Controller OK |
| T-P2-11 | The Folder (pause) + Save/Load screens | E4/E7 | 8 | P1-13 | Slot UI works |
| T-P2-12 | Settings UI generated from SettingDefs | E3/E4 | 6 | P1-16 | UI = applied values |
| T-P2-13 | Accessibility v1 (subtitles, text scale, reduced motion/flashing, hold→toggle) | E3 | 8 | P2-12 | ACCESSIBILITY §3 for L01–L04 |
| T-P2-14 | Theme, fonts, stamps, paper shaders | E4/E9 | 8 | — | Style guide conformity |
| T-P2-15 | Kit modeling (Civic hall, waiting, corridor) | E9 | 20 | — | Budgets met |
| T-P2-16 | Hero props (ticket machine, bell, Clerk window, coats) | E9 | 15 | — | Review gate passed |
| T-P2-17 | Lighting bakes + post profile (Act I) | E9 | 8 | P2-15 | Perf within budget |
| T-P2-18 | Audio v1: room tones, foley, UI, Clerk VO (temp) | E10 | 15 | — | Mix targets |
| T-P2-19 | Drift D0–D2 effects (subset) | E9 | 6 | P2-17 | Reduced Motion variants |
| T-P2-20 | Vertical-slice playtest (6) + report | E11 | 5 | all | M2 criteria |
| T-P2-21 | UI component library + master Theme from tokens + token linter | E4 | 8 | P1-17 | Zero off-token values; default Theme fails CI |
| T-P2-22 | Notification system (P0–P3) + autosave stamp + Ledger tick | E4 | 4 | P2-21 | UI_COMPONENT_SPEC §5.7 rules enforced |
| T-P2-23 | Visual polish gate review (M2) + component state sheets | E11 | 3 | P2-21 | ≥ 85%, zero Critical failures |
| T-P2-24 | Cinematic/reveal animatics for L01–L04 (CM-01, RV-01..03) | E9 | 4 | P1-18 | Animatics approved |

## Phase 3 — Truth/Lie system (12 weeks, overlaps P2)

| ID | Task | Est (d) | Deps | Acceptance |
|---|---|---|---|---|
| T-P3-01 | Conditions DSL + temporal statements | 6 | P2-02 | L07-like test case |
| T-P3-02 | Evidence graph + Ledger model (links, topology) | 8 | P2-05 | No verdict leak |
| T-P3-03 | Mutation events + Change Journal | 5 | P3-01 | R7 check |
| T-P3-04 | Reinterpretation chain + literal-consistency checker | 6 | P3-01 | R-consistency tests |
| T-P3-05 | CSV→`.tres` importer + authoring doc | 4 | P2-01 | Writers can author |
| T-P3-06 | `truth_lint` R1–R14 + inversion solver (`res://scripts/utilities/lint/`) | 12 | P3-02 | CI gate |
| T-P3-07 | Save integration (variant IDs, timeline positions) | 5 | P3-03 | Deception-safe tests |
| T-P3-08 | Truth debug view (F2) | 3 | P3-02 | Shows evaluation |
| T-P3-09 | 60-statement authored set for L01–L08 | 10 | P3-05 | Lint clean |

## Phase 4 — Behavior system (6 weeks)

| ID | Task | Est (d) | Deps | Acceptance |
|---|---|---|---|---|
| T-P4-01 | Event ingestion + counters | 4 | P1-03 | Unit tests |
| T-P4-02 | Disposition vector + EMA + confidence | 3 | P4-01 | Math tests |
| T-P4-03 | `AdaptationPlanner` + whitelist | 6 | P4-02 | Cooldown tests |
| T-P4-04 | Adaptation catalog implementations | 8 | P4-03 | Each clamped |
| T-P4-05 | Fixed/Adaptive/Tracking-off modes + Settings UI/Reset | 3 | P4-03 | Verified |
| T-P4-06 | Determinism replay harness | 5 | P4-03 | 100/100 pass |
| T-P4-07 | L10 prototype + "Your File" data model | 6 | P4-04 | Canned fallback |

## Phase 5+ — Content and polish (summary tasks)

| ID | Task | Est (d) | Notes |
|---|---|---|---|
| T-P5-01…16 | One task per level L05–L20 (spec → greybox → puzzle → art → audio → perf) | 15–25 each | Waves A–D |
| T-P5-17 | Endings and epilogues (Amend/Affirm/Expunge/Corroborate) | 20 | Needs L20 |
| T-P5-18 | Second Reading (NG+) variants | 15 | Cut-line 4 |
| T-P6-01 | Lighting/grade pass all acts | 15 | |
| T-P6-02 | Drift VFX D3–D5 + Reduced variants | 12 | |
| T-P6-03 | UI transitions/animations polish | 8 | |
| T-P7-01 | VO recording + integration | 15 | Actor schedule |
| T-P7-02 | Music + `MusicDirector` | 15 | |
| T-P7-03 | Final mix + captions audit | 10 | |
| T-P8-01 | Per-level perf pass | 20 | |
| T-P8-02 | Stutter/prewarm pass | 8 | |
| T-P9-01 | QA cycles + fairness playtests | 30 | |
| T-P9-02 | Accessibility testing | 10 | |
| T-P10-01 | Store assets, build signing, release | 15 | |

## Definition of Done (all tasks)

1. Acceptance criteria met and demonstrated (video/screenshot for visual).
2. Unit/integration tests added or updated; CI green.
3. Docs updated if behavior/API changed.
4. No new warnings; naming conventions pass.
5. Perf within budget (if scene-affecting).
6. License ledger updated (if assets added).
