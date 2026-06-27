# Thesis Defense — Presentation Content Outline

**Stage:** content only (slide order, information, figures, timing). Design decided after supervisor feedback.
**Target:** ~20 minutes oral presentation, one-directional academic style.
**Slides:** 22.
**Timing budget:** Title 0:20 · Introduction 3:00 · Theory 3:30 · Method 3:30 · Results & Analysis 9:00 · Conclusion 1:00 (≈20:20).

**Emphasis (assessment rationale):** the board rewards critical reasoning and analytical skill over re-explaining known models. So theory is framed as *tool → property we exploit → limitation that bites later*; method is rapid-fire and carries one idea (the transfer/completeness split); the analysis of the pipeline's own failures is the centerpiece.

Conventions per slide: `FIGURE:` names a file under `latex/imgs/...` (verified to exist) or is marked **to-create**. Slide bullets stay terse; numbers live in `NOTES`. All figure paths below were checked against the repository.

---

## ⚑ MAIN QUESTION FOR THE SUPERVISOR (decide first)

The reference research project **continued after thesis submission**, and two of the strongest points below are post-submission findings (slides 16 and 21, tagged **[POST-SUBMISSION]**). This forces one decision that shapes the whole talk:

> **Should the presentation be restricted to the content of the submitted thesis, or include the continued research — even though that pushes the narrative (and likely the time) beyond the thesis's scope?**

- **Option A — thesis-only:** safest, fully defensible against "is this in your thesis?"; keeps to 20 min. Slides 16/21 use only submitted results.
- **Option B — include continued work:** stronger story (a sharper comparison framing and a concrete, working fix for the main failure mode), shows the research is alive; but risks scope/time questions and a board member asking why it is not in the document.

Everything tagged **[POST-SUBMISSION]** below is written so it can be dropped (Option A) or kept (Option B) without restructuring the deck.

---

## Slide 1 — Title
FIGURE: none
- Thesis title; candidate name; supervisor; Bocconi MSc in Data Science.
- One-line subtitle: a training-free, language-bridge pipeline for cross-view object correspondence.
NOTES (~20s): Open with name and title; defer detail to the next slide.

---

### Introduction — 3 min

## Slide 2 — The problem: cross-view object correspondence
FIGURE: `latex/imgs/ch1/cross-view-intro.png`
- Same object, two synchronized cameras: egocentric and exocentric.
- Given the object's mask track in one view, recover it in the other.
- Matters for AR, robotics, and multi-agent collaboration.
NOTES (~55s): Define the task in plain terms over the figure; the object a person holds (ego) must be located in the third-person view (exo) and vice versa. Frame it as a building block for systems that must share what they see.

## Slide 3 — Why it is hard, and where prior methods fall short
FIGURE: `latex/imgs/ch1/egoexo-many-examples.png`
- Extreme gap in scale, angle, lighting, field of view; no geometric calibration.
- Existing methods are trained on the benchmark, tied to camera setups, hard to interpret.
NOTES (~55s): The viewpoint gap defeats geometric matching, and learned fusion methods buy accuracy with supervision that does not transfer. Their decisions sit in opaque weights, so a wrong answer cannot be explained. This sets up our two design goals: no training, and legibility.

## Slide 4 — Our idea: language as a bridge
FIGURE: `latex/imgs/ch1/lang-bridge-intro.png`
- Describe the object in one view, find it in the other through that description.
- Frozen foundation models, composed, never trained for this task.
- Contribution: training-free · language bridge · interpretable · camera- and dataset-agnostic.
NOTES (~60s): State the hypothesis: natural language is a semantic channel that survives the viewpoint gap. "Two eyes and one mouth" — two visual systems mediated by one description. The contribution is the first training-free, language-mediated approach, and one whose every intermediate output a human can read.

---

### Theory — 3.5 min (one family per slide; framed property → limitation → design consequence)

