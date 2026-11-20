# UI_COMPONENT_SPEC

> **Status:** DESIGNED — specification only; not yet implemented in `game/`.

Numeric, buildable specification of the UI. **Design only; no code.** Authored at **1920×1080**. All values are tokens; nothing is eyeballed.

## 1. Foundations

### 1.1 Layout and scaling
| Item | Spec |
|---|---|
| Authoring resolution | 1920×1080, Godot stretch `canvas_items`, aspect `keep` |
| UI frame | Centered 16:9 frame; ultrawide/16:10 extend only the 3D view and backgrounds |
| Safe margins | **96 px** left/right, **54 px** top/bottom (5%) |
| Spacing base | **8 px**: `s1 8 · s2 16 · s3 24 · s4 32 · s6 48 · s8 64` |
| Panel padding | 32 px (40 px for Folder) |
| Item gap | 16 px; section gap 48 px |
| Hit target | ≥ 56 px high (min 48); buttons 64 px high, min width 240 px |
| Max text column | 960 px (settings), 720 px (reading), 1100 px (captions) |
| Text scale option | 100/125/150/200% multiplies Body, Caption, Labels; **not** Display |

### 1.2 Color tokens (UI)
| Token | Hex | Use |
|---|---|---|
| `tok_paper` | `#E8DFCC` | Panels, tags |
| `tok_paper_hi` | `#F4EEDF` | Important/selected paper, reading mode |
| `tok_ink` | `#14120F` | Text, rules, plate backgrounds |
| `tok_ink_40` / `_25` | ink @ 40% / 25% | Borders / separators |
| `tok_brass` | `#B08D4A` | **World-side** interactive cues, reticle on dark |
| `tok_brass_deep` | `#7A6233` | Focus outline & accents **on paper** (≈ 4.4:1 vs paper) |
| `tok_green` | `#3F5A4C` | Ledger headers, stationery details |
| `tok_oxblood` | `#6B1F26` | Stamps, destructive confirm, Proceedings |
| `tok_dust` | `#8C8A84` | Secondary text on dark, disabled |
| `tok_cyan` | `#5FA8A0` | **Never in UI chrome.** Only in in-world anomaly props |

Contrast (verify with tooling at M2): ink on paper ≈ 14:1; paper on 72% ink plate ≥ 11:1; brass on paper ≈ 2.4:1 → **never** used for text or focus on paper.

### 1.3 Type scale (1080p, 100%)
| Token | Face | Size / line | Tracking | Use |
|---|---|---|---|---|
| `type_display_xl` | Libre Caslon Display | 96 / 1.05 | +8%, caps | Title lockup |
| `type_display_l` | Libre Caslon Display | 64 / 1.10 | +6%, caps | Screen titles, endings |
| `type_h1` | Libre Caslon Text | 44 / 1.15 | +2% | Folder tab titles |
| `type_h2` | Libre Caslon Text | 32 / 1.20 | 0 | Section headers, object tags (focused) |
| `type_body` | IBM Plex Sans Regular | 24 / 1.35 | 0 | Settings rows, Ledger text |
| `type_body_s` | IBM Plex Sans Regular | 20 / 1.35 | 0 | **Minimum size**; hints, footers |
| `type_label` | IBM Plex Sans Medium | 20 / 1.20 | +10%, caps | Tabs, tags (unfocused), speaker labels |
| `type_caption` | IBM Plex Sans Regular | 32 / 1.30 | 0 | Captions (S 24 · **M 32** · L 40 · XL 52) |
| `type_mono` | IBM Plex Mono | 28 / 1.20 | 0 | Numbers, times, ticket numbers |
| `type_doc` | Courier Prime | 24 / 1.40 | 0 | Slips (in-world textures use their own scale, §5.8) |

Rules: max 2 typefaces per screen (+ Mono for numerals). No italics for body. No all-caps for sentences > 3 words. Optional dyslexia-friendly face (Atkinson Hyperlegible) swaps Plex Sans only.

