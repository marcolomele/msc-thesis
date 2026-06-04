# Thesis ML Foundations Guide
## `src_pipeline` Technical Methods → MSc Theory Coverage

Organised to mirror the final thesis chapter structure.
Each section lists: theory to cover, key papers to cite, and the pipeline link.

---

## Chapter 2a — Visual Representation
*Transformer → Vision Transformer → Self-Supervised Learning with DINOv2*

### 2a.1 Transformer Architecture
**Theory:** Multi-head self-attention (query/key/value projections, scaled dot-product
attention); positional encodings (sinusoidal and learned); residual connections +
layer norm; encoder-only vs encoder-decoder stacks; computational complexity O(n²)
in sequence length.

**Key paper:**
- Vaswani, A. et al. *Attention Is All You Need*. NeurIPS 2017.

### 2a.2 Vision Transformer (ViT)
**Theory:** Image → non-overlapping patch tokens (16×16 default); linear projection
to embedding dimension; prepended [CLS] token as global image summary; learned
1D positional embeddings; standard Transformer encoder on top; pre-training via
supervised classification on JFT-300M/ImageNet-21k; how patch count scales with
image resolution.

**Key papers:**
- Dosovitskiy, A. et al. *An Image is Worth 16×16 Words: Transformers for Image
  Recognition at Scale*. ICLR 2021.
- Touvron, H. et al. *Training Data-Efficient Image Transformers & Distillation
  Through Attention* (DeiT). ICML 2021.
  *(useful for showing ViT can be trained without JFT-scale data)*

### 2a.3 Self-Supervised Learning with DINO and DINOv2
**Theory:**
- **Self-supervised learning motivation:** labels are scarce; pretext tasks
  (rotation prediction, contrastive learning, masked autoencoders) as alternatives.
- **DINO:** self-distillation with no labels; student/teacher with exponential
  moving average weights; centering and sharpening to prevent collapse; multi-crop
  augmentation strategy; emergence of semantic segmentation structure in CLS and
  patch tokens without any segmentation supervision.
- **DINOv2:** scaled-up curated dataset (LVD-142M); register tokens to absorb
  artefacts in patch features; improved patch-level and image-level feature
  quality; shows strong off-the-shelf linear probe and k-NN performance.
- **CLS token as global descriptor:** why [CLS] aggregates scene-level semantics;
  L2 normalisation → cosine similarity; use in retrieval and re-ranking.

**Key papers:**
- Caron, M. et al. *Emerging Properties in Self-Supervised Vision Transformers*
  (DINO). ICCV 2021.
- Oquab, M. et al. *DINOv2: Learning Robust Visual Features without Supervision*.
  TMLR 2024.
