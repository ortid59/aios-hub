# Advisor Niche Lead Engine — Project Scope

**Status:** Scoping (not yet committed to build)
**Created:** 2026-04-16
**Owner:** David Ortiz
**Origin:** Concept surfaced in 2026-04-16 Todd Howland call. Todd proposed partnership; David is scoping a solo build with Todd as possible early customer, not partner.

---

## The Problem

Financial advisors trying to target hyper-specific client niches (e.g., *"greater Miami SMBs, $2M+ revenue, fewer than 25 employees, non-professional industries"* — plumbers, construction owners, etc.) can't get clean data from existing tools:

- **Apollo, ZoomInfo, and "top-3 AI lead gen" tools** give name lists, not leads — Todd's direct quote: *"They're cold as hell. They're basically electronic names lists — one step before lead."*
- **Bounce rates on purchased data are unusable.** Todd's trial with a vendor: **70% send-fail rate** on emails. Owner emails often wrong or nonexistent.
- **Manual Yellow Pages + Google research beat paid tools** on completeness when Todd spot-checked them.
- **Ultra-specific criteria break every vendor** — Todd's test query (Miami SMBs, $2M+ rev, <25 employees, non-professional industries) got no real answer from a Sarasota-via-India vendor.

No tool on the market today combines **targeting precision + data cleanliness + compliance-safe outreach + AI qualification** specifically for financial advisors.

---

## The Concept

A three-layer system:

### Layer 1 — Criteria → Enriched, Verified Lead List
- Advisor enters criteria in plain English → LLM translates to structured query.
- **Waterfall across data sources** (don't rely on any single one):
  - **Apollo API** — baseline firmographic + person data
  - **PeopleDataLabs / FullContact** — person enrichment fill-ins
  - **Data Axle / ReferenceUSA** — deep SMB data with revenue + employee counts
  - **IRS Form 5500** (public) — gold mine for finding SMBs with retirement plans (indicator of owner wealth)
  - **State business registrations** (FL Sunbiz, etc.) — free, authoritative ownership data
  - **Google Places API** — local business matching
- **Email verification before any send** (NeverBounce / ZeroBounce) → kills the 70% bounce problem.
- Output: enriched CSV/GHL-ready list with confidence scores per row.

### Layer 2 — Compliance-Safe Outreach (David's Moat)
- Pre-built outreach sequences (email + LinkedIn + SMS) with FINRA/SEC-approved copy.
- This is already the core of David's GHL offering — existing content engine + compliance skill.
- **This is the differentiator.** Clay.com doesn't do this. Neither does Apollo.
- Multi-channel: email for cold, FB/IG/LinkedIn ads as parallel lead-collection mechanism (Todd pivoted here at 37:00 in the call — ads + qualifying forms may actually beat scraping).

### Layer 3 — AI Qualification → Advisor Handoff
- Conversational AI bot handles first 3–5 qualifying questions before advisor ever sees the lead.
- Only pre-qualified leads land in advisor's inbox/GHL pipeline.
- David already builds this (conversational AI agents in GHL) — integration work, not new invention.

---

## Build vs. Buy Analysis

**Clay.com already does ~60% of Layer 1.** Pricing $149–$720/mo. It does the waterfall enrichment and has a decent criteria UI.

**Honest position:** Don't compete with Clay. **Sit on top of it** (or Apollo's API directly). The product is NOT the scraper — it's the **advisor-specific persona templates + compliance layer + AI qualification + GHL handoff**.

