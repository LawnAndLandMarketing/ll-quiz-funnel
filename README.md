# L&L Growth Path Quiz Funnel

**Live URL:** https://ll-quiz-funnel.vercel.app  
**Vercel Project:** `ll-quiz-funnel` (`prj_lTgU2rBSu8KIhyBWF8pNOoX9bM85`)  
**Stack:** Pure HTML/CSS/JS — zero dependencies, zero build step

---

## What This Is

A quiz funnel that routes landscape business owners to one of four Lawn & Land Marketing programs based on their answers. Built on the "quiz funnel" psychology model — micro-commitments, belief seeding, objection pre-handling — adapted for a B2B service sale.

**The core insight:** Cold traffic sent directly to a "Book a Call" page converts at ~1-2%. Cold traffic that answers 8 questions first is no longer cold — they've invested, self-identified their pain, and are arriving at the offer already primed.

---

## The 4 Results

| Result | Trigger Profile | Program | CTA |
|--------|----------------|---------|-----|
| **Authority** | 3+ crews, $750K+ rev, running ads, $3.5K+/mo budget | $2,400/mo + $2,000 setup | Apply / Book Call |
| **Growth** | 2 crews, $200K–$750K, inconsistent leads, $1.5–3.5K/mo | $1,200/mo + $1,500 setup | Apply / Book Call |
| **Seed** | Solo–1 crew, $50K–$200K, no real web presence | Waitlist (price TBD) | Join Waitlist |
| **Not a Fit** | Under $50K rev OR under $500/mo budget | Free resources + come back later | Free Starter Guide |

---

## The 8 Questions

| # | Question | Purpose |
|---|----------|---------|
| 1 | How many trucks/crews? | Easy opener — zero cognitive load, starts the yes-chain |
| 2 | Annual revenue? | Hard qualifier — under $50K flags "not a fit" |
| 3 | #1 growth goal? | Aspirational — reveals intent and program fit |
| 4 | Current website situation? | Diagnoses digital maturity |
| 5 | Paid ads experience? | Authority/Growth differentiator |
| 6 | Biggest marketing struggle? | Pain point resonance |
| 7 | Monthly marketing budget? | Hard qualifier — under $500/mo flags "not a fit" |
| 8 | Timeline / urgency? | Captured for sales context, doesn't affect result |

---

## Scoring Logic

Each answer (Q1–Q7) casts a vote for: `authority`, `growth`, `seed`, or `notfit`.

**Hard overrides (not-a-fit wins):**
- 2+ `notfit` answers → route to Not a Fit
- 1 `notfit` answer AND fewer than 2 `authority` or `growth` signals → Not a Fit

**Otherwise:** Majority wins across authority / growth / seed.

```js
function computeResult() {
  const scores = { authority: 0, growth: 0, seed: 0, notfit: 0 };
  for (let i = 1; i <= 7; i++) {
    if (answers[i]) scores[answers[i].score]++;
  }
  if (scores.notfit >= 2) return 'notfit';
  if (scores.notfit === 1 && scores.authority < 2 && scores.growth < 2) return 'notfit';
  return Object.entries(scores)
    .filter(([k]) => k !== 'notfit')
    .sort((a, b) => b[1] - a[1])[0][0];
}
```

---

## UX Flow

```
Intro Screen
    ↓ "Let's Find My Path"
Q1 → Q2 → Q3 → Q4 → Q5 → Q6 → Q7 → Q8
    ↓ "See My Results"
Loading Screen (3.2s — social proof / testimonial displayed)
    ↓
Result Screen (Authority | Growth | Seed | Not a Fit)
```

- Back navigation works on all questions
- Answers are preserved when navigating back
- Progress bar updates per question
- Smooth fade-in animations between steps

---

## Design System

Follows the **L&L Digital UI Standard** exactly:

