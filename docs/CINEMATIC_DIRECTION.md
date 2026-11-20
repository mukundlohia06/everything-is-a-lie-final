# CINEMATIC_DIRECTION

> **Status:** DESIGNED — specification only; not yet implemented in `game/`.

Cinematic presentation exists **only where it improves gameplay or story clarity**. The default is uninterrupted first-person play. Design only; no code.

## 1. Principles

1. **The player's eyes are the camera.** Control is retained except in four authored moments (§7).
2. **Compose the world, not the cut.** Framing is achieved by level layout, light and anchors, not by taking the camera away.
3. **Reveal by moving, not by cutting.** Reveals are triggered by walking, turning or waiting.
4. **Restraint:** no letterbox, lens flare, motion blur, or constant shake during play.
5. **Everything has a skip/accessibility path.**

## 2. Camera rig

Layers combine in this order (each independently disable-able):

| # | Layer | Spec | Reduced Motion |
|---|---|---|---|
| 1 | Look | Player input; eye height 1.62 m; FOV 75° (60–100) | Same |
| 2 | Head bob | Amp 0.012 m, tied to step cadence | **Off** |
| 3 | Breath sway | ±0.3°, 0.25 Hz | **Off** |
| 4 | Examine/Reading offset | Dolly + FOV −12°, 350 ms | Instant FOV, 150 ms fade |
| 5 | Event shake | See §6 | **Off** |
| 6 | Constraint | Look limits during authored moments (±35° yaw) | Same |

Rules: total roll ≤ 1.5°; **no roll from look input**; no camera movement that the player did not cause or that is not telegraphed by sound/light.

## 3. Framing rules

| Rule | Application |
|---|---|
| **Axial calm, one broken element** | Halls are symmetric; exactly one object breaks symmetry (L01 clock, L02 chair) |
| **Entry composition** | The first sightline from each room entry anchor places the focal point on the axis or a third, with ≥ 1 clue visible (UI_UX_REVIEW §7) |
| **Leading lines** | Floor tiles, ceiling tubes, shelf rows lead to the focal point (L03, L14) |
| **Depth layering** | Foreground frame (door jamb/shelf), midground interactive cluster, background landmark |
| **Doorway framing** | Doors are narrower than the room beyond; light spills in a readable wedge |
| **Eye-level honesty** | No dutch angles, no low-angle "menace" shots. Unease comes from the room |
| **Anchors** | Level scenes contain `Anchor_Entry`, `Anchor_Reveal_*` markers (position + look direction) used for composition review and optional gentle reorient (never forced) |

## 4. Focal depth (DOF)

| Context | DOF | Spec |
|---|---|---|
| Free walk | **Off** | Clarity for observation puzzles |
| Examine | On | Focus = object; background blur strength 0.6, 350 ms ramp |
| Reading mode | On | Strong blur 0.9; world dim 15% |
| Ledger/Pause | On | Strong blur 1.0 |
| L16 projector reveal | Soft | Focus pulls to beam source over 1.5 s |
| Endings | Authored | Per beat |

Quality tiers: Low preset uses cheaper blur or dim-only. DOF never blurs puzzle-critical evidence in free walk.

## 5. Camera behavior by state

| State | Behavior |
|---|---|
| Idle | Breath sway only |
| Walk | Bob + gentle FOV +1° at brisk walk |
| Examine | §2 layer 4; object edge lifts softly |
| Mechanism use | Lean-in 4 cm over 150 ms toward lever/switch; settle 150 ms |
| Carry | Camera lowers 2 cm; item sways with critically damped spring |
| Listen (hold) | FOV −3°, static vignette +4%, audio emphasis (AUDIO_DESIGN §7) |
| Wait (hold) | No camera change; wristwatch glances at 0.5 s intervals |
| Ledger | Notebook alignment (UI_COMPONENT_SPEC §5.9) |
| Pause | World freezes; DOF + desaturate; camera frozen |

## 6. Event shake (controlled)

| Event | Amplitude | Duration | Frequency | Notes |
|---|---|---|---|---|
| Heavy door closes | 0.4° pitch | 220 ms | 8 Hz decaying | Only if within 6 m |
| Pipe knock (L07) | 0.25° | 160 ms | 10 Hz | Doubles as the cue |
| Transformer surge (L13) | 0.6° | 300 ms | 6 Hz | Telegraphed by hum rise |
| Collapse (D5, L20) | 1.2° | 2.0 s | Perlin, 3 Hz | Finale only |
Caps: ≤ 1.5° amplitude; ≤ 12 Hz; never during Reading/Ledger; **off** in Reduced Motion (replaced by audio + light).

## 7. Cinematic moments (authored, finite)

| ID | Level | Moment | Control | Duration | Skippable |
|---|---|---|---|---|---|
| CM-01 | L01 end | The clock steps backward | Full control | ≤ 6 s (player may ignore) | N/A |
| CM-02 | L17 | False exit and fake credits | Walk, then limited look | ≤ 75 s | After 3 s |
| CM-03 | L19 | Dictaphone reveal | Look ±35° yaw | 90 s | Replay via tape control |
| CM-04a–d | L20 | Four endings | None during epilogue | 60–120 s each | After first viewing |

### CM-01 — The clock steps backward
Beats: (1) Clerk's Window shutter opens, warm light. (2) Player walks to window; (3) wall clock second hand ticks, minute hand steps **back one minute** (no sound change); (4) nothing is said. Camera: unchanged. Audio: tick continues in the same rhythm but one tick is missing. Reduced Motion: same (tiny motion, no flashing).

