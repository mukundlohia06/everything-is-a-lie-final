# PROJECT_STRUCTURE

> **Status:** ACTIVE — describes the repository as it is now. Folders exist; the files listed as *planned* do not.

## 1. Three kinds of paths

| Notation | Meaning | Example |
|---|---|---|
| **Repository path** (no prefix) | Relative to the repo root `everything-is-a-lie/` | `docs/GAME_DESIGN.md`, `tools/asset_tools/` |
| **`res://`** | Relative to the Godot project root, which is the **`game/`** folder | `res://scripts/core/event_bus.gd` = `game/scripts/core/event_bus.gd` |
| **`user://`** | Per-player runtime storage, created by the game; never in the repo | `user://saves/slot_1.json` |

**`res:// = everything-is-a-lie/game/`**. Documentation writes `res://` for anything inside the Godot project and plain repository paths for everything else (docs, tools, tests, `.github`, `builds`). A path such as `levels/` or `autoload/` without a prefix does not exist in this project.

## 2. Repository tree

```
everything-is-a-lie/
├── README.md
├── LICENSE                  # placeholder, decision pending
├── ASSETS_LICENSES.csv      # headers only until assets exist
├── CHANGELOG.md
├── .gitignore
├── .gitattributes
│
├── docs/                    # design and technical specification (this documentation set)
│
├── game/                    # ← Godot project root (res://)
│   ├── project.godot
│   ├── README.md
│   ├── scenes/
│   ├── scripts/
│   ├── assets/
│   ├── resources/
│   ├── shaders/
│   └── tests/               # in-engine tests (res://tests/)
│
├── tools/                   # external tooling (Python/shell); engine-dependent tools live in res://scripts/utilities/
│   ├── asset_tools/
│   ├── dialogue_tools/
│   ├── level_tools/
│   └── development_tools/
│
├── tests/                   # repository-level tests and test plans (not run inside the engine)
│   ├── unit/
│   ├── integration/
│   ├── gameplay/
│   └── regression/
│
├── builds/                  # local, git-ignored export output (.gitkeep only)
│
└── .github/
    ├── workflows/godot.yml
    ├── ISSUE_TEMPLATE/      # bug_report.md, feature_request.md, puzzle_issue.md
    └── pull_request_template.md
```

## 3. Godot project (`res://`)

```
res://
├── project.godot
├── export_presets.cfg                     # planned: created in the editor at the first export test
├── addons/                                # planned: third-party plugins (each with its LICENSE)
│
├── scenes/
│   ├── main/                              # main.tscn (root scene)
│   ├── player/                            # player_rig.tscn
│   ├── world/                             # shared world-space scenes: volumes, carriers, evidence nodes, anchors
│   ├── puzzles/                           # shared puzzle controller scenes
│   ├── ui/                                # theme/, screens/, components/
│   ├── rooms/                             # lNN_<name>/level.tscn (+ room sub-scenes), one folder per level
│   └── transitions/                       # lift, door pass-through, paper-blink, act slip
│
├── scripts/
│   ├── core/                              # event_bus, game_state, settings_service, input_service, debug_service,
│   │                                      # flag_store, change_journal, rng_service, log, state_machine/
│   ├── player/                            # player controller, camera rig, hands
│   ├── interaction/                       # interactable, interaction_system, detail_layer
│   ├── puzzle/                            # puzzle_service, puzzle_def, puzzle_controller
│   ├── truth_lie/                         # truth_service, statement_def, source_def, evidence_def,
│   │                                      # evidence_node, evaluator, evidence_graph, ledger_model
│   ├── behavior/                          # behavior_service, player_profile, adaptation_planner
│   ├── dialogue/                          # dialogue_service, statement_carrier
│   ├── world/                             # level_service, level_base, level_def, level_guard, drift controller
│   ├── ui/                                # ui_router, ui_screen, ledger/, screens/, components/
│   ├── save/                              # save_service, serializer, migrations/
│   ├── audio/                             # audio_service, music_director, reverb_zones
│   └── utilities/                         # shared helpers; lint/ (headless validators: truth_lint, validate_data)
│
├── assets/
│   ├── characters/  environments/  props/  materials/  textures/  animations/
│   ├── vfx/                               # particles/, post/ (per-act environment profiles)
│   ├── audio/{music,ambience,sfx,voice}/  # sfx/ has foley/ and ui/; voice/ has en/
│   ├── fonts/
│   └── icons/
│
├── resources/
│   ├── puzzles/                           # PuzzleDef .tres
│   ├── dialogue/                          # hearing (L15) graphs, Clerk reaction sets
│   ├── rooms/                             # lNN_<name>/level_data.tres (LevelDef)
│   ├── items/                             # ItemDef for carryable objects (marble, UV lamp, pencil…)
│   └── game_data/                         # statements/, sources/, evidence/, localization/, audio/, settings/
│
├── shaders/                               # *.gdshader (paper_ink, heat_shimmer, film_grain, …)
└── tests/                                 # in-engine tests: unit/, integration/, fixtures/ (gdUnit4 or GUT)
```

