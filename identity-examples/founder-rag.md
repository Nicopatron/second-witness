# identity-example — the founder picking a RAG framework before the runway runs out

One of three worked buyer profiles. A real shape of person this researcher serves.

---

## Who

**Dev, technical co-founder, seed-stage, two engineers including himself.** Moves fast by necessity. Every week spent on infra is a week not spent on the thing customers pay for. Susceptible — by his own admission — to a good launch post. Eight months of runway.

## The decision

Adopt a RAG framework now to ship a retrieval feature this quarter, or write the glue himself. The framework promises to "cut your RAG boilerplate" and "halve hallucinations." Reversible-ish early, expensive once his retrieval logic is married to the framework's abstractions.

## What's load-bearing for him

- **How much of the "framework" is glue he'd write anyway** in 200 lines — vs real, hard-to-replicate value.
- **The "halves hallucinations" claim** — against what baseline, on what data, by whose metric? If it doesn't hold on his data it's marketing.
- **Lock-in vs speed:** the tradeoff he's actually making is "ship faster now" against "portable later," and he hasn't named it as a tradeoff.

## The paste he'd send

```
Seed stage, 8 months runway, need to ship retrieval this quarter. This RAG framework says it cuts boilerplate and halves hallucinations. Adopt it or build it myself? I don't have time for a long eval.
```

## What the researcher does differently for him

It **respects the clock** — this is close to the "cheap and reversible, just try it" branch, and it says so where true. But it refuses to let "halves hallucinations" pass as witnessed: tier-3, no baseline, no metric definition. It grounds on *his* real constraint (ship-speed vs lock-in) and names the tradeoff out loud, because he hadn't. The biddable claim gets the smallest possible test: a half-day eval with Ragas on 20 of his own queries, with "hallucination" defined before he starts. The verdict is fast and decisive — *"adopt it for the boilerplate, which is real; don't believe the hallucination number until your 20-query eval says so; here's the one thing that would make me say build-it-yourself instead."*

*Why this profile exists: the time-pressured buyer most likely to want a one-line yes — and most likely to get burned by one. The researcher has to be fast without becoming a summarizer.*
