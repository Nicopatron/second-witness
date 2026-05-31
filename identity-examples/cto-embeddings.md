# identity-example — the CTO standardizing on an embeddings model

One of three worked buyer profiles. Not a template to fill — a real shape of person this researcher serves, with the decision they're on the hook for and the paste they'd actually send. Use it to sanity-check that `rules.md` behaves differently depending on *who* is asking and what's at stake.

---

## Who

**Priya, CTO of a 30-person B2B SaaS.** Owns the infra bill and the on-call pager. Has shipped retrieval before; has also eaten a re-embedding migration once and never wants to again. Reads benchmarks but doesn't trust them. Talks in SLAs and blast radius.

## The decision

Pick *the* embeddings model the whole product will standardize on for the next ~18 months. Switching later means re-embedding the entire corpus and re-tuning every downstream threshold — a quarter of eng time. So this is the opposite of reversible.

## What's load-bearing for her

- **Re-embedding cost** is the real lock-in, not the API price. The model choice marries the corpus to a vector space.
- **Multilingual quality**, because half the corpus is Spanish — and the headline MTEB-style score is almost certainly English-weighted.
- **Version drift:** if the provider silently updates the model, her thresholds move under her.

## The paste she'd send

```
We're standardizing embeddings for our whole product — ~12M docs, half Spanish. Provider claims top-of-leaderboard retrieval quality. We'd be locked in once we embed everything. What am I not seeing?
```

## What the researcher does differently for her

It does **not** start ranking embeddings models. It surfaces that the leaderboard number is the wrong witness for her corpus (English-weighted, her load is half Spanish), tiers the "top-of-leaderboard" claim as tier-3/4, and **grounds on her actual variables**: multilingual retrieval quality on *her* document distribution, and the re-embedding cost as the lock-in that dwarfs API price. It buckets: multilingual quality → biddable (an eval on a sample of her own Spanish docs, with recall@k she defines); version drift → unknowable-pre-purchase unless the provider offers pinning → check the ToS, price the risk. The verdict map ends on "the leaderboard isn't your witness; your Spanish corpus is."

*Why this profile exists: high stakes, irreversible, and the headline metric is measured on conditions unlike hers — the case where summarizing the leaderboard would do the most damage.*
