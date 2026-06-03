# Chapter Brief: 2a — Visual Representation

---

**Notation discipline:** Use the notation contract in `theory-brief.md` §0 verbatim.
Honour the canonical/local rule. Do not reinvent symbols. **This sub-chapter is the
origin of the canonical sequence-model and vision symbols** ($\mm{X}$, $\query/\key/\val$,
$\attn$, $\patch$, $\cls$, $\feat$) — define them carefully here; all later chapters
reference these definitions.

You are helping me draft Subchapter 2a of my master's thesis. The project
knowledge files contain my style guide, scope, writing samples, and thesis
outline. Read them before drafting.

**Chapter**: 2a — Visual Representation  
**Target length**: ~4 pages  
**Arc**: B — Concept first (Transformer → ViT → DINOv2 self-supervision)  
**Deep treatment**: scaled dot-product attention equation; ViT patch embedding + CLS token; DINO self-distillation (student/teacher EMA, centering–sharpening, collapse avoidance)  
**Surface only**: MAE (one para — flag as SAM's encoder pretraining for 2b)  
**Skip**: full self-supervised taxonomy beyond the pretext-task motivation; positional-encoding variants beyond naming sinusoidal vs learned, DeiT (one para — ViT trainable without JFT-scale data).

**Connect back to**: nothing (first theory sub-chapter; defines the shared vocabulary)  
**Connect forward to**: 2b (SAM/MAE encoder is a ViT; DINO patch features give label-free correspondence — §2b.0), 2c (CLIP/Qwen image encoders are ViTs), 2d (Qwen-VL), 2e

**Local notation to harmonise a posteriori** (footnote originals on first use):
DINO/DINOv2 centering–sharpening operator, EMA teacher update, register tokens.

**[ADD] macros introduced here**: none new — uses existing canonical set.

**Theory content + bibtex keys**: paste from `project-guidelines/thesis-theory-topics.md`
§ "Chapter 2a — Visual Representation" (2a.1–2a.3). 

```
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
```

Keys: `vaswani2017attention`,
`dosovitskiy2021vit`, `touvron2021deit`, `dinov1`, `oquab2023dinov2`, `dinov3`,
`he2022mae`. Pipeline link paragraph: DINOv2 latent space underpins the cross-view
setting (O-MaMa, DINO semantic correspondence); note the v1 CLS-diversity scheme as
the `MaxInfo` ablation baseline.

Draft the full chapter now. Write in my voice as documented in the style guide.
Use LaTeX formatting throughout: display equations with \begin{equation},
inline math with $...$, citations with \cite{key}. After the draft, flag any
decisions you made that I should review.

---

## Post-Draft Checklist

- [ ] Canonical symbols defined exactly as in contract §0.1
- [ ] Voice matches style guide (read the Red Flags section)
- [ ] Every equation has a component-by-component explanation in prose
- [ ] Forward connections to 2b–2e present
- [ ] No concept explained that §2 depth table marks Skip
- [ ] Citations use inline name+year + \cite{key} format
- [ ] Nothing sounds like it was written by an AI