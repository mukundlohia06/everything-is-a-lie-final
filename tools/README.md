# tools/

External tooling that runs **outside** the Godot editor (Python/shell scripts). **Nothing here is implemented yet**; folders are placeholders.

| Folder | Planned contents |
|---|---|
| `asset_tools/` | Asset/license validators, placeholder report, Blender batch-export helpers |
| `dialogue_tools/` | Statement CSV → `.tres` importer (`truth_import.py`) |
| `level_tools/` | Level/room data checkers and generators |
| `development_tools/` | Name validator, repository checks |

Tools that need the Godot engine (e.g. the Truth/Lie lint) live inside the project under `res://scripts/utilities/lint/`, not here. See `../docs/PROJECT_STRUCTURE.md` §6.
