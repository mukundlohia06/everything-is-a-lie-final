# QA_TESTING

> **Status:** PLANNED — the tooling/process described here does not exist yet.

## 1. Goals

1. The game is stable, performant and accessible.
2. **The deception is fair** — the player feels "that was clever", never "the developer cheated".
3. Save/load and adaptation never corrupt the deception.

## 2. Test layers

| Layer | Tooling | Scope |
|---|---|---|
| Unit | gdUnit4/GUT | Truth evaluator, evidence graph, profile EMA, save serializer, migrations |
| Data lint | `res://scripts/utilities/lint/truth_lint.gd` (headless; planned) | R1–R14, ID integrity, loc keys, orphan evidence |
| Integration | Headless scene tests | Level load, carriers deliver, puzzle completes with scripted inputs |
| Replay/determinism | Recorded input playback | Same save + inputs → same adaptations/outputs |
| Soak | 2 h automated walk | Memory, hitching |
| Manual QA | Test plans per level | Feel, readability, bugs |
| Playtests | Moderated + unmoderated | Fairness, clarity, tension |
| Accessibility | See ACCESSIBILITY §6 | |
| Performance | See PERFORMANCE_REQUIREMENTS §6 | |

### Test locations

Executable tests and fixtures live in the Godot project: `game/tests/` (`res://tests/`). The repository-level `tests/` folders hold tests for the external tools (`tests/unit/`), CI harness scripts (`tests/integration/`), per-level playthrough plans (`tests/gameplay/lNN_<name>.md`) and bug-regression records (`tests/regression/`). See PROJECT_STRUCTURE §6. None of these exist yet.

## 3. Test matrix

| Area | Cases (examples) | Owner | Phase |
|---|---|---|---|
| Puzzles | Solve via intended path; alternate paths; wrong attempts recover ≤ 90 s | QA/Design | P2+ |
| Progression | Level order, flags, no soft-locks (LevelGuard), sequence breaks | QA | P2+ |
| Save/load | Save at each checkpoint; load; random-point saves; corrupt file; `.bak` recovery; migration fixtures | Eng/QA | P3+ |
| UI | All screens by KB/M + controller; text scale; focus order | QA | P2+ |
| Audio | Levels, ducking, positional correctness, caption accuracy (R12) | Audio/QA | P7 |
| Performance | Fly-through captures per level; presets | Eng | P8 |
| Resolution scaling | 720p–4K, ultrawide 21:9 (letterbox UI safe areas), 16:10, windowed | QA | P6+ |
| Controller | Xbox, PS, generic; hotplug; glyphs; rebinding | QA | P2+ |
| Accessibility | Reduced Motion/Flashing; colorblind; captions; toggles | QA/Access | P6+ |
| Unintended solutions | Exploit hunting: physics throwing carried objects, clipping | QA | P5+ |
| Sequence breaking | Skip statements, enter areas early, leave mid-puzzle | QA | P5+ |
| Contradictions | Authored vs. unintended contradictions (Change Journal audit) | Design/QA | P3+ |
| Behavior tracking | Determinism, clamps, cooldowns, Fixed Experience, tracking off | Eng/QA | P4+ |
| False information | Lint + inversion solver + human review | Design/QA | P3+ |

## 4. Automated lint (Truth)

| Check | Pass condition |
|---|---|
| R1 Verification paths | Every progress-critical non-TRUTH statement has ≥ 2 reachable evidence nodes before gate |
| R4 Inversion solver | Level solver using inverted truth values **fails** to complete level |
| R5 Cost bound | Sum of `consequence` times along any false-guidance branch ≤ 90 s (design estimate; playtest confirms) |
| R7 Mutation visibility | Every mutation has a visible/audible world event in the Change Journal |
| R8 Channel parity | Each evidence node has non-color + non-audio channel or assist mapping |
| R10 Truthfulness | Clerk true-share ≥ 50% overall |
| Reinterpretation | Each `reinterprets` pair passes literal-consistency check |
| R13 Style parity | Carriers of one source share identical style tokens (automated style-token diff) |
| R14 Deceptive UI | Only DUI-1..4 present; none under system UI scenes |
| Visual polish | VISUAL_POLISH_CHECKLIST gate score at M1/M2/M6/M8 |

## 5. Bug severity

| Sev | Definition | Example | Response |
|---|---|---|---|
| S0 | Crash, data loss, soft-lock | Save corrupts | Block build |
| S1 | Progression blocker with workaround | Door fails to open on reload | Fix before milestone |
| S2 | Wrong or unfair deception | Evidence node unreachable | Fix before milestone |
| S3 | Visual/audio glitch not affecting play | Z-fighting | Schedule |
| S4 | Polish | Typo | Backlog |

## 6. Fairness metrics (playtest)

| Metric | Target | Method |
|---|---|---|
| Articulation rate | ≥ 80% of testers can state the evidence behind a flagship deception after reveal | Post-puzzle interview |
| "Cheated" reports | ≤ 10% per deception | 5-point scale: "Was this fair?" ≥ 4 for ≥ 80% |
| "Guessing" reports | ≤ 15% per puzzle | Think-aloud coding |
| Time-to-solve | Median within ±30% of design estimate | Telemetry (local, playtest builds only, consent) |
| Stuck > 5 min without any nudge progress | ≤ 15% | Observer log |
| Tension | ≥ 70% self-report "I second-guessed ordinary objects" by L10 | Survey |
| UI immersion | ≤ 5% say "UI felt like an overlay" | Survey |

Playtest builds may log locally with explicit consent; release builds contain **no telemetry**.

## 7. Playtest plan

| Stage | Participants | Build | Focus |
|---|---|---|---|
| Alpha-1 (M1) | 3 internal | Greybox | Interaction feel |
| Alpha-2 (M2) | 6 external | Vertical slice | L01–L04 fairness, readability |
| Beta-1 (M5) | 10 | Content complete | Full flow, difficulty curve |
| Beta-2 (M6/7) | 10 | Polished | Tension, accessibility, performance |
| RC (M8) | 15 | RC | Regression, ending coverage |

## 8. Regression policy

- Every S0–S2 bug gets an automated test or a documented manual regression case.
- Nightly CI: unit + lint + integration + 20-min soak.
- Release gates in BUILD_AND_RELEASE §6.

## 9. Test data and tools

- Fixture saves per level (checkpoint states) in `game/tests/fixtures/saves/` (`res://tests/fixtures/saves/`).
- Debug commands to jump to levels and set flags.
- Bug template: build, level, steps, expected/actual, save file attached, log attached.

## 10. Acceptance criteria

- All S0–S2 closed at RC; ≤ 10 open S3.
- Lint report clean (0 errors) at each milestone.
- Determinism test passes 100/100 runs.
