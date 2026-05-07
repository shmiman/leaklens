# LeakLens — Phase 3: Go-to-Market Strategy
**Date:** 6 May 2026

---

## 1. Target Customer Profile (ICP)

### Primary ICP: The Privacy-Conscious Developer-Creator

**Profile:**
- Age: 22–38, self-employed or at a startup
- Creates technical content: YouTube tutorials, Twitch coding streams, Coursera-style courses, Loom for clients
- Uses: macOS (primarily Apple Silicon), VS Code, 1Password, GitHub
- Income: $60K–200K/year personal or $100K–500K ARR for their studio
- Core fear: "I know I've flashed API keys on screen. I just don't know when."
- Willingness to pay: $5–15/month for a tool that solves this cleanly

**Secondary ICP: DevRel Engineer at a SaaS Company**
- Creates demo videos, marketing content, tutorials
- Compliance-conscious (SOC2, ISO 27001)
- Pain point: "Our developers make tutorial videos and we need to audit them for leaked keys before publishing"
- Willingness to pay: $15/seat/month (Teams tier)
- Sales cycle: Longer, needs internal buy-in

**Excluded for now (Year 1):**
- Corporate trainers at banks (MAS-regulated — needs SSO, audit trails, compliance docs)
- Law enforcement (needs specific format support, certification)
- Enterprise (needs SSO, MDM, volume licensing)

---

## 2. Positioning Statement

**One-liner:** *"LeakLens is the privacy-first video security linter for developers who create screen content."*

**Expanded positioning:**

> Most developer content creators know they've accidentally flashed API keys, environment variables, or client data in their screen recordings. They just don't know which frames. LeakLens is the only tool that scans video files entirely locally — no cloud, no uploads, no latency — using battle-tested secret detection (TruffleHog) and OCR to surface every security flag before it reaches the public.

**Key differentiator in one sentence:**
> *"LeakLens is to video what TruffleHog is to code — a linter that catches what you missed."*