| Component | Build / Buy / Wrap |
|---|---|
| Criteria UI + LLM parser | Build (simple) |
| Data waterfall + enrichment | **Wrap Clay or Apollo API** — don't rebuild |
| Email verification | Buy (NeverBounce API) |
| Compliance-safe outreach copy | **Build** (David's edge, already partially built) |
| Multi-channel sequences | Wrap GHL + Smartlead/Instantly |
| AI qualification bot | **Build** (extend existing GHL agents) |
| Advisor persona templates (LTC prospect, 65+ divorcee, SMB owner, etc.) | **Build** (David's edge) |

---

## Persona Templates (Initial Target List)

These are the pre-built "niches" David ships with. Each is a pre-configured criteria set + compliance-safe outreach template:

1. **SMB owner, $1M+ revenue, under 25 employees, non-professional industry** (Todd's exact case — blue-collar wealth)
2. **LTC-eligible 65+** (for LifeBridge LTC advisors)
3. **Recent divorcees / widowers 55+** (wealth transition moment)
4. **Business owners with active 401(k) plan and no recent advisor change** (Form 5500 + advisor data)
5. **Sold-business owners in last 24 months** (liquidity event)
6. **Executives at companies with recent liquidity events** (IPO, acquisition)

---

## Revenue Model (Todd's Proposal — Worth Keeping)

- **$1,000–$3,000/month** subscription per advisor → covers X leads/month through the funnel.
- **Revenue share** on closed deals (5–10% typical for wholesaling-style arrangements).
- **Todd is Customer Zero** — paid pilot, not partner. Feedback loop + case study.

---

## Fit with David's Existing Business

- **LifeBridge LTC recruiting** — David uses this tool himself to find advisor wholesaling leads. Dogfooding = free testing.
- **Advisor AI Partners positioning** — fits the "Fractional AI Officer" brand. Sellable alongside SCC, Follow-Up Machine, content engine.
- **September deadline** — if MVP is ready by July, David has Aug–Sept to close 3–5 paying pilots before Lisbon relocation (Chamber pipeline goes away in September).

---

## Phased Build

### MVP (4–6 weeks, one advisor persona)
- Pick ONE persona (likely "SMB owner, non-professional industries" — Todd's use case).
- Apollo API + NeverBounce + one compliance-safe email sequence + basic AI qualifier via GHL.
- Manual criteria input (no LLM parser yet).
- Ship to Todd as paid pilot.

### V1 (2–3 months)
- 3–5 persona templates.
- LLM criteria parser.
- Multi-channel (email + LinkedIn + FB ads).
- Self-serve onboarding for advisors.

### V2 (future)
- Full Clay-wrapping or direct Apollo+PDL+Data Axle integration.
- Revenue-share billing automation.
- Advisor dashboard with pipeline metrics.

---

## Cost Estimate (Rough)

| Item | Monthly |
|---|---|
| Apollo API (or Clay) | $150–$500 |
| PeopleDataLabs / enrichment | $100–$400 |
| NeverBounce | $50–$200 |
| Claude/GPT API for qualification + parser | $50–$200 |
| Hosting + GHL (already paid) | $0 |
| **Total run rate (per advisor)** | **~$400–$1,300/mo** |

At $1,000–$3,000/mo pricing per advisor, **margin is healthy even at one customer.** With 5 advisors, margin is substantial.

---

## Open Questions / Decisions Needed

1. **Clay wrapper or direct API integration?** Clay is faster to MVP, direct APIs are more defensible long-term.
2. **Todd: pilot customer or don't bother?** David's read: Todd is valuable as a design-partner voice + paid pilot, but not as a co-builder or profit partner.
3. **Compliance depth** — How deep to go on FINRA/SEC review? Existing compliance skill may be sufficient; may need an actual compliance officer review for launch.
4. **Which persona first?** SMB non-professional (Todd) vs. LTC-eligible 65+ (LifeBridge dogfood). **Recommend LTC-eligible** — David uses it himself immediately, proof = closed LifeBridge cases.
5. **Lisbon relocation impact** — September cutoff. Build timeline must allow for disruption.

---

## Next Steps (If Greenlit)

1. **Pick the first persona** (recommend LTC-eligible 65+ for dogfooding).
2. **Run `/create-plan` to produce a detailed MVP build plan** (4–6 weeks, one persona, Apollo + NeverBounce + GHL).
3. **Decide on Todd** — offer him the SMB non-professional persona as paid pilot in V1, not MVP.
4. **Set MRR target** — e.g., "3 paying pilots by Labor Day 2026."

---

## Decision Needed From David

- [ ] Proceed to `/create-plan` for MVP?
- [ ] First persona: LTC-eligible (dogfood) vs. SMB non-professional (Todd's case)?
- [ ] Todd role: paid pilot / design partner / not involved?
- [ ] Build vs. wrap Clay?
