# VISUAL_POLISH_CHECKLIST

> **Status:** DESIGNED — specification only; not yet implemented in `game/`.

Used at **every art/UI review**, and as a formal gate at **M1, M2, M6, M8**. Companion to UI_UX_REVIEW, UI_COMPONENT_SPEC, CINEMATIC_DIRECTION.

## 1. How to use

- Score each item **0** (fail) · **1** (weak) · **2** (acceptable) · **3** (shipped-quality).
- Items marked **[C]** are Critical: any **0** fails the gate.
- Gate score = sum / (3 × items) — thresholds: **M1 ≥ 60%, M2 ≥ 85%, M6 ≥ 90%, M8 ≥ 92%**.
- Reviewers: art director, UI lead, one person who did **not** build the item. Sign-off recorded in the PR or review log.

## 2. The master question
For every screen, room, animation and UI feature: **"Would this look intentional in a commercially released indie game?"** If the answer is not an immediate yes, the item scores ≤ 1 and is redesigned.

## 3. Quick tests (run on 10 random frames/screens per review)

| Test | Method | Pass |
|---|---|---|
| **5-second screenshot** | Show a frame for 5 s to someone new, ask what it is | They name the game's mood/setting without prompting |
| **Grayscale** | Desaturate | Focal point and hierarchy still read |
| **Squint / thumbnail (25%)** | Shrink to 480 px wide | Silhouettes and focal light remain clear |
| **Brass discipline** | Isolate bright brass | Only interactables (and ≤ 1 decoy) remain |
| **Mute** | Play without sound | Visual communication still works for ≥ 1 clue per room |
| **No-UI** | Hide all UI | The scene still tells the player where to look |
| **Token audit** | Run token linter | 0 off-token colors/sizes/radii |
| **Default-look** | Search for default Godot theme/materials | None |

## 4. Checklist

### A. Identity and consistency
| # | Item | C |
|---|---|---|
| A1 | Palette obeys color language (UI_UX_REVIEW §4); accents ≤ 5% of frame | C |
| A2 | Wrong Cyan only in-world, never in UI chrome | C |
| A3 | No false-information styling difference between truthful and deceptive carriers (R13) | C |
| A4 | Typography uses only the scale in UI_COMPONENT_SPEC §1.3 | |
| A5 | Paper/stamp/brass language consistent across menus, Ledger, slips, loading | |
| A6 | No default Godot theme, no stock buttons, no glassmorphism or neon | C |
| A7 | Icons share stroke, grid and drawing logic | |

### B. UI and interaction
| # | Item | C |
|---|---|---|
| B1 | Every component has Rest/Focus/Pressed/Selected/Disabled captured | |
| B2 | Mouse hover and controller focus look identical | |
| B3 | No visible state change faster than 90 ms; no instant popups | C |
| B4 | Press feedback present (sink + sound) and distinct from hover | |
| B5 | ≤ 5 interactive elements per screen (excluding Settings); ≤ 2 UI text items during play | |
| B6 | Main menu objects have distinct silhouettes; no stacked identical buttons | C |
| B7 | Notification system rules obeyed (max 1, no toasts, no stacking) | |
| B8 | Text scale 100–200% never clips; min text 20 px at 1080p | C |
| B9 | Caption bar legible against bright and dark scenes (tested on 5 locations) | C |
| B10 | Loading never shows fake progress; min-display rules obeyed | |
| B11 | Ledger page pixel-aligned (no blur) at 1080p | C |
| B12 | Slip text readable: x-height ≥ 14 px in Reading mode | C |

### C. Animation and motion
| # | Item | C |
|---|---|---|
| C1 | Every animation has anticipation → action → settle where applicable | |
| C2 | Easing from tokens only; no linear UI motion | |
| C3 | Weight classes respected (light/medium/heavy) for doors and props | |
| C4 | Reduced Motion variant exists and tested for each animated element | C |
| C5 | No flicker > 3 Hz; Reduced Flashing variant exists | C |
| C6 | Environmental changes are telegraphed or unseen (R7) | C |
| C7 | Distortion budget obeyed (≤ 3 distinct per level, ≤ 8 s visible total) | |
| C8 | No screen-space glitch effects (RGB split, scanlines, datamosh) | C |

