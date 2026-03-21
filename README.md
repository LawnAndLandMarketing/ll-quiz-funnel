# L&L Growth Path Quiz Funnel

**Live URL:** https://ll-quiz-funnel.vercel.app  
**Vercel Project:** `ll-quiz-funnel` (`prj_lTgU2rBSu8KIhyBWF8pNOoX9bM85`)  
**Stack:** Pure HTML/CSS/JS — zero dependencies, zero build step  
**Version:** v2 — Revenue-First Routing (March 21, 2026)

---

## What This Is

A quiz funnel that routes landscape business owners to one of four Lawn & Land Marketing programs based on 8 questions. Built on quiz funnel psychology — micro-commitments, belief seeding, objection pre-handling — adapted for a B2B service sale to green industry operators.

**The core insight:** Cold traffic sent directly to "Book a Call" converts at ~1–2%. Traffic that answers 8 questions first is no longer cold — they've invested, self-identified their pain, and arrive at the offer already primed.

---

## Program Definitions

| Program | Monthly | Setup | Target Profile |
|---------|---------|-------|----------------|
| **Authority** | $2,400/mo | $2,000 | $750K+ revenue, established operators ready to scale ads |
| **Growth** | $1,200/mo | $1,500 | $200K–$750K revenue, need digital foundation before ads |
| **Seed** | $697/mo | $997 | $100K–$200K revenue, solo/1-crew, need basics done right |
| **Not a Fit** | — | — | Wrong industry, under $100K revenue, or unwilling to invest |

### Authority — $2,400/mo + $2,000 Setup
Flagship for established operators ready to scale ads. Target: $750K+ revenue.
- Meta & Google Ads management (full campaign execution)
- High-converting landing pages
- CRM + pipeline automation via Service Area Expert
- Monthly strategy calls + real-time reporting
- Dedicated account manager
- GBP optimization + local SEO

### Growth — $1,200/mo + $1,500 Setup
For operators who've proven their business but need a digital foundation. Target: $200K–$750K revenue.
- Custom lead-generating website
- CRM setup + lead tracking via Service Area Expert
- GBP optimization + local SEO
- Review generation system
- Monthly reporting + strategy calls
- Clear upgrade path to Authority

### Seed — $697/mo + $997 Setup
Entry-level for solo/1-crew operators. Operationally isolated from AM team. Target: $100K–$200K revenue.
- AI-built professional website + hosting + maintenance
- Service Area Expert CRM provisioned and configured
- Automated GBP posting (AI-generated, scheduled)
- LSA setup guidance + review request sequences
- Self-serve knowledge base + community access
- No dedicated AM, no custom strategy calls — ticket-based support only
- **Graduation trigger:** $250K annual revenue run rate → Growth consultation call

### Not a Fit
**Three independent disqualifiers — any single trigger routes here:**
1. **Wrong industry** — not in green industry (Q1, immediate exit)
2. **Under $100K annual revenue** — $697/mo would exceed sustainable marketing spend ratio
3. **Unwilling to invest** — self-selects "can't commit to monthly marketing investment"

**Two distinct result pages:**
- **Wrong industry** — clean exit, no free resources, respectful but clear
- **Too early / not ready** — 3 free action steps, free starter guide, invitation to return at $100K+

---

## Routing & Scoring Logic

### The Hierarchy (Revenue-First)

| Priority | Signal | Role |
|----------|--------|------|
| Gate | Q1: Industry | Hard disqualifier — not green industry = immediate Not a Fit |
| 1st | Q3: Revenue | **Primary gate** — $750K+ → Authority · $200K–$750K → Growth · $100K–$200K → Seed · Under $100K → Not a Fit |
| 2nd | Q2: Crews | Supporting signal — reinforces revenue but **never overrides it** |
| Floor | Q8: Investment | Disqualifier only — "can't commit" = Not a Fit. Otherwise does not change tier. |
| Context | Q4–Q7 | Shape result page copy + personalization. Do NOT move routing. |

**Key principle:** Revenue determines the tier. Everything else is context.

### Edge Case Routing

| Scenario | Signals | Routes To | Rationale |
|----------|---------|-----------|-----------|
| Big company, small budget | 5 crews, $1.2M rev, $1,500/mo budget | **Authority** | Revenue and scale indicate Authority. Budget is a sales conversation — can scope to LSA or Meta-only. |
| Small crew, big budget | 2 crews, $400K rev, $3,500+/mo budget | **Growth** | Revenue says Growth. High budget doesn't upgrade the tier. Result page highlights graduation path to Authority at $750K. |
| Revenue/crew mismatch | Solo op, $800K rev | **Authority** | Revenue wins. Result page acknowledges growth stage and notes capacity assessment at onboarding. |
| On the fence | 3 crews, $500K rev | **Growth** | Revenue is under $750K threshold. Crews support readiness but don't override. Growth with Authority upgrade messaging. |
| Big company, no ad experience | 5 crews, $1M+ rev, never ran ads | **Authority** | Revenue wins. Result page copy reframes: acknowledges referral-driven success, positions ads as accelerator not gamble, addresses $1–2M referral ceiling. |

