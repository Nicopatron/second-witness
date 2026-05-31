# claims-to-verify

Taxonomy of the claim types that show up in AI-tool due diligence. For each: **what the vendor usually says**, **the variable that actually moves it for the buyer**, **how to get a second witness**, and **default bucket** (Verifiable-now / Biddable / Unknowable). Numbers below are `[illustrative]`, never to be cited as measured.

## 1. Latency / throughput
- **Vendor says**: "sub-50ms p99," "10k QPS."
- **What moves it for you**: filter selectivity and cardinality, concurrency pattern (bursty vs sustained), hardware/region, warm vs cold cache, read vs write mix, payload size. Vendor numbers are *typically* measured under favorable conditions — often single-node, warm-cache, low-filter; confirm the config rather than assuming it.
- **Second witness**: independent benchmark repos at comparable scale; ask the vendor for p99.9 (not just p99) and the config. Confirm definition (does "p99" include cold start?).
- **Default bucket**: **Biddable** — only a pilot at your cardinality settles it.

## 2. Accuracy / quality (model & RAG claims)
- **Vendor says**: "SOTA on [benchmark]," "cuts hallucination 40%."
- **What moves it for you**: whether the benchmark resembles your task and data; metric definition (what counts as a hallucination?); benchmark contamination (was it in training data?); your prompt distribution.
- **Second witness**: independent evals on neutral data; leaderboards run by third parties; check contamination dates. Beware: "40% reduction" against *what baseline*, measured by *whose* eval?
- **Default bucket**: **Biddable** (eval on your own data) — and sometimes can't even be bucketed until you agree on a shared metric (what counts as a "hallucination"?). Pin the metric first, then bucket.

## 3. Cost at scale
- **Vendor says**: "starts at $X / month," "cheaper than running your own."
- **What moves it for you**: your token/query volume, egress, storage growth, the jump at the next pricing tier, overage rates, the cost of the *fallback* when you exceed limits.
- **Second witness**: third-party cost calculators; users reporting bills at your scale; model the next-tier cliff yourself.
- **Default bucket**: **Verifiable-now** (you can model it) once you have your real volume — **Biddable** if volume is uncertain.

## 4. Reliability / operational behavior
- **Vendor says**: "99.9% uptime," "seamless failover," "auto-scaling."
- **What moves it for you**: behavior under memory pressure, tail behavior at peak, failover *during* a real zone outage, what the SLA actually pays out, status-page honesty.
- **Second witness**: public incident history, status-page archives, users who've been through an outage.
- **Default bucket**: **Unknowable-pre-purchase** for the worst cases — you learn failover at 3am. Price it; design the abort.

## 5. Lock-in / portability
- **Vendor says**: "open standard," "easy to migrate out," "no lock-in."
- **What moves it for you**: proprietary data formats, custom query language, embedded business logic, re-indexing cost, contractual egress fees, how many of your engineers would need retraining.
- **Second witness**: people who *left* the tool (the most honest witnesses); export-format docs; try a dry-run export.
- **Default bucket**: **Verifiable-now** (read the export docs, attempt an export) — cheap to check, often skipped.

## 6. Security / compliance
- **Vendor says**: "SOC 2," "enterprise-grade," "data never leaves your VPC."
- **What moves it for you**: the actual report (Type I vs Type II, scope, date), data residency specifics, subprocessors, whether your prompts are retained or used for training.
- **Second witness**: the actual SOC 2 report (request it), the DPA, the subprocessor list, the retention clause in the ToS.
- **Default bucket**: **Verifiable-now** — these are documents you can demand and read.

## 7. Context window / capability claims
- **Vendor says**: "1M token context," "agentic," "handles multi-step."
- **What moves it for you**: effective vs nominal context (recall degradation past N tokens), whether "agentic" means a real loop or a wrapper, capability on *your* task length.
- **Second witness**: needle-in-haystack / long-context recall studies by third parties; independent agent benchmarks.
- **Default bucket**: **Verifiable-now to Biddable** — third-party long-context studies often exist; otherwise test on your length.

---

**Consequence weighting**: claims in #2 (accuracy) and #4 (reliability) usually break the product if wrong — investigate hardest. Claims in #1 (latency) and #3 (cost) are often cacheable or modelable — confirm but don't over-spend. #5/#6 are cheap to verify and frequently neglected — quick wins.
