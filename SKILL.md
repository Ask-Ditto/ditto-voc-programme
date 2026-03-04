---
name: ditto-voc-programme
description: >
  Build and run an always-on Voice of Customer programme using Ditto's
  synthetic persona platform (300K+ AI personas, 92% overlap with real
  focus groups). Covers monthly pulse checks, quarterly deep dives, ad-hoc
  probes, and longitudinal trend tracking. Produces 6 VoC deliverables:
  customer journey map, pain priority matrix, language library, unmet needs
  report, decision criteria hierarchy, and product feedback synthesis.
  Designed for CS managers, VoC programme managers, CX/UX researchers,
  and product managers running continuous feedback loops. Use when the
  user mentions voice of customer, VoC, customer feedback programme,
  pulse check, customer journey map, NPS deep dive, churn analysis,
  or continuous customer research.
allowed-tools: Bash(curl *), Bash(python3 *), Read, Grep, WebFetch
argument-hint: "[product, problem space, or pulse check brief]"
---

# Ditto for Voice of Customer Programmes

Build a continuous VoC research programme — monthly pulse checks, quarterly
deep dives, longitudinal trend tracking — using Ditto's 300,000+ synthetic
personas. Directly from the terminal.

**Full documentation:** https://askditto.io/claude-code-guide

## What Ditto Does

Ditto maintains 300,000+ AI-powered synthetic personas calibrated to census
data across USA, UK, Germany, and Canada. You ask them open-ended questions
and get qualitative responses with the specificity of real customer interviews.

- **92% overlap** with traditional focus groups (EY Americas validation)
- Monthly pulse check: **15 minutes** (3Q, 6 personas)
- Quarterly deep dive: **45 minutes** (7Q, 10 personas)
- Traditional VoC programme: $200,000+/year

## The Core Principle

**VoC is NOT an annual event. It is a continuous 2-hour/month habit.**

Research reports that are never translated into deliverables are dead on
arrival. Every finding must become a changed deliverable, updated positioning,
or product action item.

## Quick Start (Free Tier)

Get a free API key — no credit card, no sales call:

```bash
curl -sL https://app.askditto.io/scripts/free-tier-auth.sh | bash
```

Free keys (`rk_free_`): ~12 shared personas, no demographic filtering.
Paid keys (`rk_live_`): custom groups, demographic filtering, unlimited studies.

## API Essentials

**Base URL:** `https://app.askditto.io`
**Auth header:** `Authorization: Bearer YOUR_API_KEY`
**Content-Type:** `application/json`

## The Always-On Research Calendar

| Study Type | Frequency | Personas | Questions | Time | Purpose |
|-----------|-----------|----------|-----------|------|---------|
| **Pulse Check** | Monthly | 6 | 3 | ~15 min | Quick read on evolving sentiment |
| **Deep Dive** | Quarterly | 10 | 7 | ~45 min | Comprehensive VoC study |
| **Targeted Probe** | Ad hoc | 8 | 5 | ~25 min | Investigate a specific signal |
| **Pre-Launch** | Before launches | 10 | 7 | ~45 min | Concept validation |
| **Post-Launch** | 2 weeks after | 10 | 5 | ~30 min | Fresh group sentiment |

**Total monthly investment: ~2 hours.**

### Monthly Rhythm

- **Week 1:** Run pulse check (15 min study + 15 min deliverable update)
- **Week 2:** Update Language Library and Pain Matrix with pulse findings
- **Week 3:** (Quarterly only) Run deep dive
- **Week 4:** Distribute updated deliverables to stakeholders

## The VoC Workflow (6 Steps)

Same API workflow for all study types — only the questions and group size change.

### Step 1: Recruit (or Reuse) Your Panel

```bash
curl -s -X POST "https://app.askditto.io/v1/research-groups/recruit" \
  -H "Authorization: Bearer $DITTO_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "name": "US SaaS Users 25-50 - VoC Panel",
    "group_size": 10,
    "filters": {
      "country": "USA",
      "industry": ["Technology"],
      "age_min": 25,
      "age_max": 50
    }
  }'
```

Save the `uuid`. Use `group_size` (not `size`), group `uuid` (not `id`).

**Tip:** Reuse the same group for pulse checks (consistency). Recruit fresh
groups for deep dives (new perspectives). Mix both for longitudinal tracking.

### Step 2: Create Study