## Slide 5 — Visual representation
FIGURE: `latex/imgs/ch2/dinov3-results.png`
- ViT + self-supervised DINO: semantic patch features, no labels.
- Property: features carry object identity, robust to appearance shift.
- Limitation: even these degrade under the extreme ego-exo gap.
NOTES (~50s): State the property and the limitation, then the consequence: because the strongest visual features still collapse across this viewpoint gap, pure feature/geometric matching is not enough — which is what motivates reaching for language. This frames the whole thesis as a response to a limitation of the visual substrate, not a dismissal of it.

## Slide 6 — Segmentation
FIGURE: `latex/imgs/ch2/sam2.png`
- SAM: promptable masks. SAM 2: memory-based propagation through video.
- Property: tracks robustly through occlusion and fast motion.
- Limitation: it faithfully propagates whatever seed it is given.
NOTES (~50s): SAM 2 is the right tool for temporal completeness, so we delegate that to it. But the same fidelity means a wrong seed propagates perfectly to a wrong answer — flag this now, because it explains later why propagation contributes ~0% of our failures yet cannot rescue a bad anchor.

## Slide 7 — Language grounding
FIGURE: `latex/imgs/ch2/gdino-architecture-examples.png`
- CLIP → Grounding DINO → SAM 3: align text to image regions.
- Property: open-vocabulary — locate a concept from words.
- Limitation: contrastive training aligns whole image to whole caption.
NOTES (~55s): This chain is the bridge: words become a location. The limitation is the load-bearing detail — because alignment is image-to-caption, a paragraph of attributes dilutes the match while a short noun phrase concentrates it. That is exactly why Stage 3 compresses the description into a phrase, a design choice that falls directly out of how CLIP was trained.

## Slide 8 — Foundation models and agents
FIGURE: `latex/imgs/ch2/react-scheme.png`
- LLM/Qwen + ReAct: reason, call a tool, observe, repeat.
- Property: the VLM can describe an object and self-verify masks.
- Limitation: it reports what it perceives, not what is annotated.
NOTES (~55s): The agentic loop gives us both the description and a built-in verification step, and it keeps every step legible. The limitation foreshadows the dominant failure mode: when the target is tiny, the model honestly names the salient neighbour it can actually see. We return to this as the single largest source of error.

---

### Method — 3.5 min (one idea, then rapid-fire stages)

## Slide 9 — Task formulation and the key insight
FIGURE: none (simple two-box schematic, optional **to-create**)
- Input: object mask track in the source view. Output: mask for every destination frame.
- Two orthogonal sub-problems: cross-view transfer (hard) vs. temporal completeness (solved).
- Spend effort on transfer via language; delegate completeness to SAM 2.
NOTES (~50s): This is the one idea the board should leave with. Transfer across views need only succeed on a few frames; a tracker fills the rest. Every design choice downstream follows from this split — say it slowly, then move fast.

## Slide 10 — Pipeline overview
FIGURE: `latex/imgs/ch3/pipeline_full_columns.png`
- Four stages: source frame selection → description → anchor grounding → propagation.
- Every intermediate output is human-readable.
NOTES (~40s): One left-to-right pass naming each stage and its model. Stress legibility — seed frame, description, anchor masks are all inspectable — then go rapid-fire.

## Slide 11 — Stage 1: source frame selection (rapid)
FIGURE: `latex/imgs/ch3/pipeline_full_columns.png` (column 1)
- Pick the clearest seed frame by a visibility score (area-dominated). K=1.
NOTES (~30s): A bigger object gives the VLM more to describe; one good frame is enough. Detail deferred to Q&A / backup.

## Slide 12 — Stage 2: object description (rapid)
FIGURE: `latex/imgs/ch3/pipeline_full_columns.png` (column 2)
- VLM writes view- and time-independent attributes; masked frame + raw frame in, JSON out.
NOTES (~30s): Only intrinsic attributes, so the description holds from any viewpoint at any moment. The overlay points the model; the raw frame preserves appearance.

