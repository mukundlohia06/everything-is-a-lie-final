# Everything Is a Lie

A first-person psychological puzzle game for Windows PC. You are a *Witness* inside the Concord, a Hall of Records. A courteous voice, the Clerk, guides you through the building. Some of what it says is true, some is false, incomplete, technically true, true only for a while, or true in a frame you haven't noticed. The game also watches how you play. You progress by learning to question the game itself, not by inverting what it tells you.

> **Content note:** the story involves institutional negligence, a fatal fire, guilt and grief. No gore; no deaths are depicted.

## Current status

**PRE-PRODUCTION / INITIAL IMPLEMENTATION SETUP.** The design and technical specification is complete; implementation has not started.

| State | What |
|---|---|
| **IMPLEMENTED** | Repository skeleton; `game/project.godot` (minimal starter); `.gitignore`, `.gitattributes`; CI workflow `godot.yml` (project import check only); issue and PR templates; `ASSETS_LICENSES.csv` (headers only); `LICENSE` (placeholder) |
| **DESIGNED** (specification only) | Everything in `docs/`: mechanics, Truth/Lie system, behavior system, 20 levels, puzzles, UI/UX, visual and audio direction, architecture, save/input/settings, accessibility, performance, QA |
| **PLANNED** | Test framework, lint/validation tools, export presets, Steam integration, all gameplay code and content |
| **NOT YET IMPLEMENTED** | Player, interaction, Truth/Lie system, puzzles, UI, levels, audio, save system — all of it |

**Current milestone:** M0 (pre-production complete) → next **M1: interaction prototype** (`docs/MILESTONES.md`, tasks `T-P1-xx` in `docs/TASK_BREAKDOWN.md`).

## Engine

**Godot 4.7.2 (stable)**, Forward+ renderer, typed GDScript. This is the single authoritative version (`docs/DESIGN_DECISIONS.md` DD-001). Windows PC, 1920×1080, 60 FPS target.

## Repository structure

| Path | Contents |
|---|---|
| `docs/` | Design and technical specification — **start at `docs/README.md`** |
| `game/` | The Godot project. **`res://` = `game/`** |
| `tools/` | External tooling (Python/shell) — placeholders |
| `tests/` | Repository-level tests and test plans — placeholders |
| `builds/` | Local export output (git-ignored) |
| `.github/` | CI workflow, issue templates, PR template |
| `ASSETS_LICENSES.csv` | Third-party asset license register |
| `CHANGELOG.md` | Change history |

Full layout and path conventions: `docs/PROJECT_STRUCTURE.md`.

## Opening the project

1. Install **Git**, **Git LFS** (`git lfs install`, run before the first commit that adds binary assets) and **Godot 4.7.2**.
2. Clone the repository, then `git lfs pull`.
3. Open **`game/project.godot`** in Godot 4.7.2. (The project has no main scene yet; this is expected.)

## Documentation

Read in this order: `docs/GAME_VISION.md` → `GAME_DESIGN.md` → `CORE_MECHANICS.md` → `TRUTH_LIE_SYSTEM.md` → `TECHNICAL_ARCHITECTURE.md` → `PROJECT_STRUCTURE.md`. Every document begins with a status line (DESIGNED / PLANNED / ACTIVE). Decisions that resolved conflicts: `docs/DESIGN_DECISIONS.md`.

## Development workflow

- Work from `docs/TASK_BREAKDOWN.md`: **one task ID per branch and pull request**.
- Branches, commits and PRs: `docs/GIT_WORKFLOW.md`. Names and IDs: `docs/NAMING_CONVENTIONS.md`.
- Do not implement mechanics that are not in `docs/CORE_MECHANICS.md`; propose changes through a `docs/` pull request.
- Gameplay content (statements, puzzles, dialogue) is data in `res://resources/`, never hard-coded.
- Game code must read/write only `res://` and `user://`.

## License

**License decision pending.** `LICENSE` is a placeholder (`[PROJECT OWNER]`, all rights reserved). Replace both before making the repository public. Third-party assets must be listed in `ASSETS_LICENSES.csv` before they are committed.
