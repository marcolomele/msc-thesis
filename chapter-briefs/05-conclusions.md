# Chapter Brief: 5 — Conclusions

---

You are helping me draft Chapter 5 (Conclusions) of my master's thesis.
The project knowledge files contain my style guide, scope, writing samples,
and thesis outline. Read them before drafting.

**Chapter**: Conclusions
**Target length**: 2 pages
**Arc**: C — Our work first (recap the contribution, then pivot to open questions)
**Deep treatment**: The synthesis of the failure analysis into open questions / future
directions — this is the chapter's only original contribution. Everything else is recap.
**Surface only**: The pipeline architecture, the SOTA comparison, and the ablation
mechanics — one or two sentences each, since they are fully packaged in Chapters 3 and 4.
Do not re-explain, only reference with \cref.
**Skip**: Any new results, tables, figures, or equations. No re-derivation of the method.
No new citations beyond keys already in the thesis.
**Connect back to**: The abstract and \cref{chap:intro} posed the "language as a bridge"
hypothesis and named the two limitations of prior work (training dependence,
uninterpretability). \cref{chap:experiments} delivered the bimodal "dead mass"
failure analysis. Make both links explicit — the conclusion closes the loop the
abstract opened.
**Connect forward to**: NA — this is the final chapter.

---

## Framing directive (non-negotiable — this is the spine of the chapter)

The mechanistic analysis of \cref{chap:experiments} is a study of **failure modes**,
not a verdict against the hypothesis. The chapter must land three points, in order:

1. **The hypothesis holds.** Language can serve as a bridge between frozen foundation
   models. A fully training-free, camera- and dataset-agnostic pipeline reached within
   a few IoU points of the 2025 challenge winner and matched or exceeded LM-EEC on its
   working mode (IoU > 0.7). The two limitations of prior work named in the intro —
   training dependence and uninterpretability — are addressed by construction.

2. **The gap is discrete, locatable, and in-principle fixable.** The deficit to the SOTA
   is not uniform imprecision but a concentrated "dead mass" (~31–32% of cases).
   Removing it alone (+0.18 IoU) would reach LM-EEC-level performance. The failures
   reveal where nuance is needed, not that the paradigm broke down. Legibility is what
   made this diagnosis possible — a property no end-to-end competitor offers.

3. **The open questions are now sharp.** The abstract promised that the failures "pave
   the way for future work." This chapter confirms exactly what that work is, derived
   straight from the failure waterfall:
   - **(i) Constrain Stage 2 naming** to a canonical noun phrase while preserving the
     scene context the name depends on — naming is 59–81% of the dead mass, and it is a
     perceptual failure on tiny objects, not a vocabulary one. Note the negative result:
     cropping the input backfires (0.411 → 0.180), so the fix is not "zoom in."
   - **(ii) An external verification gate** that fires on confident mis-groundings — the
     failures are silent (a mask is produced 93% of the time) and the pipeline's own
     confidence cannot separate hits from misses.
   - **(iii) A re-acquisition trigger** on runs of empty masks to recover the
     propagation drift that only surfaces once the target widens below 0.3 IoU. Unlike
     the dead mass, this failure announces itself and is therefore exploitable.

   Optionally, note one secondary direction (one sentence, not part of the three above):
   **revisiting dense geometric matching as a complementary anchor signal.** RoMa was set
   aside because its correspondences collapse under the ego-exo viewpoint gap
   (\cref{subsec:matching}), but a matcher robust to extreme viewpoint change could
   reinforce the language signal at anchor selection without re-introducing any training.
   Keep this strictly subordinate to the three language-centred directions; it is a hedge,
   not a pillar.

Frame all three as refinements that move a working paradigm toward SOTA potential.
Do NOT frame any finding as a tension with or contradiction of the thesis.

---

## Suggested structure (2 pages, ~4 paragraphs)

1. **Recap of the contribution** (claim → develop). What we set out to test and what we
   built; the training-free, interpretable pipeline and the headline result. Reference
   \cref{chap:pipeline} and \cref{tab:main}, do not re-explain them.
2. **What the results confirmed about the hypothesis** (ground). The working mode matches
   SOTA; the gap is the bimodal dead mass; legibility turned the gap into a diagnosis.
3. **Open questions / future work** (develop the original contribution). The three
   directions above, each tied to its evidence in \cref{chap:experiments} and motivated
   by the relevant theory in \cref{chap:theory}.
4. **Closing** (close). Return to the broader claim from the abstract: natural language
   as a reliable bridge between machine learning architectures, and what this novel
   paradigm opens for future research. End on the paradigm, not on the metrics.

---

Draft the full chapter now. Write in my voice as documented in the style guide.
Use LaTeX formatting throughout: \cref for all cross-references, \cite{key} for any
citation (existing keys only). No new equations, tables, or figures.
After the draft, flag any decisions you made that I should review
(depth calls, skipped content, structural choices).

---

## Post-Draft Checklist

- [ ] Voice matches style guide (read the Red Flags section)
- [ ] Hypothesis framed as confirmed-and-refined, never contradicted
- [ ] The three open questions are each tied to specific evidence in Chapter 4
- [ ] Every cross-reference uses \cref, no \ref
- [ ] No new results, tables, figures, equations, or invented citation keys
- [ ] Backward connections to the abstract and Chapters 3–4 are explicit
- [ ] Closing pivots to the broader paradigm, not the metrics
- [ ] Stays within 2 pages
- [ ] Nothing sounds like it was written by an AI