## Slide 13 — Stage 3: anchor grounding (rapid)
FIGURE: `latex/imgs/ch3/pipeline_full_columns.png` (columns 3.1 / 3.2); optional `latex/imgs/ch4/anchor-selection.png`
- Grounding DINO re-selects anchors by confidence in the destination (K=3); SAM 3 agent segments and self-verifies.
- Anchor signal is swappable: language (G-DINO) or geometry (RoMa).
NOTES (~40s): One line of reasoning to keep: the same time index is rarely the best destination frame (ρ = 0.14), so we re-select. The bridge stays linguistic; the anchor signal could be geometric instead.

## Slide 14 — Stage 4: propagation (rapid)
FIGURE: `latex/imgs/ch3/pipeline_full_columns.png` (column 4)
- SAM 2 memory tracker fills all frames; bidirectional, each frame served by its nearest anchor.
NOTES (~30s): Memory reaches back to a distant reliable anchor when nearby frames fail. Two passes meet so every frame has a short path to a good anchor.

---

### Results & Analysis — 9 min (the centerpiece)

## Slide 15 — Setup and qualitative successes
FIGURE: `latex/imgs/ch4/pipeline-qualitative-examples.png`
- Ego-Exo4D Correspondences v2 test split; metrics IoU (primary), LE, CA.
- Inference only, single NVIDIA H200; Qwen 3.5 35B + Grounding DINO + SAM 3 + SAM 2.
NOTES (~55s): No component is trained, unlike every competitor. Show clean successes in both directions before turning to the aggregate numbers.

## Slide 16 — Results vs. state of the art
FIGURE: results table (**to-create** from `04-experiments.tex`, `tab:main`)
- Ours: 37.7 (Ego2Exo) / 40.6 (Exo2Ego) IoU — training-free.
- Realistic comparator → V²-SAM: 46.3 / 49.6 (no backbone retraining on the benchmark).
- In-distribution ceiling → LM-EEC: 54.98 / 65.77 (full SAM 2 fine-tuned on the benchmark).
- We beat ObjectRelator (35.3 / 40.3); only method training-free + camera- + dataset-agnostic.
NOTES (~80s): Frame the two comparators differently — this is the analytical point, not the raw gap. LM-EEC is an **in-distribution upper bound**: it fine-tunes the full SAM 2 backbone on the benchmark, so its number reports what a retrained model achieves on data it has seen, not what a fair like-for-like method achieves. The honest comparator is **V²-SAM**, which does not retrain a backbone; against that frontier our training-free pipeline sits a few points back while paying none of the supervision cost. [POST-SUBMISSION] This reframing crystallized after submission — if Option A, present LM-EEC plainly as "strongest published" without the upper-bound argument.

## Slide 17 — Ablation: what each stage contributes
FIGURE: ablation table (**to-create** from `04b-experiments-ablations.tex`, `tab:abl-summary`)
- Naive baseline 10.6 IoU → full pipeline +259.8%.
- Frame selection ≈ oracle (16.8 vs. 17.0), no privileged info.
- Propagation matches per-frame agent (37.7 vs. 35.5) at ~25× speed (0.82 vs. 21.30 s/frame).
NOTES (~70s): The numbers validate the design split. Running SAM 3 on every frame works but is impractical (59 hours); propagation recovers the same accuracy cheaply. Grounding-DINO anchors keep the bridge linguistic.

## Slide 18 — The gap is bimodal
FIGURE: `latex/imgs/ch4/pipeline-lmeec.png`
- ~32% (Ego2Exo) / 31% (Exo2Ego) "dead mass": near-zero IoU.
- Surviving cases (working mode): 56.9 / 61.7 IoU.
- Remove the dead mass → average rises ~18, level with LM-EEC.
NOTES (~75s): The deficit is not uniform imprecision; it is half discrete failure, half ordinary error. The working mode already competes with the state of the art. The whole question becomes: what causes the dead mass?

## Slide 19 — Where the loss is born
FIGURE: `latex/imgs/ch4/grounding-examples.png` (or waterfall chart, **to-create** from `tab:waterfall`)
- Charging each failure to its first failing stage.
- Stage 2 naming: 59% (Ego2Exo) / 81% (Exo2Ego) of dead mass.
- Stages 2+3 together: 74% / 92%. Propagation: 0% by construction.
NOTES (~70s): The loss is in cross-view transfer, not tracking. An anchor on the wrong object propagates faithfully to the wrong answer, so the failure is decided early, at description and grounding.

