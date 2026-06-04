# Chapter Brief: 2b — Segmentation & Correspondence

---

**Notation discipline:** Use the notation contract in `theory-brief.md` §0 verbatim.
Honour the canonical/local rule. Do not reinvent symbols. **This sub-chapter defines
the canonical mask and matching symbols** ($\mask$, $\iou$, $\warp$, match confidence
$c$); 2c and 2e reference them.

You are helping me draft Subchapter 2b of my master's thesis. The project
knowledge files contain my style guide, scope, writing samples, and thesis
outline. Read them before drafting.

**Chapter**: 2b — Segmentation & Correspondence
**Target length**: ~2 pages
**Arc**: D — Survey condensing to the key model (matching lineage → RoMa; segmentation lineage → SAM → SAM2)
**Deep treatment**: RoMa dense warp field $\warp$ with per-match confidence; SAM three-component design (ViT encoder / prompt encoder / two-way mask decoder + IoU head); SAM2 streaming memory + occlusion head.
**Surface only**: handcrafted descriptors (SIFT/SURF/ORB, one para); SuperPoint+SuperGlue, LoFTR (one para each); FCN/U-Net/Mask R-CNN/DeepLab/RITM segmentation lineage (one para survey)
**Skip**: SAM SA-1B data-engine internals; panoptic-segmentation formalism beyond naming, SAM 3 data pipeline.
**Connect back to**: 2a.1 (Transformer/attention — SAM decoder and matchers reuse it), 2a.2 (ViT — SAM/MAE encoder), 2a.3 (DINO patch features for label-free semantic correspondence)
**Connect forward to**: 2c (SAM3 PCS builds on SAM; tracker inherits SAM2), 2e.2 (expands SAM2 memory architecture for propagation)

**Local notation to harmonise a posteriori** (footnote originals on first use):
RoMa warp/match-decoder internals beyond canonical $\warp$; SAM IoU-score head; SA-1B engine notation.

**[ADD] macros introduced here**: `\warp` ($\mm{W}$), `\cossim` — ensure both are in `custom-macros.tex` (already added).

**Theory content + bibtex keys**: paste from `thesis-theory-topics.md`
§ "Chapter 2b — Correspondence & Segmentation" (2b.0–2b.3). Keys: `lowe2004sift`,
`bay2008surf`, `rublee2011orb`, `detone2018superpoint`, `sarlin2020superglue`,
`sun2021loftr`, `roma`, `long2015fcn`, `ronneberger2015unet`, `he2017maskrcnn`,
`kirillov2023sam`, `ravi2024sam2`. Pipeline link: RoMa = geometry-based, language-free
anchor selection (confident-match count inside source mask); SAM2/SAM3 video model =
Block 4 propagation. Note: SAM3 text-prompt extension is the addition covered in 2c.

Draft the full chapter now. Write in my voice as documented in the style guide.
Use LaTeX formatting throughout. After the draft, flag any decisions you made.

---

## Post-Draft Checklist

- [ ] $\mask$, $\iou$, $\warp$ defined exactly as in contract §0.1
- [ ] Voice matches style guide
- [ ] Every equation has a component-by-component explanation in prose
- [ ] Backward links to 2a + forward links to 2c/2e present
- [ ] Survey items kept to one paragraph each (§2 Surface calls)
- [ ] Citations use inline name+year + \cite{key} format
- [ ] Nothing sounds like it was written by an AI
