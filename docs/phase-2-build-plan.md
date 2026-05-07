# LeakLens — Phase 2 Build Plan (Post-Critical Review)
**Date:** 6 May 2026 | **Status:** Revised from PRD draft

> This build plan supersedes the original PRD phase structure. It incorporates the Phase 1 research findings and Phase 2 critical review.

---

## 1. Architecture Decision

### Stack (Revised)

| Component | Original PRD | Revised Decision | Rationale |
|-----------|-------------|-----------------|-----------|
| App Shell | Tauri (Rust) | **Tauri v2** | Stand by — Rust + React is the right model. Validate sidecar approach in POC. |
| Frontend | React / Tailwind CSS | **React 19 + Tailwind v4** | Correct choice. Keep. |
| Video Engine | FFmpeg (Native) | **FFmpeg static binaries (homebrew/win32)** | Bundled per-platform. Required for redaction pipeline. |
| OCR Engine | PaddleOCR (Local) | **PaddleOCR (Python sidecar)** | Change of stance — accept Python sidecar to preserve accuracy. App will be 300–500MB. This is honest. |
| Secret Scanner | TruffleHog v3 | **TruffleHog v3 (Python CLI wrapper)** | Wrap as subprocess, not PyO3. Simpler, slower, but shippable. |
| PII Detection | Microsoft Presidio | **Presidio Analyzer (Python)** | Same — Python sidecar. Microsoft ships a Docker image; we bundle the analyzer without the anonymizer. |
| Flag DB | SQLite (Tauri) | **SQLite via `rusqlite`** | Keep. Normalized bounding boxes (0–1 coordinates). |

### Architecture Diagram

```
User Video File (.mp4/.mov/.mkv)
        │
        ▼
┌─────────────────────────────────┐
│       Tauri App (Rust)          │
│  ┌───────────────────────────┐  │
│  │  FFmpeg Frame Extractor   │  │
│  │  (Rust: ffmpeg-next)      │  │
│  └─────────────┬─────────────┘  │
│                │  raw frames    │
│  ┌─────────────▼─────────────┐  │
│  │   Python Sidecar Process  │  │
│  │  ┌─────────────────────┐  │  │
│  │  │ PaddleOCR           │  │  │  ← Frame → text
│  │  │ TruffleHog v3       │  │  │  ← text → secrets
│  │  │ Presidio Analyzer   │  │  │  ← text → PII
│  │  └─────────────────────┘  │  │
│  └─────────────┬─────────────┘  │
│                │  flags (JSON)   │
│  ┌─────────────▼─────────────┐  │
│  │  SQLite Flag DB           │  │  ← timestamps, normalized bboxes, types
│  │  (rusqlite, Rust)        │  │
│  └─────────────┬─────────────┘  │
│                │                 │
│  ┌─────────────▼─────────────┐  │
│  │  React UI                 │  │  ← Flag timeline, video player, redaction queue
│  │  (TypeScript)            │  │
│  └─────────────┬─────────────┘  │
│                │                 │
│  ┌─────────────▼─────────────┐  │
│  │  FFmpeg Redaction Bake-in │  │  ← Black box / pixelate render
│  │  (Rust:Command)          │  │
│  └───────────────────────────┘  │
└─────────────────────────────────┘
        │
        ▼
Clean Video Out (.mp4) + Optional EDL File
```

### Critical Architecture Note: Python Sidecar

The Python sidecar is a separate `python3` subprocess spawned by the Tauri Rust process. It is not embedded in the Tauri binary. This has implications:

- **Pro:** Faster development (use proven Python ML libraries). No PyO3 complexity.
- **Con:** App install size ~350–500MB. Python interpreter + PaddleOCR model + Presidio models all bundled.
- **Mitigation:** Ship per-platform installers. macOS: use `macpython` + `brew` packaged deps. Windows: static Python embeddable distribution.

---

## 2. MVP Scope — Two Phases, Not Four

