# BUILD_AND_RELEASE

> **Status:** PLANNED — the tooling/process described here does not exist yet.

## 1. Targets

| Platform | Status |
|---|---|
| Windows x86_64 | **Primary** (Godot export, PCK embedded or side-by-side) |
| Linux / Steam Deck | Later (design keeps controller + 1280×800 safe UI) |
| macOS | Not planned |

## 2. Export configuration

| Item | Setting |
|---|---|
| Presets | `Windows Debug`, `Windows Release` |
| Renderer | Forward+ (Vulkan) |
| Texture compression | S3TC/BPTC |
| Features | `release`, `steam` (tag only if Steam build) |
| Exclusions | `res://tests/*`, debug-only scripts and overlays. Docs, tools, tests, source art live outside `game/` and are never exported |
| Encryption | PCK encryption **off** (no anti-cheat need; modding unsupported) |
| Icon/Metadata | Company, version, copyright set in the export preset (`game/export_presets.cfg`, planned) |
| Console | No console window in release |

## 3. Versioning

SemVer. The version is `application/config/version` in `game/project.godot` (read through `ProjectSettings` by Settings/About); it is set when the first playable build exists. Save `game_version` records it.
- `0.x.y` development builds; `0.9.x` RC; `1.0.0` release.
- Build number appended in non-release builds (`0.3.2+build.145`).

## 4. CI/CD

```mermaid
flowchart LR
  T[Tag vX.Y.Z] --> C[CI: lint+tests+truth lint]
  C --> E[Headless export Windows]
  E --> S[Sign executable]
  S --> A[Artifact + checksums]
  A --> U[Upload to Steam depot / itch (manual approval)]
```

- Nightly: unsigned export with debug overlay for playtesters.
- Release export machine: Godot 4.7.2 + the matching export templates.

**Current CI status:** `.github/workflows/godot.yml` runs a project import check only. Test, Truth lint and Windows export jobs are described in this document but **not implemented yet**; they are enabled by the tasks listed in the workflow file's comments.

## 5. Distribution

| Channel | Notes |
|---|---|
| GitHub Releases | Distributable builds (zipped export + checksums) are published as release assets. `builds/` is a local, git-ignored scratch folder; binaries are never committed |
| Steam (primary) | Steamworks SDK optional (GodotSteam) for achievements/cloud saves; cloud path `saves/` + `settings.cfg` |
| itch.io | Demo/vertical slice and fallback |
| Press/playtest builds | Itch restricted or Steam keys |

Achievements (optional, never reveal spoilers): one per ending, one for Notices, one for completing in Fixed Experience, etc. Achievement names avoid spoilers.

## 6. Release checklist

**Content & design**
- [ ] All levels/endings reachable; truth lint clean; placeholders (`TMP_`) = 0
- [ ] Content warning on title screen and store pages
- [ ] Credits complete; license attributions bundled (`licenses.txt`)

**Technical**
- [ ] Reference rig + minimum rig performance verified
- [ ] Save migration tests pass for all shipped versions
- [ ] Fresh-install + upgrade-install tested; settings defaults ok
- [ ] No debug overlays, console, or test data in build
- [ ] Logs write to `user://logs/` only; no telemetry/network calls (verified with network monitor)
- [ ] Executable signed; antivirus false-positive check

**Quality**
- [ ] S0–S2 = 0; S3 ≤ 10 documented
- [ ] Photosensitivity analysis report archived
- [ ] Accessibility checklist (ACCESSIBILITY §7) signed
- [ ] Controller-only and keyboard-only completion verified

**Legal/Store**
- [ ] Asset license ledger audited (all rows have proof)
- [ ] Fonts/music/SFX licenses confirmed for commercial distribution
- [ ] Age rating questionnaires submitted
- [ ] Store page: trailer (no misleading mechanic claims), screenshots, system requirements (from PERFORMANCE_REQUIREMENTS)

## 7. Post-release

- Day-one patch branch `release/1.0` kept open for 4 weeks.
- Crash/issue intake via email/Steam forum; logs attached by player manually (no auto-upload).
- Patch cadence: hotfix ≤ 72 h for S0/S1; roll-up patch at 2 and 6 weeks.

## 8. Acceptance criteria

- A tagged release builds reproducibly from CI with no manual steps except signing/upload approval.
- Release checklist stored with the tag.
