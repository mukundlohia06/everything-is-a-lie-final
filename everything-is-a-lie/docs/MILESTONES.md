# MILESTONES

> **Status:** ACTIVE (planning document).

Each milestone is a **gate**: all exit criteria must pass before the next phase's dependent work begins.

| ID | Name | Phase | Target (relative) |
|---|---|---|---|
| M0 | Pre-production complete | P0 | Week 0 |
| M1 | Interaction prototype | P1 | W6 |
| M2 | Vertical slice | P2 | W20 |
| M3 | Truth/Lie system complete | P3 | W18 |
| M4 | Behavior system complete | P4 | W24 |
| M5 | Content complete (L01–L20 playable) | P5 | W56 |
| M6 | Visual + Audio polish complete | P6/P7 | W66 |
| M7 | Optimization complete | P8 | W72 |
| M8 | Release candidate | P9 | W80 |
| M9 | Gold / Release | P10 | W84 |

(Weeks are from the start of Phase 1; assumes team in DEVELOPMENT_ROADMAP §1.)

## M0 — Pre-production complete
- 12 mandatory docs signed off, no TBD.
- 25 sample statements pass lint (on paper).
- L01–L04 specs and puzzle chains complete.
- Look-dev targets approved; technical spike plan agreed.
- License ledger created; risks owned.

## M1 — Interaction prototype
- Player rig, interaction, examine/use/carry operate at 60 FPS in a greybox.
- Input service with KB/M + controller.
- Ledger stub opens; save stub round-trips player position and one flag.
- Spikes S1–S5 reports done.
- Internal playtest (3 testers): "movement and interaction feel good" ≥ 4/5.
- UI motion spike and look-dev keyframes approved; Visual Polish gate ≥ 60%, zero Critical failures on A6/B3.

## M2 — Vertical slice (L01–L04)
- Four levels at final-quality art/audio/UI, fully playable start to finish.
- Real lies present: omission (L02), mismatch (L03), conflict (L04).
- Main menu, Folder, Settings (video/audio/controls), Accessibility v1, captions.
- 60 FPS on reference rig; all levels within budgets.
- External playtest (6 testers): fairness ≥ 4/5 for ≥ 80%; articulation ≥ 80% on L02–L04 deceptions.
- Visual Polish gate ≥ 85% with zero Critical failures; red-flag list empty in the review sample.
- Component state sheets and notification system complete; token linter clean.
- Decision gate: *greenlight full production / adjust scope.*

## M3 — Truth/Lie system complete
- Schema v1 frozen; evaluator, evidence graph, mutation events, reinterpretation chains.
- Lint tool R1–R14 working in CI; Change Journal operational.
- CSV→`.tres` pipeline; authoring doc for writers.
- Save integration: deception-safe save tests pass.

## M4 — Behavior system complete
- Profile, EMA, planner, cooldowns, adaptation catalog implemented.
- Fixed/Adaptive/Tracking-off modes verified.
- Determinism test: 100/100 identical replays.
- L10 functional with canned and real data.

## M5 — Content complete
- L01–L20 playable start to finish, all puzzles solvable, all endings reachable.
- Lint clean; no S0/S1 bugs open; ≤ 15 S2.
- Placeholders allowed in art/audio (tracked).
- Playtest Beta-1 (10 testers): median completion time 6–8 h; ≥ 60% reach L15.

## M6 — Polish complete
- Lighting/VFX/UI transitions final; Reduced Motion/Flashing variants done.
- VO recorded and integrated; music and dynamic system final; mix passed.
- Accessibility playtests (≥ 6 testers) completed; issues triaged.
- Placeholders ≤ 10% of assets.
- Visual Polish gate ≥ 90%; flash analysis and colorblind sheets archived; all CM/RV animatics implemented.

## M7 — Optimization complete
- PERFORMANCE_REQUIREMENTS §8 acceptance passed.
- Hitch audit clean (p99.9 ≤ 33 ms).
- Presets validated on minimum rig.

## M8 — Release candidate
- 0 S0–S2 open; ≤ 10 S3.
- Photosensitivity analysis passed; flash-risk sequences cleared.
- All placeholders (`TMP_`) removed; license ledger complete and audited.
- Save migration tests pass for all shipped versions.
- RC playtest (15 testers) with no new S0–S2.

## M9 — Gold
- Release checklist (BUILD_AND_RELEASE §6) complete.
- Store pages live; content warnings present.
- Day-one patch plan and support channel ready.

## Scope review triggers

Slip > 2 weeks on any milestone → review README §9 cut-lines. Slip > 6 weeks cumulative → formal scope reduction (20 → 18 levels).