```bash
curl -s -X POST "https://app.askditto.io/v1/research-studies" \
  -H "Authorization: Bearer $DITTO_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "title": "VoC Deep Dive Q1 2026 - [Product Name]",
    "objective": "Quarterly deep dive on customer pain points, priorities, and unmet needs",
    "research_group_uuid": "UUID_FROM_STEP_1"
  }'
```

Response nests under `data.study` — access via `response["study"]["id"]`.

### Step 3: Ask Questions (One at a Time)

```bash
curl -s -X POST "https://app.askditto.io/v1/research-studies/STUDY_ID/questions" \
  -H "Authorization: Bearer $DITTO_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{"question": "Your question here"}'
```

**Do NOT change question order.** Each question builds on conversational
context established by earlier answers.

### Step 4: Poll Until Complete

```bash
curl -s "https://app.askditto.io/v1/jobs/JOB_ID" \
  -H "Authorization: Bearer $DITTO_API_KEY"
```

Wait **45-50 seconds** before first poll, then **20 seconds** intervals.
Poll **ONE** job_id as proxy — all jobs finish together.

### Step 5: Complete the Study

```bash
curl -s -X POST "https://app.askditto.io/v1/research-studies/STUDY_ID/complete" \
  -H "Authorization: Bearer $DITTO_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{"force": false}'
```

### Step 6: Extract Deliverables + Get Share Link

```bash
curl -s "https://app.askditto.io/v1/research-studies/STUDY_ID/questions" \
  -H "Authorization: Bearer $DITTO_API_KEY"
```

```bash
curl -s -X POST "https://app.askditto.io/v1/research-studies/STUDY_ID/share" \
  -H "Authorization: Bearer $DITTO_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{"enabled": true}'
```

## Deep Dive Question Framework (7 Questions)

The quarterly deep dive. Each question feeds specific deliverables.

**Q1 — Journey Mapping**
> "Tell me about the last time you [relevant activity]. Walk me through the
> experience from start to finish. What went well? What was frustrating?"

Feeds: Customer Journey Map (touchpoints classified as pain/delight/neutral).

**Q2 — Priority Pain**
> "If you could wave a magic wand and fix ONE thing about [problem space],
> what would it be? Why that above everything else?"

Feeds: Pain Priority Matrix. If 7/10 independently identify the same
frustration, it is a high-confidence positioning opportunity.

**Q3 — Current Solutions**
> "How do you currently solve [problem]? What tools, people, or workarounds
> do you use? What do you wish you could do differently?"

Feeds: Competitive landscape, Language Library (solution language).

**Q4 — Experience Benchmarks**
> "Think about the BEST [product/service] experience you've ever had in
> any category. What made it great? Now think about the WORST.
> What made it terrible?"

Feeds: Customer Journey Map (benchmark references), Unmet Needs Report.

**Q5 — Decision Criteria**
> "When you're researching a new [product type], what do you look for first?
> Second? What's a dealbreaker?"

Feeds: Decision Criteria Hierarchy (Tier 1 dealbreakers, Tier 2 primary,
Tier 3 tiebreakers).

**Q6 — Delivery Preference**
> "If a product promised to [core value prop], how would you want to
> experience that? In the product itself? Through reports?
> Through a person helping you?"

Feeds: Product Feedback Synthesis (shapes delivery model and UX).

**Q7 — Unfiltered Frustration**
> "Is there anything about [problem space] that you feel companies just
> don't understand? What do you wish they would get right?"

Feeds: Language Library (emotional language), Unmet Needs Report,
content hooks for marketing.

## Pulse Check Framework (3 Questions — Monthly)

Quick reads on evolving sentiment. 6 personas, ~15 minutes.

**Q1:** "What's the biggest challenge you're facing right now with [problem space]?"
**Q2:** "Has anything changed in how you handle [task] in the last month?"
**Q3:** "What's the one thing you wish existed to make [task] easier?"

Track Q1 answers month-over-month to detect shifting priorities.

## The 6 VoC Deliverables

### 1. Customer Journey Map

Touchpoints from Q1 with classification:
- **Pain** — frustration, friction, confusion
- **Delight** — exceeded expectations, positive surprise
- **Neutral** — functional, neither good nor bad

Include frequency counts (how many personas mentioned each touchpoint)
and benchmark references from Q4.

### 2. Pain Priority Matrix