### 1.4 Surfaces
| Property | Spec |
|---|---|
| Paper | `tok_paper` + fiber noise 3% + edge darkening 6% + subtle 4% vignette |
| Panel rotation | Deterministic per ID, ±0.5° (never on focused row text) |
| Corner radius | **≤ 2 px** |
| Border | 1 px `tok_ink_40` |
| Shadow | Contact `0 2 0 ink@25%` + soft `0 8 16 ink@18%`. No glow, no blur-glass, no gradients |
| Grain | Static film-grain overlay on all UI paper (not animated) |

### 1.5 Icons
24 px grid (also 32/48), **2 px stroke**, mitered corners, hand-drawn masters with ≤ 1° irregularity, single color (ink or paper). Set (v1): key, ear, eye, folder, drawer, stamp, ladder, clock, hand, magnifier, notebook, speaker, keyboard, controller, binder (settings), accommodation figure (accessibility), warning (hatch triangle), arrow, tick, cross. **No gear icon.**

## 2. Motion and sound tokens

| Token | Value | Use |
|---|---|---|
| `mo_micro` | 90 ms | Hover/focus micro-lift |
| `mo_fast` | 120 ms | Stamp impact, tick draw |
| `mo_base` | 200 ms | Panel exit, state changes |
| `mo_enter` | 280 ms | Panel/Folder entrance |
| `mo_page` | 350 ms | Page turn, Ledger page |
| `mo_ceremonial` | 600 ms | Ending card, act slips |
| `ease_enter` | ease-out-cubic | Entrances |
| `ease_exit` | ease-in-cubic | Exits |
| `ease_page` | ease-in-out-sine | Pages |
| `ease_settle` | cubic-bezier(.2,.8,.2,1), ≤ 3% overshoot | Paper settling |

**Rules:** no visible state change < 90 ms; pause-menu tweens use **unscaled time**; **Reduced Motion** → 150 ms cross-fade, no translation/scale/rotation; only one moving element per user action except page turns.

| Sound token (UI bus) | Trigger | Level |
|---|---|---|
| `ui_hover_paper` | Focus move | −30 dB, 40 ms |
| `ui_press_stamp_soft` | Button press | −18 dB |
| `ui_confirm_stamp` | Confirm/commit | −14 dB |
| `ui_back_slide` | Cancel/back | −20 dB |
| `ui_tab_flip` | Tab change | −22 dB |
| `ui_toggle_pen` | Toggle | −20 dB |
| `ui_slider_tick` | Every 5% | −28 dB |
| `ui_error_thunk` | Invalid | −16 dB |
| `ui_ledger_pencil` | Ledger tick | −20 dB |
Max 1 UI sound per 60 ms; pitch variance ±3%.

## 3. Universal state model

States: **Rest · Focus · Pressed · Selected · Disabled · Busy.** Mouse hover and controller/keyboard focus produce the **same** `Focus` visual (hover sets focus) — no dual highlight systems.

| State | Visual | Timing |
|---|---|---|
| Rest | Flat on surface, contact shadow 2 px | — |
| Focus | Lifts 3 px, shadow 5 px softer, brass-deep underline draws left→right, ink corner ticks | `mo_micro`, `ease_enter` |
| Pressed | Drops to −1 px (sinks), shadow 1 px, ink darkens 8% | `mo_micro` |
| Selected | Ink tick/stamp mark, `tok_paper_hi` surface | `mo_fast` |
| Disabled | Ink @ 35%, no lift, no sound, no focus | — |
| Busy | Perforation ticks advance (real progress) | — |

## 4. Component specs

### 4.1 Interaction reticle (in-world)
| Item | Spec |
|---|---|
| Rest | 4 px dot, `tok_paper` @ 45%, 1 px ink halo |
| Interactable focus | Dot scales 1→1.6 in `mo_micro`, turns `tok_brass`, soft 6 px halo |
| Hold-to-use | 28 px ring fills clockwise over hold duration (0.3 s mechanisms), no numbers |
| Wrong attempt | Ring snaps back in `mo_fast`, mechanism resists (see ANIMATION §9) |
| High-visibility option | 10 px dot + object outline 2 px `tok_paper_hi` |

