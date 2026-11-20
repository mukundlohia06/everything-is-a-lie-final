# ART_DIRECTION

> **Status:** DESIGNED — specification only; not yet implemented in `game/`.

## 1. Strategy

20 levels share **6 room families** so a small team can reach AAA-indie polish through reuse, variation and wear — not volume.

| Room family | Levels |
|---|---|
| Civic hall (large, symmetrical) | L01, L15, L20 |
| Waiting/administrative (mid) | L02, L04, L10, L12 |
| Corridors/transit | L03, L06, L14, L17 |
| Records/stacks | L05, L11, L18 |
| Mechanical/utility | L08, L13 |
| Special (one-offs) | L07 Atrium, L09 Mirrors, L16 Projection, L19 Reading Room |

## 2. Asset categories and sourcing

Legend: **C** custom · **P** procedural · **B** purchased/licensed · **O** open source (CC0/permissive) · **T** temporary placeholder.

| Category | Examples | Source | Notes |
|---|---|---|---|
| Architectural kit | Walls, floors, ceilings, columns, trim | **C** (Blender modular, 2 m grid) | Trim sheets + decals |
| Hero props | Clerk's Window, ticket machine, switchboard, projector, dictaphone, brass bell | **C** | Unique silhouettes |
| Set dressing | Chairs, lamps, files, cups, ledgers | **C** + **B** (license-checked) | Variation via materials |
| Characters (Echoes) | 3 figures + hands | **C** (base mesh **O**/CC0 allowed) | Faceless |
| Hands (first-person) | Player hands + sleeve | **C** | Cuff matters (L04) |
| UI assets | Folder, Ledger, slips, stamps, icons | **C** | Vector sources |
| Materials | Travertine, oak, brass, linoleum, plaster, paper | **P** + **O** (ambientCG/Poly Haven CC0) | Custom wear masks |
| Textures | 1K–2K tiling; 2K hero | **C**/**O** | Compressed |
| Decals | Stains, scuffs, stamps, tally marks | **C** | Godot Decal nodes |
| VFX | Dust, paper, steam, shimmer | **P** (GPUParticles3D, shaders) | VFX_GUIDELINES |
| Animations | Props, hands, Echoes | **C** | ANIMATION_GUIDELINES |
| Lighting | Baked LightmapGI + probes | **C** | Per-level bake |
| Audio | See AUDIO_DESIGN | **C** + **B** | License ledger |
| Fonts | See VISUAL_STYLE_GUIDE | **O** (OFL) | Verify |

**Policy:** no asset enters the repo without a row in `ASSETS_LICENSES.csv` (columns: ASSET_PIPELINE §7). Placeholders carry prefix `TMP_` and are tracked until zero remain at M8.

## 3. Budgets (per level, High preset)

| Item | Budget |
|---|---|
| Visible triangles | ≤ 1.5 M |
| Unique materials | ≤ 60 |
| Draw calls | ≤ 1,200 |
| Texture memory | ≤ 2.5 GB VRAM total at High |
| Lights (dynamic) | ≤ 6 shadow-casting; rest baked |
| Hero prop | ≤ 25k tris |
| Set-dressing prop | ≤ 3k tris |

## 4. Modeling standards

- Real-world scale (1 unit = 1 m). 2 m grid for kit.
- Bevel all hard edges (normal bevel or 1–2 mm geometry on hero props).
- UV2 for lightmaps (no overlap; padding 4 px at 512 lightmap texel/m²).
- LODs: 3 levels for props > 1k tris; kit uses occlusion culling + distance fade.

## 5. Texturing standards

- PBR: albedo, normal (OpenGL +Y), ORM (occlusion/roughness/metal) packed.
- Tiling 1K for kit; unique 2K for hero.
- Wear: vertex colors / mask textures for touch-point wear.
- Text on props (slips, plaques): rendered from loc keys into `SubViewport` textures.

## 6. Environment art checklist per level

1. Greybox signed off (scale, sightlines, puzzle readability).
2. Kit dress pass (props in vignettes).
3. Wear/decals pass.
4. Lighting bake and probe pass.
5. Drift effect pass.
6. Perf pass (budgets).

## 7. Pre-production art deliverables

| Deliverable | Format |
|---|---|
| 3 mood frames | PNG/PSD |
| UI board | Vector |
| Kit module sheet | Diagram |
| Material library list | Table |
| Hero prop list (≈ 35) | Spreadsheet |
| Echo design sheet | Turnaround |

## 8. Acceptance criteria

- Any shot at 1080p has a clear focal point and no untextured/default-material surface.
- All art assets traceable to a license row.
