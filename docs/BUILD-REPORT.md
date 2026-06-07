# AUMX-WEB-001 - Build Report

Governing instrument: AUMX-WEB-001 v1.0. Scope election: **reduced (build without deploy)** per
commit `51e0050` (PBHO confirmed by the Human Architect). This file is updated at every phase gate.

Deploy-artifact constraint (C-3, "single self-contained file") governs `index.html` only. Repo
tooling and docs (`docs/`, `fonts/` sources, `README`) are not part of the deploy artifact
(plan amendment 1).

---

## Phase status

| Phase | State | Notes |
|---|---|---|
| 1 Static composition | **CONFIRMED (sealed)** by Human Architect | discs, seats, tilt, wordmark, creed, mute geometry; rev 2 composition amendments |
| 2 Field | **CONFIRMED (sealed)** by Human Architect | particles, turbulence, coherence, seeded idle flicker |
| 3 Interaction | **CONFIRMED (sealed)** by Human Architect | state machine, proposal, ceremony drafting + burst, sealed persistence, coherence steps |
| 4 Audio engine | **BUILT - awaiting confirmation gate** | context, drone, quantized scheduler, stinger pool, voice mgmt, mute bind, dev placeholder tones |
| 5 Arc | not started | disc completions, finale, hold, scored reset |
| 6 Hardening | not started | touch, keyboard, reduced motion, performance, asset integration |

---

## Typeface

- Embedded as base64 `woff2` data URIs in `index.html` (regular 400 + bold 700).
- Verified genuine: internal name **"Space Mono Regular," Version 1.003, Colophon Foundry &
  Benjamin Critton**, SIL OFL. Source: `github.com/googlefonts/spacemono` `fonts/webfonts/`.
- Repo sources kept under `fonts/` (`spacemono.woff2`, `spacemono-bold.woff2`, `SpaceMono-OFL.txt`).
- Wordmark typeface is Space Mono for v1, **provisional** pending a canonical mark (plan amendment 3).
- NOTE: the earlier `fonts/` content on `origin/main` was IBM Plex Sans (wrong family, proportional,
  not used). Recommend removing those files from `main`; pending operator decision.

---

## Final tuned values (Phase 1) - chosen within confirmed ranges (P-1)

| Parameter | Range (brief) | Value | Source |
|---|---|---|---|
| Disc ellipse ratio (w:h) | 6:1 to 8:1 | **7:1** | `Config.ELLIPSE_RATIO` |
| Seat counts (bottom->top) | Guild 8 / Project 6 / Agenda 6 | 8 / 6 / 6 = 20 | `Config.DISCS` |
| devicePixelRatio cap | <= 2 | 2 | `Config.DPR_CAP` |
| PRNG | seeded, deterministic | mulberry32, seed `0x41554D58` ("AUMX") | `Config.SEED` |

> NOTE: original 3.1 placement/sizing values (right-of-center, single shared radius, symmetric
> vertical spacing) are **superseded** by the confirmed composition amendments below.

### CONFIRMED composition amendments to Section 3.1 (Human Architect, Phase 1 rev 2)

These supersede the original brief values. Density, seat treatment, and mute mark from rev 1 are
accepted and locked, carried forward unchanged.

| # | Amendment | Final value (P-1) | Source |
|---|---|---|---|
| 1 | Disc size hierarchy (GUILD largest -> AGENDA smallest), tuned toward canon | scale 1.00 / 0.70 / 0.47 (bottom->top) | `Config.DISCS[].scale` |
| 1 | Base (bottom-disc) horizontal radius | min(0.215 vw, 320px) | `Config.DISC_RX_VW` / `DISC_RX_MAX` |
| 1 | Seat size taper with disc | floor 0.64 + 0.36 x disc.scale, x foreshorten | `Config.SEAT_SCALE_FLOOR` |
| 2 | Stack centered horizontally (was 0.575 vw) | 0.50 vw | `Config.STACK_CX_FRAC` |
| 3 | Stack seated low; GUILD center | 0.82 vh | `Config.STACK_BOTTOM_FRAC` |
| 3 | AGENDA center (clears reserved upper third) | 0.46 vh | `Config.STACK_TOP_FRAC` |
| 3 | Reserved headroom for future brain/node-graph element (NOT built this brief) | ~ upper third of viewport | composition reserve |
| 3 | Central spine extended up into headroom (fainter, dotted) to imply what arrives | axis top at 0.07 vh, opacity 0.05, dash "1 7" | `Config.HEADROOM_AXIS_TOP` / `C_AXIS_HEAD_O` |

### Phase 1 revision (canon density pass) - tuned values (P-1)

