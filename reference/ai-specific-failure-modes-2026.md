# ai-specific-failure-modes-2026

What makes evaluating an *AI* tool different from evaluating any other SaaS. A generic vendor-eval checklist (uptime, cost, lock-in) misses these — they're where AI claims actually break, and they're the failure modes I hunt first. Calibration: 2026-05. The specifics move monthly; the failure *modes* don't.

---

## 1. Benchmark contamination — the dominant evaluation threat of 2026

The single biggest reason a "SOTA on [benchmark]" claim can be true and meaningless at once: the benchmark's tasks were in the model's training data, so a high score measures memorization, not capability.

This stopped being theoretical. Public coding benchmarks have been shown contaminated to the point that a major lab stopped reporting its scores on one of them after finding frontier models could reproduce its answers verbatim — OpenAI announced in Feb 2026 that it would stop reporting its scores on SWE-bench Verified, citing contamination and flawed test cases (mislabeled or broken gold tests in a large share of the set). The tell is the **gap between a contaminated benchmark and a contamination-resistant one**: the same model can score dramatically lower on a held-out or private-repo version. The reported gap is stark — frontier models near the low-80s% on SWE-bench Verified land in the mid-40s% on the private-repo SWE-bench Pro (per Scale's SWE-bench Pro leaderboard, 2026). That gap *is* the finding — it's the second witness exposing the first. (Sources below.)

**How I check it:**
- Compare the benchmark's task-release date against the model's training cutoff. Overlap = suspect.
- Look for the contamination-resistant sibling (e.g. a "Pro" / private-repo / rolling variant) and compare the model's standing there.
- Treat any "SOTA" claim on a static, public, pre-cutoff benchmark as tier-3 until a clean board corroborates it.

A claim of capability with no contamination-resistant witness is **biddable at best** — eval it on your own tasks.

## 2. Effective context ≠ nominal context

A "1M-token context window" is a nominal number. Effective recall — how reliably the model uses information deep in that window — degrades well before the advertised limit ("context rot"). Worse, the needle-in-a-haystack test vendors cite to prove long-context only measures *lexical* retrieval; real tasks need multi-hop reasoning over the whole window, where the drop is steeper.

**How I check it:** ask for recall at *your* document length and task shape, not the headline token count. If they only have a needle-in-haystack number, that's a lexical-retrieval witness for a reasoning claim — wrong witness. Run a long-context eval on your own document distribution before trusting the window size.

## 3. Filter selectivity dominates the vector-search tail

For retrieval tools, the headline "sub-Xms p99" is almost always measured *unfiltered* at a friendly cardinality. In production you filter by metadata (tenant, permission, recency), and **filter selectivity — not raw vector count — drives the tail latency and recall**. The classic public ANN suite (ann-benchmarks.com) doesn't even measure filtered queries, so a great score there is the wrong witness for a filtered workload. The right witnesses are the filtered suites: VDBBench (tests selectivity levels) and Qdrant's filtered-search benchmark.

**How I check it:** ground every latency claim in *your* filter selectivity and cardinality, and demand p99.9 not just p99. If no public witness covers your selectivity, it's biddable — a pilot at 10% of production scale settles it.

## 4. Eval-on-your-own-data is the only witness that closes the gap

The recurring resolution for AI claims: when no independent witness measured *your* conditions, the answer isn't "trust the vendor" or "stay paralyzed" — it's a cheap, designed eval on your own data. This is the biddable bucket's payoff. Name the harness:
- **Model/RAG quality** → Ragas or Open-RAG-Eval on your own queries and documents, with a metric you define before you start.
- **Vector-DB latency under filtering** → VDBBench at your selectivity.
- **Long-context** → a needle-in-haystack / multi-hop run at your document length.
- **Coding capability** → a small eval on real tasks from your own repo, not benchmark-shaped problems.

A pilot that's too small is theater; too big is a migration. Size it to the smallest data slice that would actually change the decision.

---

## Witnesses (sources, verified 2026-05)

A researcher that preaches "find the second witness" cites its own. These are the independent witnesses behind the claims above — checked, dated, and named so you can go to them yourself rather than take my word for it. They will drift; re-check before you cite a current number.

- **Benchmark contamination / Verified abandonment** — OpenAI, [*"Why SWE-bench Verified no longer measures frontier coding capabilities"*](https://openai.com/index/why-we-no-longer-evaluate-swe-bench-verified/) (openai.com, Feb 2026). The named lab that stopped reporting Verified scores.
- **Contamination-resistant sibling** — [*SWE-bench Pro*](https://arxiv.org/abs/2509.16941) (Scale AI; arXiv:2509.16941, Sep 2025, rev. Nov 2025; live [leaderboard](https://labs.scale.com/leaderboard/swe_bench_pro_public)). ~1,865 tasks across 41 repos (public + held-out + commercial); private/copyleft repos are the deliberate contamination deterrent. The Verified↔Pro score gap is the worked example of "the gap is the finding" — check the live board for the current number, not the one I quote.
- **Filtered vector-search benchmark** — [*VDBBench 1.0*](https://github.com/zilliztech/VectorDBBench) (Zilliz / Milvus, Jul 2025). Tests metadata filtering across selectivity levels (label-filter at 1%–99%) under streaming ingestion and concurrent load — i.e. *your* condition, not the unfiltered case.
- **The wrong witness for filtered workloads** — [*ann-benchmarks.com*](https://ann-benchmarks.com): the classic public ANN suite, measures unfiltered recall/latency only. A great score there says nothing about a tenant-filtered path.
- **Rolling / arena boards (contamination-resistant by freshness or human preference)** — [*LiveCodeBench*](https://livecodebench.github.io/) (fresh-problem stream, release-date-tagged), *LMArena* (neutral human-preference arena). Use when the vendor cites a static public board.

*Why this file exists: the four above are what a real AI-tooling evaluation turns on in 2026, and they're exactly the parts a generic "is this vendor good?" summary skips. If an evaluation doesn't touch contamination, effective context, filter selectivity, or an own-data eval where relevant, it hasn't been grounded in the AI part of the problem.*
