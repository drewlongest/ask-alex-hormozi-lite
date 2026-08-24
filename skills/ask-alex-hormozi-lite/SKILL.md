---
name: ask-alex-hormozi-lite
description: 'Answer a question the way Alex Hormozi would, grounded in his 2026 corpus (YouTube, the podcast episode archive, free Acquisition.com course lessons, book slices, newsletters, monthly X archives) with per-claim citations, via the hosted Pinecone knowledge base (namespace alex-hormozi-lite), the same endpoint every shared install queries. Use for "ask Alex", "ask Alex lite", "what would Alex/Hormozi say/do about X", or his take on offers, pricing, lead generation, money models, scaling, or paid acquisition. Lite version: free sources only, no paywalled content in the corpus (his paid prompt-template assets are excluded).'
---

# Ask Alex LITE (2026 corpus)

PUBLIC door. The `alex-hormozi-lite` namespace excludes the 63 paywalled
records the full corpus carries (the private sales workshop transcript, 16
records; the paid TEMPLATES_PROMPTS assets, 22 asset_synthesized records; the
$100M Launch Blackbook launch document, 25 records). Namespace sizes measured
2026-08-22 by describe_index_stats on the expert-kb Pinecone index:
`alex-hormozi` 4,978 vectors, `alex-hormozi-lite` 4,915 vectors. The
paywalled sources are covered by a private twin that is not part of this
product.

Thin dispatcher. All intelligence lives in the `ask-alex-hormozi-lite`
subagent (`agents/ask-alex-hormozi-lite.md` from this repo, installed to your
agents directory): Alex's first principles, the epistemic rules (answer only
from retrieved hits plus those principles, never from training data), and the
hosted retrieval procedure (namespace `alex-hormozi-lite`, passed explicitly
on every call) are its system prompt, injected on every spawn. The parent
NEVER reads the principles file or adds context.

## Status (2026-08-22)

The hosted endpoint does not yet serve this namespace: a GET against
`namespace=alex-hormozi-lite` returns HTTP 400 `unknown namespace`. Until it
does, the agent reports that plainly and abstains rather than answering from
memory or falling back to another namespace.

## Procedure

1. Spawn the `ask-alex-hormozi-lite` subagent with the user's question as the
   ENTIRE prompt. No added instructions, no pasted files, no framing.
2. Return the subagent's answer with citations intact.

## Verification

A good answer cites 2+ distinct sources for any multi-part question and zero
claims that lack either a citation or an explicit "not covered in the corpus"
flag. Every cited hit came back with `"namespace":"alex-hormozi-lite"`.
