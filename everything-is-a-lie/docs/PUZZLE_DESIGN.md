# PUZZLE_DESIGN

> **Status:** DESIGNED — specification only; not yet implemented in `game/`.

## 1. Principles

1. **Solutions are deduced, not guessed.** Every solution has a logical *why* the player can reconstruct.
2. **Knowledge gates, not item gates.** A key-and-door pairing must serve a larger idea or it is cut.
3. **Two roads to the same truth.** Each deduction has ≥ 2 independent evidence nodes.
4. **No opposite rule.** The Inversion test (TRUTH_LIE_SYSTEM R4) applies.
5. **Failure teaches.** A wrong attempt yields information, not punishment.
6. **Accessibility parity.** Every puzzle has a non-audio and non-color path (or an assist equivalent).

## 2. Taxonomy

| Type | Skill | Examples |
|---|---|---|
| Contradiction | Compare sources | L04, L08 |
| Environmental observation | Notice details | L01, L05 |
| Cause & effect | Operate and watch | L03, L07 |
| Pattern recognition | Match patterns | L03 lamps, L16 frames |
| Spatial reasoning | Mental map | L06, L18 |
| Timing | Learn a cycle | L07 |
| Memory | Recall earlier info | L10, L11 |
| Sound | Listen logic | L13, L14 |
| Lighting | Light reveals | L09, L18 |
| UI manipulation (in-world) | Read lying instruments | L13 |
| Cross-level knowledge | Re-apply earlier lesson | L15, L19 |

## 3. Puzzle spec template

```
ID: PZL-L05-A
Level: L05          Type: Technical truth + observation
Goal: Retrieve card R-2209
Statements: STM-L05-017 (Clerk), STM-L05-018 (slip)
Evidence nodes: EVD-L05-CATALOG-2M, EVD-L05-LADDER-WEAR, EVD-L05-SHELF-LABEL
Deduction chain:
  1. Catalog cards read "2/M" (EVD-CATALOG-2M)
  2. Ladder wear leads to a shelf hatch (EVD-LADDER-WEAR)
  3. Therefore records are on mezzanine "2M", not floor 2
Wrong attempts: searching floor 2 → empty shelves (time_loss ≤ 60 s)
Reinterpretation line: STM-L05-019 ("I said floor, not mezzanine.")
Nudges: tier1 light pool on ladder; tier2 marginalia "Look up"; tier3 assist answer
Accessibility: shelf labels have tactile-contrast text; audio not required
Accept: 90% of testers solve ≤ 8 min; ≥ 80% can state chain after reveal
Edge cases: player climbs ladder first → statement still delivered via slip
```

## 4. Flagship puzzles with solutions

| ID | Level | Setup | Solution & why it works |
|---|---|---|---|
| PZL-L01-A | L01 | Three lamps; Clerk says "from the door inward" | Light in order; dust current flows toward ticket machine. Teaches Use + Examine; all truthful |
| PZL-L03-A | L03 | Six doors, floor tiles, six lamps | Floor-tile pattern seen from doorway = lamp pattern; streamers drift toward onward door; frosted end shows a door opening (real sound) |
| PZL-L04-A | L04 | Coat 12 vs "the coat you came in with" | Player's visible cuff = herringbone grey; only one coat matches; marginalia is wrong about frames |
| PZL-L05-A | L05 | "No files on second floor" | Catalog "2/M" (see §3) |
| PZL-L07-A | L07 | Fountain, pump | Pipe knock → steam in 8 s; collect key in safe window; same sentence later false (temporal) |
| PZL-L08-A | L08 | Two voices, jacks | Mirrored numbering n↔9−n; rear panel says OPERATOR SIDE; both statements true |
| PZL-L09-A | L09 | Mirrors show cracked tiles | Mirror cyan-cast shows crack pattern; step only on uncracked tiles; faint tick on crack tiles as redundant cue |
| PZL-L11-A | L11 | 12 cards filing wall | Link each card to evidence; two cards remain inconsistent → prompt reinterpretation |
| PZL-L13-A | L13 | Lagging indicators | Lamp = previous state; transformer pitch = true state; visual redundancy: needle vibration amplitude |
| PZL-L14-A | L14 | Looping hallway | Exit = footstep dead spot (no echo return); visual redundancy: dust doesn't swirl at dead spot |
| PZL-L16-A | L16 | Film frames | Shadow angle, clock hands, weather change monotonic → unique ordering |
| PZL-L18-A | L18 | Overwritten book | UV reveals under-ink; align 3 transparencies to restore "NOT SERVED" |

## 5. Anti-patterns (rejected on review)

- Single-point guess ("pick the right of 6 identical doors").
- Hidden pixel hunts.
- Puzzles requiring real-world knowledge not in the game.
- Audio-only or color-only critical cues without equivalents.
- Reinterpretation inconsistent with literal text.
- Timed puzzles with < 20 s margin.

## 6. Difficulty tuning knobs

| Knob | Effect |
|---|---|
| `evidence_redundancy` (2–3) | More clues |
| `cue_salience` (0–1) | Light/dust emphasis strength |
| `stall_nudge_delays` | 4/8/12 min default |
| `distractor_count` | Red herrings |
| `reinterpretation_depth` | Chain length |

## 7. Playtest protocol per puzzle

1. Silent playtest, think-aloud for 5 testers.
2. Record: time to solve, hypotheses voiced, wrong attempts, nudges used.
3. After solve ask: "Why did that work?" → score articulation.
4. Fail criteria: < 70% articulate, or any tester reports "guessing".

## 8. Acceptance criteria

- Every puzzle has a filled §3 template before greybox.
- Every puzzle passes lint (R1, R4, R6, R8) before art pass.
