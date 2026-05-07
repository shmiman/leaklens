# LeakLens — Phase 6: Content Calendar
**Date:** 6 May 2026

---

## Content Strategy Overview

### Content Pillars
| Pillar | Share | Focus |
|--------|-------|-------|
| **Educational** | 60% | How credential leaks happen, how to prevent them, industry data |
| **Launch / Product** | 25% | Launch announcements, feature releases, beta updates |
| **Community / Engagement** | 15% | Respond to others' credential leak posts, industry news commentary |

### Tone
Plain English, developer-first, no fear-mongering. Reference specific incidents (without naming names) when they happen. Use numbers and data. Be direct, not salesy. The audience is security-conscious developers — they can smell FUD from a mile away.

### Compliance Note
All posts must include a disclaimer when discussing real credential incidents: "Always rotate compromised keys immediately. Follow your provider's incident response guide." No specific investment advice. No recommendations to buy/sell any security product.

---

## Launch Week Content Package (Week of Launch)

### Day 0 — Launch Day

**Post 1: Product Hunt Launch**
- Format: PH launch post (see Phase 5 for copy)
- Timing: 12:01 AM PT (catches the PH daily chart reset)

**Post 2: Hacker News Show HN**
- Format: Text post with demo video link (60s GIF or Loom)
- Copy: See r/programming launch announcement (Phase 5)
- Timing: Post after PH is live

**Post 3: Twitter/X Thread**
- Format: 5-tweet thread
```
🧵 I accidentally leaked my AWS keys in a YouTube tutorial. 20K views. 3 days before I noticed.

Here's what happened, what I built because of it, and why I think every dev who makes screen content needs this tool.

[THREAD]
```
- Thread covers: The incident → why manual scrubbing doesn't work → how LeakLens works → demo GIF → waitlist link

---

### Day 1 — Post-Launch Momentum

**Post 4: Dev.to Article**
- Title: "How to scan your tutorial videos for leaked API keys before publishing"
- Format: Step-by-step guide. Walk through a sample video. Show before/after scan results.
- CTA: Try LeakLens free
- SEO keywords: "scan video for API keys", "video redaction tool", "API key leak prevention"

**Post 5: r/programming Launch Post**
- Format: Text post (see Phase 5)
- Note: Engage with every comment in the first 2 hours. Response speed drives HN algorithm.

---

### Day 3 — Traction + Credibility

**Post 6: Twitter/X Single Post**
> "Day 3 update: LeakLens found API keys in 3 out of the first 10 videos scanned by beta users. All were real keys. None of the creators knew."
- Purpose: Social proof without specific numbers that could be gamed

---

### Day 5 — Comparison / SEO

**Post 7: Dev.to Comparison Post**
- Title: "LeakLens vs. Manual Frame Scrubbing: Why I Built a Linter for Video"
- Format: Comparison table + narrative. No direct competitor naming.
- SEO value: Captures "video redaction tool" and "scan video for secrets" searches

---

## 30-Day Content Calendar (Post-Launch)

### Week 2

| Day | Platform | Format | Content |
|-----|----------|--------|---------|
| Mon | Dev.to | Article | "The anatomy of a credential leak: 5 real examples from open source projects" |
| Wed | Twitter/X | Single post | "Hot take: the most dangerous API key is the one you think you didn't show." |
| Fri | LinkedIn | Short post | "DevRel teams: do you have a process for auditing tutorial videos before they go live?" + poll |

### Week 3

| Day | Platform | Format | Content |
|-----|----------|--------|---------|
| Mon | r/sideproject | Story post | "What I wish I knew before I started making coding tutorials on YouTube" (LeakLens as a recurring theme) |
| Wed | Twitter/X | Thread | "How to set up a pre-publish video security checklist (3 steps)" |
| Fri | Dev.to | Tutorial | "TruffleHog is great for code. Here's why scanning your videos is harder." |

### Week 4

