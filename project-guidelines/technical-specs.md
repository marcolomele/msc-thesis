# Technical Specifications
# Experiments, Ablations & Interpretability

---

## Pipeline Overview

The pipeline addresses the Ego–Exo object correspondence task from Ego-Exo4D: given a
per-frame segmentation mask track of an object in one camera view, predict the corresponding
mask track in a second, synchronised view of the same scene. The pipeline is fully
training-free. Natural language — rather than learned cross-view features or geometric
calibration — serves as the bridge between viewpoints.

The pipeline operates on a single take (a multi-camera recording from Ego-Exo4D) and
processes one (object, source-camera, destination-camera) triplet at a time. It is divided
into four sequential blocks that play three conceptual roles. The four-block structure
mirrors the paper exactly; the destination-grounding stage that v2 treated as a separate
"Block 2.5" is now the anchor-selection half of Block 3.

- **Block 1 — Source Frame Selection** (optimisation): scores all source-view annotated
  frames by a composite function and selects the single highest-quality seed frame to feed
  the VLM, avoiding redundant calls and maximising description quality. The composite score
  is a weighted combination of normalised mask area (0.99 weight) and centrality (0.01,
  acting as a tie-breaker).

- **Block 2 — Object Description** (semantic bridging): a multimodal LLM (Qwen 3.5 35B)
  generates a structured JSON description of the target object from the seed frame. The
  description contains three fields: `colour`, `object` (canonical noun phrase), and
  `view_independent_descriptors`. The description is explicitly view- and time-independent
  (intrinsic properties only — colour, canonical identity, material, structural parts) so
  it remains valid across cameras and across the take. For each seed frame the VLM receives
  two images: one with the mask overlaid as a red semi-transparent region with a contour
  outline, and one in raw form.

- **Block 3 — SAM 3 Agentic Grounding** (semantic bridging): two stages. *(3a) Anchor
  selection* scores every destination-view frame for object visibility and keeps the top-K
  as anchor frames, using one of two interchangeable signals — **Grounding DINO** (language-
  grounded: open-vocabulary detection with the VLM noun phrase as query) or **RoMa**
  (geometry-based, language-free: counts confident dense feature matches whose source
  endpoint falls inside the source object mask). *(3b) Agentic segmentation* runs an
  independent SAM 3 Agent loop on each anchor frame, with Qwen 3.5 35B as orchestrator —
  simplify description → SAM 3 segments → VLM accepts/rejects each candidate mask → restart
  with a new prompt if all are rejected.

- **Block 4 — Bi-directional Mask Propagation** (completion): SAM 2's video tracker
  (inherited by SAM 3) propagates the accepted anchor masks forward and backward through all
  destination-view frames, conditioning each prediction on a memory bank of anchor and
  recently processed frames via cross-attention.

All foundation models are frozen at inference time. The pipeline output is a per-frame
mask track plus a JSON of frame-level predictions.

**Best-known configuration:** Qwen 3.5 35B as descriptor and agentic planner,
`n_source_frames = 1` seed frame, `K = 3` anchor frames, anchor selection via Grounding DINO
or RoMa (the two are comparable on the full test set; RoMa edges ahead on the ablation
subset), 8 agent generations per anchor frame, PixelRefer mask pre-filter disabled. Run
locally on a single NVIDIA H200 with no fine-tuning.

---

## Dataset & Evaluation Setup

- **Dataset**: Ego-Exo4D Correspondences, v2 test split
- **Evaluation directions**: ego→exo (*Ego2Exo*) and exo→ego (*Exo2Ego*); all ablations run
  in both unless noted
- **Reported metrics** (following the official benchmark):
  - **IoU** (Jaccard Index): intersection over union of predicted and ground-truth masks;
    the primary evaluation metric
  - **LE** (Location Error): normalised centroid distance; measures localisation accuracy
    independently of shape
  - **CA** (Contour Accuracy): similarity between predicted and ground-truth contours after
    translation; isolates shape fidelity from positional offset
