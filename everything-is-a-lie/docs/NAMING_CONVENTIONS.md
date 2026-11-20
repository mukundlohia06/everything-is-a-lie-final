# NAMING_CONVENTIONS

> **Status:** ACTIVE — governs the repository as it is now.

## 1. Files and folders

| Item | Convention | Example |
|---|---|---|
| Folders | `snake_case`, no spaces | `l05_records_annex/` |
| Scripts | `snake_case.gd` | `interaction_system.gd` |
| Scenes | `snake_case.tscn` | `player_rig.tscn` |
| Resources | `snake_case.tres` | `l05_statements.tres` |
| Models | `type_name_variant.glb` | `prop_chair_wood_a.glb` |
| Textures | `t_<asset>_<map>.png` | `t_travertine_albedo.png` |
| Materials | `m_<name>.tres` | `m_brass_worn.tres` |
| Audio | `<cat>_<name>_<variant>.ogg` | `sfx_door_wood_open_01.ogg` |
| Shaders | `<name>.gdshader` | `paper_ink.gdshader` |
| Animations | `anim_<object>_<action>` | `anim_door_open` |
| Docs | `UPPER_SNAKE.md` | `TRUTH_LIE_SYSTEM.md` |

Prefixes: `TMP_` placeholder, `WIP_` work-in-progress (never merged to main).

## 2. GDScript

| Item | Convention | Example |
|---|---|---|
| Classes | `PascalCase` with `class_name` | `StatementCarrier` |
| Functions/variables | `snake_case` | `get_variant()` |
| Private | Leading underscore | `_evaluate_conditions()` |
| Constants/enums | `UPPER_SNAKE` | `MAX_NUDGE_TIER` |
| Signals | past-tense `snake_case` | `statement_delivered` |
| Booleans | `is_/has_/can_` | `can_interact` |
| StringName literals | `&"snake_case"` | `&"examine"` |
| Autoloads | `PascalCase` nodes | `TruthService` |

## 3. Node naming in scenes

`PascalCase`; unique nodes with `%`. Group names `snake_case` (`interactable`, `listenable`, `drift_zone`).

## 4. Data IDs

| Entity | Pattern | Regex | Example |
|---|---|---|---|
| Level | `Lnn` | `^L\d{2}$` | `L05` |
| Statement | `STM-Lnn-nnn` | `^STM-L\d{2}-\d{3}$` | `STM-L05-017` |
| Title/global statement | `STM-<NAME>-nnn` | `^STM-[A-Z]+-\d{3}$` | `STM-TITLE-001` |
| Source | `SRC_<NAME>` | `^SRC_[A-Z_]+$` | `SRC_CLERK_VOICE` |
| Evidence | `EVD-Lnn-<NAME>` | `^EVD-L\d{2}-[A-Z0-9-]+$` | `EVD-L05-CATALOG-2M` |
| Puzzle | `PZL-Lnn-X` | `^PZL-L\d{2}-[A-Z]$` | `PZL-L05-A` |
| Object | `OBJ_Lnn_<NAME>` | `^OBJ_L\d{2}_[A-Z0-9_]+$` | `OBJ_L03_DOOR_C` |
| World change | `CH_Lnn_<NAME>` | `^CH_L\d{2}_[A-Z0-9_]+$` | `CH_L04_COAT_TAKEN` |
| Flag | `Lnn_<snake>` | `^L\d{2}_[a-z0-9_]+$` | `L05_catalog_seen` |
| Checkpoint | `CP_Lnn_X` | `^CP_L\d{2}_[A-Z]$` | `CP_L05_A` |
| Anomaly | `ANOM-<NAME>` | `^ANOM-[A-Z-]+$` | `ANOM-REFLECT-WRONG` |
| Localization key | `Lnn.source.name` | lowercase dotted | `L05.clerk.no_files_floor2` |
| Task | `T-Pn-nn` | `^T-P\d{1,2}-\d{2}$` | `T-P1-03` |
| Deceptive UI instance | `DUI-n` | `^DUI-[1-4]$` | `DUI-1` |
| Cinematic moment / reveal | `CM-nn` / `RV-nn` | `^(CM\|RV)-\d{2}[a-d]?$` | `RV-04` |
| UI tokens | `tok_*`, `type_*`, `mo_*`, `ease_*` | lowercase snake | `tok_brass_deep` |

IDs are **immutable** once merged (changing breaks saves). Retire, don't rename.

## 5. Git

| Item | Convention |
|---|---|
| Branches | `feature/T-P1-03-interaction-ray`, `fix/…`, `art/…`, `audio/…`, `docs/…` |
| Tags | `v0.1.0`, `m2-vertical-slice` |
| Commits | Conventional: `feat(truth): add evaluator [T-P3-02]` |

## 6. Acceptance criteria

- A name validator (`tools/development_tools/validate_names.py`, planned) will run in CI and fail on violations. Until it exists, reviewers enforce these rules.
- No script, scene or ID deviates from these patterns.

## 7. One concept, several names (by context)

| Context | Convention | Example (level 5) |
|---|---|---|
| Design/data ID | `Lnn` | `L05` |
| Folder (scenes, resources, docs, tests) | `lnn_<snake_name>` | `res://scenes/rooms/l05_records_annex/` |
| Scene / data file inside the folder | fixed names | `level.tscn`, `level_data.tres` |
| Class | `PascalCase` | `LevelBase`, `LevelDef`, `LevelService` |
| Flags | `Lnn_<snake>` | `L05_catalog_seen` |
| Other IDs | see §4 | `STM-L05-017`, `PZL-L05-A`, `OBJ_L05_LADDER` |

Do not mix forms (`Room01`, `room_01`, `ROOM_01`): gameplay sequences are **levels** (`Lnn`/`lnn_…`); `rooms/` is only the folder name (DD-003).