Frustrations ranked by **severity** (how painful) and **frequency**
(how many personas mentioned it) from Q1 and Q2.

The most common magic wand answer from Q2 = the #1 positioning opportunity.

### 3. Language Library

**Arguably the most commercially valuable deliverable.**

Exact words and phrases categorised as:
- **Problem language** — how customers describe their frustrations
- **Solution language** — how they describe what they want
- **Evaluation language** — how they assess and compare options
- **Emotional language** — feelings, frustrations, aspirations
- **Comparison language** — how they reference competitors and alternatives

Include demographic context for each entry (role, age, industry).

**Application:** SEO keywords, ad copy, sales messaging, positioning
statements, landing page copy, email subject lines.

### 4. Unmet Needs Report

Gaps classified by type:
- **Feature Gap** — buildable (product team action)
- **Experience Gap** — designable (UX/design action)
- **Trust Gap** — addressable via messaging (marketing action)

### 5. Decision Criteria Hierarchy

From Q5 responses:
- **Tier 1 — Dealbreakers** (must-haves, pass/fail)
- **Tier 2 — Primary Criteria** (important, heavily weighted)
- **Tier 3 — Tiebreakers** (nice-to-haves, differentiation)

### 6. Product Feedback Synthesis

Categorised as:
- **Build** — new capability needed
- **Improve** — enhance existing feature
- **Fix** — something is broken
- **Communicate** — feature exists but customers don't know about it

## Cross-Segment VoC

Run identical questions across different user roles:

```
Group A: End Users (daily product users)
Group B: Team Leads (manage end users)
Group C: Leadership (budget holders)
```

Different roles surface different pain points and priorities.
Same questions, different panels. Produces role-specific journey maps
and pain matrices.

## Cross-Market VoC

Run identical studies across countries:

```
Group A: USA users (10 personas)
Group B: UK users (10 personas)
Group C: Germany users (10 personas)
Group D: Canada users (10 personas)
```

Surfaces cultural differences in problem perception, language, and
expectations. Informs market-specific positioning and messaging.

## Longitudinal Tracking

After 4 quarters of deep dives, you have a longitudinal dataset. Track:

- **Pain intensity** over time (are problems getting better or worse?)
- **Competitive landscape** shifts (new tools mentioned? old ones abandoned?)
- **Language evolution** (are customers describing problems differently?)
- **Unmet needs** emergence and resolution (what's new? what's been addressed?)

The compounding effect: quarterly datasets produce trend lines, not snapshots.

## VoC Feeds Every Function

| Function | VoC Inputs |
|----------|-----------|
| **Positioning** | Pain Matrix + Language Library + Unmet Needs |
| **Messaging** | Language Library + Pain Matrix |
| **Competitive Intel** | Q3 current solutions + quarterly shifts |
| **Sales Enablement** | Quote bank + objection handling from Q7 |
| **Product Strategy** | Unmet Needs + Product Feedback Synthesis |
| **Content Marketing** | Language Library + journey map stories |
| **Pricing** | Decision Criteria (is price a Tier 1 dealbreaker?) |

## Complete API Reference

### Research Groups

| Method | Endpoint | Purpose |
|--------|----------|---------|
| `POST` | `/v1/research-groups/recruit` | Recruit panel |
| `POST` | `/v1/research-groups/create` | Create from agent IDs |
| `POST` | `/v1/research-groups/interview` | AI-assisted recruitment |
| `GET` | `/v1/research-groups` | List groups |
| `GET` | `/v1/research-groups/{id}` | Get group details |
| `POST` | `/v1/research-groups/{id}/update` | Update group |
| `DELETE` | `/v1/research-groups/{id}` | Archive group |
| `POST` | `/v1/research-groups/{id}/agents/add` | Add agents |
| `POST` | `/v1/research-groups/{id}/agents/remove` | Remove agents |
| `POST` | `/v1/research-groups/{uuid}/append` | Recruit more into group |

**⚠️** `append` uses `group_uuid` (not `group_id`).

### Research Studies

| Method | Endpoint | Purpose |
|--------|----------|---------|
| `POST` | `/v1/research-studies` | Create study |
| `GET` | `/v1/research-studies` | List studies (review historical) |
| `GET` | `/v1/research-studies/{id}` | Get study details |
| `POST` | `/v1/research-studies/{id}/complete` | Trigger AI analysis |
| `POST` | `/v1/research-studies/{id}/agents/remove` | Remove agents |

