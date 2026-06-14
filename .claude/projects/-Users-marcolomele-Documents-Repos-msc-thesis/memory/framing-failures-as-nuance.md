---
name: framing-failures-as-nuance
description: How to frame the thesis's mechanistic failure analysis — refinement of the hypothesis, not its refutation
metadata:
  type: feedback
---

The mechanistic analysis (Ch. 4, `sec:gap-analysis`) is a study of the pipeline's **failure modes**, not a verdict against the "language as a bridge" hypothesis. Frame it this way in all prose and discussion:

- The gap to SOTA is a discrete, locatable, in-principle-fixable "dead mass" (~31–32% of cases), not uniform imprecision.
- Removing the dead mass alone (+0.18 IoU) would reach LM-EEC performance. So the training-free pipeline **has the potential to match the SOTA** once failures are addressed with more nuanced strategies.
- The failures reveal where nuance is needed (canonical naming with preserved scene context, an external verification gate for confident-wrong cases, a re-acquisition trigger for propagation drift) — they do **not** indicate hypothesis failure. The hypothesis holds; the failures refine it.

**Why:** This is the author's intended reading of the results and the strongest honest version of the argument. Treating the failures as "tensions" or contradictions of the thesis misrepresents the work.

**How to apply:** When summarizing, contrasting, or drafting around the results/analysis, present failures as refinements/leverage points toward SOTA-level potential, not as evidence the hypothesis broke down.