| Day | Platform | Format | Content |
|-----|----------|--------|---------|
| Mon | LinkedIn | Article | "The real cost of a leaked API key: a breakdown of what can happen in the first hour" |
| Wed | Twitter/X | Single post | "Quick poll: do you scan your screen recordings before sharing? 👀" |
| Fri | Dev.to | Listicle | "7 tools every security-conscious developer should have in their workflow (including one you probably don't)" |

---

## Platform-Specific Strategy

### Twitter/X (Primary — fastest feedback loop)
- **Cadence:** 3–4 posts per week
- **Focus:** Industry commentary, leak incident reactions, product updates, developer polls
- **Best posting time (SG time):** 8–10 PM (catches US morning)

### Dev.to (SEO + Waitlist)
- **Cadence:** 1 article per week
- **Focus:** Long-form tutorials, comparisons, educational deep dives
- **CTA in every post:** "Try LeakLens" link with UTM parameter

### LinkedIn (B2B / DevRel)
- **Cadence:** 2 posts per week
- **Focus:** Thought leadership for security-aware developer advocates and technical content leads
- **Best posting time:** 9 AM SGT (catches US East Coast end of day)

### Reddit (r/programming, r/devops, r/sideproject)
- **Cadence:** 1–2 posts per month
- **Focus:** High-effort text posts that contribute to existing discussions, not pure product announcements
- **Rule:** Always contribute to a discussion before mentioning LeakLens. Zero spam.

### YouTube (Long-term)
- **Format:** 10–15 min "behind the build" documentary + tutorial
- **Content:** "How I built LeakLens" (developer story) + "How to use LeakLens" (walkthrough)
- **Timing:** Launch Month 3, once product has real users and testimonials

---

## UTM Parameters

All outbound links must use UTM parameters for funnel tracking:

| Source | Medium | Campaign |
|--------|--------|---------|
| Dev.to article | dev-to | [article-slug] |
| Twitter/X | twitter | launch / weekly-content |
| LinkedIn | linkedin | launch / thought-leadership |
| Reddit | reddit | launch / community |
| Product Hunt | product-hunt | launch |
| Hacker News | hn | launch |

**UTM URL format:** `https://leaklens.com/?utm_source={source}&utm_medium={medium}&utm_campaign={campaign}`

---

## Community Engagement Plan

### Passive Monitoring (Set up alerts)
- Google Alert: "API key leaked YouTube video"
- Google Alert: "AWS keys exposed tutorial"
- Reddit: r/programming, r/devops, r/cybersecurity (comment activity)
- Hacker News: "Ask HN: How do you prevent leaking API keys in videos?"

### Active Engagement Rules
1. **Never post LeakLens links in threads where someone already has a problem** — it looks opportunistic. Wait 24h or engage without a link.
2. **When someone posts about a real credential leak:** Offer genuine help (rotate keys, check CloudTrail) + mention LeakLens as prevention. Lead with empathy.
3. **When someone asks about video redaction:** Answer the question fully, mention LeakLens at the end if relevant. No links in first 3 responses.
4. **When someone criticizes LeakLens:** Engage respectfully. Don't defend. Ask what would make it better.

---

## Content Production Requirements

### Assets to Produce at Launch
- [ ] Demo GIF (60s, macOS recording showing scan → timeline → redaction → export)
- [ ] Screenshot kit: 6 screenshots of UI (sidebar, timeline, flag detail, redaction panel, export dialog, settings)
- [ ] Product Hunt assets: Icon, cover image, tagline
- [ ] Social profile setup: Twitter/X (@leaklens), LinkedIn company page, Dev.to
- [ ] Landing page (can be a single page — name, demo GIF, waitlist form, 3 feature bullets)

### Assets to Produce Month 2–3
- [ ] Full demo video (3–5 min, walking through real scan on a real video)
- [ ] Comparison page: "LeakLens vs. manual scrubbing vs. enterprise tools"
- [ ] Detector list page (all 800+ supported credential types)