Per operator review against the canonical AXIS/BRAIN visual: geometric vocabulary extracted
(concentric rings, radials, inner lattice, outlined hexes with inner hex, central spine). Luminance,
camera, brain galaxy, and HUD deliberately NOT extracted - idle stays dim line work, no glow, no bloom.

| Parameter | Value | Source |
|---|---|---|
| Disc concentric rings | 7 rings at u = 0.16 / 0.30 / 0.46 / 0.62 / 0.80 / 0.93 / 1.00 | `Config.DISC_RINGS` |
| Ring stroke weights | 0.5 - 1.2 px (hairline, varied) | `Config.DISC_RINGS[].w` |
| Ring opacities | 0.05 - 0.13 (dim) | `Config.DISC_RINGS[].o` |
| Hairline radials | 36 spokes, u 0.16 -> 1.00, opacity 0.05 | `Config.DISC_RADIAL_*` |
| Inner lattice | 18-point woven star polygon, skip 7, at u 0.58, opacity 0.06 | `Config.DISC_LATTICE_*` |
| Seat ring radius | 0.86 x disc rx | `Config.SEAT_RING_U` |
| Seat outer-hex radius | 13 px (instrument-sized) x foreshorten | `Config.SEAT_HEX_BASE` |
| Seat inner-hex ratio | 0.52 x outer | `Config.SEAT_INNER_RATIO` |
| Seat foreshorten scale | 0.74 (back) -> 1.18 (front) | `Config.SEAT_FORE_MIN/MAX` |
| Seat hex squash | 0.54 (sits on tilted face) | `Config.SEAT_HEX_SQUASH` |
| Seat idle outline opacity | 0.26 - 0.40, seeded variance | `Config.SEAT_O_OUTER_MIN/MAX` |
| Seat inner-hex opacity | 0.55 x outer | `Config.SEAT_O_INNER` |
| Central axis opacity | 0.10 | `Config.C_AXIS_O` |

Palette (C-1, pure monochrome): all structure is white (`#ffffff`) line work dimmed by opacity over
`#000000`; grey falloff via opacity only. No fills on discs or seats (outline only). No hue anywhere.

Mute mark: redrawn as an outlined hexagon in the seat-hex register (C-6). Live = small center node;
muted = diagonal datum strike. Speaker glyph removed.

---

## Phase 2 - field and idle life (brief 3.2 / 3.3) - tuned values (P-1)

Animated idle state. Single `requestAnimationFrame` loop driven by elapsed time from load, so the
sequence is identical on every load (restarts at T=0). Pauses on hidden tab. No `Math.random`
anywhere (C-5). Reserved headroom stays empty - field region sits in the lower frame only.

### Field (Canvas 2D, brief 3.3)

| Parameter | Value | Source |
|---|---|---|
| Particle count | 3200 desktop target, scaled by `min(1, max(0.6, w/1440))` | `Config.FIELD_COUNT` (range 2500-4000) |
| Thread bands | 4 | `Config.FIELD_BANDS` (range 3-5) |
| Band region (vh) | 0.42 -> 0.93 (lower frame; clears headroom) | `Config.FIELD_TOP/BOTTOM_FRAC` |
| In-lane scatter | 0.34 x lane gap (band thickness) | `Config.FIELD_BAND_SPREAD` |
| Flow speed | 26 px/s base, +/- 45% seeded, left -> right | `Config.FIELD_SPEED` / `_VAR` |
| Turbulence amplitude | 46 px (upstream) | `Config.FIELD_AMP` |
| Noise | seeded 2D simplex; freq x 0.0016 (+octave 0.0041), y 0.010, drift 0.055/s | `Config.NOISE_*` |
| Idle coherence | scalar 0 (idle); downstream baseline 0.06 via smoothstep gradient | `Field.coherence` / `Config.COH_IDLE_BASE` |
| Particle render | white, `globalCompositeOperation: "lighter"`, alpha ~0.34, 1.25 px | `Config.FIELD_DOT_*` |
| Noise seed | `(SEED ^ 0x1f83)`; particle stream `(SEED + 0x1f83)` | `Config.FIELD_SEED_OFFSET` |

Coherence model: displacement = `AMP x (1 - coherence(x)) x simplex`. `coherence(x)` rises downstream;
the `Field.coherence` scalar stays 0 at idle and is the seam Phase 3 raises per sealed seat/disc.

### Idle flicker (seats only; discs static line work, brief 3.2)

| Parameter | Value | Source |
|---|---|---|
| Breathing depth | 0.34 of base opacity | `Config.FLICKER_DEPTH` |
| Breathing rate | 0.62 rad/s base, +/- 50% seeded (low freq, instrument-like) | `Config.FLICKER_RATE` / `_VAR` |
| Per-seat phase/rate | seeded mulberry32 (fold of the layout stream) | `Geometry._seats` |
| Coordinated disc pulse | period 13-24 s seeded, dur 1.7 s, raised-cosine, gain 0.5 | `Config.PULSE_*` |