### Phase A: Core Engine MVP (Weeks 1–4)
**Principle:** Ship the scanning engine + basic output. No UI polish. No EDL. No PII (yet).

**Deliverables:**
- [ ] FFmpeg frame extraction — 1 frame per second from input video
- [ ] Python sidecar: PaddleOCR frame → text
- [ ] Python sidecar: TruffleHog text → secret flags (AWS, Stripe, OpenAI, GitHub — top 4 only for MVP)
- [ ] SQLite DB: flags with normalized bboxes, timestamps, confidence scores
- [ ] Rust → FFmpeg redaction bake-in (black box blur at flagged coordinates)
- [ ] Output: clean `.mp4` to user-selected location
- [ ] CLI interface (Tauri CLI command): `leaklens scan /path/to/video.mp4`

**Definition of Done:**
- 30-min 1080p video scanned in <15 minutes on recommended hardware (M3 Mac / Intel i7 16GB)
- ≥90% recall on top-50 most common API key formats (tested on synthetic dataset)
- False positive rate <20% on clean terminal/code frames

**Excluded from Phase A:**
- No UI (CLI only)
- No PII detection
- No bounding box overlay in player
- No EDL export
- No macOS installer (manual install only)

---

### Phase B: Full UI MVP (Weeks 5–8)
**Principle:** Add the triage interface. Turn the engine into a usable product.

**Deliverables:**
- [ ] React + Tailwind UI: video player with flag timeline sidebar
- [ ] Click flag → jump to frame + show bounding box overlay
- [ ] "Ignore" action → session whitelist (session-only, stored in memory)
- [ ] "Redact" action → queue for FFmpeg bake-in
- [ ] Batch redaction: apply all confirmed flags → single FFmpeg pass
- [ ] Progress indicator during scan
- [ ] macOS `.app` installer + Windows `.exe` installer (Tauri bundler)
- [ ] Phase A CLI → Phase B GUI parity (both use same scanning engine)

**Definition of Done:**
- Developer beta user tests the full scan → review → redact workflow end-to-end
- 0 critical bugs (crash, data loss) in beta

**Excluded from Phase B:**
- PII detection (Phase C)
- EDL export (Phase C)
- Team features / shared allowlists
- Auto-update (can be GitHub Releases manual download for now)

---

### Phase C: PII + Integrations (Post-Launch, v1.1)
- Presidio PII detection (NRIC/FIN, passport numbers, phone numbers, addresses)
- Singapore MAS TRM compliance mode (PDPA-aware scanning)
- EDL export for Premiere Pro / DaVinci Resolve
- Session allowlist persistence (per-user JSON config file)

---

## 3. Development Timeline

| Week | Phase A (Engine MVP) | Phase B (UI MVP) |
|------|---------------------|-----------------|
| 1 | Project setup: Tauri v2 + Python sidecar scaffold + FFmpeg bundling | |
| 2 | FFmpeg extraction + PaddleOCR integration | |
| 3 | TruffleHog wrapper + SQLite flag DB | |
| 4 | FFmpeg redaction bake-in + CLI test | |
| 5 | | React UI shell + video player proxy |
| 6 | | Flag timeline + bounding box overlay |
| 7 | | Ignore/Redact actions + batch redaction |
| 8 | Alpha test (internal) | Beta test (5 external users) |
| **End of Week 8** | **Ship to beta** | **Ship to beta** |

**Total to first beta:** 8 weeks

---

## 4. Platform Scope

| Platform | Phase A | Phase B | Notes |
|----------|--------|---------|-------|
| macOS Apple Silicon | ✅ Dev | ✅ Ship | Primary target. Best PaddleOCR support (Core ML). |
| macOS Intel | ✅ Dev | ✅ Ship | Secondary. CPU-only OCR. |
| Windows (x64) | ✅ Dev | ✅ Ship | FFmpeg static build. PaddleOCR CPU. |
| Linux | ❌ | ❌ | Phase C. FFmpeg packaging complexity. |

