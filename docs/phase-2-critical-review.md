# LeakLens — Phase 2: Critical Review of Build Plan & PRD
**Review date:** 6 May 2026 | **Reviewer:** Foundry Operations Agent

> *"Be critical. Surface every assumption, risk, and gap. Do not be gentle."*

---

## CRITICAL ISSUE #1: The Tech Stack Has a Hidden Python Problem

**The PRD says:** Tauri (Rust) + PaddleOCR + TruffleHog v3 + Presidio

**The problem:** TruffleHog v3 and Microsoft Presidio are **Python packages**. They are not Rust libraries. You cannot simply drop them into a Tauri sidecar.

**What this means for the build:**

| Library | Language | Tauri Integration Path | Complexity |
|---------|----------|----------------------|------------|
| PaddleOCR | Python | PyO3 bindings OR Python sidecar process | High — large model (~200MB), PyO3 is complex |
| TruffleHog v3 | Python | PyO3 port OR pre-compiled CLI wrapper | Medium — 800+ detectors = complex port |
| Presidio | Python | Microsoft ships a Docker/SDK, not pure Rust | High — no native Rust port exists |
| FFmpeg | C | `ffmpeg-next` Rust bindings exist | Low — well-supported |

**The Tauri sidecar promise vs reality:**
- Tauri sidecars are native Rust processes embedded in the app bundle
- If you need Python ML models, you either: (a) write PyO3 bindings (hard, requires deep Rust+Python knowledge), (b) bundle a Python interpreter (defeats Tauri size advantage), or (c) use a micro-service architecture (now you have a server component)
- PaddleOCR specifically is a 200MB+ model. A "minimal RAM footprint" Tauri app that bundles PaddleOCR will be 400MB+ installed. This undermines the lightweight positioning.

**Recommended resolution (Phase 2 must validate this):**
1. **Option A — Drop Python, go Rust-native:** Use `rust-ocr` or port Tesseract (C) + write custom regex for secrets. Accept lower OCR accuracy in exchange for a pure Rust stack.
2. **Option B — Accept the Python sidecar:** Bundle a minimal Python interpreter + PaddleOCR. Accept that the app will be 300–500MB. This is honest about the tradeoff.
3. **Option C — Go Electron + Python native:** Electron handles Python much more naturally via `python-shell` or `child_process`. If Python ML is non-negotiable, Electron is the pragmatic choice over Tauri.

**Decision needed before Phase 2 begins, not during.**

---

## CRITICAL ISSUE #2: Phase 1's "Smart Sampling Logic" is a Research Project

**The PRD says Phase 1:** "Develop an algorithm to extract frames strategically (e.g., every 0.5–1.0s) or utilize Delta-frame analysis to only scan when a significant 'screen change' is detected."

**The problem:** This is not a build task. Delta-frame analysis for screen recordings is an active research problem. The approach taken by Loom, Cloudinary, and Mux for "smart video sampling" involves ML models (scene detection, saliency detection), not simple pixel differencing.

**Specific risks:**
- Naive frame differencing will miss slow-panning shots where credentials are visible for 3+ seconds
- Every-0.5s sampling on a 30-min 60fps video = 3,600 frames. At 1080p that's ~27GB of frames to OCR. This is not fast.
- "Significant screen change" detection sounds like a ML problem — if you're already using PaddleOCR + Presidio, you may as well use a lightweight scene detection model too, but that's more complexity.

**Recommended:** Default to uniform sampling (every 1s) for MVP. Skip delta-frame analysis until you have a real performance problem. Premature optimization of the scanning pipeline is a classic path to never shipping.

---

## CRITICAL ISSUE #3: No Platform Support Definition

**The PRD says:** "Standalone desktop application"

**The problem:** No mention of Windows / macOS / Linux support. This is a significant build decision.

| Platform | FFmpeg bundling | PaddleOCR GPU | Tauri support | Build complexity |
|----------|----------------|---------------|---------------|------------------|
| macOS (Apple Silicon) | `ffmpeg` Homebrew package | Core ML acceleration available | Excellent | Medium |
| macOS (Intel) | `ffmpeg` package | CPU only | Excellent | Medium |
| Windows | Static `ffmpeg.exe` | CUDA on Windows is complex | Good | High |
| Linux | `ffmpeg` package | Limited | Good | Medium |