### 4.2 Verb tag
Appears after 0.6 s continuous focus (or on Examine). 20 px label caps on a small paper tag (padding 8×12), offset 28 px below-right of reticle, fade in `mo_fast`, out `mo_micro`. Max 1 visible. Never shown for the same object twice within 10 s.

### 4.3 StampButton (tag-style button)
| Item | Spec |
|---|---|
| Anatomy | Paper tag, 64 px high, ≥ 240 px wide, 24 px horizontal padding, `type_label` or `type_body`, left-aligned ink text, punched hole on one end (cosmetic) |
| Focus | State model §3 |
| Press | Sink 1 px + ink mark flash (opacity 0→0.2→0) in `mo_fast`; sound `ui_press_stamp_soft` |
| Commit | On release; `ui_confirm_stamp` for primary actions |
| Destructive | Oxblood ink text + hatch underline; requires confirm form (§4.10) |
| Reduced Motion | No lift/sink; focus = underline + corner ticks + 150 ms fade |

### 4.4 Folder tab
Manila tab, 56 px high, label caps. Selected tab: merges into page (no bottom border), `tok_paper_hi`. Switch: content cross-slides 24 px, `mo_page`, `ui_tab_flip`. Controller: LB/RB. Max 5 tabs.

### 4.5 Toggle
Box 32×32 px (hit area 64×64), 2 px ink border. On: ink tick drawn along path over `mo_fast`; Off: empty. Label `type_body` left, state word (`ON`/`OFF`, `type_label`) right, always shown (not color-only). Sound `ui_toggle_pen`.

### 4.6 Slider (ruler)
Track: 8 px ruler line with tick marks every 10%; handle: 24×40 brass-deep slide gauge. Value in `type_mono` right-aligned in a 96 px field. Keyboard step 5% (hold = accelerate), controller D-pad step 5%, stick continuous. Tick sound every 5%. Min width 360 px. Drag has no inertia.

### 4.7 Selector (dropdown replacement)
Left/right arrows with current value centered (Resolution, Preset). Enter opens an **index-card fan** (max 8 items visible, scroll beyond). Open `mo_enter`, close `mo_base`. Current value ticked.

### 4.8 Keybind row
Label left, two key-cap slots right (glyph by device). Selecting a slot: typewriter caret blinks (1 Hz; static in Reduced Motion), text "Press a key…", 5 s timeout. Conflict → notice slip (§5.7) offering Swap / Cancel.

### 4.9 Slot card (Save/Load)
520×320 px folder card: top "photograph" 480×240 (desaturated last-room frame, 16:8 crop, white paper border 8 px), below: level name `type_h2`, in-game time `type_mono`, real date `type_body_s` @ 70%, ending flag icon (no spoiler text). Empty slot: blank card with typed "NO ENTRY". Selected card lifts 8 px.

### 4.10 Confirm form
Paper form 720×360 px, title `type_h1`, body `type_body`, two StampButtons. Destructive confirm requires **hold 0.6 s** on the stamp (ring fills). Default focus = Cancel.

### 4.11 Caption bar
See §5.6.

### 4.12 Progress strip
Ticket strip 640×72 px; perforation ticks (16 px pitch) fill left→right with **real** progress; indeterminate: ticks advance in groups per completed load step. No spinner, no percentage unless the user enables "Detailed loading".

## 5. Screen specs