- **Internal-only metric (not reported in the paper)**: **VA** (Visibility Accuracy),
  balanced accuracy (TPR + TNR) / 2 for frame-level presence/absence; used during
  development but not in the published tables
- **Secondary metrics**: inference time (wall-clock per take / per frame), VLM-as-judge
  description-quality score (Gemma 4 as judge)
- **Comparison targets**: LM-EEC (overall SOTA — see below); O-MaMa (2025 challenge winner
  and primary peer within the no-backbone-retraining family); challenge leaderboard entries
  ObjectRelator and V2-SAM; official baselines XSegTx, XMem, and XMem+XSegTx

---

## Baseline & SOTA: LM-EEC

LM-EEC (Long-Memory Ego-Exo Correspondence, Hu 2025) is the current state-of-the-art on
the Ego-Exo4D object correspondence task and the primary comparison target for this work.
It adapts SAM 2 for the ego-exo setting with two learned components: a Mixture-of-Experts
fusion module that integrates cross-view features, and a dual compressed long-term memory
system. It achieves strong results but requires non-trivial training on Ego-Exo4D data,
is tied to the distribution of the available cross-view pairs, and is not directly
interpretable — there is no straightforward way to inspect why a particular mask was
produced for a particular frame. Our pipeline is training-free and produces a
human-readable JSON description at each step, making it directly inspectable.

Other challenge entries for context:
- **O-MaMa** (the 2025 Ego-Exo4D Correspondences Challenge winner): generates candidate
  masks in the destination view via FastSAM and selects the best match using pooled DINOv2
  features in a learned cross-view latent space, with only ~1% of ObjectRelator's trainable
  parameters. It is our closest peer — like us it does not retrain SAM's backbone — and our
  primary point of comparison; our pipeline trails it by ~5 IoU in both directions.
- **ObjectRelator** (Fu 2025): fine-tunes PSALM with view-invariant alignment modules,
  incorporating language as an explicit cross-view cue in a trainable component. We surpass
  it in IoU in both directions.
- **V2-SAM** (Pan 2025): adapts SAM 2 with dual prompt generators (geometry-aware anchor
  prompts and appearance-guided visual prompts) and a cyclic-consistency selector.

---

## VLM Backends

### Qwen Family (Block 2 and Block 3)
- **Qwen 3.5 35B no thinking** — default and best-known configuration; serves as both
  object describer (Block 2) and agentic planner/verifier (Block 3); served locally via
  Ollama at temperature 0 with fixed seed for determinism
- **Qwen 3.5 35B thinking** — extended chain-of-thought reasoning at inference time;
  configurable via `agent_thinking` flag
- **Qwen 3.5 35B batching** — per-take VLM batching variant; showed +16.5% IoU over
  baseline at full scale in v1 by letting the VLM disambiguate competing objects in the
  same scene
- **Qwen 3.6 35B no thinking** — updated model version; to be compared against 3.5
- **Qwen 3.6 35B thinking** — updated model version with extended reasoning

### PixelRefer Family (Block 2 alternative backend)
PixelRefer is a mask-conditioned region describer. Unlike Qwen, it takes the binary mask
as input rather than a bounding-box-annotated frame, and generates a region-grounded
description conditioned on pixel-level mask information. It does not require a visual
bounding-box prompt.
- **PixelRefer-7B** — full model
- **PixelRefer-Lite-7B** — architecture variant

### MiniLM (Block 3 optional pre-filter)
`all-MiniLM-L6-v2` (22M parameters) computes sentence-embedding cosine similarity between
the Block 2 description and each candidate mask description from PixelRefer, used to
pre-filter candidate masks before they reach the Qwen verifier. Embeddings are computed
on cleaned plain-prose versions of the JSON descriptions.

---

## Status Legend

- ✓ **Done** — results available
- ⟳ **In queue** — will run before submission
- ? **TBD** — may not run; decision pending

