# DESIGN_DECISIONS

> **Status:** ACTIVE — records decisions that resolved conflicts or ambiguities in the documentation set. Game concept, story, mechanics, visual identity and scope are **unchanged**; these decisions only make the specification and repository consistent.

Authority order for contradictions: GAME_VISION → GAME_DESIGN → CORE_MECHANICS → TRUTH_LIE_SYSTEM → PUZZLE_DESIGN → STORY_BIBLE → LEVEL_DESIGN → UI_UX_DESIGN → VISUAL_STYLE_GUIDE → TECHNICAL_ARCHITECTURE → CODE_ARCHITECTURE → PROJECT_STRUCTURE → other documents. For technical implementation paths, **PROJECT_STRUCTURE.md is authoritative**.

---

## DD-001 — One authoritative Godot version

| | |
|---|---|
| **Context** | Documents said "Godot 4.x (pinned)", the CI file used 4.3 and `project.godot` named no version. No version had actually been chosen. |
| **Options** | 4.6.3 (mature, third maintenance release, May 2026) · **4.7.2** (latest stable, second 4.7 maintenance release, 18 Aug 2026) · 4.8 (development snapshots only — rejected). |
| **Chosen** | **Godot 4.7.2 stable**, Forward+ renderer. |
| **Reason** | Latest stable at project start for a multi-year project; already past its first two maintenance releases; avoids an early forced upgrade. 4.8 is not stable. |
| **Impact** | Version appears in README, `game/project.godot` (`config/features` = `4.7`), `.github/workflows/godot.yml`, TECHNICAL_ARCHITECTURE, BUILD_AND_RELEASE, DEVELOPMENT_ROADMAP. Only patch releases (4.7.x) during development; a minor upgrade needs a new decision here and happens at a milestone boundary. **Open check (T-P1-01):** confirm that the chosen test framework (gdUnit4/GUT) and any Steam integration support 4.7.2; if not, record the result here and reconsider 4.6.3. Engine-feature claims in other documents (FSR 2, shader pre-compilation, screen-reader support) remain "verify in the Phase 1 spike". |

## DD-002 — `res://` is `game/`

| | |
|---|---|
| **Context** | The earlier documents described a `res://` tree (`autoload/`, `systems/`, `levels/`, `data/`…) that did not match the repository (`game/scenes`, `game/scripts`…). |
| **Options** | Keep a mapping table · rewrite every reference. |
| **Chosen** | Rewrite the references. PROJECT_STRUCTURE defines three path kinds (repository, `res://`, `user://`); every document uses them consistently. |
| **Reason** | A mapping table forces readers and AI agents to translate paths; correct paths remove the failure mode. |
| **Impact** | All `res://` and folder references in `docs/` were rewritten; the old tree and the mapping table were removed. |

## DD-003 — "Level" is the design term; `scenes/rooms/` holds level scenes

| | |
|---|---|
| **Context** | Design documents use *level* (L01–L20, `LevelService`, `LevelBase`, `level_data.tres`); the repository folder map uses `rooms/`. A level can span 1–4 rooms. |
| **Options** | Rename everything to *room* · rename the folders to `levels/` · keep both terms with defined roles. |
| **Chosen** | Keep both with fixed roles: a **level** is a numbered gameplay sequence; the `rooms/` folders hold level scenes (`res://scenes/rooms/lNN_<name>/level.tscn`) and level data (`res://resources/rooms/lNN_<name>/level_data.tres`). No `RoomManager` exists. |
| **Reason** | The repository folder map was fixed by the project owner; the design vocabulary (L01–L20, IDs, class names) is used in every document. |
| **Impact** | NAMING_CONVENTIONS §7 lists the term → identifier mapping. |

## DD-004 — `GameState` is the single owner of flags and the Change Journal

| | |
|---|---|
| **Context** | CORE_MECHANICS and CODE_ARCHITECTURE described a `WorldState` (flags, Change Journal, `apply()`) while TECHNICAL_ARCHITECTURE described `GameState` (flags, progress, endings): two owners of the same data. |
| **Options** | Two classes with a split · merge. |
| **Chosen** | Merge into the autoload `GameState`, which owns `FlagStore`, `ChangeJournal`, `RngService` and exposes `apply_change(change_id)`. `WorldState` no longer exists. |
| **Reason** | One source of truth simplifies save/load (SAVE_SYSTEM) and the determinism guarantee. |
| **Impact** | `WorldState.apply(x)` → `GameState.apply_change(x)` in CORE_MECHANICS and CODE_ARCHITECTURE; class table updated. |

## DD-005 — Thirteen autoloads; no `LocalizationService`

| | |
|---|---|
| **Context** | TECHNICAL_ARCHITECTURE listed 14 autoloads including a custom `LocalizationService`. |
| **Chosen** | Drop `LocalizationService`; use Godot's built-in `TranslationServer` (`tr()`). Font swaps for the dyslexia-friendly option are handled by the UI theme in `UIRouter`. `Log` is a static utility class, not an autoload. |
| **Reason** | Do not add architecture the engine already provides. |
| **Impact** | TECHNICAL_ARCHITECTURE §3 rewritten with script paths and load order. |

## DD-006 — Autoload scripts live in their domain folders

