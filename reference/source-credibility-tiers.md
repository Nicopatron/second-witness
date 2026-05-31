# source-credibility-tiers

The witness ladder, expanded with AI-tooling-specific instances. Used at the "tier every claim" step. A claim's tier = the **highest independent witness** found for it, adjusted for recency and condition-match.

## The five tiers

### Tier 1 — Independent reproduction
A third party ran the test and published method + data + version.
- Examples: an independent lab re-running a model on a held-out eval; a practitioner's reproducible benchmark repo with hardware + config + dataset; an academic replication study. Concretely in 2026: a published VDBBench run at a stated selectivity; the Chroma "context rot" study re-testing many models on long-context recall.
- Caveat: independence ≠ relevance. A reproduction on different hardware or data shape than yours is tier-1 evidence for *a* claim, not necessarily *your* claim.

### Tier 2 — Peer-context work
Neutral comparative work that situates the claim against alternatives.
- Examples: peer-reviewed papers with baselines; multi-tool benchmark suites run by a neutral party; a comparative writeup by someone with no stake. Concretely in 2026: contamination-resistant or rolling leaderboards (SWE-bench Pro, LiveCodeBench, GPQA Diamond), Stanford HELM, the human-preference arena LMArena, the Hugging Face Open LLM Leaderboard. Note even these have caveats — check who funded the suite and whether the tasks match yours.
- Caveat: check the date hard. In AI tooling, a 14-month-old comparison can be describing a different product.

### Tier 3 — Vendor docs / vendor benchmark
The first witness. Has an incentive. The starting point, almost never the finish.
- Examples: the vendor's published latency numbers, their own eval results, their changelog claims, their architecture docs.
- How to read it: assume the vendor chose the most favorable true configuration. Ask what they measured *on* and what they left unsaid.

### Tier 4 — Marketing / launch material / demos
Framing, not evidence. Tells you what's being claimed, not whether it's so.
- Examples: launch blog posts, "SOTA" announcements, conference demos, comparison tables the vendor drew.
- Use: extract the testable claim, then drop the adjectives and go tier it.

### Tier 5 — Anecdote
A lead to chase, not a finding.
- Examples: a tweet, a single "we use it in prod and love it," a Reddit thread, a HN comment.
- Use: aggregate enough tier-5 dissent and you have a hypothesis to test (e.g., "three independent people report memory blowup at scale" → a biddable claim to verify).

## Adjustments that override raw tier

- **Recency**: a tier-3 vendor number from last month can beat a tier-2 paper from last year if the product changed. Always compare dates and versions, not just tiers.
- **Condition-match**: tier-1 evidence measured on conditions unlike the buyer's is downgraded for *their* decision.
- **Incentive-of-the-independent-source**: a "neutral" benchmark funded by a competitor is not tier-2. Check who paid.
- **Metric definition**: two witnesses using different definitions of the same word (what counts as a "hallucination," what "p99" includes) are not corroborating — they're measuring different things.
