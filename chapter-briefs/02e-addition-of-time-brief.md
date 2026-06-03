# Chapter Brief: 2e — Addition of Time

---

**Notation discipline:** Use the notation contract in `theory-brief.md` §0 verbatim.
Honour the canonical/local rule. Do not reinvent symbols. **Uses the reserved temporal
symbols**: frame index $\tau$, frame count $T$; memory bank is $\set{M}$ (calligraphic),
distinct from mask $\mask$. Reuses $\mask$ (2b) and SAM2 memory (2b.3 — expand here).

You are helping me draft Subchapter 2e of my master's thesis. The project
knowledge files contain my style guide, scope, writing samples, and thesis
outline. Read them before drafting.

**Chapter**: 2e — Addition of Time
**Target length**: ~2 pages
**Arc**: A — Problem first (temporal completeness: a single-frame mask must become a full-video masklet)
**Deep treatment**: SAM2 memory attention (cross-attending stored (frame,mask) pairs) expanded from 2b.3; bi-directional propagation (forward from earliest seed, backward from latest, nearest-seed selection); mask union at shared frames
**Surface only**: VOS task definition (semi- vs unsupervised, one para); OSVOS/OnAVOS (one line — per-video fine-tuning cost); STM/XMem memory lineage (one para)
**Skip**: unsupervised VOS; DEVA/open-vocab VOS detail (optional one-line mention only)
**Connect back to**: 2b.3 (SAM2 streaming memory + occlusion head — this chapter expands it), 2b.0 (warp/drift framing), 2c.3 (SAM3 tracker inherits SAM2)
**Connect forward to**: 4 (Block 4: `propagation.py` runs SAM3 video model forward+backward from seed frames, merges by nearest-seed); 5 (evaluated by IoU/LE/CA per frame)

**Local notation to harmonise a posteriori** (footnote originals on first use):
STM/XMem memory read/write operators; SAM2 occlusion-head output; `max_cond_frames_in_attn` cap as a stated constraint.

**[ADD] macros introduced here**: `\frameidx` ($\tau$), `\nframes` ($T$) — already added; memory bank $\set{M}$ written raw.

**Theory content + bibtex keys**: paste from `thesis-theory-topics.md`
§ "Chapter 2e — Addition of Time" (2e.1–2e.2). Keys: `caelles2017osvos`, `oh2019stm`,
`cheng2022xmem`, `ravi2024sam2`. Pipeline link: bi-directional propagation strategy,
multi-seed conditioning (cap 4), mask union (pixel-wise OR), temporal-consistency vs
per-frame-accuracy trade-off (drift over long occlusions).

Draft the full chapter now. Write in my voice as documented in the style guide.
Use LaTeX formatting throughout. After the draft, flag any decisions you made.

---

## Post-Draft Checklist

- [ ] Temporal symbols: $\tau$ (frame), $T$ (count), $\set{M}$ (memory) — never overloaded with $t$/$n$/$\mask$
- [ ] Voice matches style guide
- [ ] Every equation has a component-by-component explanation in prose
- [ ] SAM2 memory expanded from 2b.3 (not re-introduced from scratch)
- [ ] Forward links to Block 4 + Chapter 5 metrics present
- [ ] Citations use inline name+year + \cite{key} format
- [ ] Nothing sounds like it was written by an AI
