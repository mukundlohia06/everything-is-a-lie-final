# GAME_DESIGN

> **Status:** DESIGNED — specification only; not yet implemented in `game/`.

## 1. Format

- **Perspective:** first-person 3D, walking pace, no jump, no sprint (a "brisk walk" toggle at 1.4×).
- **Structure:** 20 levels in 5 acts, connected by diegetic transitions (doors, lifts, stairs, tube capsules). No hard loading screens.
- **Spaces:** each level is a self-contained scene of 1–4 rooms (≈ 150–400 m² walkable). The Concord is one coherent building; levels are *wings*.

## 2. Acts

| Act | Levels | Theme | Deception ladder |
|---|---|---|---|
| I — Courtesy | L01–L04 | Establish trust | Truth → omission → mismatch → conflicting source |
| II — Contradiction | L05–L09 | Learn the language | Technically true, temporal, framing, honest-looking-wrong |
| III — Pressure | L10–L14 | The building adapts | Behavior mirror, self-audit, character testimony, UI lies (in-world), audio-only logic |
| IV — Proceedings | L15–L17 | Case is heard | Cross-examination, evidence reconstruction, false resolution |
| V — Testimony | L18–L20 | The original lie | Vault, Clerk revealed, verdict |

## 3. Player verbs

| Verb | Input (KB/M) | Result |
|---|---|---|
| Move / Look | WASD / Mouse | — |
| Examine | E (tap) | Zoom focus on object; reveals detail layers (text, wear, labels) |
| Use / Operate | E (hold 0.3 s on mechanisms) or LMB | Switches, doors, levers |
| Pick up / Carry | E on carriable | One carried item at a time; drop with Q |
| Listen | Hold C | Narrow-band focus: boosts positional cues, dims music; not a wallhack |
| Wait | Hold Z (visible hourglass-less diegetic cue: wristwatch) | Fast-forward 5× local time only inside puzzle volumes that allow it |
| Ledger | Tab | Opens the Ledger (diegetic notebook) |
| Annotate | In Ledger | Mark statements Trust / Doubt, link evidence |
| Pause | Esc | The Clerk's Folder (pause menu) |
| Ask | `interact` on an intercom or brass bell | Elicits a rephrased statement (CORE_MECHANICS §4); no separate input |
| Cross-check / Test / Compare | Ledger links · `interact`/probe on objects · sequential `examine` | Verification tools of CORE_MECHANICS §4; no separate inputs |

*No jump, crouch or combat.* Complexity belongs in observation, not execution.

## 4. Progression

- **Linear acts, branching rooms.** Within a level, players can often tackle sub-puzzles in any order.
- **Gates are knowledge, not items.** A gate opens when the player *demonstrates understanding* (e.g., sets lamps to match a pattern they inferred), not when they hold a key.
- **Carry items** exist but are physical, single-slot, and used for in-world logic (marble, pencil, UV lamp).

## 5. Difficulty model

Difficulty rises through **ambiguity, not execution**:

| Axis | L01 | L10 | L20 |
|---|---|---|---|
| Source count in a puzzle | 1 | 3 | 4–5 |
| Share of critical statements that are non-true | 0% | ≤ 35% | ≤ 40% |
| Evidence redundancy needed | 1 | 2 | 2–3 |
| Time pressure | none | rare, soft | none |
| Reinterpretation of earlier info | none | 1 per level | frequent |

**Hard cap:** the *Inversion test* (TRUTH_LIE_SYSTEM §9, R4) — inverting every statement may never solve a level.

## 6. Hint & assist system (anti-frustration)

Three layers, all optional:

1. **Diegetic nudges** (default): Previous Witness's marginalia and environmental emphasis (light pools, dust drift) appear after long stalls. Always *true*. Stall thresholds: 4 / 8 / 12 min of no-progress (adaptable, see PLAYER_BEHAVIOR_SYSTEM).
2. **Assist: Ask for Help** (Pause → *Out-of-world note*): clearly stylized outside the fiction (plain paper insert, different type). Three tiers: *Nudge → Pointer → Answer*. **Always truthful.**
3. **Assist Modes** (Accessibility): skip puzzle (with confirm), extended timers, highlight interactables (non-diegetic reticle option).

## 7. Failure

There is no death. Failure = *time cost and misinformation consequence*, capped by R5 (≤ 90 s to detect and recover; no permanent loss without checkpoint).

## 8. Collectibles

- **Returned Notices (7):** envelopes hidden across levels, each bearing a household name from the fire. Optional. All 7 unlock the secret ending.
- **Marginalia:** Previous Witness annotations (non-mandatory lore/hints).
- No stat-chasing collectibles.

## 9. Fourth-wall policy

**Allowed (authored, finite):**
1. Clerk references the player's *in-game* behavior profile ("Your File").
2. Reaction to pausing, long idle, quitting to menu, and Continue after absence (using in-game timestamps stored in the save).
3. Menu skinning that changes with story state (title screen reflects last ending).
4. A fake "credits" sequence at L17 (skippable after 3 s, clearly in-engine).
5. In-world "Settings"-like panels (L13) that are *props*.

**Forbidden (hard):**
- Reading or displaying real OS user names, file names, hardware ids, clipboard, webcam, microphone, network.
- Faking a crash, file deletion, malware, forced window resize/title tricks that persist.
- Modifying anything outside `user://`.
- The **real** Settings/Accessibility/Save UI ever lying. (Rule R9.)
- Altering audio levels or brightness settings without the player's input.

## 10. Replayability

- **Second Reading (NG+):** same hidden truth, different *surface*. Contextual statements swap variants; the Clerk's tells are rebalanced; ~25% of Ledger evidence nodes relocate. Seven Notices relocate.
- Endings: Amend, Affirm, Expunge (+ secret Corroborate).
- Fixed Experience (adaptation off) vs Adaptive Experience (on) as replay variety.

## 11. Pacing curve (target)

| Minute | Intensity | Content |
|---|---|---|
| 0–30 | Low | Trust-building, tutorial through play |
| 30–120 | Rising | Omission, mismatch, first Ledger use |
| 120–240 | Plateau + spikes | Temporal truths, mirrors, dispatcher |
| 240–330 | High | Behavior mirror, UI lies, testimony |
| 330–390 | Peak | Hearing, reel, false exit |
| 390–450 | Release + weight | Vault, reveal, verdict |

## 12. Acceptance criteria (document-level)

- A new designer can read this and list every verb, its input and its consequence.
- Every system in CORE_MECHANICS maps to at least one verb or rule here.
