# Chapter Brief: 4 – Experiments

---

You are helping me draft Chapter 4 of my master's thesis.
The project knowledge files contain my style guide, scope, writing samples,
and thesis outline. Read them before drafting.

**Chapter**: Experiments
**Target length**: 15
**Arc**: Problem First
**Deep treatment**: Experimental set up, comparison with the state of the art, ablation study and interpretability anlyses. 
**Surface only**: NA
**Skip**: Development journal. 
**Connect back to**: Chapter 3 introduced the pipeline blocks, Chapter 2 introduced the theoretical components used in the pipeline — make the link explicit in prose
**Connect forward to**: NA. 

**Paper section to expand** (follow this order of information):
```
\section{Experiments}
\label{sec:experiments}

% ------------------------------------------------------------
\subsection{Experimental Setup}
% ------------------------------------------------------------

\paragraph{Training-Free.}
No component of our pipeline is trained.
Every module is taken off-the-shelf and used in pure inference
mode, run locally on a NVIDIA H200 and with no fine-tuning on the Ego-Exo4D dataset or any task-specific supervision, making our pipeline agnostic to camera configurations and test dataset. This distinguishes our method from all competitive baselines, which require dedicated training on the benchmark. To the best of our knowledge, we are the first to attempt a pipeline of this flavor on the cross-view object correspondence task.

\paragraph{Evaluation.}
Following the official Ego-Exo4D Correspondences benchmark~\cite{grauman2024egoexo4d},
we adopt Intersection over Union (IoU) as the primary evaluation metric.
We additionally report Location Error (LE), which quantifies the normalized
distance between the predicted and ground-truth mask centroids, and Contour Accuracy (CA)~\cite{perazzi2016benchmark}, which measures the similarity between predicted and ground-truth mask contours after translation.

\paragraph{Implementation details.}
We set $|\mathcal{T}^S| = 1$ seed frame and $|\mathcal{T}^A| = 3$ anchor frames.
A single seed frame is sufficient to produce a reliable object description, as
the ablations (Section~\ref{sec:ablations}) show that adding further frames
yields negligible improvement.
Three anchor frames balance propagation stability against computational cost.
We report results under both GroundingDINO~\cite{liu2024groundingdino} and
RoMa~\cite{roma} as anchor frame selection strategies, as the two
produce comparable results.
We use Qwen3.5-35B~\cite{qwen35blog} as the vision-language model backend for both
Block~2 and the SAM~3 agentic loop, and we explicitly instruct it to restrict
descriptions to colour and canonical object identity, excluding view-dependent
contextual cues that would introduce noise into anchor frame selection. 
% ------------------------------------------------------------
\subsection{Comparison with the State of the Art}
\label{sec:comparison}
% ------------------------------------------------------------
\begin{table*}[t]
\centering\scriptsize
\setlength{\tabcolsep}{2pt}
\caption{Results on the Ego-Exo4D Correspondences v2 test split.}
\label{tab:main}
\resizebox{\linewidth}{!}{%
\begin{tabular}{l ccc ccc ccc}
  \toprule
    & \rotatebox{90}{Training Free\;}
    & \rotatebox{90}{Camera Agnostic\;}
    & \rotatebox{90}{Dataset Agnostic\;}
    & \multicolumn{3}{c}{\textit{Ego2Exo}}
    & \multicolumn{3}{c}{\textit{Exo2Ego}} \\
  \cmidrule(lr){5-7}\cmidrule(lr){8-10}
  Method
  & & &
    & IoU$\uparrow$ & LE$\downarrow$ & CA$\uparrow$
    & IoU$\uparrow$ & LE$\downarrow$ & CA$\uparrow$ \\
  \midrule
  XSegTx~\cite{grauman2024egoexo4d}
    & $\times$ & $\times$ & $\times$ & 18.9 & 0.070 & 0.386 & 27.1 & 0.104 & 0.358 \\
  XMem~\cite{grauman2024egoexo4d}
    & $\times$ & $\times$ & $\times$ & 19.3 & 0.151 & 0.262 & 16.6 & 0.160 & 0.240 \\
  XMem+XSegTx~\cite{grauman2024egoexo4d}
    & $\times$ & $\times$ & $\times$ & 34.9 & 0.038 & 0.559 & 25.0 & 0.117 & 0.237 \\
  \midrule
  ObjectRelator~\cite{fu2025objectrelator}
    & $\times$ & $\times$ & \checkmark & 35.3 & 0.036 & 0.540 & 40.3 & 0.068 & 0.500 \\
  O-MaMa~\cite{o-mama}
    & $\times$ & $\times$ & $\times$ & 42.6 & 0.033 & 0.590 & 44.1 & 0.082 & 0.524 \\
  V$^2$-SAM~\cite{pan2025v2sam}
    & $\times$ & $\times$ & \checkmark & 46.3 & 0.056 & 0.616 & 49.6 & 0.078 & 0.575 \\
  LM-EEC~\cite{hu2025lmeec}
    & $\times$ & $\times$ & $\times$
    & \textbf{54.98} & \textbf{0.017} & \textbf{0.778}
    & \textbf{65.77} & \textbf{0.031} & \textbf{0.774} \\
  \midrule
  \shortstack[l]{Ours (Language as Bridge + G-DINO)} & \checkmark & \checkmark & \checkmark & 37.7 & 0.071 & 0.471 & 40.6 & 0.114 & 0.468 \\
  \shortstack[l]{Ours (Language as Bridge + RoMa)} & \checkmark & \checkmark & \checkmark & -- & -- & -- & -- & -- & -- \\
  \midrule
  \shortstack[l]{Ours (Language as Bridge + G-DINO) \\ \% change w.r.t. O-MaMa} & & & & $-11.5\%$ & $-114.4\%$ & $-20.2\%$ & $-7.9\%$ & $-39.0\%$ & $-10.7\%$ \\
  \shortstack[l]{Ours (Language as Bridge + RoMa) \\ \% change w.r.t. O-MaMa} & & & & -- & -- & -- & -- & -- & -- \\
  \bottomrule
\end{tabular}}
\end{table*}

Table~\ref{tab:main} presents results on the EgoExo4D Correspondences v2 test
set. As expected, the gap to the current state of the art set by
LM-EEC~\cite{hu2025lmeec} is significant: our pipeline scores 37.7 and 40.6
IoU in the Ego2Exo and Exo2Ego directions, respectively, against 54.98 and
65.77 for LM-EEC. Nonetheless, within the set of methods that do not retrain SAM~2's backbone,
the comparison becomes more competitive.
Our approach trails O-MaMa~\cite{o-mama}, winner of the 2025 Ego-Exo4D
Correspondences Challenge, by approximately 5 IoU points in both directions
($-11.5\%$ Ego2Exo, $-7.9\%$ Exo2Ego), while surpassing
ObjectRelator~\cite{fu2025objectrelatorenablingcrossviewobject} in IoU across
both settings (37.7 vs.\ 35.3 Ego2Exo; 40.6 vs.\ 40.3 Exo2Ego).
 
Notably, in the Ego2Exo direction, our LE of 0.071 is worse than
the single-view baselines XMem+XSegTx (0.038), despite our superior IoU.
This indicates that our pipeline tends to recover the correct object region but
with coarser spatial precision than competing methods. This behaviour
is qualitatively distinct from the mask-matching failure modes of ObjectRelator
and O-MaMa, and consistent with the language-to-anchor grounding mechanism in
Block~3. 
% ------------------------------------------------------------
\subsection{Ablation Study}
\label{sec:ablations}
We analyse pipeline performance across four dimensions: architecture choices, VLM description quality, description content, and failure profile against state-of-the-art baselines. Each ablation isolates a single design decision or foundation model, identifying where the pipeline gains and where it remains limited on a task none of its constituent models were explicitly designed for. In \cref{app:add-ablations} we expand on each experiment to further validate our method's design.

\begin{table}[h]
  \centering
  \footnotesize
  \setlength{\tabcolsep}{4pt}
  \caption{Contribution of each pipeline component on 10\% of the validation set takes, Ego2Exo direction.}
  \label{tab:abl-summary}
  \resizebox{\linewidth}{!}{%
  \begin{tabular}{lrrr}
  \toprule
  Exp. & IoU$\uparrow$ & $t$/frame\,(s)$\downarrow$ & Total$\downarrow$ \\
  \midrule
  Baseline & 0.1059 & 1.32 & 3h\,50m \\
  Baseline Oracle & 0.1703 & 0.12 & 21m\,20s \\
  \midrule
  (A) Best Single Frame & 0.1684 & 0.13 & 22m\,53s \\
  (B) SAM3 Agent Loop & 0.3554 & 21.30 & 59h\,15m \\
  (C) Propagation & 0.3769 & 0.82 & 2h\,23m \\
  (D) G-DINO & 0.3751 & 0.90 & 2h\,37m \\
  (E) RoMa & \textbf{0.4331} & 1.04 & 3h\,01m \\
  \midrule
  \shortstack[l]{E \% change w.r.t.\\ Baseline $\tfrac{x-y}{y}$}    
    & \textcolor[rgb]{0,0.55,0}{$+309.0\%$}
    & \textcolor[rgb]{0,0.55,0}{$-21.2\%$}
    & \textcolor[rgb]{0,0.55,0}{$-21.4\%$} \\
  \bottomrule
  \end{tabular}}
\end{table}

\paragraph{Pipeline architecture.} \Cref{tab:abl-summary} reports the 
contribution of each pipeline component on the \textit{Ego2Exo} direction. The 
baseline queries the VLM independently per source frame, runs SAM3 once per 
destination frame, and achieves an IoU of $0.106$ at $1.32$\,s per frame. 
Replacing the VLM description with the ground-truth object name (Baseline Oracle) raises IoU to $0.170$, suggesting that description quality is a primary bottleneck.

Generating the object description on every source frame is redundant. Experiment~A 
selects a single salient source frame via the composite visibility score, recovering 
nearly all of the oracle gain at IoU~$0.168$ and $0.13$\,s per frame, without any 
privileged information. This confirms that object appearance is stable enough across 
a take, and that running correspondence from a frame where the object is poorly 
visible degrades prediction accuracy.

Experiment~B applies the full SAM3 Agent loop across all destination frames, yielding 
IoU~$0.355$; however, repeated tool-calling raises latency to $21.30$\,s per frame, 
making the configuration impractical at scale. Experiment~C addresses this with propagation, reaching a comparable IoU~$0.377$ at $0.82$\,s per frame, a $25\times$ 
speedup. Together, B and C establish propagation as the computationally efficient 
route to high per-frame accuracy, and reinforce the principle of anchoring cross-view 
correspondence in few destination frames.

To address the asymmetry in mask quality between views, Experiments~D and E evaluate 
destination frame selection strategies. GroundingDINO (D) yields IoU~$0.375$ at 
$0.90$\,s per frame, with no measurable gain over C alone. RoMa (E) scores candidate 
frames by the density of traceable geometric correspondences from source object pixels, 
achieving the best result of IoU~$0.433$. This represents a $+309.0\%$ relative improvement over the baseline while reducing  latency by $21\%$, suggesting that object-aware geometric matching is a more reliable anchor-selection criterion than language-vision grounding, particularly for visually ambiguous objects. Nonetheless, we attribute most of this performance gain to noise of the ablation subset, and report both GroundingDINO and RoMa on the full test set because they achieve comparable results.  

\paragraph{Description Quality.} We compare Block~2 backends on description quality using Gemma4 ~\cite{gemma4blog} as-judge (~\cref{tab:abl-vlm}), passing each generated description alongside the source image and ground-truth label and scoring correctness as a binary variable across object identity, contextual descriptors, and view-independent attributes. Qwen~3.5~35B achieves the highest judge scores in both directions ($77.0\%$ on \textit{Ego2Exo} and $70.0\%$ on \textit{Exo2Ego}) motivating its selection as the Block~2 and~3 backend. The batched variant is $43\%$ faster, but loses $23.4$\,pp on \textit{Ego2Exo} quality, making the trade-off unfavorable. In contrast, Pixelrefer models underperform Qwen despite their region-aware training, supporting our case for using general-purpose foundation models.

\begin{table}[h]
    \centering\small
    \setlength{\tabcolsep}{4pt}
    \caption{Description quality judged by a Gemma 4.}
    \label{tab:abl-vlm}
    \resizebox{\linewidth}{!}{
    \begin{tabular}{l ccc}
      \toprule
      Backend & \shortstack{Success Score$\uparrow$\\\textit{Ego2Exo}}
              & \shortstack{Success Score$\uparrow$\\\textit{Exo2Ego}}
              & \shortstack[c]{Single Call \\ Time\,(s)}$\downarrow$ \\
      \midrule
      \shortstack[l]{Qwen~3.5~35B\\(batched)} & 53.6\% & 41.3\% & \textbf{4.30} \\
      Pixelrefer-Lite-7B  & 53.7\% & 37.2\% & 6.00 \\
      Pixelrefer-7B       & 54.6\% & 39.9\% & 5.23 \\
      Qwen~3.6~35B        & 75.0\% & 58.2\% & 7.55 \\
      Qwen~3.5~35B        & \textbf{77.0\%} & \textbf{70.0\%} & 7.58 \\
      \bottomrule
    \end{tabular}}
\end{table}
 


\paragraph{Description Content.} We assess whether richer object descriptions improve destination frame selection. To this end, we fix Block~3 to GroundingDINO and vary the description passed as input across three cumulative conditions: object identity alone, extended with colour, and further with view-independent context attributes. We report two containment-based proportions: Fully Inside (FI), the fraction of cases where the ground-truth bounding box lies fully within the predicted region, and Centroid in BBox (CB), the fraction where at least the ground-truth centroid falls inside.

\begin{table}[h]
\centering\small
\setlength{\tabcolsep}{3pt}
\caption{Description content impact on GroundingDINO.}
\label{tab:abl-desc}
\resizebox{\linewidth}{!}{%
\begin{tabular}{l ccc ccc}
  \toprule
  & \multicolumn{3}{c}{\textit{Ego2Exo}}
  & \multicolumn{3}{c}{\textit{Exo2Ego}} \\
  \cmidrule(lr){2-4}\cmidrule(lr){5-7}
  Description & IoU$\uparrow$ & FI$\uparrow$ & CB$\uparrow$ & IoU$\uparrow$ & FI$\uparrow$ & CB$\uparrow$ \\
  \midrule
  Object only
    & -- & -- & -- & -- & -- & -- \\
  Object\,+\,colour
    & -- & -- & -- & -- & -- & -- \\
  \shortstack[l]{Object\,+\,colour\\+\,descriptors}
    & -- & -- & -- & -- & -- & -- \\
  \bottomrule
\end{tabular}}
\end{table}

\paragraph{Performance Profile.} We compare the full pipeline against LM-EEC~\cite{hu2025lmeec} on $30\%$ of the validation set in the \textit{Exo2Ego} direction, considering a segmentation successful when its IoU$>0.5$. \Cref{fig:iou-dist} reveals a bimodal IoU distribution: a large zero-IoU mass reflects cases where anchor selection or segmentation fails entirely, but successful cases concentrate at high IoU ($>0.7$), matching or exceeding LM-EEC. The scenario breakdown (\cref{fig:scenario}) shows that basketball ($81\%$ joint success) and music ($65\%$) are the pipeline's strongest scenarios, while health ($52\%$ LM-EEC only) and unknown scenarios ($44\%$ LM-EEC only) are the primary failure modes. Object size follows the same pattern (Figure~\ref{fig:obj-size}); joint success rises from $34\%$ on small objects to $62\%$ on large ones as the both-fail rate drops from $38\%$ to $5\%$, suggesting that larger, more distinctive masks benefit both anchor selection and propagation. Interestingly, \cref{fig:anchor-iou} indicates that anchor quality is the single strongest predictor of pipeline success: when seed-frame IoU is high ($[0.7,\,1]$), both methods succeed jointly in $80\%$ of cases and the LM-EEC advantage shrinks to $7\%$; in the low bucket ($[0,\,0.3)$), the pipeline collapses to $6\%$ joint success, confirming that anchor selection remains the highest-leverage direction for future improvement. 

\begin{figure}
    \centering
    \includegraphics[width=\linewidth]{imgs/scenario-iou.png}
    \caption{Ours vs LM-EEC success by scenario.}
    \label{fig:scenario}
\end{figure}

\begin{figure}
    \centering
    \includegraphics[width=\linewidth]{imgs/size-iou.png}
    \caption{Ours vs LM-EEC success by object size.}
    \label{fig:obj-size}
\end{figure}

\begin{figure}
    \centering
    \includegraphics[width=\linewidth]{imgs/anchor-iou.png}
    \caption{Ours vs LM-EEC success by anchor quality.}
    \label{fig:anchor-iou}
\end{figure}

\begin{figure}
    \centering
    \includegraphics[width=\linewidth]{imgs/iou-distr.png}
    \caption{Ours vs LM-EEC IoU distribution.}
    \label{fig:iou-dist}
\end{figure}
```

**Additional interpretability analysis** (consider as new section, not present in paper yet because it is more recent):
Check these files in root directory:
– pipeline_gap_findings.md
– Takeaways.md

---

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
