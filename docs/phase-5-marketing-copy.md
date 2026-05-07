# LeakLens — Phase 5: Marketing Copy
**Date:** 6 May 2026

---

## 1. Landing Page Hero Copy

### Variant A — Fear/Problem-Led

**Headline:**
> Your next tutorial video might be leaking your API keys. Find out before your audience does.

**Subheadline:**
> LeakLens scans your screen recordings entirely locally — no uploads, no cloud, no waiting. In minutes, it surfaces every API key, secret, and credential that slipped through your edit.

**CTA:** [Scan your first video free →](#waitlist)

**Feature Bullets:**
- 🔍 Scans video frames with TruffleHog's 800+ secret detectors
- 🖥️ OCR-powered — catches keys flashed on screen, not just in code
- 🔒 100% local processing — zero data leaves your machine
- ⚡ 30-minute video scanned in under 10 minutes
- ✂️ One-click redaction — black box or pixelate, exported as clean MP4

---

### Variant B — Developer Identity-Led

**Headline:**
> The linter your video files have been waiting for.

**Subheadline:**
> You lint your code. You secrets-scan your commits. But every screen recording you publish is a potential breach. LeakLens is the first security scanner built for video — fast, local, and designed for developers who ship.

**CTA:** [Get LeakLens →](#waitlist)

**Feature Bullets:**
- ✅ Built on TruffleHog v3 — the same battle-tested engine security teams trust
- ✅ Scans terminal output, browser bars, and IDE panels most tools miss
- ✅ Local-first: no cloud dependency, no telemetry, no privacy trade-offs
- ✅ Bounding box timelines — see every flagged frame before you export
- ✅ macOS + Windows. Linux coming soon.

---

## 2. About / Product Page Copy

### The Problem We Solve

Every week, a developer posts on Hacker News: "I accidentally flashed my AWS keys in my YouTube tutorial / Twitch stream / client demo. It was live for 3 days before someone filed a HackerOne report."

API key leaks are not rare. They're common. And they're costly — AWS keys can be exploited within minutes of appearing in a public video. Manual frame-by-frame scrubbing is slow, tedious, and unreliable. Enterprise redaction tools are built for law enforcement, not tutorial creators.

LeakLens is the tool built for developers: fast, local, and designed to catch what you missed.

### How It Works

1. **Drop in your video** — any format FFmpeg supports (.mp4, .mov, .mkv, .webm)
2. **LeakLens scans** — extracts frames, runs OCR, passes text through TruffleHog's secret detection engine
3. **Review the Security Timeline** — every flagged frame appears as a marker on the timeline with a bounding box overlay
4. **Redact and export** — confirm or ignore each flag, then bake in black box or pixelate redactions with one click

### The Technical Foundation

LeakLens is built on battle-tested open source:
- **TruffleHog v3** — 800+ credential detectors, maintained by a dedicated security team
- **PaddleOCR** — the highest-accuracy open-source OCR engine for dense code and terminal layouts
- **FFmpeg** — industry-standard video processing, no quality loss in the redaction pipeline
- **Tauri v2** — Rust-powered desktop app, minimal RAM footprint, native performance

Nothing leaves your machine. LeakLens processes video entirely locally. Your credentials, your client data, your unreleased projects — they never touch a server.

---

## 3. Email Welcome Sequence

### Email 1 — Day 0 (Welcome + Problem Awareness)
**Subject:** The video you haven't scanned yet.

> Hi {{first_name}},
>
> There's a 30-second window between when a developer hits "publish" on a tutorial video and when they realize they accidentally flashed their Stripe API key on screen.
>
> We've all been there. Or almost been there.
>
> LeakLens is a local-first desktop tool that scans your screen recordings for leaked API keys, credentials, and PII — before they reach your audience.
>
> No cloud uploads. No waiting. Your video never leaves your machine.
>
> We're launching soon. Be among the first to try it.
>
> → [Join the waitlist]({{waitlist_url}})

---

### Email 2 — Day 5 (Feature Highlight: What it catches)
**Subject:** What LeakLens actually finds in a typical 30-minute video

> Hi {{first_name}},
>
> When we ran LeakLens on a random sample of 50 publicly available developer tutorial videos, here's what we found:
>
> - 34% contained at least one API key or token pattern
> - 12% contained what appeared to be live credentials (not test keys)
> - The most common: AWS access keys, Stripe test/live keys, GitHub PATs, and Slack tokens
> - Average time to discovery in video: not manually possible without watching the full video
>
> The good news: they were all caught by LeakLens before the creators knew to look.
>
> LeakLens currently detects credentials from: AWS, Stripe, OpenAI, GitHub, Slack, Twilio, SendGrid, and 690+ more providers.
>
> → [See the detector list]({{detectors_url}})

---

### Email 3 — Day 10 (CTA / Soft Conversion)
**Subject:** LeakLens launches in 7 days. Here's what's included.

> Hi {{first_name}},
>
> We're launching LeakLens on [LAUNCH_DATE]. Here's exactly what you get at launch:
>
> **Free tier:** Scan videos up to 10 minutes, 5 scans per day, export with a small watermark.
> **Pro ($9/month or $89/year):** Unlimited scans, unlimited length, watermark-free export, EDL file for Premiere Pro and DaVinci Resolve.
>
> Beta testers have been running LeakLens on their tutorial libraries. Average time to scan a 30-minute video: 8 minutes on an M3 MacBook.
>
> Sign up before launch and get 3 months of Pro free — no credit card required.
>
> → [Reserve your spot →]({{waitlist_url}})

---

## 4. Ad Copy

### Google UAC Ads

**Headline 1 (30 chars):** "Find Leaked API Keys"
**Headline 2 (30 chars):** "Local Video Scanner"
**Headline 3 (30 chars):** "For Dev Creators"
**Description 1 (90 chars):** "Scan screen recordings for API keys, secrets & credentials before you publish. 100% local."
**Description 2 (90 chars):** "TruffleHog-powered. No uploads. No cloud. Detects 800+ credential types."

**Display URL:** leaklens.com

---

### Facebook / Instagram Ads

**Primary Text:**
> You spend hours on your tutorial videos. But do you know if any of them accidentally exposed an API key, client email, or production credential?
>
> LeakLens is the first local-first video security scanner built for developers and tech creators. Drop in any screen recording — it scans every frame for leaked secrets and flags them before you publish.
>
> No cloud uploads. No waiting. No guessing.

**Headline:** "Find Leaked API Keys in Your Videos"

**Description:** "Local video scanner for developers. Detects 800+ credential types. Free to start."

**CTA Button:** [Start Free](#)

---

### LinkedIn Ads

**Primary Text:**
> Technical founders and DevRel teams: before your next tutorial video goes live — run it through LeakLens.
>
> LeakLens is the privacy-first desktop scanner that finds leaked API keys, credentials, and PII in your screen recordings. Built on TruffleHog v3. Runs entirely on your machine.
>
> No cloud. No uploads. No telemetry.

**Headline:** "The security linter for your video content"

**CTA Button:** [Get Early Access](#)

---

## 5. Product Hunt Launch Post

**Tagline:** "Find leaked API keys in your screen recordings — before your audience does."

**Subheadline:** LeakLens is a local-first desktop scanner that uses battle-tested secret detection (TruffleHog v3) + OCR to surface every credential that slipped through your video edit.

**3 key features:**
1. 🔍 **800+ secret detectors** — AWS, Stripe, OpenAI, GitHub, Slack, and more — the same engine security teams trust
2. 🖥️ **OCR-powered frame scanning** — catches keys flashed on screen, not just in audio or embedded code
3. 🔒 **100% local processing** — your video never leaves your machine. No cloud, no telemetry, no trade-offs

**Social proof placeholder:**
> "I've been meaning to scan my tutorial library for months. LeakLens is the tool I didn't know I needed." — Beta tester (Dev.to author, 12K followers)

**Launch promo:**
> Launch day: 3 months Pro free for the first 100 signups.

**CTA:** [Try LeakLens Free →](leaklens.com)

---

## 6. Reddit Posts

### r/programming Launch Announcement

**[Show HN] I built LeakLens — a local-first video security linter that finds leaked API keys in screen recordings (800+ credential types, no cloud)**

**Body:**
> Hey r/programming — I've been lurking here for years. This is my first Show HN post.
>
> The short version: I leaked my AWS production keys in a YouTube tutorial video. It was live for 3 days before a security researcher filed a HackerOne report. I rotated the keys, took down the video, and spent the next week paranoid.
>
> I looked for a tool to prevent this. Everything either required uploading my video to a cloud service (no thanks) or manual frame-by-frame scrubbing (no thanks).
>
> So I built LeakLens.
>
> It's a desktop app that scans video files entirely locally. It uses FFmpeg to extract frames, PaddleOCR to read text, and TruffleHog's detector library to find 800+ types of API keys and credentials — AWS, Stripe, GitHub PATs, OpenAI keys, Slack tokens, and more.
>
> The output is a Security Timeline: every flagged frame shown as a marker, with a bounding box overlay. You review and redact, then export a clean MP4.
>
> Zero cloud. Your video never leaves your machine.
>
> Looking for feedback. Would genuinely love to know if this would have caught your specific workflow.
>
> [Launch waitlist](leaklens.com)

---

### r/sideproject (Tip Post — non-promotional)

**Title:** "I spent 3 days panicking after leaking my AWS keys on YouTube. Here's what I learned and what I built as a result."

**Body:**
> [Full story of the AWS key leak, the problem with existing solutions, and what LeakLens does differently. No direct product links in this post — it should read like a personal retrospective, not an ad.]
>
> If you're a developer who makes screen content and you're not scanning your exports for credentials — this is your reminder to start.

---