---

## Main Runs

Headline results on the Ego-Exo4D Correspondences v2 test split (IoU):

| Run | Direction | IoU | vs. O-MaMa | vs. baseline | Status |
|---|---|---|---|---|---|
| Ours (Language as Bridge + G-DINO) | ego→exo | 37.7 | −11.5% | +9% | ✓ |
| Ours (Language as Bridge + G-DINO) | exo→ego | 40.6 | −7.9% | +62% | ✓ |
| Ours (Language as Bridge + RoMa) | ego→exo | TBD | — | — | ⟳ |
| Ours (Language as Bridge + RoMa) | exo→ego | TBD | — | — | ⟳ |

Reference points (test v2 IoU, Ego2Exo / Exo2Ego): LM-EEC 54.98 / 65.77 (SOTA),
V2-SAM 46.3 / 49.6, O-MaMa 42.6 / 44.1 (challenge winner), ObjectRelator 35.3 / 40.3,
XMem+XSegTx 34.9 / 25.0. Results are presented in one table reporting IoU, LE, and CA,
with `Training Free / Camera Agnostic / Dataset Agnostic` flags distinguishing our method
from all competing baselines.

---

## Ablations

All ablations are run in both ego→exo and exo→ego directions unless noted. Each ablation
holds all other switches at best-known configuration (ceteris paribus).

---

### Block 1 — Source Frame Selection

**Goal**: compare composite score formulations for source frame quality.

| Variant | Description | Status |
|---|---|---|
| Current (v2) | `q = 0.90 area_ratio + 0.10 centrality`; optimised for object size and on-axis position | ✓ |
| Old (v1) | More complex; combined occlusion (bounding-box fill ratio), visibility (mean masked brightness), saliency, and mask quality | ✓ |

Comparison on IoU, LE, CA, VA.
Note: restricted to frame selection of main v2 with `n_source_frames = 1`.

---

### Block 2 — VLM Backend

**Goal**: compare VLM backends on description quality and inference cost.

| Variant | Status |
|---|---|
| PixelRefer-7B | ⟳ |
| PixelRefer-Lite-7B | ⟳ |
| Qwen 3.5 35B no thinking | ✓ (best-known config; judge 77.0% Ego2Exo / 70.0% Exo2Ego) |
| Qwen 3.6 35B no thinking | ✓ (75.0% / 58.2%) |
| Qwen 3.5 35B batching | ✓ (43% faster but −23.4 pp Ego2Exo quality) |
| Qwen 3.5 35B thinking | ? |
| Qwen 3.6 35B thinking | ? |

Comparison on description-quality score (Gemma 4 as judge, scoring object identity,
contextual descriptors, and view-independent attributes as binary correctness) and on
single-call inference time. Qwen 3.5 35B wins on quality in both directions; the
region-aware PixelRefer models underperform it, supporting the case for general-purpose
foundation models.

---

### Block 2 — VLM Description Content

**Goal**: isolate the contribution of each description field to downstream IoU.

| Variant | Description | Status |
|---|---|---|
| Object only | `object` field only | ⟳ |
| Object + colour | `colour` + `object` fields | ⟳ |
| Object + colour + view-independent descriptors | All three fields (current config) | ✓ |

---

### Block 3 — Anchor (Destination) Frame Selection

**Goal**: isolate the contribution of language-grounded vs. geometry-based vs. random
selection for anchor frame choice. In the paper these are Experiments D (Grounding DINO
family) and E (RoMa family).

