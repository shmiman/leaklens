# LeakLens — Phase 1 Executive Summary
**Date:** 6 May 2026

---

## What's the opportunity?

LeakLens sits at the intersection of three large, growing markets: **video creation tools**, **developer security tooling**, and **data loss prevention**. No single player owns the "local-first, privacy-centric video security scanner" position. This is a genuine category gap.

**TAM estimate:** $8–12B (converging DLP + video + devtools markets)
**SAM estimate:** $30M–360M ARR at maturity (500K–2M ICP users)
**Year 1 realistic SOM:** $240K–600K ARR (2,000–5,000 paying users at $10/mo)

---

## Is the problem real?

**Yes. The JTBD is documented and urgent.**

Developers and tech creators have a documented fear of leaking API keys, AWS credentials, and PII in screen recordings. The consequences are real: credential compromise can lead to exploit within minutes. The developer community actively discusses this problem on Reddit, Hacker News, and security forums.

**Three tailwinds make now the right time:**
1. **DORA enforcement** (EU, Jan 2025) + MAS TRM guidelines = compliance pressure on financial video content
2. **Local-first software movement** gaining mindshare among privacy-sensitive developers
3. **Tauri maturity** — Rust-based desktop apps are now viable without Electron's overhead

---

## Who are the competitors?

| Competitor | Verdict |
|-----------|---------|
| Nightfall AI / GitGuardian | Strong on API key detection, but cloud-only, no video |
| Amazon Macie | AWS-native only, complex setup |
| TruffleHog | Best secret detection, CLI-only, no video |
| Sidewall / Oops | Video redaction, dated UX, Chinese market |
| Manual editing | Tedious, error-prone |

**LeakLens's moat:** Local-first + zero cloud + developer-tool aesthetic. No competitor has this combination.

---

## What's the critical risk?

**Distribution is the #1 risk.** GitGuardian or Nightfall adding a video module is a low-technical-effort, high-impact move given their existing user base. The go-to-market must be fast and community-driven (Product Hunt, Hacker News, Dev.to, r/programming) before incumbents notice.

**Technical risk #1:** PaddleOCR accuracy on terminal/code screenshots — this must be validated before Phase 2 commits to architecture.

---

## Recommended Pricing

| Tier | Price |
|------|-------|
| Free | $0 — 10-min video cap, watermark |
| Pro | $9/mo or $89/yr — unlimited |
| Teams | $15/seat/mo (min 5) |
| Enterprise | Custom |

---

## Recommended Next Steps (Phase 2)

1. **Validate OCR pipeline** before building anything — PaddleOCR on terminal frames is the make-or-break technical assumption.
2. **Benchmark Tauri sidecar** architecture for ML inference (PaddleOCR + Presidio).
3. **Write a one-page PRD** for the MVP scope — this PRD has 4 phases. MVP should be deliverable in 6–8 weeks, not 4 phases.
4. **Nail the false-positive rate** — developers will abandon a noisy linter. Tuning is non-negotiable.