- He, K. et al. *Masked Autoencoders Are Scalable Vision Learners* (MAE).
  CVPR 2022. *(SAM's image encoder is MAE-pretrained — worth a paragraph)*

### Pipeline link
DINOv2 features underpin the broader cross-view setting (O-MaMa selects masks in a learned
DINOv2 latent space; DINO patch features give label-free semantic correspondence — §2b.0).
In the current pipeline, anchor (destination) frame selection is handled by Grounding DINO
or RoMa rather than DINOv2 CLS-token diversity; the DINOv2 cosine-distance diversity scheme
was a v1 design retained here for theoretical grounding and as the `MaxInfo`/diversity
ablation baseline (CLIP-embedding variant, Experiment D.2).

---

## Chapter 2b — Correspondence & Segmentation
*Feature matching & dense correspondence (RoMa) → previous segmentation approaches → SAM → SAM2*

### 2b.0 Feature Matching & Dense Correspondence
**Theory:**
- **Handcrafted local descriptors:** SIFT, SURF, ORB — keypoint detection + descriptor
  matching; robustness limits under large viewpoint and illumination change.
- **Learned sparse matching:** SuperPoint (trained keypoint detector/descriptor) +
  SuperGlue (graph-neural-network matcher with attention).
- **Detector-free dense matching:** LoFTR and RoMa bypass keypoint detection and produce
  dense correspondences directly. **RoMa** learns a robust warp field between an image pair
  of a 3D scene, achieving state-of-the-art dense feature matching under wide baseline
  changes; each match carries a confidence score.
- **Semantic correspondence without geometry:** DINO/DINOv2 patch features establish
  correspondences between semantically similar images without geometric priors. Ego-exo
  correspondence is an extreme case of this setting, stressing all of the above.

**Key papers:**
- Lowe, D. *Distinctive Image Features from Scale-Invariant Keypoints* (SIFT). IJCV 2004.
- Bay, H. et al. *SURF: Speeded Up Robust Features*. ECCV 2006.
- Rublee, E. et al. *ORB: An Efficient Alternative to SIFT or SURF*. ICCV 2011.
- DeTone, D. et al. *SuperPoint*. CVPRW 2018.
- Sarlin, P.-E. et al. *SuperGlue*. CVPR 2020.
- Sun, J. et al. *LoFTR: Detector-Free Local Feature Matching with Transformers*. CVPR 2021.
- Edstedt, J. et al. *RoMa: Robust Dense Feature Matching*. CVPR 2024.

### Pipeline link
RoMa provides the **geometry-based, language-free anchor-selection** signal in Block 3
(Experiment E): the source object mask is matched against every destination frame, and
frames are ranked by the count of confident matches (confidence > 0.5) whose source
endpoint falls inside the object mask. It is the alternative to Grounding DINO and reaches
comparable full-test-set IoU.

### 2b.1 Segmentation Landscape (brief survey)
**Theory:** Semantic vs instance vs panoptic segmentation; fully convolutional
networks; encoder-decoder skip connections; region proposal networks;
interactive/click-based segmentation; the "universal segmenter" motivation.

**Key papers (representative milestones — cite selectively):**
- Long, J. et al. *Fully Convolutional Networks for Semantic Segmentation*
  (FCN). CVPR 2015.
- Ronneberger, O. et al. *U-Net: Convolutional Networks for Biomedical Image
  Segmentation*. MICCAI 2015.
- He, K. et al. *Mask R-CNN*. ICCV 2017.
- Chen, L.-C. et al. *Encoder-Decoder with Atrous Separable Convolution for
  Semantic Image Segmentation* (DeepLabv3+). ECCV 2018.
- Sofiiuk, K. et al. *Reviving Iterative Training with Mask Guidance for
  Interactive Segmentation* (RITM). ICIP 2022. *(click-based; direct ancestor
  of SAM's interactive paradigm)*

### 2b.2 SAM — Segment Anything Model
**Theory:**
- **Three-component design:** (1) MAE-pretrained ViT image encoder; (2) prompt
  encoder — sparse inputs (points, boxes, coarse masks) projected via positional +
  learned embeddings, text via CLIP-style encoding; (3) lightweight two-way
  Transformer mask decoder with IoU-score head.
- **Ambiguity handling:** three mask outputs per prompt; model selects based on
  predicted IoU score.
- **SA-1B dataset:** model-in-the-loop data engine; 1.1B masks, 11M images;
  why scale matters for zero-shot generalisation.
- **Zero-shot transfer:** evaluated on 23 downstream tasks without fine-tuning.

**Key paper:**
- Kirillov, A. et al. *Segment Anything*. ICCV 2023.

### 2b.3 SAM2 — Extending to Video
**Theory:**
- **Streaming memory architecture:** per-frame image encoder + lightweight
  memory encoder that writes selected past frames into a memory bank; memory
  attention module reads the bank with cross-attention to condition current
  predictions; avoids re-encoding all frames.
- **Occlusion head:** explicit binary output for "object not visible"; prevents
  the model from hallucinating a mask when the object is absent.
- **Multi-object support and conditioning frame selection.**
- **SA-V dataset:** 50.9K videos with 642.6K masklets.
- SAM2 as joint image+video model (image inference = single-frame video).

**Key paper:**
- Ravi, N. et al. *SAM 2: Segment Anything in Images and Videos*. arXiv 2024.
  *(SAM3 is the text-prompted extension used in the pipeline; cover SAM2 as the
  base architecture and note text-prompt conditioning as the SAM3 addition)*

### Pipeline link
`sam3_agent.py` invokes `build_sam3_image_model` for per-frame text-to-mask
proposals inside the agent loop; `propagation.py` uses `build_sam3_video_model`
for Block 4 bi-directional propagation.

---

## Chapter 2c — Language-Grounding
*Multimodality → GroundingDINO → SAM3 Promptable Concept Segmentation → PixelRefer*

### 2c.1 Multimodality: Aligning Vision and Language
**Theory:**
- **Contrastive image-text pre-training (CLIP):** dual encoder (image ViT +
  text Transformer); InfoNCE contrastive loss over large batches of (image, caption)
  pairs; zero-shot classification via text template matching; emergent open-vocabulary
  understanding.
- **Limitations of CLIP for grounding:** image-level alignment only; no spatial
  localisation; motivates region-level grounding models.
- **Visual instruction tuning:** LLaVA paradigm — freeze CLIP encoder, train
  lightweight projector, fine-tune LLM on visual instruction data; enables
  image-as-input to any LLM.

**Key papers:**
- Radford, A. et al. *Learning Transferable Visual Models From Natural Language
  Supervision* (CLIP). ICML 2021.
- Liu, H. et al. *Visual Instruction Tuning* (LLaVA). NeurIPS 2023.
- Alayrac, J.-B. et al. *Flamingo: a Visual Language Model for Few-Shot Learning*.
  NeurIPS 2022. *(optional; useful for multi-image interleaving concept)*

### 2c.2 GroundingDINO — Open-Set Object Detection
**Theory:**
- **DETR background:** transformer-based detector; bipartite Hungarian matching;
  learnable object queries; eliminates anchors and NMS.
- **DINO detector** (precursor): contrastive denoising training; mixed query
  selection; improved convergence and accuracy.
- **GroundingDINO:** dual backbone (Swin image encoder + BERT text encoder);
  feature enhancer with bidirectional cross-attention between modalities; language-
  guided query selection; cross-modal decoder outputs region-phrase alignments;
  text-conditioned confidence score per region.
- **Open-vocabulary vs closed-set detection:** why text conditioning generalises to
  arbitrary noun phrases without class-specific heads.
- **Detection threshold trade-offs:** recall-biased low threshold (0.05 in the
  pipeline) for frame-scoring use vs precision-biased higher threshold for strict
  detection.

**Key papers:**
- Carion, N. et al. *End-to-End Object Detection with Transformers* (DETR).
  ECCV 2020.
- Zhang, H. et al. *DINO: DETR with Improved DeNoising Anchor Boxes for
  End-to-End Object Detection*. ICLR 2023.
- Liu, S. et al. *Grounding DINO: Marrying DINO with Grounded Pre-Training for
  Open-Set Object Detection*. ECCV 2024.

### 2c.3 SAM3 — Promptable Concept Segmentation
**Theory:**
- **Promptable Concept Segmentation (PCS):** SAM 3's central task — detect, segment, and
  track *all* instances of a visual concept specified by a noun phrase, image exemplars, or
  both. Generalises beyond points/boxes to semantic queries; the grounding challenge
  (phrase → spatial region) and how it differs from classic referring expression
  comprehension.
- **Agentic interface:** to overcome the limits of short noun-phrase prompts, SAM 3 exposes
  an interface in which a multimodal LLM acts as a planner that iteratively invokes SAM 3,
  inspects the resulting masks, and refines its queries. This loop handles complex relational
  expressions ("left-most", "near the") that matter for cluttered scenes — and is exactly the
  loop the pipeline's Block 3 builds on.
- **Tracker inherits SAM 2:** the module that propagates masks through a video reuses the
  SAM 2 architecture (§2b.3 / §2e); Hu et al. (LM-EEC) validate SAM 2's tracker for video
  cross-view correspondence.
- **Why canonical noun phrases matter:** short discriminative phrases ("blue ceramic mug")
  outperform verbose descriptions in text-to-mask accuracy — connects directly to the
  pipeline's VLM prompt design (the agent simplifies the rich description into a short
  prompt) and the PixelRefer-Lite finding.

**Key paper:**
- Ravi, N. et al. *SAM 3: Segment Anything with Concepts*. 2025.
  *(distinct citation from SAM 2; cover PCS and the agentic interface as the SAM 3
  contributions on top of the SAM 2 base architecture)*

### 2c.4 PixelRefer — Mask-Conditioned Region Description
**Theory:**
- **Referring expression generation (REG) vs comprehension (REC):** inverse
  tasks; REG given a mask must produce a *discriminative* expression (identifies
  the region uniquely), harder than captioning.
- **PixelRefer architecture:** Qwen2-VL backbone; mask tensor (1×H×W uint8)
  injected as an additional input channel alongside the image; trained to
  generate region-grounded text conditioned on the pixel-level mask.
- **Sentence-BERT for re-ranking** (`all-MiniLM-L6-v2`): bi-encoder architecture;
  mean pooling over token embeddings; cosine similarity between source description
  and candidate descriptions as a filtering signal.
- **MiniLM knowledge distillation** (Wang et al., 2020): teacher-student attention
  transfer; why a 22M-parameter model retains strong semantic similarity signal.
- **Repetition penalty in autoregressive decoding:** modifying logit distribution
  to discount already-generated tokens; prevents degenerate short-text loops.

**Key papers:**
- Chen, J. et al. *PixelRefer: Pixelwise Referring Image Segmentation*.
  Alibaba DAMO Academy, 2024. *(check for final venue/arxiv ID)*
- Reimers, N. & Gurevych, I. *Sentence-BERT: Sentence Embeddings using Siamese
  BERT-Networks*. EMNLP 2019.
- Wang, W. et al. *MiniLM: Deep Self-Attention Distillation for Task-Agnostic
  Compression of Pre-Trained Transformers*. NeurIPS 2020.

### Pipeline link
`gdino.py` scores destination frames by GDINO detection confidence for the object
phrase; optionally overlays bounding-box hints. `vlm.py` runs PixelRefer for
mask-to-description inference; `sam3_agent.py` uses MiniLM cosine similarity
to pre-filter SAM3 candidate masks before the expensive MLLM verification step.

---

## Chapter 2d — Foundation Models & Agents
*Qwen Family → ReAct → Tool Use*

### 2d.1 Large Language Models — Foundations
**Theory:**
- **Autoregressive pre-training:** next-token prediction; cross-entropy loss;
  causal self-attention mask; tokenisation (BPE/SentencePiece).
- **Scaling laws** (Kaplan et al., 2020; Hoffmann et al., 2022): power-law
  relationships between loss, parameters, data, compute; Chinchilla-optimal
  training; why 35B at inference time is a practical frontier.
- **Instruction fine-tuning:** SFT on (instruction, response) pairs; how it
  converts a next-token predictor into an instruction follower; why it produces
  reliable structured JSON outputs.
- **RLHF / DPO:** preference alignment; connection to JSON-mode reliability and
  reduced hallucination in tool calls.
- **KV-cache and context windows:** caching past keys/values to avoid quadratic
  re-computation; why a 131 072-token context is needed for multi-turn agent loops;
  memory cost of long contexts.

**Key papers:**
- Kaplan, J. et al. *Scaling Laws for Neural Language Models*. arXiv 2020.
- Hoffmann, J. et al. *Training Compute-Optimal Large Language Models*
  (Chinchilla). NeurIPS 2022.
- Ouyang, L. et al. *Training Language Models to Follow Instructions with Human
  Feedback* (InstructGPT / RLHF). NeurIPS 2022.
- Rafailov, R. et al. *Direct Preference Optimization: Your Language Model is
  Secretly a Reward Model* (DPO). NeurIPS 2023.

### 2d.2 Qwen Family — Multimodal Foundation Models
**Important Note:** Final Qwen backbone used in pipeline is Qwen 3.5 35B.

**Theory:**
- **Qwen2 architecture:** grouped-query attention, RoPE positional embeddings,
  RMSNorm, SwiGLU activations; 7B–72B range.
- **Qwen2-VL multimodal extension:** dynamic resolution input via Naive Dynamic
  Resolution; Vision Transformer with 2D-RoPE; multi-image and video inputs;
  visual token compression with MLP projector.
- **Qwen3.5 / reasoning models:** extended thinking (chain-of-thought at inference
  time); configurable `agent_thinking` flag in the pipeline.

**Key papers:**
- Bai, J. et al. *Qwen Technical Report*. arXiv 2023.
- Hui, B. et al. *Qwen2.5-Coder Technical Report*. arXiv 2024.
  *(cite the Qwen2-VL and Qwen2.5 reports most relevant to your exact model version)*
- Wang, P. et al. *Qwen2-VL: Enhancing Vision-Language Model's Perception of the
  World at Any Resolution*. arXiv 2024.
- @misc{yang2025qwen3technicalreport,
      title={Qwen3 Technical Report}, 
      author={An Yang and Anfeng Li and Baosong Yang and Beichen Zhang and Binyuan Hui and Bo Zheng and Bowen Yu and Chang Gao and Chengen Huang and Chenxu Lv and Chujie Zheng and Dayiheng Liu and Fan Zhou and Fei Huang and Feng Hu and Hao Ge and Haoran Wei and Huan Lin and Jialong Tang and Jian Yang and Jianhong Tu and Jianwei Zhang and Jianxin Yang and Jiaxi Yang and Jing Zhou and Jingren Zhou and Junyang Lin and Kai Dang and Keqin Bao and Kexin Yang and Le Yu and Lianghao Deng and Mei Li and Mingfeng Xue and Mingze Li and Pei Zhang and Peng Wang and Qin Zhu and Rui Men and Ruize Gao and Shixuan Liu and Shuang Luo and Tianhao Li and Tianyi Tang and Wenbiao Yin and Xingzhang Ren and Xinyu Wang and Xinyu Zhang and Xuancheng Ren and Yang Fan and Yang Su and Yichang Zhang and Yinger Zhang and Yu Wan and Yuqiong Liu and Zekun Wang and Zeyu Cui and Zhenru Zhang and Zhipeng Zhou and Zihan Qiu},
      year={2025},
      eprint={2505.09388},
      archivePrefix={arXiv},
      primaryClass={cs.CL},
      url={https://arxiv.org/abs/2505.09388}, 
}

### 2d.3 Agentic LLM Systems — ReAct and Tool Use
**Theory:**
- **ReAct (Reasoning + Acting):** interleaved Thought → Action → Observation
  traces; why explicit reasoning steps before each action improve correctness and
  interpretability vs pure action chains.
- **Tool-use / function-calling:** JSON schema as a structured action space; how
  the model selects and parameterises tools; parse-execute-observe cycle; why
  strongly typed schemas reduce argument hallucination.
- **Toolformer paradigm:** self-supervised discovery of when and how to call
  external APIs; contrasted with the supervised tool schema used in the pipeline.
- **Multi-turn agent design:** system prompt as persistent instruction; memory via
  conversation history; message pruning strategies (keep system + N anchor messages
  + latest tool call) to prevent context overflow without losing grounding.
- **Failure mode taxonomy:** repeated actions, hallucinated tool arguments,
  reward hacking (selecting trivially easy prompts); recovery strategies (duplicate
  rejection, consecutive-rejection abort, graceful degradation to empty mask).

**Key papers:**
- Yao, S. et al. *ReAct: Synergizing Reasoning and Acting in Language Models*.
  ICLR 2023.
- Schick, T. et al. *Toolformer: Language Models Can Teach Themselves to Use Tools*.
  NeurIPS 2023.
- Wei, J. et al. *Chain-of-Thought Prompting Elicits Reasoning in Large Language
  Models*. NeurIPS 2022. *(foundational context for why reasoning traces help)*
- Significant-Gravitas. *AutoGPT*. GitHub 2023.
  *(cite as a landmark practical agentic system; not a peer-reviewed paper)*

### Pipeline link
`agent_core.py` implements a ReAct-style loop: the LLM (Qwen3.5-35B via Ollama)
receives an image + system prompt, emits a `segment_phrase` tool call, receives
SAM3 masks, calls `examine_each_mask` for MLLM verification, and iterates up to
15 generations per frame with history pruning and duplicate-prompt rejection.

---

## Chapter 2e — Addition of Time
*Video Object Segmentation → SAM2/SAM3 Propagation*

### 2e.1 Video Object Segmentation (VOS)
**Theory:**
- **Task definition:** track and segment a target object across all frames of a
  video given a segmentation mask (semi-supervised) or no prior (unsupervised).
- **Early approaches:** OSVOS (fine-tune per video at test time); OnAVOS
  (online adaptation); limitations of per-video optimisation at inference cost.
- **Propagation-based methods:** matching-based tracking using feature similarity
  (FEELVOS, CFBI).
- **Memory-based VOS (STM paradigm):** space-time memory network; encode past
  (frame, mask) pairs into a memory bank; retrieve matching features via soft
  nearest-neighbour attention; XMem extends to long videos with hierarchical
  memory management.
- **Why memory-based methods transfer well:** no per-video fine-tuning; generalise
  to new categories; scale with the quality of the pre-trained feature space.

**Key papers:**
- Caelles, S. et al. *One-Shot Video Object Segmentation* (OSVOS). CVPR 2017.
- Oh, S. W. et al. *Video Object Segmentation Using Space-Time Memory Networks*
  (STM). ICCV 2019.
- Cheng, H. K. & Schwing, A. G. *XMem: Long-Term Video Object Segmentation with
  an Atkinson-Shiffrin Memory Model*. ECCV 2022.
- Yang, Z. et al. *Decoupling Features in Hierarchical Propagation for Video
  Object Segmentation* (DEVA). ICCV 2023. *(optional; relevant for open-vocab VOS)*

### 2e.2 SAM2 for Video and Bi-Directional Propagation
**Theory:**
- **SAM2 memory architecture in detail** (expand from §2b.3): streaming inference;
  memory attention cross-attending to stored (frame, mask) pairs; occlusion head.
- **Bi-directional propagation strategy:** forward pass from earliest seed frame
  to end; backward pass from latest seed frame to start; for each output frame,
  select the propagation result from the nearest-seed direction to minimise
  accumulated temporal drift.
- **Multi-seed conditioning:** adding multiple annotated frames as memory entries;
  `max_cond_frames_in_attn=4` cap as a hard computational constraint.
- **Mask union at shared frames:** pixel-wise OR when forward and backward
  propagations both produce a non-empty mask at the same frame.
- **Temporal consistency vs per-frame accuracy trade-off:** why propagation
  produces temporally smoother masks than independent per-frame segmentation but
  accumulates drift over long occlusions.

**Key paper:**
- Ravi, N. et al. *SAM 2: Segment Anything in Images and Videos*. arXiv 2024.

### Pipeline link
`propagation.py` takes the seed masks from Block 3, copies frames to a temp
video directory, runs SAM3's video model in forward + backward passes from the
seed frames, then merges results using nearest-seed assignment. Evaluated by
IoU/LE/CA/VA against GT per-frame annotations.

---

## Cross-Cutting: Evaluation Metrics
*(Belongs in Chapter 5 setup — listed here for completeness)*

- **IoU (Jaccard Index):** |A ∩ B| / |A ∪ B|; sensitivity to object scale;
  relation to Dice.
- **Location Error (LE):** normalised centroid distance; separates localisation
  from shape accuracy.
- **Contour Accuracy (CA):** IoU after centroid-aligning prediction to GT;
  isolates shape fidelity from positional offset.
- **Visibility Accuracy (VA):** balanced accuracy (TPR + TNR) / 2 for
  frame-level presence/absence prediction. Used internally during development but
  **not reported in the paper** — the published tables report IoU, LE, and CA only.

---

## Mapping: Pipeline Block → Thesis Section

| Pipeline Block | Technique | Thesis Section |
|---|---|---|
| Block 1: source frame scoring | Area (0.99) + centrality (0.01) heuristic | §4 Pipeline |
| Block 2: VLM description | Qwen 3.5 35B multimodal inference | §2d |
| Block 2 (alt.): description | PixelRefer mask-conditioned | §2c |
| Block 3a: anchor selection (language) | Grounding DINO confidence top-K | §2c |
| Block 3a: anchor selection (geometry) | RoMa dense-match count top-K | §2b.0 |
| Block 3b: agent loop | ReAct tool-use, Qwen 3.5 35B | §2d |
| Block 3b: mask proposal | SAM 3 PCS text-to-mask | §2b / §2c |
| Block 3b: pre-filter (ablated) | MiniLM cosine re-ranking | §2c (PixelRefer) |
| Block 3b: MLLM verification | Qwen 3.5 35B multimodal reasoning | §2d |
| Block 4: propagation | SAM 2/SAM 3 bi-directional video | §2e |
| Evaluation | IoU (primary), LE, CA | §5 Experiments |

---

## Core Reference List (bibtex keys to collect)

**Key convention:** keys follow the paper's `references.bib` style — lowercase
`author + year + firstword` (e.g. `oquab2023dinov2`, `liu2024groundingdino`). The paper's
`references.bib` is the single source of truth for any work that already appears in the
paper; reuse those exact keys and never invent a parallel key (the paper currently carries a
duplicate, `fu2025objectrelator` vs. `fu2025objectrelatorenablingcrossviewobject` — use the
former and treat the long form as a bug to reconcile). Theory-only works below are not yet in
`references.bib` and will be added during drafting using the same convention.

**Already in the paper's `references.bib` (use verbatim):**
```
kirillov2023sam       Segment Anything (SAM)
ravi2024sam2          SAM 2
ravi2025sam3          SAM 3 — Segment Anything with Concepts (PCS + agent)
dinov1                DINO
oquab2023dinov2       DINOv2
dinov3                DINOv3
long2015fcn           FCN
he2017maskrcnn        Mask R-CNN
lowe2004sift          SIFT
bay2008surf           SURF
rublee2011orb         ORB
detone2018superpoint  SuperPoint
sarlin2020superglue   SuperGlue
sun2021loftr          LoFTR
roma                  RoMa — Robust Dense Feature Matching
liu2024groundingdino  Grounding DINO
lai2024lisa           LISA
rasheed2024glamm      GLaMM
psalm2024eccv         PSALM
yuan2025sa2va         Sa2VA
oh2019stm             Space-Time Memory Networks (STM)
cheng2022xmem         XMem
qwen35blog            Qwen 3.5 (model blog)
gemma4blog            Gemma 4 (judge; model blog)
```

**Theory-only — to add to `references.bib` during drafting (same convention):**
```
vaswani2017attention     Attention Is All You Need
dosovitskiy2021vit       ViT — An Image is Worth 16×16 Words
touvron2021deit          DeiT
he2022mae                MAE
ronneberger2015unet      U-Net
radford2021clip          CLIP
liu2023llava             Visual Instruction Tuning (LLaVA)
carion2020detr           DETR
zhang2023dino            DINO detector
chen2024pixelrefer       PixelRefer
reimers2019sentencebert  Sentence-BERT
wang2020minilm           MiniLM
kaplan2020scaling        Scaling Laws
hoffmann2022chinchilla   Chinchilla
ouyang2022instructgpt    InstructGPT / RLHF
rafailov2023dpo          DPO
bai2023qwen              Qwen Technical Report
wang2024qwen2vl          Qwen2-VL
yao2023react             ReAct
schick2023toolformer     Toolformer
wei2022cot               Chain-of-Thought
caelles2017osvos         OSVOS
```