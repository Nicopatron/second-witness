# second-witness

**A due-diligence researcher for one decision: should you adopt this AI tool, build on it, or walk away?**

I've written the "should we adopt this" memo more than once — for vector DBs, RAG frameworks, model providers — and I've been wrong once, on a vendor benchmark that looked airtight until the production load test. That migration is why this folder exists. `second-witness` is the researcher I wish I'd had then: it doesn't summarize the vendor's pitch back at you, it goes and finds the *second* witness.

> **The thesis in one line:** a vendor's claim and an independent benchmark are two witnesses with opposite incentives — so the researcher's job is to go find the second one, then tell you exactly which parts of your decision still rest on only one.

---

## Built to the brief — the five things

The brief asks for five things. Here they are, and they're all you need to read:

| The brief asked for | In this folder |
|---|---|
| **identity.md** — who the researcher is | [identity.md](./identity.md) — a due-diligence researcher for one decision: adopt, build-on, or walk away |
| **rules.md** — how they research | [rules.md](./rules.md) — the contract: interrogate → tier → hunt → ground → bucket → price (Rule 0: never summarize) |
| **examples.md** — what good looks like | [examples.md](./examples.md) — five worked exchanges, one behavior each |
| **reference/** — frameworks, sources, concepts | [reference/](./reference/) — the witness ladder, claims taxonomy, hype-patterns, and the 2026 AI-failure-modes |
| **README.md** — how to use it | this file |

Everything else is **supporting reference the researcher loads on demand, not onboarding you have to read** — which is the point of a folder-as-architecture: you read five files, the agent pulls the rest only when a claim needs it. For the curious: [JUDGE_GUIDE.md](./JUDGE_GUIDE.md) is a five-test cold-eval for graders; [identity-examples/](./identity-examples/) shows the same rules producing different work for a CTO vs a founder vs a consultant; [researcher-pending.md](./researcher-pending.md) is the catch-file where the folder logs its own drifts and earns new rules from real sessions — proof it's alive, not frozen; [AGENTS.md](./AGENTS.md) points any agent runtime at the read order.

---

## Why this exists (the problem)

Reading a vendor's landing page takes **5 minutes**. Undoing a wrong infrastructure bet — the wrong vector DB with 8M vectors loaded, the framework your retrieval logic is now married to — takes **weeks to months** of migration. That asymmetry is the whole game. Most "AI tool research" just reorganizes the vendor's framing with a confident voice, which means the buyer's confidence is built on a single witness with an incentive. `second-witness` exists to break that.

It is **not a summarizer.** Paste it a topic and it won't condense the input — it'll ask what your decision is, what breaks if the claim is wrong, and what you've already read. Then it does the work.

---

## Quick terms (for the impatient)

| Term | Means |
|---|---|
| **Witness** | any source for a claim. The vendor is the *first* witness; an independent source is the *second*. |
| **Witness tier** | source credibility, 1 (independent reproduction) → 5 (anecdote). |
| **Load-bearing claim** | the claim your decision actually depends on. Investigated hardest. |
| **The three buckets** | every claim lands in **Verifiable-now** / **Biddable** (a pilot settles it) / **Unknowable-pre-purchase** (you learn it in production). |
| **Grounding** | re-stating a claim in the variable that moves it for *your* conditions, not the vendor's. |
| **The map of certainty** | the output: confirmed / biddable / unknowable + prices — not a single yes/no. |

---

## What you get back (real output, inline)

Paste: *"Standardizing our retrieval on VectorDB-A. Site says sub-50ms p99 at 1M vectors. We filter heavily by tenant_id, loading ~8M, search path is a hard SLA."*

You get (abridged from [examples.md](./examples.md) Example 2):

> The headline claim and the claim your decision rests on are not the same. "Sub-50ms p99 at 1M" is **tier-3 (vendor sole witness)** — and it's at 1M while you're loading 8M, almost certainly unmeasured under tenant filtering, which is *your* condition.
> **Second witness:** independent benchmarks show filter selectivity, not raw count, dominates the tail. I found **no** independent reproduction at 8M *with* tenant filtering on hardware like yours — that absence is the finding.
> **Map:** *Verifiable* — architecture supports filtered search. *Biddable* — your real p99 at 8M+filtering: a 1-week pilot on 800k vectors settles it. *Unknowable* — behavior under sustained write load at peak.
> **Verdict:** Don't standardize on the vendor's 50ms. Run the filtered pilot first — days instead of a migration. Your decision rests entirely on the biddable claim.

A summarizer would have said "yes, sub-50ms is great at that scale." That difference is the whole assignment.

---

## The flow on every paste

```
  you paste a claim / tool / "is X good?"
            │
            ▼
   ┌──────────────────┐   no decision/stakes given?
   │ 1. INTERROGATE   │── ask ≤4 framing questions ──┐
   └──────────────────┘                              │
            │ framing known                          │
            ▼                                         │
   2. TIER each claim by witness (vendor = tier 3)    │
            ▼                                         │
   3. HUNT the second witness (independent repro,     │
      papers, churned users) — report gaps too        │
            ▼                                         │
   4. GROUND in your conditions (the variable that     │
      actually moves the number for you)               │
            ▼                                         │
   5. BUCKET: verifiable-now / biddable / unknowable   │
            ▼                                         │
   6. PRICE + VERDICT MAP + decision trace             │
                                                        │
   ── refusal branch ──────────────────────────────────┘
   "just summarize the docs" → redirect to investigation once;
   out-of-scope (legal, pricing) → one-line decline;
   cheap reversible decision → "just try it, you don't need me."
```

---

## How to use it

**Setup (2 minutes):** `git clone` this repo (or download the ZIP), then add the `second-witness/` folder to a [Claude Project](https://claude.ai)'s **knowledge base** (not just attached to one message — the researcher needs `rules.md` loaded as context), or open it in Claude Code / any agent that reads a folder. No install, no dependencies, no API key — it's plain markdown. Claude becomes the researcher. To customize it for your own stack, fork the repo and edit `rules.md` / `reference/` directly; the folder *is* the program.

**What to expect on the first paste:** if you paste a tool or claim cold, it will ask you a few framing questions *before* it researches anything — that's the design, not a stall. A researcher that summarized your paste back at you would be the exact failure this folder exists to avoid. Give it the decision and it goes to work.

Three ways in:

- **A — 60-second cold test:** paste *"Should we use [any AI tool you know]?"* with no other context. If it asks you what decision it's for instead of summarizing — it's working.
- **B — Real evaluation:** paste an actual claim you're weighing, with your conditions (scale, constraints, what breaks if wrong). Get the bucketed map back.
- **C — Agent / CLI:** the contract lives in [rules.md](./rules.md); [AGENTS.md](./AGENTS.md) points agent runtimes at the read order. Works with Claude, Codex, Cursor, any capable model.

**Want a specific move?** Ask in plain language — *"scope this," "compare A vs B," "design the pilot," "what should I be asking?"* — and it runs that stage of the contract instead of the full pass. Nothing to install; it's a directable research partner, not a command menu. Full list: [rules.md](./rules.md) § How to point me.

### Try these five (each exercises a different behavior)
1. *"Research vector databases for us."* → should interrogate, not summarize. (Example 1)
2. *"Vendor says sub-50ms p99, we filter heavily, loading 8M."* → grounded bucketed verdict. (Example 2)
3. *"99.9% uptime, seamless failover — solid enough for checkout?"* → unknowable bucket + abort plan. (Example 3)
4. *"Just TL;DR these docs."* → scope-gate redirect. (Example 4)
5. *"Vendor benchmark says it beats GPT on coding, a thread says worse — who's right?"* → synthesize the divergence, don't average. (Example 5)

---

## When NOT to use it

If your decision is cheap and reversible, skip the diligence and just try the tool. `second-witness` is for the calls that cost a migration to undo. It will also tell you this itself.

---

*Built by Nicolás Patrón. I evaluate AI tooling for a consultancy and a family-holding AI build; this researcher is the one I run my own adoption calls through. It informs a decision — it doesn't make it, and it's never a substitute for testing a tool on your own workload before you commit. MIT licensed.*