All flicker is a deterministic function of seeded params + elapsed time, so it is identical on every
load. Discs never move or pulse as bodies; only seats carry light.

### Reduced motion / performance

- `prefers-reduced-motion`: no loop; composition static, seats dim-lit, field shows obsidian + faint
  band only (no particles).
- Hidden tab: loop paused (visibilitychange); dt capped at 50 ms to avoid jumps on resume.
- Viewport-width particle scaling in place; full adaptive downscale-after-budget lands in Phase 6.

---

## Phase 3 - interaction (brief 3.4 / 3.5) - tuned values (P-1)

State machine per seat: `idle -> proposed -> ceremony -> sealed`; disc state `complete`. Hover is
proposal, the click is the only governed act (C-4: drafting is REGISTRATION, never deliberation - no
element evaluates, scores, or approves the human). Anti-retrigger is enforced by the state machine,
not a debounce: `ceremony` and `sealed` seats ignore all input, so rapid clicks yield exactly one
ceremony. Audio is Phase 4 - ceremonies render silent for now; the `Clock.nextBurst()` seam is shaped
to accept the Phase-4 quantized AudioContext scheduler (one-shots to the next half-bar/bar).

| Parameter | Value | Source |
|---|---|---|
| Draft duration | 1.75 s (range 1.5-2.0; elastic to grid in Phase 4) | `Config.DRAFT_DURATION` |
| Burst / register-flicker | 0.42 s envelope | `Config.BURST_DURATION` |
| Proposal thread convergence | radius 140 px, pull 0.85, flow hold 0.85 | `Config.PROPOSE_*` |
| Coherence step per sealed seat | +0.025 | `Config.COH_SEAT_STEP` |
| Coherence snap per completed disc | +0.08 | `Config.COH_DISC_SNAP` |
| Coherence cap pre-finale | 0.82 (Phase 5 finale -> full); eased at 6/s | `Config.COH_MAX_PREFINALE` / `COH_EASE` |
| Seal radius | 0.94 x seat outer-hex radius | `Config.SEAL_R` |
| Hover label | Space Mono 11 px, uppercase, opacity 0.72, 16 px above seat | `Config.LABEL_*` |
| Disc-complete trace | 1.1 s circumference draw, settles to opacity 0.18 | `Config.DISC_TRACE_DUR` |

Coherence math check (headless sim): 20 seats x 0.025 + 3 discs x 0.08 = 0.74 target (under the 0.82
pre-finale cap), leaving room for the Phase 5 finale to take it to full. One disc complete = 0.230,
verified.

Ceremony / seal: on click the seat locks to full luminance and input closes; the provisional seal
drafts via SVG `stroke-dashoffset` (set-out datum lines -> compass circle -> compass arc -> Datum
Cross), staggered, over the draft duration; at the scheduled burst time the seat register-flickers,
the seal settles as a permanent mark, and downstream threads snap a coherence step. When all seats on
a disc are sealed, the disc ellipse traces its circumference once and a disc seal registers on the
central axis. Seals persist for the session and are restored across a layout rebuild (resize) via
`Session` + `Restore` (verified 6/6 in the headless sim).

Deferred to Phase 6 (per build sequence): keyboard focus order + Enter, and the touch two-step
(first tap proposes, second confirms). Reduced motion currently seals immediately (cross-fade)
rather than drafting; the full reduced-motion ceremony cross-fade is a Phase 6 hardening item.

Validation: `node --check` clean; headless DOM-stub simulation passes full agenda-disc completion,
rapid-click -> exactly one ceremony, coherence = 0.230 after one disc, and 6/6 seal restore after a
simulated resize.

---

## Provisional seal geometry (P-3 / OI-2)

- **Drawn as of Phase 3, PROVISIONAL.** Seat seal = set-out datum construction lines + compass circle
  + compass arc + **Datum Cross** (the only confirmed canonical mark). Disc seal (on the central axis)
  = squashed ring + Datum Cross. All flagged provisional in code and here; final per-disc canonical
  geometry awaits the OI-2 design pass. The central vertical **axis column** remains the line the disc
  seals register onto; each seat's **inner hex** hosts its seal draft.

---

## Phase 4 - audio engine (brief 4.1 / 4.2 / 4.4) - BUILT, silent by default

Separable module `Audio` (C-7, forkable for SS-BOOM-001). One `AudioContext`, **suspended until the
first user gesture** (4.4) - armed on the first pointer/keydown (capture phase, so even the first
ceremony is grid-aligned) and on any seat press / mute toggle. **Silent by default**: real assets are
OI-3 (NOT delivered), used if present, otherwise the engine stays armed and silent.