### Scoring Code

```js
function computeResult() {
  // Q8: investment mindset floor
  if (answers[8]?.score === 'notfit_invest') return 'notfit-early';

  // Q3: revenue primary gate (absolute)
  if (answers[3]) {
    const rev = answers[3].score;
    if (rev === 'notfit_revenue') return 'notfit-early';
    if (rev === 'seed') return computeWithRevenue('seed');
    if (rev === 'growth') return computeWithRevenue('growth');
    if (rev === 'authority') return computeWithRevenue('authority');
  }
  return majorityVote(); // fallback
}

function computeWithRevenue(revTier) {
  const crews = answers[2]?.score;
  const adsExp = answers[6]?.meta;

  // Solo + high revenue → still Authority (revenue wins)
  if (revTier === 'authority' && crews === 'seed') return 'authority';

  // 3+ crews + <$750K → Growth (revenue wins over crew count)
  if (revTier === 'growth' && crews === 'authority') return 'growth';

  return revTier; // standard
}
```

---

## The 8 Questions

| # | Question | Role |
|---|----------|------|
| **Q1** | Does your company provide outdoor/green industry services? | **Hard gate** — No = immediate Not a Fit (wrong industry) |
| **Q2** | How many trucks/crews? | Low-friction opener, starts yes-chain. Supporting signal only. |
| **Q3** | Annual revenue? | **Primary routing gate.** Under $100K = Not a Fit. Tier boundaries at $100K/$200K/$750K. |
| **Q4** | #1 growth goal? | Aspirational — hits harder now that they're invested. Informs result page copy. |
| **Q5** | Current website situation? | Diagnoses digital maturity. Context signal. |
| **Q6** | Paid ads experience? | Authority/Growth context. Never-ran-ads + high revenue = personalized copy on Authority result. |
| **Q7** | Biggest marketing struggle? | Pain point resonance. Echoed back on result page. |
| **Q8** | Monthly investment commitment? | Hybrid mindset + dollar frame (Hormozi-inspired). "Can't commit" = Not a Fit. Otherwise doesn't override revenue. |

### Q8 Investment Options (Hormozi framing)
- "I can't commit to a monthly marketing investment right now" → **Not a Fit**
- "$500–$1,000/mo — ready to build the foundation" → Seed signal
- "$1,000–$2,500/mo — ready to grow consistently" → Growth signal
- "$2,500+/mo — ready to dominate my market" → Authority signal

---

## Personalization (Live)

Result pages adapt based on Q4 (goal) and Q6/Q7 answers:

- **Authority + never-ran-ads + high revenue** → Reframes ads as accelerator, references $1–2M referral ceiling, emphasizes transparent ROI
- **Authority + solo-op + high revenue** → Acknowledges efficient operation, notes capacity assessment at onboarding
- **Growth + high budget** → Highlights fast-track graduation path to Authority at $750K
- **Standard** → Goal and struggle echoed back in personalization callout block

---

## Build Roadmap

### Sprint 1 ✅ Done — Lead Capture + Webhook
- [x] Email capture gate before results screen ("Where should we send your growth plan?")
- [x] Fields: First Name, Last Name, Email, Phone, Company Name
- [x] Client-side form validation with inline error messages
- [x] N8N webhook fires on submit: full lead info + all 8 answers + computed result + routing signals + timestamp
- [x] Webhook is non-blocking — UX never waits on it; silent fail if it errors
- [x] **TODO: Paste N8N webhook URL into `N8N_WEBHOOK_URL` constant in index.html**
- [ ] N8N workflow: receive payload → create/update contact in Service Area Expert (GHL)
- [ ] N8N workflow: tag contact in SAE with quiz result + key answer signals
- [ ] N8N workflow: trigger automated follow-up sequence per program path (Authority / Growth / Seed / Not a Fit)

### Sprint 2 ✅ Done — Scoring Logic (v2)
- [x] Revenue-first routing hierarchy (replaces equal-weight majority vote)
- [x] Q1 industry gate with immediate Not-a-Fit exit
- [x] Q3 revenue as primary gate ($100K/$200K/$750K thresholds)
- [x] Q2 crews as supporting signal (never overrides revenue)
- [x] Q8 investment as floor-only gate with hybrid mindset/dollar frame
- [x] All 5 edge cases wired in with correct routing
- [x] Two distinct Not-a-Fit result page variants
- [x] Personalization callouts on Authority + Growth result pages

