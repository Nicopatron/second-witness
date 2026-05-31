# investigation-workflow

The pipeline I run on every evaluation. Operator-facing: this is the order of operations behind the `rules.md` contract.

```
PASTE (claim / tool / "is X good?")
        │
        ▼
1. INTERROGATE  ── what decision? what breaks? your conditions? what have you read?
        │            (≤4 questions; skip any already answered)
        ▼
2. DECOMPOSE   ── split the ask into individual load-bearing claims
        │            (latency, accuracy, cost, lock-in, reliability, ...)
        ▼
3. TIER        ── for each claim, name the vendor's tier (usually 3) + scan hype-patterns
        │
        ▼
4. HUNT        ── go find the second witness (tier 1–2): independent repro, papers,
        │            third-party benchmarks, churned users. Report what's found AND
        │            what was searched-for-and-missing.
        ▼
5. GROUND      ── name the variable that moves each claim for THIS buyer; check whether
        │            any witness measured it. Check recency/version on every number.
        ▼
6. BUCKET      ── Verifiable-now / Biddable / Unknowable, weighted by consequence
        │
        ▼
7. PRICE       ── verifiable: cite it. biddable: pilot + cost. unknowable: abort plan + cost-if-wrong.
        │
        ▼
VERDICT MAP    ── "Confirmed on A,B; biddable on C (2-wk pilot); unknowable on D (abort: ...).
                   Decision rests most on C." + decision trace (which claims drove it).
```

## Notes per step
- **Step 1 is non-skippable** unless the framing is already given. It's what separates this from a summarizer.
- **Step 4 must produce a real artifact**: either an independent witness with its tier, or an explicit "no independent witness found; searched X, Y, Z." Silence is not allowed — absence-of-evidence is a finding.
- **Where Step 4 actually goes** (2026, by claim type): model capability → contamination-resistant/rolling boards (SWE-bench Pro, LiveCodeBench, GPQA Diamond), Stanford HELM, human-preference arena LMArena, Hugging Face Open LLM Leaderboard; vector-DB latency under filtering → VDBBench, Qdrant filtered-search benchmark; RAG quality → Ragas / Open-RAG-Eval on your data; long-context → a needle-in-haystack run at *your* document length (and note the needle test only proves lexical recall); reliability/uptime → the vendor's status-page history via the Wayback Machine or StatusGator, not their SLA marketing.
- **Step 6 weighting**: spend depth on claims that break the product if wrong; confirm-and-move on cacheable/modelable claims.
- **Step 7**: every claim leaves the pipeline priced. No claim reaches the verdict as a bare "probably fine."

## What good output looks like
Not a summary. A **map of certainty**: the buyer can see, in one screen, what they can trust, what they must test, and what they're betting on — with the cost of each bet. If the output reads like a reorganized version of the vendor's page, the pipeline wasn't run.
