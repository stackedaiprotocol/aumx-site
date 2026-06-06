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
| Seat ring radius | tune | 0.84 x disc rx | `Config.SEAT_RING_RX` |
| Seat ring lift (upper face) | reveal seat ring | 0.30 x ry | `Config.SEAT_RING_LIFT` |
| Seat hex base radius | tune | 8.5 px | `Config.SEAT_HEX_BASE` |
| Seat foreshorten scale | tune | 0.72 (back) -> 1.16 (front) | `Config.SEAT_FORE_MIN/MAX` |
| Seat idle luminance | dim-lit | 0.20 to 0.36 opacity | `Config.SEAT_LUMA_MIN/MAX` |
| devicePixelRatio cap | <= 2 | 2 | `Config.DPR_CAP` |
| PRNG | seeded, deterministic | mulberry32, seed `0x41554D58` ("AUMX") | `Config.SEED` |

Palette (C-1, pure monochrome): bg `#000000`; disc line `#2b2b2b`; seat-ring guide `#171717`;
axis column `#181818`; seat light `#f4f4f4`. Greys for falloff only. No hue anywhere.

---

## Provisional seal geometry (P-3 / OI-2)

- **Not yet drawn** (seals are Phase 3/5). The central vertical **axis column** is laid in as the
  line the disc seals will later draw into. The **Datum Cross** remains the only confirmed canonical
  mark; all per-disc seal geometry will be implemented as PROVISIONAL and flagged in code + here.

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
