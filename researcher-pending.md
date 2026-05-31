# researcher-pending

The catch file. When the researcher drifts in a real session, the deviation gets logged here before it gets a rule. This is how the folder stays alive instead of frozen — `rules.md` only changes after a deviation shows up enough times to earn a line. Nothing here is a rule yet; these are observations on probation.

Format per entry: **what happened → why it's a drift → candidate rule (if it recurs).**

---

## Observed during build + cold testing

### 1. Summary creep on *well-specified* inputs
- **What happened:** when a buyer pastes a fully-framed claim (decision + conditions + vendor number all present), the researcher sometimes skips straight to a clean recap of the vendor's claim before tiering it — because there's nothing to interrogate, the "first move" reflex relaxes and a summary slips in.
- **Why it's a drift:** Rule 0 says never summarize-on-paste, but the intake gate is what normally prevents it. When the gate is satisfied immediately, the guard is weakest exactly when the input is richest.
- **Promoted to `rules.md` Rule 0 (2026-05):** "When the framing is already complete, the first move is *tiering*, not a recap. Skip the questions, not the witness ladder." Recurred across cold testing on well-specified pastes, so it earned a line — this is the catch-file doing its job.

### 2. Naming a source vs actually weighing it
- **What happened:** under time pressure the hunt step can name the right place to look (VDBBench, a rolling leaderboard) without reporting what it *found there* — citing the existence of a witness instead of its testimony.
- **Why it's a drift:** the whole value is the second witness's *content*, not a reading list. "You should check VDBBench" is homework; "VDBBench at your selectivity shows X, and here's the gap to your config" is research.
- **Candidate rule:** "A named source with no found/not-found result is an incomplete hunt. State what the witness said, or that it was searched and came back empty." (Already half-covered by `investigation-workflow.md` Step 4 — promote to `rules.md` if it recurs.)

### 3. The temptation to fill an [illustrative] number with a real-looking one
- **What happened:** when an example needs a figure, there's a pull to produce a plausible specific number rather than tag it illustrative — the confident-voice reflex.
- **Why it's a drift:** fatal for a source-rigor tool. A fabricated benchmark in our own output is the exact failure we warn buyers about.
- **Status:** held hard by `rules.md` Voice rules ("Never invent a number"). Logged here as a permanent watch item, not a candidate rule — this one never gets relaxed.

### 4. Over-asking on the intake gate
- **What happened:** occasionally asks all four intake questions even when two would scope the investigation, making the gate feel bureaucratic.
- **Why it's a drift:** `rules.md` says "I do not gate forever" and to stop once there's enough to scope — over-asking violates the spirit while obeying the letter.
- **Candidate rule:** "Ask the *fewest* questions that change the investigation. If two answers scope it, stop at two." (Soft-coded in the intake gate; watch whether it needs hardening.)

---

*If you're using this folder and you catch a drift not listed here, that's the most useful thing you can send back. The researcher is supposed to earn its rules from real sessions, not from the author's imagination.*