### Questions

| Method | Endpoint | Purpose |
|--------|----------|---------|
| `POST` | `/v1/research-studies/{id}/questions` | Ask question (returns job_ids) |
| `GET` | `/v1/research-studies/{id}/questions` | Get all Q&A data |
| `POST` | `/v1/research-agents/{id}/questions` | Quick question to one persona |
| `POST` | `/v1/research-groups/{id}/questions` | Quick pulse question to group |

### Jobs, Sharing, Media

| Method | Endpoint | Purpose |
|--------|----------|---------|
| `GET` | `/v1/jobs/{job_id}` | Poll async job status |
| `POST` | `/v1/research-studies/{id}/share` | Enable sharing |
| `GET` | `/v1/research-studies/{id}/share` | Check share state |
| `POST` | `/v1/media-assets` | Upload image/PDF |

### Agents, NL Requests, Zeitgeist, Free Tier

| Method | Endpoint | Purpose |
|--------|----------|---------|
| `GET` | `/v1/agents/find` | Find matching persona |
| `GET` | `/v1/agents/search` | Search personas |
| `POST` | `/v1/research-study-requests` | NL study request |
| `POST` | `/v1/research-group-requests` | NL group request |
| `GET` | `/v1/research-group-requests/{id}` | Group request status |
| `POST` | `/v1/zeitgeist/surveys/create` | Quick single-Q survey |
| `GET` | `/v1/zeitgeist/surveys/{id}/results` | Survey results |
| `DELETE` | `/v1/zeitgeist/surveys/{id}` | Delete survey |
| `POST` | `/v1/free/questions` | Free-tier question |

## Demographic Filters

| Filter | Type | Examples | Notes |
|--------|------|----------|-------|
| `country` | string | `"USA"`, `"UK"`, `"Canada"`, `"Germany"` | Required. Only these 4 |
| `state` | string | `"TX"`, `"CA"` | **2-letter codes ONLY** |
| `city` | string | `"Austin"` | Narrows pool |
| `age_min` | integer | 25 | Recommended |
| `age_max` | integer | 55 | Recommended |
| `gender` | string | `"male"`, `"female"`, `"non_binary"` | Optional |
| `is_parent` | boolean | `true`, `false` | Optional |
| `education` | string | `"bachelors"`, `"masters"` | Optional |
| `industry` | array | `["Technology"]` | Recommended |

**NOT supported:** `income`, `employment`, `ethnicity`, `political_affiliation`.

## Polling Strategy

| Study type | First poll | Interval | Group size |
|-----------|-----------|----------|-----------|
| Pulse (6 personas) | 30-40 seconds | 20 seconds | 6 |
| Deep Dive (10 personas) | 45-50 seconds | 20 seconds | 10 |
| Targeted Probe (8 personas) | 40-45 seconds | 20 seconds | 8 |

Poll ONE job_id as proxy — all jobs finish together.

## Response Structure

**Study creation:** `response["study"]["id"]` (nested under `study` key)

**Question responses:** `response_text` (may contain HTML), `agent_name`,
`agent_age`, `agent_occupation`, `agent_summary`, `agent_city`, `agent_state`.

**Share link:** Prefer `share_link` over `share_url`.

## Common Mistakes

- Treating VoC as an annual event (should be monthly)
- Producing reports that never become updated deliverables
- Changing question order (each builds on conversational context)
- Skipping the Language Library (most commercially valuable output)
- Using `size` instead of `group_size`
- Using `response["id"]` instead of `response["study"]["id"]`
- Polling every 10-15s (use 45-50s first, then 20s)
- Not tracking longitudinal trends after 4+ quarters
- Running only deep dives without monthly pulse checks
- Generic groups instead of ICP-matched panels

## Limitations

Ditto personas have NOT used your specific product. For:
- Actual in-product usage data → use analytics
- Real customer churn reasons → use exit surveys
- NPS scores → use real customer surveys
- Individual customer feedback → use support tickets and calls

**Recommended hybrid:** Ditto for continuous market-level VoC intelligence.
Real customer data for product-specific metrics and individual feedback.

## Further Reading

- Full API guide: https://askditto.io/claude-code-guide
- Question design: @question-playbook.md
- VoC calendar template: @advanced/voc-calendar.md
