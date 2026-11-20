# ACCESSIBILITY

> **Status:** DESIGNED — specification only; not yet implemented in `game/`.

A game about deception must be **legible to everyone**. Accessibility features are never part of the deception.

## 1. Core rules

1. **Parity rule (R8):** no puzzle-critical information is conveyed *only* by color, *only* by audio, or *only* by fast motion. Each has an equivalent channel or an assist.
2. **Truthful system UI (R9):** Settings, Accessibility, Save/Load, captions never mislead.
3. Accessibility options available at **first launch** and in-game.
4. Options never disable the story; they only change presentation and assistance.

## 2. Feature matrix

| Area | Feature | Spec | Default |
|---|---|---|---|
| Subtitles | On/Off; sizes S/M/L/XL (24/32/40/52 px @1080p) | Background opacity 0–100%; speaker labels | On, M |
| Captions | Dialogue only / Dialogue + key sounds / Full | Describes **sound heard** (R12) | Dialogue + key |
| Text | Scale 100/125/150/200%; dyslexia-friendly font option (e.g., Atkinson Hyperlegible, OFL) | Reflow without clipping | 100% |
| Contrast | High-contrast UI theme | ≥ 7:1 for all text | Off |
| Colorblind | Protanopia/Deuteranopia/Tritanopia filters + non-color cues | Shape/pattern differences for Wrong Cyan vs warm cues | Off |
| Visual assists | High-visibility reticle; interactable outline | Non-diegetic | Off |
| Motion | Reduced Motion | See ANIMATION/VFX variants; disables head bob, FOV ease, camera effects | Off |
| Flashing | Reduced Flashing | Disables flutter/lightning; limits luminance deltas | **On if OS reports preference**, else Off |
| Brightness | 0.6–1.4 with calibration | Gamma | 1.0 |
| Audio | Individual bus sliders; mono audio; night mode | Mono sums L+R | Off |
| Audio cues | Visual equivalents for audio puzzles | Optional "sound visualizer" ripple icons at screen edge for `listenable` sources | Off |
| Controls | Full rebinding; Hold→Toggle; one-handed layout presets; sensitivity | See INPUT_SYSTEM | Hold |
| Timing | No required fast reactions; Wait/timing puzzles have extended-window option (×2) | | Off |
| Difficulty/Assist | Ask for Help (3 tiers); Skip Puzzle (confirm); Extended nudges (earlier) | Always truthful | Available |
| Cognitive | "Case Notes" auto-summary of what has been established (no verdicts) | Ledger page | On |
| Photosensitivity | Warning on first launch; analyzer-tested | | |
| Motion sickness | FOV slider, head-bob off, static vignette off, crosshair always | | |

## 3. Parity table for puzzle cues (excerpt)

| Puzzle | Primary cue | Redundant cue | Assist equivalent |
|---|---|---|---|
| L03 door sound | Positional audio | Streamers drift, frosted glass silhouette | Sound visualizer |
| L07 pump | Pipe knock | Steam hiss visual + pipe vibration | Visual countdown ring (Assist) |
| L09 mirrors | Cyan-cast reflection | Crack pattern shape, faint tick | Highlight cracked tiles (Assist) |
| L13 transformer pitch | Hum pitch | Needle vibration amplitude | Pitch visualizer |
| L14 dead spot | Echo absence | Dust swirl absent | Compass pulse (Assist) |

## 4. Deaf/hard-of-hearing

- Every Clerk line has captions.
- Audio-only puzzle cues have visual redundancies (above).
- Directional caption indicators for off-screen key sounds (`◂ [door, far away]`).

## 5. Blind/low-vision (scope)

Full blind play is out of scope; improve legibility: high contrast, scalable text, screen-reader support for **menus** via Godot's accessibility APIs where available in Godot 4.7.2 (verify in the Phase 1 spike), text-to-speech for slips (optional, system TTS).

## 6. Testing

| Test | Method |
|---|---|
| Colorblind | Simulation filters on all puzzle screenshots; check cue distinctness |
| Text scaling | Automated UI snapshots at 100–200% |
| Flash analysis | Harding-style/PEAT-equivalent automated analysis on captured footage |
| Controller-only | Complete game without keyboard/mouse |
| One-handed | Complete L01–L05 with one-handed preset |
| Accessibility playtest | ≥ 6 testers with varied needs by M6 |

## 7. Acceptance criteria

- All entries in §3 parity table implemented and verified.
- First-launch accessibility screen reachable and skippable by controller.
- No accessibility option reveals or alters puzzle truth.