| Variant | Description | Status |
|---|---|---|
| D.1 `gdino` | Grounding DINO filters destination frames by language-vision grounding confidence, ranks survivors by bounding-box area; top-K anchors | ✓ |
| D.2 `MaxInfo` | Replaces area ranking with MaxInfo selection, maximising geometric diversity in CLIP embedding space | ✓ |
| D.3 `random` | K anchor frames sampled uniformly at random; control isolating principled selection from multi-frame budget | ✓ |
| E.1 `roma` (fp32) | RoMa scores each destination frame by confident feature matches (conf > 0.5) whose source endpoint falls inside the source object mask; top-K by match count | ✓ |
| E.2 `roma` + subsample | Same RoMa scoring over ≤20 uniformly-sampled frames; −47% latency, −6.2 pp IoU | ✓ |
| E.3 `roma` (fp16) | RoMa scoring in fp16; −30% latency vs. E.1, best ablation IoU (0.433) | ✓ |

Headline: D.1 and E.3 reach comparable IoU on the full test set, so the paper reports both
Grounding DINO and RoMa as the two anchor-selection strategies; RoMa's lead on the ablation
subset is attributed largely to subset noise.

---

### Block 3 — Agent Thinking

**Goal**: measure the effect of extended chain-of-thought reasoning in the agentic loop.

| Variant | Status |
|---|---|
| Thinking on | ⟳ |
| Thinking off | ✓ (best-known config) |

---

### Block 3 — PixelRefer Mask Pre-filter

**Goal**: measure the trade-off between Block 3 verifier-call savings and final IoU.

| Variant | Status |
|---|---|
| Enabled, `top_k = 3` | ⟳ |
| Enabled, `top_k = 5` | ⟳ |
| Enabled, `top_k = 8` | ⟳ |
| Disabled | ✓ (best-known config) |

---

### Block 4 — Propagation Method

**Goal**: compare SAM 3 propagation strategies on temporal consistency and IoU.

| Variant | Description | Status |
|---|---|---|
| SAM 3 sequential | Standard sequential propagation | ✓ |
| SAM 3.1 point query multiplex | Point-query multiplexed conditioning | ⟳ |
| SAM 3.1 VG with conditioning | Visual grounding with explicit conditioning frames | ⟳ |

---

## Interpretability Analyses

---

### Oracle Runs

**Goal**: establish the ceiling of the method by injecting ground-truth object names into
the pipeline, bypassing any VLM description error.

| Run | Direction | Status |
|---|---|---|
| Pipeline with GT object names | ego→exo | ⟳ |
| Pipeline with GT object names | exo→ego | ⟳ |

**Gap decomposition**: a single summary table showing where the training-free pipeline
loses headroom relative to the oracle, decomposed by pipeline block.

---

### Source-Destination Symmetry Break

**Goal**: validate the v2 design decision to use two distinct frame selection stages.

The analysis compares:
- **Source selection**: optimised for generating the highest-quality object description
  (current Block 1 objective — object visibility and centrality)
- **Destination selection**: optimised for alignment between the destination frame content
  and the VLM-generated description (Block 2.5 grounding objective)

Status: ✓ (motivated v2 redesign; documented in preliminary findings)

---

### Quality Flow — Sankey Diagram

**Goal**: map how quality propagates through the pipeline stage by stage, identifying
where the most headroom is lost and which upstream conditions determine downstream outcomes.

**Construction**:
- The five columns of the diagram correspond to the five pipeline stages: B1 (source
  frame composite score), B2 (VLM description correctness, assessed by judge), B25
  (destination frame grounding confidence), B3 (seed mask IoU), B4 (final mean IoU)
- Results from intermediate ablation runs are pooled together across configurations to
  maximise sample size and study how quality moves from one block to the next at the
  aggregate level
- Each stage is divided into quality bands (e.g. B1: <0.051, 0.051–0.056, 0.056–0.081,
  ≥0.081; B4: 0, 0–0.5, 0.5–0.75, >0.75); flow width encodes case count
- **Group-by-group analysis**: a subset group is selected (e.g. upper quantile of B1)
  and the diagram traces how quality changes downstream from that starting condition,
  identifying which subsequent blocks preserve or degrade the initial quality advantage.
  This is repeated for lower quantiles and for B2 correctness conditions to isolate
  the causal contribution of each block.

Status: ⟳ (to be run on final test set results)