**Quantized scheduler (4.2):** the `Clock.nextBurst()` seam now resolves against the AudioContext
clock. Each burst is pulled to the next **half-bar** boundary (`bar / AUDIO_QUANTIZE_DIV`) and the
stinger is scheduled there; the returned time drives the visual draft so the burst lands exactly on
the beat. The finale path (`scheduleFinale`) aligns on the next **full bar** (Phase 5 wires the visual
finale to it). The grid is anchored at drone start; timing is identical whether or not sound is audible.

**Stinger pool / voice management (4.1):** round-robin over the pool with **no immediate repeat**, each
voice through its own gain into master so overlaps mix, **polyphony cap 6** (oldest voice gain-faded).
Anti-retrigger remains enforced by the Phase-3 state machine (one ceremony -> one burst -> one stinger).
Optional **drone duck** (master drone dips to `AUDIO_DUCK` then releases) while a stinger rings.

**Mute (4.4):** the drawn geometric toggle binds to the **master gain** (ramped to 0 / back), held in
memory only (no storage APIs, C-3).

**Disc-complete voice (brief 4.3 / amendment 2):** `Audio.disc()` fires on disc completion, quantized
to the next full bar; plays `/audio/disc-complete.*` if present, the placeholder swell under the dev
flag, or silence.

### Audio constants

`AUDIO_BPM` / `AUDIO_BEATS_PER_BAR` / key are **placeholder grid values for review** - the real values
are declared on drone delivery (OI-3) and the drone defines the true tempo/key the grid locks to.

| Constant | Value | Status / source |
|---|---|---|
| BPM | 72 (placeholder) | `Config.AUDIO_BPM`; real value on drone delivery (OI-3) |
| Bar length (beats) | 4 (placeholder) | `Config.AUDIO_BEATS_PER_BAR`; real value on drone delivery |
| Key | A (placeholder drone root A1=55Hz) | placeholder only; real key on drone delivery |
| Quantize (one-shots) | next half-bar | `Config.AUDIO_QUANTIZE_DIV = 2`, per brief 4.2 |
| Quantize (finale / disc voice) | next full bar | `scheduleFinale` / `Audio.disc`, per brief 4.2 |
| Polyphony cap | 6 | `Config.AUDIO_POLYPHONY`, per brief 4.1 |
| Stinger pool size | 5 | `Config.AUDIO_STINGERS` (range 3-6, brief 4.1) |
| Master gain (unmuted) | 0.5 | `Config.AUDIO_MASTER` |
| Drone duck / release | x0.7 / 0.45 s | `Config.AUDIO_DUCK` / `AUDIO_DUCK_REL` |

### Placeholder audio flag (P-2)

- `Config.DEV_PLACEHOLDER_AUDIO` = **`false`** (committed state). MUST stay OFF for any deploy.
- When ON: synthesized placeholder tones (clearly NOT canon) - a continuous root+fifth sine drone, a
  triangle-bell stinger pool (pentatonic A C D E G), and a root+fifth+octave disc swell - so the
  quantized timing is demonstrable in review without assets.
- When OFF (default) and no `/audio/` assets: engine is armed and **silent**, grid timing unchanged.

### Asset hook (OI-3)

On unlock the engine probes `audio/drone.*`, `audio/stinger-1..5.*`, `audio/disc-complete.*`
(`webm/mp3/ogg/wav`); any found are decoded and used, the drone loops gaplessly. Absent assets fail
silently. (Under `file://`, fetch is blocked, so review uses the placeholder flag; a deployed host
serves the assets normally.)

Validation (headless Web Audio stub): half-bar grid math, `nextDivision` boundaries, burst on-grid and
>= now + draft duration with the visual time tracking the ctx delta, round-robin with no immediate
repeat across the whole pool, polyphony cap saturating at 6, and the silent path still grid-aligned.
End-to-end interaction+audio sim seals a full disc with grid-timed bursts and no exceptions.

---

## Acceptance criteria (brief 8) - tracking

- [ ] Idle identical on every load (deterministic) - PRNG seeded; verify on reload (Phase 2 flicker pending)
- [x] Hover proposes / exit reverts (Phase 3) - verified in headless sim
- [x] Rapid clicks -> one ceremony, one stinger (Phase 3 state machine + Phase 4 audio) - verified in sim
- [x] Bursts land on quantized boundary (Phase 4) - half-bar grid verified in headless audio sim
- [ ] Full 20-seat arc -> 3 disc completions -> finale -> scored reset (Phase 5)
- [x] Zero network requests beyond the file and `/audio/` (no CDN / third-party; font embedded)
- [x] Standing text = wordmark + the one sentence, nothing else
- [ ] Usable muted / keyboard-only / reduced motion (Phase 6)
