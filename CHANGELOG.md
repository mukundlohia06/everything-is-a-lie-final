# CHANGELOG

All notable changes to this project and its documentation. Format follows *Keep a Changelog*; versions follow SemVer (`0.x` before release). Documentation-only changes are logged under `docs`.

## [Unreleased]

### Added
- (empty)

## [0.0.3] — Pre-implementation repository audit — 2026-11-19

### Added
- `docs/DESIGN_DECISIONS.md` (DD-001 … DD-017: every conflict resolved in this audit).
- `ASSETS_LICENSES.csv` (headers only), `game/README.md`, `tools/README.md`, `tests/README.md`.
- A status line (DESIGNED / PLANNED / ACTIVE) at the top of every document.

### Changed
- **Godot version pinned to 4.7.2 stable** in README, `game/project.godot` (`config/features`), CI, TECHNICAL_ARCHITECTURE, BUILD_AND_RELEASE, DEVELOPMENT_ROADMAP, TASK_BREAKDOWN.
- **PROJECT_STRUCTURE rewritten** for the actual repository; `res://` = `game/`. Old `res://` tree and mapping table removed; stale paths rewritten in LEVEL_DESIGN, QA_TESTING, SAVE_SYSTEM, ASSET_PIPELINE, VFX_GUIDELINES, VISUAL_POLISH_CHECKLIST, GIT_WORKFLOW, CODE_ARCHITECTURE, TRUTH_LIE_SYSTEM.
- Architecture made consistent: `WorldState` merged into `GameState`; 13 autoloads with script paths and load order; `LocalizationService` replaced by `TranslationServer`; signals declared once on `EventBus`; autoload scripts live in domain folders; engine-dependent lint tools in `res://scripts/utilities/lint/`, external tools in `tools/`; test locations clarified.
- UI/visual documents consolidated: review amendments folded into the body of UI_UX_DESIGN, VISUAL_STYLE_GUIDE and ANIMATION_GUIDELINES; brass focus color, menu focus timing and camera-control wording corrected.
- Lint rule references updated to R1–R14.
- CI workflow rewritten (Godot 4.7.2, import check only); `.gitignore`, `.gitattributes`, `LICENSE` (now `[PROJECT OWNER]`, decision pending) and root README rewritten.

### Removed
- `.godot-version` references, `.mono/` and `*.translation` ignore rules, the `|| true` import-check workaround, the `art_source/`/`audio_source/` assumption from the repository tree.

### Open questions
- Q-006 test framework choice after compatibility check with 4.7.2 (T-P1-01); Q-007 final licence and legal owner. Others: see DESIGN_DECISIONS.

## [0.0.2] — UI/UX and visual quality review — 2026-11-14

### Added (docs)
- UI_UX_REVIEW, UI_COMPONENT_SPEC, CINEMATIC_DIRECTION, VISUAL_POLISH_CHECKLIST.
- TRUTH_LIE_SYSTEM rules R13 (style parity) and R14 (deceptive UI budget); QA lint rows.
- Tasks T-P1-17/18, T-P2-21..24; milestone art gates.

### Changed (docs)
- VISUAL_STYLE_GUIDE: type scale pointer, vignette row clarified, color-language amendments.
- UI_UX_DESIGN §10, ANIMATION_GUIDELINES §9, LEVEL_DESIGN §6 amendments.

### Decisions
- **D-011** False information has no dedicated color/style; truthful and deceptive carriers are styled identically (R13).
- **D-012** Bright brass is reserved for interactables; decorative brass is patinated.
- **D-013** Brass never used for text/focus on paper (contrast); `brass_deep` used instead.
- **D-014** Main menu is six distinct desk objects; pause is a tabbed Folder; settings is a binder.
- **D-015** No screen-space glitch effects; distortion is physical with per-level budget.
- **D-016** Deceptive UI limited to DUI-1..4; never in system UI.
- **D-017** Ledger final state is pixel-aligned 1:1.
- **D-018** No toasts/banners; four-priority notification system, max one visible.

## [0.0.1] — Pre-production documentation v1.0 — 2026-11-14

### Added (docs)
- Full documentation set (33 files): vision, design, mechanics, story, world, levels (L01–L20), puzzles, Truth/Lie system (schema v1, rules R1–R12), behavior system, UI/UX, visual/art/animation/VFX/audio direction, technical and code architecture, project structure, save/input/settings, accessibility, performance, QA, asset pipeline, naming, Git workflow, roadmap, milestones, task breakdown, build & release.
- Decision log (see below).

### Decisions
- **D-001** Engine: Godot 4.x (pinned at Phase 1), Forward+ renderer, typed GDScript. *(Superseded by DD-001: Godot 4.7.2.)*
- **D-002** First-person 3D, no jump/sprint/combat; single carry slot; no HUD objectives.
- **D-003** Truth schema v1 frozen: categories TRUTH/FALSE/MISLEADING/INCOMPLETE/CONTEXTUAL/TEMPORAL/TECHNICAL/DECEPTIVE/UNVERIFIABLE.
- **D-004** Fair-play rules R1–R12 are lint-enforced; real UI never lies (R9).
- **D-005** Behavior system is local-only, bounded, deterministic, and optional (Fixed Experience).
- **D-006** Fourth-wall policy: authored, finite, never touches the real system (GAME_DESIGN §9).
- **D-007** Saves live in `user://` (moved out of `res://` from the master prompt's example).
- **D-008** Phase 2 ships a Truth *stub*; Phase 3 formalizes (deliberate deviation, README §4).
- **D-009** AI-generated assets not used in shipped content.
- **D-010** Level count 20, cut-line to 18 (README §9).

### Open questions (to resolve before M0 sign-off)
- Q-001 Confirm team size/schedule assumptions in DEVELOPMENT_ROADMAP §1.
- Q-002 Confirm VO language scope and actor availability.
- Q-003 Confirm Steam as primary distribution and Steamworks integration scope.
- Q-004 Confirm final Godot 4.x version and whether FSR 2.x scaling is available in it. *(Version resolved by DD-001; FSR availability is checked in the Phase 1 spike.)*
- Q-005 Confirm content-warning wording and age-rating strategy.
