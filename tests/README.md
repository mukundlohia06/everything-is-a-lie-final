# tests/ (repository level)

**Nothing here is implemented yet.** Executable in-engine tests (gdUnit4/GUT) live in the Godot project at `game/tests/` (`res://tests/`) because they need `res://`. This folder holds everything that is not run inside the engine:

| Folder | Contents |
|---|---|
| `unit/` | Tests for the external Python tools in `tools/` |
| `integration/` | CI/headless harness scripts that launch Godot and run the in-engine tests |
| `gameplay/` | Manual and scripted playthrough plans, one per level (`lNN_<name>.md`) |
| `regression/` | One record per S0–S2 bug: repro steps, save reference, test that covers it |

See `../docs/QA_TESTING.md` and `../docs/PROJECT_STRUCTURE.md` §6.