**Position against alternatives:**
- vs. Manual scrubbing: 100× faster, catches what the eye misses
- vs. Nightfall/GitGuardian: Works on video (they don't), runs entirely offline
- vs. Enterprise redaction suites: 1% of the cost, 10× faster, designed for developers not compliance officers

---

## 3. Channel Strategy

### Primary Channel (Weeks 1–4): Developer Communities

**Why first:** Developers self-select for security tooling. Community endorsement from peers is the highest-trust signal.

| Channel | Action | Goal |
|---------|--------|------|
| **Hacker News** | Launch day "Show HN" post with demo video | 200–500 visitors, 10–20 signups |
| **r/programming** | Post: "I built a tool that finds API keys in your screen recordings" | Thread engagement, waitlist signups |
| **Dev.to** | Tutorial: "How to scan your tutorial videos for leaked API keys" | SEO traffic, waitlist signups |
| **Product Hunt** | Launch day launch | 300–1,000 visitors, top 5 of the day |

**Content approach for all:** Lead with the personal story ("I leaked my AWS keys on YouTube and spent 3 days in panic") — it's relatable, generates comments, and signals that LeakLens is built by someone who felt the problem.

---

### Secondary Channel (Weeks 5–12): Technical SEO + Comparison Pages

**Why:** Developers search for solutions when they have an immediate problem. "How to find API keys in video" and "video redaction tool" are low-competition keywords.

| Action | Goal |
|--------|------|
| LeakLens.com landing page with demo GIF | Convert HN/PH traffic to signups |
| "LeakLens vs TruffleHog" comparison page | SEO for comparison queries |
| "How to scan Loom videos for API keys" blog post | Capture "Loom API key leak" search traffic |
| GitHub repository (open-core) | Seed the developer OSS community |

---

### Tertiary Channel (Month 3+): DevRel Tooling Integrations

**Why later:** Integration with Loom, Camtasia, OBS, ScreenPal requires API access and partnership discussions. Not for Month 1.

| Target | Integration Type |
|--------|-----------------|
| OBS Studio | Plugin to scan before recording export |
| Camtasia | Export workflow integration |
| Loom | (If they open an API) — scan before publish |

---

## 4. Pricing Model

### Recommended Structure

| Tier | Price | Includes | Target |
|------|-------|---------|--------|
| **Free** | $0 | 10-min video max, 5 scans/day, watermark on export | Evaluate before buying |
| **Pro** | **$9/mo** or **$89/yr** | Unlimited videos, unlimited scans, no watermark, EDL export | Solo developer-creator |
| **Teams** | **$15/seat/mo** (min 5 seats) | Everything in Pro + shared team allowlist + audit log | DevRel teams, small studios |

**Annual discount rationale:** $89/yr = 2 months free. Drives commitment, reduces churn, improves cash flow for bootstrapped development.

**Free tier purpose:** The free tier is not for lead gen — it's a trust builder. A developer who scans 3 videos and gets real value will convert. The watermark is a soft nudge: "this was made with the free version."

---

## 5. Launch Timeline

### Pre-Launch (Weeks 1–4 before launch day)

| Week | Action |
|------|--------|
| -4 | Write 3 blog posts (dev.to). Seed on HN "Indie" thread. Begin waitlist collection (Typeform on simple landing page). |
| -3 | Recruit 5 beta users — personal outreach to developers who make YouTube coding content. Offer free Pro for life in exchange for feedback. |
| -2 | Set up LeakLens.com landing page with demo GIF. Submit to Product Hunt (preview). Submit to Hacker News "Upcoming" thread. |
| -1 | Final beta testing. Prepare launch assets: demo video (60s), HN/PH copy, Dev.to posts, screenshot kit. |
| Launch week | **Day 0:** HN Show HN + PH Launch. **Day 1:** Dev.to post + r/programming post. **Day 3:** Twitter/X thread of leak examples caught by LeakLens. |

### Post-Launch (Weeks 1–4 after)

| Week | Action |
|------|--------|
| 1 | Monitor HN/PH comments. Respond to every comment on launch threads. Collect emails from waitlist. |
| 2 | Send first email to waitlist: "LeakLens is live — here's what we built and why." |
| 3 | Publish comparison page: "LeakLens vs. Manual Scrubbing" |
| 4 | Analyze funnel: which channel drove signups? Double down on top channel. |

---

## 6. Pre-Launch Hook Strategy

**The story hook (for all launch content):**

> "I spent 3 days panicking after realizing I'd flashed my AWS production keys in a YouTube tutorial that had 20,000 views. No automated tool existed to catch this — so I built LeakLens."

**Why this works:**
- It's personal and authentic — not a company blog post
- It acknowledges the fear before selling the solution
- It signals domain expertise (the founder actually understands the problem deeply)
- It's falsifiable (someone will comment "AWS keys were rotated, right? 👀")

**Secondary hook (for DevRel audiences):**

> "Every DevRel team at a SaaS company has a policy against showing real API keys in public videos. Almost none of them have a tool to enforce it. LeakLens is the automated policy enforcement for developer video content."

---

## 7. Open Questions / Risks

| Risk | Likelihood | Mitigation |
|------|-----------|-----------|
| GitGuardian adds video module before LeakLens reaches critical mass | Medium | Speed to market is critical. 8-week MVP is the answer. |
| False positives kill user trust in beta | High | Run beta with 5 friendly users first. Tune on their feedback before public launch. |
| No distribution — tool ships and nobody finds it | High | HN/Reddit launch is non-negotiable. Personal outreach to 20 dev YouTubers for early feedback. |
| Free tier users never convert | Medium | Focus on making Pro feel like obvious value. Annual discount is the conversion lever. |
| macOS Tauri signing / notarization issues | Low | Apple's Gatekeeper can block unsigned apps. Must notarize before shipping. Budget $99/yr Apple Developer account. |
