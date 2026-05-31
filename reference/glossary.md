# glossary

Terms used across this folder. Two layers: domain terms (AI-tooling) and internal terms (how this researcher works).

## Domain (AI tooling)
- **p99 / p99.9**: the latency below which 99% (or 99.9%) of requests complete. The tail (p99.9) is where production pain lives; vendors often show only p99.
- **Filter selectivity / cardinality**: in a vector DB, how much metadata filtering narrows the search, and how many vectors total. Both dominate real latency and are usually absent from vendor benchmarks.
- **Recall@k**: fraction of the true top-k results a retrieval system actually returns. The quality number that matters for retrieval, distinct from latency.
- **Benchmark contamination**: when a model was trained on data that includes the benchmark, inflating its score. Check benchmark-release date vs model training cutoff.
- **RAG**: retrieval-augmented generation — feeding retrieved documents into a model's context to ground its answers.
- **Effective vs nominal context**: a "1M token context" (nominal) where recall degrades badly past some smaller N (effective).
- **SOC 2 Type I vs Type II**: Type I attests controls exist at a point in time; Type II attests they operated over a period. Type II is the meaningful one.
- **Egress fees**: charges to move your data *out* of a vendor. A core lock-in cost, often unmentioned.
- **Cost cliff**: the jump in price at the next usage tier; the real cost question at scale.

## Internal (how I work)
- **Witness**: any source for a claim. The vendor is the *first* witness; an independent source is the *second*.
- **Witness ladder / tier**: the credibility ranking of a source (tier 1 independent reproduction → tier 5 anecdote). See `source-credibility-tiers.md`.
- **Load-bearing claim**: a claim the buyer's decision actually depends on. Investigated hardest.
- **The three buckets**: Verifiable-now / Biddable / Unknowable-pre-purchase — where each claim lands, which gates the verdict.
- **Biddable**: a claim that only a pilot on the buyer's own data/scale can settle.
- **Grounding**: re-stating a claim in terms of the variable that moves it for *this* buyer's conditions.
- **The map of certainty**: the output format — confirmed / biddable / unknowable + prices — instead of a single yes/no.
- **Decision trace**: the 2–4 closing lines naming which claims drove the verdict and their buckets.
