# Chapter Brief: 2f — Related Works

---

**Notation discipline:** Use the notation contract in `theory-brief.md` §0 verbatim.
Honour the canonical/local rule. Do not reinvent symbols. **No new canonical symbols are
introduced here** — this sub-chapter is a literature survey. Reuse $\mask$ (2b), $\warp$
(2b), $t$ (text prompt, 2c) when referring back to prior-section concepts; do not define
them again from scratch.

You are helping me draft Subchapter 2f of my master's thesis. The project
knowledge files contain my style guide, scope, writing samples, and thesis
outline. Read them before drafting.

**Chapter**: 2f — Related Works  
**Target length**: ~5 pages  
**Arc**: D — Survey condensing to the positioning statement (datasets → correspondence methods → segmentation methods → ego-exo methods → our gap)  
**Deep treatment**: ego-exo correspondence methods (§2f.4) — each prior method described and positioned; Ego-Exo4D Correspondences benchmark annotation protocol  
**Surface only**: handcrafted/learned correspondence lineage (one para — already treated in 2b.0; back-reference only); segmentation milestones (one para survey — mechanistically covered in 2b/2c; back-reference only); dataset details for EPIC-Kitchens and Ego4D (two–three sentences each)  
**Skip**: Ego4D subtask benchmarks beyond Correspondences; SAM/SAM2/SAM3 internals (covered in 2b/2c — name and cite only); LM-EEC architecture details (high-level positioning only)  
**Connect back to**: 2b.0 (correspondence lineage), 2b.1–2b.3 (SAM family + segmentation milestones), 2c (language-conditioned segmenters), 2e (XMem as a memory-VOS component and as an official baseline)  
**Connect forward to**: 3 (every prior method motivates a design choice — XMem → Block 4 propagation; language gap → language-as-bridge framing; O-MaMa DINOv2 space → why we avoid a learned latent); 4 (benchmark definition + metrics)

**Local notation to harmonise a posteriori** (footnote originals on first use):
Ego-Exo4D Correspondences task definition (source ego mask → destination exo mask); per-method supervision requirements.

**[ADD] macros introduced here**: none new.

**Theory content + bibtex keys**: paste from `thesis-theory-topics.md`
§ "Chapter 2f — Related Works" (2f.1–2f.4). Keys: `grauman2022ego4d`,
`grauman2024egoexo4d` (verify exact keys in `references.bib`); segmentation back-refs:
`long2015fcn`, `he2017maskrcnn`, `kirillov2023sam`, `ravi2024sam2`, `ravi2025sam3`,
`lai2024lisa`, `rasheed2024glamm`, `psalm2024eccv`, `yuan2025sa2va`, `cheng2022xmem`,
`oh2019stm`; correspondence back-refs: `lowe2004sift`, `bay2008surf`, `rublee2011orb`,
`detone2018superpoint`, `sarlin2020superglue`, `sun2021loftr`, `roma`, `oquab2023dinov2`;
ego-exo methods: `fu2025objectrelator`; add keys for XSegTx, O-MaMa, V2-SAM, LM-EEC
when confirmed in `references.bib`.

Close §2f.4 with the positioning statement: all prior methods learn correspondences from
paired ego-exo visual features or require task-specific fine-tuning; our pipeline treats
the task as training-free composition of foundation models with language as the semantic
bridge. This statement is the logical conclusion of Chapter 2 and the premise of Chapter 3.

--

**Paper section to expand** (follow this order of information):