### CM-02 — The false exit (L17)
| t (s) | Beat |
|---|---|
| 0 | Door opens onto dawn exterior; light floods; music enters (sparse piano) |
| 0–20 | Player walks the road (full control); the Concord behind; flat painted sky edge faintly visible |
| 20 | Controls ease to look ±35° only; camera begins slow, 2 m/10 s dolly away from building |
| 25 | Fade to paper-white, typed credit slips appear (diegetic conveyor style) |
| 28 | Skip prompt (paper corner "Hold to skip") |
| 70 | Music cuts; silence 1.5 s |
| 72 | Clerk (true statement): "This is not the end." |
| 75 | Cut back to the road end: it loops to the building's rear door |
If skipped, the Clerk line plays at skip. No flashing; no fake error or system messages.

### CM-03 — Dictaphone reveal (L19)
Beats: player presses play; tape hiss; Clerk voice reads the original statement; the voice **slips register** to the Witness's own; window reflection shows the player; "Your File" dossier fans onto the table (data-driven, in-game stats only). Camera: yaw ±35°, slow push 20 cm over 30 s; DOF eases to the dictaphone.

### CM-04 — Endings
| Ending | Camera | Light | Audio |
|---|---|---|---|
| Amend | Slow pull-back from the book; stamp NOT SERVED | Warm key rises | Single sustained note, then silence |
| Affirm | Static, centered; Intake Hall reset glimpse | Perfect, too even | Clerk's thanks, ticking |
| Expunge | Handheld-feeling drift (micro, 0.3°) as pages burn | Hard top-light, paper particles | Fire-bed sound, no music |
| Corroborate | Seven slow lateral dollies across names | Soft lamp | Names spoken in Witness's voice |
Bars: 2.39:1 letterbox applies **only** in CM-04; option "Cinematic bars: Auto/Off".

## 8. Transitions

| Transition | Duration | Behavior |
|---|---|---|
| Door pass-through (default) | 0 s cut-free | Walk through; load/prewarm behind door; room tone crossfade 400 ms |
| Lift | 3–6 s | Camera stable, light strips pass (≤ 1 Hz), look-around retained |
| Stairs | Walk | No transition |
| Pneumatic capsule | 2.5 s (rare) | First-person in capsule, fixed look ±20° |
| **Act slip** | 4 s | A typed slip drops into a tube tray at the start of each act ("ACT II — RECORDS"), control retained |
| Paper-blink | 400 ms | Fade through paper-white; used only between New Game ticket and L01 and at CM-02 |
| Death/failure | — | None exist |
Rules: no black-frame cuts mid-level; no whip pans; no cross-dissolves between playable spaces.

## 9. Environmental reveals

| ID | Level | Trigger | Staging | Time | Audio |
|---|---|---|---|---|---|
| RV-01 | L01 | Third lamp lit | Shutter of Clerk's Window rolls up | 2.0 s | Shutter rattle, bell |
| RV-02 | L03 | Enter corridor | Frosted end wall glows; silhouette passes | 1.5 s | Far door |
| RV-03 | L05 | Find ladder wear | Mezzanine hatch light slants down | 1.0 s | Latch |
| RV-04 | L07 | Pump cycle | Steam builds from basin | 8 s telegraph | Knock, hiss |
| RV-05 | L09 | Enter gallery | Reflection reveals cracks (cyan cast) | Immediate | Faint tick |
| RV-06 | L12 | Sit/approach table | Pendant lights warm; Echoes' lights lift | 2.0 s | Cutlery |
| RV-07 | L16 | Frames ordered | Projector beam reveals film | 3.0 s | Projector spin-up |
| RV-08 | L17 | Walk road | Sky edge becomes legible (hint) | 20 s gradual | Wind thins |
| RV-09 | L18 | Enter vault | Single lamp lights ledger | 1.5 s | Low hum |
| RV-10 | L20 | Enter hall | Lecterns lit in sequence | 3 × 0.8 s | Soft chimes |
Reveals are triggered by player movement/looking and **never remove control**.

## 10. Lens and post (cinematic context)

| Item | Setting |
|---|---|
| DOF | See §4 |
| Motion blur | Off by default; optional 0.15 in settings |
| Film grain | Static 0.02 |
| Chromatic aberration | Only D3+/CM-04d; ≤ 0.5 px |
| Bloom | Low, high threshold |
| Lens flare/god rays | None; volumetric fog only for projector beam and L05 skylight (≤ 1.0 ms) |
| Exposure | Fixed per level; manual eased shifts at reveals (±0.3 EV) |

## 11. Authoring workflow

1. **Storyboard + animatic** (timed with temp audio) for each CM and RV; reviewed before production.
2. `CinematicBeat` resources + `AnimationPlayer` timelines in level scenes; camera constraints as nodes (design: `CameraConstraint`, `RevealTrigger`, `AnchorMarker`).
3. Saving disabled during CMs; resume at beginning of the moment (consistent with SAVE_SYSTEM §8).
4. Every CM supports: pause, skip rule above, captions, Reduced Motion/Flashing variants.

## 12. Aspect ratios
16:9 reference; 16:10 and 21:9 extend 3D view (UI frame stays 16:9, centered); 4:3 letterboxes UI frame only. Camera FOV is **horizontal-plus** (vertical FOV kept consistent) with clamp to avoid distortion on ultrawide (max horizontal FOV 110°).

## 13. Acceptance criteria
- Each CM/RV has an approved animatic and a Reduced Motion plan.
- Look control is never removed (limited to ±35° yaw in CM-02/CM-03, authored in CM-04); movement is locked only during CM-02, CM-03 and CM-04.
- In recorded playtest footage, no frame sequence flashes > 3 Hz or luminance delta > 10% at > 3 Hz.
- Reveal timings feel unforced: ≥ 80% of testers see RV-02, RV-05, RV-08 without prompting.
