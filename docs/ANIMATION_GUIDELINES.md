# ANIMATION_GUIDELINES

> **Status:** DESIGNED — specification only; not yet implemented in `game/`.

**Style:** restrained, physical, institutional. Motion has weight; nothing bounces.

## 1. Timing and easing

| Element | Duration | Easing |
|---|---|---|
| UI panel open | 280 ms | `ease_out_cubic` |
| UI panel close | 200 ms | `ease_in_cubic` |
| Menu item focus / hover | 90 ms (`mo_micro`) | `ease_out_quad` |
| Stamp impact | 120 ms (scale 1.08→1, rot −2°) | Custom overshoot ≤ 8% |
| Page turn (Ledger) | 350 ms | `ease_in_out_sine` |
| Examine camera ease | 350 ms FOV −12° | `ease_out_cubic` |
| Door open | by weight class, see §9 | `ease_in_out_quad` |
| Lamp switch | 150 ms flick + 400 ms bulb warm-up | Linear + exp |
| Transition fade | 600 ms | `ease_in_out_sine` |

**Reduced Motion:** replace motion with 150 ms cross-fades; no scale/rotation tweens; camera ease disabled (instant FOV change).

## 2. Prop and mechanism animation

- Authored in Blender (actions exported glTF) or via `AnimationPlayer`/Tween for simple ones.
- Each interactable defines **anticipation (resist) → action → settle** with sound sync markers.
- Wrong-attempt animation: 8–12 px (or 1.5°) shake, 180 ms, dull audio; never comedic.

## 3. First-person hands

- Idle breathing offset ±0.5 mm at 0.25 Hz; off in Reduced Motion.
- Examine pose: hands hold object; sleeve cuff visible (L04 dependency).
- Carry spring: critically damped, ω = 12.

## 4. Echoes

- Minimal idle (weight shifts every 6–10 s), no walking cycles during testimony.
- Voice-synced head micro-turn only (no lip-sync — faces are blank).
- Heat shimmer shader provides life.

## 5. Camera animation

- Head bob amplitude 0.012 m, frequency tied to step cadence; toggle.
- Authored moments (CINEMATIC_DIRECTION §7) keep look control within ±35° yaw; movement is locked only in CM-02, CM-03 and CM-04.
- No forced camera roll. No full-control cutscenes except endings.

## 6. Drift animations

Subtle: prop offsets ≤ 3 cm, 1–2° rotations, only when *not in focus*. Never animate in view of the player unless the effect is the telegraphed event.

## 7. Tooling

- Godot AnimationTree for characters; AnimationPlayer for props; Tweens for UI.
- Naming: `anim_<object>_<action>` (e.g., `anim_door_open`).
- Animation events call `AudioService.play_event()` via method tracks.

## 8. Acceptance criteria

- Every animation has a Reduced Motion variant or is exempt (documented).
- No animation exceeds 3 Hz flicker.

## 9. Doors, objects, characters, environmental change and distortion

**Global rules:** no instant popups (minimum visible transition 90 ms); anticipate → act → settle; one moving element per user action; motion tokens in UI_COMPONENT_SPEC §2.

### Doors and mechanisms by weight class
| Class | Examples | Anticipation | Action | Settle | Sound sync |
|---|---|---|---|---|---|
| Light | Cabinet door, drawer | 60 ms latch tick | 400–700 ms | 80 ms | Latch, slide |
| Medium | Interior door, ladder hatch | 120 ms latch jiggle | 1.2–1.6 s, ease-in-out | 150 ms | Hinge creak keyed to angle |
| Heavy | Vault, hearing-room door, lift | 200 ms resistance | 2.0–3.5 s, slow start | 300 ms | Low thud, mechanism |
Locked/wrong door: 8–12 mm (1.5°) shake, 180 ms, dull click; never comedic.

### Objects
Carried items follow camera with critically damped spring (ω = 12). Dropped items settle by physics; no bounce > 20%. Stamps: impact 120 ms (scale 1.08→1, rot −2°) then ink spread 180 ms.

### Characters (Echoes)
Weight shift every 6–10 s; head micro-turn ≤ 8° over 400 ms toward the speaking source; **no walk cycles**; no lip-sync (faces are blank); hands may fidget at ≤ 0.2 Hz.

### Environmental changes
| Type | Rule |
|---|---|
| Off-screen change | Allowed only outside the player's view cone (and ≥ 2 m away); logged in Change Journal |
| In-view change | Telegraphed 0.5–2 s by sound or light, then 3–8 s slow motion |
| Never | Instant pops of geometry in view; changes during Reading/Ledger |

### Glitches and reality distortions (physical, never screen-space)
| Allowed | Spec |
|---|---|
| Light shift / flutter | ≤ 2%, ≤ 3 Hz (D3+) |
| Shadow/reflection mismatch | 1–2° / state difference (D2+) |
| Prop offset/scale on look-away | ≤ 3 cm / ≤ 3% |
| Text re-typesetting on slips (when unobserved) | Only via registered statement reinterpretation |
| Paper curl, dust stall, clock step | Ambient anomalies |
**Budget:** ≤ 3 distinct distortions per level, ≤ 8 s visible in total, none in the first 60 s of a level (except L01 clock). **Banned:** RGB split, scanlines, datamosh, screen tearing, fake error dialogs.