### 5.1 Main menu — "The Intake Desk"
| Item | Spec |
|---|---|
| Scene | Live 3D desk viewed over the counter: camera pitch −35°, FOV 50°, shallow DOF; slow breathing sway ±0.3° @ 0.1 Hz (off in Reduced Motion); dust motes, ticking clock |
| Title lockup | `type_display_xl` as engraved plaque on the wall behind the Clerk's Window, upper-left safe area, subtle tungsten rim light. No tagline |
| Menu objects (6, distinct silhouettes) | **New Game** = ticket machine · **Continue** = filing drawer (opens 4 cm on focus) · **Settings** = *Regulations* binder · **Accessibility** = *Accommodations* form on clipboard · **Credits** = *Staff Register* · **Quit** = *Sign-out sheet* with pen on a string |
| Tags | Each object carries a paper tag: `type_label` at rest, `type_h2` when focused |
| Focus behavior | A desk lamp swings toward the focused object (240 ms), others dim 15%; tag lifts 3 px; small material sound of the object (drawer slide, paper rustle) |
| Navigation | Mouse hover/click on objects; D-pad/arrows move spatially (left/right order above; up/down wraps); Enter/A confirms |
| Idle | After 90 s, intercom hum rises slightly; no timeouts, no attract mode |
| Post-ending | Desk gains an ending-specific object (Amend: stamp lid closed; Affirm: ticket numbers subtly wrong [DUI-4]; Expunge: ash tray; Corroborate: seven envelopes aligned) |
| Simple menu option | Accessibility toggle: linear folder-style list for screen readers/low vision |
| Transition in | From black: 600 ms fade up, plaque lights 300 ms later, music cue |
| New Game transition | Ticket prints (900 ms), player "takes" it (camera push 300 ms), cut to L01 via paper-blink fade 400 ms |

### 5.2 The Folder (pause)
| Item | Spec |
|---|---|
| Open | Folder slides up from bottom, 1180×760 px, 280 ms `ease_enter` (unscaled); world desaturates 25%, DOF blur ramps in 240 ms, music low-pass 800 Hz in 240 ms |
| Tabs | Resume · Save/Load · Settings · Help · Title |
| Footer | `type_body_s` input glyph hints (e.g., `Esc Resume · Q/E Tabs`) |
| Close | 200 ms `ease_exit`; Esc/B anywhere in root closes |
| Help tab | Plain Courier card, labeled "OUT-OF-WORLD NOTE" — visually distinct, always truthful |

### 5.3 Settings — "Regulations Binder"
| Item | Spec |
|---|---|
| Layout | Binder pages with 5 tabs: Video, Audio, Controls, Gameplay, Language; accessibility lives in its own binder (main menu & Folder) |
| Rows | Height 64 px, separator 1 px ink@25%, label left `type_body`, control right; section header `type_h2` with 1 px rule |
| Footer | Selected row's description in `type_body_s` (no tooltips) |
| Apply | Immediate; display changes show 10 s revert slip |
| Reset | Per-section StampButton (confirm form) |

### 5.4 Save/Load
Three slot cards in a drawer view (3 × 520 px + 2 × 24 px gap, centered). Overwrite = confirm form requiring hold. Autosave slot shown as 4th narrow card (read-only label "AUTO"). Delete: secondary action (hold X / Del) with confirm form.

### 5.5 Loading
Prefer **diegetic transitions** (doors, lifts) with no UI. When data isn't ready: progress strip (§4.12) bottom-center over a darkened last frame; **minimum display 1.2 s** to avoid flash (skipped if load < 300 ms and the door animation covers it); fade 200 ms. Never shows fake progress or tips that lie (R9).

### 5.6 Dialogue and captions
| Item | Spec |
|---|---|
| Clerk (voice) | Intercom grille pulses warm in sync with amplitude (diegetic source cue); caption bar shows text |
| Caption bar | Max 1100 px wide, centered, bottom 96 px margin, padding 16×24, plate `tok_ink` @ 72% (user 0–100%), text `tok_paper`, 2 lines max, ~48 chars/line |
| Speaker label | `type_label` brass-on-ink above text, e.g. `CLERK` — **same style for every speaker** (no truth coding, R13); color-coding only as optional Accessibility toggle |
| Timing | Min 1.2 s, reading speed 15 cps (scaled with text size), fade in 120 ms / out 200 ms; no typewriter effect; phrase-synced |
| Sounds | `[a door opens, far away]` style, `type_body` italics **not** used, bracketed |
| Directional hint | `◂ ▸` glyph beside bracketed caption for off-screen sources |
| Slips | Read in Reading mode (§5.8) |
| L15 Hearing | Responses are **three paper cards** on the lectern (Affirm / Doubt / Silence), selected by looking and pressing; no dialogue list |

