# Chapter Brief: 2c — Language-grounding

---

**Notation discipline:** Use the notation contract in `theory-brief.md` §0 verbatim.
Honour the canonical/local rule. Do not reinvent symbols. **This sub-chapter defines
the canonical multimodal symbols** (text prompt $t$, encoders $g_\text{txt}/g_\text{img}$);
reuses $\mm{X},\attn$ (2a) and $\mask$ (2b). Reserve $t$ for text — frame index is $\tau$.

You are helping me draft Subchapter 2c of my master's thesis. The project
knowledge files contain my style guide, scope, writing samples, and thesis
outline. Read them before drafting.

**Chapter**: 2c — Language-grounding
**Target length**: ~4 pages
**Arc**: B — Concept first (multimodal alignment → open-set detection → promptable concept segmentation → mask-conditioned description)
**Deep treatment**: CLIP dual-encoder + InfoNCE; GroundingDINO cross-modal feature enhancer + language-guided query selection; SAM3 PCS task + architecture innovations + agentic interface (planner LLM iteratively invoking SAM3)
**Surface only**: LLaVA visual instruction tuning (one para); Flamingo (optional, one line); PixelRefer architecture + MiniLM re-ranking (condense — it is a pipeline component, not a backbone)
**Skip**: DETR full Hungarian-matching derivation (name it, cite, move on); referring-expression-comprehension literature beyond the REG/REC distinction
**Connect back to**: 2a.1 (attention — cross-modal attention), 2a.2 (ViT — CLIP/GDINO image encoders), 2b.2 (SAM — SAM3 extends it), 2b.3 (SAM2 — SAM3 tracker inherits it)
**Connect forward to**: 2d (Qwen-VL is the agent backbone; the SAM3 agentic interface = pipeline Block 3), 4 (canonical noun-phrase finding → VLM prompt design)

**Local notation to harmonise a posteriori** (footnote originals on first use):
CLIP InfoNCE temperature; GroundingDINO query-selection indices; PixelRefer mask-channel injection; Sentence-BERT mean-pooling.

**[ADD] macros introduced here**: `\cossim` (re-rank similarity — already added); $g_\text{txt}/g_\text{img}$ written raw.

**Theory content + bibtex keys**: paste from `thesis-theory-topics.md`
§ "Chapter 2c — Language-Grounding" (2c.1–2c.4). Keys: `radford2021clip`,
`liu2023llava`, `carion2020detr`, `zhang2023dino`, `liu2024groundingdino`,
`ravi2025sam3`, `chen2024pixelrefer`, `reimers2019sentencebert`, `wang2020minilm`.
Pipeline link: GDINO confidence top-K = language anchor selection; PixelRefer =
mask-to-description; MiniLM cosine pre-filter before MLLM verification; SAM3 agentic
loop = Block 3.

Draft the full chapter now. Write in my voice as documented in the style guide.
Use LaTeX formatting throughout. After the draft, flag any decisions you made.

---

## Post-Draft Checklist

- [ ] $t$ used for text only (frame index is $\tau$); reuses 2a/2b symbols, no redefinition
- [ ] Voice matches style guide
- [ ] Every equation has a component-by-component explanation in prose
- [ ] Backward links to 2a/2b + forward links to 2d/4 present
- [ ] SAM2 not re-derived (referenced from 2b.3); DETR not fully derived (§2 Skip)
- [ ] Citations use inline name+year + \cite{key} format
- [ ] Nothing sounds like it was written by an AI
