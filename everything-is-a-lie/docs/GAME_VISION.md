# GAME_VISION

> **Status:** DESIGNED — specification only; not yet implemented in `game/`.

## 1. Vision statement

*Everything Is A Lie* is a first-person psychological puzzle game in which the game itself is an unreliable narrator that you must learn to read — and in which learning to read it is the story.

## 2. Player fantasy

"I am a careful, observant person in a place that is lying to me politely. I get *better* at telling how it lies. At the end I understand why."

## 3. Experience goals

| Moment | Target feeling | Delivered by |
|---|---|---|
| First 10 minutes | Safe, curious, slightly courted | Truthful Clerk, warm light, tutorial by environment |
| First anomaly (L02–L03) | "Wait… did that just change?" | Omission, sound/visual mismatch |
| Mid-game | Productive paranoia | Conflicting sources, temporal truths, Ledger |
| Revelation (L15–L17) | Vertigo, then clarity | False resolution + true statement when a lie is expected |
| Ending | Moral weight, not shock | Choice with ambiguous consequences |

## 3b. Tone

Quiet, courteous, bureaucratic dread. Think *polite institution* rather than *haunted house*. The Clerk never raises its voice.

## 4. Pillars

1. **Fair deception.** Every lie has a path to discovery before it matters (TRUTH_LIE_SYSTEM §9).
2. **The world speaks.** No quest log HUD. Objectives are inferred from environment and sources.
3. **Doubt is a skill.** The player has verbs for testing claims.
4. **The building knows you.** Adaptation is bounded, narratively grounded, and optional to disable.
5. **Quiet dread.** Uncertainty over scares.
6. **Controlled fourth wall.** Meta moments are authored, finite, and never touch the player's real system.

## 5. Reference touchstones (inspiration, not imitation)

| Reference | What we take | What we avoid |
|---|---|---|
| The Stanley Parable | Narrator as antagonist/ally | Comedy-first tone |
| Return of the Obra Dinn | Deduction with confirmable evidence | Pure ledger-only play |
| Papers, Please | Bureaucracy as moral pressure | Resource-management loop |
| Antichamber / Superliminal | Perception-based spatial puzzles | Abstract aesthetics |
| Inscryption | Meta-structure that pays off | Real-system fourth-wall breaks |
| P.T. | Loop dread, domestic spaces | Jump-scare reliance |

## 6. Audience

- Players of narrative puzzle / walking-sim / deduction games, ages 16+ (themes, not graphic content).
- Streamer-friendly: **Fixed Experience** mode (adaptation off) for fair comparisons and spoiler-safe runs.

## 7. Scope

| Metric | Target |
|---|---|
| Playtime (first run) | 6–8 hours |
| Levels | 20 across 5 acts (cut line → 18) |
| Average level length | 15–25 min |
| Replay | 3 endings + 1 secret; "Second Reading" New Game+ |
| Platform | Windows PC (controller-ready) |
| Team assumption | 3–5 people (design/dev lead, 1–2 artists, audio, part-time writer); solo = ×2 timeline |

## 8. Non-goals

- Not a text game, visual novel, or browser-style prototype.
- No combat, no stealth chases, no inventory management.
- No procedural levels (puzzles are authored and testable).
- No online features, telemetry, or accounts.

## 9. Success criteria

| Criterion | Measure |
|---|---|
| Fairness | ≥ 80% of playtesters can state the evidence behind each flagship deception after reveal |
| Immersion | ≤ 5% of playtesters report UI "felt like a web app/overlay" |
| Tension | Playtest self-report: ≥ 70% "I second-guessed ordinary objects" by L10 |
| Performance | 60 FPS, no frame > 33 ms in 99.9% on reference rig |
| Completion | ≥ 60% of internal playtesters reach L15 |

## 10. Title as statement

"EVERYTHING IS A LIE" is itself a **statement in the Truth system** (`STM-TITLE-001`). The game ultimately evaluates it (CONTEXTUAL / DECEPTIVE — see STORY_BIBLE). It is the Clerk's thesis and its deepest excuse.
