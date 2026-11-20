# PERFORMANCE_REQUIREMENTS

> **Status:** DESIGNED — specification only; not yet implemented in `game/`.

## 1. Targets

| Metric | Target |
|---|---|
| Resolution | 1920×1080 |
| Frame rate | 60 FPS locked (vsync) |
| Frame pacing | 99% of frames ≤ 18 ms; 99.9% ≤ 33 ms |
| Reference rig | Ryzen 5 3600, 16 GB, GTX 1660 Super, SSD |
| Minimum rig | i5-8400/Ryzen 5 1600, 8 GB, GTX 1060 6GB — Low preset at 60 FPS |
| Level load | ≤ 5 s on SSD, ≤ 12 s on HDD (masked diegetically) |
| Startup to menu | ≤ 10 s |
| Install size | ≤ 8 GB |
| RAM | ≤ 6 GB resident (16 GB system) / ≤ 4 GB (Low) |
| VRAM | ≤ 3.5 GB High / ≤ 2.5 GB Low |

## 2. Frame budget (16.6 ms @ 60 FPS, reference rig)

| Slice | Budget |
|---|---|
| Gameplay scripts + physics | ≤ 3.0 ms |
| UI | ≤ 0.8 ms |
| Audio | ≤ 1.0 ms |
| Render (CPU submit) | ≤ 3.5 ms |
| Render (GPU) | ≤ 9.5 ms |
| Headroom | ≥ 1.0 ms |

## 3. Scene budgets (High preset)

| Item | Budget |
|---|---|
| Visible triangles | ≤ 1.5 M |
| Draw calls | ≤ 1,200 |
| Dynamic shadow lights | ≤ 6 |
| Active particles | ≤ 2,000 |
| Unique materials | ≤ 60 |
| Active audio voices | ≤ 48 |
| Active physics bodies | ≤ 60 (rigid), ≤ 200 (static colliders) |

## 4. Techniques

| Area | Approach |
|---|---|
| Lighting | Baked `LightmapGI` for static; probes for dynamic props; limited real-time lights |
| Culling | `OccluderInstance3D`, room portals where useful; visibility ranges |
| LOD | Auto LOD import; visibility ranges for small props |
| Textures | VRAM compression (BPTC/S3TC); mipmaps; ≤ 2K hero |
| Materials/shaders | Shared materials; ubershader/pipeline pre-warm during transitions (verify availability in Godot 4.7.2) |
| Pooling | Particles, one-shot audio players, Ledger ink stamps, footstep decals |
| Loading | Threaded async; streaming of next-level data during corridors |
| Scripts | Avoid `_process` unless needed; use signals and timers; cache node refs |
| Physics | Simple colliders; no continuous collision except carried object |
| UI | Static layouts cached; avoid per-frame text rebuilds; SubViewports only when open |

## 5. Scalable settings (see SETTINGS_SYSTEM §3)

Render scale + FSR, shadow quality, SSAO/SSR tiers, particle density, texture mip bias.

## 6. Profiling process

1. **Per-level perf capture** at M2 and each content milestone (fly-through script, 90 s).
2. Record: avg/1% low FPS, frame time p99, draw calls, VRAM, RAM, load time.
3. Fail gate: any level > budget by > 10% blocks milestone.
4. Tools: Godot Profiler/Monitors, RenderDoc, Tracy (if GDExtension later), OS tools (PresentMon).

## 7. Stutter prevention

- Shader pre-warm list per level.
- Preload next-level materials during transitions.
- No synchronous file IO in gameplay.
- Save serialization on a worker thread.
- GC-like spikes: avoid allocation churn in `_process` (reuse arrays/dictionaries).

## 8. Acceptance criteria

- Reference rig, High preset: 60 FPS in all 20 levels with p99.9 ≤ 33 ms.
- Minimum rig, Low preset: 60 FPS in ≥ 18 of 20 levels; remaining ≥ 50 FPS.
- 2-hour soak test: memory growth < 5%.