**Recommendation:** Define MVP as **macOS (Apple Silicon + Intel) + Windows** only. Linux support as 1.1. Cross-platform FFmpeg/PaddleOCR bundling will multiply your QA burden in Phase 1.

---

## CRITICAL ISSUE #4: Bounding Box → FFmpeg Redaction is a Gap

**The PRD says:** Phase 4: "Use FFmpeg to bake a 'Black Box' or 'Pixelate' filter over the coordinates"

**The problem:** There's no mention of how the bounding box coordinates (pixel coordinates from the OCR/Presidio layer) map to FFmpeg's crop/blur filter syntax. This is non-trivial:

- FFmpeg's `drawbox` filter uses top-left origin, pixel coordinates
- OCR bounding boxes are normalized (0–1 float) or pixel-based depending on the library
- The video may be scaled (e.g., screenshot captured at 1920×1080, embedded in a 4K timeline then exported)
- Rotation, letterboxing, and aspect-ratio changes in the video pipeline break naive coordinate mapping

**Recommendation:** Add a coordinate normalization step to Phase 2's scope. The bounding box storage in SQLite must store normalized coordinates (0.0–1.0 relative to frame dimensions), not raw pixel values. This needs to be defined in the database schema before Phase 3 begins.

---

## CRITICAL ISSUE #5: The "Zero Cloud Dependency" Claim Has a Gap — Auto-Updates

**The PRD says:** "The application must never send a single video frame or parsed text string to the internet."

**The problem:** How does the app get auto-updates without cloud infrastructure?

- Tauri uses GitHub Releases or a custom update server for auto-update推送
- If you use GitHub Releases, you have a privacy-safe update mechanism (the app just downloads its own binary from GitHub — no telemetry)
- If you go truly air-gapped, you have no auto-updates, which is a UX problem for non-technical users

**Resolution:** Use Tauri's built-in updater pointing to a GitHub releases endpoint. This is technically zero-cloud in the sense that no user data ever leaves the machine. This must be explicitly documented in the privacy architecture.

---

## CRITICAL ISSUE #6: 4 Phases Before Any Ship is Too Long

**The PRD structure:**
- Phase 1: Core engine (~weeks?)
- Phase 2: Intelligence layer (~weeks?)
- Phase 3: Triage interface (~weeks?)
- Phase 4: Redaction & handoff (~weeks?)

**The problem:** At 4 phases, you're looking at potentially 4–6 months before anything ships. In startup time, this is an eternity. If GitGuardian or Nightfall add a video module, all this work may be obsolete.

**The fix:** Collapse to **2 phases**:

| Phase | Scope | Target |
|-------|-------|--------|
| **Phase A (MVP)** | Core scanning engine + basic redaction output. Single video in, flagged timestamps out, manual redaction via FFmpeg CLI bake-in | Ship to 10–20 beta users |
| **Phase B** | Full UI (timeline, flag management), EDL export, bounding box UI, PII detection | Public launch |

This gets a testable product out in 6–8 weeks, not 4+ months.

---

## CRITICAL ISSUE #7: False Positive Rate is the #1 Product Risk

**The PRD says:** Phase 2 will "actively ignore common 'safe' technical terms to reduce false positives."

**The problem:** This is underspecified and is the most likely reason the product fails. Consider:

- `const API_KEY = process.env.STRIPE_SECRET_KEY` — this contains `STRIPE_SECRET_KEY` but it's a variable name, not a real key. Does TruffleHog flag this?
- Terminal output often shows environment variable names that look like keys but are unset
- Code comments contain strings that match API key patterns but aren't real credentials
- `sk_live_...` in a video frame where the camera briefly captures a screen — the blur might not fully redact the key

**What "reduce noise" actually requires:**
1. A curated allowlist of common technical terms (ENV_VAR_NAMES, function parameters, code keywords)
2. Confidence thresholds — only flag if the OCR text passes multiple pattern matchers
3. User feedback loop — "Ignore" must train a session model
4. Visual confirmation before redaction — users must see the flagged frame before committing

