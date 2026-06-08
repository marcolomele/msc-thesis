## Narrative Arc
The thesis formally presents the work I completed as an empirical research assistant during the last semester of my Master studies. It builds upon all the research, studies, experiments, results, and learnings that my team and I went through. Therefore, the narrative flow will follow that of academic research. 

First, we will introduce the problem we are trying to solve, highlighting the gap in the field, anticipating our pipeline, and listing our contributions. Because our method leverages the inference capabilities of several state of the art models, we will follow the intruduciton with a theoretical chapter. The aim is to build a mechanistic understanding of the architectures used later in the pipeline. Next, we will move on the actual work done in the last months. First, we define formally the problem and present related works, again underlining the gap in the field in more detail. Second, we will discuss our pipeline, validating choices with empirical analyses, experiments, results, and ablations. Finally, we will wrap up the findings with a conclusion, connecting again to the problem statement, highlighting limitations, and suggesting future research directions.

## Table of contents 
1.	Introduction (3 pages)
a.	Problem motivation: object correspondence, motivation.
b.	Research gap: no inference only, no exploration of language. 
c.	Research contribution: a fully training-free (camera- and dataset-agnostic) "language as a bridge" pipeline for cross-view object mask transfer on video; two interchangeable anchor-frame selection signals — language-grounded (Grounding DINO) and geometry-based (RoMa); a detailed ablation and interpretability analysis of a stack of foundation models in a setting none were designed for. Results reach within a few points of the 2025 challenge winner (O-MaMa) and surpass the official baselines and ObjectRelator.
d.	Chapter roadmap.
 
2.	Theory & Related Works (~20 pages)
a.	Visual Representation (~4 pages): Transformer, Vision Transformer, Self-supervised learning with DINOv2.
b.	Segmentation (~4 pages): prior segmentation works (FCN, U-Net, Mask R-CNN, RITM); Segment Anything Model (SAM); video propagation with SAM 2 (VOS history, memory-based paradigm, bi-directional propagation).
c.	Language-grounding (~4 pages): multimodality, Grounding DINO, SAM 3 Promptable Concept Segmentation (PCS) and its agentic interface, PixelRefer.
d.	Foundation Models & Agents (~2 pages): Qwen Family, ReAct, tool use.
e.	Related Works (~5 pages) — feature matching history first, then ego-exo survey
	i.   Feature matching and dense correspondence (handcrafted descriptors → SuperPoint/SuperGlue → LoFTR → RoMa).
	ii.  Ego and ego-exo datasets (EPIC-Kitchens, Ego4D, Ego-Exo4D and its Correspondences benchmark).
	iii. Ego-exo correspondence methods: official baselines (XSegTx, XMem+XSegTx), ObjectRelator, O-MaMa (challenge winner), V2-SAM, LM-EEC (SOTA). Positioning: all prior methods learn correspondences from visual features and require paired ego-exo supervision; we treat the task as a test of training-free foundation-model composition with language as the bridge.

3.	Proposed Pipeline (~5 pages)
a.	Task formulation and the dual problem (cross-view transfer + temporal completeness); preliminary on SAM 3.
b.	Block by block (1 frame selection, 2 VLM description, 3 SAM 3 agentic grounding = anchor selection + agentic segmentation, 4 propagation), motivation for each design choice. 

4.	Experiments (~10 pages)
a.	Setup, metrics
b.	Results
c.	Ablations & interpretability
 
5.	Conclusions (~5 pages)
a.	Summary of findings
b.	Limitations
c.	Future work