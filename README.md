# Ditto for Voice of Customer Programmes — Claude Code Skill

Build and run an always-on VoC research programme — monthly pulse checks,
quarterly deep dives, longitudinal trend tracking — using
[Ditto](https://askditto.io)'s synthetic persona platform. Directly from
your terminal via Claude Code.

## What This Skill Does

When installed, Claude Code automatically loads this skill whenever you
ask it to set up VoC research, run pulse checks, build customer journey
maps, or establish continuous feedback loops.

Claude will:
- Set up an always-on research calendar (monthly pulse + quarterly deep dive)
- Run studies with proven VoC question frameworks
- Produce 6 VoC deliverables (journey map, pain matrix, language library, and more)
- Track longitudinal trends across quarters

## The Core Principle

**VoC is NOT an annual event. It is a continuous 2-hour/month habit.**

## Research Calendar

| Study Type | Frequency | Time |
|-----------|-----------|------|
| Pulse Check | Monthly | ~15 min |
| Deep Dive | Quarterly | ~45 min |
| Targeted Probe | Ad hoc | ~25 min |

## 6 VoC Deliverables

1. **Customer Journey Map** — touchpoints with pain/delight/neutral
2. **Pain Priority Matrix** — frustrations by severity and frequency
3. **Language Library** — exact customer words for SEO, copy, messaging
4. **Unmet Needs Report** — feature gaps, experience gaps, trust gaps
5. **Decision Criteria Hierarchy** — dealbreakers, primary, tiebreakers
6. **Product Feedback Synthesis** — build, improve, fix, communicate

## Installation

```bash
# For a specific project
cp -r ditto-voc-programme /path/to/your/project/.claude/skills/

# For all your projects (personal skills)
cp -r ditto-voc-programme ~/.claude/skills/
```

## Setup

Get a free Ditto API key (no credit card required):

```bash
curl -sL https://app.askditto.io/scripts/free-tier-auth.sh | bash
```

Set it as an environment variable:

```bash
export DITTO_API_KEY="rk_free_YOUR_KEY_HERE"
```

## Usage

```
"Set up a VoC programme for our SaaS product.
Run a quarterly deep dive with 10 US tech professionals."

"Run a monthly pulse check on customer sentiment.
3 questions, 6 personas."

"Build a customer journey map and language library
from our latest VoC study."
```

Or invoke directly:

```
/ditto-voc-programme "quarterly deep dive for [product]"
```

## What's Included

| File | Purpose |
|------|---------|
| `SKILL.md` | Main skill — VoC calendar, question frameworks, 6 deliverables |

## About Ditto

Ditto maintains 300,000+ AI-powered synthetic personas calibrated to
census data across USA, UK, Germany, and Canada. Total monthly VoC
investment: ~2 hours. Traditional VoC programme: $200,000+/year.

## Links

- **Ditto:** https://askditto.io
- **Full API Guide:** https://askditto.io/claude-code-guide
- **General Research Skill:** https://github.com/Ask-Ditto/ditto-product-research-skill

## License

MIT