| | |
|---|---|
| **Context** | The old layout had `autoload/` (facades) plus `systems/` (implementations), neither of which exists in the repository map. |
| **Chosen** | Each autoload script sits in its domain folder under `res://scripts/` next to the classes it delegates to; `core/` holds the cross-cutting ones (EventBus, GameState, Settings, Input, Debug). |
| **Impact** | PROJECT_STRUCTURE §3–§4, TECHNICAL_ARCHITECTURE §3, CODE_ARCHITECTURE file-location table. |

## DD-007 — Custom Resource scripts live with their domain

`StatementDef`/`SourceDef`/`EvidenceDef` → `res://scripts/truth_lie/`; `PuzzleDef` → `res://scripts/puzzle/`; `LevelDef` → `res://scripts/world/`; `ItemDef` → `res://scripts/interaction/`. Their `.tres` instances live in `res://resources/…`. (There was no `core/resources/` folder in the repository map.)

## DD-008 — Engine-dependent tools inside the project, external tools in `tools/`

| | |
|---|---|
| **Context** | Documents placed `truth_lint.gd`, `prewarm_list.gd`, `validate_data.gd` in `tools/`. Scripts that load `.tres` resources need `res://`, which only resolves inside the Godot project. |
| **Chosen** | Headless/engine tools → `res://scripts/utilities/lint/` (run with `godot --headless --path game --script res://…`). External Python/shell tools → `tools/<area>/` (`truth_import.py` in `dialogue_tools/`, `validate_assets.py` in `asset_tools/`, `validate_names.py` in `development_tools/`). All are *planned*. |

## DD-009 — Two test locations with different purposes

Executable in-engine tests need `res://`, so they live in `game/tests/`. The root `tests/` folders (kept from the repository map) hold tests for external tools, CI harness scripts, per-level playthrough plans and bug-regression records. Fixture saves live in `game/tests/fixtures/saves/`.

## DD-010 — Source art/audio are not in the repository map

| | |
|---|---|
| **Context** | GIT_WORKFLOW and ASSET_PIPELINE assumed root folders `art_source/` and `audio_source/`; the repository map has none, and putting `.blend`/`.psd` inside `game/` makes Godot import them. |
| **Chosen** | Source files stay outside `game/`. If the team wants them versioned, add root-level `art_source/` and `audio_source/` with Git LFS and record it here. `game/assets/` holds game-ready files only. |

## DD-011 — Signals are declared once, on `EventBus`

`TruthService` previously declared its own `statement_delivered`/`statement_reinterpreted` signals while the same events were on `EventBus`. Cross-system signals now exist only on `EventBus`; services emit through it.

## DD-012 — License is pending; placeholder owner

`LICENSE` carries `[PROJECT OWNER]` and "LICENSE DECISION PENDING". No open-source licence was chosen. Replace both before making the repository public.

## DD-013 — `ASSETS_LICENSES.csv` schema

Columns: `Asset, Category, Source, Author, License, License URL, Usage Rights, Attribution Required, Attribution Text, Notes`. `Asset` holds the repository-relative path; `Notes` holds acquisition date and the proof file reference. The file contains headers only until assets exist.

## DD-014 — Input actions are created in the editor, not hand-written into `project.godot`

The InputMap actions of INPUT_SYSTEM §1 are added by task T-P1-05 using the editor (the serialized event format is error-prone to write by hand). `project.godot` remains a minimal starter.

## DD-015 — Documents carry an implementation status

Each document starts with a status line: **DESIGNED** (specification only), **PLANNED** (tooling/process not yet built), or **ACTIVE** (governs the repository now). Statements such as "CI fails if…" in DESIGNED/PLANNED documents are target behavior, not existing behavior. The only implemented items are listed in README.md ("Current status").

## DD-016 — UI/visual review amendments folded into the body

UI_UX_DESIGN, VISUAL_STYLE_GUIDE and ANIMATION_GUIDELINES contained "review amendments" sections that "supersede earlier text". Conflicting earlier text (main-menu row, Ledger tilt/quad, brass focus outline, menu focus timing) was corrected in place so no document contradicts another. Authoritative numeric detail: UI_COMPONENT_SPEC.

## DD-017 — Small cross-document corrections

| Conflict | Resolution |
|---|---|
| CORE_MECHANICS defines the verb **Ask**; GAME_DESIGN's verb table and INPUT_SYSTEM did not | Ask = `interact` on an intercom or brass bell; added to GAME_DESIGN §3 (no new input action) |
| CINEMATIC_DIRECTION said "no cinematic removes control > 3 s except CM-04" while CM-02/CM-03 lock movement for tens of seconds | Wording corrected: look is always retained (±35° yaw in CM-02/03); movement is locked only in CM-02/03/04 |
| Lint rules referenced as R1–R12 after R13/R14 were added | All references now R1–R14 |
| `T-P1-03` called `Log` an autoload | `Log` is a static utility class (DD-005) |
| Brass `#B08D4A` specified as the focus outline on paper (≈ 2.4:1 contrast) | On paper: `brass_deep` `#7A6233`; in-world: polished brass |

## Open items (not design conflicts)

| ID | Item |
|---|---|
| Q-001 | Team size/schedule assumptions in DEVELOPMENT_ROADMAP §1 |
| Q-002 | VO language scope and actor availability |
| Q-003 | Steam as primary distribution; Steamworks integration scope |
| Q-005 | Content-warning wording and age-rating strategy |
| Q-006 | Test framework (gdUnit4 vs GUT) — decided in T-P1-01 after compatibility check with 4.7.2 |
| Q-007 | Final licence and legal owner (DD-012) |