### Sprint 3 — Conversion Optimization
- [ ] Per-question drop-off tracking (analytics instrumentation)
- [ ] Breather / social proof slide after Q4 (midpoint fatigue point)
- [ ] Urgency / scarcity element before loading screen
- [ ] Richer personalization: echo Q7 struggle verbatim in result headline

### Sprint 4 — Retargeting Data Play
- [ ] Segment quiz non-buyers into Meta Custom Audiences by Q7 (struggle) + Q4 (goal) — pull from SAE tags
- [ ] Build "didn't finish quiz" retargeting audience separately
- [ ] Run retargeting ads matched to stated pain point + goal

### Open Questions
- [ ] Seed: tech stack for automated GBP posting at scale (Roshi/n8n/GHL feasibility)
- [ ] Seed: community format — Facebook group vs. channel inside Service Area Expert
- [ ] Whether to add residential/commercial question for result page personalization
- [ ] Seed waitlist mechanics and controlled rollout plan
- [ ] Whether monthly AI performance snapshot email should be added to Seed (low effort, high perceived value)
- [ ] Wire real CTA URLs (apply page, book-a-call, seed waitlist, free resources)
- [ ] Real client testimonials for loading screen

---

## Design System

Follows the **L&L Digital UI Standard** exactly:
- **Background:** `#040a04` (near-black, green-tinted)
- **Accents:** L&L Lime `#ACE71D` + L&L Green `#5DCA49`
- **Depth layers:** SVG grain overlay (3.5% opacity) + radial ambient glow
- **Cards:** Glass morphism — `rgba(255,255,255,0.04)` fill, `backdrop-filter: blur(12px)`, green-tinted border
- **Typography:** Rethink Sans 800 Italic (headings) + Inter (body)

---

## Context for Claude (when building out)

**What Lawn & Land Marketing does:**
Digital marketing agency exclusively for lawn & landscape companies. ~54 clients, ~$88K MRR, 97% retention, $3M+ ad spend managed (95% Meta). Green industry only — they turn away anyone outside it.

**The three programs:**
- **Authority** ($2,400/mo + $2K setup) — Full Meta/Google ad management, landing pages, CRM, dedicated AM. For established operators ready to scale.
- **Growth** ($1,200/mo + $1.5K setup) — Website + local SEO + CRM + reviews. For operators who need a real digital foundation before running ads.
- **Seed** ($697/mo + $997 setup) — AI website + hosting + automated GBP posting + LSA guidance + Service Area Expert CRM. Self-serve model, no dedicated AM, ticket-based support. For solo/1-crew operators $100K–$200K revenue. Graduation to Growth at $250K run rate.

**Routing philosophy:**
Revenue is the single strongest signal. Crew size is supporting context. Budget is a mindset floor only — it can disqualify but can't upgrade a tier. Industry is an absolute gate. Questions Q4–Q7 inform messaging and personalization, not routing.

**The target customer:**
Landscape business owners. Hands-on operators, not marketers. They respond to direct, no-fluff language. They care about leads, jobs booked, and ROI — not impressions or brand awareness.

**Quiz psychology applied:**
1. Industry gate first — protects the funnel from irrelevant leads immediately
2. Low-friction opener (crews) — dead easy, starts the yes-chain
3. Revenue gate early (Q3) — primary qualifier, sets the tier
4. Aspirational question (Q4) after they're clicking — hits harder when invested
5. Loading screen = captive attention — best testimonial goes here
6. Result page = personalized recommendation, not a sales pitch
7. Investment question uses Hormozi framing — not a budget question, a commitment question

**Brand voice:** Direct, confident, green industry native. Never fluffy. "We know your world" energy. "Leads, jobs booked, ROI" language only.

---

## Deployment

```bash
VERCEL_TOKEN="your_token"
TEAM_ID="team_sPQaPdEOVQaR42djHV2cTm51"
HTML_B64=$(base64 -i index.html | tr -d '\n')

curl -X POST "https://api.vercel.com/v13/deployments?teamId=${TEAM_ID}" \
  -H "Authorization: Bearer ${VERCEL_TOKEN}" \
  -H "Content-Type: application/json" \
  -d "{
    \"name\": \"ll-quiz-funnel\",
    \"target\": \"production\",
    \"files\": [{\"file\": \"index.html\", \"data\": \"${HTML_B64}\", \"encoding\": \"base64\"}],
    \"projectSettings\": {\"framework\": null}
  }"
```
