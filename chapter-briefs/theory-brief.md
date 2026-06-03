# Theory Chapter — Master Brief & Notation Contract

This file is the **spec** for Chapter 2 (Theory, ~15 pages). It is *not* drafted
into LaTeX. It fixes notation, the define-once dependency map, and the page
budget so the five sub-chapters (2a–2e), each drafted in a separate Cursor
session, speak one consistent language.

Every sub-brief must open with: *"Use the notation contract in `theory-brief.md`
§0 verbatim. Honour the canonical/local rule. Do not reinvent symbols."*

---

## §0 — Notation Contract

### The canonical/local rule (read first)

> A symbol is **canonical** — defined once, used verbatim everywhere — if it
> recurs across two or more subtopics. A symbol is **local** — adopt the source
> paper's own notation — if it belongs to a single backbone. When a paper's
> primitive clashes with a canonical symbol, rewrite the math to the canonical
> form and footnote the paper's original notation once, on first occurrence, so
> an expert can trace it back. Unify what recurs; preserve what is local.

Canonical symbols are seeded **a priori** below (they provably recur — see the
dependency map). Local machinery is harmonised **a posteriori**, per sub-brief,
at drafting time.

### §0.1 Canonical symbols (a priori — use verbatim across 2a–2e)

All macros below already live in `latex/style/custom-macros.tex` unless marked
**[ADD]**, which means: add the macro before first use and keep the comment.

**Tensors, sets, spaces**


| Object                   | Symbol                     | Macro               |
| ------------------------ | -------------------------- | ------------------- |
| Vector (bold lowercase)  | $\vv{x}$                   | `\vv{x}`            |
| Matrix (bold uppercase)  | $\mm{A}$                   | `\mm{A}`            |
| Calligraphic set / space | $\set{X}$                  | `\set{X}`           |
| Reals                    | $\R$                       | `\R`                |
| Transpose                | $\mm{A}^\top$              | `\transpose`        |
| Norm / abs               | $\norm{\vv{x}}$, $\abs{a}$ | `\norm{}`, `\abs{}` |


**Sequence model (Transformer core — defined in 2a, referenced everywhere)**


| Object                       | Symbol                          | Macro                    |
| ---------------------------- | ------------------------------- | ------------------------ |
| Input token sequence         | $\mm{X}\in\R^{n\times \dmodel}$ | `\mm{X}`                 |
| Sequence length              | $n$                             | —                        |
| Model / embedding dim        | $\dmodel$                       | `\dmodel`                |
| Query / Key / Value          | $\query,\key,\val$              | `\query \key \val`       |
| Key dim                      | $\dkey$                         | `\dkey`                  |
| Head dim                     | $\dhead$                        | `\dhead`                 |
| Scaled dot-product attention | $\attn(\query,\key,\val)$       | `\attn`                  |
| Multi-head attention         | $\mhattn(\cdot)$                | `\mhattn`                |
| Softmax / LayerNorm          | $\softmax$, $\layernorm$        | `\softmax`, `\layernorm` |


**Vision (defined in 2a, referenced by 2b–2e)**


| Object                 | Symbol      | Macro                                 |
| ---------------------- | ----------- | ------------------------------------- |
| Image                  | $\img$      | `\img`                                |
| Patch token            | $\patch_i$  | `\patch`                              |
| CLS / global token     | $\cls$      | `\cls`                                |
| Generic feature vector | $\feat$     | `\feat`                               |
| Patch grid size        | $h\times w$ | **[ADD]** none needed (raw)           |
| Number of patches      | $N_p$       | **[ADD]** `\newcommand{\npatch}{N_p}` |


**Segmentation & matching (defined in 2b, referenced by 2c–2e)**


| Object                            | Symbol                    | Macro                                                                     |
| --------------------------------- | ------------------------- | ------------------------------------------------------------------------- |
| Segmentation mask                 | $\mask$                   | `\mask`                                                                   |
| IoU                               | $\iou$                    | `\iou`                                                                    |
| Cosine similarity                 | $\cossim{\vv{a}}{\vv{b}}$ | **[ADD]** `\newcommand{\cossim}[2]{\langle #1, #2\rangle}`                |
| Dense correspondence / warp field | $\mathbf{W}$              | **[ADD]** `\newcommand{\warp}{\mm{W}}` (canonical for RoMa→2b, reused 2e) |
| Match confidence                  | $c\in[0,1]$               | — (raw)                                                                   |


**Language & multimodal (defined in 2c/2d)**


| Object                         | Symbol                       | Macro                                          |
| ------------------------------ | ---------------------------- | ---------------------------------------------- |
| Text / noun-phrase prompt      | $t$                          | — (raw)                                        |
| Token vocabulary               | $\set{V}$                    | `\set{V}`                                      |
| Text encoder / image encoder   | $g_\text{txt}, g_\text{img}$ | **[ADD]** as written, raw subscripts           |
| Autoregressive LM distribution | $p_\theta(x_i\mid x_{<i})$   | — (raw)                                        |
| Time / frame index (video)     | $\tau$                       | **[ADD]** reserve $\tau$ for frames, never $t$ |


**Reserved-symbol discipline** (prevents the most common clashes):

