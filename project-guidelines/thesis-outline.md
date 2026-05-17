## Narrative Arc
The thesis formally presents the work I completed as an empirical research assistant during the last semester of my Master studies. It builds upon all the research, studies, experiments, results, and learnings that my team and I went through. Therefore, the narrative flow will follow that of academic research. 

First, we will introduce the problem we are trying to solve, highlighting the gap in the field, anticipating our pipeline, and listing our contributions. Because our method leverages the inference capabilities of several state of the art models, we will follow the intruduciton with a theoretical chapter. The aim is to build a mechanistic understanding of the architectures used later in the pipeline. Next, we will move on the actual work done in the last months. First, we define formally the problem and present related works, again underlining the gap in the field in more detail. Second, we will discuss our pipeline, validating choices with empirical analyses, experiments, results, and ablations. Finally, we will wrap up the findings with a conclusion, connecting again to the problem statement, highlighting limitations, and suggesting future research directions.

## Table of contents 
1.	Introduction (3-5 pages)
a.	Problem motivation: object correspondence, motivation.
b.	Research gap: no inference only, no exploration of language. 
c.	Research contribution: language as a bridge pipeline for cross-view correspondence on video data, language-video grounding for conditioned frame selection, ablation and interpretability analysis on stack of foundation models in a setting they were not designed for.
d.	Chapter roadmap.
 
2.	Theory (~20 pages)
a.	Visual Representation: Transformer, Vision Transformer, Self-supervised learning with DINOv2.
b.	Segmentation: previous approaches, SAM & SAM2.
c.	Language-grounding: multimodality, Grounding DINO, SAM 3 Promotable Concept Segmentation, PixelRefer.
d.	Foundation Models & Agents: Qwen Family, ReAct, tool use.
e.	Addition of time: Video Object Segmentation and Propagation

3.	Related works (~5 pages)
a.	Challenge baselines
b.	O-MaMa
c.	LM-EEC.

4.	Proposed Pipeline (~5 pages)
a.	Block by block, motivation for each design choice. 

5.	Experiments (~10 pages)
a.	Setup, metrics
b.	Results
c.	Ablations & interpretability
 
6.	Conclusions (~5 pages)
a.	Summary of findings
b.	Limitations
c.	Future work