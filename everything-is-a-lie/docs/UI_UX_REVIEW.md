# UI_UX_REVIEW

> **Status:** DESIGNED — specification only; not yet implemented in `game/`.

Senior UI/UX + art direction review of the full specification (33 documents, v1.0). Companion documents: **UI_COMPONENT_SPEC.md** (components, tokens), **CINEMATIC_DIRECTION.md** (camera, reveals), **VISUAL_POLISH_CHECKLIST.md** (gates).

> **Quality bar applied to every item:** *"Would this look intentional in a commercially released indie game?"* If no → redesigned below.

---

## 1. Verdict

The v1.0 concept is strong: "paper, stamps, brass, civic modernism" is a **specific** identity, not a template. The risk is not the idea; it is **execution drift** into the failure modes you listed. A document cannot guarantee polish, so this review does three things: (a) fixes spec defects, (b) turns principles into numbers an artist/dev can obey, (c) adds gates (look-dev keyframes, UI motion spike, polish checklist) that catch generic output *before* it is built at scale.

## 2. What is already working (keep)

| Area | Why it works |
|---|---|
| Single metaphor (the Concord's stationery) | One coherent source for menus, notifications, HUD, loading |
| No objective HUD | Forces environmental communication |
| Warm = known / cool-cyan = wrong | Simple, learnable, cheap |
| Wear tells stories (brass worn at touch points) | Natural interaction affordance without UI |
| Reduced Motion/Flashing variants specified | Polish and accessibility together |
| Echoes: blank paper-white faces | Distinctive, avoids horror cliché |

## 3. Findings and resolutions

| # | Sev | Where | Problem | Resolution | Patched in |
|---|---|---|---|---|---|
| F1 | **Critical** | Color brief ("false information" color) | Giving false information a color/style would **leak truth** and destroy the game | **False information has no color and no style.** Truthful and deceptive carriers are styled identically (new rule **R13**). Falsity is discoverable only via evidence and source voice tells | §4, TRUTH_LIE_SYSTEM R13 |
| F2 | High | UI_UX_DESIGN §3 main menu | "Stamped forms on a desk" risks becoming a stack of identical buttons | Menu is **six distinct desk objects**, each with its own silhouette, light response, and tag (UI_COMPONENT_SPEC §5.1) | UI_UX_DESIGN §10 |
| F3 | High | Whole spec | **No notification system** | Defined: 4 priorities, max 1 visible, no popups/toasts (UI_COMPONENT_SPEC §5.7) | UI_UX_DESIGN §10 |
| F4 | High | VISUAL_STYLE / ACCESSIBILITY | Font sizes scattered (20/24/28 vs 24–52); no spacing/margin system | Single **type scale, spacing grid, safe area** (UI_COMPONENT_SPEC §1) | VISUAL_STYLE_GUIDE |
| F5 | High | Focus/interaction color | Brass `#B08D4A` on paper is ≈ 2.4:1 — **fails 3:1 non-text contrast** | Brass is world-side only; on paper surfaces use `brass_deep #7A6233` (≈ 4.4:1) + ink corner ticks | UI_COMPONENT_SPEC §1.2 |
| F6 | Med | UI_UX_DESIGN §5 Ledger | World-quad SubViewport at 40° tilt → blurry text at 1080p | Camera eases until notebook is **pixel-aligned 1:1** (final state = flat), tilt is only transitional | UI_COMPONENT_SPEC §5.9 |
| F7 | Med | VISUAL_STYLE §9 | Ambiguous table row ("Drops with Reduced Motion? No—static OK") | Clarified: static vignette unchanged | VISUAL_STYLE_GUIDE |
| F8 | Med | Slips/plaques | Text-in-texture can be soft | Slip texture density ≥ **9.5 px/mm** (2048 px across 210 mm); x-height ≥ 14 px on screen in Examine | UI_COMPONENT_SPEC §5.8 |
| F9 | Med | Environments | "Focal point, clue, landmark" stated but not per room | Per-level composition table (§7) | LEVEL_DESIGN §6 |
| F10 | Med | Cinematics | Camera/reveals underspecified | CINEMATIC_DIRECTION | — |
| F11 | Med | "Glitches" | Screen-space glitch effects are the #1 generic tell | **No screen-space glitch.** Distortion is physical (geometry, light, reflection, audio, paper) with a per-level budget | ANIMATION_GUIDELINES §9 |
| F12 | Med | Echoes | Blank-faced mannequins can read cheap/uncanny | Hand/posture/clothing-wear detail, folded-paper crease on faces, heat shimmer, **no walk cycles** | ART_DIRECTION (via this doc §10) |
| F13 | Low | Ultrawide | Not specified | UI in centered 16:9 frame; 3D fills; cinematic bars only in endings | UI_COMPONENT_SPEC §1.1 |
| F14 | Low | Clerk's Window | Clerk is voice-only; window risks being an empty prop | Silhouette behind frosted glass; moves only when unobserved at D2+ (never faces blank) | §7 L01 |

## 4. Color language (normative)

**Budget by screen area (typical frame):** neutrals 60% · warm materials/light 25% · institutional green/secondary 10% · accents (brass, anomaly, danger) **≤ 5% combined**.

| State | Signal | Hex | Rule |
|---|---|---|---|
| **Normal** | Warm tungsten + paper + oak | `#F2B563`, `#E8DFCC` | The "home" temperature of the game |
| **Anomaly** | Wrong Cyan, **world only** | `#5FA8A0` | ≤ 5% of frame; appears in reflections, drift light cast; **never in UI chrome** |
| **Danger / consequence** | Oxblood **+ ink hazard hatch** (pattern, not just hue) | `#6B1F26` | Stamps (SERVED), heat/steam labels, Proceedings. Rare. Colorblind-safe because of hatch |
| **False information** | **None. Identical styling to truthful information.** | — | R13. Never encode truth in color, font, animation, or sound design of the carrier |
| **Important information** | **Value, not hue**: brightest paper `#F4EEDF`, lit pool, typed underline | `#F4EEDF` | Highest luminance in frame; plus composition (centered, light-isolated) |
| **Interactive objects** | **Polished brass** + specular glint + worn touch points | `#B08D4A` | **Bright brass = touch.** Decorative brass is patinated `#7A6233` (≥ 70% of brass in a room must be non-interactive-dim, so bright brass stays a signal) |
| **Environmental clues** | Wear, chalk `#DAD6C8`, dust drift, light pools | — | Clues never use a unique hue; they use value contrast, wear and motion |

**Brass discipline test:** desaturate a screenshot and mask everything except bright brass. Everything left should be interactable or a deliberate decoy (max 1 decoy per room, and decoys must be *explainable* by evidence).

## 5. Diegetic audit (what lives where)

| Information | v1.0 | Recommendation | Class |
|---|---|---|---|
| Objectives | World | Keep. Add **light-pool emphasis** after stalls (true) | Diegetic |
| Time | None | Wristwatch glance (hold `wait` shows watch) and room clocks | Diegetic |
| Statement delivery | Intercom/slips/plaques | Intercom grille has a faint warm pulse synced to voice → source is visible | Diegetic |
| Interaction prompt | Dot + delayed verb | Keep; verb label appears as **typeset tag**, not UI text | Hybrid |
| Hint (Assist) | Pause menu | Keep out-of-world, plain, different type (Courier on plain card) | Non-diegetic (honest) |
| Autosave | Stamp glyph | Small ink stamp bottom-right, 1.2 s | Hybrid |
| Ledger update | none | Graphite tick + pencil sound (§UI_COMPONENT_SPEC 5.7) | Hybrid |
| Door state | World | Latch animation + sound; no icons | Diegetic |
| Power state (L13) | Panel | In-world lamps/needles | Deceptive diegetic |
| Collectibles | None | Envelope slides into Ledger back pocket; no popup | Diegetic |
| Loading | Strip | Truthful progress only | Hybrid |
| Controller disconnect | Slip | Notice slip, pauses game | Non-diegetic honest |
| Credits | Conveyor | Typed slips | Diegetic |

## 6. Deceptive UI register (strategic, finite)

Deceptive **UI** (not statements — those live in the Truth system) is limited to **four** registered instances. Each must pass: never touches function, never in Settings/Pause/Save/Captions, has an evidence-based tell, has a Reduced Motion/Flashing variant, fires once per level at most.

| ID | Where | Deception | Tell (evidence) |
|---|---|---|---|
| DUI-1 | L13 breaker panel | Indicator lamps lag one step | Transformer hum pitch, needle vibration |
| DUI-2 | L10 display cases | One habit statement about *you* is false | Your own memory of play |
| DUI-3 | L17 fake credits | Looks like the end | Painted sky edge; road loops; Clerk's true line |
| DUI-4 | Title screen after *Affirm* | Ticket number subtly different | Compare with L01 ticket (4471) |

Anything else that "looks like UI lying" must be re-classified as an in-world statement or removed.

## 7. Environment composition (per level)

**Room template (mandatory):** 1 focal point · 2–3 secondary points · 1 visual landmark · 1 interactive cluster · ≥ 2 sound sources · lighting hierarchy (key > fill > accent) · 1 human trace · 1 institutional trace · 1 wrongness trace (WORLD_BUILDING §8).

| Level | Focal point | Secondary points | Landmark | Sound sources | Lighting hierarchy |
|---|---|---|---|---|---|
| L01 Intake | Clerk's Window (grille, silhouette behind frosted glass) | Ticket machine, 3 lamps, plaque | Wall clock | Clock, intercom hum, bell | Key: tungsten pool at window · Fill: fluorescent hall · Accent: unlit lamps |
| L02 Waiting | Noticeboard (single lit tube) | Two doors, worn chair, cups | Row of 18 chairs | Typing, off-beat clock | Flat fluorescent · tungsten spill from left door |
| L03 Corridor | Frosted end wall, glowing | Six doors, floor tile pattern, streamers | Tally-scratched door D | Streamer rustle, far door | Receding tube rhythm · cool at end |
| L04 Coat Check | Hook wall | Chalk ledger, UV lamp, pinned note | Hook 12 brass tag | Hangers, fabric | Counter lamp key · rail soft fill |
| L05 Records | Card catalog bank | Ladder, rolling stacks, mezzanine hatch | Banker lamp on mezzanine edge | Roller creaks, paper | Skylight shaft with dust · lamp accents |
| L06 Stairwell | Newel post with tally | Plate number, paint coats, shutter | Wall stain | Footfall reverb shifts | Single window shaft, repeating |
| L07 Atrium | Fountain | Pipes, glass roof, bench | Brass key glint in basin | Water, pipe knock, hiss | Top daylight · warm reflections |
| L08 Switchboard | Board face with jacks | Radio, headset, rear panel | Lamp bank | Radio crackle, relays | Board self-lit in dim room |
| L09 Mirrors | Mirror wall | Floor tiles, door seen in mirror | Tall oval mirror | Faint crack tick | Even soft · cyan cast in reflections only |
| L10 Observation | Diorama under glass | Four cases, rail, tiny figure | Diorama clock | Soft tick per trait | Top-lit diorama · dim gallery |
| L11 Ledger Room | Filing wall (12 cards) | Card tables, tube terminal | Glowing linked cards | Card shuffles, tube thump | Green lamp pools |
| L12 Refectory | Long table, three Echoes | Place cards, clock, window | The clock | Cutlery (slightly out of sync) | Pendant lamps over table · dark edges |
| L13 Maintenance | Breaker panel | Transformer, pipes, needles | Oxblood main switch | Transformer hum | Caged work lamps |
| L14 Long Hallway | Vanishing-point door | Ceiling lamps, plaques, floor stripe | One cracked tile | Footstep reverb | Lamp rhythm · dead spot has still dust |
| L15 Hearing | Lectern and bench | Evidence trays, gallery benches | Oxblood curtain | Gavel, wood creak | Window slant across floor |
| L16 Projection | Beam and screen | Film cans, frame table | Dust in beam | Projector | Beam is key light |
| L17 Exit | Dawn road, horizon | Building rear, lamppost | The Concord behind you | Wind | Dawn gold · long shadows · flat sky edge |
| L18 Vault | Original ledger under lamp | Shelves, UV lamp, transparencies | Vault door | Low hum | Single amber lamp, dark |
| L19 Reading | Dictaphone | Window reflection of player, envelopes | The window | Tape hiss | Lamp + dusk window |
| L20 Verdict | Three lecterns | Inscribed names, great clock | Great clock | Low room tone | Central shaft |

## 8. Visual storytelling matrix

| Channel | What it communicates | Concrete examples |
|---|---|---|
| Lighting | Attention, safety, wrongness | Pool on the useful object; shadow angle off by 1–2° in D2 |
| Composition | Hierarchy, suspicion | Axial hall with one broken symmetry (L01 clock, L02 chair) |
| Object placement | Recent human activity | Cup rings, chair pulled out, ladder foot-scuffs |
| Environmental change | Consequence | Hooks 40→39 (L04); Echoes' lights warm (L12) |
| Sound | Off-screen truth | L03 far door; L13 hum |
| Animation | Weight, intent | Latch resistance on wrong door |
| Color | Category (see §4) | Brass = touch; cyan = wrong |
| Shadows | Hidden state | Missing shadow under an Echo's chair (D3) |
| Reflections | Honest or wrong state | L09 cracks; D2 mirror door open |
| Architecture | Institution's mood | Oversized public spaces; low, compressed Vault |

## 9. Animation principles (summary)

Full numeric tables: UI_COMPONENT_SPEC §2, ANIMATION_GUIDELINES §9.

1. **Weight before speed:** heavy things start slow, settle with a small secondary motion.
2. **Anticipate → act → settle** for every mechanism and UI component.
3. **No instant popups.** Minimum visible transition 90 ms; reduced-motion = cross-fade.
4. **Telegraph changes:** world changes happen unseen *or* are telegraphed (0.5–2 s of sound/light) — otherwise they violate R7.
5. **Restraint:** at most one non-ambient animated element competing for attention.

## 10. Anti-generic rules (design guardrails)

| Generic tell | Rule that prevents it |
|---|---|
| Default Godot theme / rounded gray buttons | Zero default Theme usage; CI check for `Theme` fallback |
| Vertical stack of identical buttons | Menu = distinct objects; Folder = tabs; Settings = binder pages |
| Floating glass panels, gradients, glow | Paper only; no blur-glass; shadows are contact + soft, no outer glow |
| Neon/cyberpunk accents | Palette-locked; cyan only in world and ≤ 5% |
| Screen-space glitch / RGB split | Banned; physical distortion only |
| Perfect clean surfaces | Wear mask on every material; 3 decals minimum per room |
| Repeated visible tiling | Trim sheets + decal breakup + vertex-color variation |
| Over-bloom, crushed blacks | Bloom threshold high; exposure fixed per level; blacks ≥ `#0E0D0B` |
| Text-heavy overlays | Text budget: ≤ 2 UI text elements simultaneously on screen during play |
| Inconsistent corner radii/strokes | Tokens only (UI_COMPONENT_SPEC §1) |
| Stock-looking props | Hero prop silhouette review; every room has 1 bespoke prop |
| Mannequin Echoes | Hands, posture, wear, shimmer; no walk cycles; paper crease on faces |

## 11. Process to protect the bar

1. **Look-dev keyframes** (3 paintovers: Intake Hall, Mirror Gallery, Vault) approved *before* kit modeling (T-P1-18).
2. **UI motion spike** in Godot: StampButton, Folder open, Ledger open, caption bar, loading strip, 60 FPS (T-P1-17).
3. **Polish gates** at M1, M2, M6 using VISUAL_POLISH_CHECKLIST (target ≥ 85% score, no zeroes in Critical items).
4. Weekly **screenshot review** (10 random frames, grayscale + thumbnail tests).

## 12. Patch log (existing documents updated in this pass)

VISUAL_STYLE_GUIDE (F4, F7, color/anti-generic pointers) · UI_UX_DESIGN (§10 amendments) · ANIMATION_GUIDELINES (§9) · TRUTH_LIE_SYSTEM (R13, R14) · QA_TESTING (lint rows) · LEVEL_DESIGN (§6) · TASK_BREAKDOWN (T-P1-17/18, T-P2-21..23) · MILESTONES (art gates) · NAMING_CONVENTIONS (tokens, DUI IDs) · README (doc map) · CHANGELOG (0.0.2).

## 13. Residual risks

| Risk | Mitigation |
|---|---|
| Small team cannot hit per-level composition | Room families + wear library; cut-line L10/L14 |
| Paper UI becomes skeuomorphic clutter | Restraint rules: ≤ 5 interactive elements per screen, 1 accent per view |
| Diegetic notifications missed by players | Audio cue always accompanies; Assist "High-visibility" option |
| Look-dev not reproducible in-engine | Keyframes judged against in-engine S1 spike capture |