- **Background:** `#040a04` (near-black, green-tinted)
- **Accents:** L&L Lime `#ACE71D` + L&L Green `#5DCA49`
- **Depth layers:** SVG grain overlay (3.5% opacity) + radial ambient glow
- **Cards:** Glass morphism — `rgba(255,255,255,0.04)` fill, `backdrop-filter: blur(12px)`, green-tinted border
- **Typography:** Rethink Sans 800 Italic (headings) + Inter (body)
- **Buttons:** Lime-to-green gradient (primary), ghost green (secondary)

---

## TODO / Build Out

### Immediate
- [ ] Wire real CTA URLs (apply page, book-a-call, seed waitlist, free resources)
- [ ] Set Seed Program price point when locked in
- [ ] Add real client testimonials to loading screen

### Phase 2 — Conditional Logic Enhancements
- [ ] Add tie-breaking logic (what happens when authority == growth == 2?)
- [ ] Weight Q2 (revenue) and Q7 (budget) more heavily — they're the strongest signals
- [ ] Add "edge case" routing: e.g., 3+ crews but budget under $1.5K → Growth not Authority
- [ ] Consider adding Q9: "What type of work do you primarily do?" (residential / commercial / both) — affects which program angle to lead with on the result page

### Phase 3 — Lead Capture
- [ ] Add email capture before showing results ("Where should we send your growth plan?")
- [ ] Connect to N8N webhook → route lead to correct ClickUp list based on result
- [ ] Tag contact in GHL CRM with quiz result + answers
- [ ] Trigger automated follow-up sequence per program path

### Phase 4 — Optimization
- [ ] A/B test question order (currently: easy opener → revenue → goal → website → ads → struggle → budget → urgency)
- [ ] Track per-question drop-off (instrument with analytics)
- [ ] Add "breather" slide with social proof after Q4 (per quiz funnel best practice)
- [ ] Add urgency/scarcity element after Q8 before loading screen

### Phase 5 — Personalization
- [ ] Personalize result page headline with their answer from Q3 (their stated goal)
- [ ] Surface back their biggest struggle (Q6 answer) in the result copy
- [ ] Segment retargeting ads based on quiz answers (Meta Custom Audiences)

---

## File Structure

```
ll-quiz-funnel/
├── index.html        # The entire quiz — single self-contained file
└── README.md         # This file
```

No build process. No dependencies. Edit `index.html` and deploy.

---

## Deployment

**Vercel (current):**
```bash
# Deploy to production
vercel deploy --prod
```

Or push to this repo — connect to Vercel for auto-deploys.

**Manual API deploy (what Kai uses):**
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

---

## Context for Claude (when building out)

**What Lawn & Land Marketing does:**  
Digital marketing agency exclusively for lawn & landscape companies ($750K–$8M revenue). ~54 clients, ~$88K MRR, 97% retention, $3M+ ad spend managed (95% Meta).

**The three programs:**
- **Authority** ($2,400/mo + $2K setup) — Full Meta/Google ad management, landing pages, CRM, dedicated AM. For established operators ready to scale.
- **Growth** ($1,200/mo + $1.5K setup) — Website + local SEO + CRM + reviews. For operators who need a real digital foundation before running ads.
- **Seed** (price TBD) — AI website + hosting + LSA management guidance + Service Area Expert CRM. For new/small operators who need the basics done right.

**The target customer:**  
Landscape business owners. Hands-on operators, not marketers. They respond to direct, no-fluff language. "Dominate your market" > "achieve digital growth synergies". They care about leads, jobs booked, and ROI — not impressions or brand awareness.

**The quiz psychology being applied:**
1. Low-friction opener (crews = dead easy to answer)
2. Hard qualifiers early (revenue Q2, budget Q7) to protect sales team's time
3. Aspirational question (Q3) comes after they're already clicking — hits harder when they're invested
4. Loading screen = captive attention — use for best testimonial
5. Result page = personalized recommendation, not a sales pitch — they arrived at this conclusion themselves

**Brand voice:** Direct, confident, green industry native. Never fluffy. "We know your world" energy.