### D. Environment art
| # | Item | C |
|---|---|---|
| D1 | Room has focal point, 2–3 secondary points, landmark, interactive cluster | C |
| D2 | Lighting hierarchy key > fill > accent is legible in grayscale | |
| D3 | Wear masks and ≥ 3 decals per room; no clean-room look | |
| D4 | No visible texture tiling at 1080p standing positions | |
| D5 | One bespoke hero prop per room; silhouettes distinct from kit | |
| D6 | Human, institutional and wrongness traces present (WORLD_BUILDING §8) | |
| D7 | Brass polish indicates interactivity (brass discipline test) | C |
| D8 | Shadows and reflections consistent except where Drift is intended | |
| D9 | Blacks ≥ `#0E0D0B`; no crushed shadows hiding puzzle evidence | C |
| D10 | Sound sources visible/plausible in the room (≥ 2) | |

### E. Characters and props
| # | Item | C |
|---|---|---|
| E1 | Echoes: hands/posture/clothing wear detailed; no walk cycles; paper crease on face | |
| E2 | Hands/sleeve correct (L04 cuff) | C |
| E3 | Props use trim sheets/shared materials within budgets | |
| E4 | Bevels on hard edges; no razor-sharp CG edges | |

### F. Cinematics and camera
| # | Item | C |
|---|---|---|
| F1 | Camera layers combine without nausea; roll ≤ 1.5° | C |
| F2 | Entry composition hero shots reviewed per level | |
| F3 | Each reveal tested with first-time players | |
| F4 | CM-01..04 have animatics and skip/Reduced Motion plans | |
| F5 | Letterbox only in endings; DOF only in allowed contexts | |
| F6 | Fake credits clearly in-engine; skip works; no fake errors | C |

### G. Technical polish
| # | Item | C |
|---|---|---|
| G1 | 60 FPS in the reviewed scene on the reference rig | C |
| G2 | No shader-compile hitch on first view of effects | |
| G3 | Ultrawide/16:10 layouts verified; UI safe frame intact | |
| G4 | Colorblind simulations preserve key cues (patterns/shapes backup) | C |
| G5 | Photosensitivity analysis on footage | C |

## 5. Red-flag list ("looks AI-generated / template-like")

Any of these in a review screenshot is an automatic **fix before merge**:

1. Even, shadowless fill lighting with no focal pool.
2. Perfectly symmetrical clutter (props mirrored left/right).
3. Repeating identical props at regular intervals.
4. Uniform noise texture on every surface (no wear logic).
5. Glossy rounded UI rectangles or gradient glows.
6. Generic neon/teal-orange grade.
7. Text overlays that restate what the room already shows.
8. Floating, unanchored UI with no drop contact or paper logic.
9. Props with melted/nonsensical details (labels with gibberish text).
10. Decorative glitch/scanline effects.
11. Inconsistent scale (door handle heights, chair sizes).
12. Menu laid out as a centered vertical list of equal buttons.

## 6. Review cadence and artifacts

| When | What | Artifact |
|---|---|---|
| Weekly | 10 random frames + 3 UI screens | Review log (scores, notes) |
| Per level (greybox→art) | D/E/F sections | Level review sheet in `docs/levels/lNN_<name>.md` |
| M1 | UI motion spike + look-dev keyframes vs in-engine capture | Gate report |
| M2 | Full vertical slice | Gate report + screenshot sheet (component states) |
| M6 | All levels | Gate report + flash analysis + colorblind sheets |
| M8 | RC | Final sign-off |

## 7. Gate report template

```
Gate: M2    Date:    Build:
Reviewers:
Score: __ / __  (__%)   Critical failures: __
Top 5 strengths:
Top 5 fixes (owner, due):
Red-flag findings:
Decision: PASS / PASS WITH CONDITIONS / FAIL
```

## 8. Acceptance criteria
- Gate thresholds met with zero Critical failures.
- Red-flag list empty in the final review sample.
- Screenshots of the 10 hero frames approved by the art director for store material.
