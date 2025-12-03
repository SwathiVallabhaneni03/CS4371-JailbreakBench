# JailbreakBench Project Intro Script (Student Voice)

## Speaking guide (3 minutes max)

**Opening (~20s)**
- "Hi everyone, I'm walking you through our CS4371 project on JailbreakBench. It's an open-source benchmark we used to check how well large language models defend against jailbreak attempts that try to trigger restricted or harmful outputs. We leaned on the authors' goal of making the benchmark standardized and reproducible, and we tried to extend it with our own experiments."

**What we built and tested (~30s)**
- "As a team, we set up the benchmark with a mix of published jailbreak prompts and our own tweaks. We ran evaluations across several safety-aligned models, logged attack success rates, and compared how quickly defenses recovered from bad outputs. We also tuned the defense settings so we could see the trade-off between blocking risky prompts and keeping helpful responses."

**Why `defense.py` matters (~30s)**
- "`defense.py` is our reference defense layer. It's useful because it gives us a consistent allow/monitor/block policy and clear reasons when a prompt is flagged. That consistency made it easier to compare different models and to debug why a prompt was refused. We leaned on it as a baseline so any improvements we tried—like different prompt filters—could be compared fairly."

**Step-through of `analyze()` (~60s)**
- "The `analyze(prompt)` function is the decision engine and here's what it's doing in plain language:
  1) It calls `_smoothed_score`, which perturbs the prompt a few ways, scores each variant with `_raw_score`, then blends the max and average scores (60/40). That smoothing reacts to worst-case phrasing without over-weighting a single odd variant.
  2) `_benign_adjustment` subtracts a small offset for obviously harmless content, so we don't over-block safe prompts.
  3) The final score is clamped between 0 and 1, then checked against two thresholds: if it crosses `block_threshold`, the label is `blocked`; if it's between `monitor_threshold` and `block_threshold`, it's `monitor` (still refused but gentler messaging); otherwise it's `allow`.
  4) `_sanitize` scrubs risky details, and `_build_response` returns the final text with the label, reasons, and whether we blocked the prompt."

**How the thresholds shaped our results (~30s)**
- "The two thresholds were our tuning knobs. A higher `block_threshold` cut false positives but let some edgy prompts through; lowering `monitor_threshold` widened the gray zone where we responded cautiously. By sweeping those values, we plotted how safety and usability shifted, which was a big part of our analysis."

**Closing (~30s)**
- "So that's our JailbreakBench work: we built and ran the benchmark, used `defense.py` to standardize decisions, and showed how smoothing plus allow/monitor/block thresholds can be calibrated. Thanks for listening!"
