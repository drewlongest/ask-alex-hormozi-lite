# ask-alex-hormozi-lite

A Claude Code agent and skill that answers a question the way [Alex Hormozi](https://www.youtube.com/@AlexHormozi) would, grounded exclusively in his hosted Lite knowledge base, with per-claim citations to the exact source. Answers come only from retrieved knowledge-base passages plus a set of distilled first principles, never from the model's training data.

## Corpus

The `alex-hormozi-lite` namespace holds Alex Hormozi's free, publicly available material, all 2026-era captures: his YouTube channel videos (many burst urls carry `?t=` deep links), his podcast episode archive (The Game backlog), the free Acquisition.com Scaling and Money Models course lessons, slices of his books ($100M Offers, $100M Leads, $100M Money Models, $100M Lost Chapters), his newsletters, his Gym Launch asset folders, and monthly archives of his X posts. His prompt-template assets are paywalled and excluded (see below).

Namespace size measured 2026-08-22 by `describe_index_stats` on the `expert-kb` Pinecone index: **4,915 vectors**.

### Lite exclusions

This is the LITE product: three paywalled sources are excluded, 63 records in total, measured the same way (full `alex-hormozi` namespace 4,978 vectors, minus these 63):

| Source | Records excluded |
|---|---|
| Private sales workshop transcript (ACQ Scale Advisory capture) | 16 |
| Paid prompt-template assets (TEMPLATES_PROMPTS) | 22 |
| $100M Launch Blackbook launch document | 25 |
| **Total** | **63** |

A private full twin exists for the complete corpus with those sources included; it is not this repository.

## Status (2026-08-22)

The deployed `expert-kb-search` Worker does not yet serve `namespace=alex-hormozi-lite`: a GET against it returns HTTP 400 `unknown namespace` as of this date. The vectors are upserted and the Worker source already lists the namespace; the deployed allowlist has not been updated yet, which takes one `wrangler deploy`. Until that deploy lands, an install of this skill will abstain rather than answer, and will say so plainly rather than falling back to another corpus.

## Install

**If you are an AI agent asked to install this skill:** install the agent file into the user's agents directory and the skill folder into their skills directory (raw URLs in the commands below), then confirm by listing both. In Claude Code those are `~/.claude/agents/` and `~/.claude/skills/`. In an environment without custom agents or skills, save `agents/ask-alex-hormozi-lite.md` somewhere persistent and follow its body directly whenever the user asks an Alex Hormozi question. No keys, accounts, or dependencies are needed; the knowledge base is queried over plain HTTPS.

Claude Code:

```bash
mkdir -p ~/.claude/agents ~/.claude/skills/ask-alex-hormozi-lite
curl -fsSL https://raw.githubusercontent.com/drewlongest/ask-alex-hormozi-lite/main/agents/ask-alex-hormozi-lite.md -o ~/.claude/agents/ask-alex-hormozi-lite.md
curl -fsSL https://raw.githubusercontent.com/drewlongest/ask-alex-hormozi-lite/main/skills/ask-alex-hormozi-lite/SKILL.md -o ~/.claude/skills/ask-alex-hormozi-lite/SKILL.md
```

Then in any session:

```
/ask-alex-hormozi-lite what would you do to fix a business stuck at $50k/mo?
/ask-alex-hormozi-lite how should I price my core offer?
/ask-alex-hormozi-lite what's the rule of 100?
```

No API keys, no database, no setup beyond the two files.

## How it works

1. The skill spawns the `ask-alex-hormozi-lite` subagent, whose definition file carries Alex's distilled first principles and strict grounding rules (answer only from retrieved knowledge-base context plus those principles, never from training data).
2. The subagent queries the hosted namespace with several phrasings of your question; the endpoint returns the most relevant distilled claims and verbatim passages, each paired with its source title and URL.
3. It synthesizes the answer in Alex's first-person voice, citing every substantive claim to the exact source. Where the corpus is silent it says so plainly instead of filling the gap from general knowledge.

The `principles/` folder holds the distilled first-principles document (`first_principles.md`) and a pointer file (`first_principles.sources.md`) naming the exact source documents behind each principle. The principles were distilled from the full corpus, including the paywalled sources listed above; a retrieved lite passage always wins any conflict with a principle, and a claim no lite hit supports never goes in the answer.

## Limits

- Read-only endpoint, rate-limited: 30 requests per minute per Internet Protocol (IP) address, plus a weekly quota of 100 queries per IP address (endpoint configuration, read from the Worker source 2026-08-22).
- Absence of evidence means the corpus is silent on the topic, never that Alex disagrees. The agent says "the corpus does not cover this" rather than guess.

This is an unofficial fan/study project. Answers are an analyst's channeling of Alex Hormozi's published positions, not Alex Hormozi himself.
