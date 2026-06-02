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
into four blocks that play three conceptual roles:

- **Block 1 — Source Frame Selection** (optimisation): scores all source-view annotated
  frames by a composite function and selects the single highest-quality frame to feed the
  VLM, avoiding redundant calls and maximising description quality.

- **Block 2 — Object Description** (semantic bridging): a multimodal LLM (Qwen 3.5 35B)
  generates a structured JSON description of the target object from the seed frame. The
  description contains three fields: `colour`, `object` (canonical noun phrase), and
  `view_independent_descriptors`. The description is explicitly view- and time-independent
  so it remains valid across cameras and across the take.

- **Block 2.5 — Destination Grounding** (semantic bridging): Grounding DINO scores every
  destination-view frame against the VLM-generated noun phrase and returns the top-K frames
  by detection confidence. This replaces v1's temporal-proximity selection and is the
  central addition of v2.

- **Block 3 — Agentic SAM 3 Segmentation** (semantic bridging): a ReAct-style agentic loop
  uses Qwen 3.5 35B as both planner and verifier to produce candidate masks on each of the
  K destination frames, with SAM 3 as the segmentation backbone.

- **Block 4 — Bi-directional Mask Propagation** (completion): SAM 3's video tracker
  propagates the surviving seed masks forward and backward through all destination-view
  frames, using a nearest-seed assignment to minimise temporal drift.

All foundation models are frozen at inference time. The pipeline output is a per-frame
mask track plus a JSON of frame-level predictions.

**Best-known configuration (exo→ego):** Qwen 3.5 35B as descriptor and agentic planner,
`n_source_frames = 1`, `diversity_mode = gdino` with `colour_object` query, `K = 3`
destination frames, 8 agent generations per frame, PixelRefer mask pre-filter disabled.

---

## Dataset & Evaluation Setup

- **Dataset**: Ego-Exo4D
- **Evaluation directions**: ego→exo and exo→ego (all ablations run in both unless noted)
- **Test set**: 191-pair manifest (full scale); 50-pair curated set used for v1 phase 1
- **Primary metrics**:
  - **IoU** (Jaccard Index): intersection over union of predicted and ground-truth masks;
    primary measure of segmentation quality
  - **LE** (Location Error): normalised centroid distance; measures localisation accuracy
    independently of shape
  - **CA** (Contour Accuracy): IoU after centroid-aligning prediction to ground truth;
    isolates shape fidelity from positional offset
  - **VA** (Visibility Accuracy): balanced accuracy (TPR + TNR) / 2 for frame-level
    presence/absence prediction; preferred over raw accuracy when visible/occluded frames
    are imbalanced
- **Secondary metrics**: inference time (wall-clock per take), VLM-as-judge score
- **Comparison targets**: LM-EEC (SOTA baseline — see below), challenge leaderboard entries
  ObjectRelator and V2-SAM

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
- **ObjectRelator** (Fu 2025): extends PSALM with a Multimodal Condition Fusion module and
  an SSL-based cross-view alignment module.
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

| Run | Direction | Status |
|---|---|---|
| Pipeline v2 on test set | ego→exo | ⟳ |
| Pipeline v2 on test set | exo→ego | ⟳ |

Results to be presented in a table against LM-EEC, ObjectRelator, and V2-SAM.

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
| Qwen 3.5 35B no thinking | ✓ (best-known config) |
| Qwen 3.6 35B no thinking | ⟳ |
| Qwen 3.5 35B batching | ✓ (v1; +16.5% IoU at full scale) |
| Qwen 3.5 35B thinking | ? |
| Qwen 3.6 35B thinking | ? |

Comparison on VLM-as-judge output score and inference time.

---

### Block 2 — VLM Description Content

**Goal**: isolate the contribution of each description field to downstream IoU.

| Variant | Description | Status |
|---|---|---|
| Object only | `object` field only | ⟳ |
| Object + colour | `colour` + `object` fields | ⟳ |
| Object + colour + view-independent descriptors | All three fields (current config) | ✓ |

---

### Block 2.5 — Destination Frame Selection (Diversity Mode)

**Goal**: isolate the contribution of language-driven grounding vs. temporal proximity vs.
random selection for destination frame choice.

| Variant | Description | Status |
|---|---|---|
| `off` (K=1) | Single destination frame by temporal proximity to source seed; v1 behaviour | ✓ |
| `dino` | Two additional frames B, C selected by maximising DINOv2-S CLS-token cosine distance from time-aligned frame A; no language signal | ⟳ |
| `dino_band` (0.90) | B, C sampled at 90th percentile band of DINOv2 similarity to A | ⟳ |
| `dino_band` (0.80) | B, C sampled at 80th percentile band of DINOv2 similarity to A | ⟳ |
| `MaxInfo` | Diversity-maximising selection from v1 evaluation | ✓ (v1; −3.7% IoU) |
| `dino_random` | K frames selected uniformly at random; control condition isolating grounding signal from multi-frame benefit | ⟳ |
| `gdino` | Grounding DINO scores all destination frames against VLM noun phrase; top-K by confidence forwarded to Block 3 | ✓ (best-known config) |

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

2. **Destination frame selection dominates source frame selection.** Investing the frame
   budget in grounding-based destination selection (Block 2.5) yields larger IoU gains
   than equivalent investment in source-view diversity or description quality, as
   evidenced by the v1→v2 ablation programme.

3. **Open-set grounding outperforms temporal proximity and visual diversity for
   destination frame selection.** The `gdino` diversity mode consistently outperforms
   `off`, `dino`, `dino_band`, and `gdino_random` across both directions, confirming
   that the language signal — not merely the multi-frame budget — drives the improvement.

4. **Foundation model composition introduces characteristic failure modes.** The failure
   taxonomy and quality flow analysis identify VLM hallucination and GDINO localisation
   failure as the dominant upstream causes of downstream IoU degradation, with Block 4
   propagation drift a secondary cause conditional on a correct seed.

5. **The pipeline is interpretable by construction.** The oracle gap decomposition
   quantifies exactly how much headroom each block leaves on the table, enabling precise
   diagnosis of failure in a way that learned end-to-end systems cannot provide.