See `/assets/figures/sankey-draft.png` for a working example of the diagram structure.

---

### GDINO Confidence vs. IoU Correlation

**Goal**: validate that Grounding DINO confidence (`gdino_max_score`, logged per case in
`run_index.jsonl`) is a meaningful proxy for downstream segmentation quality.

Analysis: scatter plot of `gdino_max_score` vs. `mean_iou` per case, with correlation
coefficient. If the relationship is strong, it supports using GDINO confidence as a
quality signal for destination frame selection.

Status: ⟳

---

### Relationships Between Pipeline Components

**Goal**: quantify the strength of the causal chain from upstream block outputs to final IoU.

Three pairwise relationships to analyse:

| Relationship | X | Y | Status |
|---|---|---|---|
| Description quality → destination frame quality | B2 judge score | B25 composite score of selected frames | ⟳ |
| Destination frame quality → seed IoU | B25 composite score | B3 seed IoU | ⟳ |
| Seed IoU → propagation IoU | B3 seed IoU | B4 mean IoU | ⟳ |

Together these three plots characterise the full quality chain and identify the weakest
link in the pipeline.

---

### LM-EEC Comparison

#### Quantitative Analysis
| Analysis | Status |
|---|---|
| IoU distribution comparison (our pipeline vs. LM-EEC) | ⟳ |
| IoU comparison across key dimensions: scenario, object size, seed IoU | ⟳ |

#### Qualitative Analysis
Four-quadrant inspection:

| Quadrant | Status |
|---|---|
| Both succeed | ⟳ |
| Both fail | ⟳ |
| Ours succeeds, LM-EEC fails | ⟳ |
| LM-EEC succeeds, ours fails | ⟳ |

#### Failure Mode Taxonomy
Manual inspection of ~30 failure cases from our pipeline, categorised by root cause:

| Failure Mode | Description | Status |
|---|---|---|
| VLM hallucination | Wrong object name or colour in Block 2 description | ⟳ |
| GDINO localisation failure | Grounding DINO fails to detect object in any destination frame | ⟳ |
| SAM 3 agent abort | Agent loops or aborts; no mask produced in Block 3 | ⟳ |
| Block 4 propagation drift | Correct seed mask at B3, wrong mask at subsequent frames | ⟳ |
| Irreducible occlusion | Object fundamentally invisible in destination view | ⟳ |

---

## Key Claims This Thesis Makes

These are the empirical arguments the experiments chapter is built to support.
Each claim has a corresponding ablation or analysis designed to confirm or qualify it.

1. **Language is a viable view bridge.** A training-free pipeline using natural language
   as the only cross-view signal achieves competitive performance against learned
   cross-view feature matching methods on the Ego-Exo4D object correspondence task.

2. **Anchor (destination) frame selection is the highest-leverage stage.** Anchor quality
   is the single strongest predictor of pipeline success: when seed-frame IoU is high the
   gap to LM-EEC nearly closes, and when it is low the pipeline collapses. Investing the
   frame budget in anchor selection (Block 3a) yields larger IoU gains than equivalent
   investment in source-view selection or description quality.

3. **Principled anchor selection beats random, and language and geometry are interchangeable
   signals for it.** Both Grounding DINO (language-grounded) and RoMa (geometry-based,
   language-free) outperform random anchor selection and reach comparable IoU on the full
   test set, so the paper reports both. The improvement comes from principled selection, not
   merely from the multi-frame budget.

4. **Foundation model composition introduces characteristic failure modes.** The failure
   taxonomy and quality flow analysis identify VLM hallucination and GDINO localisation
   failure as the dominant upstream causes of downstream IoU degradation, with Block 4
   propagation drift a secondary cause conditional on a correct seed.

5. **The pipeline is interpretable by construction.** The oracle gap decomposition
   quantifies exactly how much headroom each block leaves on the table, enabling precise
   diagnosis of failure in a way that learned end-to-end systems cannot provide.