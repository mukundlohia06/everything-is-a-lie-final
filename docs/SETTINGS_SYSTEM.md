# SETTINGS_SYSTEM

> **Status:** DESIGNED — specification only; not yet implemented in `game/`.

Stored in `user://settings.cfg` (Godot `ConfigFile`). Applied immediately where possible, with a 10 s revert prompt for display-mode changes.

## 1. Principles

1. **Settings UI never lies** (R9): values shown are always the applied values.
2. First-launch flow offers Accessibility and Display basics before New Game.
3. Defaults chosen to be safe and comfortable (no heavy effects on by default).
4. Unknown keys preserved on save; missing keys fall back to defaults.

## 2. Schema and defaults

### Video
| Key | Type | Default | Notes |
|---|---|---|---|
| `video/window_mode` | enum | Fullscreen (borderless) | Windowed / Borderless / Exclusive |
| `video/resolution` | Vector2i | Desktop | List from display |
| `video/vsync` | enum | On | Off/On/Adaptive |
| `video/fps_cap` | int | 60 | 30/60/90/120/144/Unlimited |
| `video/preset` | enum | Auto-detected | Low/Medium/High/Custom |
| `video/render_scale` | float | 1.0 | 0.5–1.0 (FSR option via Godot scaling) |
| `video/scaling_mode` | enum | Bilinear | FSR 1.0 / FSR 2.2 if available in Godot 4.7.2 (verify in the Phase 1 spike) |
| `video/shadows` | enum | High | Low–High |
| `video/ssao` | enum | Medium | Off–High |
| `video/ssr_reflections` | enum | Medium | Used in L09 |
| `video/gi_quality` | enum | Baked | Fixed (baked); probes quality |
| `video/texture_quality` | enum | High | Mip bias |
| `video/anisotropic` | int | 8 | 1–16 |
| `video/msaa/taa` | enum | TAA | Off / FXAA / TAA / MSAA 2x/4x (if supported) |
| `video/film_grain` | bool | On | |
| `video/chromatic_aberration` | bool | On | Off in Accessibility preset |
| `video/motion_blur` | bool | Off | |
| `video/brightness` | float | 1.0 | 0.6–1.4 gamma slider with calibration image |
| `video/fov` | int | 75 | 60–100 |

### Audio
| Key | Default |
|---|---|
| `audio/master` | 80% |
| `audio/music` | 70% |
| `audio/ambience` | 80% |
| `audio/sfx` | 80% |
| `audio/voice` | 90% |
| `audio/ui` | 70% |
| `audio/output_mode` | Headphones (Headphones/Stereo/Surround) |
| `audio/dynamic_range` | Normal (Night mode compresses) |
| `audio/mute_unfocused` | On |

### Gameplay
| Key | Default |
|---|---|
| `gameplay/adaptive_experience` | On |
| `gameplay/tracking_enabled` | On |
| `gameplay/head_bob` | On |
| `gameplay/assist_hints` | Available |
| `gameplay/auto_pause_on_focus_loss` | On |
| `gameplay/walk_toggle` | Hold |
| `gameplay/language` | System/EN |

### Accessibility (see ACCESSIBILITY.md)
Subtitles, caption mode, text scale, subtitle background, colorblind mode, high-visibility reticle, reduced motion, reduced flashing, hold→toggle, UI contrast.

### Controls
See INPUT_SYSTEM §4–5.

## 3. Presets

| Preset | Render scale | Shadows | SSAO | SSR | Particles | Target GPU |
|---|---|---|---|---|---|---|
| Low | 0.80 (FSR) | Low | Off | Off | 40% | GTX 1060 / RX 580 |
| Medium | 0.90 | Medium | Low | Low | 70% | GTX 1660 |
| High | 1.0 | High | Medium | Medium | 100% | GTX 1660 Super+ |
| Ultra | 1.0 | High + extra | High | High | 100% | RTX 2060+ |

Auto-detect on first launch with a 15 s benchmark fly-through (optional; skippable).

## 4. Implementation notes

```gdscript
class_name SettingsService extends Node
signal changed(key: StringName)
func get_value(key: StringName) -> Variant
func set_value(key: StringName, v: Variant, apply := true) -> void
func apply_all() -> void
func reset_section(section: StringName) -> void
func save() -> void  # debounced 0.5 s
```

- A `SettingDef` resource list describes every setting (key, type, range, default, UI control, localization key, applier callable). The Settings UI is generated from this list → guarantees UI and values agree.

## 5. Revert-safety

Display mode/resolution/scaling changes show a 10 s "Keep these settings?" paper prompt; auto-revert on timeout or Esc.

## 6. Acceptance criteria

- Deleting `settings.cfg` yields a valid default config without errors.
- Every setting is reachable by controller.
- Changing settings never alters saved game state.