**Phase 2 must include a false-positive benchmark.** Define: "90% precision at 95% recall on a test set of 100 developer screen recordings" as a shipped quality gate. Without this, Phase 3's UI is just a noisy flag list that drives users away.

---

## ISSUE #8: No Hardware Baseline Defined

**The PRD says:** "Performance Over Completeness — prioritize rapid scanning"

**The problem:** No hardware baseline means no performance contract. What's "fast"?

- On an M3 MacBook Pro: 30-min 1080p video at 1s sampling = 1,800 frames × PaddleOCR (with GPU) ≈ 3–8 minutes
- On a 2019 Intel i5 Dell: same video ≈ 20–40 minutes
- On a 4K 60fps video: 6x the frame count

**Recommendation:** Define a minimum spec:
- **Recommended:** Apple Silicon Mac or Intel i7 (8GB RAM) / Windows i7 (16GB RAM)
- **Minimum:** Intel i5, 8GB RAM, integrated graphics — accept 2–3× slower scans
- **Exclude:** 4K 60fps content on minimum spec (document this)

---

## ISSUE #9: EDL Export is Premature for MVP

**The PRD puts EDL (Edit Decision List) export for Premiere Pro/DaVinci Resolve in Phase 4.**

**Recommended:** Move EDL to Phase 2 or post-launch. The technical spec for XML/EDL is trivial (it's a timestamp + track + action file). Focus Phase 4 on the core redaction workflow first. EDL is a nice-to-have for v1.1.

---

## ISSUE #10: Pricing & ICP Mismatch

**The PRD says:** Target audience is "developers, tech creators, corporate trainers, educators"

**The problem:** These four groups have very different:
- Willingness to pay
- Procurement processes
- Feature needs
- Security requirements

A corporate trainer at a bank (in Singapore: MAS-regulated firm) needs SSO, audit trails, and compliance documentation. A solo developer needs a fast, cheap, zero-friction tool. Trying to serve all four simultaneously leads to feature bloat and muddled positioning.

**Recommendation:** Pick ONE beachhead ICP for launch. Best candidate: **individual developer / tech creator** (solo, pays $9/month, moves fast, gives direct feedback). Corporate trainers and regulated industry come in Year 2 as a separate "Teams/Enterprise" tier.

---

## Summary: Critical Issues Ranked by Severity

| Priority | Issue | Impact | Fix Complexity |
|----------|-------|--------|---------------|
| 🔴 P0 | Python stack vs Rust sidecar | App architecture choice | High (requires re-evaluation) |
| 🔴 P0 | False positive rate undefined | Product fails or gets abandoned | Medium (research + tuning) |
| 🔴 P0 | 4 phases before any ship | Time-to-market risk | Low (re-structure to 2 phases) |
| 🟡 P1 | Platform support undefined | Build complexity underestimated | Medium (pick MVP platforms) |
| 🟡 P1 | Bounding box coordinate mapping gap | Redaction won't work correctly | Medium (schema design needed) |
| 🟡 P1 | No hardware baseline | Performance claims unverifiable | Low (add spec docs) |
| 🟡 P1 | EDL export in Phase 4 | Delays launch, low value-add | Low (move to post-MVP) |
| 🟢 P2 | Auto-update gap in privacy arch | UX/security gap | Low (use GitHub releases) |
| 🟢 P2 | Pricing ICP mismatch | Positioning confusion | Low (pick beachhead ICP) |

---

## Phase 2 Recommended Actions

1. **[Do this first]** Validate PaddleOCR performance on terminal/code frames — run a POC before committing to the Python-sidecar architecture
2. **Define MVP scope as Phase A + Phase B** — 2 phases, not 4. Target shippable MVP in 6–8 weeks
3. **Write coordinate normalization into database schema** — bounding boxes must store normalized 0–1 coordinates
4. **Define false-positive benchmark** — "90% precision at 95% recall on our test set" as a quality gate
5. **Pick MVP platform target** — macOS (Apple Silicon) + Windows as v1, Linux as v1.1
6. **Define hardware baseline** — minimum and recommended specs documented
7. **Pick ONE beachhead ICP** — developer/creator solo, not corporate trainers