- $t$ = text prompt only. Frame/time index is **$\tau$**. Never overload.
- $n$ = sequence length. Patch count is **$N_p$**, frame count is **$T$**.
- $d$ alone is ambiguous — always subscript ($\dmodel,\dkey,\dhead$).
- $\mm{M}$ = mask. Memory bank (SAM2/STM) is **$\set{M}$** (calligraphic).

### §0.2 Local notation (a posteriori — adopt the paper's own)

Do **not** pre-define these; pull them from the source paper at drafting time and
align only their shared primitives to §0.1. Footnote the original on first use.

- **2a** DINO/DINOv2: centering–sharpening operator, EMA teacher update, register tokens.
- **2b** RoMa: the warp/match decoder internals beyond the canonical $\warp$; SAM IoU head; SA-1B engine notation.
- **2c** CLIP InfoNCE temperature; GroundingDINO query-selection indices; PixelRefer mask-channel injection.
- **2d** Qwen GQA/RoPE/RMSNorm specifics; ReAct thought/action/observation trace symbols.
- **2e** STM/XMem memory read/write; SAM2 occlusion-head output.

---

## §1 — Define-Once Dependency Map

The second consistency lever: each concept is *defined* in exactly one place and
*referenced* thereafter. Sub-briefs state this in "Connect back to".


| Concept                                            | Defined in   | Referenced by                                               |
| -------------------------------------------------- | ------------ | ----------------------------------------------------------- |
| Transformer, attention, MHA, positional encoding   | **2a.1**     | 2b (SAM decoder), 2c (GDINO, CLIP), 2d (Qwen, ReAct), 2e    |
| ViT, patch/CLS tokens, image→token                 | **2a.2**     | 2b (SAM/MAE encoder), 2c (CLIP image encoder), 2d (Qwen-VL) |
| Self-supervised features, DINOv2, cosine retrieval | **2a.3**     | 2b.0 (semantic correspondence), 2c                          |
| Dense correspondence, warp field $\warp$           | **2b.0**     | 2e (propagation drift framing)                              |
| Mask $\mask$, SAM encoder/decoder, IoU head        | **2b.2**     | 2c (SAM3 PCS), 2e (SAM2 propagation)                        |
| SAM2 streaming memory, occlusion head              | **2b.3**     | 2c (SAM3 tracker inherits SAM2), **2e.2 expands this**      |
| Multimodal alignment (CLIP), grounding             | **2c.1–2.2** | 2c.3 (SAM3 PCS), 2d (Qwen-VL)                               |
| Autoregressive LM, instruction tuning              | **2d.1**     | 2c (VLM backbones), 2d.3 (agent)                            |
| ReAct loop, tool use                               | **2d.3**     | 4 (pipeline Block 3)                                        |
| VOS, memory-based propagation                      | **2e.1**     | — (terminal)                                                |


**Rule:** if a concept's "Defined in" cell is not the current sub-chapter, the
draft *references* it ("as introduced in §2a.1") and does not re-derive it.

---

## §2 — Arc, Budget, and Depth Calls

Source of distilled content per topic: `project-guidelines/thesis-theory-topics.md`
(already maps each subtopic → theory → bibtex keys → pipeline link). Sub-briefs
paste from there; this chapter does not require new research, only sequencing.


| Sub-ch                           | Pages | Arc                  | Deep (mechanistic math)                                | Surface (1 para)                           | Skip                       |
| -------------------------------- | ----- | -------------------- | ------------------------------------------------------ | ------------------------------------------ | -------------------------- |
| 2a Visual Representation         | ~4    | B (concept-first)    | Attention eq., ViT patch embed, DINO self-distillation | DeiT, MAE                                  | full SSL taxonomy          |
| 2b Correspondence & Segmentation | ~2    | D (survey→key model) | RoMa dense match, SAM 3-component design               | handcrafted descriptors, FCN/UNet/MaskRCNN | SAM training engine detail |
| 2c Language-grounding            | ~4    | B                    | CLIP InfoNCE, GroundingDINO cross-modal, SAM3 PCS      | LLaVA, Flamingo                            | DETR full derivation       |
| 2d Foundation Models & Agents    | ~2    | C (our-use-first)    | ReAct loop, tool schema                                | scaling laws, RLHF/DPO                     | LLM pretraining detail     |
| 2e Addition of Time              | ~2    | A (problem-first)    | SAM2 memory attention, bi-directional propagation      | OSVOS/STM/XMem lineage                     | unsupervised VOS           |


**Global depth principle (from scope.md):** rigorous and unambiguous but not
exhaustive — show the key equation, skip full derivations unless central to our
contribution. The Transformer attention equation and the SAM/SAM3 mask
formulation are central (they recur in the pipeline); everything else is
one-equation-and-move-on.

---

## §3 — Sub-Brief Production Checklist

For each of 2a–2e, fill `00-chapter-brief-template copy.md` →
`02x-[name]-brief.md` with:

- [ ] Header line pinning notation contract §0 + canonical/local rule
- [ ] "Connect back to" populated from the dependency map (§1) — name the exact section that *defines* each reused concept
- [ ] "Deep / Surface / Skip" copied from §2 table
- [ ] Paper/theory content pasted from `thesis-theory-topics.md` (matching subtopic)
- [ ] bibtex keys listed verbatim from `thesis-theory-topics.md` (never invented)
- [ ] Any **[ADD]** macro this sub-chapter introduces, noted so it lands in `custom-macros.tex` before first use