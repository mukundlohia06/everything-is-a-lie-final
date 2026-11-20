# VISUAL_STYLE_GUIDE

> **Status:** DESIGNED — specification only; not yet implemented in `game/`.

**Identity:** *Civic Modernism, Quietly Wrong.* A beautifully maintained 1960s institution where light is warm and exact — until it isn't.

## 1. Palette

| Role | Name | Hex | Use |
|---|---|---|---|
| Base light | Archive Paper | `#E8DFCC` | Paper, UI surfaces |
| Base dark | Ink Black | `#14120F` | Text, deep shadow |
| Accent 1 | Brass | `#B08D4A` | Interaction, reticle, trim |
| Accent 1 (on paper) | Brass Deep | `#7A6233` | Focus outline and accents on paper surfaces (≈ 4.4:1 vs paper); decorative/patinated brass |
| Highlight | Archive Paper Hi | `#F4EEDF` | Important/selected paper; brightest value in a frame |
| Clue marks | Chalk | `#DAD6C8` | Chalk and wear marks used as environmental clues |
| Accent 2 | Institutional Green | `#3F5A4C` | Records, lamps, stationery |
| Danger / contradiction | Oxblood | `#6B1F26` | Proceedings, stamps (SERVED) |
| Warm light | Tungsten | `#F2B563` | Lamps |
| Cool light | Fluorescent | `#C9D6D2` | Tubes, corridors |
| Neutral | Dust Grey | `#8C8A84` | Concrete, secondary text |
| Anomaly | Wrong Cyan | `#5FA8A0` | Only for "wrongness" cues (mirror cast, drift) |

**Rules:** warm = stable/known; cool-cyan = wrong/unverified. Wrong Cyan occupies ≤ 5% of any frame outside L09. Contrast ratios for UI text ≥ 7:1 on paper surfaces.

## 2. Typography

| Role | Typeface (verify licenses) | Notes |
|---|---|---|
| Display (titles, stamps) | Libre Caslon Display / Libre Caslon Text (OFL) | Caps with tracking +8% |
| UI body | IBM Plex Sans (OFL) | Sizes per the type scale in UI_COMPONENT_SPEC §1.3 (min 20 px @1080p) |
| Documents / slips | Courier Prime (OFL) | Typed-on-paper feel |
| Handwriting (marginalia) | Custom scan or Reenie Beanie (OFL) placeholder | Replace before release |
| Numerals | Plex Mono (OFL) | Ticket numbers |

Text scaling 100/125/150/200%. Min body size at 1080p 100% = 20 px.

## 3. UI language

- **Paper, stamps, ink, brass.** Rounded corners ≤ 2 px (paper edges), never glossy.
- Dividers: thin rules (1 px, 40% ink).
- Stamps: rotated −2° to +3°, slight ink noise (shader), 120 ms impact animation.
- Iconography: single-weight line icons (2 px), 24 px grid; subjects: key, ear, hourglass-less clock, eye, folder, stamp, ladder.
- Focus state: on paper surfaces a `brass_deep` (`#7A6233`) outline 2 px with 3 px offset plus ink corner ticks; in the world, polished brass (`#B08D4A`). Brass is never used for text.

## 4. Lighting philosophy

| Zone | Light | Notes |
|---|---|---|
| Normal (D0–D1) | Warm tungsten key + cool fluorescent fill, baked | Stable, readable shadows |
| Slip (D2) | Same lights; shadows 1–2° off from sources | Detect via reflection/shadow mismatch |
| Fray (D3) | Intensity flutter ≤ 2%, color temp drift ±150 K | Respect Reduced Flashing |
| Fold (D4) | Light sources inconsistent between adjacent rooms | Used sparingly |
| Collapse (D5) | Hard top light, paper particles | Finale only |

**Do not default to darkness.** Dark = narrative punctuation (Vault only). Contrast through *warm vs cool* and *clarity vs inconsistency*.

## 5. Material philosophy

Travertine (honed), oak veneer, brass (lacquered, worn at touch points), frosted glass, linoleum, painted steel, fabric (wool herringbone), paper. **Wear tells stories:** touch points worn, unused areas pristine.

## 6. Environmental style

- Strong axial compositions, symmetry broken by one object.
- Large, calm surfaces; props grouped in intentional vignettes.
- Scale: slightly oversized public spaces (ceilings 5–6 m) to feel institutional.

## 7. Character visual style (Echoes)

Tailored period clothing (muted), **blank paper-white faces** (smooth plane, no features), hands detailed. Heat-shimmer shader at edges. No horror contortion.

## 8. Camera

- FOV 75° default (user 60–100°). Eye height 1.62 m.
- Gentle head bob (off in Reduced Motion). Examine = FOV ease.
- No cinematic cuts during play; scripted moments use constrained look-around.

## 9. Post-processing (controlled)

| Effect | Setting | Notes |
|---|---|---|
| Tonemap | Filmic/ACES-like, exposure fixed per level | Avoid auto-exposure pumping |
| Bloom | Low, threshold high | Lamps only |
| SSAO | Medium | Contact grounding |
| Vignette | ≤ 12%, static | Unchanged under Reduced Motion (it does not move) |
| Film grain | 0.02, toggle | Static in Reduced Motion |
| Chromatic aberration | Only D3+, ≤ 0.5 px, toggle | Off by default in Accessibility preset |
| Color grading | One LUT per act | Subtle shifts across acts |

## 10. Scene composition checklist

- Silhouette readable at 25% size.
- One focal light.
- Interactables separated from background by value, not outline.
- At least one environmental clue visible from the room entry.

## 11. Look-dev targets (to produce in pre-production)

3 mood frames (Intake Hall, Mirror Gallery, Vault), 1 UI board (Folder, Ledger, slip), 1 typography sheet.

## 12. Acceptance criteria

- A new artist can build a prop that matches without asking about palette or wear.
- Screenshots contain no default Godot UI look (no default theme).

## 13. Color language, accent budget and anti-generic rules

- **Color language is normative** and lives in UI_UX_REVIEW §4: normal, anomaly, danger, important, interactive and clue states each have one signal. **False information has no color or style** (rule R13).
- **Bright brass = touch.** Decorative brass is patinated (`#7A6233`); at least 70% of brass in any room must be dim so polished brass stays a signal.
- **Accent budget:** brass + anomaly + danger ≤ 5% of any frame; neutrals ≈ 60%, warm materials/light ≈ 25%, institutional green ≈ 10%.
- **Wrong Cyan** never appears in UI chrome.
- **Type scale, spacing grid, safe margins, radii, shadows, icons:** see UI_COMPONENT_SPEC §1.
- **Anti-generic rules:** UI_UX_REVIEW §10. **Gate checklist:** VISUAL_POLISH_CHECKLIST.
- No screen-space glitch effects (RGB split, scanlines); distortion is physical (ANIMATION_GUIDELINES §9).
