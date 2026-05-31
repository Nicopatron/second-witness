# hype-patterns

Red flags in AI-tool marketing. Each is a pattern that makes a claim *look* witnessed when it isn't. When I see one, I don't dismiss the tool — I downgrade the claim's tier and go hunt the real number.

## Benchmark games
- **Cherry-picked config**: the one hardware/dataset/setting where they win. Ask: what's the config, and is it yours?
- **Cherry-picked baseline**: "2x faster than X" where X is the slowest or oldest competitor. Ask: faster than the *current* leader?
- **Contamination**: model scores high on a benchmark that was in its training data. Ask: when was the benchmark released vs the model's training cutoff?
- **Metric redefinition**: "99% accuracy" on a metric they defined. Ask: what counts as a hit? Who else uses this metric?
- **p99 without p99.9**: tail hidden one digit over. Ask for the next nine.
- **Aggregate hiding distribution**: "average latency 20ms" can hide a brutal tail. Ask for percentiles, not means.
- **Nominal without effective** (AI-specific): "1M-token context," "handles your whole codebase." Nominal window ≠ effective recall — accuracy degrades well before the advertised limit ("context rot"), and the needle-in-haystack test that vendors cite measures only lexical retrieval, not the multi-hop reasoning real tasks need. Ask for recall at *your* document length and task shape, not the headline token count.

## Version & time games
- **Stale-claim recycling**: a true number from v1, still on the site at v3. Check the date on every figure.
- **Roadmap-as-present**: "supports X" where X is "coming soon." Ask: shipped, in beta, or planned?
- **Demo ≠ product**: the conference demo ran on a curated input. Ask: is this in the GA product, at my input distribution?

## Framing games
- **Adjective stacking**: "revolutionary, state-of-the-art, enterprise-grade." Strip every adjective; what testable claim remains?
- **"Drop-in replacement"**: rarely is. Ask what breaks: query language, data format, latency profile, your eval harness.
- **Borrowed authority**: "used by [big logo]" — for what, at what scale, still using it? Logos aren't witnesses.
- **"Open" theater**: "open standard" / "no lock-in" while the data format is proprietary. Test the export.

## Social-proof games
- **Testimonial selection**: the three happy customers, none of the churned ones. Find someone who left.
- **Manufactured consensus**: a wave of launch-day posts that all rhyme. Volume ≠ independence; check if they're incentivized (affiliates, early access, employees).
- **Authority laundering**: "as seen in [publication]" where the publication ran the vendor's press release.

## How I use this file
When a claim arrives, I scan for these patterns. A hit doesn't kill the tool — it tells me the claim is currently at tier 3–4 and the real evidence is elsewhere. The pattern is the *signal to go hunt*, not the verdict.

*A vendor exhibiting these patterns isn't necessarily lying — marketing does this by default. My job isn't to catch them; it's to not mistake the framing for the finding.*