### 5.7 Notification system
| Priority | Purpose | Form | Duration | Blocking |
|---|---|---|---|---|
| P0 | Errors/data (save failed, corrupt file) | Notice of Difficulty slip (modal, 720×360) | Until dismissed | Yes |
| P1 | Safety (controller disconnected) | Notice slip + auto-pause | Until resolved | Yes |
| P2 | System feedback | **Autosave stamp** 48 px ink glyph, bottom-right safe corner | 1.2 s | No |
| P3 | Progress feedback | **Ledger tick**: 24 px graphite mark at lower-right + `ui_ledger_pencil` | 1.6 s | No |
Rules: max **1** visible; queue ≤ 2 (drop oldest P3); none during Examine/cinematics; 3 s cooldown between P2/P3; no stacking, no toasts, no achievement banners (platform handles achievements). Collectibles: envelope slides into the Ledger back pocket with a paper sound only.

### 5.8 Reading mode (slips, plaques, notes)
Trigger: Examine on text object. Camera eases to 40 cm, FOV −12°, 350 ms; world dims 15%. Text texture ≥ **9.5 px/mm** (2048 px across 210 mm); x-height ≥ 14 px on screen at 100%; Courier Prime (typed) / handwriting (marginalia); a **Zoom** action (+25%) is available. Exit: release or `cancel`, 200 ms.

### 5.9 Ledger
| Item | Spec |
|---|---|
| Open | Notebook rises into view (tilted ≈ 40°) then camera aligns so the page is **flat and pixel-aligned 1:1** by 350 ms; world dims 40% |
| Layout | Two pages 2 × 780 px, 48 px margins; left: sources & statements (statement chips); right: evidence & links |
| Statement chip | 56 px high, source icon + `type_body` text (max 2 lines), mark state: **Trust** (open tick), **Doubt** (open cross), none; marks are shape-coded, not color-coded |
| Evidence chip | Same size, location glyph + short label |
| Link thread | 2 px graphite line with slight sag, draws in `mo_fast`; styles: solid = supports, double-dash = conflicts (pattern, not color) |
| No verdicts | The Ledger never shows truth state; provenance stamps show *where/when* |
| Close | 280 ms reverse |

### 5.10 Ending card & credits
Ending card: 960×540 paper card, stamp lands (120 ms, scale 1.08→1, −2°), `type_display_l` ending name, 3 lines `type_body` summary. Credits: typed slips on a conveyor, 140 px/s, hold `interact` 1 s to skip. Fake credits (DUI-3): same system, ≤ 75 s, skip prompt appears at 3 s as paper corner "Hold to skip".

## 6. Navigation rules
- Every screen: focus order top→bottom, left→right; wrap on lists, no wrap on sliders.
- `cancel` always goes back one level; `pause` always closes to game.
- ≤ 5 interactive elements visible at once outside Settings.
- Mouse and controller can be mixed at any time; last-used device shows glyphs.

## 7. Implementation notes (design level)
- One master `Theme` resource generated from tokens; fallback to default Theme **fails CI**.
- Paper surfaces: `StyleBoxTexture` + paper shader (grain, edge darkening); text in `Label`/`RichTextLabel` with project fonts.
- Folder/Ledger/Settings are 2D `CanvasLayer` screens over the 3D scene; Menu objects are 3D with `Area3D` hit tests and 2D tags.
- Tweens use `process_mode = ALWAYS` and unscaled time while paused.
- Icons as SVG → pre-rasterized atlases at 24/32/48.
- All strings by localization key; allow +30% growth; test in German/Russian pseudo-loc.

## 8. Acceptance criteria
- Zero component deviates from tokens (automated token linter).
- Every component has Rest/Focus/Pressed/Selected/Disabled captured as a screenshot sheet at M2.
- UI motion spike runs at 60 FPS; each UI frame ≤ 0.8 ms.
- Text scale 100–200% never clips; focus never lost on controller.
- "Intentional" review (VISUAL_POLISH_CHECKLIST §4) passed for every screen.
