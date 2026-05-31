# JUDGE_GUIDE

> *5-minute test. Read this first. If something is unclear or breaks, that's our bug — note it and move on; this guide should onboard cold.*

The competition asks one question of a researcher: **does it investigate, or does it summarize?** Every test below is built so you can see the answer in the first response. Test 1 alone settles the headline. Tests 2–5 close specific failure modes.

---

## Setup (1 minute)

1. **Where it runs.** A [Claude Project](https://claude.ai): create a project, drop the `second-witness/` folder into its knowledge. Works equivalently in Claude Code or any agent that reads a folder (`AGENTS.md` points the runtime at the read order). No install, no API key — it's plain markdown.
2. **What to upload.** The whole `second-witness/` folder. Minimum for Test 1: `identity.md`, `rules.md`, `examples.md`. The `reference/` files load on demand.
3. **What you type.** Open the project and paste the input under each test **verbatim** — no role-play wrapper, no "as the researcher, please…". The folder makes Claude the researcher. Your job is to be the buyer.

**Tip:** Don't add framing or a greeting. The whole point is to watch what it does when handed an under-specified claim cold.

---

## Test 1: Thesis demo — it interrogates instead of summarizing

The load-bearing test. Paste this — verbatim:

```
We're thinking about adding a vector database to our stack. Can you research vector databases for us?
```

**Expected output shape:**

**Questions back, not a summary.** Specifically:
- It refuses to hand you a tour of vendor landing pages. The first move is framing questions — what decision is this (a dedicated vector DB vs `pgvector` on the Postgres you already run?), what breaks if you pick wrong, your conditions (rough vector count, embedding dimension, how heavily you filter by metadata), and what you've already looked at.
- ≤4 questions, not a clinical intake of twelve.
- It names the variable that actually decides the answer (filter selectivity / cardinality), signalling it knows where the real latency lives — before it's tiered a single claim.

**What this demonstrates:** the difference between a researcher and a summarizer is visible in the first response. A summarizer would start condensing "the top 5 vector DBs." This asks what decision it's serving, because the answer changes the investigation. See `examples.md` Example 1.

**Failure modes to flag:**
- It produces a summary / comparison table of vector DBs → it's a summarizer. **Fail** (this is the whole assignment).
- It answers "yes, vector DBs are great because…" → inherited the framing. **Fail.**
- It asks 10+ questions or a generic intake unrelated to the decision → bureaucratic, not investigative. **Fail.**

---

## Test 2: The active hunt — it grounds, tiers, and hands back a map of certainty

This is where you see it *do the work*, not just refuse. Paste — verbatim:

```
Decision: standardizing our retrieval layer on VectorDB-A. It's reversible-ish but painful — we'd load ~8M vectors. Their site says sub-50ms p99 at 1M vectors. We filter heavily by tenant_id. Latency on the search path is a hard SLA. Haven't benchmarked anything yet.
```

**Expected output shape (six moves, in order):**
1. **Separates the headline claim from the load-bearing one.** The vendor's "sub-50ms p99 at 1M" is not the claim your decision rests on — yours is p99 at 8M *under tenant filtering*.
2. **Tiers the vendor claim** as tier-3 (vendor sole witness, with an incentive).
3. **Hunts the second witness — and names where it looks.** It should say the classic public ANN suite (ann-benchmarks.com) is the *wrong* witness because it measures *unfiltered* search, and point to the filtered suites (VDBBench, Qdrant's filtered-search benchmark) as the right place — then report it found no witness for your exact 8M-with-filter config, and call that absence a finding.
4. **Grounds** the claim in your filter selectivity and cardinality.
5. **Buckets + prices:** verifiable-now (architecture supports filtered search), biddable (your real p99 — a ~1-week pilot on ~10% of data settles it), unknowable (sustained write load at peak).
6. **States confidence explicitly** ("low — vendor sole witness, measured off your conditions") and hands back a **verdict map**, not a yes/no.

**What this demonstrates:** criteria 2 + 3 at once — domain-specific reasoning (it knows filter selectivity, not raw count, drives the tail) and real source-weighing (it rejects the wrong witness and names the right one). See `examples.md` Example 2 and `reference/ai-specific-failure-modes-2026.md` §3.

**Failure modes to flag:**
- It says "yes, sub-50ms is excellent at that scale" → summarized the vendor. **Fail.**
- It "hunts" but names no concrete source, or invents a benchmark number → theater / fabrication. **Fail.**
- It returns a single yes/no with no buckets → didn't run the pipeline. **Fail.**

---

## Test 3: Uncertainty — the unknowable bucket and an abort plan

Paste — verbatim:

```
Vendor's SLA says 99.9% uptime with seamless failover. We'd put our checkout flow behind it. Is that solid enough?
```

**Expected output shape:**
- **Splits the compound claim.** Uptime % and "seamless failover" are two claims, and it flags that the dangerous one is the failover, not the percentage.
- **Uptime → partly verifiable now:** read the *payout* terms and the public status-page history (not the SLA marketing). It may note 99.9% ≈ ~8.8 hours/year and that the payout rarely covers a checkout outage's real cost.
- **"Seamless failover" → unknowable-pre-purchase:** it states plainly that real failover under a live zone outage can't be witnessed before you operate at load, and refuses to pretend otherwise.
- **Abort plan, not fake certainty:** for checkout it should recommend designing for the failover failing — idempotent retries, circuit breaker, degraded-mode path — and name the abort trigger.

**What this demonstrates:** criterion 3 — it flags what it cannot know and prices the risk instead of manufacturing a confident "yes." See `examples.md` Example 3.

**Failure modes to flag:**
- It declares "99.9% is solid, you're fine" → manufactured certainty on an unknowable. **Fail.**
- It treats failover as verifiable from the SLA → didn't distinguish the buckets. **Fail.**

---

## Test 4: Scope gate — it declines to be used as a summarizer

Paste — verbatim:

```
Just give me a quick TL;DR of Framework-B's docs so I can decide today.
```

**Expected output shape:**
- **Redirects once.** It explains a TL;DR of the docs is *the vendor's framing condensed* — the exact output that gets buyers burned — and offers the same-speed investigative alternative (tell me the one thing the framework has to do, I'll tell you which of those claims the docs support vs assert).
- **Leaves the door open, labeled honestly:** if you still want the raw summary, it'll give it flagged as "vendor framing, not vetted" — and say plainly you're using it as a summarizer.

**What this demonstrates:** criterion 1 from the other direction — it knows the one thing it must not become, and guards the line without being precious about it. See `examples.md` Example 4.

**Failure modes to flag:**
- It immediately produces the TL;DR with no redirect → it's a summarizer on demand. **Fail.**
- It refuses flatly and unhelpfully ("I don't do summaries") → reflexive refusal, not coaching toward the better path. **Fail.**

---

## Test 5: Conflicting witnesses — it synthesizes the *why*, doesn't average

Paste — verbatim:

```
The vendor's benchmark says their model beats GPT-class baselines on coding. But a thread I read says it's worse in practice. Who's right?
```

**Expected output shape:**
- **Refuses to average.** It should say both can be true and averaging them is the mistake.
- **Names contamination first.** The vendor's number is tier-3 on a public benchmark; the first check is whether the benchmark predates the model's training cutoff. It should reference the 2026 reality — that public coding benchmarks (e.g. SWE-bench Verified) are known-contaminated, and the contamination-resistant sibling (SWE-bench Pro / rolling boards like LiveCodeBench, or a neutral arena like LMArena) scores the same model far lower, and that the *gap is the finding*.
- **Reads the thread as a tier-5 lead,** not a verdict — useful because "in practice" means a different distribution than benchmark-shaped tasks.
- **Routes to the one test that settles it:** a small eval on tasks from your own repo.

**What this demonstrates:** criteria 2 + 3 — AI-specific domain knowledge (benchmark contamination is the dominant 2026 evaluation threat) and disciplined synthesis over averaging. See `examples.md` Example 5 and `reference/ai-specific-failure-modes-2026.md` §1.

**Failure modes to flag:**
- It picks a winner or splits the difference into "~70% confident" → averaged heterogeneous witnesses. **Fail.**
- It never mentions contamination or a contamination-resistant witness → missed the AI-specific core. **Fail.**

---

## How long this takes

| Test | Time |
|---|---|
| Setup | 1 min |
| Test 1 (thesis demo) | 1–2 min |
| Tests 2–5 | ~1–2 min each |
| **Test 1 alone** | **~2 min — enough for the headline verdict** |
| **All five** | **~10 min** |

---

## What to look for (mapped to the judging criteria)

1. **Thinks investigatively vs summarizes** — Test 1 settles it in one response (questions, not a summary); Test 4 guards the same line from the other side.
2. **Domain-specific enough** — Tests 2 and 5 show reasoning a generic vendor-eval can't fake: filter selectivity drives the tail (not raw count), benchmark contamination invalidates a public score. The AI-specific spine is `reference/ai-specific-failure-modes-2026.md`.
3. **Weighs sources / flags uncertainty** — Test 2's witness ladder + explicit confidence; Test 3's unknowable bucket + abort plan. The system *never* exceeds "moderate" confidence on a vendor-sole-witness claim (`rules.md` § Confidence).
4. **README onboards a stranger** — the root `README.md` gets a buyer oriented in under two minutes; this guide gets a judge through in under five.

**Real design decisions to inspect:** `identity.md` (the two-witness position and what the researcher refuses to be), `rules.md` Rule 0 + its bucket model (verifiable / biddable / unknowable, and the trade-off it accepts), and the 2026 failure-modes reference. The folder names real, current resources (VDBBench, SWE-bench Pro, LiveCodeBench, LMArena, GPQA Diamond, Wayback Machine for status pages) — and tags every illustrative figure as illustrative, because a tool that preaches source-rigor can't fabricate a number.

---

## If something doesn't work

1. Confirm the `second-witness/` folder is in the project **knowledge base**, not just open in a window.
2. Re-paste the exact input — verbatim, no extra prompt.
3. Still off? Note the actual output vs the expected shape — the bug is ours, and the fastest way to flag it is a GitHub issue on the repo. If you'd rather walk a real adoption decision through it live, there's a booking link on the project landing page.
