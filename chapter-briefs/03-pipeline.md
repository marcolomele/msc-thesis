# Chapter Brief: 3 – Proposed Pipeline

---

You are helping me draft Chapter 3 of my master's thesis.
The project knowledge files contain my style guide, scope, writing samples,
and thesis outline. Read them before drafting.

**Chapter**: Proposed Pipeline
**Target length**: 3
**Arc**: C — Our work first.
**Deep treatment**: task definition and notation, mechanics and operations of each block, motivations of each choice.
**Surface only**: implemented with python (pytorch, higging face, and all other typical libraries), tested and developed on h200 cuda nvidia gpus. 
**Skip**: development journal, straight to end result. 
**Connect back to**: Chapter 2 introduced theoryes — make the link explicit in prose.
**Connect forward to**: Chapter 4 will have experimens, spcific design choices motivated by ablations, and results — foreshadow without over-explaining.

**Paper section to expand** (follow this order of information):
```
% ------------------------------------------------------------
\subsection{Method Overview}
% ------------------------------------------------------------

Our pipeline mirrors the dual problem structure described above, with most of
its modules addressing the harder \emph{cross-view transfer} problem, while
delegating \emph{temporal completeness} to SAM~2's native video
tracker~\cite{ravi2024sam2}.
As illustrated in Figure~\ref{fig:pipeline}, the pipeline processes each
(take, object, direction) triplet through four sequential blocks.
First, a set of seed frames is selected from the source video $\textbf{I}^S$
(Block~1).
Then, a vision-language model generates a view- and time-independent
description of the target object from the seed frames (Block~2).
Next, this description guides the selection of a set of anchor frames from
$\textbf{I}^D$, and then an independent SAM~3 agentic loop on each anchor
frame to segment the target object (Block~3).
Finally, the resulting anchor masks are propagated across the remaining frames
of $\textbf{I}^D$ (Block~4).

\paragraph{Block 1: Frame selection.}
The accuracy of the VLM description in Block~2 depends directly on how clearly
the target object is visible in the source frames presented to it.
To this end, we score each annotated frame $I_t^S$ with a composite heuristic
on the ground-truth mask $M_t^S$ defined as a weighted combination of
(i)~the normalised mask area, measuring the fraction of the frame occupied by
the target object, and (ii)~centrality, defined as the complement of the
normalised distance between the mask centroid and the frame center.
Area is the dominant factor, receiving 99\% of the total weight, with
centrality acting as a marginal tie-breaker that prefers well-centered objects.
We show the effectiveness of this method in the extended ablations in the
appendix.
The frames achieving the highest scores are selected as seed frames, defining
the seed index set $\mathcal{T}^S \subseteq \{1, \ldots, T\}$, and their
ground-truth masks $\{M_t^S\}_{t \in \mathcal{T}^S}$ are used to construct the
annotated images passed to Block~2.

\paragraph{Block 2: VLM description.}
We bridge the source and destination frames using object descriptions generated via a vision-language model (VLM). For the bridge to be effective, the resulting description must simultaneously be \emph{view-independent}, valid when the object is observed from the radically different viewpoint of camera $D$, and \emph{time-independent}, holding for every destination frame $I_t^D$ and not only those temporally close to the seed frames. The second requirement is crucial for destination frame selection. The extreme viewpoint change provides no guarantee that the corresponding destination frames of the seed frames will be reliable segmentation anchors, and the object's local surroundings may change entirely across the video. In other words, there is no reliable symmetry in quality across the two views.   Therefore, we instruct the VLM to generate only intrinsic object properties, namely colour, canonical identity, material, and structural parts, all elements that remain constant with respect to both viewpoint and time. For each seed frame $I_t^S$, $t \in \mathcal{T}^S$, the VLM receives two images: one with an overlay mask annotation rendering $M_t^S$ as a red semi-transparent region with a contour outline, and the other in raw form. Finally, the VLM generated object description $w_t$ for each $\,t\in \mathcal{T^S}$. In the ablations (Section~\ref{sec:ablations}), we validate that descriptions satisfying both requirements lead to more effective destination frame selection in Block~3.

\paragraph{Block 3: SAM~3 agentic grounding.}
To identify the target object in the destination video, the pipeline proceeds
with anchors selection and agentic segmentation. 
In anchors selection, candidate destination frames are scored by how likely they are to contain the target object, using either GroundingDINO~\cite{liu2024groundingdino}
or RoMa~\cite{roma}. GroundingDINO scores each frame $I_t^D$ by running open-vocabulary object detection with the VLM text description $w_t$ as the query, providing a language-grounded visibility signal, whereas RoMa offers a geometry-based alternative, scoring each frame $I_t^D$ by how many pixels of the source object region $\{M_t^S\}_{t \in \mathcal{T}^S}$ find a confident visual match within it, providing a language-free visibility signal. The highest-scoring frames are selected as anchor frames, defining the index set $\mathcal{T}^A \subseteq \{1, \ldots, T\}$.  In agentic segmentation, each anchor frame $I_t^D$, $t \in \mathcal{T}^A$, is then independently passed through the SAM~3 Agent loop. Fundamentally, the loop the same VLM from Block~2 as an orchestrator. First, the VLM simplifies the generated description into a short noun-phrase prompt. Then, SAM~3 segments all matching instances in the anchor frame. Finally, the resulting masks are inspected one-by-one by the VLM, which accepts or rejects each candidate against the original object description. If the VLM rejects all masks, the loop restarts with a different simplified prompt. The mask accepted at convergence for each anchor frame produces the anchor mask set $\{\hat{M}_t^D\}_{t \in \mathcal{T}^A}$. Its will be anchoring mask propagation by providing examples of the object's appearance and location in the destination view. 

\paragraph{Block 4: Video propagation.}
The anchor masks $\{\hat{M}_t^D\}_{t \in \mathcal{T}^A}$ are propagated bidirectionally across the remaining frames $\{I_t^D\}_{t \notin \mathcal{T}^A}$ of the destination video using SAM~2's video tracker. In essence, the tracker maintains a memory bank that holds both the anchor frames, to which the masks have been assigned, and the frames most recently processed during propagation. At each new frame $I_t^D$, $t \notin \mathcal{T}^A$, the model conditions its prediction on this entire memory via cross-attention, which is composed of the anchors and the most recently processed frames. This design is particularly effective in egocentric video, where high temporal variability driven by rapid camera motion and frequent temporary occlusions means that the frames immediately preceding $I_t^D$ are not necessarily the most informative reference for mask propagation.By relying on attention for memory selection, the tracker can draw from whichever anchor or recent frame best represents the current appearance of the object, regardless of temporal distance. Any non-empty predicted masks at any frame $I_t^D$ for the take is recorded as a prediction for the target object.


```

Draft the full chapter now. Write in my voice as documented in the style guide.
Use LaTeX formatting throughout: display equations with \begin{equation},
inline math with $...$, citations with \cite{key}.
After the draft, flag any decisions you made that I should review
(depth calls, skipped content, structural choices).

---

## Post-Draft Checklist

- [ ] Voice matches style guide (read the Red Flags section)
- [ ] Structural flow matches the paper section's order of information
- [ ] Every equation has a component-by-component explanation in prose
- [ ] Every figure is referenced in text before it appears
- [ ] Forward and backward connections are present
- [ ] No concept is explained that scope.md says to skip
- [ ] Citations use inline name+year + \cite{key} format
- [ ] Closing paragraph pivots correctly to what comes next
- [ ] Nothing sounds like it was written by an AI
