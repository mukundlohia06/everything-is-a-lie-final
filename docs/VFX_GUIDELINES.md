# VFX_GUIDELINES

> **Status:** DESIGNED — specification only; not yet implemented in `game/`.

**Principle:** VFX is atmosphere and information, never spectacle. Every effect either grounds the space or encodes an anomaly.

## 1. Particle usage

| Effect | Node | Count | Notes |
|---|---|---|---|
| Dust motes | GPUParticles3D | ≤ 400 per room | Drift reveals airflow (L01, L03, L14 puzzle info) |
| Paper flecks | GPUParticles3D | ≤ 150 | Records; collapse (D5) ≤ 1,500 |
| Steam (L07) | GPUParticles3D + quad shader | ≤ 200 | Warning cue paired with sound |
| Lamp bloom | Shader/Glow | — | Subtle |
| Heat shimmer (Echoes) | Spatial shader (refraction) | — | Edge only |

## 2. Anomaly VFX catalog (by Drift level)

| ID | Effect | Drift | Spec | Reduced Motion |
|---|---|---|---|---|
| ANOM-LIGHT-FLUTTER | Light intensity wobble | D3 | ≤ 2%, ≤ 3 Hz | Static |
| ANOM-SHADOW-OFF | Shadow direction offset 1–2° | D2 | Baked + dynamic mismatch | Same (no motion) |
| ANOM-REFLECT-WRONG | Reflection mismatch | D2, L09 | Planar reflection / SSR state diff | Same |
| ANOM-SCALE | Prop scale ±3% on look-away | D1–D2 | Off-screen only | Disabled |
| ANOM-TEXT-SHIFT | Peripheral text changes | D3 | Triggers outside center 30° | Disabled |
| ANOM-GEO-FOLD | Impossible connection | D4 | Portals/stitched rooms | Fade instead of slide |
| ANOM-CYAN-CAST | Cool cast on reflections | L09 | ≤ 5% frame | — |
| ANOM-COLLAPSE | Paper storm, light shift | D5 | Finale only | Reduced density |

## 3. Shaders

| Shader | Purpose | Cost target |
|---|---|---|
| `paper_ink` | Ink noise on stamps/slips | < 0.1 ms |
| `heat_shimmer` | Echo edge | < 0.15 ms |
| `film_grain` | Post | < 0.1 ms |
| `drift_light` | Uniform-driven flicker | < 0.05 ms |
| `uv_reveal` | L18 UV overlay | < 0.1 ms |

Pre-warm every shader/material during diegetic transitions to avoid hitching.

## 4. Post-processing profile

Defined per act in `res://assets/vfx/post/act_N.tres`: tonemap, bloom, SSAO, LUT, vignette, grain. Chromatic aberration only in D3+, off in the Accessibility preset.

## 5. Flashing/safety rules

- No flash > 3 per second; no luminance change > 10% over full screen at > 3 Hz.
- No strobing in D5; use slow pulses (≤ 1 Hz).
- **Reduced Flashing** disables light flutter, lightning-like effects and any high-contrast changes.

## 6. Performance

- Particle total per scene ≤ 2,000 active (High), ≤ 800 (Low).
- Overdraw budget: translucent layers ≤ 3 on average.
- LOD for effects: distance-based emission reduction.

## 7. Acceptance criteria

- Every anomaly effect has an ID and a spec row, a Reduced Motion variant, and a perf cost recorded.
- Photosensitivity review with an automated flash analyzer before release.
