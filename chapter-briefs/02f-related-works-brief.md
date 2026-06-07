## Chapter Brief — 02f Related Works

**File:** `latex/chapters/02f-related-works.tex`
**Target length:** ~5 pages
**Role in the thesis:** Closes Chapter 2 (Theory & Related Works) by surveying the directly relevant prior work — datasets, component models, and ego-exo correspondence methods — and positioning our pipeline relative to them. Mirrors the four-paragraph structure of the companion paper's related-works section.

---

### Expansion directive

Write this section as a flowing literature review divided into four sub-sections. Each sub-section should close by drawing an explicit line to the theoretical building blocks covered in §2a–2e or to design choices in Chapter 3.

**§2f.1 Ego and Ego-Exo Datasets**
Introduce the egocentric video lineage: EPIC-Kitchens established large-scale egocentric benchmarks; Ego4D broadened scope and standardised evaluation; Ego-Exo4D introduced paired ego/exo recordings and the Correspondences benchmark (the evaluation protocol for this work). Describe the Correspondences benchmark annotation protocol — per-frame object masks in the exo view corresponding to ego source — and the class distribution relevant to our experiments.

**§2f.2 Learning Correspondences**
Survey handcrafted local descriptors (SIFT, SURF, ORB) through learned sparse matchers (SuperPoint + SuperGlue) to detector-free dense methods (LoFTR, RoMa) and DINO-based semantic correspondence. Frame ego-exo correspondence as an extreme version of this problem: wide baseline, viewpoint flip, no shared geometric prior. This prepares the reader to understand why classical matchers fail and why our geometry-based signal (RoMa, §3) and semantic signal (Grounding DINO, §3) are the right tools.

**§2f.3 Segmentation Models**
Rapid survey of the segmentation milestones treated mechanistically in §2b and §2c — reframed here as prior art. Group them: (a) classical convolutional segmenters (FCN, Mask R-CNN); (b) promptable foundational models (SAM, SAM2, SAM3); (c) language-conditioned segmenters (Grounding DINO, LISA, GLaMM, PSALM, Sa2VA); (d) memory-based VOS (XMem, STM). Note that XMem is both a baseline (official XMem) and a component family (memory VOS) — distinguish the two uses clearly.

**§2f.4 Ego-Exo Correspondence Methods**
The core positioning paragraph. Describe each prior method:
- **XSegTx** — cross-view segmentation transfer; requires paired supervision.
- **XMem (as baseline)** — memory-based VOS applied cross-view; no paired training but limited to visual feature matching.
- **XMem + XSegTx** — combined official baseline.
- **ObjectRelator** — learns relational visual features for cross-view object matching; supervised on ego-exo pairs.
- **O-MaMa** — 2024 challenge winner; mask matching in a DINOv2 latent space learned from paired data.
- **V2-SAM** — adapts SAM for video-to-video correspondence; requires task-specific fine-tuning.
- **LM-EEC (SOTA)** — language-mediated ego-exo correspondence; closest to our approach in motivation; validates SAM 2's tracker for cross-view use.

Close with the positioning statement: all prior methods either learn visual correspondences from paired ego-exo supervision or require task-specific fine-tuning; our pipeline treats the task as training-free composition of foundation models with language as the semantic bridge — the central contribution of Chapter 3.

---

### Citation keys to use

From `references.bib` (use verbatim):
- Datasets: `grauman2022ego4d`, `grauman2024egoexo4d` (verify exact keys)
- Segmentation: `long2015fcn`, `he2017maskrcnn`, `kirillov2023sam`, `ravi2024sam2`, `ravi2025sam3`, `lai2024lisa`, `rasheed2024glamm`, `psalm2024eccv`, `yuan2025sa2va`, `cheng2022xmem`, `oh2019stm`
- Correspondence: `lowe2004sift`, `bay2008surf`, `rublee2011orb`, `detone2018superpoint`, `sarlin2020superglue`, `sun2021loftr`, `roma`, `oquab2023dinov2`
- Prior ego-exo methods: `fu2025objectrelator`; add keys for XSegTx, O-MaMa, V2-SAM, LM-EEC when confirmed in `references.bib`

Back-references: `\cref{sec:visual-representation}`, `\cref{sec:correspondence-segmentation}`, `\cref{sec:language-grounding}`, `\cref{sec:addition-of-time}`.
