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
| 1 Static composition | COMPLETE (awaiting operator confirmation) | discs, seats, tilt, wordmark, creed, mute geometry |
| 2 Field | not started | particles, turbulence, coherence, seeded idle flicker |
| 3 Interaction | not started | state machine, proposal, ceremony, sealed persistence |
| 4 Audio engine | not started | context, drone, quantized scheduler, stinger pool, voice mgmt, mute bind |
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
| Disc horizontal radius | tune | min(0.255 vw, 360px) | `Config.DISC_RX_VW` / `DISC_RX_MAX` |
| Stack center x | right of center | 0.575 vw | `Config.STACK_CX_FRAC` |
| Stack center y | tune | 0.520 vh | `Config.STACK_CY_FRAC` |
| Disc vertical spacing | tune | 2.55 x ry | `Config.DISC_GAP_RY` |
| Seat counts (bottom->top) | Guild 8 / Project 6 / Agenda 6 | 8 / 6 / 6 = 20 | `Config.DISCS` |
| devicePixelRatio cap | <= 2 | 2 | `Config.DPR_CAP` |
| PRNG | seeded, deterministic | mulberry32, seed `0x41554D58` ("AUMX") | `Config.SEED` |

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

## Provisional seal geometry (P-3 / OI-2)

- **Not yet drawn** (seals are Phase 3/5). The central vertical **axis column** is laid in as the
  line the disc seals will later draw into; each seat's **inner hex** reserves the host area for its
  future seal draft. The **Datum Cross** remains the only confirmed canonical mark; all per-disc seal
  geometry will be implemented as PROVISIONAL and flagged in code + here.

---

## Audio (P-2 / 4.x) - engine not built until Phase 4

- Assets NOT delivered (Asset NOT READY). Engine will ship **audio-ready and silent by default**.
- Dev-only placeholder tones behind `Config.DEV_PLACEHOLDER_AUDIO`, **default OFF for deploy** (P-2).
  Not yet present (Phase 4).
- Optional **disc-complete voice** included in the engine contract: loaded/fired if
  `/audio/disc-complete.*` present, silent if absent (plan amendment 2).
- Audio constants to be declared on asset delivery (OI-3); current placeholders pending the
  delivered drone:

  | Constant | Value | Status |
  |---|---|---|
  | BPM | TBD | declared on drone delivery |
  | Bar length (beats) | TBD | declared on drone delivery |
  | Key | TBD | declared on drone delivery |
  | Quantize (one-shots) | next half-bar (bar acceptable) | per brief 4.2 |
  | Quantize (finale) | next full bar | per brief 4.2 |
  | Polyphony cap | 6 | per brief 4.1 |

---

## Acceptance criteria (brief 8) - tracking

- [ ] Idle identical on every load (deterministic) - PRNG seeded; verify on reload (Phase 2 flicker pending)
- [ ] Hover proposes / exit reverts (Phase 3)
- [ ] Rapid clicks -> one ceremony, one stinger (Phase 3/4)
- [ ] Bursts land on quantized boundary (Phase 4)
- [ ] Full 20-seat arc -> 3 disc completions -> finale -> scored reset (Phase 5)
- [x] Zero network requests beyond the file and `/audio/` (no CDN / third-party; font embedded)
- [x] Standing text = wordmark + the one sentence, nothing else
- [ ] Usable muted / keyboard-only / reduced motion (Phase 6)