```
\section{Related Work}
 
\paragraph{Ego and ego-exo datasets.}
Large-scale egocentric datasets (EPIC-Kitchens \cite{damen2021epickitchens100}
and Ego4D \cite{grauman2022ego4d}) established first-person video as a core
research area, driving progress in action recognition~\cite{bansal2022myview, radevski2023multimodal}, action anticipation~\cite{furnari2020rolling, murlabadia2024affttention}, affordance segmentation~\cite{murlabadia2023multilabel, nagarajan2020egotopo}, and episodic memory~\cite{barmann2022episodic, mai2023egoloc}. Ego-Exo4D~\cite{grauman2024egoexo4d} extended this to synchronized multi-camera recordings of skilled human activities, and enabling tasks that require relating observations across first- and third-person perspectives, including the Correspondences benchmark, depicted in figure \ref{fig:cross-view-corr}.
 
\paragraph{Learning correspondences.}
Traditional feature matching relies on handcrafted local descriptors such as
SIFT~\cite{lowe2004sift}, SURF~\cite{bay2008surf}, and ORB~\cite{rublee2011orb}.
Learning-based approaches improve robustness through trained keypoint
detectors~\cite{detone2018superpoint} and graph neural network
matchers~\cite{sarlin2020superglue}. Detector-free methods~\cite{sun2021loftr, roma} bypass keypoint detection entirely, producing dense correspondences under large viewpoint and illumination changes. Among these, RoMa~\cite{roma} achieves state-of-the-art dense feature matching by learning a robust warp field between image pairs of a 3D scene. At the semantic level, the DINO model family \cite{dinov1, oquab2023dinov2, dinov3} features establish correspondences between semantically similar images without geometric priors. Ego-exo correspondence is an extreme case of this last setting, challenging all methods above.
 
\paragraph{Segmentation models.}
Classical methods process single images, from fully convolutional networks~\cite{long2015fcn} to region-based instance segmenters~\cite{he2017maskrcnn}. SAM~\cite{kirillov2023sam} introduced promptable segmentation, generating masks from spatial prompts in a task-agnostic manner.
SAM2~\cite{ravi2024sam2} extended this to video via a streaming memory architecture, and SAM3~\cite{ravi2025sam3} adds text-prompt conditioning, enabling open-vocabulary segmentation. Large multimodal models fused with segmentation decoders like LISA~\cite{lai2024lisa}, GLaMM~\cite{rasheed2024glamm}, PSALM~\cite{psalm2024eccv}, and Sa2VA~\cite{yuan2025sa2va}, that produce pixel-level masks from free-form language instructions. For temporal propagation, memory-based VOS methods~\cite{oh2019stm, cheng2022xmem} store past frame-mask pairs and retrieve matching features frame by frame. While not a direct segmentation method, GroundingDINO~\cite{liu2024groundingdino} grounds noun phrases to spatial regions, providing the localization signal through a bounding box and confidence score.
 
\paragraph{Ego-exo correspondence methods.}
The Ego-Exo4D Correspondences benchmark~\cite{grauman2024egoexo4d} defines
the task of predicting an object's mask across synchronized ego and exo views.
The official baselines, XSegTx and XMem+XSegTx~\cite{grauman2024egoexo4d},
extend image co-segmentation with cross-view temporal memory; XSegTx adapts
SegSwap~\cite{segswap} for this role, while XMem+XSegTx~\cite{cheng2022xmem}
embeds source object appearances and retrieves them via cross-attention.
ObjectRelator~\cite{fu2025objectrelator} fine-tunes PSALM with view-invariant
alignment modules, incorporating language as an explicit cross-view cue in a
trainable component to improve object localisation.O-MaMa~\cite{o-mama}, winner of the 2025 Ego-Exo4D Correspondences Challenge,
generates candidate masks in the destination view via FastSAM and selects the
best match using pooled DINOv2 features in a learned cross-view latent space,
reaching competitive performance with only 1\% of ObjectRelator's trainable
parameters. V\textsuperscript{2}-SAM~\cite{pan2025v2sam} combines SAM2 with multi-prompt experts for cross-view segmentation. LM-EEC~\cite{hu2025lmeec} adapts SAM2 with a Mixture-of-Experts cross-view fusion module and a dual compressed long-term memory trained end-to-end on the correspondence task, representing the current state of the art. All these methods learn correspondences from visual features alone and require paired ego-exo supervision. 

We take a different route: natural language bridges the two views, allowing our pipeline to locate objects in the destination view using a combination of modules that were not initially designed for this task but are foundational enough that they do not require additional training steps. Where all prior methods treat this as a supervised correspondence learning problem, we treat it as a test of whether general-purpose foundation models, used without any task-specific supervision, can transfer to a novel visual task.
```

--

Draft the full chapter now. Write in my voice as documented in the style guide.
Use LaTeX formatting throughout. After the draft, flag any decisions you made.

---

## Post-Draft Checklist

- [ ] No new symbol definitions — only back-references to 2b/2c/2e symbols
- [ ] Voice matches style guide
- [ ] Survey paragraphs kept tight (one para each for EPIC-Kitchens, Ego4D; correspondence lineage back-references 2b.0; segmentation lineage back-references 2b/2c)
- [ ] §2f.4 covers every prior method and closes with the positioning statement
- [ ] Backward links to 2b/2c/2e + forward links to Chapter 3 present
- [ ] Citations use inline name+year + \cite{key} format
- [ ] Nothing sounds like it was written by an AI