## 4. Folder responsibilities

| Folder | Holds | Does not hold |
|---|---|---|
| `scenes/rooms/lNN_<name>/` | The level scene and its room sub-scenes | Reusable props (→ `assets/props/`), shared volumes (→ `scenes/world/`) |
| `scenes/world/` | Reusable world-space nodes (WaitVolume, ReverbZone, DriftZone, AnchorMarker, RevealTrigger, CameraConstraint, StatementCarrier, EvidenceNode scenes) | Level-specific content |
| `scenes/ui/` | UI scenes and the master `Theme` (`theme/`) | UI logic (→ `scripts/ui/`) |
| `scripts/<domain>/` | Autoload service script **and** the classes it uses, per domain | Content/data |
| `resources/game_data/` | Authored data: statements, sources, evidence, localization, audio events, settings defaults | Scripts |
| `resources/rooms/` | One `level_data.tres` per level | Scenes |
| `assets/` | Game-ready imported assets only (`.glb`, `.png`, `.ogg`, fonts) | Source files (`.blend`, `.psd`, DAW sessions) — see §8 |
| `shaders/` | Shader files | Materials (→ `assets/materials/`) |

**Terminology.** A **level** is a numbered gameplay sequence (L01–L20). It may span one to four rooms. The `rooms/` folder holds level scenes; there is **no** separate `RoomManager`. `LevelService` and `LevelBase` are the authoritative level classes (DESIGN_DECISIONS DD-003).

**Autoloads** are registered in `project.godot` in the order given in TECHNICAL_ARCHITECTURE §3. Their scripts live in their domain folder (e.g. `res://scripts/truth_lie/truth_service.gd`); there is no `autoload/` or `systems/` folder (DD-006).

## 5. Planned files (not yet created)

| File/folder | Created by |
|---|---|
| `res://scenes/main/main.tscn` and the main-scene setting in `project.godot` | T-P1-10/T-P1-11 |
| Autoload entries and scripts (13) | T-P1-03, T-P1-05 and later tasks |
| InputMap actions in `project.godot` | T-P1-05 (INPUT_SYSTEM §1) |
| `game/addons/` (test framework) | T-P1-01 compatibility check, then T-P1-02 |
| `game/export_presets.cfg` | First export test (BUILD_AND_RELEASE §2) |
| `docs/spikes/`, `docs/levels/` | First spike report / first level spec |
| `art_source/`, `audio_source/` (optional, Git LFS) | Only if source files are versioned (DD-010) |

## 6. Tools and tests

**Tools.** External scripts (Python/shell, no engine needed) live in `tools/<area>/`. Tools that need the engine (they load `.tres` resources or scenes) live in the project at `res://scripts/utilities/lint/` and run headless, e.g. `godot --headless --path game --script res://scripts/utilities/lint/truth_lint.gd`. All are **planned**; none exists yet.

**Tests.**

| Location | Purpose | Runs in |
|---|---|---|
| `game/tests/` (`res://tests/`) | Executable unit/integration tests and fixtures (`fixtures/saves/`) | Godot, headless or editor |
| `tests/unit/` | Tests for the Python tools | Python |
| `tests/integration/` | Scripts that launch Godot and run `res://tests/` | CI / shell |
| `tests/gameplay/` | Per-level manual and scripted playthrough plans | Humans / replay harness |
| `tests/regression/` | Bug records (S0–S2) with repro and covering test | Documentation |

## 7. Runtime data (`user://`, never committed)

```
user://
├── saves/       slot_1.json, slot_1.bak, slot_2.json, slot_2.bak, slot_3.json, slot_3.bak, global.json
├── settings.cfg
└── logs/
```

## 8. Not in the repository

- Source art/audio (`.blend`, `.psd`, `.kra`, DAW sessions, raw recordings). Keep them outside `game/` so Godot does not import them. If they are versioned, use a root-level `art_source/` / `audio_source/` with Git LFS (DD-010).
- Compiled builds. `builds/` is a local scratch folder; distributables are published through GitHub Releases and/or store depots (BUILD_AND_RELEASE §5).
- `.godot/` (editor cache), `user://` data, secrets (`.env`).

## 9. Rules

1. Gameplay content (statements, puzzles, dialogue) is data in `res://resources/`, never hard-coded in scripts.
2. Autoload scripts stay thin (≤ 200 lines); logic lives in classes in the same domain folder.
3. A level folder contains only what that level uses; assets used by two or more levels move to `assets/props/` or `assets/environments/`.
4. No spaces and no uppercase in folder or file names (NAMING_CONVENTIONS); level folders are `lNN_<snake_name>`.
5. Every new top-level folder requires a DESIGN_DECISIONS entry and an update to this document.

## 10. Acceptance criteria

- Every path in the documentation resolves to a folder in §2/§3 or is listed in §5 as planned.
- CI (planned) fails if a file is added outside the structure above.
