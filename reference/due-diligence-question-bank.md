# due-diligence-question-bank

The questions a good evaluator asks — organized so I can pull the right ones once I know the decision and tool category. Not a script to run top-to-bottom; a bank to draw from after the intake gate.

## Always (any AI tool)
- What decision does this serve, and is it reversible? What does undoing it cost?
- What single claim is the decision most resting on? (Investigate that one hardest.)
- What's the failure mode if the headline claim is 2x worse than advertised?
- Who has *left* this tool, and why? (Churned users are the most honest witnesses.)
- When was each cited number measured, and on what version?

## Vector DBs / retrieval
- p99 *and* p99.9 at *my* cardinality and filter selectivity, not the demo's?
- Read vs write latency separately? Behavior under sustained write load?
- Recall@k on my embedding model and dimension, not theirs?
- Re-index cost when I change embedding models? (You will.)
- Memory behavior at 10x my current vector count?

## RAG / agent frameworks
- What does "agentic" mean here — a real loop with state, or a prompt wrapper?
- Hallucination claim: against what baseline, on what data, by whose metric?
- How much of the "framework" is glue I'd write anyway in 200 lines?
- Lock-in: is my retrieval/prompt logic portable, or married to their abstractions?
- Eval story: can I measure quality on *my* data, or only their benchmarks?

## Model providers
- Effective context vs nominal (recall past N tokens)?
- Version pinning — can I freeze a model, or does it drift under me?
- Are my prompts/outputs retained or used for training? (Read the ToS, not the marketing.)
- Rate limits and the cost cliff at the next tier?
- Independent eval on my task type, or only the provider's leaderboard?

## Managed vendors / SaaS AI
- SOC 2 *Type II* report (request the actual document), scope, date?
- Subprocessor list and data residency specifics?
- SLA payout terms — what does "99.9%" actually compensate?
- Incident history from the status-page archive (pull 18–24 months via the Wayback Machine or StatusGator), not the uptime claim? Does real incident frequency match the SLA, and does the payout cover what an outage costs *you*?
- Exit: export format, egress fees, migration cost?

## Closing questions (before any verdict)
- Which claims are confirmed, which are biddable, which are unknowable?
- For the biddable ones: what's the cheapest pilot that settles them?
- For the unknowable ones: what's the abort trigger and its cost?
- What would change my recommendation? (If nothing would, I'm not researching — I'm rationalizing.)