---

## 5. Hardware Baseline

| | Minimum | Recommended |
|--|---------|------------|
| **OS** | macOS 12+ / Windows 10+ | macOS 14+ (M-series) / Windows 11 |
| **CPU** | Intel i5 (4-core) | Apple Silicon M-series or Intel i7 (8-core) |
| **RAM** | 8 GB | 16 GB |
| **GPU** | Integrated | M-series GPU or NVIDIA with CUDA |
| **Storage** | 2 GB free | 5 GB free |
| **Scan speed** | 30-min 1080p in <30min | 30-min 1080p in <10min |

---

## 6. Key Technical Decisions (Resolved)

| Decision | Choice | Rationale |
|----------|--------|-----------|
| Python sidecar vs PyO3 | **Python subprocess** | Faster to build. Acceptable install size (~400MB). |
| Secret detection engine | **TruffleHog v3 CLI wrapped** | 800+ detectors, battle-tested. Wrap as subprocess. |
| PII detection | **Presidio Python SDK** | Best-in-class for structured PII identifiers. Sidecar. |
| Bounding box storage | **Normalized (0.0–1.0)** | Resolves to any output resolution. Critical for redaction accuracy. |
| FFmpeg wrapper | **`ffmpeg-next` Rust crate** | Type-safe, no subprocess overhead for frame extraction |
| Redaction filter | **FFmpeg `drawbox` + `boxblur`** | Black box or pixelate, single pass |
| DB | **SQLite via `rusqlite`** | Embedded, no server, ACID-compliant |
| UI framework | **React 19 + Tailwind v4** | Correct. Hot reload dev experience is worth the bundle size. |
| Install distribution | **Tauri bundler (`.app` / `.exe`)** | One-command install for non-technical users |

---

## 7. Open Technical Risks (Must Resolve in Phase A Week 1)

| Risk | Probability | Impact | Mitigation |
|------|------------|--------|-----------|
| PaddleOCR on terminal text accuracy <80% | Medium | High | Run POC before committing. Alternative: Tesseract + custom post-processing. |
| TruffleHog subprocess overhead kills performance | Low | Medium | Benchmark. If >20% overhead, port hot path to Rust regex. |
| FFmpeg bundled size too large | Medium | Low | Strip FFmpeg to only required codecs (H.264, AAC, VP9). |
| Apple Silicon PaddleOCR GPU acceleration fails | Low | Medium | Fallback to CPU. Add Core ML model explicitly in build. |
| Presidio false positives on code context | High | High | Add code-context aware pre-filter (skip frames with `function`, `const`, `let`, `var` keywords nearby). |

---

## 8. Resource Requirements

| Resource | Phase A | Phase B |
|----------|---------|---------|
| Developer time | 6–8 weeks full-time | 4–6 weeks full-time |
| Compute (testing) | 3–5 test videos of varying length/resolution | Same + beta user collection |
| Synthetic test dataset | 50 videos with seeded API keys (own creation) | 100 videos for false-positive benchmarking |
| Estimated build artifacts | ~400MB installer | ~400MB installer + auto-updater |

---

## 9. What This Plan Changes from the Original PRD

1. **4 phases → 2 phases** before a shippable product
2. **Added Python sidecar architecture** — honest about the install size tradeoff
3. **Added hardware baseline** — enables honest performance claims
4. **Added normalized coordinate schema** — prevents redaction misalignment
5. **Added false-positive benchmark** — quality gate before shipping UI
6. **Added platform scope** — macOS + Windows MVP, Linux deferred
7. **Deferred EDL export to Phase C** — low value-add for MVP
8. **Deferred PII detection to Phase C** — core secret scanning first
9. **Deferred auto-update to Phase C** — GitHub Releases manual download for v1
10. **Added beachhead ICP** — individual developer/creator, not corporate trainers