## Slide 20 — Why the VLM mislabels: perception, not vocabulary
FIGURE: `latex/imgs/ch4/vlm-description-failure.png`
- On dead cases the source mask is 3–6× smaller; the model names a real, larger neighbour.
- It never hallucinates an absent object — the failure is perceptual, not lexical.
NOTES (~80s): This is the analytical heart. With too few pixels the model honestly describes the salient surface or tool the target rests on. The diagnosis matters because it rules out the obvious fix (a bigger vocabulary or better prompt) and points at perception — exactly the kind of evidence-led conclusion the board is listening for.

## Slide 21 — Testing the diagnosis: disciplined negatives → a concrete fix
FIGURE: none (small 2–3 row results strip, **to-create** from §4.5) or reuse `vlm-description-failure.png`
- Scene-vocabulary conditioning, destination cropping, frame brightening — each tried, each net-negative.
- Cropping helps failing frames but corrupts the many already-correct ones; brightening confirms pixels are not the constraint.
- Negatives point the same way: the fix must use the VLM better, not pre-process the image.
- [POST-SUBMISSION] Two working directions: (i) pass the **mask signal into the VLM** so it describes the target, not its neighbour; (ii) add a **judge layer after SAM 3** to filter candidate masks against the description.
NOTES (~80s): Frame this as the practitioner's method, not a list of misses — this slide is where critical-reasoning scoring lives, so do not rush it. The negatives are informative precisely because they rule out the cheap fixes (vocabulary, cropping, lighting) and isolate the constraint at how the VLM perceives small objects. [POST-SUBMISSION] That diagnosis directly motivated the continued work: feeding the mask signal to the VLM addresses the perceptual mislabel at its source, and a judge stage after SAM 3 catches the confident mis-groundings that have no internal confidence signal. If Option A, end on the diagnosis and move these two directions to "future work" / a backup slide instead of presenting them as done.

---

### Conclusion — 1 min

## Slide 22 — Conclusions and future work
FIGURE: none
- Language is a viable cross-view bridge — and, crucially, a legible one.
- Legibility turned the gap to SOTA into a diagnosis, not a verdict.
- Near-term: description optimization (mask signal to the VLM), stronger grounding, a judge/verification gate.
- Paradigm: legibility vs. performance trade-off; composing foundation models rather than training them.
NOTES (~60s): Close on the larger claim. As foundation models absorb more general understanding, the most capable systems may be composed rather than trained, with language the human-readable medium that keeps people in the loop. [POST-SUBMISSION] Under Option A the two continued-work directions live here as "near-term future work"; under Option B they were already shown working on slide 21, so here just point forward to the open paradigm questions instead of re-listing them.

---

## Open questions / figures to create

- **Results table (slide 16)** — render `tab:main` as a slide graphic; decide which competitors to keep (suggest: XMem+XSegTx, ObjectRelator, O-MaMa, V²-SAM, LM-EEC, Ours) and highlight the three "agnostic" columns.
- **Ablation table (slide 17)** — simplify `tab:abl-summary` to IoU + time/frame; possibly a bar chart instead of a table.
- **Failure waterfall (slide 19)** — `tab:waterfall` is currently a table; a waterfall/stacked-bar chart would read better on a slide.
- **Task schematic (slide 9)** — optional two-box "transfer vs. completeness" diagram; could reuse part of the pipeline figure instead.
- **Interventions strip (slide 21)** — small table/figure summarizing the three §4.5 negative results (on/off deltas); keep it readable, not a dense table.
- **Backup slides (Q&A)** — candidates: full theory detail (2a–2f), VLM-backend comparison, anchor-count K sweep, per-intervention numbers, scenario/object-size breakdowns, Stage-1/2 mechanics deferred from the rapid slides. Decide after supervisor feedback.
- **Figure legibility** — pipeline figure columns are referenced on slides 10–14; confirm a single full-pipeline figure reads at projection size, or split it per stage.
